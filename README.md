# BODHAI — Concert Ticket & Pass Generator 🎟️⚡

> *"Create. Customize. Pass. Experience."*

**BODHAI** is a single-page, zero-backend, client-side event ticketing and checkpoint pass management system. Designed for live concerts, music festivals, and high-security backstage credentials, BODHAI gives event managers a full suite of creation, customization, QR generation, validation, and analytics tools directly in the browser.

---

## 📑 Table of Contents

1. [Features & Capabilities](#-features--capabilities)
2. [Brand & Design Identity](#-brand--design-identity)
3. [System Architecture & Data Flow](#-system-architecture--data-flow)
4. [Technology Stack](#-technology-stack)
5. [Getting Started](#-getting-started)
6. [Data Schema & Storage Engine](#-data-schema--storage-engine)
7. [Ticket & Pass Templates](#-ticket--pass-templates)
8. [Gate Verification & Anti-Fraud Flow](#-gate-verification--anti-fraud-flow)
9. [Export & Print System](#-export--print-system)
10. [Roadmap & License](#-roadmap--license)

---

## ⚡ Features & Capabilities

- **Executive Real-Time Dashboard**
  - Live metric counters: Total Events, Generated Tickets, Check-in Turnout, Verified Passes, and Total Sales Value.
  - Interactive SVG Donut and Bar charts tracking pass tier distributions.
- **Concert & Event Studio**
  - Full scheduling (gate opening, closing, multi-day schedules).
  - Dynamic image ingestion with Base64 encoding saved directly into local storage.
  - Multi-tier seat/pass catalog builder (General, Silver, Gold, VIP, VVIP, Backstage, Custom).
- **Split-Screen Ticket Generator**
  - **Left pane**: Configuration form with autofill presets.
  - **Right pane**: Real-time ticket rendering with instant updates as fields change.
  - Realistic horizontal concert ticket layout with an encrypted watermark and a **detachable perforated gate stub**.
- **Lanyard Pass & Badge Designer**
  - Vertical ID credentials for Artists, VIP Guests, Media, and Backstage Crew with live custom accents and lanyard punch-hole simulation.
- **Checkpoint Turnstile Verification**
  - Laser scan graphic simulation.
  - Verifies pass authenticity by scanning payload or entering monospace ID codes.
  - Instant validation states: `✓ VALID PASS`, `⚠ ALREADY USED`, or `✕ INVALID CODE`.
  - One-click check-in button updates attendee attendance and analytics in real time.
- **Data Portability & Zero-Server Persistence**
  - Browser `LocalStorage` database with auto-seeding demo datasets.
  - Single-click Full JSON Database Export and Import capabilities.

---

## 🎨 Brand & Design Identity

- **Logo Concept:** An abstract fusion of a **perforated concert ticket**, **letter "B"**, **digital QR nodes**, and a **sound audio waveform**.
- **Aesthetic:** Dark cyberpunk/concert mode (`#090d16`), electric cyan (`#06b6d4`), and neon violet (`#a855f7`).
- **Typography:**
  - Headlines & Accents: `Outfit`
  - Body & UI: `Inter`
  - Ticket IDs, Gates & Perforations: `JetBrains Mono`

---

## 📐 System Architecture & Data Flow

### Architecture Model

```
                    ┌─────────────────────────┐
                    │      ORGANIZER / ADMIN  │
                    └────────────┬────────────┘
                                 │
                                 ▼
                    ┌─────────────────────────┐
                    │     BODHAI SYSTEM UI    │
                    └────────────┬────────────┘
                                 │
          ┌──────────────────────┼──────────────────────┐
          ▼                      ▼                      ▼
   EVENT MANAGER           TICKET ENGINE          PASS DESIGNER
   (Venues/Roster)        (ID + QR Engine)       (Vertical Badges)
          │                      │                      │
          └──────────────────────┼──────────────────────┘
                                 ▼
                    ┌─────────────────────────┐
                    │     GENERATED PASS      │
                    │   (Encrypted Payload)   │
                    └────────────┬────────────┘
                                 │
                     ┌───────────┴───────────┐
                     ▼                       ▼
              DOWNLOAD (PNG/PDF)        GATE PRINT
                     │
                     ▼
          ┌─────────────────────┐
          │   QR VERIFICATION   │
          │ (Checkpoint Gate)   │
          └──────────┬──────────┘
                     │
          ┌──────────┴──────────┐
          ▼                     ▼
    [VALID TICKET]       [INVALID / USED]
          │
          ▼
      MARK USED ──────► [ANALYTICS ENGINE]
```

### Data Flow Process (DFD Level 1)

1. **Process 1.0 (Event Management):** Accepts event details, schedules, and custom pricing tiers; writes records to `bodhai_events`.
2. **Process 2.0 (Ticket Generation):** Generates cryptographically structured unique IDs (`BDH-YYYY-TIER-XXXXXX`) and packages them into high-contrast QR codes.
3. **Process 3.0 (Pass Customization):** Applies user-selected themes (Obsidian, Neon, Gold, Festival, Minimal) to the DOM canvas.
4. **Process 4.0 (Gate Verification):** Scans or checks ticket records, rejects duplicate scans, and marks the pass as `USED`.
5. **Process 5.0 (Analytics Pipeline):** Recalculates real-time turnout percentages and financial velocity upon gate admission.

---

## 💻 Technology Stack

| Layer | Library / Engine | Source |
|---|---|---|
| **Markup & Semantics** | HTML5 | Native |
| **Styling & Theme** | Tailwind CSS + Custom CSS | CDN |
| **Icons** | Lucide Icons | CDN |
| **QR Code Engine** | QRCode.js | CDN |
| **Canvas Export** | html2canvas v1.4.1 | CDN |
| **Vector PDF Engine** | jsPDF v2.5.1 | CDN |
| **Storage Engine** | Browser LocalStorage | Native API |

> **No Node.js, npm, or build tools are required.**

---

## 🚀 Getting Started

1. Clone or download this repository.
2. Locate the `index.html` file in the project folder.
3. Open `index.html` directly in any modern browser (Chrome, Edge, Firefox, Safari):

```bash
# Optional: run a quick local HTTP server if desired
npx serve .
# or simply double-click index.html
```

4. The application will initialize with default sample concerts and issued passes.

---

## 🗄️ Data Schema & Storage Engine

The application automatically persists state to the browser using the following keys:

### 1. `bodhai_events`
```json
{
  "id": "EV-2026-BDH01",
  "name": "BODHAI MUSIC NIGHT",
  "subtitle": "The Ultimate Live Audio Experience",
  "artist": "The Midnight Waves",
  "artistImage": "data:image/png;base64,...",
  "bannerImage": "data:image/png;base64,...",
  "venue": "Bodh Ghat Arena",
  "city": "Varanasi",
  "date": "2026-12-25",
  "startTime": "19:00",
  "endTime": "23:30",
  "categories": [
    { "name": "GENERAL", "price": 499, "seats": 1000 },
    { "name": "VIP", "price": 2499, "seats": 200 }
  ]
}
```

### 2. `bodhai_tickets`
```json
{
  "id": "BDH-2026-VIP-8F92A1",
  "eventId": "EV-2026-BDH01",
  "holderName": "Raj Gautam",
  "phone": "+91 98765 43210",
  "email": "raj.gautam@bodhai.tech",
  "category": "VIP",
  "price": 2499,
  "seat": "VIP-24",
  "row": "ROW 04",
  "gate": "GATE A",
  "qrData": "BDH-2026-VIP-8F92A1|EV-2026-BDH01|VIP|Raj Gautam|2026-12-25",
  "status": "ACTIVE",
  "usedAt": null,
  "theme": "obsidian"
}
```

---

## 🎫 Ticket & Pass Templates

The built-in template system offers 5 switchable themes that update the ticket style in real time:

1. **Obsidian Glass (`theme-obsidian`):** Frosted glass look with cyan and violet accents. Designed for modern electronic music festivals.
2. **Cyber Neon (`theme-neon`):** High-contrast purple neon with glowing borders for nightclub tours and DJ sets.
3. **Royal Gold (`theme-gold`):** Jet-black finish with amber-gold borders for orchestral galas and luxury VVIP lounges.
4. **Festival Electric (`theme-festival`):** Vibrant magenta-blue gradient mesh for multi-day open-air concerts.
5. **Minimalist Clean (`theme-minimal`):** High-contrast light monochrome pass for acoustic sets, press conferences, and corporate auditoriums.

---

## 🛡️ Gate Verification & Anti-Fraud Flow

```
[Attendee Presents Pass]
          │
          ▼
[Camera / Barcode Reader]
          │
          ▼
Scan QR payload or input Monospace Ticket ID (e.g. BDH-VIP-2026-004)
          │
     ┌────┴─────────────────────────────┐
     │ Match found in LocalStorage DB?  │
     └────┬─────────────────────────────┘
          │
     ├──► NO  ──► ✕ INVALID TICKET (Admission Denied)
     │
     └──► YES
           │
           ├──► status === "CANCELLED" ──► ✕ PASS REVOKED (Access Denied)
           │
           ├──► status === "USED"      ──► ⚠ ALREADY USED (Duplicate Alert)
           │
           └──► status === "ACTIVE"    ──► ✓ VALID TICKET (Access Granted)
                                                │
                                                ▼
                                    [Authorize & Mark As Used]
                                                │
                                                ▼
                                    Status becomes "USED"
                                    Timestamp logged
                                    Turnout Analytics updated
```

---

## 🖨️ Export & Print System

- **Download PNG:** Uses `html2canvas` at high pixel density ($2.5\times$ scale) to create crisp, social-ready ticket cards.
- **Download PDF:** Uses `jsPDF` to compile a vector landscape concert pass measuring $195\text{ mm} \times 80\text{ mm}$.
- **Print System (`@media print`):** Strips the dashboard, navbar, background controls, and buttons—sending only the physical concert stub directly to thermal or document printers.

---

## 📄 License & Attribution

- Built as an enterprise event-technology prototype under the **MIT License**.
- Developed for **BODHAI Event Technology**.

---

*© 2026 BODHAI — Event Technology. All rights reserved.*
