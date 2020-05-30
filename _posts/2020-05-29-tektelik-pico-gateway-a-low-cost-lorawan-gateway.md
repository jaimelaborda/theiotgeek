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

\[Teardown with photos]



# Connecting it to TTN

This gateway configuration is very different for what I was used to. I'm used to work with gateways that usually runs some custom Linux based firmware, usually Yocto distro in case of the Multitech Conduit, or Debian in case of the RAK but always with SSH support. But, I was surprise on my first attempt to configure this things as I cannot SSH it and I have to dig around with it and ask for support help to Tektelic. In fact, this is one of the main causes why I'm writing this post, as I thing it may help others Makers like me to get it working without pain.

The first you may be aware of is that Tektelic seems to be a little bit closed with that things as it is more focused on the professional and industrial market, than on the Maker side, so you have to request the documentation (registering on their [Tektelic support website](https://support.tektelic.com/)) as it is not easy to found online. 

# Conclusions