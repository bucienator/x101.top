---
title: "How I Did Not Reflash My Smart Plug with ESPHome"
date: 2023-01-19T21:34:30+01:00
tags: ["smart plug", "esp8266"]
---

I'm just a little bit into home automation, really, I have a HomeAssistant 
server running, but the only useful thing it does is log the temperature
in our winter garden.

Last fall I managed to insulate the small wooden house we call the winter garden,
so we have a chance to heat it when it is cold outside and give a chance to
our more sensitive plants to survive. So I also bought a small heater with a built-in
thermostat.

It turned out that the thermostat's minimum setting keeps the temperature between
8-9 °C, which is probably good for the plants, but not as good for our energy bills, and
so here came the idea: let's automate the heater, leverage HomeAssistant, and create a real
and useful automation to limit the temperature to 3-5 °C. That should be still fine for
the plants, they just don't want to freeze.

{{< figure
  src="temperature chart.png"
  caption="8-9 °C is nice and warm when it is freezing outside."
>}}

Now that sounds easy, just need to choose a supported wifi smart plug, and set up the automation.
But then it turned out, getting a supported smart plug isn't that straightforward where I live - or
maybe I was looking for some adventure? Anyway, my choice fell on a device that isn't supported by
HomeAssistant right away, but according to the available documentation could be reflashed with
ESPHome. It was the BlitzWolf BW-SHP6.

That plug uses the esp8266 SoC, and I found this documentation showing how to open it up,
and what connections need to be made: https://tasmota.github.io/docs/devices/BlitzWolf-SHP6/
I was happy to see that it already mentioned two versions of the plug, so I hoped the one I
ordered will be one of these.

It wasn't...

{{< figure
  src="blitzwolf front.jpg"
  caption="Front view of the plug, the receptacle part. There's already one difference, all the specs of the plug are written on a sticker on the bottom of the receptacle."
>}}
{{< figure
  src="blitzwolf side.jpg"
  caption="Side view of the plug, the button."
>}}

There was no screw to remove anywhere on the plug, which should have been the first step in the guide. The whole thing seemed to be glued together into one piece. But then, with it's own
software there was no way to integrate it with HomeAssistant, so buying it would have been pointless.

I was dedicated to open it up and reflash the thing, even if required irreversible damage. It seemed like
there was a single way this could be opened, and that was removing the transparent plastic part from the
receptacle end of it. I just hoped, that only the rim, where the white and transparent plastics meet was
glued, so I took a saw, and sawed around the perimeter, just going into the first millimeter.

{{< figure
  src="front without sticker.jpg"
  caption="After removing the sticker, there's still no screw to remove."
>}}
{{< figure
  src="used the saw.jpg"
  caption="Carefully sawed around the perimeter, just enough to cut through the white part, but not the transparent inner layer."
>}}
{{< figure
  src="transparent plastic pulled out.jpg"
  caption="Voila! The case opened!"
>}}

I was lucky this time, after carefully going around the full perimeter, I could remove the transparent plastic.
But that in itself wasn't enough to reach the internals of the plug, there was no access to the electronics.
And once again, it seemed there was no way to reversibly go further. The ground contacts from the plug and receptacle
were soldered together holding the electronics in place. Once again, there was no other way, to go further
than to break the soldered parts. I can resolder them when reassembling. And that worked, I could finally pull out
the electronics from the case. It felt like finally I'm getting to the point where I'll start building something
instead of just breaking things.

{{< figure
  src="electronics exposed.jpg"
  caption="Electronics exposed! But wait, that's no esp8266!"
>}}

But then this time I wasn't lucky. It turned out the new version of the plug is not only built differently to
make disassembly more difficult - or more probably to bring the cost of material lower - but it wasn't based on
an esp8266 anymore... The internals looked completely different compared to what I anticipated based on the
Tuya documentation, there were no labeled connection points, and the SoC was also something I never heard of,
and what's even more strange, I couldn't even find any information about it.

The label says W701H, probably the model of the chip, and K71RYH1 and GK34T on the next two lines, but probably these
are more like batch numbers. Any search for these strings didn't come up with any results. Looking for candidates,
I stumbled upon the Chinese Winner Micro shop, they have a WiFi SoC series W600, and another series W800. So actually
this W701 would fit into their line of products, but then I couldn't find any reference to W701 on their site at all.

{{< figure
  src="w701h soc.jpg"
  caption="The mystery SoC."
>}}
{{< figure
  src="power meter ic.jpg"
  caption="The other side of the module is more straightforward, the BL 0937 is a power meter ic, which google finds for me. There are also some pads exposed."
>}}


How weird? I just recently saw a Reddit post, where someone asked about a component on a PCB, and got a reply on
how easy is to identify components based on their labels. But then it seems there are exceptions to that rule.

And of course, after going through all this, I found a post on the HomeAssistant forums, about the new version
of BW-SHP6: https://community.home-assistant.io/t/blitzwolf-shp6-pro-new-design-no-longer-has-esp-chip/425199

Now I know too.
