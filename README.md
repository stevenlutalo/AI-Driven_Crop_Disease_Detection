# 🌾 CropGuard AI — README

> **AI-Driven Crop Disease Detection & Management System**
> Based on: *Ansari, Singh & Akhtar — IJSRST Vol 12, Issue 3, 2025*

---

## Table of Contents

1. [What This System Does](#what-this-system-does)
2. [Prerequisites](#prerequisites)
3. [Project Setup (Step by Step)](#project-setup-step-by-step)
4. [Folder Structure](#folder-structure)
5. [Running the App](#running-the-app)
6. [Using the App](#using-the-app)
7. [Troubleshooting](#troubleshooting)
8. [Bug Fixes Reference](#bug-fixes-reference)
9. [Technology Stack](#technology-stack)

---

## What This System Does

CropGuard AI is a web-based smart agriculture platform with 5 pages:

| Page | Description |
|---|---|
| **Dashboard** | Overview stats + CNN performance table from the research paper |
| **Detect Disease** | Upload a crop/leaf image → AI diagnoses the disease and suggests treatments |
| **Weather** | 5-day forecast with crop disease risk assessment |
| **Marketplace** | Browse and filter crops listed by local farmers |
| **Price Predict** | AI-powered 30-day price forecast for selected crops |

---

## Prerequisites

Before running anything, make sure you have these installed on your computer:

### 1. Node.js (version 18 or higher)

Check if you already have it:
```bash
node --version
```
If you don't, download it from: **https://nodejs.org** (choose the "LTS" version)

### 2. npm (comes with Node.js automatically)

Check:
```bash
npm --version
```

### 3. An Anthropic API Key

The Disease Detection and Price Prediction pages make real AI calls. You need a key from Anthropic.

- Sign up at: **https://console.anthropic.com**
- Go to **API Keys** → **Create Key**
- Copy the key — it starts with `sk-ant-...`

> ⚠️ **Keep your API key private. Never share it or commit it to GitHub.**

---

## Project Setup (Step by Step)

### Step 1 — Create a new React project

Open your terminal (Command Prompt on Windows, Terminal on Mac/Linux) and run:

```bash
npm create vite@latest cropguard-ai -- --template react
```

When prompted, choose:
- **Framework:** React
- **Variant:** JavaScript

### Step 2 — Enter the project folder

```bash
cd cropguard-ai
```

### Step 3 — Install dependencies

```bash
npm install
```

### Step 4 — Place the app file

Copy `CropGuardAI.jsx` into your project's `src` folder, replacing the default `App.jsx`:

```
cropguard-ai/
  src/
    App.jsx        ← replace this with CropGuardAI.jsx content
                     OR rename CropGuardAI.jsx to App.jsx
```

**Option A — Rename the file:**
Simply rename `CropGuardAI.jsx` to `App.jsx` and drop it into the `src/` folder.

**Option B — Update the import:**
If you keep the filename as `CropGuardAI.jsx`, open `src/main.jsx` and change:
```jsx
// Before:
import App from './App'

// After:
import App from './CropGuardAI'
```

### Step 5 — Set up your API key

Create a file called `.env` in the **root** of your project (same level as `package.json`):

```
VITE_ANTHROPIC_API_KEY=sk-ant-your-key-here
```

> Replace `sk-ant-your-key-here` with your actual key from Step 3 of Prerequisites.

Then, in `CropGuardAI.jsx`, find the `callClaudeAPI` function and update the fetch headers:

```jsx
// Find this line in callClaudeAPI():
headers: { "Content-Type": "application/json" },

// Change it to:
headers: {
  "Content-Type": "application/json",
  "x-api-key": import.meta.env.VITE_ANTHROPIC_API_KEY,
  "anthropic-version": "2023-06-01",
  "anthropic-dangerous-direct-browser-access": "true",
},
```

Do the same for the `handlePredict` function in the `PricePage` component — it also has a `fetch` call with the same `headers` object.

### Step 6 — Update index.html (optional but recommended)

Open `index.html` in the root folder and update the title:

```html
<title>CropGuard AI</title>
```

### Step 7 — Clean up default files (optional)

You can safely delete the following default Vite files that are no longer needed:

```
src/App.css
src/assets/react.svg
public/vite.svg
```

Also clear out `src/index.css` — the app handles all its own styles internally.

---

## Folder Structure

After setup, your project should look like this:

```
cropguard-ai/
├── public/
├── src/
│   ├── App.jsx          ← your CropGuardAI.jsx file goes here
│   ├── main.jsx         ← entry point (do not delete)
│   └── index.css        ← can be emptied
├── .env                 ← your API key (never commit this!)
├── .gitignore           ← make sure .env is listed here
├── index.html
├── package.json
└── vite.config.js
```

> ✅ Make sure `.env` appears in your `.gitignore` file. Open `.gitignore` and confirm this line exists:
> ```
> .env
> ```

---

## Running the App

Once setup is complete, start the development server:

```bash
npm run dev
```

You will see output like:

```
  VITE v5.x.x  ready in 300ms

  ➜  Local:   http://localhost:5173/
  ➜  Network: use --host to expose
```

Open your browser and go to: **http://localhost:5173**

The app will reload automatically every time you save changes to the code.

### To stop the server

Press `Ctrl + C` in the terminal.

---

## Using the App

### Disease Detection (most important feature)

1. Click **Detect Disease** in the left sidebar
2. Click the green upload area or drag and drop a leaf/crop image
3. Supported formats: **JPG, PNG, WebP** (max ~10MB)
4. Click the **Analyse Disease** button
5. Wait 2–5 seconds for the AI result
6. You will see:
   - Crop type detected
   - Disease name (or "Healthy")
   - Confidence percentage
   - Severity level (Mild / Moderate / Severe / None)
   - Clinical description
   - Recommended treatments
   - Prevention tips

### Price Prediction

1. Click **Price Predict** in the sidebar
2. Select a crop from the dropdown (Tomato, Potato, Rice, etc.)
3. Click **Generate Prediction**
4. The AI will return a 30-day price forecast with key market factors

### Weather

- Shows a simulated 5-day weather forecast
- Displays disease risk levels based on humidity and temperature
- In production, connect a real OpenWeatherMap API key (see Section 5.3 of the paper)

### Marketplace

- Browse crop listings
- Use the filter pills at the top to filter by crop type
- Click **Contact Farmer** on any listing

---

## Troubleshooting

| Problem | Likely Cause | Fix |
|---|---|---|
| Blank white screen | JavaScript error | Open browser DevTools (F12) → Console tab, read the error |
| "API error 401" | Invalid or missing API key | Double-check your `.env` file and headers update from Step 5 |
| "API error 403" | CORS or missing header | Ensure `anthropic-dangerous-direct-browser-access: true` is in your headers |
| "Failed to read file" | Corrupt or unsupported image | Try a different JPG or PNG file |
| "Detection failed: SyntaxError" | API returned malformed JSON | This is rare; click Analyse again |
| Images not showing | Wrong file path | Make sure image files are inside `public/` or imported correctly |
| App not updating | Vite cache issue | Stop the server, run `npm run dev` again |
| Port already in use | Another app on port 5173 | Run `npm run dev -- --port 3000` to use a different port |

---

## Bug Fixes Reference

These bugs were identified and fixed during development. All are documented with comments in the source code.

| # | Bug | Location | Fix Applied |
|---|---|---|---|
| 1 | `FileReader` async race condition | `fileToBase64()` | Wrapped in a `Promise` so the base64 result is fully ready before the API call |
| 2 | Base64 `data:` URI prefix leaked to API | `fileToBase64()` | Stripped with `.split(",")[1]` after `readAsDataURL` |
| 3 | LLM wraps JSON in markdown fences (`\`\`\`json`) | `stripJsonFences()` | Regex strips fences before `JSON.parse()` |
| 4 | `setInterval` memory leak in clock | `WeatherPage` `useEffect` | Cleanup function `() => clearInterval(id)` returned from `useEffect` |

---

## Technology Stack

| Layer | Technology | Purpose |
|---|---|---|
| Frontend Framework | React 18 (via Vite) | Component-based UI |
| Styling | CSS-in-JS (style tag) | Scoped, variable-driven design |
| AI / Vision | Anthropic claude-sonnet-4 | Disease detection + price prediction |
| Dataset Reference | PlantVillage (38 disease classes) | Model training basis (paper) |
| Model Architecture | EfficientNet-B3, MobileNetV2 | CNN backbone (paper Section 5.1) |
| Build Tool | Vite | Fast dev server and bundling |
| Fonts | DM Serif Display + DM Sans | Typography (loaded via Google Fonts) |

---


---

*Built with React + Anthropic AI · CropGuard AI v1.0*
