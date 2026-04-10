# Project Spec: Substrate Savings Calculator – SoilSteam

## Overview
A mobile-first single-page web app that lets growers calculate how much they can save by reusing substrate with SoilSteam's machine. Users enter their substrate cost, annual volume, and energy details; the app instantly shows savings, energy use, machine run time, and new substrate needed. Deployed as a PWA and accessed via QR code at agri-tech trade stands.
After the user has inserted the data and seen the values a form should appear at the bottom where they can fill in contact information. 

---

## Users
Who will use this and how do they access it?
- Primary user: Growers / farm operators at a trade stand
- Access method: QR code → mobile browser
- Technical level: Non-technical

---

## Goals
What does success look like? Keep to 3-5 bullet points.
- [x] Show estimated annual savings vs. buying all new substrate
- [x] Show energy consumption (diesel liters or kWh) and machine run hours
- [x] Show how much new substrate still needs to be purchased
- [x] Work well on mobile with no install required (PWA)
- [x] Reflect SoilSteam brand accurately to serve as a promotional tool

## Out of Scope
What are you explicitly NOT building? This prevents scope creep.
- User accounts, data persistence, or sharing
- Backend / server-side computation
- Multi-language support

---

## Technical Stack
- Hosting: Azure Static Web App
- Frontend: Vanilla HTML / CSS / JS (single `index.html`)
- Backend: None
- Database: None
- Special requirements: PWA (manifest + service worker), mobile-first, offline-capable

---

## Functional Requirements

### Inputs
| Field | Type | Default | Notes |
|-------|------|---------|-------|
| Price per m³ of new substrate | number | — | Required, > 0, unit: €/m³ |
| Total substrate per year | number | — | Required, > 0, unit: m³ |
| Fee to get rid of old substrate | number | 0 | unit €/m³
| Reuse percentage | range slider | 90% | 0–100%, step 1 |
| Substrate type | dropdown | Peat | Should be linked to CO2 saved |
| Energy type | radio toggle | Diesel | Options: Diesel / Electricity |
| Energy price | number | €2.00/L (diesel) or €0.20/kWh (electricity) | Switches automatically when energy type changes |

### Outputs / Results
- **New substrate needed** — m³ still required after reuse
- **Energy usage** — liters of diesel or kWh depending on selected energy type
- **Machine running time** — hours per year
- **Substrate reused** — m³ processed by the machine
- **Estimated annual savings** — € saved vs. buying all new substrate (highlighted, full-width)
- **Estimated annual equivalent CO2 savings** - should use the substrate type and amount to calculate saved CO2 should be given in tons

### Business Logic / Formulas
```
Constants:
  KWH_PER_M3_PROCESSED   = 35       (kWh per m³ of reused substrate)
  M3_PER_HOUR            = 3.5      (machine throughput)
  OPERATOR_COST_PER_HOUR = 25       (€/h)
  DIESEL_KWH_EQUIVALENT  = 9.6      (kWh per liter of diesel)
  CO2_EQUIVALENT_PEAT = 250 (kg)
  CO2_EQUIVALENT_COCO = 70 (kg)
  CO2_EQUIVALENT_ROCKWOOL = 125 (kg)
  CO2_EQUIVALENT_DIESEL = 2.640 (kg)
  CO2_EQUIVALENT_ELECTRICITY = 0.009 (kg)

Derived values:
  reused_volume   = volume * reuse_ratio
  new_substrate   = volume * (1 - reuse_ratio)
  energy_kwh      = reused_volume * KWH_PER_M3_PROCESSED
  energy_liters   = energy_kwh / DIESEL_KWH_EQUIVALENT
  hours           = reused_volume / M3_PER_HOUR

  energy_cost     = energy_liters * energy_price   (if diesel)
                  = energy_kwh    * energy_price   (if electricity)

  co2_energy      = energy_liters * CO2_EQUIVALENT_DIESEL (if diesel)
                    energy_kwh    * CO2_EQUIVALENT_ELECTRICITY (if electricity)

  savings = volume * price
            - new_substrate * price
            - energy_cost
            - hours * OPERATOR_COST_PER_HOUR
            + landfill_fee * volume
  
  tot_co2_eq = reused_volume * co2_for_chosen_substrate - co2_energy

Edge cases:
  - Results hidden until both price and volume are > 0
  - Savings can be negative — displayed as -€ value
```

### User Actions / Flows
1. User scans QR code, lands on the page on their phone
2. User fills in substrate price and annual volume
3. Results appear immediately (real-time, no submit button)
4. User can adjust reuse %, substrate type, and energy type/price
5. All outputs update live as inputs change
6. Once results are visible, a contact card appears below with flavor text: *"Like what you see? Get in touch with one of our sales representatives."*
7. A "Contact us" button links to soilsteam.com/contact-us/ in a new tab

---

## UI / Visual Requirements

### Brand Colors
Define exact values — do not leave this to interpretation.
```
Primary:     #34876D   (card titles, slider accent, footer links, input focus border)
Secondary:   #06515D   (headings, result values, savings box background, theme-color meta)
Background:  #EDEBE6   (page background — sand-light)
Sand:        #D1CBBD   (card borders, slider track, result item background)
Other:       #564A3D   (form labels)
Text:        #1A1A1A   (body text, input values)
Muted:       #6B6B6B   (placeholder text, units, secondary labels)
White:       #FFFFFF   (card backgrounds, header, active toggle label)
```

### Typography
```
Heading font:  Inter, weight 600, ~1.1rem
Body font:     Inter, weight 400, ~0.82–0.95rem
Source:        https://fonts.google.com/specimen/Inter
Weights used:  300, 400, 500, 600, 700
```

### Logo & Assets
- Logo file: `Logo_soilsteam.png` (in project root)
- Usage: displayed in header, height 36px
- Style reference screenshots: `C:\Users\GustavOmbergOften-So\substrate_calculator\style`

### Layout
- Priority: mobile-first
- Max content width: 520px (centered)
- Cards: 12px border radius, 1px sand border, subtle drop shadow
- Base spacing unit: 16px

### Visual Tone
- [x] Light / minimal / mostly white
- [x] Professional / technical
- [x] Friendly / approachable
- Reference screenshots: `style/` folder

### Must Include
- [x] Logo placement: top left in white header bar
- [x] Footer: link to [soilsteam.com](https://soilsteam.com)
- [ ] No mandatory legal text identified yet
- [ ] Contact form

---

## Non-Functional Requirements
- Performance: instant load, works offline after first visit (service worker)
- Cost: static hosting only — no server compute
- Accessibility: readable contrast ratios, labeled inputs; WCAG AA target
- Browser support: all modern mobile browsers (Chrome, Safari iOS)

---

## Open Questions
Things that are undecided or need clarification before or during build.
- [ ] Should the operator cost per hour (€25/h) be exposed as a user-adjustable input or kept as a hidden constant?
- [ ] Are there any legal/compliance requirements for promotional tools shown at trade stands?

---

## Change Log
| Date | Change |
|------|--------|
| 2026-04-09 | Initial spec written from requirements.md + implemented index.html |
