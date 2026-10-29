# Tuva — beta guide

A short guide for the beta. It covers connecting an FS1R, what each page does, and what to report. Tuva is still changing, so this guide is kept deliberately brief.

**Open Tuva:** https://phonk.github.io/tuva/

---

## 1. Getting started

### What you need

- A Yamaha FS1R.
- A MIDI interface connected both ways: interface **MIDI Out → FS1R MIDI In**, and **FS1R MIDI Out → interface MIDI In**.
- **Chrome or Edge** on a computer. Safari and Firefox don't handle Web MIDI with SysEx reliably.

### On the FS1R

Press **UTILITY** and go to the **MIDI** settings:

| Setting | Set it to |
|---|---|
| `01: Device Num` | `1` (or `all`) |
| `03: RcvBulkDump` | `on` |
| `04: Rcv SysExcl` | `on` |

Also check that the part you want to edit receives on a MIDI channel you can play.

### In Tuva

1. Open the **midi** tab and press **Connect MIDI**.
2. Chrome asks for permission to use MIDI devices, including system exclusive messages. Allow it.
3. Choose your interface in **Output**. Set **Device** to the same number as the FS1R (1 if you left it at 1; any if it is `all`), and **Notes on** to the MIDI channel of the part you want to play.
   You don't need to get **Input** right: Tuva listens on every input and switches to the one the FS1R answers on. Some interfaces number their in and out ports differently (an ESI MUEx answers on *Port 15* to a request sent on *Port 16*).
4. Press **Request part voice**. The voice of part 1 should appear in Tuva: its name in the top bar, its formants on the **spectrum** tab.
5. From now on, every edit is sent to the FS1R as you make it ("Send parameters in real time" is on).

To push a whole voice to the synth, use **Send voice to part**. To play from the computer, hold **Play** in the top bar or use the computer keys `a w s e d f t g y h u j k`: with MIDI connected they send notes to the FS1R. The keys in **morph** and in **jam mode** do the same.

### Sound

- **browser sound on/off** in the top bar switches the approximate preview in the browser. It turns off by itself when you connect an FS1R, so you hear only the synth.
- **stop** (or the **Esc** key) silences everything: the browser preview, the sequencer, and the FS1R (All Sound Off and All Notes Off on all 16 channels).
- Switching to another window releases any key you were holding.

### If nothing happens

| Symptom | Check |
|---|---|
| **Connect MIDI** says access is blocked | The page has to be the real site (phonk.github.io), not a preview inside another app. Check that Chrome didn't block the permission: click the icon at the left of the address bar. |
| No ports in the lists | The interface is not connected or not recognised by the computer. Close other programs that may be holding it (a DAW, a librarian). |
| Request part voice does nothing | After 3 seconds Tuva says whether anything came back. If nothing did: the FS1R's MIDI Out isn't reaching the interface's MIDI In, or `Rcv SysExcl` is off, or the Device numbers differ. The **SysEx traffic** log on the midi tab shows what was sent, what came back and on which input. |
| A sound keeps playing | Press **stop** or **Esc**. If it happens again, tell me which page and which button you used. |
| The FS1R shows "System Exclusive … Error" | Note the exact message and tell me which button you pressed. That is very useful to fix. |
| Edits sound on the preview but not on the synth | Check that **Part** in the top bar is the part you are listening to on the FS1R. |

---

## 2. The pages

**mouth** — Drag the red point across the vowel plane: left to right moves the tongue (i → u), top to bottom opens the mouth. **throat**, **breath**, **brightness** and **width** shape all formants at once. Four slots store mouths and glide between them. The mouth writes operators **V1–V4** (voiced) and **N1–N4** (unvoiced) of the voice as fixed formants; undo takes it back.

**phrase** (on the mouth tab) — Type vowels and a few consonants (`aia-uoi, mama`), or press **record a gesture** and move on the vowel plane. Tuva builds an Fseq. **send to the fs1r** sends a voice wired to it, the Fseq, and the performance settings needed to play it.

**choir** — The 16 operators as singers placed by frequency. Singers standing on the floor reach the output; floating ones modulate the singer below them. Drag a singer up or down to change its level; **solo** and **mute** in its card. Below, all 88 algorithms as diagrams; **suggested** lists those that fit the current voice.

**morph** — Four mouths in the corners. Hold the pad and slide between them; play the keys underneath.

**spectrum** — Every operator as a hill (voiced) or a band (unvoiced). Drag to change frequency and level; wheel or Alt+drag for bandwidth. All parameters of the selected operator are below.

**envelopes** — Drag the points of the amplitude, frequency, pitch and filter envelopes.

**voice** — Algorithm, LFOs, filter, carrier attenuation, which operators follow the Fseq, Formant and FM control.

**fseq** — Draw frequency (upper area) and intensity (lower lane) per track, or load an audio file to analyse. Header settings at the bottom.

**library** — Save voices and Fseqs in the browser, import and export `.syx`.

**midi** — Connection, dumps, and the SysEx traffic log.

**jam mode** (button in the top bar) — A self-contained instrument: four pages (sing, shape, seq, talk), four knobs whose job is printed at the bottom of the screen, a two-octave keyboard. Space starts the sequencer; `a w s e d f t g y h u j k` play notes, `z` and `x` change octave.

### Things worth knowing

- The FS1R steps an Fseq at most once every ~5.2 ms, so a full-resolution Fseq lasts about 2.7 seconds. Longer phrases use slower frames.
- An Fseq only plays when the performance assigns it to a part with play mode "fseq". Tuva sets this up when **Set up the performance** is ticked.
- The sound you hear in the browser is an approximation to help you orient yourself. It is not an FS1R emulation.

---

## 3. What to test and report

Tuva has been checked against [FSVR](https://github.com/musicastudio/FSVR), a software FS1R, but **never on hardware**. These tests matter most, roughly in this order:

1. **Request part voice**: does the voice arrive, with the right name?
2. **Edit live**: drag a formant on **spectrum**. Does the FS1R change, and does its display show an edited voice?
3. **Send voice to part**: does the synth take a whole voice?
4. **Mouth**: drag between a, i and u. Are the vowels recognisable on the hardware?
5. **Algorithm**: pick one in **choir**. Does the FS1R display the same algorithm number?
6. **Fseq**: make a phrase and press **send to the fs1r**. Does it play when you hold a note? Try both *edit buffer* and an *Int* slot.
7. **Request current Fseq**: does an Fseq come back from the synth?
8. **.syx import**: load a voice bank you already have. Do the names look right?

For each test, a short note is enough:

- what you did;
- what happened on the FS1R (sound and display);
- for errors, the exact message on the FS1R display and the last lines of the SysEx traffic log (copy and paste them);
- your browser, operating system and MIDI interface.

Send reports to **[GitHub issues](https://github.com/phonk/tuva/issues)**, or reply wherever you found the invitation. Thank you!
