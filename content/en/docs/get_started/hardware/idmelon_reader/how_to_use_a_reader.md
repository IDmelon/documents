---
title: "How to Use a Reader?"
description: "How to set up and use the IDmelon Reader for passwordless authentication."
lead: ""
date: 2022-02-16T18:14:02-08:00
lastmod: 2022-02-16T18:14:02-08:00
draft: false
images: []
menu:
  docs:
    parent: "idmelon_reader"
weight: 402000
toc: true
---

## Discover working Mode

---

You can discover the working mode of the reader by observing the color of the LED, at the time you insert the hardware into the USB port.
Once the hardware is plugged into USB, the LED flashes once on the colors of green, blue, or white.

- The White means Smartphone BLE Reader mode.
- The Green means FIDO2 USB Security key mode.
- The Blue means Paired Smartphone mode.

The user needs to push and keep the button for 3 seconds, and then the LED starts blinking. As soon as blinking stops,
the IDmelon USB key is ready to work in a new mode.

## Switching between working modes

---

You can switch between the different mentioned working modes by pressing and keeping the push button for 3 seconds. Switching to a new mode depends on the current mode. You can only switch from **security Key mode** to **reader mode** or **paired mode**, and when the reader is in **paired mode** or **reader mode**, you can only switch to **security key mode**. It means if you want to switch from **paired mode** to **reader mode**, first you have to switch to **security key mode**, and then from **security key mode** to **reader mode**.

How the switching works:

1. Click and hold the push button for 3 seconds; you can release the button when you hear a beep sound, and the LED light starts blinking.
2. If the LED blinks green, it means the device is in the process of switching to **security key mode**. You need to wait for about 15 seconds until it stops blinking.
3. If the LED blinks Blue, it means the device is ready to switch to **reader mode** or **paired mode**. You can switch to **paired mode** if you have **IDmelon Key** app installed on your smartphone; otherwise, the reader automatically switches to **reader mode**. It starts blinking Blue for about 15 seconds until it blinks white once at the end.
