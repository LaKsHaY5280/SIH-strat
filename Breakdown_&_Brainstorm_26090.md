# PS 26090 — Breakdown + Combined Brainstorm (Step 1)

> Goal of this doc: Break the confusing PS into 7 small parts, then brainstorm a single solution that covers ALL of them. Architecture & uniqueness later.

---

## PART-BY-PART BREAKDOWN

### Part 1: WHO are we building for?

**PS says:** `marginalized communities, micro-entrepreneurs, artisans, weavers`
**Real meaning:** Low-income, often rural, 40-60 yr old, shared low-end Android phone, Hindi/regional mother tongue, uses WhatsApp/YouTube(not the majority) but never Amazon Seller, fears English forms.
**Analysis:** The target user is not just "digitally illiterate" but "digitally anxious". They trust voice and visual cues over text.
**What we must solve:** If SHE can't use it without help, we failed.

### Part 2: WHAT is the current system & why it fails?

**PS says:** `Govt gives financial help + Melas like Shilp Samagam, Surajkund, Dilli Haat`
**Real meaning:** Artisans get temporary 5-10 day sales spike, then 355 days no income. Melas cost govt lakhs, but not sustainable. They have products, but no shop for rest of year.
**Analysis:** The "Mela Model" is an event-based economy. We need to transition this to a "Digital Storefront" economy.
**What we must solve:** Give them a 365-day shop that works after mela ends.

### Part 3: WHY can't they sell online today? (4 Barriers)

**PS says:** `low digital literacy, language barriers, lack of technical skills to photograph, price, catalog`
**Breakdown:**

- **Barrier A - Literacy:** Can't fill 15-field Amazon form.
- **Barrier B - Language:** Can't type English description, doesn't know SEO.
- **Barrier C - Skills:** Photos are dark/cluttered, pricing is guesswork.
- **Barrier D - Hardware:** Some don't even own a smart phone or have stable 4G.
  **What we must solve:** Remove typing, remove English, remove photo/price expertise. Automate all 3.

### Part 4: WHAT is the BIG ASK? (Virtual Business Manager)

**PS says:** `build intuitive AI-driven mobile app that acts as 'virtual business manager'`
**Real meaning:** Not just a listing tool. It should do what a shop manager does: Click photo, write board, set price, talk to buyer, take order, pack slip.
**Analysis:** The AI shouldn't just be a feature; it should be the _interface_. The artisan speaks/clicks; the AI manages the "corporate" side of the business.
**What we must solve:** One app = Inventory + Orders + Buyers, not just catalog.

### Part 5: FEATURE 1 — AI Image Enhancer & Studio

**PS says:** `auto-remove cluttered backgrounds, correct lighting, format to e-commerce standards`
**Real meaning:** Photo should look like Amazon white-background, even if taken on mud floor in low light.
**Technical Path:** Use Segment Anything Model (SAM) or similar for BG removal + AI Relighting for consistent studio-quality look.
**Brainstorm seed:** Voice-guided capture ("thoda ujale me jao") + offline BG remove + auto-crop to 1:1.

### Part 6: FEATURE 2 — Multilingual Auto-Cataloger

**PS says:** `describe via voice notes in regional languages → translate & generate SEO-friendly descriptions in English/Hindi`
**Real meaning:** Speak 10 sec in Bhojpuri → Get professional title+desc+tags in Hindi+English that ranks on search.
**Technical Path:** Bhashini STT (Govt of India) → LLM (GPT-4o/Claude) for structured extraction → SEO Template → Multilingual Output.
**Brainstorm seed:** Voice -> Bhashini STT -> Extract {material, craft, time} -> Template + Light LLM -> Hindi+English + Audio read-back.

### Part 7: FEATURE 3 — Dynamic Pricing Assistant

**PS says:** `analyze image+description → suggest optimal competitive price based on market trends & raw material costs`
**Real meaning:** Artisan says "kapda 500 ka, 2 din lage" + AI sees image -> says "Rs. 1650 becho, bazaar me 1800 hai".
**Technical Path:** Scrape data from GeM/ONDC/Etsy for "similar items" + Cost-Plus pricing model (Material + Labor hours \* Rate).
**Brainstorm seed:** Cost-plus (labour+material) + Market anchor (avg price scraped weekly) = Suggested price with 3 options + voice reason.

### Part 8: HOW should it be built? (Non-functional)

**PS says:** `cross-platform, robust scalable backend, highly responsive minimalist UI/UX, modern clean visual hierarchy`
**Real meaning:** Must run on cheap Android + iPhone with one codebase, handle 1 lakh users during mela, and be so simple that text is replaced by icons & voice.
**Brainstorm seed:** Flutter Lite (12MB) + Offline-first + Serverless backend + 3-tap flow + Every text has 🔊.

### Part 9: WHAT is success?

**PS says:** `year-round sales, lower barrier, improve literacy & financial independence, increase annual income`
**Real meaning:** Govt will measure: Avg income up? Daily active sellers? Time to first sale?
**What we must solve:** Build + Measure dashboard for MoSJE.

---

## MARKET RESEARCH & COMPETITIVE ANALYSIS

### Existing Solutions
1. **GeM (Govt e Marketplace):** Robust but high entry barrier (GST, complex registration, professional cataloging required).
2. **ONDC (Open Network for Digital Commerce):** A protocol, not an app. Great for reach, but artisans need a "seller app" to join.
3. **Etsy/Amazon Karigar:** High quality, but target audience is "professional" artisans, not marginalized rural ones.

### The "Gap" (Our Opportunity)
Current platforms assume the seller is **digitally capable**. We are building for the **digitally anxious**.
- **Competitors' Flow:** Sign up $\rightarrow$ Upload Photo $\rightarrow$ Write Desc $\rightarrow$ Set Price $\rightarrow$ List.
- **Our Flow:** Tap Mic/Call Bot $\rightarrow$ Click Photo $\rightarrow$ AI does everything else $\rightarrow$ List.

### Strategic Linkages
- **Bhashini:** Essential for regional voice-to-text.
- **ONDC Integration:** This is the "Killer Feature". Instead of building a new marketplace, we act as the "Onboarding Layer" for ONDC, giving artisans instant access to 100s of buyer apps.
- **GeM Integration:** Path for B2G (Business to Government) sales.

---

## THE UNIQUENESS STACK (Our "Winning" Hooks)

To stand out from other SIH teams, we aren't just building a tool; we are building a **Multi-Modal Business Interface**.

### 1. The "Zero-UI" AI Voice Agent (MVP)
**The Concept:** A phone-call based business manager for those without smartphones.
- **Flow:** Artisan calls a number $\rightarrow$ Conversational AI (via Bhashini + LLM) handles listing, pricing, and order updates.
- **Value:** Total digital inclusion. No app, no internet, no literacy required.

### 2. Natural Voice Edit ("Voice-to-Action")
**The Concept:** For app users, a "conversational layer" to manage the shop.
- **Example:** Instead of navigating a menu to change a price, the user says *"is saree ka daam 200 rupaye kam kar do"* $\rightarrow$ AI updates the database.
- **Value:** Removes the friction of complex UI navigation.

### 3. Offline SMS Order Pipeline
**The Concept:** Guaranteed order delivery regardless of internet connectivity.
- **Flow:** Order on ONDC $\rightarrow$ System triggers structured SMS to artisan $\rightarrow$ Artisan replies "YES" via SMS to confirm.
- **Value:** Operational reliability in deep rural areas with patchy 4G.

### 4. Heritage Visual Search (Buyer Side)
**The Concept:** An AR/Visual discovery engine for urban buyers.
- **Flow:** Buyer uploads a photo of a craft $\rightarrow$ AI identifies the specific regional style (e.g., *Warli* vs *Madhubani*) $\rightarrow$ Connects them to the exact artisan.
- **Value:** Bridges the "naming gap" between rural producers and urban consumers.

---

## COMBINED BRAINSTORM — ONE FLOW THAT COVERS ALL 9 PARTS


## COMBINED BRAINSTORM — ONE FLOW THAT COVERS ALL 9 PARTS

(Keep existing flow as it is strong)

We combine everything into a single **60-Second Listing Flow**. No separate features — all 3 AIs fire together.

```
START (Home Screen)
  |
  v
[Tap Big MIC Button]  ← Solves Part 3B (Language), Part 8 (Minimal UI)
  | "Apni bhasha chuno"
  v
[Photo Lo] — AI guides frame, auto-clicks  ← Solves Part 5
  | On-device BG remove + lighting fix (works offline)
  v
[Bolo — 10 sec voice] "Ye Tussar saree hai..." ← Solves Part 6
  | Bhashini → Text → Template → Hindi+Eng catalog
  v
[AI Together Decides Price] ← Solves Part 7
  | Shows: Market Rate | Your Cost | Suggested Price (with voice explain)
  v
[One Tap: Becho] → Creates:
  1. WhatsApp Storefront Link (instant sales, no KYC) ← Solves Part 2 (Year-round)
  2. Queued for ONDC/GeM (when KYC ready)
  3. Inventory + Order manager opens ← Solves Part 4 (Business Manager)
```

**This one flow checks every PS box:**
Part 1 ✓ (voice+offline for low-end), Part 2 ✓ (WhatsApp shop day 1), Part 3 ✓, Part 4 ✓, Part 5 ✓, Part 6 ✓, Part 7 ✓, Part 8 ✓

**Role of Human (SHG Didi/CRP):**
App has "Didi Mode" — CRP can onboard 10 artisans in village, first 3 listings she helps, after that artisan is solo. Solves trust + training.

---

## NEXT STEP: ARCHITECTURE

We have locked the base flow and the 4 uniqueness hooks. The next phase is to design a robust, scalable architecture that can support:
1. The Mobile App (Flutter)
2. The AI Voice Agent (Phone Call Interface)
3. The ONDC/GeM Integration Layer
4. The Visual Search Engine (Buyer Side)

We will now move to Step 3: Architecture.

_Teams that win SIH don't build 3 separate AIs — they build 1 flow where 3 AIs are invisible to user._
