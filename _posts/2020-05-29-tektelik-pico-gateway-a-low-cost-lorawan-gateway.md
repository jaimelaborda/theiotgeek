---
layout: default
title: "Tektelik KONA Micro Lite: A low cost LoRaWAN gateway"
published: 2020-05-31T19:02:34.635Z
date: 2020-05-31T19:02:35.406Z
thumbnail: /assets/images/tektelic-2.png
categories: lorawan
tags:
  - lorawan
  - gateway
comments: false
---
LoRaWAN gateways are getting cheaper and cheaper everyday, and nowadays (May 2020) we have available 8 channel LoRaWAN compliant gateways for under 100€ ([TTIG](https://www.thethingsnetwork.org/docs/gateways/thethingsindoor/) is around [80€](https://es.rs-online.com/web/p/kits-de-desarrollo-de-radio-frecuencia/1843981/)). In the post we will see all the bells and whistles on how to configure on this gateway, the Tektelic KONA Micro Lite, to connect to The Things Network LoRaWAN server and we will compare it with another options available on the market.

<!--more-->

Today I come to talk to you about what it is my mains home LoRaWAN gateway right now, the [Tektelic KONA Micro Lite](https://tektelic.com/wp-content/uploads/KONA-Micro-Lite.pdf) (seems to be also called Tektelic Pico, as it is a lightweight version of the [Tektelic KONA Micro](https://tektelic.com/wp-content/uploads/KONA-Micro.pdf)). This is the cheapest option available from Tektelic right now. I purchased some months ago at [The Things Conference](https://www.thethingsnetwork.org/conference/) so I have been able to test it for quite some time. 

[Tektelic Communications](https://tektelic.com/) is a Canadian company that is is one of the biggest suppliers of LoRaWAN IoT products right now. It not only supplies ones of the best (and most expensive) gateways on the market but it also provides sensors and custom applications. So it is a well reputed company on the LoRaWAN IoT sector.

# Specifications

The Tektelic KONA Pico is the cheapest product in their catalogue and it is intended to be an indoor home/office gateway to get LoRaWAN coverage indoors. 

This is a resume of their specifications they announce on the datasheet:

* Global frequency LoRaWAN band operation: NA915, EU868, AS923, AU915 and CH779
* Designed for Plug and Play Operability (we will see)
* Extremely compact, simple and fully integrated design (96 x 96 x 25 mm)
* RJ45 Ethernet port
* Up to 27dBm external antenna (with SMA port)
* IP31 external enclosure (for indoor usage only)

I would have love to find PoE support in this gateway but not surprise for the price point.

This is the content you will found on the box, no surprise here:

![](/assets/images/tektelic_content_768x1365.jpg)

* The Tektelic Kona Micro Lite itself in an ESD bag
* Power adaptor (5V - 2A)
* Ethernet cable (2 meters)
* External 868MHz indoor antenna

The box is so simple but you wouldn't expect to much more.

# Take it apart!

First things first... What is it inside this thing? The answer is not to exciting, we have what we were expecting, nothing fancy, but as you can see on the photos the PCB layout quality is pretty good and the RF design excellent. No doubt they have pay quite a lot attention to details and everythings looks fantastic. Well it is what you expect  from a company like Tektelic, right?

![](/assets/images/tektelic_pcba_blocks_872x768.png)

We found a one single PCB construction, so it seems that they have layout the board from scratch, what it is not so common in LoRaWAN gateways as normally they tend to do the design of the difficult part (the radio section) into a mini-pcie size 8-channels board and they reuse it on all of their designs, as the [TTIG does](https://tinkerman.cat/post/hacking-the-tti-indoor-gateway/). It is not uncommon to subcontract this module to a RF expert third party and include it as a block in your product. This option is also the most common as it is more flexible, and it saves a lot of design efforts reducing the time to market of your product. But on the other hand, it is probably more expensive to manufacture as there is two boards to be assembled and then to put it together, so it is not surprising that Tektelic has opt for the cheapest for their low cost gateway, despite the bigger design effort. Also, Tektelic has a lot of experience on RF design (it is it's thing), so I have no doubt they have design the whole board theyself and they have done a really good job.

We found a [NXP Kinetics 64bit ARM M4 microcontroller](https://www.nxp.com/products/processors-and-microcontrollers/arm-microcontrollers/general-purpose-mcus/k-series-cortex-m4/k6x-ethernet/kinetis-k64-120-mhz-256-kb-sram-microcontrollers-mcus-based-on-arm-cortex-m4-core:K64_120) as main CPU (microcontroller? not a microprocessor? we will talk later about that). This is a low cost low power all included powerful microcontroller running up to 120MHz

No surprise also with the RF design as we found the typical chips for a LoRaWAN gateway, the SX1308 digital signal processor for modulating/demodulating the RF signals and surely two SX1257 below the RF shielding can. It you are interested yo know more about LoRaWAN RF design I would recommend [this video](https://www.youtube.com/watch?v=0FaFVD5fedc) from Mobilefish.com.

What has caught my attention it is the footprint we see next to the SMA connectors that appears to be for a chip antenna, so we may have an option for placing an embedded antena for a even more size reduction (probably with a performance reduction). I have not been able to found the specific Part Number for this.

![](/assets/images/tektelic_embedded_antenna_1176x768.jpg)

We also found a place for what it looks like a Wifi module, in fact it is. Do they plan to build WiFi in this thing but couldn't get it for the right price point? Do the firmware support it so we can upgrade it in the future? Will Tektelic offer a WiFi version in the future? Are they sharing the board with the Micro version that does have WiFi? I don't know.

![](/assets/images/tektelic_wifi_1366x708.jpg)

What I know is that the they have left some trace in the firmware that indicates that the footprints WiFi module appears to be a [ISM-43362-MG3](https://www.inventeksys.com/wp-content/uploads/ISM43362_M3G_L44_Functional_Spec.pdf) from **Inventek Systems**. Cool! But I have not found any manner to indicate to the firmware that it connects to a specific SSID and password.

If you are interested, you can download some more high res photos from [here](https://photos.app.goo.gl/UzdSsmK7B24SvDnM9).

# Connecting it to TTN

This gateway configuration is very different for what I was used to. I'm used to work with gateways that usually runs some custom Linux based firmware, usually Yocto distro in case of the Multitech Conduit, or Debian in case of the RAK but always with SSH support. But, I was surprise on my first attempt to configure this things as I cannot SSH it and I have to dig around with it and ask for support help to Tektelic. In fact, this is one of the main causes why I'm writing this post, as I thing it may help others Makers like me to get it working without pain.

The first you may be aware of is that Tektelic seems to be a little bit closed with its things as it is more focused on the professional and industrial market, than on the Maker side, so you have to request the documentation (registering on their [Tektelic support website](https://support.tektelic.com/)) as it is not easy to found online. 

They recently have done a demonstration video on the last [The Things Virtual Conference](https://www.thethingsnetwork.org/conference/the-things-virtual-conference/) configuring this Gateway to connect to TTN but they have left many things. You can view it following this link: <https://www.youtube.com/watch?v=Jc6h65tjXi4>. 

The first thing you have to done is attach the external antena to the SMA connector, connect the Gateway to your home router, office switch or whatever through a  Ethernet cable as we don't have Wifi support (at least for now) and attach the 5V power cord. Then you will see that the power while LED starts to blink right away, this means that the router in not properly configured, but it is ready to do so.

You will have to register the gateway on [The Things Network Console](https://console.thethingsnetwork.org/). If you already have register a gateway on TTN this shouldn't be rare for you. You will find the gateway ID printed on a label on the bottom of the unit.

![](/assets/images/tektelic_sticker_1024x576.jpg)

So, it is just a matter of registering this ID as your gateway in the TTN Console, don't forget to check the "I'm using the legacy packet forwarder":

Well, according to Tektelic that should be pretty much all. Power LED should become fixed, gateway should appears as connected on TTN and this should be everything (Plug and Play, right?). But I don't know if it was me but I did not get lucky.

It seems that my unit come configured for the US market (915MHz), although I purchased in a UK shop and it come on with a EU power adaptor, and configured to use the Tektelic network server. So I have to tick around a bit.

In order to deeply configure the first thing you will need is to find it in your network. The gateways come with DHCP dynamic IP activated as default so I will recommend to give it a static lease on your LAN so it always receive the same IP address (and then you will now it). This depends on the router brand so you will have to do some research for your particular one if you don't already know how to do it but usually it is under **Local Network -> DHCP Reservation** as can be seen here:

![](/assets/images/mac_reservation.png)

[Example for TPLink router](https://www.tp-link.com/us/support/faq/182/)

Notice that you will find the MAC of the gateway on the sticker placed on the bottom of the unit with the Gateway-ID you have previously look at.

You can also used some IP scanner to guess it as [Advance IP Scanner](https://www.advanced-ip-scanner.com/es/) if you don't want (or can't) do the IP reservation but the host name seems to be 'unknown' so may be you will have to done some connect/disconnect trials in order to guest it.

Nevertheless, when you have the IP, you are ready to SSH into it, right?... Nop, there is no SSH access. The software running in this gateway is very basic, in fact it is a FreeRTOS based running on a microcontroller, so it is very quick booting but we lose some cool services like SSH. The gateway is configuring using the old very simple and lightweight [TFTP](https://en.wikipedia.org/wiki/Trivial_File_Transfer_Protocol)  protocol and it works pretty well. This is just a transfer file protocol, just like FTP that may sounds more to you, so Tektelic defines a table with the files that you can send or request

![](/assets/images/tektelic_table_lowres.jpg)

In order to get ir working you just have (at least in my case) to update the *Customer.json* and the *LoRaWAN.json* files. *Customer.json* file will tell the gateway where to send the LoRa packets and the LoRaWAN.json will indicate in what RF channels it has to listen. The content you have to put in these files, as well as the firmware binary in case it need an update (mine is running with 1.7) can be downloaded from [Tektelic Support website](https://support.tektelic.com). Nevertheless, I let here the content from my configuration (I'm using EU868 and TTN EU server)

**Customer.json**

```json
{
    "private_key_password": "",
    "network": "semtech",
    "semtech": {
        "host": "router.eu.thethings.network",
        "up_port": 1700,
        "down_port": 1700
    }
}
```

**LoRaWAN.json**

```json
{
    "public": true,
    "radio": [{
            "enable": true,
            "freq": 867500000
        }, {
            "enable": true,
            "freq": 868500000
        }
    ],
    "lora_multi": [{
            "enable": true,
            "radio": 1,
            "offset": -400000,
            "bandwidth": "",
            "sf7": false,
            "sf8": false,
            "sf9": false,
            "sf10": false,
            "sf11": false,
            "sf12": false
        }, {
            "enable": true,
            "radio": 1,
            "offset": -200000,
            "bandwidth": "",
            "sf7": false,
            "sf8": false,
            "sf9": false,
            "sf10": false,
            "sf11": false,
            "sf12": false
        }, {
            "enable": true,
            "radio": 1,
            "offset": 0,
            "bandwidth": "",
            "sf7": false,
            "sf8": false,
            "sf9": false,
            "sf10": false,
            "sf11": false,
            "sf12": false
        }, {
            "enable": true,
            "radio": 0,
            "offset": -400000,
            "bandwidth": "",
            "sf7": false,
            "sf8": false,
            "sf9": false,
            "sf10": false,
            "sf11": false,
            "sf12": false
        }, {
            "enable": true,
            "radio": 0,
            "offset": -200000,
            "bandwidth": "",
            "sf7": false,
            "sf8": false,
            "sf9": false,
            "sf10": false,
            "sf11": false,
            "sf12": false
        }, {
            "enable": true,
            "radio": 0,
            "offset": 0,
            "bandwidth": "",
            "sf7": false,
            "sf8": false,
            "sf9": false,
            "sf10": false,
            "sf11": false,
            "sf12": false
        }, {
            "enable": true,
            "radio": 0,
            "offset": 200000,
            "bandwidth": "",
            "sf7": false,
            "sf8": false,
            "sf9": false,
            "sf10": false,
            "sf11": false,
            "sf12": false
        }, {
            "enable": true,
            "radio": 0,
            "offset": 400000,
            "bandwidth": "",
            "sf7": false,
            "sf8": false,
            "sf9": false,
            "sf10": false,
            "sf11": false,
            "sf12": false
        }
    ],
    "lora_std": {
        "enable": true,
        "radio": 1,
        "offset": -200000,
        "bandwidth": "250kHz",
        "spread_factor": "SF7"
    },
    "fsk": {
        "enable": true,
        "radio": 1,
        "offset": 300000,
        "bandwidth": "125kHz",
        "datarate": 50000
    },
    "gain_lut": {
        "size": 16,
        "entries": [{
                "dig_gain": 0,
                "pa_gain": 0,
                "dac_gain": 3,
                "mix_gain": 9,
                "rf_power": -1
            }, {
                "dig_gain": 0,
                "pa_gain": 0,
                "dac_gain": 3,
                "mix_gain": 11,
                "rf_power": 2
            }, {
                "dig_gain": 0,
                "pa_gain": 0,
                "dac_gain": 3,
                "mix_gain": 13,
                "rf_power": 4
            }, {
                "dig_gain": 0,
                "pa_gain": 0,
                "dac_gain": 3,
                "mix_gain": 15,
                "rf_power": 6
            }, {
                "dig_gain": 0,
                "pa_gain": 1,
                "dac_gain": 3,
                "mix_gain": 8,
                "rf_power": 8
            }, {
                "dig_gain": 0,
                "pa_gain": 1,
                "dac_gain": 3,
                "mix_gain": 9,
                "rf_power": 10
            }, {
                "dig_gain": 0,
                "pa_gain": 1,
                "dac_gain": 3,
                "mix_gain": 10,
                "rf_power": 11
            }, {
                "dig_gain": 0,
                "pa_gain": 1,
                "dac_gain": 3,
                "mix_gain": 11,
                "rf_power": 13
            }, {
                "dig_gain": 0,
                "pa_gain": 1,
                "dac_gain": 3,
                "mix_gain": 12,
                "rf_power": 15
            }, {
                "dig_gain": 0,
                "pa_gain": 1,
                "dac_gain": 3,
                "mix_gain": 13,
                "rf_power": 17
            }, {
                "dig_gain": 0,
                "pa_gain": 1,
                "dac_gain": 3,
                "mix_gain": 15,
                "rf_power": 19
            }, {
                "dig_gain": 0,
                "pa_gain": 2,
                "dac_gain": 3,
                "mix_gain": 11,
                "rf_power": 21
            }, {
                "dig_gain": 0,
                "pa_gain": 2,
                "dac_gain": 3,
                "mix_gain": 12,
                "rf_power": 23
            }, {
                "dig_gain": 0,
                "pa_gain": 2,
                "dac_gain": 3,
                "mix_gain": 14,
                "rf_power": 25
            }, {
                "dig_gain": 0,
                "pa_gain": 2,
                "dac_gain": 3,
                "mix_gain": 15,
                "rf_power": 26
            }, {
                "dig_gain": 0,
                "pa_gain": 3,
                "dac_gain": 3,
                "mix_gain": 10,
                "rf_power": 27
            }
        ]
    }
}
```

In order to transfer to the router, in Ubuntu you have to install tftp from apt in order to transfer these files:

```shell
$ sudo apt install tftp
$ tftp
tftp> connect 192.168.1.50
tftp> put Customer.json Customer.json
tftp> put LoRaWAN.json LoRaWAN.json
```

If you are using Windows you can use [tftpd64](http://tftpd32.jounin.net/tftpd32_download.html)

If everything is okay, the power LED will stop blinking and come fixed, also it will appears as "Connected" in the TTN Console. If you didn't get lucky may be you need a firmware update... Try to read *Status.json* through TFTP in order to see if you are running the latest release (1.7 (release) 2019-09-13) at the time of writing this post.

# Conclusions

For around [140€](https://connectedthings.store/gb/lorawan-gateways/indoor-lorawan-gateways/tektelic-kona-micro-lite-iot-lorawan-gateway.html?SubmitCurrency=1&id_currency=2) this is a good option for a low cost 8 channel LoRaWAN gateway from a reputed brand as Tektelic is. I know there are more options on the market this days and you can probably find better options as the MikroTik with it also includes PoE and WiFi. May be on the future I can try to do some performance tests comparing this two in order to see which ones performs better.