# ShilpSathi — Viable Solution for PS 26090
### AI-Driven Market Linkage & Smart Cataloging for Marginalized Artisans | MoSJE

> **Core Idea:** Don't build AI that *replaces* artisans. Build AI that *removes the photo-typing-pricing burden* in under 60 seconds, on a Rs. 7000 phone, without internet.

---

## 1. Why Most Solutions Are NOT Viable (And Ours Is)

| Common SIH Mistake | Why It Fails in Field | ShilpSathi Viable Fix |
|---|---|---|
| `GPT-4 + remove.bg API` for every product | Rs. 4-5 per product, needs internet, fails in village, recurring cost unsustainable for govt | **On-device + Govt-funded free APIs (Bhashini) + Self-hosted open models.** Cost ~ Rs. 0.08/product |
| ONDC/GeM direct integration Day 1 | ONDC seller onboarding needs GST, PAN, KYC — 90% artisans don't have it | **Phased linkage:** Phase 1 = Internal Marketplace + WhatsApp Storefront (no KYC). Phase 2 = Assisted GeM/ONDC via Cluster Coordinator |
| Heavy ML Pricing Model | No labeled pricing data for 500+ crafts exists | **Hybrid Rule-Based (Cost+) + Market Reference Table.** ML only after 10k transactions collected |
| English-first App with Translation | Translation is poor for craft terms (e.g. "Tussar", "Zari") | **Voice-First, Template-First catalog.** No typing, no translation errors |
| Flutter App 50MB+ | Won't install on 16GB phones, no updates | **Lite APK 12MB + PWA fallback + Offline Sync** |

**Viability Definition for MoSJE:** Works for a 45-year-old Bhojpuri-speaking weaver in Bhagalpur with Redmi 9A, 1GB data/month, intermittent 2G.

---

## 2. Viability Constraints — We Designed For These

1.  **Device:** Android 8+, 2GB RAM, 16GB storage. No iPhone assumption.
2.  **Network:** 60% offline. 2G in clusters. Upload must work on 50kbps.
3.  **Language:** 22 official languages + dialects. Roman typing not viable.
4.  **Trust:** Artisans fear online fraud. Need human (CRP/SHG Didi) in loop.
5.  **Economics:** Artisan can't pay subscription. Govt can't pay OpenAI bills forever.

---

## 3. Architecture — Viable & Low Cost

```
[Artisan Phone]                          [Cluster / Cloud when online]
┌─────────────────────────┐              ┌──────────────────────────┐
│ Flutter Lite App (12MB) │              │ Backend: FastAPI + Node │
│ ├ SQLite (Offline DB)   │◄── Sync ────►│ ├ Bhashini/STT Proxy    │
│ ├ On-Device AI (TFLite) │  WorkManager │ ├ Catalog Service       │
│ │  • BG Remove (U2Net)  │  (bg sync)   │ ├ Pricing Engine        │
│ │  • Blur Check         │              │ └ ONDC/GeM Connector    │
│ └ Voice UI (Bhashini)   │              │                         │
└─────────────────────────┘              │ Postgres + S3 (MeitY Cloud) │
        │ WhatsApp Share Link            └──────────────────────────┘
        └──────────► Buyer (No App Needed)
```

**Hosting:** MeitY Empanelled Cloud (NIC/MeghRaj) or AWS Mumbai (data residency). Auto-scale OFF — we use Serverless (Cloud Run) to keep cost low when idle.

---

## 4. Feature 1: AI Image Studio — Viable Implementation

**What Artisan Does:** Place product on floor -> Tap `Photo Lo` -> App says "Rukiye, photo saaf kar rahe hain"

**Viable Pipeline (No Paid API):**
1.  **On-Phone (Instant, Offline):** `TFLite U2Net` (2.1MB model) removes background -> checks blur/lighting -> auto-crops to 1:1. Takes <2 sec on 2GB RAM.
2.  **If Online (Enhanced):** Sends 80KB compressed image to Server -> `BiRefNet` (open-source, self-hosted) does high-quality cutout + `Real-ESRGAN-light` sharpens + adds white background + shadow.
3.  **Fallback:** If AI fails, offer 3 background choices as static images (White, Jute Texture, Lifestyle). No generation needed.

**Why Viable:** No per-image cost. Works offline. We don't promise "studio lifestyle mockup generation" (needs Stable Diffusion, heavy). We promise "GeM-compliant white background" — which is what actually sells.

**Output:** 4 images auto-created: 1) Main White BG 2) Zoom 3) Lifestyle Template 4) Size Reference. All <200KB.

---

## 5. Feature 2: Multilingual Auto-Cataloger — Viable Implementation

**What Artisan Does:** Hold mic -> Speak 10 sec: *"Ye Bhagalpuri Tussar saree hai, haath se bani hai, teen din lagte hain"*

**Viable Pipeline (Govt Stack):**
```
Voice → Bhashini ASR (Free, supports 22 langs) → Text (Bhojpuri/Hindi)
Text → Entity Extractor (Small LLM/Sarvam AI - Hosted) → JSON {craft, material, time}
JSON → Template Engine → Title + Description (Hindi + English)
```

**We DO NOT use GPT-4 for every description.** We use:

*   **80% Template-Based:** 50 curated templates by craft type. E.g. Tussar Saree Template: `"Handmade {craft} {product} in Pure {material} | {region} Traditional Art | {USP} | Ideal for {occasion}"` — Filled by extracted entities. Zero hallucination, SEO perfect, runs offline.
*   **20% LLM Polishing:** Only if artisan wants "story" — then one call to `Sarvam-M` (Indian LLM, cheap, hosted) or `Llama-3-8B` self-hosted. Cost Rs. 0.02 vs Rs. 1.5 for GPT-4.

**Bhashini > Whisper:** Because Bhashini is free for MoSJE projects and trained on Indian dialects. Whisper fails on Maithili/Bhojpuri.

**What Gets Generated:**
*   Title (Hindi + English)
*   Description 80-120 words
*   Auto-attributes: Material, Craft, Color (from image classifier MobileNetV3 1MB), Size
*   Hashtags + Search Keywords
*   **Voice Output:** Bhashini TTS reads back description to artisan for confirmation — she can say "Haan" or "Badlo"

**Editable by Voice:** *"Price wala line hata do"* -> Intent detected -> Removed.

---

## 6. Feature 3: Dynamic Pricing Assistant — Viable Implementation

**Reality Check:** True ML pricing needs 50k+ sales data which doesn't exist. So we build VIABLE Hybrid:

**Step 1 (Day 1, No ML): Cost-Plus + Market Anchor**

```
Artisan says: "Kapda 600 ka, 3 din lage"
→ LLM extracts: cost=600, labour_days=3
→ Labour Rate = Rs. 350/day (state-wise config)
→ Base Cost = 600 + (3*350) = 1650
→ Market Anchor = Avg price of "Tussar Saree" from 3 sources (scraped weekly via cron: Amazon, GeM, ONDC) = e.g. 2200
→ Suggested Price = (Base Cost * 1.15 profit) clamped between Anchor ±20% = Rs. 1897 → Show as Rs. 1890
```

**UI Shows Breakdown in Voice + Visual:**
```
Market me daam: Rs. 2000-2400
Aapki lagat: Rs. 1650
Munafa (15%): Rs. 247
>> Suggested: Rs. 1890 [Becho] 
[ Kam Rs. 1750 ] [ Zyada Rs. 2100 ]
```
Artisan chooses. We never force.

**Step 2 (After 3 months data):** Train simple `XGBoost` on our own sales: Features = craft, material, cost, market anchor -> Target = sold price. This is viable because data comes from our app, not scraped.

**Why Viable:** Explainable, trustable, works Day 1 without training data, no black-box.

---

## 7. Market Linkage — Viable Phased Plan

**Phase 1 (SIH Demo + Pilot 0-6 Months): No KYC Headache**
*   **Internal Marketplace + WhatsApp Store:** Every artisan gets `shilpsathi.in/rani` link. Buyer (even without app) can view, chat on WhatsApp, order via COD. Zero ONDC/GST needed. This alone solves "year-round channel".
*   **Assisted B2B:** B2B Buyers app/portal where they post bulk demand: "Need 500 Tussar Sarees". Cluster Coordinator matches artisans. Trust layer = human.

**Phase 2 (6-12 Months): Assisted ONDC/GeM**
*   Artisan with Udyam/GST -> One-tap publish to ONDC via Seller App (like Mystore). We don't build ONDC protocol; we integrate via `ONDC Gateway Sandbox` as Seller.
*   For others: Products pooled under `Cluster ID` (e.g., Bhagalpur Handloom Cluster) which has GST, managed by NGO. Artisan gets credited. This is how GeM actually works for artisans today — we digitize it.

**Logistics Viable:** No own fleet. Integrate `India Post` (has village reach, govt tie-up easy) + `Shiprocket` API. COD via India Post. UPI via `e-RUPI`/`UPI Autopay`. Order alert via SMS if no internet.

---

## 8. Viable Tech Stack with Cost

| Layer | Choice | Viable Reason | Cost |
|---|---|---|---|
| **App** | Flutter + SQLite + WorkManager | 1 codebase, 12MB, offline queue, runs on Android 8 | Free |
| **On-Device AI** | TFLite U2Net + MobileNetV3 | 3MB total, offline, no server cost | Free |
| **Speech** | Bhashini ASR/TTS + Sarvam AI | Free for govt, 22 langs, better than Google for dialects | Rs. 0 |
| **Backend AI** | Self-hosted Llama-3-8B + BiRefNet on Cloud Run (CPU) | No OpenAI bill. Scales to 0 when idle | ~Rs. 3000/mo |
| **Backend** | Python FastAPI + Postgres + S3 | All open source, MeitY compliant | ~Rs. 2500/mo (NIC) |
| **Search** | Postgres Full-Text (no Pinecone) | Works for 1L products, no vector DB cost initially | Free |
| **Auth** | OTP via Fast2SMS (Rs. 0.10/SMS) | No password, works on feature phones | Pay per use |

**Total Infra for 10,000 artisans: ~Rs. 6000-8000/month.** Viable for MoSJE to sustain.

---

## 9. Offline-First & Low-End Strategy

*   List product fully offline (photo+voice stored locally). Syncs when WhatsApp opens (user has net).
*   Images: Auto-compress to 80KB (WebP) for 2G upload. Server reconstructs high-res.
*   **PWA Fallback:** If artisan can't install APK, open `app.shilpsathi.in` in Chrome — works 90% same.
*   **IVR Helpline:** For no-smartphone artisans: Call toll-free, speak product details -> CRP lists on their behalf (human-in-loop).

---

## 10. Onboarding — How We Get 1000 Artisans in 3 Months (Viable GTM)

Tech alone fails. We piggyback on existing structure:

1.  **Via Cluster Resource Persons (CRPs) & SHG Didis:** Train 1 CRP per 50 artisans (already employed by MoSJE). CRP has "Master App" to bulk onboard via Aadhaar OTP, does first 3 listings with artisan. Artisan learns by seeing.
2.  **Demo at Shilp Samagam Melas:** Set up stall: "60 sec me apna saman online karo". Instant WhatsApp link = wow moment.
3.  **Incentive:** First 3 orders zero commission. MoSJE can give Rs. 500 digital onboarding incentive via DBT (existing scheme).

**Material:** 2-min video in Maithili/Bhojpuri with local artisan hero, not Hindi explainer.

---

## 11. What We Will Demo Live at SIH (100% Viable MVP)

Judges can test on their phone (no mock):

1.  **Photo Studio:** Take photo of pen/bottle -> See background removed offline in 2 sec + GeM crop.
2.  **Voice Catalog:** Speak in Hindi/Bhojpuri -> See title/description in Hindi+English + auto-tags in 4 sec (via Bhashini + template).
3.  **Pricing:** Speak cost -> See breakdown + slider + voice justification.
4.  **WhatsApp Storefront:** Tap Publish -> Get shareable link -> Open on another phone, place COD order -> Artisan gets order notification + SMS.

**All running on real APIs, no hard-coded slides.** Backend on Render/Railway free tier for demo, ready to move to NIC.

---

## 12. Business Sustainability

*   **For Artisan:** Free forever. 0 fees till Rs. 50k sales/year. After that 5% commission only on B2B bulk orders (where we added value). No upfront cost.
*   **For Govt:** Infra cost < 1% of current Mela subsidy per artisan. Dashboard shows ROI: Sales, income uplift.
*   **Revenue for Scale:** Commission + Logistics margin (Rs. 10/order from Shiprocket) + Premium B2B Buyer subscription (Rs. 999/mo for verified artisan access).

---

## 13. Roadmap (Viable Sprints)

| Sprint | Weeks | Shippable |
|---|---|---|
| S1 | 1-3 | Flutter app, offline DB, on-device BG remove, Bhashini ASR working |
| S2 | 4-6 | Template catalog (50 templates), Cost-plus pricing, Shareable storefront |
| S3 | 7-9 | Server enhancement (BiRefNet), Order + India Post slip, SMS alerts |
| S4 | 10-12 | B2B Buyer portal, CRP Master App, Field test with 20 artisans in cluster |
| S5 | Post-SIH | ONDC sandbox + GeM assisted onboarding, Vector search, Income dashboard |

---

## 14. Risks & Viable Mitigations

| Risk | Mitigation |
|---|---|
| Bhashini STT fails for heavy dialect | Show transcript back, artisan says "Sahi/Nahi" by voice -> retry or CRP corrects once, system learns |
| Pricing distrust | Always show 3 options + breakdown, never auto-set price. Add "Padosi bech raha hai Rs. X" social proof |
| Internet = 0 for days | SMS order alert: "Naya Order #123: 1 Saree Rs. 1890. CALL 1800-... to accept" |
| Fake/low quality listings | CRP verification queue before public listing (1 tap approve) |

---

## 15. Impact Metrics (MoSJE Will Actually Measure)

*   Time to first listing: **<60 sec** (vs 25 mins on Amazon)
*   Listings without CRP help: **>80%**
*   Active sellers with ≥1 sale/month: **Target 60% in 6 months**
*   Avg income uplift: **+35% Year 1** (track via order value)
*   Women onboarding: **>50%**

---

## 16. Conclusion — Viability Statement

**ShilpSathi is viable because:**
1.  Costs Rs. 0 to artisan, <Rs. 1 to govt per product
2.  Works offline, on cheapest phone, in artisan's dialect
3.  Uses government-free stack (Bhashini, India Post, NIC Cloud)
4.  Solves Year-Round sales Day 1 via WhatsApp (no ONDC wait)
5.  Builds trust via human CRP + explainable pricing

> **Jury One-Liner:** *Bol kar becho, WhatsApp pe becho — no English, no typing, no internet needed.*

---
*Ready to Build Stack: Flutter, FastAPI, Postgres, TFLite, Bhashini, BiRefNet, Shiprocket | Repo: /app | Demo APK: Lite 12MB*
