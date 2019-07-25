# Radio chipsets

In order to create a Sigfox-enabled solution, you will need to integrate a Sigfox compatible radio chipset in your device. Thanks to our open approach regarding hardware integration, our semiconductor ecosystem offers a wide variety of solutions to answer all our customer needs.

This page will bring you up to speed with the available technologies, and how to choose one depending on your use-case.


## Types of chipset

There are three types of chipset you can use in your project: a Module, a Reference Design, or a Transceiver. They each have their advantages and disadvantages, which you must consider carefully so that you can make a sensible decision.

Two essential aspects differ between each:
* Whether the chipset implements the Sigfox Library or not (https://build.sigfox.com/sigfox-library-for-devices). Modules and Reference Designs do. We call them "Sigfox Verified modular design".
* Whether you want to customize the hardware or not. Reference Designs and Transceivers allow that.


### Buying a Module

With a module, you get the fastest and easiest solution to work with Sigfox while providing very good performance.

As a Sigfox Verified modular design, it already integrates the Sigfox Library and is very easy to use.

Note that modules can get expensive for high volumes.


### Using a Reference Design

A SoC (for System on Chip) with a reference design makes it easy to get started with Sigfox. 

As a Sigfox Verified modular design, it already integrates the Sigfox Library but requires that you source the various components yourself.

It gives a good balance between price, customizability, and ease of use.


### Building a Transceiver

Building your own modem design based on a transceiver is the most complex solution as you will have to:
* integrate the Sigfox library yourself.
* be required to pass the Sigfox Verified certification.

Transceivers are perfect for large-volume projects, since they require a larger investment from the device maker. Indeed, with a complex integration and a certification, using transceiver can be more demanding both financially and in terms of manpower. Still, it can be interesting for high-volume projects.


## Which one to choose?

Which solution is best? This depends on your needs, volume targets and radio knowledge. Each product has its own advantages (electrical consumption, footprint, price, other connectivity, etc.), so it is up to the device maker to find the best solution for their product. 

In order to achieve a quick time-to-market, we strongly advise device makers to use a Sigfox Verified module, as this is the fastest and easiest solution to work with Sigfox while providing very good performance. For larger volumes, all options are open: it depends on what your objectives are.

There is no single perfect module that fits all use-cases. The best way for you is to get better at choosing a module that fits your current project, is to explore your choices thoroughly every single time.

There are a few criteria to keep in mind: technology-related and environment-related.


### Technology-related criteria

When benchmarking modules for your projects, dive into their technical specification in order to learn more about the module.

Here are some important aspects to consider:
* Electrical consumption. This is the most important point. Some modules consume twice than others.
* Price. Obviously, this has a real impact on your business plan.
* Size. This can force your option in terms of PCB or casing.
* Additional technologies (Wi-Fi, Bluetooth, etc.).
* Openness. Does it need an MCU, or is it a closed module with its own MCU?
* Support for Sigfox features. Which RC does it support? If not multi-RC, do chips for different RC have the same pin design, so as to be interchangeable in the same casing? Does it support Monarch and/or Atlas?
* Minimum order quantity (MOQ) from the maker.

Electrical consumption might be the most important aspect to consider. Consumption depends on radio frequency: the one announced by the maker is sometimes lower than the one required for Sigfox, so be sure to double-check what the techspec says. 

Also, do compare modules, as some can consume twice than others -- but can have better documentation or ecosystem. It's up to you to find the right balance. Sometimes it's better to have good support and documentation with a "good enough" module, rather than an extremely optimized but bare-bane module.


### Environment-related criteria

So, by now you should have filtered down your chipset options down to a handful of candidates. How to pick The One for your current project?

Here are a few important additional aspects to consider:

* Is the chipset documented? Some makers sell excellent chipsets with no documentation whatsoever; others make good enough chipsets with plenty of documentation.
* Is it available in your timeframe? This is extremely important, as this will have an impact on your whole project delivery. Chipsets can be marked as available from a seller, but in reality will only be delivered to you months after you ordered them. 6 months is a good timeframe in the current market; actual delivery can often take 9 to 12 months. You have to budget for this.
* Is it available in your needed volume? A chipset can tick all your boxes, but if the maker cannot deliver enough items for your project, then you might want to rethink your choice.
* Is there a community around it? You don't want to be the only one to use a chipset. Find out whether your pick is used elsewhere, as it can be helpful to have other users to talk with.
* Is it easy to use? This means having an SDK and a programmable MCU, not an MCU that only does Sigfox modulations with AT commands. Also, check that the SDK environment is maintained and up-to-date.
* Is there an evaluation board/kit (evalkit) available? Contrary to a devkit, an evalkit is bundled with XXXXXXX
* Do you have people at hand who know the chipset already? Ideally, it would be in-house, but having a local provider available can help tremendously.


As you can see, there's a lot to consider, and you'll have to read through piles of document and webpages. But it's worth it!


## From the Sigfox Partner Network

Sigfox Build is agnostic: we won't promote on chipset over another, we prefer to give you the keys to finding your own ideal.

What we can provide is a list of the most popular modules on Sigfox Partner Network. You still have to check that it is indeed the right choice for your current project.




## Oscillator Tech Note

Building your own radio chipset involves a lot of RF knowledge, even when using a transceiver. To help you achieve the best RF performance possible, we have created a technote presenting recommendation on the creation of your device's RF oscillator. Keep it in mind when selecting a crystal for your device! 

Access the Oscillator Tech Note (pdf)
https://support.sigfox.com/docs/oscillator
