# Uzhavan AI

> **Uzhavan** (உழவன்) means *Farmer* in Tamil.

A smart farming copilot — an AI-first web prototype that guides beginner farmers in India through crop selection, cost planning, risk assessment, and profit estimation in one conversation.

---

## Demo

Open `index.html` directly in any modern browser — no build step, no server, no dependencies beyond a Google Fonts CDN call.

---

## Features

| Feature | Description |
|---|---|
| Crop Recommendations | Top 3 crops ranked by soil-fit, risk level, and estimated return |
| Chat Interface | Conversational Q&A with preset and free-text questions |
| Daily Insights | Condition-aware farming tip shown on the left panel |
| Farm Profile | Quick summary of land size, budget, location, and method |
| Responsive Layout | 3-column desktop → 2-column tablet → single-column mobile with slide-in drawer |

---

## Project Structure

```
Uzhavan-Prototype/
└── index.html    # Complete self-contained app (HTML + CSS + JS)
```

The entire prototype lives in one file:

- **HTML** — semantic layout with three panels (left sidebar, main chat shell, right summary)
- **CSS** — custom properties, CSS Grid, Flexbox, and media-query breakpoints at 1180 px and 900 px
- **JavaScript** — functional-style modules wired at the bottom of the file (see Architecture below)

---

## Architecture — Functional JavaScript

The script section is organized into three explicit layers:

### 1. Data (immutable constants)
```js
const REPLIES = Object.freeze({ ... });   // keyword → canned response map
const FALLBACK_REPLY = (message) => ...;  // pure fallback generator
```

### 2. Pure functions (no side effects)
```js
normalizeKey(text)          // string → lowercase lookup key
getReply(message)           // string → string (reply lookup)
createBotRow(text)          // string → HTMLElement
createUserRow(text)         // string → HTMLElement
createMessageRow(role,text) // dispatches to the above factories
```

Pure functions are referentially transparent — given the same input they always return the same output and touch no global state.

### 3. Effects (side-effect sinks, explicit and minimal)
```js
appendMessage(container)(role, text)    // appends row + scrolls
scheduleReply(append, delay)(userText)  // deferred bot reply
consumeInput(inputEl)                   // reads + clears input field
```

Effects are pushed to the edge and composed from pure functions, making the logic easy to follow and test.

---

## Sample Crop Data (prototype)

| Crop | Soil Fit | Risk | Est. Return |
|---|---|---|---|
| Groundnut | 92% | Low | ₹58,000 |
| Millets | 88% | Low | ₹46,000 |
| Black Gram | 81% | Medium | ₹41,000 |

*Demo profile: 2 acres · Ariyalur district · ₹40,000 budget · Organic-first*

---

## Preset Questions

The chat supports these preset questions out of the box:

- Best crop for red soil
- Compare organic vs chemical farming
- Estimate cost for 2 acres
- Low water farming options
- Which crop gives highest profit?
- Show low risk farming plan
- Organic farming benefits
- Water-saving crop ideas

Any other free-text message returns a fallback prototype reply.

---

## Tech Stack

| Layer | Technology |
|---|---|
| Markup | HTML5 |
| Styling | CSS3 (custom properties, Grid, Flexbox, `backdrop-filter`) |
| Scripting | Vanilla JavaScript (ES2020+, functional style) |
| Font | [Inter](https://fonts.google.com/specimen/Inter) via Google Fonts |
| Backend | None (static prototype) |

---

## Roadmap

- [ ] Connect to a real AI backend (e.g., Claude API) for dynamic recommendations
- [ ] Soil-type and location selector with live data
- [ ] Multi-language support (Tamil, Hindi, Telugu)
- [ ] Offline PWA mode for low-connectivity rural areas
- [ ] Voice input / output for low-literacy users

---

## License

MIT
