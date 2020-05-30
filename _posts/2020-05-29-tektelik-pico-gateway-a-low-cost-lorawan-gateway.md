---
layout: default
title: "Tektelik Pico Gateway: A low cost LoRaWAN gateway"
published: 2020-05-29T18:55:02.218Z
date: 2020-05-29T18:55:02.243Z
thumbnail: /assets/images/tektelic-2.png
categories: lorawan
tags:
  - lorawan
  - gateway
comments: false
---
LoRaWAN gateways are getting cheaper and cheaper everyday, and nowadays (May 2020) we have available 8 channel LoRaWAN compliant gateways for under 100€ ([TTIG](https://www.thethingsnetwork.org/docs/gateways/thethingsindoor/) is around [80€](https://es.rs-online.com/web/p/kits-de-desarrollo-de-radio-frecuencia/1843981/)). In the post we will see all the bells whistles on how to configure on of this gateway, the Tektelic Pico, to connect to The Things Network LoRaWAN server and we will compare it with another options available on the market.

<!--more-->

Today I come to talk to you about what it is my mains home LoRaWAN gateway right now, the Tektelic Pico (also called [Tektelic KONA Micro Lite](https://tektelic.com/wp-content/uploads/KONA-Micro-Lite.pdf), as it is a light version of the [Tektelic KONA Micro](https://tektelic.com/wp-content/uploads/KONA-Micro.pdf)). This is the cheapest option available from Tektelic right now. I purchased some months ago at [The Things Conference](https://www.thethingsnetwork.org/conference/) so I have been able to test it for quite some time. 

[Tektelic Communications](https://tektelic.com/) is a Canadian company that is is one of the biggest suppliers of LoRaWAN IoT products right now. It not only supplies ones of the best (and most expensive) gateways on the market but it also provides sensors and custom applications. So it is a well reputed company on the LoRaWAN IoT sector.

# Specifications

The Tektelic KONA Pico is the cheapest product in their catalogue and it is intended to be an indoor home/office gateway get LoRaWAN coverage indoors. 

# Take it apart!

First things first... What is it inside this thing? The answer is not to exciting, we have what we were expecting, nothing fancy, but as you can see on the photos the layout quality is pretty good and the RF design excellent. No doubt they have pay quite a lot attention to details and everythings looks fantastic. Well it is what you expect  from a company like Tektelic, right?

![](/assets/images/tektelic_pcba_blocks_872x768.png)

We found a one single PCB construction, so it seems that they have layout the board from scratch, what it is not so common in LoRaWAN gateways as normally they tend to design a mini-pcie size 8-channels LoRa board with the difficult part (that is the radio frequency section) and they reuse it on all of their designs. It is not uncommon to subcontract this module to a RF expert third party and include it as a block in your product. This option is also the most common as it is more flexible, and it saves a lot of design efforts reducing the time to market of your product. But on the other hand, it is probably more expensive to produce as there is two boards to manufacture and then to assemble together, so it is not surprising that Tektelic has opt for the cheapest for their low cost gateway, despite the bigger design effort. Also, Tektelic has a lot of experience on RF design (it is it's thing), so I have no doubt they have design the whole board theyself and they have done a really good job.

We found a [NXP Kinetics 64bit ARM M4 microcontroller](https://www.nxp.com/products/processors-and-microcontrollers/arm-microcontrollers/general-purpose-mcus/k-series-cortex-m4/k6x-ethernet/kinetis-k64-120-mhz-256-kb-sram-microcontrollers-mcus-based-on-arm-cortex-m4-core:K64_120) as main CPU (microcontroller? not a microprocessor? we will talk later about that). This is a low cost low power all included powerful microcontroller running up to 120MHz

No surprise also with the design as we found the typical chips for a LoRaWAN gateway. A

One 

![](/assets/images/tektelic_embedded_antenna_1176x768.jpg)

\[Teardown with photos]

# Connecting it to TTN

This gateway configuration is very different for what I was used to. I'm used to work with gateways that usually runs some custom Linux based firmware, usually Yocto distro in case of the Multitech Conduit, or Debian in case of the RAK but always with SSH support. But, I was surprise on my first attempt to configure this things as I cannot SSH it and I have to dig around with it and ask for support help to Tektelic. In fact, this is one of the main causes why I'm writing this post, as I thing it may help others Makers like me to get it working without pain.

The first you may be aware of is that Tektelic seems to be a little bit closed with that things as it is more focused on the professional and industrial market, than on the Maker side, so you have to request the documentation (registering on their [Tektelic support website](https://support.tektelic.com/)) as it is not easy to found online. 

# Conclusions