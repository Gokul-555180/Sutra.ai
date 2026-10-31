# SUTRA.ai — Design System Specification

> **Document Version:** 1.0.0  
> **Status:** Proposed Design System Specification  
> **Aesthetic Philosophy:** Restrained Industrial Desktop (Inspired by Linear, Raycast, Cursor, Arc)  
> **Related Documents:** [UI_UX_GUIDELINES.md](UI_UX_GUIDELINES.md), [BRAND_GUIDELINES.md](BRAND_GUIDELINES.md), [AGENTS.md](../../AGENTS.md)

---

## 1. Visual Direction & Aesthetic Standard

SUTRA.ai rejects the visual clichés of generic "AI SaaS" templates (rainbow gradient text, animated starfields, floating blurry cards). Instead, it adopts a **dark-first, high-density, restrained desktop software aesthetic** crafted for deep focus, extended daily usage, and fast keyboard-driven navigation.

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                          SUTRA DESIGN PRINCIPLES                            │
├──────────────────────┬──────────────────────┬───────────────────────────────┤
│ 1. Deep Substrates   │ 2. Subtle Borders    │ 3. Restrained Violet Accents  │
│    Slate & Charcoal  │    1px low-contrast  │    Reserved strictly for      │
│    surfaces (#0B0D11)│    dividers (#222733)│    active states and focus    │
├──────────────────────┼──────────────────────┼───────────────────────────────┤
│ 4. Crisp Typography  │ 5. Micro-Animations  │ 6. Predictable Hierarchy      │
│    Sans for chrome,  │    150–200ms ease-out│    Consistent spacing and     │
│    Mono for prompts  │    state feedback    │    content density            │
└──────────────────────┴──────────────────────┴───────────────────────────────┘
```

---

## 2. Color Palette & Token Architecture

All colors are exposed as semantic CSS variables and mapped through Tailwind CSS tokens:

### Dark Theme Tokens (Default)

| Token Name | Hex Value | Semantic Usage |
| :--- | :--- | :--- |
| `background` | `#0B0D11` | Application base canvas substrate |
| `surface-1` | `#12151B` | Sidebar navigation, toolbars, top header chrome |
| `surface-2` | `#1A1E26` | Card panels, dialog surfaces, dropdown menus |
| `surface-3` | `#232834` | Hovered states, active item backgrounds |
| `border-subtle` | `#222733` | 1px structural dividing lines (low contrast) |
| `border-strong` | `#32394A` | Focused input borders, highlighted boundaries |
| `text-primary` | `#F1F3F9` | Primary headings, active prompt text |
| `text-secondary`| `#9BA3B5` | Labels, secondary descriptions, metadata |
| `text-muted` | `#636C80` | Placeholders, disabled text, hotkey hints |
| `accent-primary`| `#7C3AED` | Primary active tabs, action buttons, active toggles |
| `accent-hover` | `#6D28D9` | Hovered button state |
| `diff-add` | `#10B981` | Added text in prompt diff engine (green) |
| `diff-del` | `#EF4444` | Removed text in prompt diff engine (red) |
| `warning` | `#F59E0B` | Contradiction alerts, missing constraint warnings |

### Light Theme Tokens (Supported)

| Token Name | Hex Value | Semantic Usage |
| :--- | :--- | :--- |
| `background` | `#F8FAFC` | Crisp, low-glare canvas |
| `surface-1` | `#FFFFFF` | Primary white panels and chrome |
| `surface-2` | `#F1F5F9` | Muted card backgrounds |
| `border-subtle` | `#E2E8F0` | Subtle structural dividers |
| `text-primary` | `#0F172A` | Deep slate high-contrast body text |
| `accent-primary`| `#6D28D9` | Deep violet brand accent |

---

## 3. Typography Scale & Fonts

* **UI Chrome & Navigation:** `Inter`, `system-ui`, `-apple-system`, `sans-serif`
* **Prompt Code, Delimiters & Variables:** `JetBrains Mono`, `Fira Code`, `monospace`

| Style Token | Size | Line Height | Weight | Usage |
| :--- | :--- | :--- | :--- | :--- |
| `text-xs` | 11px (0.6875rem) | 16px | 400/500 | Badges, hotkey shortcuts, token counts |
| `text-sm` | 13px (0.8125rem) | 18px | 400/500 | Default UI labels, buttons, table rows |
| `text-base` | 14px (0.875rem) | 22px | 400/500 | Editor prompt text, descriptive paragraphs |
| `text-md` | 16px (1.0rem) | 24px | 600 | Card titles, section headers |
| `text-lg` | 18px (1.125rem) | 26px | 600 | View headers, modal titles |
| `text-xl` | 22px (1.375rem) | 28px | 700 | Workspace dashboard title |

---

## 4. Spacing, Borders & Shadows

* **Spacing Grid:** 4px baseline (`p-1` = 4px, `p-2` = 8px, `p-3` = 12px, `p-4` = 16px, `p-6` = 24px).
* **Border Radii:**
  * `rounded-sm`: 4px (Buttons, badges, keyboard shortcut chips).
  * `rounded-md`: 6px (Input fields, dropdown items, dialog boxes).
  * `rounded-lg`: 8px (Cards, workspace panels, editor wrappers).
* **Shadows:** Avoid multi-layered drop shadows. Rely instead on 1px borders with subtle ambient elevation:
  * `shadow-sm`: `0 1px 2px 0 rgba(0, 0, 0, 0.4)` (Cards and panels).
  * `shadow-lg`: `0 8px 16px -2px rgba(0, 0, 0, 0.6)` (Floating modals and dropdowns).

---

## 5. Component Primitives (shadcn/ui Based)

### Buttons
* **Primary:** Background `accent-primary` (`#7C3AED`), text white, hover `accent-hover` (`#6D28D9`).
* **Secondary:** Background `surface-2`, border `border-subtle`, text `text-primary`.
* **Ghost:** Zero border, hover background `surface-3`, text `text-secondary`.
* **Destructive:** Background `#DC2626`, hover `#B91C1C`, text white.

### Inputs & Editors
* Background `surface-1`, 1px border `border-subtle`, focus ring 2px `accent-primary` with offset.
* Monospace font applied strictly to prompt body inputs.

### State Presentations
* **Idle:** Clean onboarding cards with actionable shortcut hints.
* **Loading / Streaming:** Non-jarring pulse skeletons; zero content-shifting spinners.
* **Empty:** Reusable empty-state illustration and "Create First Prompt" CTA.
* **Error:** Human-readable diagnostic message with explicit retry and recovery buttons.
