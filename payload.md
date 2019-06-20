# Payload

Sigfox messages are small and optimized for sensors, as they require only a small amount of power.

The Sigfox uplink payload is limited to 12 bytes (excluding the payload headers). The downlink payload is limited to 8 bytes (excluding the payload headers).

Although this might seem to be a very restricted maximum payload size, there's actually a lot that can be done with 12 bytes.


## Of Bits and Bytes

The easiest way to think of using these 12 bytes is to just fill them up with an integer provided by the device's sensor. For instance, is it 20 degrees outside? Just put "20" in the payload!

However, that would fill your payload with plain ASCII values, which is a huge loss of space and uses a lot of bandwidth.

The clever way is to rely on binary encoding to optimize your payload, with calculations in place, both on the device side (the firmware calculates the bits) and the platform side (the server takes the bits and calculates the content).

What's with using binary instead of ASCII? It means making full use of the space available, and sending messages such as "FA23BC02" or "01AA36AC" which in appearance mean nothing, but are in fact packed with information once you know how to recalculate the initial message.

So, let's take a step back and remember what bits and bytes are.

A bit is the simplest unit of information there is: it's either 0 or 1. In itself that's very useful: one/off, hot/cold, day/night, alive/dead, plus/minus, etc. There are numerous things that a binary number (or binary digit, hence "bit") can tell us.

A byte is made of 8 bits. For instance, 0 is 00, 1 is 01, 2 is 10, etc. As a developer, you certainly know that -- and you know your table for hexadecimal values:

<table>
	<tbody>
		<tr align="center">
			<td>Decimal</td>
			<td>Hexadecimal</td>
			<td>Binary</td>
		</tr>
		<tr align="center">
			<td>0<br />1<br />2<br />3<br />4<br />5<br />6<br />7<br /> 8<br />9<br />10<br />11<br />12<br />13<br />14<br />15</td>
			<td>0<br />1<br />2<br />3<br />4<br />5<br />6<br />7<br /> 8<br />9<br />A<br />B<br />C<br />D<br />E<br />F</td>
			<td>0000<br />0001<br />0010<br />0011<br /> 0100<br />0101<br />0110<br />0111<br /> 1000<br />1001<br />1010<br />1011<br /> 1100<br />1101<br />1110<br />1111</td>
		</tr>
	</tbody>
</table>

Hence, a message such as FA23BC02 is four bytes with the following content:

<table>
	<tbody>
		<tr align="center">
			<td>Byte 0</td>
			<td>Byte 1</td>
			<td>Byte 2</td>
      <td>Byte 3</td>
		</tr>
		<tr align="center">
			<td>FA</td>
			<td>23</td>
      <td>BC</td>
      <td>02</td>
		</tr>
		<tr align="center">
			<td>11111010</td>
			<td>00100011</td>
      <td>10111100</td>
      <td>00000010</td>
		</tr>
  </tbody>
</table>

This 4-byte message thus turns into a 32-bit message, which can then be used in multiple ways, as we will soon see.


## Effect on Battery

Devices do not have to send 12 bytes: they can even just send an empty message (as a "I'm still alive" message), or a 1 bytes message ("on/off" message).

This is important, as a long messages means a long radio signal, thus more battery use. Therefore, using all 12 bytes in all messages uses more energy than using just the right size for a payload.

In short: optimize your payload in order to give your device a longer battery life!


## A technicality: padding bits

In theory, you can use just the right amount of data for your payload: 3 bytes, 7 bytes, you are in control!

But that's not the complete info. In effect, the payload bytes have to fit within a certain transmission length, predefined by the Sigfox protocol. The reason for this flexibility is to optimize transmission time and hence save battery consumption at the device level.

The transmission lengths depend on the number of bytes used in the payload. For an uplink message:

<table>
	<tbody>
		<tr align="center">
			<td align="right"><b>Bytes of data</b></td>
			<td>0</td>
      <td>1</td>
      <td>2</td>
      <td>3</td>
      <td>4</td>
      <td>5</td>
      <td>6</td>
      <td>7</td>
      <td>8</td>
      <td>9</td>
      <td>10</td>
      <td>11</td>
      <td>12</td>
		</tr>
		<tr align="center">
			<td align="right"><b>Payload size in bytes</b></td>
      <td>0</td>
      <td>1</td>
      <td colspan="3">4</td>
      <td colspan="4">8</td>
      <td colspan="4">12</td>
		</tr>
  </tbody>
</table>


Where a payload falls between 2 pre-defined lengths, the free "space" is filled with protocol padding. Example: if your payload is set to 9 bytes, 3 bytes of padding are added by the protocol firmware to generate a 12-byte transmission.

As such, the payload has a direct impact on transmission time:

<table>
  <tr>
    <th rowspan="2">Payload length</th>
    <th colspan="2" align="center">Frame transmission time</th>
  </tr>
  <tr align="center">
    <td>RC1/RC3/RC5</td>
    <td>RC2/RC4</td>
  </tr>
  <tr align="center">
    <td>&lt; 1 bit</td>
    <td>1.1 seconds</td>
    <td>190 ms</td>
  </tr>
  <tr align="center">
    <td>2 bits - 1 byte</td>
    <td>1.2 seconds</td>
    <td>200 ms</td>
  </tr>
  <tr align="center">
    <td>2 - 4 bytes</td>
    <td>1.45 seconds</td>
    <td>250 ms</td>
  </tr>
  <tr align="center">
    <td>5 - 8 bytes</td>
    <td>1.75 seconds</td>
    <td>300 ms</td>
  </tr>
  <tr align="center">
    <td>9 - 12 bytes</td>
    <td>2 seconds</td>
    <td>350 ms</td>
  </tr>
</table>

You could think of this padding as "lost bytes", since they use time and energy but do not contain anything useful to you. It is therefore up to you to optimize your payload so that all of its transmitted bytes (4, 8, or 12) are useful.

Downlink messages have a fixed length too: The payload must be 8 bytes long exactly. Hence, if less information bits are to be transmitted, padding is necessary.


## Basic Payload Ideas

A 12-byte uplink message can be used for:

- 2 GPS coordinates down to 3 meters precision: 6 bytes.
- 6 temperature reports within a range of -100 °C to +200°C, with a 0.004 degree precision: 2 bytes.
- 12 reports of speed radar up to 255 km/h: 2 bytes.
- 96 switch reports (on/off, day/night, hot/cold, etc.): 1 byte.
- ...and anything your sensor can produce!

Of course, you can combine reports! For instance, one message can contain:

- 8 bytes for more precise lat/long coordinates.
- 1 byte for a volume report.
- 1 byte for a satisfaction report.
- 1 byte for a switch report.
- 1 byte for speed report.

An 8-bytes downlink message can be used for:

- Changing the device's configuration.
- Adjusting the scale of a sensor.
- Adjusting the frequency of messages -- because less messages per hour also means less energy use.
- Requesting an uplink message with additional data for a recent report from the device.
- Requesting a firmware upgrade through a high-speed connection (GSM, for instance).

See the "What Kind Of Data A Sigfox Message Can Carry?" video: [https://www.youtube.com/watch?v=JijOpH75xG8](https://www.youtube.com/watch?v=JijOpH75xG8)


## Payload Example: Sens'it v3

The Sens'it device is an example of what can be done with well-made payloads.

Sens'it is a multi-sensor device made by Sigfox, to serve as [a devkit and a playground for developer](https://build.sigfox.com/sensit-for-developers). It features six sensors and one button. Each sensor is tied to a specific mode. This means that Sens'it give us 6 payloads to explore!

Whatever the current mode, the uplink payload for Sens'it message is 4 bytes, the first byte being the same for all modes.

- Byte 0: 5 bits are used for the battery level. The 3 remaining bits are reserved.
- Byte 1: 5 bits are used for the mode itself; 1 bit is used for the button alert flag; 2 bits are used differently depending on the mode.
- Byte 2: Depending on the mode, the bits are either used for sensor value, or for firmware information.
- Byte 3: Depending on the mode, the bits are either used for sensor value, or for firmware information.


[Read the Sens'it 3 Discovery payload structure document](https://storage.sbg1.cloud.ovh.net/v1/AUTH_669d7dfced0b44518cb186841d7cbd75/dev_medias/build/4059ah1jm2278g1/sensit-3-discovery-payload.pdf).

The important thing to see here is that values are not depending on bytes, but on bits.

Let's take a real Sens'it payload as an example: ee098b6c. When a platform receives such a message from the Sigfox Backend, it has to discover the mode and then sort out the payload content, depending on that mode — all based on the bits.

ee098b6c is a hexadecimal value containing 4 bytes of data. It's made of the following bits:

- 0xee: 0b11101110
- 0x09: **0b00001001**
- 0x8b: 0b10001011
- 0x6c: **0b01101100**

_(we're putting every other byte in bold to help differentiate where their individual bits in the explanation below)_

Since we have to decode the Sens'it payload, the grouping should be done this way:  
11101 110 **00001 0 01**10001011 **01101100**

Here's the grouping, explained as per the Sens'it documentation (linked above):

- 11101: Battery level
- 110: Reserved
- **00001**: Mode (Here: 1, so "Temperature & Humidity")
- **0**: Button Alert Flag
- **01**10001011: Mode-dependent data (Here: temperature)
- **01101100**: Mode-dependent data (Here: humidity level)

Such group is called coding, and since Sigfox does not change your message content, it is up to the device to code sensor values into bits and bytes, and up to the device platform to decode the bytes and bits back into values.

Let's see how Sens'it does it.


### Battery Level

The documentation tells us that "in order to convert the payload value into Volts, we should use the following formula:

volts = (payloadValue * 0.05) + 2.7

The payload value is 11101 in our example, which in decimal is 29. That gives us a battery level of: 
(29*0.05)+2.7 = 4.15 volts.

Knowing that a full Sens'it v3 battery contains 4.15 volts, we know we are at 100%.


### Button Alert Flag

The documentation tells us that the flag is "set to 1 when the user has double-pressed the button, otherwise it is set to 0." Obviously, no button has been pushed here: 0 is the number.


### Temperature & Humidity Mode


The documentation tells us that in order to convert the payload value into °C (Celsius degrees), we should use the following formula:

temperature = (payloadValue - 200) / 8

The payload value is **01**10001011 in our example, which in decimal is 395. That gives us a temperature of:
(395-200)/8 = 24.375 °C

As for humidity, the documented formula is:

humidity (%) = payloadValue / 2

The payload value is **01101100**, which in decimal is 108. That gives us a relative humidity of:
108/2 = 54%

As you can see, this payload can be quite packed with information! Of course, all those decoding calculations are done by the platform. Likewise, the device's software takes care of encoding sensor values into the payload. In short, it's up to you, as a device maker and platform builder, to encode and decode a payload that packs the most data for the given byte number.


### Vibration and Brightness Modes

Let's explore the payloads for two other modes.

Here's one for the Vibration mode: ee098b6c.

Converted from hexadecimal to binary, we get 11101110 **00001001** 10001011 **01101100**, which gives us, according to the Sens'it v3 payload documentation:

- 11101: Battery level
- 110: Reserved
- **00100**: Mode (Here: 4, so "Vibration", as expected)
- **0**: Button Alert Flag - No button was clicked here.
- **01**: Mode-dependent data (Here: 1, which means "A vibration is detected.")
- 00000000**00000001**: Mode-dependent data (Here: 1, which means that one vibration has been detected since the last mode change)

<br/>Now let's turn to the Brightness mode. Here's the payload the Sigfox Cloud receives when we select that mode: ee100022.

Converted from hexadecimal to binary, we get 11101110 **00010000** 00000000 **00100010**, which gives us, according to the Sens'it v3 payload documentation:

- 11101: Battery level
- 110: Reserved
- **00010**: Mode (Here: 2, so "Brightness", as expected)
- **0**: Button Alert Flag - No button was clicked here.
- **00**: Spare block, always as 0b00 in this mode.
- 00000000**00100010**: Mode-dependent data (Here: 34). Using the documentation, we obtain the brightness value by dividing that value by 96 : 34/96 = 0,35 lux.


## Tips & Tricks to Optimize your Payload

Exploring how the payload of the Sens'it device is made can teach a lot about the ways you can improve your own payload.

There are few generic ideas that you should keep in mind when conceiving your payload and its encoding/decoding:

- Use binary encoding. If nothing else, the above examples should be proof that you can do a lot by playing with the bits of the payload and a little bit of clever calculation. DO NOT use plain text!
- Combine sensor values to avoid sending one message per sensor.
- Choose a relevant value precision. If a rough value is enough for the end-customer to take a decision, then limit yourself to one or two decimals — or even drop the decimals altogether if that makes sense.

You can pack a lot of information in 12 bytes. Make good use of it!
