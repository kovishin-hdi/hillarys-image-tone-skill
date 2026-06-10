# Hillarys Image Tone Guide

A comprehensive specification for color grading and editing raw images to match the Hillarys Curtains & Blinds brand aesthetic.

---

## 1. Brand Tone Overview

**Brand**: Hillarys Curtains & Blinds
**Visual Identity**: Premium home comfort — warm, inviting, and aspirational without being cold or clinical.

### Mood Keywords
Warm | Soft | Inviting | Clean | Premium | Homely | Cozy | Natural | Approachable | Refined

### Core Visual Principles
- Images should feel like a soft afternoon — gentle natural light, gentle shadows, nothing harsh
- Colors are present but never loud; the palette whispers rather than shouts
- Skin tones are always flattering — natural, warm, healthy
- Fabrics and textures look tactile and rich without oversaturation
- The overall feel is "lived-in luxury" — not sterile showroom, not messy reality

---

## 2. Color Profile Parameters

### Color Temperature
- **Direction**: Warm shift
- **Degree**: Gentle warm (+8 to +12 on a -100 to +100 scale)
- **Cast**: Creamy/neutral-warm — avoid yellow or orange; the warmth should feel like soft natural light, not golden hour
- **Kelvin equivalent**: Aim for ~5600-6000K feel (just slightly warmer than neutral daylight)

### Highlights
- **Character**: Soft, clean, subtly warm
- **Recovery**: Pull back any blown-out highlights slightly (-10 to -20)
- **Tint**: Very subtle warm tone in highlights — never yellow, and never cool blue/white
- **Goal**: Highlights should glow, not glare

### Shadows
- **Lift**: Moderate lift (+15 to +25) — shadows should never be pure black
- **Tone**: Slightly warm shadows (hint of brown, not amber or yellow)
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
- **Cast**: Natural-warm — healthy and clean, not overly golden
- **Saturation**: Slightly boosted relative to overall image (+5 selective on orange/skin channel)
- **Luminance**: Slightly bright, glowing quality
- **Avoid**: Redness, sallowness, or overly tanned/orange

### White Balance
- **Temperature**: +6 to +10 warm (toward amber, but restrained — avoid yellow cast)
- **Tint**: +2 to +3 toward magenta (prevents green cast)

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
    // Step 1: Subtle warm color temperature shift
    .modulate({
      brightness: 1.02,      // Slight brightness lift
      saturation: 0.87,      // Global desaturation (-13%)
      hue: 5                 // Gentle warm hue rotation (degrees)
    })
    // Step 2: Subtle warm tint overlay (creamy, not golden)
    .tint({ r: 240, g: 230, b: 215 })
    // Step 3: Gamma correction for lifted shadows
    .gamma(1.7, 1.65)        // Lifts shadows, slightly warm-biased
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
    // Subtle warm tint
    .tint({ r: 240, g: 230, b: 215 })
    // Modulate: desaturate, slight brightness, gentle hue shift
    .modulate({
      brightness: 1.02,
      saturation: 0.85,
      hue: 6
    })
    // Lift shadows via gamma
    .gamma(1.7)
    // Soft contrast
    .linear(0.88, 16)
    // Very subtle warm color overlay using composite
    .composite([{
      input: {
        create: {
          width: metadata.width,
          height: metadata.height,
          channels: 4,
          background: { r: 250, g: 240, b: 220, alpha: 0.04 }
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
| `modulate.brightness` | 1.02 | Slight overall lift |
| `modulate.saturation` | 0.85-0.87 | Muted, understated colors |
| `modulate.hue` | 5-6 | Gentle warm hue shift (not yellow) |
| `tint` | rgb(240, 230, 215) | Subtle creamy cast |
| `gamma` | 1.65-1.7 | Lifted shadows |
| `linear.a` | 0.88-0.9 | Reduced contrast |
| `linear.b` | 15-16 | Lifted black point |
| `sharpen.sigma` | 0.8 | Clean, not over-sharpened |
| Overlay alpha | 0.03-0.04 | Very subtle warm wash |

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

COLOR TEMPERATURE: Shift subtly warm — the image should feel like soft natural daylight, not golden hour. Add a gentle, creamy warmth throughout. Avoid any yellow, orange, or golden cast. Also avoid cool or blue tones. The warmth should be barely noticeable — just enough to feel inviting without looking tinted.

SHADOWS: Lift the shadows moderately. No area should be pure black. The darkest tones should be a warm charcoal, creating a soft, matte, film-like quality.

HIGHLIGHTS: Soften the highlights. They should feel clean and creamy — not yellow, not harsh white, not blown out.

SATURATION: Reduce overall saturation by about 12-15%. Colors should be present but muted and understated. Warm tones (oranges, warm reds) should retain slightly more saturation than cool tones (blues, greens), which should be noticeably muted. Yellows should also be muted to avoid a yellow cast.

CONTRAST: Reduce contrast slightly. The image should have soft, gradual tonal transitions. No harsh edges between light and dark areas.

SKIN TONES (if people are present): Skin should look natural and healthy with a subtle warmth. Slightly luminous. Never red, sallow, overly orange, or overly golden/yellow.

OVERALL FEEL: The final image should feel softly warm, inviting, premium, and clean — like a high-end home lifestyle magazine. Think "lived-in luxury." The editing should be invisible — it should look naturally beautiful, not tinted or filtered. If it looks yellow or obviously warm, the warmth has been pushed too far.

Do NOT: Oversaturate, add vignette, crush blacks, make it look cold/clinical, add grain, make it look yellow/golden, or make it look overly processed.
```

### Shorter Prompt (for quick use)

```
Apply a subtle warm, premium home lifestyle grade to this image: lift shadows to matte charcoal (no true blacks), add a gentle creamy warmth (not golden or yellow), reduce saturation by 12% (keep warm tones, mute cool tones and yellows), soften contrast, keep highlights clean and soft. The result should feel like soft natural light in a cozy, refined home — natural and inviting, never tinted, yellow, or obviously filtered.
```

---

## 5. Lightroom / Manual Editing Reference

### Basic Panel

| Slider | Value | Notes |
|--------|-------|-------|
| Temperature | +6 to +10 | Gentle warm direction (avoid yellow cast) |
| Tint | +2 to +3 | Slight magenta to prevent green cast |
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
| Red | +3 | Slight push toward orange |
| Orange | 0 | Keep natural |
| Yellow | -3 | Slight push toward orange |
| Green | -5 | Push greens toward yellow (subtle) |
| Blue | -3 | Slight mute |

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
| Highlights | 40 (warm cream) | 6-8 | Subtle creamy highlights (not golden) |
| Midtones | 30 (warm neutral) | 3-5 | Very subtle warmth |
| Shadows | 35 (warm brown) | 5-8 | Slightly warm shadows, never cool |

**Balance**: +5 (slight bias toward highlights)

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
- Can push the creamy warmth very slightly further (but still avoid yellow)
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
| Cool/blue daylight | Increase temperature shift to +12-15 |
| Already warm/golden hour | Reduce temperature shift to +2-5 (image is already warm — don't stack) |
| Flash/artificial light | Add moderate warmth (+12), increase shadow lift |
| Overcast/flat light | Standard parameters, add slight exposure lift (+0.3) |
| Harsh midday sun | Reduce highlights more (-30), increase shadow lift (+35) |
| Indoor tungsten | Reduce temperature shift to +2-3 (already warm), focus on saturation and contrast |

### Quality Checklist (per image)
- [ ] No true blacks anywhere — darkest tone is warm charcoal
- [ ] Highlights are creamy, not blown or cool
- [ ] Skin tones (if present) look natural-warm and healthy (not yellow/golden)
- [ ] Colors are muted but not dead — warmth is preserved
- [ ] Fabrics show texture and look tactile
- [ ] Overall mood: warm, inviting, premium
- [ ] No visible color banding or artifacts
- [ ] Image is clean and sharp without being over-processed

---

## Quick Reference Card

```
HILLARYS TONE IN 6 MOVES:

1. WARM IT       → Temperature +8, subtle cream (not golden/yellow)
2. LIFT BLACKS   → Shadows +20, never true black
3. MUTE COLORS   → Saturation -12, mute blues/greens/yellows
4. SOFTEN        → Contrast -12, clarity -8
5. CLEAN GLOW    → Highlights soft/creamy, skin natural-warm
6. SHARPEN       → Light sharpen, noise reduction
```
