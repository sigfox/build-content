## Sigfox classes

### What are Sigfox classes

Sigfox defines an uplink classification for each RC, which applies to every device and is assessed when passing the Sigfox Ready certification. 

These classes are ranked from strongest to weakest: U0, U1, U2, and U3. They indicate the RF radiated performance of a device, which can have a significant impact on the message success rate. They are based on EIRP (effective isotropic radiated power) intervals. 

Simply put, a U0 device enjoys a much better message reception than a non-U0 device. This means better user feedback and fewer support requests for your team.

For instance, in RC1, U0 must have a low EIRP limit of 12 dBm. In RC2, the low limit is at 20 dBm. See more in this document: https://support.sigfox.com/docs/radio-configuration

Therefore, while not mandatory, U0 is the class to aim for when building a device in the majority of use-cases. It is the best way to ensure Sigfox Network Service delivery at nominal performance level.


### Tips to improve emission power

Anything that is added between the module and the open air can lower the EIRP of your device: the antenna itself can lower EIRP by 2 dBm, the casing also has a strong impact, the way the insides of the device is organized can have an impact, same for the ground plane, etc.

There are best practices to follow when building a device, that will ensure maximal emission power:

* Plan for U0 at the very start of your project. That means integrating the complete RF design (including antenna and mechanical designs) early on, to maximize your chances to be U0 without having to deeply rework your plan later on.
* Start with a powerful radio base. For instance, choose a very good module/transceiver, so that weakening elements cannot bring the device's EIRP lower than 12 dBm. This means choosing a module/transceiver according to your target RCs.
* Choose your antenna carefully. Better yet: hire an antenna specialist. You can find some on Sigfox Partner Network: https://partners.sigfox.com/companies/antenna-designer
* Never use a metal casing for the radio part! Prefer a plastic casing. Note that you can choose to have part of your device in a metal casing, provided that the device will be placed on a metal support (antenna, etc.).
* Leave room within the device, to favor a bigger antenna. This might mean a slightly bigger casing.
* Test the complete device, not with just a PCB and an antenna: include the casing and any other component that might have an impact on emission.

The two best tips:

* Aim for U0.
* Hire an antenna designer.


### When is U1 enough?

You should always aim for the U0 class. Because the Sigfox coverage is calculated for U0 devices, non-U0 devices might not get the coverage they need. Try simulating coverage for U1 to u3 in the Sigfox backend and see whether it suits your project's needs: https://backend.sigfox.com/.

That being said, U0 is not mandatory, U1 or even U2 might be enough when some conditions are available:

* The device is located in a fixed location, with a dense network.
* The device is located outside, on a high tower.

In any other case, U0 should be the target. The Sigfox Device Cookbook has a whole section dedicated to matching use-case and device class: https://build.sigfox.com/sigfox-device-cookbook

Keep in mind that it is very hard to update a U1 device to a U0 one. If you chose to commercialize a U1 device and you get bad feedback about the reception, you will have to rework your device from the start in order to reach U0. It is much easier to aim for U0 right from the start.


### Energy consideration

With good antenna design (U0 class device) you can sometimes lower on purpose the radiated power thus saving energy. For example, if you set the transceiver output power at 0dB instead of 14dB you can save about 50% power. This output power can be changed by downlink after installation to optimize the battery life when level of reception is good enough and position is fixed.

The transceiver output power can also be set at factory level for energy saving purpose. You have to know what you are doing here (see §When is U1 enough?)! In that case class U1 or U2 doesn’t mean the hardware is not U0 capable.



