# Phase 06: Joystick Audio — Warm Analog Sound

**Milestone:** v1.0 — Launch-Ready Site
**Goal:** Transform the XY pad from a digital raw oscillator engine into a warm, lush, analog-feeling synth — Elka Synthex / Oberheim OB-8 reference.

---

## Sound reference

**Elka Synthex + Oberheim OB-8** — thick analog poly pads. Characteristics:
- Warm, rounded low-mids
- Smooth filter (never harsh or bright)
- Lush plate reverb tail
- Punchy but soft transients
- Rich but not cluttered — space and air

---

## Decisions

### Waveforms
- **Sine + triangle only** — remove sawtooth and square from the random waveform pool
- Existing subtle detune (±1.25 cents, slow drift) stays as-is
- Root voice alternation (sine/square) → change to sine/triangle only
- Warmth is consistent across the entire pad — does not vary with position

### Filter / Brightness
- Current filter sweeps from 2000Hz (centre) to ~9000Hz (edge) — too bright
- **Reduce sweep range significantly** — new range: ~800Hz (centre) to ~2500Hz (edge)
- Filter never fully opens — stays in warm, low-mid territory throughout
- Filter Q stays as-is (0.8 base, rises slightly with brightness)

### Envelope
- **Attack:** ~30ms soft ramp (currently near-instant) — punchy but not harsh
- **Release:** 1s fade when chord lifts (currently ~0.3s)
- Sub-oscillator (one octave down, upper half of pad) retained unchanged

### Reverb
- **Type:** Studio plate — smooth, non-spatial shimmer (not room/hall)
- **Presence:** Medium — clearly audible but not overwhelming (~25–35% wet)
- **Behaviour on chord change:** Old reverb tail fades quickly (~200ms crossfade) — does not ring through new chord
- **Sustain mode:** No difference — same reverb in all modes
- **Implementation:** Algorithmic (Web Audio `ConvolverNode` with generated impulse response, or a simple Schroeder/FDN reverb using delay nodes — no external files)

### Delay
- **Time:** ~200ms (free, not tempo-synced)
- **Character:** Darker than dry signal — low-pass filter on feedback path (~1200Hz cutoff)
- **Repeats:** 3 decaying repeats (~60% feedback per repeat)
- **Presence:** Always on, every chord hit
- **Mix:** Subtle — delay sits behind reverb in the mix

---

## Code context

| Element | Location | Notes |
|---------|----------|-------|
| `initAudio()` | ~line 1857 | Creates `audioCtx`, `filterNode`, `masterGain`, compressor chain |
| `filterNode` | line 1860 | Lowpass, currently 2200Hz base — reduce |
| `playChordNow()` | line 2031 | Filter sweep + oscillator creation — main target |
| `filterFreq` calc | line 2035 | `2000 * Math.pow(4.5, brightness)` — replace |
| `gainScale` | line 2067 | Sawtooth/square path — simplify after waveform change |
| `WAVEFORMS` | line 1881 | `['triangle','sine','sawtooth','square']` — trim to sine+triangle |
| `rootWaveforms` | line 2064 | `['sine','square']` — change to `['sine','triangle']` |
| Attack envelope | line 2055–2056 | Sub-osc attack: `setTargetAtTime(0.42, now, 0.07)` — soften |
| Release / `stopAll` | line 1873 | `setTargetAtTime(0, audioCtx.currentTime, fade)` — increase fade |
| Signal chain | line 1868–1870 | `filterNode → comp → masterGain → destination` — insert reverb + delay |

### Target signal chain
```
oscillators → filterNode → saturation(optional) → delay send → reverb send → masterGain → compressor → destination
```
Or simpler:
```
oscillators → filterNode → masterGain → delay → reverb → compressor → destination
```

---

## Sub-plans

- **06-01:** Waveform pool → sine+triangle only. Filter range reduced (800–2500Hz). Envelope: 30ms attack, 1s release.
- **06-02:** Plate reverb — algorithmic implementation using Web Audio delay nodes (Schroeder allpass + comb filters), ~25–35% wet, fast fade on chord change.
- **06-03:** Delay engine — single `DelayNode` with `BiquadFilter` on feedback (~1200Hz), 3 decaying repeats at ~200ms, always mixed in at low level.
