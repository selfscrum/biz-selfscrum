+++
title = "Home Office Video IT"
date = "2021-02-14T00:00:00"
image = "blog/green.jpg"
description = "Workplace equipment for virtual collaboration"
disableComments = true
Author = "Martin"
+++

## The virtual workplace is quite real

.

For me, the Corona period didn't bring much new in terms of work - even before that I worked from home a lot, occasionally in video conferences, mostly via conference calls and screen sharing.

However, the boom of Zoom and comparable other systems like Teams, Jitsi, BigBlueButton has changed communication in the meantime. Only rarely do people still make phone calls, most of the time they chat right away via video call. That's why I recently decided to finally equip my workplace in such a way that I can optimally support this way of working. I wanted

* A better video image
* The possibility to create virtual backgrounds better than the software of the video meeting providers can do
* Easily switch between different input sources or mix them together
* A way to record the resulting video stream
* A teleprompter
* Reasonable lighting
* The ability to quickly switch between different recording scenarios.

Sounds elaborate! What took the longest was finding concepts and the right tools. Once I got those together, the setup went along pleasantly quickly. Not perfect yet, but a good start with which I will now gain experience. 

![home_office_it diagram](https://res.cloudinary.com/dzw4emsdt/image/upload/c_scale,w_900,q_auto/v1613335266/selfscrum/Portfolio-Digital_Consulting_mfjsmx.png)

The circuit of the connections results from the diagram. The decisions for the connection technique result from the available hardware:

* My Windows notebook has only one HDMI output. Fortunately, I have a small docking station connectable via USB-C that has a second HDMI output in addition to an RJ-45 Ethernet jack. A relatively powerful system is important for video. I am very happy with my Lenovo ThinkPad P1 Gen 3 performance-wise, but the fan is relatively loud and also jumps on more frequently during video processing. I will probably banish computer under the desk so that the acoustics do not suffer.
* The two screens are the same model with a relatively high resolution. Important more for my Adobe Creative Sessions than for video conferencing.
* Keyboard and mouse by Logitech from the MX series - finally a keyboard where not all important keys are hidden behind double assignments. Bluetooth operation runs wonderfully, even without the USB dongle, which is always lying unused on the desk in the way.
* Router, Fritzbox, printer, all standard and not particularly exciting.
* Elgato Stream Deck as a programmable additional keyboard. Elgato comes from the gamer and video scene, you can tell that from the devices. Ultimately solid technology, a bit overpriced but well usable.
* Elgato Key Light is a video light that has its own WLAN access and can thus be controlled remotely. Unfortunately, only in 2G WLAN standard and unfortunately only via a proprietary Elgato protocol. The setup hobbles for a while until everything fits, but then it runs stably in operation. It can be controlled well via the Stream Deck. The color temperature can be adjusted. For the Green Wall at a distance of 1.50 meters, one light is just enough, in the shadier areas the screen flickers a bit. Two would be better - but at over 200 EUR, the lamp is no bargain...
* The "Green Wall" in my case is an easily rollable stand screen from celexon. From Elgato there is also a slightly cheaper variant, but on the one hand, it is not available right now - too much demand. And besides, the protruding feet bother me there, which can become tripping hazards. 
* The main cam is a Logitech StreamCam, a relatively new device with decent display quality and USB-C cable. I briefly considered using my Nikon DSLR, but decided against it. Even more tech components that need to be managed again - that's where the webcam is good enough for everyday use.
* Since I live in the Apple universe with my mobile devices, the second cam and also the teleprompter are located on these devices. an iPad serves as a display engine for the text, if it should be something prefabricated. The iPhone controls the teleprompter (optionally also via a browser window. I would like to have a REST interface, but most client apps are not ready yet) or serves as a second camera. And that brings us to the video processing structure shown in the next image.

![OB and NDI](https://res.cloudinary.com/dzw4emsdt/image/upload/c_scale,w_900,q_auto/v1613339559/selfscrum/Portfolio-Digital_Consulting_ckt3dt.png)

I use NDI, which is an open video streaming standard that uses WLAN as a carrier. This sounds rather normal for IT people, but in the video industry this has been a paradigm shift, as video used to be transmitted via proprietary line systems. For me, this means that I can use my iPhone or iPad as an input source just like a webcam. NDI as a manufacturer offers many more utilities, for example screen capturing as well.

The most important system is undoubtedly OBS, Open Broadcasting Studio, which is very good as a video effects and mixing system to merge and recombine the different video sources. Conveniently, there is an NDI plugin so that WLAN image integration is secured and also an Elgato Stream Deck plugin provides better control options. This way, certain configurations can be retrieved at the push of a button.

The actual function comes from compiling different video sources into scenes. Much like Photoshop or similar programs, each source is a layer that can be manipulated individually through positioning, scaling and filters. A filter, for example, also filters out the green screen, so that the filmed person can then be further used with a different background. The layers are superimposed to create a scene, which can then be forwarded via various output paths, for example to a video conference. Conveniently, scenes can also be used again as a data source, so that you can create a small scene library that can then be used again and again in other contexts.

Of course, audio is also managed here. However, this is another chapter in itself, so I will go into it separately.

Functionally, I'm very happy with this combination now because it covers all the requirements I have. There are still a few integration hiccups to overcome, for example the increased use of the stream deck causes the webcam (or the OBS driver for it) to hang at some point. But these are teething troubles that will grow out. The next show can begin!




