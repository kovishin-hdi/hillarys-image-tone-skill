# Hillarys Image Tone AI

**Turning usable images into Hillarys-ready visuals.**

---

## What it is

A reusable AI editing instruction system that packages the Hillarys visual identity into a portable, consistent set of rules — so any AI tool, editor, or developer can take a raw image and make it look like it belongs on the Hillarys feed.

The core file is **`Hillarys-Image-Tone-Guide.md`** — a standalone markdown specification you can upload to ChatGPT, Claude, Gemini, or any AI agent. It contains everything needed to colour-grade and tone-match images to the Hillarys Curtains & Blinds brand.

---

## Why it exists

Hillarys has images. Not all are brand-ready.

- Many images come from phones, WhatsApp groups, installers, consultants, or customer homes.
- The content may be useful, but the tone is often too cold, harsh, dark, saturated, or inconsistent.
- Rejecting these images reduces content volume and creates unnecessary dependency on professional shoots.

**This skill solves that.** It upgrades what already exists — applying Hillarys visual identity quickly and consistently, converting average usable images into stronger organic assets, and increasing the usable content bank without increasing shoot cost.

---

## What the tone looks like

Warm. Soft. Premium. Homely.

The skill moves images toward the Hillarys look:
- Creamy, golden highlights
- Lifted shadows (never true black — warm charcoal)
- Muted, understated colour (warm tones preserved, cool tones quietened)
- Faded blacks for a soft, film-like matte finish
- Peachy, healthy skin tones
- A natural "lived-in luxury" feel

---

## What's inside

```
hillarys-image-tone-skill/
│
├── Hillarys-Image-Tone-Guide.md    ← The portable tone spec (use anywhere)
├── CLAUDE.md                        ← Project instructions for Claude Code
├── README.md                        ← You are here
│
└── .claude/
    └── skills/
        └── hillarys-image-tone.md   ← Same spec, loaded as a Claude skill
```

### The Guide covers:

| Section | What it gives you |
|---------|-------------------|
| **Brand Tone Overview** | Mood, visual principles, and the "why" behind every adjustment |
| **Colour Profile Parameters** | Detailed specs: warmth, saturation, contrast, shadow lift, skin tones, black point |
| **Sharp.js Parameters** | Copy-paste Node.js functions for programmatic colour grading |
| **AI Prompt Templates** | Full and short prompts for ChatGPT, Claude, Gemini, or any image-editing AI |
| **Lightroom / Manual Editing** | Exact slider values, tone curves, HSL channels, and split toning settings |
| **Batch Processing Notes** | Per-category adjustments and source lighting compensation tables |

---

## How to use it

### With any AI agent (ChatGPT, Claude, Gemini, etc.)
1. Download **`Hillarys-Image-Tone-Guide.md`** from this repo
2. Upload it as context/instructions alongside your raw image
3. Ask the AI to edit the image following the guide
4. The AI Prompt Templates section has ready-to-use prompts

### With Claude Code (as a skill)
The `.claude/skills/hillarys-image-tone.md` file is automatically loaded when Claude Code works in this repo — no upload needed.

### With Sharp.js (programmatic)
Copy the functions from Section 3 of the guide. Run them against any image directory for automated batch processing.

### With Lightroom / manual editing
Section 5 has every slider value, tone curve point, and HSL adjustment you need to replicate the look by hand.

---

## How the workflow runs

1. **Collect** — Bring in raw images from installs, customer homes, consultants, site visits, BTS captures, or WhatsApp drops.
2. **Select** — Choose images with a usable moment, angle, product, room, or story. The source image must still have content value.
3. **Apply Skill** — Run the Hillarys Image Tone instruction set to warm, soften, lift shadows, mute colours, and align the visual mood.
4. **Review** — Check whether the edited image feels warm, natural, premium, soft, and usable — not fake, over-filtered, or campaign-heavy.
5. **Deploy** — Use the output for organic posts, Reel covers, Stories, project highlights, before-after content, or visual content banks.

---

## What it improves

- **Visual consistency** across all organic content
- **Content volume** — more images become usable
- **Speed** — seconds per image instead of hours
- **Brand alignment** — every image carries the Hillarys tone
- **Cost efficiency** — reduces dependency on professional shoots for everyday content

---

## What it does not do

| Boundary | Detail |
|----------|--------|
| **Not a shoot replacement** | Campaign visuals, catalogues, print assets, website hero images, and major brand films still need higher production control. |
| **Not a miracle fix** | It can improve tone and brand fit, but cannot fully rescue poor framing, low resolution, bad composition, or irrelevant content. |
| **Not 10/10 output** | This is a 5-to-7 system. The purpose is practical uplift at scale — not turning every image into a final campaign masterpiece. |

---

## Built for

Hillarys Curtains & Blinds — India market, organic content pipeline.

Part of the **Hillarys AI Content Engine**.

---

*Maintained by the Hillarys marketing team.*
