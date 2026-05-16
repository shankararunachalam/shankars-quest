---
title: "Norns in hand: first power-on"
date: 2026-05-12
trails: "music"
tags: ["norns", "hardware", "carnatic"]
summary: "Three years of planning compressed into the moment of pressing the boot button. Notes from Phase 0 of the music dev journey."
---

The Norns Shield arrived at Superbooth in a small white box that fit in my coat pocket. I carried it back through three flights and put it on the desk in my study without ceremony, because the ceremony was supposed to be the first power-on, and I wanted that to happen at home with a proper screen and a proper cup of tea.

So: tea, screen, USB-C cable, and the soft satisfying click of the boot button. The screen flickered, drew a logo, and the Norns was alive in my hands for the first time.

{{< break >}}

## What surprised me

The form factor is smaller than I'd remembered from videos. Not pocket-size, but desk-size in a way that suggests "instrument" rather than "computer." The encoders have a satisfying detent — clicky in a measured way, not the loose feel of cheap pots. The screen is OLED and crisp, which matters because the entire UX of the device lives there.

The thing I hadn't appreciated from documentation alone: how much of the Norns experience is *modal*. You're never on a generic desktop. You're always in *something* — a script, a menu, a parameter view. That's a design choice with consequences for how I'll build the rhythmic engine. Modes are good for instruments. Modes are bad for general-purpose tools. The Norns has staked out its position clearly.

{{< break >}}

## What I did first

Hello-world. Of course.

```lua
-- hello.lua: prove the screen and encoders work
function init()
  redraw()
end

function enc(n, d)
  -- print which encoder turned and by how much
  print("enc " .. n .. " " .. d)
  redraw()
end

function redraw()
  screen.clear()
  screen.move(64, 32)
  screen.text_center("sam · " .. util.time())
  screen.update()
end
```

Pasted into the script, hit play, watched the screen update with a timestamp every redraw. Trivial code, but the loop closes: edit, deploy over SSH, see the result on the screen, turn an encoder, watch the print output. The development feedback loop works. Phase 0 is real.

{{< break >}}

## What's next

Verify the Grid firmware. The 2010–2012 monobright generation may need an update to play cleanly with current serialosc — that's the next yak to shave before Phase 1 begins. Once that's done, the rhythmic engine architecture starts: tala-native event addressing, shared clock, multi-channel output abstraction, the whole forward-compatible-for-melody design.

But for now — the device is on the desk, the script editor works, and the long planning phase has crossed into making. That's enough for one evening.
