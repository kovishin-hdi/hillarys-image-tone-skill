# Hillarys Image Tone Guide

A comprehensive specification for color grading and editing raw images to match the Hillarys Curtains & Blinds brand aesthetic.

---

## 1. Brand Tone Overview

**Brand**: Hillarys Curtains & Blinds
**Visual Identity**: Premium home comfort — warm, inviting, and aspirational without being cold or clinical.

### Mood Keywords
Warm | Soft | Inviting | Clean | Premium | Homely | Cozy | Natural | Approachable | Refined

### Core Visual Principles
- Images should feel like a warm afternoon — golden light, gentle shadows, nothing harsh
- Colors are present but never loud; the palette whispers rather than shouts
- Skin tones are always flattering — peachy, golden, healthy
- Fabrics and textures look tactile and rich without oversaturation
- The overall feel is "lived-in luxury" — not sterile showroom, not messy reality

---

## 2. Color Profile Parameters

### Color Temperature
- **Direction**: Warm shift
- **Degree**: Moderate warm (+15 to +20 on a -100 to +100 scale)
- **Cast**: Golden/creamy — avoid orange; the warmth should feel like natural golden-hour light
- **Kelvin equivalent**: Aim for ~6000-6500K feel (slightly warmer than neutral daylight)

### Highlights
- **Character**: Creamy, soft, warm-tinted
- **Recovery**: Pull back any blown-out highlights slightly (-10 to -20)
- **Tint**: Subtle warm/peach tone in highlights — never cool blue/white
- **Goal**: Highlights should glow, not glare

### Shadows
- **Lift**: Moderate lift (+15 to +25) — shadows should never be pure black
- **Tone**: Warm-tinted shadows (slight brown/amber in dark areas)
- **Character**: Matte, faded quality — think film stock, not digital
- **Goal**: You should be able to see detail in shadow areas; nothing disappears into darkness

### Contrast
- **Level**: Low to medium (reduce from default by ~10-15)
- **Transitions**: Soft, gradual — no harsh light-to-dark edges
- **Tone curve**: Gentle S-curve with lifted blacks and pulled-down whites
- **Goal**: Flat enough to feel soft, but enough separation to not look washed out

### Saturation & Vibrance
- **Global Saturation**: Slightly reduced (-10 to -15 from neutral)
- **Vibrance**: Low-medium (-5 to -10)
- **Selective preservation**: Warm tones (oranges, yellows, warm reds) retain more saturation than cool tones
- **Blues/Greens**: Desaturate more aggressively (-15 to -20) to keep them muted
- **Goal**: Colors feel natural and understated; nothing looks "Instagram filtered"

### Skin Tones
- **Cast**: Peachy-golden — warm and healthy
- **Saturation**: Slightly boosted relative to overall image (+5 selective on orange/skin channel)
- **Luminance**: Slightly bright, glowing quality
- **Avoid**: Redness, sallowness, or overly tanned/orange

### White Balance
- **Temperature**: +10 to +15 warm (toward amber)
- **Tint**: +3 to +5 toward magenta (prevents green cast, adds warmth to skin)

### Blacks
- **Level**: Faded/lifted — black point raised to ~10-15% (never true 0,0,0 black)
- **Character**: Matte finish — the deepest darks should be dark charcoal, not black
- **Goal**: This is the single most defining characteristic of the Hillarys tone — the faded, lifted blacks create the soft, premium, film-like quality

---

## 3. Sharp.js Parameters (Programmatic)

```javascript
const sharp = require('sharp');

async function applyHillarysTone(inputPath, outputPath) {
  await sharp(inputPath)
    // Step 1: Warm color temperature shift
    .modulate({
      brightness: 1.03,      // Slight brightness lift
      saturation: 0.85,      // Global desaturation (-15%)
      hue: 8                 // Slight warm hue rotation (degrees)
    })
    // Step 2: Warm tint overlay (creamy golden cast)
    .tint({ r: 245, g: 225, b: 200 })
    // Step 3: Gamma correction for lifted shadows
    .gamma(1.8, 1.7)         // Lifts shadows, warm-biased
    // Step 4: Gentle contrast with lifted blacks
    .linear(
      0.9,                   // Reduced contrast multiplier (a)
      15                     // Lifted black point offset (b)
    )
    // Step 5: Slight sharpening for clean finish
    .sharpen({
      sigma: 0.8,
      m1: 0.5,
      m2: 0.3
    })
    .toFile(outputPath);
}
```

### Advanced Sharp.js Pipeline (with channel manipulation)

```javascript
async function applyHillarysToneAdvanced(inputPath, outputPath) {
  const image = sharp(inputPath);
  const metadata = await image.metadata();

  await sharp(inputPath)
    // Warm tint
    .tint({ r: 245, g: 225, b: 200 })
    // Modulate: desaturate, slight brightness, warm hue shift
    .modulate({
      brightness: 1.02,
      saturation: 0.82,
      hue: 10
    })
    // Lift shadows via gamma
    .gamma(1.8)
    // Soft contrast
    .linear(0.88, 18)
    // Slight warm color overlay using composite
    .composite([{
      input: {
        create: {
          width: metadata.width,
          height: metadata.height,
          channels: 4,
          background: { r: 255, g: 235, b: 205, alpha: 0.06 }
        }
      },
      blend: 'over'
    }])
    // Clean sharpening
    .sharpen({ sigma: 0.8 })
    // Output as high-quality JPEG
    .jpeg({ quality: 92, chromaSubsampling: '4:4:4' })
    .toFile(outputPath);
}
```

### Parameter Explanation

| Parameter | Value | Purpose |
|-----------|-------|---------|
| `modulate.brightness` | 1.02-1.03 | Slight overall lift |
| `modulate.saturation` | 0.82-0.85 | Muted, understated colors |
| `modulate.hue` | 8-10 | Warm hue shift |
| `tint` | rgb(245, 225, 200) | Creamy warm cast |
| `gamma` | 1.7-1.8 | Lifted shadows |
| `linear.a` | 0.88-0.9 | Reduced contrast |
| `linear.b` | 15-18 | Lifted black point |
| `sharpen.sigma` | 0.8 | Clean, not over-sharpened |
| Overlay alpha | 0.05-0.07 | Subtle warm wash |

### Batch Processing with Sharp

```javascript
const sharp = require('sharp');
const fs = require('fs');
const path = require('path');

async function batchProcess(inputDir, outputDir) {
  const files = fs.readdirSync(inputDir)
    .filter(f => /\.(jpg|jpeg|png|tiff|webp)$/i.test(f));

  for (const file of files) {
    const inputPath = path.join(inputDir, file);
    const outputPath = path.join(outputDir, `hillarys_${file}`);
    await applyHillarysToneAdvanced(inputPath, outputPath);
    console.log(`Processed: ${file}`);
  }
}
```

---

## 4. AI Prompt Template

### For AI Image Editing Models (GPT-4o, Gemini, etc.)

Use the following prompt when sending a raw image to an AI model for tone-matching:

```
Edit this image to match the Hillarys Curtains & Blinds brand tone. Apply the following adjustments:

COLOR TEMPERATURE: Shift warm — the image should feel like golden afternoon light. Add a creamy, golden cast throughout. Avoid any cool or blue tones.

SHADOWS: Lift the shadows significantly. No area should be pure black. The darkest tones should be a warm charcoal/dark brown, creating a soft, matte, film-like quality.

HIGHLIGHTS: Soften and warm the highlights. They should glow with a creamy, peachy warmth — never harsh white or blown out.

SATURATION: Reduce overall saturation by about 15%. Colors should be present but muted and understated. Warm tones (oranges, yellows, warm reds) should retain slightly more saturation than cool tones (blues, greens), which should be noticeably muted.

CONTRAST: Reduce contrast slightly. The image should have soft, gradual tonal transitions. No harsh edges between light and dark areas.

SKIN TONES (if people are present): Skin should look peachy-golden, warm and healthy. Slightly luminous. Never red, sallow, or overly orange.

OVERALL FEEL: The final image should feel warm, inviting, premium, and soft — like a high-end home lifestyle magazine. Think "lived-in luxury." The editing should be invisible — it should look naturally beautiful, not obviously filtered.

Do NOT: Oversaturate, add vignette, crush blacks, make it look cold/clinical, add grain, or make it look overly processed.
```

### Shorter Prompt (for quick use)

```
Apply a warm, premium home lifestyle grade to this image: lift shadows to matte charcoal (no true blacks), add a creamy golden cast, reduce saturation by 15% (keep warm tones, mute cool tones), soften contrast, warm the highlights to a peachy glow. The result should feel like golden afternoon light in a cozy, refined home — natural and inviting, never filtered or processed.
```

---

## 5. Lightroom / Manual Editing Reference

### Basic Panel

| Slider | Value | Notes |
|--------|-------|-------|
| Temperature | +12 to +18 | Warm/amber direction |
| Tint | +3 to +5 | Slight magenta to prevent green cast |
| Exposure | +0.1 to +0.2 | Slight lift |
| Contrast | -10 to -15 | Soften tonal transitions |
| Highlights | -15 to -25 | Recover and soften bright areas |
| Shadows | +20 to +30 | Lift shadow detail |
| Whites | -5 to -10 | Prevent blowout |
| Blacks | +15 to +25 | The key Hillarys move: fade the blacks |
| Clarity | -5 to -10 | Soften micro-contrast for dreamy quality |
| Vibrance | -5 to -10 | Gentle muting |
| Saturation | -10 to -15 | Global desaturation |

### Tone Curve

```
Point Curve: Custom

Highlights:   (190, 180)   — pulled down slightly
Lights:       (145, 148)   — near linear, very slight lift
Darks:        (75, 85)     — lifted
Shadows:      (25, 38)     — significantly lifted (this creates the matte look)
```

The tone curve should be a **gentle S with a raised floor** — the bottom-left point should not touch the corner. This is the signature "faded blacks" look.

### HSL Adjustments

**Hue:**
| Channel | Shift | Purpose |
|---------|-------|---------|
| Red | +5 | Push reds toward orange/warm |
| Orange | 0 | Keep natural |
| Yellow | -5 | Push yellows toward orange/warm |
| Green | -10 | Push greens toward yellow (warm) |
| Blue | -5 | Mute blues |

**Saturation:**
| Channel | Value | Purpose |
|---------|-------|---------|
| Red | -5 | Slightly mute |
| Orange | +5 | Preserve warmth in skin and fabrics |
| Yellow | -5 | Mute yellows |
| Green | -20 | Heavily mute greens |
| Blue | -25 | Heavily mute blues |
| Purple | -15 | Mute purples |

**Luminance:**
| Channel | Value | Purpose |
|---------|-------|---------|
| Red | 0 | Neutral |
| Orange | +10 | Brighten skin tones |
| Yellow | +5 | Slight lift |
| Green | +5 | Slight lift |
| Blue | -5 | Darken slightly |

### Split Toning / Color Grading

| Zone | Hue | Saturation | Notes |
|------|-----|------------|-------|
| Highlights | 40 (warm gold) | 10-12 | Creamy golden highlights |
| Midtones | 30 (warm amber) | 5-8 | Subtle warmth |
| Shadows | 35 (amber/brown) | 8-12 | Warm shadows, never cool |

**Balance**: +10 (bias warm tones toward highlights)

### Sharpening & Noise

| Parameter | Value | Notes |
|-----------|-------|-------|
| Sharpening Amount | 30-40 | Gentle, clean |
| Radius | 1.0 | Standard |
| Detail | 25 | Not over-sharpened |
| Masking | 60-70 | Protect smooth areas |
| Noise Reduction | 15-20 | Clean finish without losing texture |

---

## 6. Batch Processing Notes

### Image Categories & Adjustments

Different source images need slightly different treatment to arrive at the same Hillarys look:

#### Lifestyle Shots (people in rooms)
- Standard application of all parameters above
- Extra attention to skin tone warmth
- Ensure fabric textures remain visible and rich
- Example reference: "Made for little moments of joy" (child with curtains), "A little chaos, a lot of love" (siblings playing)

#### Product/Detail Shots (fabric close-ups)
- Slightly less shadow lift (+15 instead of +25) to preserve fabric texture depth
- Maintain more contrast to show weave and stitch detail
- Keep saturation closer to neutral for accurate fabric color representation
- Example reference: "Embroidered" (hand touching fabric)

#### Room/Interior Shots (no people)
- Full shadow lift and warmth
- Can push the creamy/golden cast slightly further
- Ensure window light looks natural and inviting
- Example reference: "Layer for better control" (curtains with roman blind)

#### Commercial/Office Settings
- Slightly less warm than residential imagery
- Cleaner, more neutral warmth — less golden, more cream
- Maintain professional clarity
- Example reference: "Light, on your terms" (roller blinds in office)

### Source Lighting Compensation

| Source Condition | Adjustment |
|-----------------|------------|
| Cool/blue daylight | Increase temperature shift to +20-25 |
| Already warm/golden hour | Reduce temperature shift to +5-10 |
| Flash/artificial light | Add extra warmth (+20), increase shadow lift |
| Overcast/flat light | Standard parameters, add slight exposure lift (+0.3) |
| Harsh midday sun | Reduce highlights more (-30), increase shadow lift (+35) |
| Indoor tungsten | Reduce temperature shift to +5 (already warm), focus on saturation and contrast |

### Quality Checklist (per image)
- [ ] No true blacks anywhere — darkest tone is warm charcoal
- [ ] Highlights are creamy, not blown or cool
- [ ] Skin tones (if present) look peachy-golden and healthy
- [ ] Colors are muted but not dead — warmth is preserved
- [ ] Fabrics show texture and look tactile
- [ ] Overall mood: warm, inviting, premium
- [ ] No visible color banding or artifacts
- [ ] Image is clean and sharp without being over-processed

---

## Quick Reference Card

```
HILLARYS TONE IN 6 MOVES:

1. WARM IT       → Temperature +15, golden cast
2. LIFT BLACKS   → Shadows +25, never true black
3. MUTE COLORS   → Saturation -12, kill blues/greens
4. SOFTEN        → Contrast -12, clarity -8
5. GLOW          → Highlights warm/creamy, skin peachy
6. CLEAN         → Light sharpen, noise reduction
```
