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

### 1. The "Zero-UI" AI Voice Agent (Future Accessibility Channel)
**The Concept:** A phone-call based business manager for those without smartphones.
- **Flow:** Artisan calls a number $\rightarrow$ Conversational AI (via Bhashini + LLM) handles listing, pricing, and order updates.
- **Value:** Total digital inclusion. No app, no internet, no literacy required.
- **Scope Clarification:** 
  - **Not in MVP:** Due to complexity and cost, the Zero-UI Voice Agent is scheduled for Phase 3
  - **Actual MVP:** Mobile App + Voice AI (within app) + Catalog + Pricing + Orders + WhatsApp Storefront
  - **Future Accessibility Channel:** Phone-call interface will be developed after validating core MVP
  - **Reasoning:** Allows us to focus on perfecting the primary user experience while maintaining the long-term vision of total inclusion

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

## STRESS TEST & LOOPEHOLE ANALYSIS - ADDRESSING CRITICAL FEEDBACK

Based on deep-dive analysis and incorporating external feedback, here are the critical improvements made to our solution:

### MAJOR LOOPOLES ADDRESSED

#### 1. **Market Linkage vs. Just Digitization** (Most Critical Feedback)
- **Original Gap:** Solution focused on creating online-ready products but didn't ensure buyer demand
- **Fix Implemented:** Added explicit **Market Access Engine** with actual matching mechanisms (not just connectivity):
  - **Demand Matching Layer:** Core algorithmic function that connects structured product profiles to actual demand
    - **B2C Matching:** Product profile → matched against existing buyer search patterns, seasonal trends, and social commerce algorithms
    - **B2B Matching:** Buyer requirements (craft/material/quantity/budget/location/deadline) → matched to artisan/cluster capacity → generates structured quotation
    - **Discovery Boost:** Featured placement in ONDC participant apps based on relevance scoring
    - **Social Amplification:** AI-generated shareable content snippets optimized for platform algorithms
  - **Channel Infrastructure:** 
    - **B2C Channel:** ONDC integration (via appropriate Seller Network Participant) + shareable storefronts + social commerce
    - **B2B Channel:** Bulk buyer matching system (hotels, corporations, NGOs) with quotation workflow
    - **Government Channel:** GeM readiness assessment + guided onboarding
- **New Flow:** Create → Publish → **Discover (via Matching Engine)** → Receive Order → Fulfill → Payment
- **Key Change:** Market linkage is now an active algorithmic subsystem that creates liquidity, not just passive channels

#### 2. **ONDC Integration Realism**
- **Original Gap:** Oversimplified ONDC as a simple API switch
- **Fix Implemented:** Clearly defined our role as **Artisan-Facing Seller Layer** with cold-start strategy:
  - We prepare, validate, and manage artisan catalogs
  - We connect to ONDC through an appropriate **Seller Network Participant** or technology partner
  - **Cold-Start Strategy:** Rather than building a new buyer network, we position ourselves as an AI commerce infrastructure layer that connects artisans to existing demand networks:
    - Leverage existing ONDC participant apps for B2C demand
    - Connect to government procurement programs (GeM) for institutional demand
    - Partner with NGOs/corporates for B2B bulk orders
    - Enable social commerce via WhatsApp/Instagram shareable links
  - For MVP: Demonstrate integration boundary via sandbox/testing environment
  - Handle: Catalog formatting, compliance checks, basic inventory sync
- **Key Change:** No longer claiming "ONDC integration" but rather "ONDC readiness enablement" with explicit cold-start mitigation

#### 3. **GeM Readiness vs. Simple KYC**
- **Original Gap:** "Queued for GeM when KYC ready" was dangerously oversimplified
- **Fix Implemented:** Introduced **Market Readiness Engine** with 4-stage assessment:
  - **Stage 0:** Product created (basic listing)
  - **Stage 1:** Basic selling ready (WhatsApp/storefront)
  - **Stage 2:** ONDC-ready (catalog compliance, basic logistics)
  - **Stage 3:** B2B-ready (bulk capacity, quality consistency, invoicing)
  - **Stage 4:** Government procurement-ready (GeM compliance, PAN/business details, certifications)
- **Artisan Feedback:** System tells user: "Aapka product ONDC ke liye ready hai" or "Government ke liye PAN chahiye"

#### 4. **User Assumptions Made Evidence-Based**
- **Original Gap:** Overly specific demographic assumptions (40-60 yr, rural, Android)
- **Fix Implemented:** Reframed as research-backed **User Model**:
  - Primary design target: Low-digital-literacy artisan with limited e-commerce experience
  - May operate on low-end mobile device AND/OR assisted through local facilitator (SHG/CRP/Didi)
  - Focus on accessibility barriers rather than assuming specific demographics
  - User research will establish actual demographic profile during deployment

#### 5. **Voice-to-Database → Voice-to-Confirmation** (Critical Fix)
- **Original Gap:** Voice → database (risk of silent errors)
- **First Fix Implemented:** **Voice → AI Extraction → Confirmation** workflow:
  - System: "Maine suna: Bhagalpuri silk, 6 metres, handwoven, ₹1,800 material cost. Sahi hai?"
  - Artisan: Yes / Change / Repeat
  - **Foundational Rule:** AI proposes. Artisan confirms.
- **Enhancement for Clarity:** **Multimodal Confirmation** to reduce ambiguity in voice-only confirmation:
  - **AI Voice:** "6 metre? ₹1,800 cost?"
  - **Screen Shows:** 
    ```
    6 m
    ₹1,800
    ```
  - **Artisan Response Options:**
    - **🎤 Haan** (voice confirmation)
    - **✏️ Change** (to modify specific fields)
  - **Principle Maintained:** Still voice-first, but not voice-only - leverages user trust in visual cues
  - **Applied to:** Product details, pricing, image enhancements, catalog descriptions

#### 6. **Image Enhancement Ethics & Trust**
- **Original Gap:** Risk of moving from enhancement to fabrication
- **Fix Implemented:** **Split image processing into:**
  - **Safe Transformations (Allowed):**
    - Crop, background removal, exposure correction
    - White balance, mild denoise, perspective correction
  - **Prohibited/Flagged Transformations:**
    - Changing product structure, adding/removing craftsmanship
    - Modifying patterns, altering colors beyond calibrated correction
  - **Trust Feature:** Show "Original photo" + "Store photo" side-by-side

#### 7. **Pricing: Optimal → Advisor with Transparency**
- **Original Gap:** Claiming "optimal price" from vague formula
- **Fix Implemented:** **AI Price Advisor** with transparent breakdown and uncertainty quantification:
  - **Suggested Price:** ₹1,650
  - **Cost Floor:** ₹1,100 (Material + Labor × Skill Rate + Packaging)
  - **Comparable Market Band:** ₹1,500–₹1,900 (from GeM/ONDC/Etsy scraping)
  - **Confidence Score:** Medium (based on 8 comparable listings found)
  - **Suggested Range:** ₹1,600–₹1,750
  - **Explanation:** "₹1,650 gives you approximately ₹550 above estimated production cost"
  - **Labor Economics Grounding:** 
    - System supports government/organization configured labor benchmarks
    - Plus artisan-adjustable personal labor rate
    - Does not pretend AI knows the "correct wage" - provides transparent calculation

#### 8. **Realistic Timeline: "60-Second Assisted Creation"**
- **Original Gap:** Overly optimistic "complete listing in 60 seconds"
- **Fix Implemented:** Clearly demarcated:
  - **60-second assisted product creation** (photo + voice → AI draft)
  - **Artisan confirmation & refinement** (variable time based on user comfort)
  - **Final publishing** (one tap after confirmation)
- **Manages Expectations:** First listing may be 2-3 minutes with confirmation loop

#### 9. **Proper Commerce Data Model**
- **Original Gap:** Assumed one product = one listing
- **Fix Implemented:** **Commerce-oriented data model from Day 1:**
  - Product → Variants (size/color/design) → Inventory Levels
  - Inventory → Orders → Customers → Fulfilment
  - Supports: Made-to-order, batches, one-of-one, custom orders
  - UI remains simple while backend handles complexity

#### 10. **B2B Capability Elevated from Afterthought**
- **Original Gap:** Underdeveloped B2B despite explicit PS mention
- **Fix Implemented:** **Bulk Buyer Matching & Quotation System** (coordination handled in later phases):
  - **Matching Function:** Buyer (Hotel/NGO/Corp) submits: "500 jute bags, ₹700-900/unit, 30-day delivery"
  - **System Response:** Matches to artisan cluster capacity → provides structured quotation with:
    - Item specifications
    - Quantity available
    - Price per unit
    - Estimated delivery timeline
    - Quality standards
  - **Coordination Boundary:** For SIH MVP, we focus on **opportunity matching and quotation workflow**
    - Cluster-level fulfillment, quality control, and payment distribution are operational layers to be addressed in Phase 4
    - This keeps the MVP credible while establishing the foundation for B2B commerce
  - **Handles:** Minimum order quantities, basic delivery scheduling
  - **Directly addresses PS:** "connecting artisans with larger B2B buyers" through opportunity creation

#### 11. **Progressive Scope Implementation (MVP Definition)**
- **Original Risk:** Attempting to build everything at once
- **Fix Implemented:** **Clearly phased approach:**
  - **Core MVP (SIH Submission):** Mobile App + Voice AI (within app) + Catalog + Pricing + Orders + WhatsApp Storefront
    - **What is Voice AI?** Voice AI refers to our conversational AI capability stack that enables natural voice interactions within the mobile app. It comprises:
      - Speech Input (microphone capture)
      - Language Identification (detecting regional language)
      - Speech-to-Text (STT) conversion via Bhashini or fallback providers
      - Intent/Entity Extraction (understanding product details, commands)
      - Action Planner (determining what database operation to perform)
      - Confirmation System (voice + multimodal verification)
      - Database Action (executing the intended operation)
      - Response Generation (TTS/visual feedback)
  - **Phase 2:** ONDC integration + assisted onboarding via sandbox
  - **Phase 3:** Phone-call interface (Zero-UI Voice Agent) - Future Accessibility Channel
  - **Phase 4:** B2B matching + Visual discovery + GeM readiness
- **Architecture** designed to support all phases without rework

#### 12. **Visual Search: From Bolted-on to Strategic Buyer-Side Tool**
- **Original Status:** Nice-to-have buyer feature
- **Strategic Positioning:** Kept as **Innovation Roadmap item** (not MVP)
  - **Rationale:** Solves discovery problem for B2C buyers who see crafts but don't know names
  - **Implementation:** Post-MVP enhancement once core marketplace linkage is proven
  - **Focus:** Heritage-specific visual search (Warli vs Madhubani etc.) with confidence scores

#### 13. **Hybrid Intelligence Architecture (Offline-First Realism)**
- **Original Risk:** Over-promising offline AI capabilities
- **Fix Implemented:** **Hybrid Intelligence Model:**
  - **On Device (Always Available):**
    - Camera controls, basic cropping, compression
    - Cached UI elements, local drafts, offline queue
  - **Cloud (When Connected):**
    - Advanced segmentation, STT, translation, LLM catalog gen
    - Market intelligence, pricing updates, compliance checks
  - **Offline Fallback:**
    - Save product/voice locally, sync via SMS when connectivity returns
  - **Manages Expectations:** No "47-second waits" - clear connectivity status indicators

#### 14. **Complete Order Lifecycle Management**
- **Original Gap:** Focused only on Create → Sell
- **Fix Implemented:** **Full Order Operations Manager:**
  - **States:** New → Confirmed → Preparing → Shipped → Delivered → Completed
  - **Exception Flows:** Cancel / Return / Refund / Customer Support
  - **ONDC Alignment:** Proper handling of returns/refunds/cancellations per ONDC guidance
  - **SMS Role:** Notification channel only (not replacement for full commerce protocol)

#### 15. **Trust & Provenance Built-In**
- **Original Gap:** Missing trust signals for buyers
- **Fix Implemented:** **Trust Signals in Every Listing:**
  - Artisan Verified (via SHG/CRP onboarding or govt ID)
  - Craft Region/Geographic Indicator
  - Handmade Declaration + Production Story (voice/text)
  - Original Photo + AI-enhanced Photo
  - Artisan Voice Narrative (optional): "Mainne yeh saree 3 din mein buni..."
  - Preserves cultural heritage while enabling commerce

#### 16. **UX Philosophy: Radical Simplification**
- **Original Risk:** Feature overload overwhelming target user
- **Initial Claim:** "User experience must be simpler than WhatsApp"
- **Issue Identified:** This was treated as an outcome rather than a design goal
- **Fix Implemented:** **Reframed as measurable design principle** with validation approach:
  - **Design Principle:** "Target UX principle: fewer interaction decisions than conventional messaging/e-commerce apps"
  - **Validation Metrics (to be measured during testing):**
    - Time to first product creation
    - Number of taps/actions required for core tasks
    - Error correction rate
    - Task completion rate
    - Assistance required percentage
  - **Implementation Approach:** 
    - Progressive disclosure: Home screen shows only Sell / My Orders / My Money
    - Voice can expose deeper functions: "Meri saree ka daam 200 rupaye kam karo."
    - Contextual UI appears when needed (edit product, view customers, etc.)
    - Everything else hidden: Inventory management, pricing logic, ONDC sync, etc.
  - **Central Principle:** User experience must be radically simpler than conventional apps - measurable through the above metrics, not just an aspiration

#### 17. **Didi Mode Elevated to Core Architecture**
- **Original Status:** Afterthought near end of document
- **Fix Implemented:** **Core Deployment Mechanism:**
  - Government/NGO/SHG/CRP onboard cluster via Didi Mode
  - CRP helps first 3 listings per artisan
  - Artisan gradually becomes independent (tracked via usage/competency)
  - Directly addresses adoption challenge: Not "launch app → hope they use"
  - Aligns with ONDC recognition of need for seller hand-holding/training

#### 18. **Sustainable Business Model Framework**
- **Original Gap:** No answer to "How does this survive post-hackathon?"
- **Fix Implemented:** **Three-Pronged Approach:**
  - **Deployment Subsidy:** Government/institutional covers onboarding (Didi Mode costs)
  - **Infrastructure Fee:** Tiny transaction-based fee (only on successful sales)
  - **Partnership Funding:** Corporate CSR/NGO grants for specific craft clusters
  - **Core Principle:** Never charge artisans directly; monetize via value created for buyers/institutions

#### 19. **API Abstraction Layer (Dependency Risk Mitigation)**
- **Original Risk:** Tight coupling to specific APIs (Bhashini, LLM providers, etc.)
- **Fix Implemented:** **AI Gateway with Pluggable Providers:**
  ```
  AI Gateway
   ├── STT Provider (Bhashini/Google/Whisper fallback)
   ├── Translation Provider (Bhashini/Microsoft/Amazon fallback)
   ├── LLM Provider (Open-source Llama/OpenAI/Anthropic fallback)
   └── Vision Provider (OpenCV/SAM/TensorFlow fallback)
  ```
  - Enables model switching without app changes
  - Critical for SIH technical defense and long-term viability

#### 20. **Presentation Clarity: From Feature List to Cohesive Solution**
- **Original Problem:** Too many "killer features" diluting message
- **Fix Implemented:** **Unified Narrative Around 5-Layer Architecture:**
  - **Layer 1 - Create:** Voice + Camera (Speak → Capture → AI understands)
  - **Layer 2 - Prepare:** AI Commerce Engine (Image + Catalog + Translation + Pricing)
  - **Layer 3 - Sell:** Market Access Engine (B2C + ONDC + B2B + Government)
  - **Layer 4 - Operate:** Business Manager (Inventory + Orders + Payments + Fulfilment)
  - **Layer 5 - Assist:** Voice / Didi / Offline (Artisan ↔ AI ↔ Assisted human)
  - **Core Story:** Before: Artisan has product but can't operate digital commerce. After: Artisan speaks naturally → creates professional listing → gets fair price → reaches buyers → manages business — with AI handling complexity.
---

## NEXT STEP: ARCHITECTURE

Having completed systematic debugging, stress testing, and incorporating critical feedback, we now have a battle-tested solution ready for architectural design.

The solution now addresses:
- **True Market Linkage** (not just digitization)
- **Realistic ONDC/GeM integration** (with proper roles and readiness stages)
- **Evidence-based user modeling** (instead of demographic assumptions)
- **Voice confirmation workflow** (AI proposes, artisan confirms)
- **Ethical image processing** (safe transformations only)
- **Transparent pricing advisory** (with cost floors and market ranges)
- **Realistic timelines** (60-second assisted creation, not complete listing)
- **Proper commerce data model** (Product → Variants → Inventory → Orders)
- **Elevated B2B capability** (bulk buyer matching as core subsystem)
- **Phased implementation** (MVP → ONDC → Voice Agent → B2B/Visual)
- **Complete order lifecycle** (including returns/refunds/support)
- **Trust & provenance built-in** (artisan verification, craft region, production story)
- **Radical UX simplification** (Artisan sees only: Sell, My Orders, My Money)
- **Didi Mode as core deployment mechanism**
- **Sustainable business model** (subsidized onboarding + transaction fees)
- **API abstraction layer** (to avoid vendor lock-in)
- **Clear 5-layer architecture narrative**

We are now ready to design the technical architecture that will support:
1. The Mobile App (Flutter) with adaptive UI
2. The AI Voice Agent (Phone Call Interface) for zero-smartphone users
3. The AI Commerce Engine (Catalog + Pricing + Image Processing)
4. The Market Access Engine (B2C + ONDC + B2B + Government channels)
5. The Business Manager (Inventory + Orders + Payments + Fulfilment)
6. The Assist Layer (Voice / Didi / Offline support systems)
7. The AI Gateway (pluggable provider abstraction)

_Teams that win SIH don't build 3 separate AIs — they build 1 flow where 3 AIs are invisible to user._
