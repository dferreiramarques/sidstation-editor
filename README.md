# SidStation Editor

A free sound editor and patch librarian for the **Elektron SidStation** (1999), the MOS 6581 SID-chip synthesizer that never had a computer editor. It runs in the browser and talks to the synth over MIDI, wrapped in a Commodore 64 style CRT monitor.

<!-- Add a screenshot here, e.g. ![SidStation Editor](screenshot.png) -->

## Features

- **Live editing.** Every knob and switch is sent to the SidStation as you move it, so the hardware itself is the preview. With **Auto note** on, each edit also plays a short test note on the unit — the note is held while you drag and replayed when you toggle a switch — so you hear every change immediately. Turn **Live** off to edit freely, then press **Send to edit buffer** to audition the whole patch.
- **Every parameter on screen.** Three oscillators (waveform, sync, ring mod, pitch, envelope, pulse width, vibrato, table speed), the filter and its envelope, four LFOs (shape, speed, depth, sample & hold, lace, mixing, control source) and the front-panel knob assignments.
- **Wavetable editor.** Build the step tables used for C64-style drums and arpeggios, with presets for kick, snare, hi-hat, Rob Hubbard-style attacks and chord arps.
- **Starting points.** Templates for PWM lead, sync lead, ring-mod bell, acid bass, poly pad and drum sounds, plus **Mutate** and **Random** per section.
- **Patch library.** Receive a single patch or the whole 100-patch bank from the unit, store patches back, and open or save `.syx` files, including Elektron's factory soundset.
- **Play from the browser.** On-screen keyboard (or computer keys A–K, Z/X for octave), mod wheel, pitch bend, the four front knobs (CC 16–19) and LFO controls (CC 20–23).
- **Undo** for templates, randomising and loads.
- **CRT look.** Scanlines and glow (can be switched off), a working power button, an indicator light that flickers with MIDI traffic, channel buttons 1–7 to switch pages (also keys 1–7), and a C64 loading screen during transfers.

## Requirements

- **Chrome or Edge on a computer.** The editor uses Web MIDI with SysEx, which Safari and iOS don't support.
- A MIDI interface connected both ways: interface **Out → SidStation In** and **SidStation Out → interface In** (needed to receive dumps).
- SidStation OS 1.1 or later. OS R33 gives the most complete MIDI control.

## Getting started

1. Open `sidstation-editor.html` in Chrome or Edge, or host it on GitHub Pages.
2. Press **Connect MIDI** and allow MIDI with SysEx when the browser asks.
3. Choose your MIDI in and out ports and the SidStation's base channel (System › MIDI › BaseCh on the unit).
4. **Receive one patch from the unit first** (System › MIDI › SYSEXDump › Patch). The editor learns two details of the dump format from it (see below).
5. Edit with Live on: every change is sent to the unit straight away, and Auto note plays a test note so you hear it. Hold a key on the on-screen keyboard or play your own keyboard if you prefer. Nothing is kept on the unit until you store it, with the SidStation's own Store or with **Store on SidStation**.

To back up the unit, go to channel 6 (Library), press **Receive bank from SidStation**, then on the unit choose System › MIDI › SYSEXDump › **AllPatch**.

## How it works

Everything is based on the MIDI implementation in the SidStation owner's manual (rev 2.2b, OS 1.1):

- **Live edits** use the documented *Direct Program* SysEx messages, which write single parameters into the current patch — masked parameters now follow the manual's Direct Program chart exactly (including the envelope and select nibble masks, the combined filter-routing sequence `00 18 07 00`, and sync / ring mod as the documented CCs).
- **Patch dumps** use the documented dump format: name in ASCII, then patch data as high/low nibbles, followed by the three wavetables.
- Messages are queued and spaced out (adjustable in Setup) because the SidStation is known to struggle with fast SysEx.

The table-programming messages produced by the editor match the worked example in the manual byte for byte.

### Where the manual is unclear

- **Table pitch.** The manual contradicts itself on which byte range means "add semitones" and which "subtract". The editor follows the worked example. If table steps play in the wrong direction, switch on **Swap table + / − encoding** in Setup.
- **Dump size and negative values.** The size field and the encoding of negative transpose/detune are not fully specified. The editor learns both from the first patch you receive.
- **Front knob targets** are stored as numbers (0–96); the manual doesn't list what each number controls.

## Tips

- Back up your bank before sending anything large to the unit.
- If the SidStation hangs during a bank transfer, raise the gap between dumps in Setup and try again.
- Settings, the current patch and the library are kept in the browser (localStorage) between visits.

## Files

A single self-contained HTML file. Fonts (Press Start 2P, VT323) load from Google Fonts; without internet the page falls back to a system monospace font.

## Status

Built from the manual and tested against its worked examples and a simulated device. Reports from real hardware are very welcome.

## Support

If this editor is useful to you, you can [buy me a coffee on Ko-fi](https://ko-fi.com/dferreiramarques).

## Disclaimer

Not affiliated with or endorsed by Elektron. SidStation is a trademark of Elektron Music Machines AB. Use at your own risk and keep backups of your patches.

## License

<!-- Choose a license, e.g. MIT, and add a LICENSE file. -->
