# Case Study: Scalable UI Architecture & Design Token Migration 🎵

**Project:** Proprietary Music Analytics Dashboard (NDA)
**Role:** Technical Lead / Frontend Developer

## 📌 Overview

Building upon my experience in architecting high-end interfaces for the music industry, this case study documents the modernization of a complex analytics dashboard. The objective was to migrate a high-fidelity Figma design system into a scalable codebase, ensuring a premium "Dark Glassmorphism" aesthetic while future-proofing the technology stack.

## 🛠️ The Technical Challenges

**1. Design Token Volatility (Tailwind v4 Beta):**
The project required migrating to the newly released **Tailwind CSS v4**. The architectural challenge was that v4 drastically changed how custom themes and CSS variables are processed. Injecting complex HSL color variables with opacity channels directly into standard theme blocks caused critical compiler crashes.

**2. Component Coupling & Monolithic Layouts:**
The legacy routing structure tightly coupled the visual layouts (like Sidebars and Headers) with the main application shell, making it difficult to scale the UI without affecting global state.

## 💡 The Solution (Architectural Approach)

### 1. 3-Layer Design Token Engine (CSS Architecture)
To resolve the Tailwind v4 compilation errors and achieve a pixel-perfect Dark Mode, I engineered a strict 3-layer CSS foundation. I separated the raw HSL mathematical values (`@layer base`) from the utility class mapping (`@theme inline`). 

**Technical Snippet:**
```css
/* Decoupled variables prevent v4 compiler crashes */
@layer base {
  :root {
    --bg-hsl: 240 38% 6%; /* Proprietary Dark Tone */
    --primary-hsl: 265 98% 56%; /* Accent Neon */
  }
}

/* Modern Theme Mapping (Figma tokens to utility classes) */
@theme inline {
  --color-background: hsl(var(--bg-hsl));
  --color-primary: hsl(var(--primary-hsl));
  --color-sidebar: hsl(var(--bg-hsl));
}
\```

### 2. Standalone UI Orchestration (Angular)
I refactored the core views away from traditional module-based routing into **Angular 19 Standalone Components** (`standalone: true`). 
* Created a decoupled `SidebarComponent` and reusable `MetricCardComponent`.
* Implemented modern Angular Signals (`signal()`) to prep the UI for zero-latency reactive data streams.

## 📸 Visual Impact: Figma-to-Code Execution

Here is a side-by-side comparison demonstrating the architectural shift from a generic layout to a fully customized, brand-aligned analytics dashboard.

| **Before (Generic Shell)** | **After (Custom Design System Applied)** |
| :---: | :---: |
| <img src="assets/before%201.png" width="400" alt="Initial UI state"> | <img src="assets/after%201.png" width="400" alt="New Premium UI state"> |

*(Confidentiality Note: Logos, specific artist data, and proprietary branding have been sanitized or redacted to comply with NDA policies. This showcase focuses purely on UI implementation).*

## 🚀 Key Technologies

* **Angular 19** (Standalone Components, Signals)
* **Tailwind CSS v4** (Advanced `@theme inline` API, HSL variables)
* **TypeScript** (Strict Mode)
* **Figma** (Design Token Extraction)
