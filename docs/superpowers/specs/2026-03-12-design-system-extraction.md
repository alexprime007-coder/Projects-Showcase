# Design System Extraction — Spec

**Date:** 2026-03-12
**Status:** Approved
**Goal:** Extract design systems from Pencil files into self-contained JSON files that give any AI code agent enough information to build pixel-accurate applications in the same visual language — even for components and screens not explicitly designed.

---

## 1. Problem

When a client approves a showcase direction, we need to hand off the design system to a code agent building the production app. Today that information lives only inside the `.pen` file. The agent has no structured way to access it.

We need a **single JSON file per project direction** that captures everything: colors, typography, spacing, components, layout rules, and — critically — the design principles so the agent can extrapolate and create new elements that feel native to the system.

## 2. Output Format

### File location and naming

```
Projects Showcase/
├── design-systems/
│   ├── institut-sarah-soft.json
│   ├── institut-sarah-fresh.json
│   ├── institut-sarah-dark.json
│   ├── institut-sarah-brutalist.json
│   └── {project-name}.json
```

- One file per project direction
- Named in kebab-case matching the showcase folder name
- Lives at repo root in `design-systems/`, decoupled from showcase folders
- Added to `.vercelignore` (not needed for showcase deployment)

### JSON Schema

```json
{
  "$schema": "design-system-v1",
  "units": "All numeric values are in px unless suffixed (e.g., '0.01em'). lineHeight without units is a multiplier.",

  "meta": {
    "name": "Institut Sarah — Brutalist Technical",
    "project": "institut-sarah-brutalist",
    "client": "Institut Sarah",
    "industry": "Beauty / Wellness",
    "language": "fr",
    "extractedFrom": "institut-sarah-dark/source/institut-sarah.pen",
    "pencilNodeId": "K2CyC",
    "pencilFrameName": "Direction D — Brutalist Technical",
    "extractedAt": "2026-03-12",
    "extractionMethod": "explicit-frame"
  },

  "principles": {
    "mood": "Brutalist, technical, high-contrast, industrial",
    "philosophy": "Zero decoration. Every element earns its place through function. Typography does the heavy lifting — no gradients, no shadows, no rounded corners. Contrast is the primary visual tool.",
    "doThis": [
      "Use sharp, geometric forms with zero border-radius",
      "Rely on bold typography contrast (Anton display vs Inter body)",
      "Use thick 2px borders as the primary visual separator",
      "Keep layouts structured and grid-aligned",
      "Use uppercase with letter-spacing for all labels and navigation",
      "Reserve the neon green accent for CTAs and key highlights only",
      "Use generous whitespace to let content breathe"
    ],
    "avoidThis": [
      "Rounded corners on any element",
      "Drop shadows or box shadows",
      "Gradients of any kind",
      "More than one accent color at a time",
      "Decorative elements that don't serve a function",
      "Lowercase text in navigation, labels, or badges",
      "Small, timid typography — go bold or go home"
    ]
  },

  "colors": {
    "palette": {
      "primary":    { "value": "#000000", "role": "Primary / text, borders, fills" },
      "background": { "value": "#FFFFFF", "role": "Page background, card fills" },
      "accent":     { "value": "#00FF00", "role": "CTAs, highlights, active states — use sparingly" },
      "surface":    { "value": "#F5F5F5", "role": "Section backgrounds, subtle differentiation" },
      "muted":      { "value": "#999999", "role": "Secondary text, captions, metadata" }
    },
    "usage": {
      "text": {
        "default": "#000000",
        "onPrimary": "#FFFFFF",
        "onAccent": "#000000",
        "muted": "#999999"
      },
      "border": {
        "default": "#000000",
        "width": 2
      },
      "hover": {
        "primaryButton":  { "fill": "#FFFFFF", "text": "#000000", "border": "2px solid #000000" },
        "accentButton":   { "fill": "#000000", "text": "#00FF00", "border": "none" },
        "outlineButton":  { "fill": "#000000", "text": "#FFFFFF", "border": "2px solid #000000" },
        "ghostButton":    { "fill": "transparent", "text": "#00FF00", "border": "none" },
        "links": { "textDecoration": "underline", "color": "#000000" }
      },
      "focus": {
        "outline": "2px solid #000000",
        "outlineOffset": 2
      }
    }
  },

  "typography": {
    "fontFamilies": {
      "display": {
        "family": "Anton",
        "source": "Google Fonts",
        "usage": "Headlines, hero text, section titles, display numbers. Always uppercase with tight tracking."
      },
      "body": {
        "family": "Inter",
        "source": "Google Fonts",
        "usage": "Body copy, labels, navigation, buttons, badges, captions. All functional text."
      }
    },
    "scale": {
      "display":  { "fontFamily": "Anton", "fontSize": 96, "fontWeight": 400, "lineHeight": 1.0, "letterSpacing": "0.01em", "textTransform": "uppercase", "use": "Hero headlines, major impact moments", "responsive": { "tablet": 64, "mobile": 48 } },
      "headline": { "fontFamily": "Anton", "fontSize": 48, "fontWeight": 400, "lineHeight": 1.1, "letterSpacing": "0.01em", "textTransform": "uppercase", "use": "Section titles, page headers", "responsive": { "tablet": 36, "mobile": 28 } },
      "title":    { "fontFamily": "Anton", "fontSize": 32, "fontWeight": 400, "lineHeight": 1.15, "letterSpacing": "0.01em", "textTransform": "uppercase", "use": "Card titles, subsection headers", "responsive": { "tablet": 28, "mobile": 24 } },
      "subtitle": { "fontFamily": "Anton", "fontSize": 24, "fontWeight": 400, "lineHeight": 1.2, "letterSpacing": "0.01em", "textTransform": "uppercase", "use": "Supporting headlines, feature titles" },
      "body":     { "fontFamily": "Inter", "fontSize": 16, "fontWeight": 400, "lineHeight": 1.5, "use": "Paragraphs, descriptions, long-form content" },
      "bodyBold": { "fontFamily": "Inter", "fontSize": 16, "fontWeight": 700, "lineHeight": 1.5, "use": "Emphasized body text, strong callouts" },
      "label":    { "fontFamily": "Inter", "fontSize": 12, "fontWeight": 700, "lineHeight": 1.0, "letterSpacing": "0.15em", "textTransform": "uppercase", "use": "Navigation, buttons, badges, metadata labels" },
      "caption":  { "fontFamily": "Inter", "fontSize": 11, "fontWeight": 400, "lineHeight": 1.4, "use": "Timestamps, footnotes, helper text" }
    }
  },

  "spacing": {
    "base": 8,
    "scale": {
      "xs": 4,
      "sm": 8,
      "md": 16,
      "lg": 24,
      "xl": 32,
      "2xl": 48,
      "3xl": 64,
      "4xl": 96
    },
    "patterns": {
      "sectionPadding": 48,
      "sectionGap": 64,
      "groupGap": 24,
      "elementGap": 16,
      "pageGutter": 24
    },
    "note": "Component-specific spacing (card padding, button paddingX, etc.) lives in the components section to avoid duplication."
  },

  "borders": {
    "default": { "width": 2, "style": "solid", "color": "#000000" },
    "light": { "width": 1.5, "style": "solid", "color": "#000000" },
    "defaultRadius": 0,
    "note": "Zero border-radius is the default. Component-specific overrides (e.g., badge: 4px) live in the components section."
  },

  "effects": {
    "shadows": "none — this design system does not use shadows",
    "gradients": "none — this design system does not use gradients",
    "transitions": {
      "default": "150ms ease",
      "use": "Hover states, focus states, active states"
    }
  },

  "icons": {
    "library": "Lucide",
    "cdn": "https://unpkg.com/lucide@latest/dist/umd/lucide.js",
    "defaultSize": 20,
    "strokeWidth": 2,
    "style": "Outlined, geometric — matches the brutalist aesthetic"
  },

  "images": {
    "treatment": "High contrast, desaturated or b&w preferred. No rounded corners on images. Use object-fit: cover. Prefer editorial/documentary style over stock photography.",
    "optimization": {
      "format": "webp",
      "heroWidth": 1080,
      "cardWidth": 600,
      "heroQuality": 80,
      "cardQuality": 75
    }
  },

  "layout": {
    "maxWidth": 1200,
    "centered": true,
    "grid": {
      "columns": 12,
      "gutter": 24,
      "note": "Use CSS Grid or Flexbox. Prefer 2-column and 3-column layouts. Full-bleed sections for hero and CTA."
    },
    "breakpoints": {
      "desktop": { "minWidth": 1025, "note": "Full multi-column layouts" },
      "tablet":  { "minWidth": 769, "maxWidth": 1024, "note": "2-column layouts, reduce font sizes" },
      "mobile":  { "maxWidth": 768, "note": "Single column, stacked layouts" }
    },
    "patterns": {
      "hero": "Two-column: content left, image right. Full-width background. Image has no border-radius.",
      "services": "3-column grid of cards with consistent height. Each card: number, title, description, price badge.",
      "testimonial": "Two-column: quote content left/right, image opposite side. Large blockquote in Anton.",
      "cta": "Centered text block with heading, subtext, and primary button. Can have accent background.",
      "footer": "4-column: brand + 3 link columns. Bordered top. Copyright bar at bottom.",
      "navbar": "Logo left, links center/right, CTA button right. Fixed height 64px. Bottom border."
    }
  },

  "components": {
    "button": {
      "shared": {
        "height": 52,
        "paddingX": 32,
        "typography": "label",
        "cornerRadius": 0,
        "cursor": "pointer"
      },
      "variants": {
        "primary":  { "fill": "#000000", "text": "#FFFFFF", "border": "none" },
        "accent":   { "fill": "#00FF00", "text": "#000000", "border": "none" },
        "outline":  { "fill": "#FFFFFF", "text": "#000000", "border": "2px solid #000000" },
        "ghost":    { "fill": "transparent", "text": "#000000", "border": "none" }
      }
    },
    "badge": {
      "shared": {
        "height": 28,
        "paddingX": 12,
        "cornerRadius": 4,
        "typography": "label",
        "fontSize": 10
      },
      "variants": {
        "outline": { "fill": "transparent", "text": "#000000", "border": "1.5px solid #000000" },
        "filled":  { "fill": "#000000", "text": "#FFFFFF", "border": "none" },
        "accent":  { "fill": "#00FF00", "text": "#000000", "border": "none" },
        "number":  { "fill": "transparent", "text": "#000000", "border": "1.5px solid #000000" }
      }
    },
    "navbar": {
      "height": 64,
      "fill": "#FFFFFF",
      "border": "2px solid #000000",
      "paddingX": 24,
      "layout": "space-between",
      "logo": "Brand name in Anton, uppercase",
      "links": "Inter label style, uppercase, letter-spaced",
      "cta": "Button/Primary variant"
    },
    "card": {
      "service": {
        "fill": "#FFFFFF",
        "border": "2px solid #000000",
        "cornerRadius": 0,
        "padding": 24,
        "minHeight": 280,
        "layout": "vertical, space-between",
        "structure": [
          "Top: number badge + title (Anton) + description (Inter body)",
          "Bottom: price label + arrow/link"
        ]
      }
    },
    "divider": {
      "height": 2,
      "color": "#000000",
      "fullWidth": true,
      "note": "Used between major sections. The thick line is a signature element."
    },
    "input": {
      "note": "Not explicitly designed yet. When building: use 52px height (same as buttons), 2px black border, no border-radius, Inter label placeholder, 16px padding-x. Follow the button's visual language.",
      "height": 52,
      "border": "2px solid #000000",
      "cornerRadius": 0,
      "paddingX": 16,
      "typography": "body",
      "placeholder": "muted color, label style"
    }
  },

  "extrapolation": {
    "note": "When building components not explicitly defined above, follow these rules to stay consistent with the design system:",
    "rules": [
      "Borders over shadows — always use 2px solid black borders to define containers",
      "Zero border-radius everywhere except badges (4px)",
      "Typography hierarchy: Anton for anything that needs to grab attention, Inter for everything else",
      "All navigation, labels, and metadata text is uppercase with letter-spacing",
      "Heights align to the system: 28px (badges), 52px (buttons/inputs), 64px (navbar)",
      "Padding follows the spacing scale: 12px (tight), 24px (standard), 32px (comfortable), 48px (spacious)",
      "Color usage: black/white for structure, accent green ONLY for the most important interactive element on the screen",
      "When in doubt, remove — this system favors absence over decoration",
      "Hover states: invert colors (black becomes white bg with black border, or vice versa)",
      "New components should feel like they were designed at the same time as the existing ones"
    ]
  }
}
```

## 3. Extraction Process

The extraction is performed by Claude (LLM) reading the Pencil file via MCP tools. The `principles`, `extrapolation`, and `mood` fields are LLM-generated based on analyzing the visual patterns — this is intentional, not a gap.

### Source: Explicit design system frame (preferred)

When the Pencil file has a frame named `Design System — *`:

1. Read the frame via `batch_get(filePath, nodeIds: [frameId], readDepth: 3)`
2. Follow up with deeper reads on any truncated `"..."` children
3. Extract color swatches from the color section (fill values + label text)
4. Extract typography from text samples (fontFamily, fontSize, fontWeight, letterSpacing, lineHeight)
5. Extract components from reusable nodes (fill, stroke, padding, height, cornerRadius)
6. Extract spacing from layout gaps and padding values
7. Read variables via `get_variables(filePath)` — if themes exist, document the default theme and note alternatives
8. Infer principles from the visual patterns (border-radius usage, shadow usage, color count)
9. Set `meta.extractionMethod` to `"explicit-frame"`

### Source: Inference from design (no explicit frame)

When only the showcase direction frame exists, Claude analyzes the design:

1. Read the direction frame via `batch_get(filePath, nodeIds: [frameId], readDepth: 4)`
2. **Colors**: collect all unique `fill` and `stroke` values, assign roles using context:
   - Frame fills covering large areas → background/surface
   - Text node fills → primary (high frequency) and muted (low contrast)
   - Small interactive frame fills → accent (if distinct from primary)
   - Stroke colors → border
3. **Typography**: collect all text node specs, deduplicate by fontFamily+fontSize+fontWeight, rank by size to build the scale
4. **Components**: identify by structural patterns:
   - Buttons: frames with `justifyContent: center`, single text child, height 40-56px
   - Cards: frames with padding, multiple children including text nodes, height > 150px
   - Badges: frames with `justifyContent: center`, single text child, height < 36px
   - Navbar: top-level horizontal frame spanning full width
5. **Spacing**: collect `gap` and `padding` values from layout frames, identify the base unit
6. **Style**: detect cornerRadius patterns, border usage, shadow/gradient presence
7. **Principles**: Claude generates mood, philosophy, doThis/avoidThis based on all detected patterns
8. Set `meta.extractionMethod` to `"inferred"`

### Quality check

After extraction, validate. If a check fails, **proceed but add a `"warnings"` array** to the JSON noting what's missing:

- [ ] All colors have roles assigned
- [ ] Typography scale has at least: display/headline, body, label sizes
- [ ] At least button component documented
- [ ] Spacing values are present
- [ ] Principles section has both "do" and "avoid" lists
- [ ] Extrapolation rules are present

### Re-extraction

When re-running extraction on an already-extracted project:
- Overwrite the JSON entirely — the Pencil file is the source of truth
- If manual edits to the JSON are needed (e.g., adding extrapolation rules), make them in Pencil instead so they survive re-extraction

## 4. Integration with Showcase Skill

Add a new step between "Create HTML showcase" and "Commit and push":

```
... → Create HTML showcase → Extract design system → Commit and push → ...
```

The extraction step:
1. Read the approved Pencil direction (already done for HTML creation)
2. Check for explicit design system frame in the `.pen` file
3. Extract tokens using the appropriate method
4. Write to `design-systems/{project-name}.json`
5. Include in the git commit

For existing showcases, run a one-time bootstrap extraction.

**Multiple directions in one `.pen` file:** A single Pencil file may contain multiple direction frames (e.g., Direction A, B, C, D). The `meta.pencilNodeId` and `meta.pencilFrameName` fields identify which frame was extracted. The extraction reads only that specific frame.

## 5. .vercelignore Update

Add `design-systems` to `.vercelignore` — these files are for agent consumption, not deployment.

## 6. Consuming Agent Instructions

When handing off to a code agent, the instruction is:

> "Build [app type] for [client]. Design system: `design-systems/{project-name}.json`. Follow it exactly for all colors, typography, spacing, and component styles. For any component not explicitly defined, follow the extrapolation rules in the JSON."

The agent reads one file and has everything it needs.
