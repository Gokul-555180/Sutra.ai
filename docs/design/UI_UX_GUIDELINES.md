# SUTRA.ai — UI & UX Interaction Guidelines

> **Document Version:** 1.0.0  
> **Status:** Active Design Guidelines  
> **Target Audience:** Frontend Engineers, Product Designers, AI Coding Agents  
> **Related Documents:** [DESIGN_SYSTEM.md](DESIGN_SYSTEM.md), [BRAND_GUIDELINES.md](BRAND_GUIDELINES.md), [AGENTS.md](../../AGENTS.md)

---

## 1. Ergonomic Philosophy & Principles

SUTRA.ai is an industrial workstation. The user interface exists to serve the user's cognitive flow, not to display visual ornamentation.

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                          SUTRA ERGONOMIC PILLARS                            │
├─────────────────────┬─────────────────────┬─────────────────────────────────┤
│ 1. Zero Jitter      │ 2. Keyboard-First   │ 3. Non-Blocking Streaming       │
│    Zero layout-shift│    Every feature has│    Token generation never locks │
│    during streaming │    a global shortcut│    the UI or stalls typing      │
├─────────────────────┴─────────────────────┴─────────────────────────────────┤
│ 4. Clear Hierarchy: Information density prioritized over empty white space  │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## 2. Navigation & Window Shell Architecture

* **Collapsible Navigation Rail:** A 64px icon rail that expands to a 220px labeled sidebar on hover or via shortcut (`Ctrl+B` / `Cmd+B`).
* **Custom Frameless Titlebar:** Windows and macOS native window buttons integrated smoothly into the top chrome, including active workspace title, search trigger (`Ctrl+K`), and provider status pill.
* **Status Bar:** Bottom 28px strip showing:
  * Current SQLite database status.
  * Active AI model name and latency.
  * Token count of the active prompt buffer.

---

## 3. Prompt Editing Ergonomics

* **Dual-Pane Workspaces:** The editor prioritizes a side-by-side visual workflow:
  * Left Pane: Raw unstructured input or facet editor.
  * Right Pane: Real-time compiled prompt preview or diff visualization.
* **Variable Highlighting:** Dynamic Mustache variables (`{{variable}}`) are styled as interactive pill tags that can be clicked to configure default values.
* **Delimiter Distinction:** Delimiters (`###`, `"""`, `<instructions>`, `</instructions>`) are distinctly tinted to help the user visually verify delimiter nesting.

---

## 4. Keyboard Navigation & Global Shortcuts

SUTRA.ai provides a comprehensive keyboard control map:

| Shortcut (Win/Linux) | Shortcut (macOS) | Action |
| :--- | :--- | :--- |
| `Ctrl + K` | `Cmd + K` | Open global command palette & instant search |
| `Ctrl + N` | `Cmd + N` | Create new prompt in Architect |
| `Ctrl + S` | `Cmd + S` | Save current prompt as version snapshot |
| `Ctrl + Enter` | `Cmd + Enter` | Execute prompt in Playground / Trigger Optimization |
| `Ctrl + B` | `Cmd + B` | Toggle navigation sidebar collapse |
| `Ctrl + 1..9` | `Cmd + 1..9` | Switch between workspace modules |
| `Esc` | `Esc` | Close active dialog / cancel stream |

---

## 5. Declarative State Presentation Rules

Every view must handle the four fundamental lifecycle states without exception:

1. **Idle / Uninitialized:** Clean workspace with keyboard hints and recent templates.
2. **Loading / Streaming:**
   * Text streaming renders at 60 FPS (16ms throttle).
   * Skeleton loaders match the exact geometry of expected content to eliminate Content Layout Shift (CLS).
3. **Empty:** Highlighting actionable next steps (e.g., "No prompts found in collection. Press `Ctrl+N` to create one").
4. **Error:** Human-readable diagnosis, exact error code, and one-click recovery action.
