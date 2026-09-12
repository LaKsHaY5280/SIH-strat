# WIREFRAME_26090.md
## Kala Setu — Complete Dual-Platform Wireframe & UX Specification

> **Status:** Rebuilt from the supplied wireframe after full structural review.
> **Surfaces:** Seller App for Artisan + Didi/CRP; Buyer Web for B2C + B2B.
> **Key rule:** Screen diagrams are static UI compositions. Flowcharts describe navigation. Sequence diagrams describe system interaction. State diagrams describe lifecycle transitions.

---

# 0. Full-File Review Findings

| Finding | Evidence in previous file | Correction in this version |
|---|---|---|
| Missing screen definitions | Inventory contained 62 screens but only a subset had dedicated screen sections | Every inventory ID below has a dedicated screen-composition diagram |
| Stale copied frame labels | Later sections reused frame names from earlier sections | Every diagram is generated from its own screen ID and title |
| Buyer and seller navigation mixed | Several Buyer screens showed Seller navigation | Buyer B2C and B2B navigation is isolated |
| Screen composition looked like a process chart | Header to body to actions arrows appeared inside static screens | Screen diagrams contain grouped regions with no transition arrows |
| Critical journey diagrams were placeholders | End-to-end sections reused an empty buyer screen | Critical journey diagrams now reference real screen IDs |
| B2B coverage was incomplete visually | Many inventory screens had no actual screen diagram | All 12 B2B inventory screens are represented |
| Missing states were disconnected from screens | Error, empty, confirmation and responsive sections stood alone | Shared states are mapped to the affected screen families |

## 0.1 Visual grammar
```mermaid
flowchart LR
  A0["Static screen"] -->|"no navigation arrows"| B0["grouped UI regions"]
  A1["Navigation"] -->|"flowchart"| B1["screen to screen"]
  A2["Backend interaction"] -->|"sequence diagram"| B2["client to service"]
  A3["Lifecycle"] -->|"state diagram"| B3["state to state"]
```

---

# 1. Platform Boundary

```mermaid
flowchart LR
  A0["Artisan"] --> B0["Seller App"]
  A1["Didi / CRP"] -->|"scoped assistance"| B1["Seller App"]
  A2["B2C Buyer"] --> B2["Buyer Web"]
  A3["B2B Buyer"] --> B3["Buyer Web"]
  A4["Seller App"] --> B4["Shared FastAPI + Domain + PostgreSQL"]
  A5["Buyer Web"] --> B5["Shared FastAPI + Domain + PostgreSQL"]
```

| Surface | Primary user | Navigation | Main outcome |
|---|---|---|---|
| Seller App | Artisan | Sell / My Orders / My Money | Independent digital selling and operations |
| Didi Mode | CRP / Didi | Home / Artisans / Sessions / Progress | Scoped assisted commerce |
| Buyer B2C | Consumer | Discover / Cart / Orders / Account | Product discovery and purchase |
| Buyer B2B | Business buyer | Workspace / Requirements / Matches / Quotes | Bulk sourcing and quotation |

---

# 2. Wireframe Rules

## 2.1 Screen diagrams
Every screen diagram below follows one of two composition templates. The regions are spatial groups, not process steps.

```mermaid
flowchart LR
  A0["Mobile frame"] --> B0["App bar + body + action area + bottom navigation"]
  A1["Web frame"] --> B1["Global header + main content + side rail or grid + footer"]
```

## 2.2 Seller rules
- One primary task per screen.
- One dominant primary action.
- Voice is contextual, not a replacement for visual confirmation.
- AI proposals are visually distinguishable from confirmed data.
- Offline and sync status never masquerade as live commerce state.
- Didi mode always exposes artisan identity and active assistance scope.

## 2.3 Buyer rules
- B2C stays optimized for discovery and purchase.
- B2B stays optimized for requirements, matching and quotations.
- Buyer UI never exposes seller-side concepts such as revision counters or assistance sessions.
- Inventory, reservation, order and payment state are domain-authoritative.

## 2.4 Baseline frames
| Surface | Baseline | Responsive variants |
|---|---|---|
| Seller App | 390 x 844 | 360 x 800, 412 x 915 |
| Buyer Web | 1440 x 1024 | 1280, 1024 tablet, 390 mobile |

---

# 3. Global Seller Shell

```mermaid
flowchart TB
  subgraph SCREEN["SHELL Seller Global Shell | 390 x 844"]
    direction TB
    APPBAR["HEADER | Seller Global Shell"]
    subgraph BODY["BODY"]
      direction TB
      B1["HEADER | Page title, audio affordance, status"]
      B2["BODY | Current task content"]
      B3["VOICE | Contextual Ask Kala Setu affordance"]
      B4["ACTION | One primary task action"]
    end
    subgraph ACTIONS["PRIMARY ACTION AREA"]
      direction LR
      PRIMARY["[ Primary action ]"]
      SECONDARY["[ Secondary action ]"]
    end
    NAV["NAV | Sell | My Orders | My Money"]
  end
  classDef app fill:#EDEDED,stroke:#333,stroke-width:2px;
  classDef body fill:#FFFFFF,stroke:#888,stroke-width:1px;
  classDef action fill:#F6F6F6,stroke:#555,stroke-width:2px;
  classDef nav fill:#E3E3E3,stroke:#444,stroke-width:1px;
  class APPBAR app
  class BODY body
  class ACTIONS action
  class NAV nav
```

```mermaid
flowchart LR
  A0["Home"] --> B0["Sell"]
  A1["Home"] --> B1["My Orders"]
  A2["Home"] --> B2["My Money"]
  A3["Any seller screen"] -->|"contextual"| B3["Voice Assistant"]
  A4["Any seller screen"] -->|"contextual"| B4["Settings"]
```

---

# 4. Seller App — Screen System

## 4.1 Coverage
```mermaid
flowchart TB
  A0["Onboarding"] --> B0["S01-S06"]
  A1["Seller core"] --> B1["S10 S20"]
  A2["Create and publish"] --> B2["S30-S39"]
  A3["Products"] --> B3["S40-S42"]
  A4["Orders"] --> B4["S50-S53"]
  A5["Money"] --> B5["S60-S61"]
  A6["Reliability"] --> B6["S70-S72"]
  A7["Didi"] --> B7["D01-D05"]
  A8["Settings"] --> B8["S80-S81"]
```

## S01 — Welcome
```mermaid
flowchart TB
  subgraph SCREEN["S01 Welcome | 390 x 844"]
    direction TB
    APPBAR["HEADER | Welcome"]
    subgraph BODY["BODY"]
      direction TB
      B1["STEP | Single onboarding decision"]
      B2["INPUT | Touch or voice"]
      B3["HELP | Audio explanation"]
      B4["PROGRESS | Minimal step indicator"]
    end
    subgraph ACTIONS["PRIMARY ACTION AREA"]
      direction LR
      PRIMARY["[ Shuru Karein ]"]
      SECONDARY["[ Sunayein ]"]
    end
    NAV["NAV | No primary nav | Back or Exit"]
  end
  classDef app fill:#EDEDED,stroke:#333,stroke-width:2px;
  classDef body fill:#FFFFFF,stroke:#888,stroke-width:1px;
  classDef action fill:#F6F6F6,stroke:#555,stroke-width:2px;
  classDef nav fill:#E3E3E3,stroke:#444,stroke-width:1px;
  class APPBAR app
  class BODY body
  class ACTIONS action
  class NAV nav
```

**Entry:** contextual seller navigation or prior screen. **Priority:** P0. **Primary API:** `See API contract for the supporting route group`. **Primary tables:** `See database coverage`.

## S02 — Language
```mermaid
flowchart TB
  subgraph SCREEN["S02 Language | 390 x 844"]
    direction TB
    APPBAR["HEADER | Language"]
    subgraph BODY["BODY"]
      direction TB
      B1["STEP | Single onboarding decision"]
      B2["INPUT | Touch or voice"]
      B3["HELP | Audio explanation"]
      B4["PROGRESS | Minimal step indicator"]
    end
    subgraph ACTIONS["PRIMARY ACTION AREA"]
      direction LR
      PRIMARY["[ Continue ]"]
      SECONDARY["[ Voice ]"]
    end
    NAV["NAV | No primary nav | Back or Exit"]
  end
  classDef app fill:#EDEDED,stroke:#333,stroke-width:2px;
  classDef body fill:#FFFFFF,stroke:#888,stroke-width:1px;
  classDef action fill:#F6F6F6,stroke:#555,stroke-width:2px;
  classDef nav fill:#E3E3E3,stroke:#444,stroke-width:1px;
  class APPBAR app
  class BODY body
  class ACTIONS action
  class NAV nav
```

**Entry:** contextual seller navigation or prior screen. **Priority:** P0. **Primary API:** `See API contract for the supporting route group`. **Primary tables:** `See database coverage`.

## S03 — Phone
```mermaid
flowchart TB
  subgraph SCREEN["S03 Phone | 390 x 844"]
    direction TB
    APPBAR["HEADER | Phone"]
    subgraph BODY["BODY"]
      direction TB
      B1["STEP | Single onboarding decision"]
      B2["INPUT | Touch or voice"]
      B3["HELP | Audio explanation"]
      B4["PROGRESS | Minimal step indicator"]
    end
    subgraph ACTIONS["PRIMARY ACTION AREA"]
      direction LR
      PRIMARY["[ Aage Badhein ]"]
      SECONDARY["[ Privacy ]"]
    end
    NAV["NAV | No primary nav | Back or Exit"]
  end
  classDef app fill:#EDEDED,stroke:#333,stroke-width:2px;
  classDef body fill:#FFFFFF,stroke:#888,stroke-width:1px;
  classDef action fill:#F6F6F6,stroke:#555,stroke-width:2px;
  classDef nav fill:#E3E3E3,stroke:#444,stroke-width:1px;
  class APPBAR app
  class BODY body
  class ACTIONS action
  class NAV nav
```

**Entry:** contextual seller navigation or prior screen. **Priority:** P0. **Primary API:** `See API contract for the supporting route group`. **Primary tables:** `See database coverage`.

## S04 — PIN
```mermaid
flowchart TB
  subgraph SCREEN["S04 PIN | 390 x 844"]
    direction TB
    APPBAR["HEADER | PIN"]
    subgraph BODY["BODY"]
      direction TB
      B1["STEP | Single onboarding decision"]
      B2["INPUT | Touch or voice"]
      B3["HELP | Audio explanation"]
      B4["PROGRESS | Minimal step indicator"]
    end
    subgraph ACTIONS["PRIMARY ACTION AREA"]
      direction LR
      PRIMARY["[ Confirm ]"]
      SECONDARY["[ Biometric later ]"]
    end
    NAV["NAV | No primary nav | Back or Exit"]
  end
  classDef app fill:#EDEDED,stroke:#333,stroke-width:2px;
  classDef body fill:#FFFFFF,stroke:#888,stroke-width:1px;
  classDef action fill:#F6F6F6,stroke:#555,stroke-width:2px;
  classDef nav fill:#E3E3E3,stroke:#444,stroke-width:1px;
  class APPBAR app
  class BODY body
  class ACTIONS action
  class NAV nav
```

**Entry:** contextual seller navigation or prior screen. **Priority:** P0. **Primary API:** `See API contract for the supporting route group`. **Primary tables:** `See database coverage`.

## S05 — Artisan Basics
```mermaid
flowchart TB
  subgraph SCREEN["S05 Artisan Basics | 390 x 844"]
    direction TB
    APPBAR["HEADER | Artisan Basics"]
    subgraph BODY["BODY"]
      direction TB
      B1["STEP | Single onboarding decision"]
      B2["INPUT | Touch or voice"]
      B3["HELP | Audio explanation"]
      B4["PROGRESS | Minimal step indicator"]
    end
    subgraph ACTIONS["PRIMARY ACTION AREA"]
      direction LR
      PRIMARY["[ Save and Continue ]"]
      SECONDARY["[ Voice ]"]
    end
    NAV["NAV | No primary nav | Back or Exit"]
  end
  classDef app fill:#EDEDED,stroke:#333,stroke-width:2px;
  classDef body fill:#FFFFFF,stroke:#888,stroke-width:1px;
  classDef action fill:#F6F6F6,stroke:#555,stroke-width:2px;
  classDef nav fill:#E3E3E3,stroke:#444,stroke-width:1px;
  class APPBAR app
  class BODY body
  class ACTIONS action
  class NAV nav
```

**Entry:** contextual seller navigation or prior screen. **Priority:** P0. **Primary API:** `See API contract for the supporting route group`. **Primary tables:** `See database coverage`.

## S06 — Ready
```mermaid
flowchart TB
  subgraph SCREEN["S06 Ready | 390 x 844"]
    direction TB
    APPBAR["HEADER | Ready"]
    subgraph BODY["BODY"]
      direction TB
      B1["STEP | Single onboarding decision"]
      B2["INPUT | Touch or voice"]
      B3["HELP | Audio explanation"]
      B4["PROGRESS | Minimal step indicator"]
    end
    subgraph ACTIONS["PRIMARY ACTION AREA"]
      direction LR
      PRIMARY["[ Bechein ]"]
      SECONDARY["[ Sunayein ]"]
    end
    NAV["NAV | No primary nav | Back or Exit"]
  end
  classDef app fill:#EDEDED,stroke:#333,stroke-width:2px;
  classDef body fill:#FFFFFF,stroke:#888,stroke-width:1px;
  classDef action fill:#F6F6F6,stroke:#555,stroke-width:2px;
  classDef nav fill:#E3E3E3,stroke:#444,stroke-width:1px;
  class APPBAR app
  class BODY body
  class ACTIONS action
  class NAV nav
```

**Entry:** contextual seller navigation or prior screen. **Priority:** P0. **Primary API:** `See API contract for the supporting route group`. **Primary tables:** `See database coverage`.

## S10 — Home
```mermaid
flowchart TB
  subgraph SCREEN["S10 Home | 390 x 844"]
    direction TB
    APPBAR["HEADER | Home"]
    subgraph BODY["BODY"]
      direction TB
      B1["PRIMARY | Sell now or resume a draft"]
      B2["ATTENTION | One highest priority banner only"]
      B3["VOICE | Ask Kala Setu"]
      B4["STATUS | Online or sync state"]
    end
    subgraph ACTIONS["PRIMARY ACTION AREA"]
      direction LR
      PRIMARY["[ Naya Product ]"]
      SECONDARY["[ Back ]"]
    end
    NAV["NAV | Sell | My Orders | My Money"]
  end
  classDef app fill:#EDEDED,stroke:#333,stroke-width:2px;
  classDef body fill:#FFFFFF,stroke:#888,stroke-width:1px;
  classDef action fill:#F6F6F6,stroke:#555,stroke-width:2px;
  classDef nav fill:#E3E3E3,stroke:#444,stroke-width:1px;
  class APPBAR app
  class BODY body
  class ACTIONS action
  class NAV nav
```

**Entry:** contextual seller navigation or prior screen. **Priority:** P0. **Primary API:** `See API contract for the supporting route group`. **Primary tables:** `See database coverage`.

## S20 — Voice Proposal
```mermaid
flowchart TB
  subgraph SCREEN["S20 Voice Proposal | 390 x 844"]
    direction TB
    APPBAR["HEADER | Voice Proposal"]
    subgraph BODY["BODY"]
      direction TB
      B1["PRIMARY CONTENT | Voice Proposal"]
      B2["CONTEXT | Relevant record and current state"]
      B3["DETAILS | Domain-backed information"]
      B4["ACTIONS | Only actions allowed in this state"]
    end
    subgraph ACTIONS["PRIMARY ACTION AREA"]
      direction LR
      PRIMARY["[ Haan ]"]
      SECONDARY["[ Badlein ]"]
    end
    NAV["NAV | Sell | My Orders | My Money"]
  end
  classDef app fill:#EDEDED,stroke:#333,stroke-width:2px;
  classDef body fill:#FFFFFF,stroke:#888,stroke-width:1px;
  classDef action fill:#F6F6F6,stroke:#555,stroke-width:2px;
  classDef nav fill:#E3E3E3,stroke:#444,stroke-width:1px;
  class APPBAR app
  class BODY body
  class ACTIONS action
  class NAV nav
```

**Entry:** contextual seller navigation or prior screen. **Priority:** P0. **Primary API:** `See API contract for the supporting route group`. **Primary tables:** `See database coverage`.

## S30 — Start Product
```mermaid
flowchart TB
  subgraph SCREEN["S30 Start Product | 390 x 844"]
    direction TB
    APPBAR["HEADER | Start Product"]
    subgraph BODY["BODY"]
      direction TB
      B1["ENTRY | Photo voice or draft"]
      B2["GUIDANCE | Simple next step"]
      B3["INPUT | Camera or microphone"]
      B4["DRAFT | Local save supported"]
    end
    subgraph ACTIONS["PRIMARY ACTION AREA"]
      direction LR
      PRIMARY["[ Photo ]"]
      SECONDARY["[ Drafts ]"]
    end
    NAV["NAV | Sell | My Orders | My Money"]
  end
  classDef app fill:#EDEDED,stroke:#333,stroke-width:2px;
  classDef body fill:#FFFFFF,stroke:#888,stroke-width:1px;
  classDef action fill:#F6F6F6,stroke:#555,stroke-width:2px;
  classDef nav fill:#E3E3E3,stroke:#444,stroke-width:1px;
  class APPBAR app
  class BODY body
  class ACTIONS action
  class NAV nav
```

**Entry:** contextual seller navigation or prior screen. **Priority:** P0. **Primary API:** `/v1/products`. **Primary tables:** `products`.

## S31 — Camera Guidance
```mermaid
flowchart TB
  subgraph SCREEN["S31 Camera Guidance | 390 x 844"]
    direction TB
    APPBAR["HEADER | Camera Guidance"]
    subgraph BODY["BODY"]
      direction TB
      B1["CAMERA | Live product framing"]
      B2["GUIDE | Lighting and framing hint"]
      B3["ORIGINAL | No mutation yet"]
      B4["ACTION | Capture"]
    end
    subgraph ACTIONS["PRIMARY ACTION AREA"]
      direction LR
      PRIMARY["[ Capture ]"]
      SECONDARY["[ Retake ]"]
    end
    NAV["NAV | Sell | My Orders | My Money"]
  end
  classDef app fill:#EDEDED,stroke:#333,stroke-width:2px;
  classDef body fill:#FFFFFF,stroke:#888,stroke-width:1px;
  classDef action fill:#F6F6F6,stroke:#555,stroke-width:2px;
  classDef nav fill:#E3E3E3,stroke:#444,stroke-width:1px;
  class APPBAR app
  class BODY body
  class ACTIONS action
  class NAV nav
```

**Entry:** contextual seller navigation or prior screen. **Priority:** P0. **Primary API:** `/v1/products/{product_id}/media/upload-init`. **Primary tables:** `product_media, media_processing_jobs`.

## S32 — Photo Review
```mermaid
flowchart TB
  subgraph SCREEN["S32 Photo Review | 390 x 844"]
    direction TB
    APPBAR["HEADER | Photo Review"]
    subgraph BODY["BODY"]
      direction TB
      B1["PHOTO | Original image"]
      B2["QUALITY | Basic quality feedback"]
      B3["TRUST | Original preserved"]
      B4["CHOICE | Retake or use"]
    end
    subgraph ACTIONS["PRIMARY ACTION AREA"]
      direction LR
      PRIMARY["[ Use Photo ]"]
      SECONDARY["[ Retake ]"]
    end
    NAV["NAV | Sell | My Orders | My Money"]
  end
  classDef app fill:#EDEDED,stroke:#333,stroke-width:2px;
  classDef body fill:#FFFFFF,stroke:#888,stroke-width:1px;
  classDef action fill:#F6F6F6,stroke:#555,stroke-width:2px;
  classDef nav fill:#E3E3E3,stroke:#444,stroke-width:1px;
  class APPBAR app
  class BODY body
  class ACTIONS action
  class NAV nav
```

**Entry:** contextual seller navigation or prior screen. **Priority:** P0. **Primary API:** `See API contract for the supporting route group`. **Primary tables:** `See database coverage`.

## S33 — Voice Description
```mermaid
flowchart TB
  subgraph SCREEN["S33 Voice Description | 390 x 844"]
    direction TB
    APPBAR["HEADER | Voice Description"]
    subgraph BODY["BODY"]
      direction TB
      B1["VOICE | Microphone state"]
      B2["PROMPT | What to describe"]
      B3["TRANSCRIPT | Optional live transcript"]
      B4["CONTROL | Redo or complete"]
    end
    subgraph ACTIONS["PRIMARY ACTION AREA"]
      direction LR
      PRIMARY["[ Ho Gaya ]"]
      SECONDARY["[ Dobara ]"]
    end
    NAV["NAV | Sell | My Orders | My Money"]
  end
  classDef app fill:#EDEDED,stroke:#333,stroke-width:2px;
  classDef body fill:#FFFFFF,stroke:#888,stroke-width:1px;
  classDef action fill:#F6F6F6,stroke:#555,stroke-width:2px;
  classDef nav fill:#E3E3E3,stroke:#444,stroke-width:1px;
  class APPBAR app
  class BODY body
  class ACTIONS action
  class NAV nav
```

**Entry:** contextual seller navigation or prior screen. **Priority:** P0. **Primary API:** `/v1/voice/interactions`. **Primary tables:** `voice_interactions`.

## S34 — Extracted Facts
```mermaid
flowchart TB
  subgraph SCREEN["S34 Extracted Facts | 390 x 844"]
    direction TB
    APPBAR["HEADER | Extracted Facts"]
    subgraph BODY["BODY"]
      direction TB
      B1["FACTS | Material craft size time"]
      B2["SOURCE | AI extracted"]
      B3["CONFIDENCE | Only when useful"]
      B4["EDIT | Field level correction"]
    end
    subgraph ACTIONS["PRIMARY ACTION AREA"]
      direction LR
      PRIMARY["[ Sahi ]"]
      SECONDARY["[ Badlein ]"]
    end
    NAV["NAV | Sell | My Orders | My Money"]
  end
  classDef app fill:#EDEDED,stroke:#333,stroke-width:2px;
  classDef body fill:#FFFFFF,stroke:#888,stroke-width:1px;
  classDef action fill:#F6F6F6,stroke:#555,stroke-width:2px;
  classDef nav fill:#E3E3E3,stroke:#444,stroke-width:1px;
  class APPBAR app
  class BODY body
  class ACTIONS action
  class NAV nav
```

**Entry:** contextual seller navigation or prior screen. **Priority:** P0. **Primary API:** `/v1/ai/decisions/{decision_id}`. **Primary tables:** `ai_decisions, product_field_sources`.

## S35 — Image Studio
```mermaid
flowchart TB
  subgraph SCREEN["S35 Image Studio | 390 x 844"]
    direction TB
    APPBAR["HEADER | Image Studio"]
    subgraph BODY["BODY"]
      direction TB
      B1["ORIGINAL | Original image"]
      B2["STORE | Enhanced image"]
      B3["CHANGES | Safe transformations"]
      B4["COMPARE | Original versus store"]
    end
    subgraph ACTIONS["PRIMARY ACTION AREA"]
      direction LR
      PRIMARY["[ Use Store Photo ]"]
      SECONDARY["[ Original Dekhein ]"]
    end
    NAV["NAV | Sell | My Orders | My Money"]
  end
  classDef app fill:#EDEDED,stroke:#333,stroke-width:2px;
  classDef body fill:#FFFFFF,stroke:#888,stroke-width:1px;
  classDef action fill:#F6F6F6,stroke:#555,stroke-width:2px;
  classDef nav fill:#E3E3E3,stroke:#444,stroke-width:1px;
  class APPBAR app
  class BODY body
  class ACTIONS action
  class NAV nav
```

**Entry:** contextual seller navigation or prior screen. **Priority:** P0. **Primary API:** `/v1/products/{product_id}/media/{media_id}/enhance`. **Primary tables:** `media_derivatives, media_quality_checks`.

## S36 — Catalog Draft
```mermaid
flowchart TB
  subgraph SCREEN["S36 Catalog Draft | 390 x 844"]
    direction TB
    APPBAR["HEADER | Catalog Draft"]
    subgraph BODY["BODY"]
      direction TB
      B1["TITLE | Hindi or source language"]
      B2["DESCRIPTION | Buyer facing copy"]
      B3["ENGLISH | Translated copy if enabled"]
      B4["TAGS | Category and search terms"]
    end
    subgraph ACTIONS["PRIMARY ACTION AREA"]
      direction LR
      PRIMARY["[ Confirm Catalog ]"]
      SECONDARY["[ Edit ]"]
    end
    NAV["NAV | Sell | My Orders | My Money"]
  end
  classDef app fill:#EDEDED,stroke:#333,stroke-width:2px;
  classDef body fill:#FFFFFF,stroke:#888,stroke-width:1px;
  classDef action fill:#F6F6F6,stroke:#555,stroke-width:2px;
  classDef nav fill:#E3E3E3,stroke:#444,stroke-width:1px;
  class APPBAR app
  class BODY body
  class ACTIONS action
  class NAV nav
```

**Entry:** contextual seller navigation or prior screen. **Priority:** P0. **Primary API:** `/v1/products/{product_id}/catalog/generate`. **Primary tables:** `catalog_entries, catalog_entry_versions`.

## S37 — Price Advisor
```mermaid
flowchart TB
  subgraph SCREEN["S37 Price Advisor | 390 x 844"]
    direction TB
    APPBAR["HEADER | Price Advisor"]
    subgraph BODY["BODY"]
      direction TB
      B1["COST FLOOR | Material labor packaging"]
      B2["MARKET BAND | Comparable range"]
      B3["SUGGESTED | Advisory price"]
      B4["EXPLANATION | Plain language reason"]
    end
    subgraph ACTIONS["PRIMARY ACTION AREA"]
      direction LR
      PRIMARY["[ Choose Price ]"]
      SECONDARY["[ Listen ]"]
    end
    NAV["NAV | Sell | My Orders | My Money"]
  end
  classDef app fill:#EDEDED,stroke:#333,stroke-width:2px;
  classDef body fill:#FFFFFF,stroke:#888,stroke-width:1px;
  classDef action fill:#F6F6F6,stroke:#555,stroke-width:2px;
  classDef nav fill:#E3E3E3,stroke:#444,stroke-width:1px;
  class APPBAR app
  class BODY body
  class ACTIONS action
  class NAV nav
```

**Entry:** contextual seller navigation or prior screen. **Priority:** P0. **Primary API:** `/v1/products/{product_id}/pricing/recommend`. **Primary tables:** `pricing_runs, pricing_factors, price_recommendations, market_comparables`.

## S38 — Final Confirmation
```mermaid
flowchart TB
  subgraph SCREEN["S38 Final Confirmation | 390 x 844"]
    direction TB
    APPBAR["HEADER | Final Confirmation"]
    subgraph BODY["BODY"]
      direction TB
      B1["PRODUCT | Title material dimensions"]
      B2["PRICE | Selected price"]
      B3["STOCK | Quantity"]
      B4["TRUST | Claims and origin"]
    end
    subgraph ACTIONS["PRIMARY ACTION AREA"]
      direction LR
      PRIMARY["[ Sab Sahi ]"]
      SECONDARY["[ Badlein ]"]
    end
    NAV["NAV | Sell | My Orders | My Money"]
  end
  classDef app fill:#EDEDED,stroke:#333,stroke-width:2px;
  classDef body fill:#FFFFFF,stroke:#888,stroke-width:1px;
  classDef action fill:#F6F6F6,stroke:#555,stroke-width:2px;
  classDef nav fill:#E3E3E3,stroke:#444,stroke-width:1px;
  class APPBAR app
  class BODY body
  class ACTIONS action
  class NAV nav
```

**Entry:** contextual seller navigation or prior screen. **Priority:** P0. **Primary API:** `/v1/voice/interactions/{interaction_id}/confirm`. **Primary tables:** `ai_decisions, catalog_entry_versions`.

## S39 — Publish Result
```mermaid
flowchart TB
  subgraph SCREEN["S39 Publish Result | 390 x 844"]
    direction TB
    APPBAR["HEADER | Publish Result"]
    subgraph BODY["BODY"]
      direction TB
      B1["RESULT | Published state"]
      B2["STOREFRONT | Public availability"]
      B3["INVENTORY | Available quantity"]
      B4["READINESS | Channel readiness stage"]
    end
    subgraph ACTIONS["PRIMARY ACTION AREA"]
      direction LR
      PRIMARY["[ Product Dekhein ]"]
      SECONDARY["[ Agla Product ]"]
    end
    NAV["NAV | Sell | My Orders | My Money"]
  end
  classDef app fill:#EDEDED,stroke:#333,stroke-width:2px;
  classDef body fill:#FFFFFF,stroke:#888,stroke-width:1px;
  classDef action fill:#F6F6F6,stroke:#555,stroke-width:2px;
  classDef nav fill:#E3E3E3,stroke:#444,stroke-width:1px;
  class APPBAR app
  class BODY body
  class ACTIONS action
  class NAV nav
```

**Entry:** contextual seller navigation or prior screen. **Priority:** P0. **Primary API:** `/v1/products/{product_id}/catalog/{catalog_entry_id}/publish`. **Primary tables:** `catalog_publications, market_readiness`.

## S40 — Product Detail
```mermaid
flowchart TB
  subgraph SCREEN["S40 Product Detail | 390 x 844"]
    direction TB
    APPBAR["HEADER | Product Detail"]
    subgraph BODY["BODY"]
      direction TB
      B1["PRODUCT | Title photo price stock"]
      B2["CATALOG | Published or draft content"]
      B3["EDIT | Allowed fields"]
      B4["REVISION | Pending changes or conflict if present"]
    end
    subgraph ACTIONS["PRIMARY ACTION AREA"]
      direction LR
      PRIMARY["[ Edit ]"]
      SECONDARY["[ Share ]"]
    end
    NAV["NAV | Sell | My Orders | My Money"]
  end
  classDef app fill:#EDEDED,stroke:#333,stroke-width:2px;
  classDef body fill:#FFFFFF,stroke:#888,stroke-width:1px;
  classDef action fill:#F6F6F6,stroke:#555,stroke-width:2px;
  classDef nav fill:#E3E3E3,stroke:#444,stroke-width:1px;
  class APPBAR app
  class BODY body
  class ACTIONS action
  class NAV nav
```

**Entry:** contextual seller navigation or prior screen. **Priority:** P0. **Primary API:** `See API contract for the supporting route group`. **Primary tables:** `See database coverage`.

## S41 — Product Edit
```mermaid
flowchart TB
  subgraph SCREEN["S41 Product Edit | 390 x 844"]
    direction TB
    APPBAR["HEADER | Product Edit"]
    subgraph BODY["BODY"]
      direction TB
      B1["PRODUCT | Title photo price stock"]
      B2["CATALOG | Published or draft content"]
      B3["EDIT | Allowed fields"]
      B4["REVISION | Pending changes or conflict if present"]
    end
    subgraph ACTIONS["PRIMARY ACTION AREA"]
      direction LR
      PRIMARY["[ Save Change ]"]
      SECONDARY["[ Cancel ]"]
    end
    NAV["NAV | Sell | My Orders | My Money"]
  end
  classDef app fill:#EDEDED,stroke:#333,stroke-width:2px;
  classDef body fill:#FFFFFF,stroke:#888,stroke-width:1px;
  classDef action fill:#F6F6F6,stroke:#555,stroke-width:2px;
  classDef nav fill:#E3E3E3,stroke:#444,stroke-width:1px;
  class APPBAR app
  class BODY body
  class ACTIONS action
  class NAV nav
```

**Entry:** contextual seller navigation or prior screen. **Priority:** P1. **Primary API:** `See API contract for the supporting route group`. **Primary tables:** `See database coverage`.

## S42 — Drafts
```mermaid
flowchart TB
  subgraph SCREEN["S42 Drafts | 390 x 844"]
    direction TB
    APPBAR["HEADER | Drafts"]
    subgraph BODY["BODY"]
      direction TB
      B1["PRODUCT | Title photo price stock"]
      B2["CATALOG | Published or draft content"]
      B3["EDIT | Allowed fields"]
      B4["REVISION | Pending changes or conflict if present"]
    end
    subgraph ACTIONS["PRIMARY ACTION AREA"]
      direction LR
      PRIMARY["[ Continue ]"]
      SECONDARY["[ Delete ]"]
    end
    NAV["NAV | Sell | My Orders | My Money"]
  end
  classDef app fill:#EDEDED,stroke:#333,stroke-width:2px;
  classDef body fill:#FFFFFF,stroke:#888,stroke-width:1px;
  classDef action fill:#F6F6F6,stroke:#555,stroke-width:2px;
  classDef nav fill:#E3E3E3,stroke:#444,stroke-width:1px;
  class APPBAR app
  class BODY body
  class ACTIONS action
  class NAV nav
```

**Entry:** contextual seller navigation or prior screen. **Priority:** P0. **Primary API:** `See API contract for the supporting route group`. **Primary tables:** `See database coverage`.

## S50 — Order Inbox
```mermaid
flowchart TB
  subgraph SCREEN["S50 Order Inbox | 390 x 844"]
    direction TB
    APPBAR["HEADER | Order Inbox"]
    subgraph BODY["BODY"]
      direction TB
      B1["ORDER | Order number item quantity amount"]
      B2["STATUS | Current lifecycle state"]
      B3["NEXT ACTION | One clear next step"]
      B4["HELP | Exception or support path"]
    end
    subgraph ACTIONS["PRIMARY ACTION AREA"]
      direction LR
      PRIMARY["[ Open Order ]"]
      SECONDARY["[ Filter ]"]
    end
    NAV["NAV | Sell | My Orders | My Money"]
  end
  classDef app fill:#EDEDED,stroke:#333,stroke-width:2px;
  classDef body fill:#FFFFFF,stroke:#888,stroke-width:1px;
  classDef action fill:#F6F6F6,stroke:#555,stroke-width:2px;
  classDef nav fill:#E3E3E3,stroke:#444,stroke-width:1px;
  class APPBAR app
  class BODY body
  class ACTIONS action
  class NAV nav
```

**Entry:** contextual seller navigation or prior screen. **Priority:** P0. **Primary API:** `/v1/orders?owner=me`. **Primary tables:** `orders, order_items`.

## S51 — Order Detail
```mermaid
flowchart TB
  subgraph SCREEN["S51 Order Detail | 390 x 844"]
    direction TB
    APPBAR["HEADER | Order Detail"]
    subgraph BODY["BODY"]
      direction TB
      B1["ORDER | Order number item quantity amount"]
      B2["STATUS | Current lifecycle state"]
      B3["NEXT ACTION | One clear next step"]
      B4["HELP | Exception or support path"]
    end
    subgraph ACTIONS["PRIMARY ACTION AREA"]
      direction LR
      PRIMARY["[ Confirm Order ]"]
      SECONDARY["[ Help ]"]
    end
    NAV["NAV | Sell | My Orders | My Money"]
  end
  classDef app fill:#EDEDED,stroke:#333,stroke-width:2px;
  classDef body fill:#FFFFFF,stroke:#888,stroke-width:1px;
  classDef action fill:#F6F6F6,stroke:#555,stroke-width:2px;
  classDef nav fill:#E3E3E3,stroke:#444,stroke-width:1px;
  class APPBAR app
  class BODY body
  class ACTIONS action
  class NAV nav
```

**Entry:** contextual seller navigation or prior screen. **Priority:** P0. **Primary API:** `/v1/orders/{order_id}`. **Primary tables:** `orders, order_items, order_addresses`.

## S52 — Fulfilment
```mermaid
flowchart TB
  subgraph SCREEN["S52 Fulfilment | 390 x 844"]
    direction TB
    APPBAR["HEADER | Fulfilment"]
    subgraph BODY["BODY"]
      direction TB
      B1["ORDER | Order number item quantity amount"]
      B2["STATUS | Current lifecycle state"]
      B3["NEXT ACTION | One clear next step"]
      B4["HELP | Exception or support path"]
    end
    subgraph ACTIONS["PRIMARY ACTION AREA"]
      direction LR
      PRIMARY["[ Shipment Ready ]"]
      SECONDARY["[ Listen ]"]
    end
    NAV["NAV | Sell | My Orders | My Money"]
  end
  classDef app fill:#EDEDED,stroke:#333,stroke-width:2px;
  classDef body fill:#FFFFFF,stroke:#888,stroke-width:1px;
  classDef action fill:#F6F6F6,stroke:#555,stroke-width:2px;
  classDef nav fill:#E3E3E3,stroke:#444,stroke-width:1px;
  class APPBAR app
  class BODY body
  class ACTIONS action
  class NAV nav
```

**Entry:** contextual seller navigation or prior screen. **Priority:** P0. **Primary API:** `/v1/orders/{order_id}/fulfillment/events`. **Primary tables:** `fulfillments, fulfilment_events`.

## S53 — Order Exception
```mermaid
flowchart TB
  subgraph SCREEN["S53 Order Exception | 390 x 844"]
    direction TB
    APPBAR["HEADER | Order Exception"]
    subgraph BODY["BODY"]
      direction TB
      B1["ORDER | Order number item quantity amount"]
      B2["STATUS | Current lifecycle state"]
      B3["NEXT ACTION | One clear next step"]
      B4["HELP | Exception or support path"]
    end
    subgraph ACTIONS["PRIMARY ACTION AREA"]
      direction LR
      PRIMARY["[ Choose Issue ]"]
      SECONDARY["[ Support ]"]
    end
    NAV["NAV | Sell | My Orders | My Money"]
  end
  classDef app fill:#EDEDED,stroke:#333,stroke-width:2px;
  classDef body fill:#FFFFFF,stroke:#888,stroke-width:1px;
  classDef action fill:#F6F6F6,stroke:#555,stroke-width:2px;
  classDef nav fill:#E3E3E3,stroke:#444,stroke-width:1px;
  class APPBAR app
  class BODY body
  class ACTIONS action
  class NAV nav
```

**Entry:** contextual seller navigation or prior screen. **Priority:** P1. **Primary API:** `/v1/orders/{order_id}/cancel or return-requests or refunds`. **Primary tables:** `orders, payment_records, fulfillments`.

## S60 — My Money
```mermaid
flowchart TB
  subgraph SCREEN["S60 My Money | 390 x 844"]
    direction TB
    APPBAR["HEADER | My Money"]
    subgraph BODY["BODY"]
      direction TB
      B1["MONEY | Gross net and current balance"]
      B2["DETAIL | Fees adjustments payout state"]
      B3["RECENT | Recent transactions"]
      B4["AUDIO | Read consequential amounts"]
    end
    subgraph ACTIONS["PRIMARY ACTION AREA"]
      direction LR
      PRIMARY["[ Detail Dekhein ]"]
      SECONDARY["[ Listen ]"]
    end
    NAV["NAV | Sell | My Orders | My Money"]
  end
  classDef app fill:#EDEDED,stroke:#333,stroke-width:2px;
  classDef body fill:#FFFFFF,stroke:#888,stroke-width:1px;
  classDef action fill:#F6F6F6,stroke:#555,stroke-width:2px;
  classDef nav fill:#E3E3E3,stroke:#444,stroke-width:1px;
  class APPBAR app
  class BODY body
  class ACTIONS action
  class NAV nav
```

**Entry:** contextual seller navigation or prior screen. **Priority:** P0. **Primary API:** `/v1/earnings/summary`. **Primary tables:** `earnings_ledger, payment_records`.

## S61 — Earnings Detail
```mermaid
flowchart TB
  subgraph SCREEN["S61 Earnings Detail | 390 x 844"]
    direction TB
    APPBAR["HEADER | Earnings Detail"]
    subgraph BODY["BODY"]
      direction TB
      B1["MONEY | Gross net and current balance"]
      B2["DETAIL | Fees adjustments payout state"]
      B3["RECENT | Recent transactions"]
      B4["AUDIO | Read consequential amounts"]
    end
    subgraph ACTIONS["PRIMARY ACTION AREA"]
      direction LR
      PRIMARY["[ View Order ]"]
      SECONDARY["[ Listen ]"]
    end
    NAV["NAV | Sell | My Orders | My Money"]
  end
  classDef app fill:#EDEDED,stroke:#333,stroke-width:2px;
  classDef body fill:#FFFFFF,stroke:#888,stroke-width:1px;
  classDef action fill:#F6F6F6,stroke:#555,stroke-width:2px;
  classDef nav fill:#E3E3E3,stroke:#444,stroke-width:1px;
  class APPBAR app
  class BODY body
  class ACTIONS action
  class NAV nav
```

**Entry:** contextual seller navigation or prior screen. **Priority:** P1. **Primary API:** `/v1/earnings/ledger`. **Primary tables:** `earnings_ledger, payout_records`.

## S70 — Offline State
```mermaid
flowchart TB
  subgraph SCREEN["S70 Offline State | 390 x 844"]
    direction TB
    APPBAR["HEADER | Offline State"]
    subgraph BODY["BODY"]
      direction TB
      B1["CONNECTIVITY | Online offline state"]
      B2["LOCAL | Saved on device"]
      B3["SERVER | Accepted server state"]
      B4["ACTION | Retry review or resolve"]
    end
    subgraph ACTIONS["PRIMARY ACTION AREA"]
      direction LR
      PRIMARY["[ Draft Dekhein ]"]
      SECONDARY["[ Retry ]"]
    end
    NAV["NAV | Sell | My Orders | My Money"]
  end
  classDef app fill:#EDEDED,stroke:#333,stroke-width:2px;
  classDef body fill:#FFFFFF,stroke:#888,stroke-width:1px;
  classDef action fill:#F6F6F6,stroke:#555,stroke-width:2px;
  classDef nav fill:#E3E3E3,stroke:#444,stroke-width:1px;
  class APPBAR app
  class BODY body
  class ACTIONS action
  class NAV nav
```

**Entry:** contextual seller navigation or prior screen. **Priority:** P0. **Primary API:** `/v1/sync/status`. **Primary tables:** `sync_queue_items, devices`.

## S71 — Sync Status
```mermaid
flowchart TB
  subgraph SCREEN["S71 Sync Status | 390 x 844"]
    direction TB
    APPBAR["HEADER | Sync Status"]
    subgraph BODY["BODY"]
      direction TB
      B1["CONNECTIVITY | Online offline state"]
      B2["LOCAL | Saved on device"]
      B3["SERVER | Accepted server state"]
      B4["ACTION | Retry review or resolve"]
    end
    subgraph ACTIONS["PRIMARY ACTION AREA"]
      direction LR
      PRIMARY["[ Review ]"]
      SECONDARY["[ Retry ]"]
    end
    NAV["NAV | Sell | My Orders | My Money"]
  end
  classDef app fill:#EDEDED,stroke:#333,stroke-width:2px;
  classDef body fill:#FFFFFF,stroke:#888,stroke-width:1px;
  classDef action fill:#F6F6F6,stroke:#555,stroke-width:2px;
  classDef nav fill:#E3E3E3,stroke:#444,stroke-width:1px;
  class APPBAR app
  class BODY body
  class ACTIONS action
  class NAV nav
```

**Entry:** contextual seller navigation or prior screen. **Priority:** P0. **Primary API:** `/v1/sync/status and /v1/sync/pull`. **Primary tables:** `sync_queue_items, sync_conflicts`.

## S72 — Conflict Review
```mermaid
flowchart TB
  subgraph SCREEN["S72 Conflict Review | 390 x 844"]
    direction TB
    APPBAR["HEADER | Conflict Review"]
    subgraph BODY["BODY"]
      direction TB
      B1["CONNECTIVITY | Online offline state"]
      B2["LOCAL | Saved on device"]
      B3["SERVER | Accepted server state"]
      B4["ACTION | Retry review or resolve"]
    end
    subgraph ACTIONS["PRIMARY ACTION AREA"]
      direction LR
      PRIMARY["[ Use Server ]"]
      SECONDARY["[ Compare ]"]
    end
    NAV["NAV | Sell | My Orders | My Money"]
  end
  classDef app fill:#EDEDED,stroke:#333,stroke-width:2px;
  classDef body fill:#FFFFFF,stroke:#888,stroke-width:1px;
  classDef action fill:#F6F6F6,stroke:#555,stroke-width:2px;
  classDef nav fill:#E3E3E3,stroke:#444,stroke-width:1px;
  class APPBAR app
  class BODY body
  class ACTIONS action
  class NAV nav
```

**Entry:** contextual seller navigation or prior screen. **Priority:** P0. **Primary API:** `/v1/conflicts/{conflict_id}/comparison and /resolve`. **Primary tables:** `sync_conflicts, sync_conflict_resolutions`.

## D01 — Didi Home
```mermaid
flowchart TB
  subgraph SCREEN["D01 Didi Home | 390 x 844"]
    direction TB
    APPBAR["HEADER | Didi Home"]
    subgraph BODY["BODY"]
      direction TB
      B1["ARTISAN | Who is being assisted"]
      B2["SCOPE | Current assistance scope"]
      B3["ACTIONS | Only permitted actions"]
      B4["AUDIT | Session and confirmation visibility"]
    end
    subgraph ACTIONS["PRIMARY ACTION AREA"]
      direction LR
      PRIMARY["[ Assist Artisan ]"]
      SECONDARY["[ Progress ]"]
    end
    NAV["NAV | Home | Artisans | Sessions | Progress"]
  end
  classDef app fill:#EDEDED,stroke:#333,stroke-width:2px;
  classDef body fill:#FFFFFF,stroke:#888,stroke-width:1px;
  classDef action fill:#F6F6F6,stroke:#555,stroke-width:2px;
  classDef nav fill:#E3E3E3,stroke:#444,stroke-width:1px;
  class APPBAR app
  class BODY body
  class ACTIONS action
  class NAV nav
```

**Entry:** contextual seller navigation or prior screen. **Priority:** P0. **Primary API:** `See API contract for the supporting route group`. **Primary tables:** `See database coverage`.

## D02 — Artisan List
```mermaid
flowchart TB
  subgraph SCREEN["D02 Artisan List | 390 x 844"]
    direction TB
    APPBAR["HEADER | Artisan List"]
    subgraph BODY["BODY"]
      direction TB
      B1["ARTISAN | Who is being assisted"]
      B2["SCOPE | Current assistance scope"]
      B3["ACTIONS | Only permitted actions"]
      B4["AUDIT | Session and confirmation visibility"]
    end
    subgraph ACTIONS["PRIMARY ACTION AREA"]
      direction LR
      PRIMARY["[ Open Artisan ]"]
      SECONDARY["[ Filter ]"]
    end
    NAV["NAV | Home | Artisans | Sessions | Progress"]
  end
  classDef app fill:#EDEDED,stroke:#333,stroke-width:2px;
  classDef body fill:#FFFFFF,stroke:#888,stroke-width:1px;
  classDef action fill:#F6F6F6,stroke:#555,stroke-width:2px;
  classDef nav fill:#E3E3E3,stroke:#444,stroke-width:1px;
  class APPBAR app
  class BODY body
  class ACTIONS action
  class NAV nav
```

**Entry:** contextual seller navigation or prior screen. **Priority:** P0. **Primary API:** `See API contract for the supporting route group`. **Primary tables:** `See database coverage`.

## D03 — Assistance Session Setup
```mermaid
flowchart TB
  subgraph SCREEN["D03 Assistance Session Setup | 390 x 844"]
    direction TB
    APPBAR["HEADER | Assistance Session Setup"]
    subgraph BODY["BODY"]
      direction TB
      B1["ARTISAN | Who is being assisted"]
      B2["SCOPE | Current assistance scope"]
      B3["ACTIONS | Only permitted actions"]
      B4["AUDIT | Session and confirmation visibility"]
    end
    subgraph ACTIONS["PRIMARY ACTION AREA"]
      direction LR
      PRIMARY["[ Session Shuru Karein ]"]
      SECONDARY["[ Cancel ]"]
    end
    NAV["NAV | Home | Artisans | Sessions | Progress"]
  end
  classDef app fill:#EDEDED,stroke:#333,stroke-width:2px;
  classDef body fill:#FFFFFF,stroke:#888,stroke-width:1px;
  classDef action fill:#F6F6F6,stroke:#555,stroke-width:2px;
  classDef nav fill:#E3E3E3,stroke:#444,stroke-width:1px;
  class APPBAR app
  class BODY body
  class ACTIONS action
  class NAV nav
```

**Entry:** contextual seller navigation or prior screen. **Priority:** P0. **Primary API:** `See API contract for the supporting route group`. **Primary tables:** `See database coverage`.

## D04 — Active Assistance Session
```mermaid
flowchart TB
  subgraph SCREEN["D04 Active Assistance Session | 390 x 844"]
    direction TB
    APPBAR["HEADER | Active Assistance Session"]
    subgraph BODY["BODY"]
      direction TB
      B1["ARTISAN | Who is being assisted"]
      B2["SCOPE | Current assistance scope"]
      B3["ACTIONS | Only permitted actions"]
      B4["AUDIT | Session and confirmation visibility"]
    end
    subgraph ACTIONS["PRIMARY ACTION AREA"]
      direction LR
      PRIMARY["[ Perform Action ]"]
      SECONDARY["[ End Session ]"]
    end
    NAV["NAV | Home | Artisans | Sessions | Progress"]
  end
  classDef app fill:#EDEDED,stroke:#333,stroke-width:2px;
  classDef body fill:#FFFFFF,stroke:#888,stroke-width:1px;
  classDef action fill:#F6F6F6,stroke:#555,stroke-width:2px;
  classDef nav fill:#E3E3E3,stroke:#444,stroke-width:1px;
  class APPBAR app
  class BODY body
  class ACTIONS action
  class NAV nav
```

**Entry:** contextual seller navigation or prior screen. **Priority:** P0. **Primary API:** `See API contract for the supporting route group`. **Primary tables:** `See database coverage`.

## D05 — Assistance History
```mermaid
flowchart TB
  subgraph SCREEN["D05 Assistance History | 390 x 844"]
    direction TB
    APPBAR["HEADER | Assistance History"]
    subgraph BODY["BODY"]
      direction TB
      B1["ARTISAN | Who is being assisted"]
      B2["SCOPE | Current assistance scope"]
      B3["ACTIONS | Only permitted actions"]
      B4["AUDIT | Session and confirmation visibility"]
    end
    subgraph ACTIONS["PRIMARY ACTION AREA"]
      direction LR
      PRIMARY["[ Open Session ]"]
      SECONDARY["[ Filter ]"]
    end
    NAV["NAV | Home | Artisans | Sessions | Progress"]
  end
  classDef app fill:#EDEDED,stroke:#333,stroke-width:2px;
  classDef body fill:#FFFFFF,stroke:#888,stroke-width:1px;
  classDef action fill:#F6F6F6,stroke:#555,stroke-width:2px;
  classDef nav fill:#E3E3E3,stroke:#444,stroke-width:1px;
  class APPBAR app
  class BODY body
  class ACTIONS action
  class NAV nav
```

**Entry:** contextual seller navigation or prior screen. **Priority:** P1. **Primary API:** `See API contract for the supporting route group`. **Primary tables:** `See database coverage`.

## S80 — Profile
```mermaid
flowchart TB
  subgraph SCREEN["S80 Profile | 390 x 844"]
    direction TB
    APPBAR["HEADER | Profile"]
    subgraph BODY["BODY"]
      direction TB
      B1["ACCOUNT | Identity and profile"]
      B2["SECURITY | Session and device state"]
      B3["PRIVACY | Export voice data deletion"]
      B4["VERIFICATION | Verification status"]
    end
    subgraph ACTIONS["PRIMARY ACTION AREA"]
      direction LR
      PRIMARY["[ Edit ]"]
      SECONDARY["[ Listen ]"]
    end
    NAV["NAV | Sell | My Orders | My Money"]
  end
  classDef app fill:#EDEDED,stroke:#333,stroke-width:2px;
  classDef body fill:#FFFFFF,stroke:#888,stroke-width:1px;
  classDef action fill:#F6F6F6,stroke:#555,stroke-width:2px;
  classDef nav fill:#E3E3E3,stroke:#444,stroke-width:1px;
  class APPBAR app
  class BODY body
  class ACTIONS action
  class NAV nav
```

**Entry:** contextual seller navigation or prior screen. **Priority:** P1. **Primary API:** `See API contract for the supporting route group`. **Primary tables:** `See database coverage`.

## S81 — Data & Privacy
```mermaid
flowchart TB
  subgraph SCREEN["S81 Data & Privacy | 390 x 844"]
    direction TB
    APPBAR["HEADER | Data & Privacy"]
    subgraph BODY["BODY"]
      direction TB
      B1["ACCOUNT | Identity and profile"]
      B2["SECURITY | Session and device state"]
      B3["PRIVACY | Export voice data deletion"]
      B4["VERIFICATION | Verification status"]
    end
    subgraph ACTIONS["PRIMARY ACTION AREA"]
      direction LR
      PRIMARY["[ Download My Data ]"]
      SECONDARY["[ Privacy Choices ]"]
    end
    NAV["NAV | Sell | My Orders | My Money"]
  end
  classDef app fill:#EDEDED,stroke:#333,stroke-width:2px;
  classDef body fill:#FFFFFF,stroke:#888,stroke-width:1px;
  classDef action fill:#F6F6F6,stroke:#555,stroke-width:2px;
  classDef nav fill:#E3E3E3,stroke:#444,stroke-width:1px;
  class APPBAR app
  class BODY body
  class ACTIONS action
  class NAV nav
```

**Entry:** contextual seller navigation or prior screen. **Priority:** P1. **Primary API:** `See API contract for the supporting route group`. **Primary tables:** `See database coverage`.

## 4.2 Seller onboarding journey
```mermaid
flowchart LR
  A0["S01 Welcome"] --> B0["S02 Language"]
  A1["S02 Language"] --> B1["S03 Phone"]
  A2["S03 Phone"] --> B2["S04 PIN"]
  A3["S04 PIN"] --> B3["S05 Artisan Basics"]
  A4["S05 Artisan Basics"] --> B4["S06 Ready"]
  A5["S06 Ready"] --> B5["S10 Home"]
```

## 4.3 Product creation journey
```mermaid
flowchart LR
  A0["S30 Start Product"] --> B0["S31 Camera"]
  A1["S31 Camera"] --> B1["S32 Photo Review"]
  A2["S32 Photo Review"] --> B2["S33 Voice Description"]
  A3["S33 Voice Description"] --> B3["S34 Extracted Facts"]
  A4["S34 Extracted Facts"] --> B4["S35 Image Studio"]
  A5["S35 Image Studio"] --> B5["S36 Catalog Draft"]
  A6["S36 Catalog Draft"] --> B6["S37 Price Advisor"]
  A7["S37 Price Advisor"] --> B7["S38 Final Confirmation"]
  A8["S38 Final Confirmation"] --> B8["S39 Publish Result"]
```

```mermaid
sequenceDiagram
  participant U as "Artisan"
  participant APP as "Seller App"
  participant API as "API"
  participant AI as "AI Gateway"
  participant DOM as "Domain"
  U->>APP: Speak product detail
  APP->>API: Create voice interaction
  API->>AI: Transcribe and extract
  AI->>API: Return proposal
  API->>APP: Show proposed state
  APP->>U: Request confirmation
  U->>APP: Confirm
  APP->>API: Commit confirmed action
  API->>DOM: Execute deterministic mutation
  DOM->>API: Return authoritative state
  API->>APP: Render updated screen
```

## 4.4 Product state machine
```mermaid
stateDiagram-v2
  [*] --> Draft
  Draft --> MediaCaptured: "capture media"
  MediaCaptured --> FactsProposed: "voice processed"
  FactsProposed --> CatalogProposed: "confirm facts"
  CatalogProposed --> PriceProposed: "generate price"
  PriceProposed --> ReadyToPublish: "confirm price"
  ReadyToPublish --> Published: "publish"
  FactsProposed --> Draft: "edit"
  CatalogProposed --> Draft: "edit"
  PriceProposed --> Draft: "edit"
```

## 4.5 Order lifecycle
```mermaid
stateDiagram-v2
  [*] --> New
  New --> Confirmed: "confirm"
  Confirmed --> Preparing: "start preparing"
  Preparing --> Shipped: "ship"
  Shipped --> Delivered: "deliver"
  Delivered --> Completed: "complete"
  New --> Cancelled: "cancel"
  Confirmed --> Cancelled: "cancel"
  Delivered --> ReturnRequested: "return"
  ReturnRequested --> Refunded: "refund"
```

## 4.6 Offline lifecycle
```mermaid
stateDiagram-v2
  [*] --> LocalDraft
  LocalDraft --> Queued: "save"
  Queued --> Syncing: "connection"
  Syncing --> Synced: "accepted"
  Syncing --> Conflict: "revision mismatch"
  Conflict --> Review: "open conflict"
  Review --> Resolved: "select resolution"
  Resolved --> Syncing: "retry"
  Syncing --> Failed: "retryable failure"
  Failed --> Queued: "retry later"
```

## 4.7 Didi session lifecycle
```mermaid
sequenceDiagram
  participant D as "Didi"
  participant APP as "Seller App"
  participant API as "API"
  participant DOM as "Domain"
  participant A as "Artisan"
  D->>APP: Open assigned artisan
  APP->>API: Start scoped session
  API->>DOM: Validate assignment and scope
  DOM->>API: Authorize session
  API->>APP: Show session scope
  D->>APP: Perform scoped action
  APP->>API: Submit action
  API->>A: Request confirmation when required
  A->>APP: Confirm
  APP->>API: Commit
  API->>DOM: Mutate authoritative state
  D->>APP: End session
  APP->>API: Close session
```

---

# 5. Buyer Web — Global Shell

```mermaid
flowchart TB
  subgraph SCREEN["WEB-SHELL Buyer Global Shell | Responsive Web"]
    direction TB
    APPBAR["HEADER | Buyer Global Shell"]
    subgraph BODY["BODY"]
      direction TB
      B1["HEADER | Logo, search, mode switch, cart, account"]
      B2["NAV | B2C discovery and B2B workspace"]
      B3["MAIN | Responsive content container"]
      B4["FOOTER | Policies, help, contact"]
    end
    subgraph ACTIONS["PRIMARY ACTION AREA"]
      direction LR
      PRIMARY["[ Context Action ]"]
      SECONDARY["[ Secondary Action ]"]
    end
    NAV["NAV | Home | B2C | B2B | Account"]
  end
  classDef app fill:#EDEDED,stroke:#333,stroke-width:2px;
  classDef body fill:#FFFFFF,stroke:#888,stroke-width:1px;
  classDef action fill:#F6F6F6,stroke:#555,stroke-width:2px;
  classDef nav fill:#E3E3E3,stroke:#444,stroke-width:1px;
  class APPBAR app
  class BODY body
  class ACTIONS action
  class NAV nav
```

```mermaid
flowchart LR
  A0["Public"] --> B0["B2C discovery"]
  A1["Product detail"] --> B1["Cart"]
  A2["Cart"] --> B2["Checkout"]
  A3["Checkout"] --> B3["Order tracking"]
  A4["B2B home"] --> B4["Requirement"]
  A5["Requirement"] --> B5["Matches"]
  A6["Matches"] --> B6["Quotation"]
  A7["Quotation"] --> B7["Converted order"]
```

---

# 6. Buyer Web — B2C

```mermaid
flowchart LR
  A0["B10"] --> B0["B20 Search"]
  A1["B20 Search"] --> B1["B21 Filters"]
  A2["B20 Search"] --> B2["B30 Product"]
  A3["B30 Product"] --> B3["B31 Story"]
  A4["B30 Product"] --> B4["B32 Artisan"]
  A5["B30 Product"] --> B5["B40 Cart"]
  A6["B40 Cart"] --> B6["B50 Checkout"]
  A7["B50 Checkout"] --> B7["B51 Address"]
  A8["B50 Checkout"] --> B8["B52 Confirmation"]
  A9["B52 Confirmation"] --> B9["B60 Tracking"]
  A10["B60 Tracking"] --> B10["B61 History"]
  A11["B10"] --> B11["B70 Account"]
```

## B10 — B2C Home
```mermaid
flowchart TB
  subgraph SCREEN["B10 B2C Home | Responsive Web"]
    direction TB
    APPBAR["HEADER | B2C Home"]
    subgraph BODY["BODY"]
      direction TB
      B1["DISCOVER | Featured crafts and categories"]
      B2["SEARCH | Persistent search"]
      B3["STORY | Craft or maker story entry"]
      B4["TRUST | Visible verification cue"]
    end
    subgraph ACTIONS["PRIMARY ACTION AREA"]
      direction LR
      PRIMARY["[ Explore Products ]"]
      SECONDARY["[ Browse Categories ]"]
    end
    NAV["NAV | Home | B2C | B2B | Account"]
  end
  classDef app fill:#EDEDED,stroke:#333,stroke-width:2px;
  classDef body fill:#FFFFFF,stroke:#888,stroke-width:1px;
  classDef action fill:#F6F6F6,stroke:#555,stroke-width:2px;
  classDef nav fill:#E3E3E3,stroke:#444,stroke-width:1px;
  class APPBAR app
  class BODY body
  class ACTIONS action
  class NAV nav
```

**Priority:** P0. **Primary API:** `/v1/discovery/products`. **Primary tables:** `catalog_entries, catalog_publications, products`.

## B20 — Search Results
```mermaid
flowchart TB
  subgraph SCREEN["B20 Search Results | Responsive Web"]
    direction TB
    APPBAR["HEADER | Search Results"]
    subgraph BODY["BODY"]
      direction TB
      B1["QUERY | Search term and result count"]
      B2["FILTERS | Craft region price material"]
      B3["GRID | Product cards"]
      B4["SORT | Relevance price newest"]
    end
    subgraph ACTIONS["PRIMARY ACTION AREA"]
      direction LR
      PRIMARY["[ Open Product ]"]
      SECONDARY["[ Filter ]"]
    end
    NAV["NAV | Home | B2C | B2B | Account"]
  end
  classDef app fill:#EDEDED,stroke:#333,stroke-width:2px;
  classDef body fill:#FFFFFF,stroke:#888,stroke-width:1px;
  classDef action fill:#F6F6F6,stroke:#555,stroke-width:2px;
  classDef nav fill:#E3E3E3,stroke:#444,stroke-width:1px;
  class APPBAR app
  class BODY body
  class ACTIONS action
  class NAV nav
```

**Priority:** P0. **Primary API:** `/v1/discovery/products`. **Primary tables:** `catalog_entries, products, inventory`.

## B21 — Filters
```mermaid
flowchart TB
  subgraph SCREEN["B21 Filters | Responsive Web"]
    direction TB
    APPBAR["HEADER | Filters"]
    subgraph BODY["BODY"]
      direction TB
      B1["CRAFT | Category selection"]
      B2["REGION | Craft geography"]
      B3["PRICE | Range"]
      B4["AVAILABILITY | In stock or made to order"]
    end
    subgraph ACTIONS["PRIMARY ACTION AREA"]
      direction LR
      PRIMARY["[ Apply Filters ]"]
      SECONDARY["[ Clear ]"]
    end
    NAV["NAV | Home | B2C | B2B | Account"]
  end
  classDef app fill:#EDEDED,stroke:#333,stroke-width:2px;
  classDef body fill:#FFFFFF,stroke:#888,stroke-width:1px;
  classDef action fill:#F6F6F6,stroke:#555,stroke-width:2px;
  classDef nav fill:#E3E3E3,stroke:#444,stroke-width:1px;
  class APPBAR app
  class BODY body
  class ACTIONS action
  class NAV nav
```

**Priority:** P0. **Primary API:** `/v1/discovery/products`. **Primary tables:** `catalog_entries, products`.

## B30 — Product Detail
```mermaid
flowchart TB
  subgraph SCREEN["B30 Product Detail | Responsive Web"]
    direction TB
    APPBAR["HEADER | Product Detail"]
    subgraph BODY["BODY"]
      direction TB
      B1["GALLERY | Original and store image"]
      B2["FACTS | Material size craft"]
      B3["TRUST | Artisan verification"]
      B4["PURCHASE | Price variant stock"]
    end
    subgraph ACTIONS["PRIMARY ACTION AREA"]
      direction LR
      PRIMARY["[ Add to Cart ]"]
      SECONDARY["[ Buy Now ]"]
    end
    NAV["NAV | Home | B2C | B2B | Account"]
  end
  classDef app fill:#EDEDED,stroke:#333,stroke-width:2px;
  classDef body fill:#FFFFFF,stroke:#888,stroke-width:1px;
  classDef action fill:#F6F6F6,stroke:#555,stroke-width:2px;
  classDef nav fill:#E3E3E3,stroke:#444,stroke-width:1px;
  class APPBAR app
  class BODY body
  class ACTIONS action
  class NAV nav
```

**Priority:** P0. **Primary API:** `/v1/public/products/{product_id} and /v1/carts/{cart_id}/items`. **Primary tables:** `products, product_variants, inventory`.

## B31 — Craft Story
```mermaid
flowchart TB
  subgraph SCREEN["B31 Craft Story | Responsive Web"]
    direction TB
    APPBAR["HEADER | Craft Story"]
    subgraph BODY["BODY"]
      direction TB
      B1["STORY | Maker narrative"]
      B2["AUDIO | Listen to story"]
      B3["CRAFT | Technique and region"]
      B4["MEDIA | Process media if available"]
    end
    subgraph ACTIONS["PRIMARY ACTION AREA"]
      direction LR
      PRIMARY["[ Listen ]"]
      SECONDARY["[ Back to Product ]"]
    end
    NAV["NAV | Home | B2C | B2B | Account"]
  end
  classDef app fill:#EDEDED,stroke:#333,stroke-width:2px;
  classDef body fill:#FFFFFF,stroke:#888,stroke-width:1px;
  classDef action fill:#F6F6F6,stroke:#555,stroke-width:2px;
  classDef nav fill:#E3E3E3,stroke:#444,stroke-width:1px;
  class APPBAR app
  class BODY body
  class ACTIONS action
  class NAV nav
```

**Priority:** P0. **Primary API:** `/v1/products/{product_id}/stories`. **Primary tables:** `production_stories, craft_provenance_records`.

## B32 — Artisan Profile
```mermaid
flowchart TB
  subgraph SCREEN["B32 Artisan Profile | Responsive Web"]
    direction TB
    APPBAR["HEADER | Artisan Profile"]
    subgraph BODY["BODY"]
      direction TB
      B1["ARTISAN | Name and location"]
      B2["VERIFY | Verification"]
      B3["CRAFT | Skills and craft"]
      B4["SHOP | Published products"]
    end
    subgraph ACTIONS["PRIMARY ACTION AREA"]
      direction LR
      PRIMARY["[ View Products ]"]
      SECONDARY["[ Share ]"]
    end
    NAV["NAV | Home | B2C | B2B | Account"]
  end
  classDef app fill:#EDEDED,stroke:#333,stroke-width:2px;
  classDef body fill:#FFFFFF,stroke:#888,stroke-width:1px;
  classDef action fill:#F6F6F6,stroke:#555,stroke-width:2px;
  classDef nav fill:#E3E3E3,stroke:#444,stroke-width:1px;
  class APPBAR app
  class BODY body
  class ACTIONS action
  class NAV nav
```

**Priority:** P0. **Primary API:** `/v1/public/artisans/{artisan_id}`. **Primary tables:** `artisan_profiles, craft_profiles, verifications`.

## B40 — Cart
```mermaid
flowchart TB
  subgraph SCREEN["B40 Cart | Responsive Web"]
    direction TB
    APPBAR["HEADER | Cart"]
    subgraph BODY["BODY"]
      direction TB
      B1["ITEMS | Cart lines and quantities"]
      B2["PRICE | Subtotal delivery total"]
      B3["REVALIDATION | Price and stock status"]
      B4["ACTION | Checkout"]
    end
    subgraph ACTIONS["PRIMARY ACTION AREA"]
      direction LR
      PRIMARY["[ Checkout ]"]
      SECONDARY["[ Continue Shopping ]"]
    end
    NAV["NAV | Home | B2C | B2B | Account"]
  end
  classDef app fill:#EDEDED,stroke:#333,stroke-width:2px;
  classDef body fill:#FFFFFF,stroke:#888,stroke-width:1px;
  classDef action fill:#F6F6F6,stroke:#555,stroke-width:2px;
  classDef nav fill:#E3E3E3,stroke:#444,stroke-width:1px;
  class APPBAR app
  class BODY body
  class ACTIONS action
  class NAV nav
```

**Priority:** P0. **Primary API:** `/v1/carts/current`. **Primary tables:** `buyer_sessions, carts, cart_items`.

## B50 — Checkout
```mermaid
flowchart TB
  subgraph SCREEN["B50 Checkout | Responsive Web"]
    direction TB
    APPBAR["HEADER | Checkout"]
    subgraph BODY["BODY"]
      direction TB
      B1["ADDRESS | Selected delivery address"]
      B2["REVIEW | Line items and totals"]
      B3["PAYMENT | Available method"]
      B4["CONFIRM | Place order"]
    end
    subgraph ACTIONS["PRIMARY ACTION AREA"]
      direction LR
      PRIMARY["[ Place Order ]"]
      SECONDARY["[ Edit ]"]
    end
    NAV["NAV | Home | B2C | B2B | Account"]
  end
  classDef app fill:#EDEDED,stroke:#333,stroke-width:2px;
  classDef body fill:#FFFFFF,stroke:#888,stroke-width:1px;
  classDef action fill:#F6F6F6,stroke:#555,stroke-width:2px;
  classDef nav fill:#E3E3E3,stroke:#444,stroke-width:1px;
  class APPBAR app
  class BODY body
  class ACTIONS action
  class NAV nav
```

**Priority:** P0. **Primary API:** `/v1/checkout/validate and /v1/checkout/orders`. **Primary tables:** `orders, order_items, order_addresses, payment_records, inventory_reservations`.

## B51 — Address
```mermaid
flowchart TB
  subgraph SCREEN["B51 Address | Responsive Web"]
    direction TB
    APPBAR["HEADER | Address"]
    subgraph BODY["BODY"]
      direction TB
      B1["SAVED | Saved addresses"]
      B2["FORM | New address fields"]
      B3["MASKING | Sensitive address display"]
      B4["SELECT | Use this address"]
    end
    subgraph ACTIONS["PRIMARY ACTION AREA"]
      direction LR
      PRIMARY["[ Use This Address ]"]
      SECONDARY["[ Add New ]"]
    end
    NAV["NAV | Home | B2C | B2B | Account"]
  end
  classDef app fill:#EDEDED,stroke:#333,stroke-width:2px;
  classDef body fill:#FFFFFF,stroke:#888,stroke-width:1px;
  classDef action fill:#F6F6F6,stroke:#555,stroke-width:2px;
  classDef nav fill:#E3E3E3,stroke:#444,stroke-width:1px;
  class APPBAR app
  class BODY body
  class ACTIONS action
  class NAV nav
```

**Priority:** P0. **Primary API:** `/v1/checkout/validate`. **Primary tables:** `order_addresses`.

## B52 — Order Confirmation
```mermaid
flowchart TB
  subgraph SCREEN["B52 Order Confirmation | Responsive Web"]
    direction TB
    APPBAR["HEADER | Order Confirmation"]
    subgraph BODY["BODY"]
      direction TB
      B1["CONFIRMATION | Order number"]
      B2["SUMMARY | Items amount destination"]
      B3["FULFILMENT | Expected next state"]
      B4["TRACK | Link to tracking"]
    end
    subgraph ACTIONS["PRIMARY ACTION AREA"]
      direction LR
      PRIMARY["[ Track Order ]"]
      SECONDARY["[ Continue Shopping ]"]
    end
    NAV["NAV | Home | B2C | B2B | Account"]
  end
  classDef app fill:#EDEDED,stroke:#333,stroke-width:2px;
  classDef body fill:#FFFFFF,stroke:#888,stroke-width:1px;
  classDef action fill:#F6F6F6,stroke:#555,stroke-width:2px;
  classDef nav fill:#E3E3E3,stroke:#444,stroke-width:1px;
  class APPBAR app
  class BODY body
  class ACTIONS action
  class NAV nav
```

**Priority:** P0. **Primary API:** `/v1/orders/{order_id}`. **Primary tables:** `orders, fulfillments, payment_records`.

## B60 — Order Tracking
```mermaid
flowchart TB
  subgraph SCREEN["B60 Order Tracking | Responsive Web"]
    direction TB
    APPBAR["HEADER | Order Tracking"]
    subgraph BODY["BODY"]
      direction TB
      B1["TIMELINE | Confirmed preparing shipped delivered"]
      B2["ITEM | Product summary"]
      B3["DELIVERY | Masked address and progress"]
      B4["HELP | Support path"]
    end
    subgraph ACTIONS["PRIMARY ACTION AREA"]
      direction LR
      PRIMARY["[ Need Help ]"]
      SECONDARY["[ View Details ]"]
    end
    NAV["NAV | Home | B2C | B2B | Account"]
  end
  classDef app fill:#EDEDED,stroke:#333,stroke-width:2px;
  classDef body fill:#FFFFFF,stroke:#888,stroke-width:1px;
  classDef action fill:#F6F6F6,stroke:#555,stroke-width:2px;
  classDef nav fill:#E3E3E3,stroke:#444,stroke-width:1px;
  class APPBAR app
  class BODY body
  class ACTIONS action
  class NAV nav
```

**Priority:** P0. **Primary API:** `/v1/orders/{order_id}/fulfillment`. **Primary tables:** `orders, fulfillments, fulfilment_events`.

## B61 — Order History
```mermaid
flowchart TB
  subgraph SCREEN["B61 Order History | Responsive Web"]
    direction TB
    APPBAR["HEADER | Order History"]
    subgraph BODY["BODY"]
      direction TB
      B1["ORDERS | Active and completed"]
      B2["FILTERS | Status or date"]
      B3["SUMMARY | Amount and seller"]
      B4["OPEN | Order detail"]
    end
    subgraph ACTIONS["PRIMARY ACTION AREA"]
      direction LR
      PRIMARY["[ Open Order ]"]
      SECONDARY["[ Filter ]"]
    end
    NAV["NAV | Home | B2C | B2B | Account"]
  end
  classDef app fill:#EDEDED,stroke:#333,stroke-width:2px;
  classDef body fill:#FFFFFF,stroke:#888,stroke-width:1px;
  classDef action fill:#F6F6F6,stroke:#555,stroke-width:2px;
  classDef nav fill:#E3E3E3,stroke:#444,stroke-width:1px;
  class APPBAR app
  class BODY body
  class ACTIONS action
  class NAV nav
```

**Priority:** P1. **Primary API:** `/v1/orders?mine=true`. **Primary tables:** `orders, fulfillments`.

## B70 — Account
```mermaid
flowchart TB
  subgraph SCREEN["B70 Account | Responsive Web"]
    direction TB
    APPBAR["HEADER | Account"]
    subgraph BODY["BODY"]
      direction TB
      B1["IDENTITY | Buyer identity"]
      B2["MODES | B2C and B2B access"]
      B3["ORDERS | Order history"]
      B4["SECURITY | Session controls"]
    end
    subgraph ACTIONS["PRIMARY ACTION AREA"]
      direction LR
      PRIMARY["[ Manage Account ]"]
      SECONDARY["[ Switch Mode ]"]
    end
    NAV["NAV | Home | B2C | B2B | Account"]
  end
  classDef app fill:#EDEDED,stroke:#333,stroke-width:2px;
  classDef body fill:#FFFFFF,stroke:#888,stroke-width:1px;
  classDef action fill:#F6F6F6,stroke:#555,stroke-width:2px;
  classDef nav fill:#E3E3E3,stroke:#444,stroke-width:1px;
  class APPBAR app
  class BODY body
  class ACTIONS action
  class NAV nav
```

**Priority:** P1. **Primary API:** `/v1/buyer/session and /v1/users/me`. **Primary tables:** `customers, buyer_sessions, users`.

## 6.1 B2C checkout transaction
```mermaid
sequenceDiagram
  participant B as "Buyer"
  participant WEB as "Buyer Web"
  participant API as "API"
  participant INV as "Inventory"
  participant ORD as "Order"
  B->>WEB: Open checkout
  WEB->>API: Validate cart
  API->>INV: Check price stock
  INV->>API: Return authoritative state
  API->>WEB: Show validated summary
  B->>WEB: Place order
  WEB->>API: Create checkout order
  API->>INV: Reserve stock transactionally
  API->>ORD: Create order
  ORD->>API: Return order
  API->>WEB: Confirmation
```

---

# 7. Buyer Web — B2B

```mermaid
flowchart LR
  A0["BB10"] --> B0["BB20 Step 1"]
  A1["BB20 Step 1"] --> B1["BB21 Quantity Budget"]
  A2["BB21 Quantity Budget"] --> B2["BB22 Deadline Delivery"]
  A3["BB22 Deadline Delivery"] --> B3["BB23 Review"]
  A4["BB23 Review"] --> B4["BB30 Matches"]
  A5["BB30 Matches"] --> B5["BB31 Match Detail"]
  A6["BB31 Match Detail"] --> B6["BB32 Quote Request"]
  A7["BB32 Quote Request"] --> B7["BB40 Quotation"]
  A8["BB40 Quotation"] --> B8["BB41 Compare"]
  A9["BB40 Quotation"] --> B9["BB42 Change Request"]
  A10["BB40 Quotation"] --> B10["BB50 Converted Order"]
```

## BB10 — B2B Home
```mermaid
flowchart TB
  subgraph SCREEN["BB10 B2B Home | Responsive Web"]
    direction TB
    APPBAR["HEADER | B2B Home"]
    subgraph BODY["BODY"]
      direction TB
      B1["OVERVIEW | Active requirements"]
      B2["MATCHES | New matches"]
      B3["QUOTES | Waiting responses"]
      B4["ORDERS | Converted orders"]
    end
    subgraph ACTIONS["PRIMARY ACTION AREA"]
      direction LR
      PRIMARY["[ New Requirement ]"]
      SECONDARY["[ Open Requirement ]"]
    end
    NAV["NAV | Home | B2C | B2B | Account"]
  end
  classDef app fill:#EDEDED,stroke:#333,stroke-width:2px;
  classDef body fill:#FFFFFF,stroke:#888,stroke-width:1px;
  classDef action fill:#F6F6F6,stroke:#555,stroke-width:2px;
  classDef nav fill:#E3E3E3,stroke:#444,stroke-width:1px;
  class APPBAR app
  class BODY body
  class ACTIONS action
  class NAV nav
```

**Priority:** P0. **Primary API:** `/v1/b2b/requirements?mine=true`. **Primary tables:** `buyer_requirements, quotations`.

## BB20 — Requirement Step 1
```mermaid
flowchart TB
  subgraph SCREEN["BB20 Requirement Step 1 | Responsive Web"]
    direction TB
    APPBAR["HEADER | Requirement Step 1"]
    subgraph BODY["BODY"]
      direction TB
      B1["PROGRESS | Requirement step"]
      B2["FORM | Only fields for current step"]
      B3["SUMMARY | Accumulated requirement"]
      B4["SAVE | Save draft"]
    end
    subgraph ACTIONS["PRIMARY ACTION AREA"]
      direction LR
      PRIMARY["[ Next ]"]
      SECONDARY["[ Save Draft ]"]
    end
    NAV["NAV | Home | B2C | B2B | Account"]
  end
  classDef app fill:#EDEDED,stroke:#333,stroke-width:2px;
  classDef body fill:#FFFFFF,stroke:#888,stroke-width:1px;
  classDef action fill:#F6F6F6,stroke:#555,stroke-width:2px;
  classDef nav fill:#E3E3E3,stroke:#444,stroke-width:1px;
  class APPBAR app
  class BODY body
  class ACTIONS action
  class NAV nav
```

**Priority:** P0. **Primary API:** `/v1/b2b/requirements`. **Primary tables:** `buyer_requirements, buyer_requirement_items`.

## BB21 — Requirement Step 2
```mermaid
flowchart TB
  subgraph SCREEN["BB21 Requirement Step 2 | Responsive Web"]
    direction TB
    APPBAR["HEADER | Requirement Step 2"]
    subgraph BODY["BODY"]
      direction TB
      B1["PROGRESS | Requirement step"]
      B2["FORM | Only fields for current step"]
      B3["SUMMARY | Accumulated requirement"]
      B4["SAVE | Save draft"]
    end
    subgraph ACTIONS["PRIMARY ACTION AREA"]
      direction LR
      PRIMARY["[ Next ]"]
      SECONDARY["[ Back ]"]
    end
    NAV["NAV | Home | B2C | B2B | Account"]
  end
  classDef app fill:#EDEDED,stroke:#333,stroke-width:2px;
  classDef body fill:#FFFFFF,stroke:#888,stroke-width:1px;
  classDef action fill:#F6F6F6,stroke:#555,stroke-width:2px;
  classDef nav fill:#E3E3E3,stroke:#444,stroke-width:1px;
  class APPBAR app
  class BODY body
  class ACTIONS action
  class NAV nav
```

**Priority:** P0. **Primary API:** `/v1/b2b/requirements/{id}`. **Primary tables:** `buyer_requirements, buyer_requirement_items`.

## BB22 — Requirement Step 3
```mermaid
flowchart TB
  subgraph SCREEN["BB22 Requirement Step 3 | Responsive Web"]
    direction TB
    APPBAR["HEADER | Requirement Step 3"]
    subgraph BODY["BODY"]
      direction TB
      B1["PROGRESS | Requirement step"]
      B2["FORM | Only fields for current step"]
      B3["SUMMARY | Accumulated requirement"]
      B4["SAVE | Save draft"]
    end
    subgraph ACTIONS["PRIMARY ACTION AREA"]
      direction LR
      PRIMARY["[ Review ]"]
      SECONDARY["[ Back ]"]
    end
    NAV["NAV | Home | B2C | B2B | Account"]
  end
  classDef app fill:#EDEDED,stroke:#333,stroke-width:2px;
  classDef body fill:#FFFFFF,stroke:#888,stroke-width:1px;
  classDef action fill:#F6F6F6,stroke:#555,stroke-width:2px;
  classDef nav fill:#E3E3E3,stroke:#444,stroke-width:1px;
  class APPBAR app
  class BODY body
  class ACTIONS action
  class NAV nav
```

**Priority:** P0. **Primary API:** `/v1/b2b/requirements/{id}`. **Primary tables:** `buyer_requirements`.

## BB23 — Requirement Review
```mermaid
flowchart TB
  subgraph SCREEN["BB23 Requirement Review | Responsive Web"]
    direction TB
    APPBAR["HEADER | Requirement Review"]
    subgraph BODY["BODY"]
      direction TB
      B1["PROGRESS | Requirement step"]
      B2["FORM | Only fields for current step"]
      B3["SUMMARY | Accumulated requirement"]
      B4["SAVE | Save draft"]
    end
    subgraph ACTIONS["PRIMARY ACTION AREA"]
      direction LR
      PRIMARY["[ Submit Requirement ]"]
      SECONDARY["[ Save Draft ]"]
    end
    NAV["NAV | Home | B2C | B2B | Account"]
  end
  classDef app fill:#EDEDED,stroke:#333,stroke-width:2px;
  classDef body fill:#FFFFFF,stroke:#888,stroke-width:1px;
  classDef action fill:#F6F6F6,stroke:#555,stroke-width:2px;
  classDef nav fill:#E3E3E3,stroke:#444,stroke-width:1px;
  class APPBAR app
  class BODY body
  class ACTIONS action
  class NAV nav
```

**Priority:** P0. **Primary API:** `/v1/b2b/requirements/{id}/publish`. **Primary tables:** `buyer_requirements, buyer_requirement_items`.

## BB30 — Match Results
```mermaid
flowchart TB
  subgraph SCREEN["BB30 Match Results | Responsive Web"]
    direction TB
    APPBAR["HEADER | Match Results"]
    subgraph BODY["BODY"]
      direction TB
      B1["REQUIREMENT | Pinned buyer requirement"]
      B2["RANKED | Match score and rank"]
      B3["FACTORS | Material quantity geography lead time"]
      B4["ACTION | Request quote"]
    end
    subgraph ACTIONS["PRIMARY ACTION AREA"]
      direction LR
      PRIMARY["[ Request Quote ]"]
      SECONDARY["[ Adjust Requirement ]"]
    end
    NAV["NAV | Home | B2C | B2B | Account"]
  end
  classDef app fill:#EDEDED,stroke:#333,stroke-width:2px;
  classDef body fill:#FFFFFF,stroke:#888,stroke-width:1px;
  classDef action fill:#F6F6F6,stroke:#555,stroke-width:2px;
  classDef nav fill:#E3E3E3,stroke:#444,stroke-width:1px;
  class APPBAR app
  class BODY body
  class ACTIONS action
  class NAV nav
```

**Priority:** P0. **Primary API:** `/v1/b2b/requirements/{id}/matches`. **Primary tables:** `market_opportunities, market_matches, market_match_factors`.

## BB31 — Match Detail
```mermaid
flowchart TB
  subgraph SCREEN["BB31 Match Detail | Responsive Web"]
    direction TB
    APPBAR["HEADER | Match Detail"]
    subgraph BODY["BODY"]
      direction TB
      B1["MATCH | Cluster or artisan context"]
      B2["CAPACITY | Available capacity"]
      B3["WHY | Human readable match factors"]
      B4["ACTION | Request quote"]
    end
    subgraph ACTIONS["PRIMARY ACTION AREA"]
      direction LR
      PRIMARY["[ Request Quote ]"]
      SECONDARY["[ Back to Matches ]"]
    end
    NAV["NAV | Home | B2C | B2B | Account"]
  end
  classDef app fill:#EDEDED,stroke:#333,stroke-width:2px;
  classDef body fill:#FFFFFF,stroke:#888,stroke-width:1px;
  classDef action fill:#F6F6F6,stroke:#555,stroke-width:2px;
  classDef nav fill:#E3E3E3,stroke:#444,stroke-width:1px;
  class APPBAR app
  class BODY body
  class ACTIONS action
  class NAV nav
```

**Priority:** P0. **Primary API:** `/v1/b2b/matches/{id}`. **Primary tables:** `market_matches, market_match_factors, cluster_capacity_snapshots`.

## BB32 — Quote Request
```mermaid
flowchart TB
  subgraph SCREEN["BB32 Quote Request | Responsive Web"]
    direction TB
    APPBAR["HEADER | Quote Request"]
    subgraph BODY["BODY"]
      direction TB
      B1["REQUEST | Selected match and quantity"]
      B2["TERMS | Required specifications"]
      B3["DEADLINE | Response deadline"]
      B4["SEND | Send request"]
    end
    subgraph ACTIONS["PRIMARY ACTION AREA"]
      direction LR
      PRIMARY["[ Send Request ]"]
      SECONDARY["[ Cancel ]"]
    end
    NAV["NAV | Home | B2C | B2B | Account"]
  end
  classDef app fill:#EDEDED,stroke:#333,stroke-width:2px;
  classDef body fill:#FFFFFF,stroke:#888,stroke-width:1px;
  classDef action fill:#F6F6F6,stroke:#555,stroke-width:2px;
  classDef nav fill:#E3E3E3,stroke:#444,stroke-width:1px;
  class APPBAR app
  class BODY body
  class ACTIONS action
  class NAV nav
```

**Priority:** P0. **Primary API:** `/v1/b2b/matches/{id}/quotation-requests`. **Primary tables:** `quotations, quotation_items`.

## BB40 — Quotation Detail
```mermaid
flowchart TB
  subgraph SCREEN["BB40 Quotation Detail | Responsive Web"]
    direction TB
    APPBAR["HEADER | Quotation Detail"]
    subgraph BODY["BODY"]
      direction TB
      B1["QUOTE | Seller or cluster terms"]
      B2["ITEMS | Quantity unit price total"]
      B3["DELIVERY | Lead time"]
      B4["QUALITY | Specifications attachments"]
    end
    subgraph ACTIONS["PRIMARY ACTION AREA"]
      direction LR
      PRIMARY["[ Accept Quote ]"]
      SECONDARY["[ Request Changes ]"]
    end
    NAV["NAV | Home | B2C | B2B | Account"]
  end
  classDef app fill:#EDEDED,stroke:#333,stroke-width:2px;
  classDef body fill:#FFFFFF,stroke:#888,stroke-width:1px;
  classDef action fill:#F6F6F6,stroke:#555,stroke-width:2px;
  classDef nav fill:#E3E3E3,stroke:#444,stroke-width:1px;
  class APPBAR app
  class BODY body
  class ACTIONS action
  class NAV nav
```

**Priority:** P0. **Primary API:** `/v1/b2b/quotations/{id}`. **Primary tables:** `quotations, quotation_items`.

## BB41 — Quote Comparison
```mermaid
flowchart TB
  subgraph SCREEN["BB41 Quote Comparison | Responsive Web"]
    direction TB
    APPBAR["HEADER | Quote Comparison"]
    subgraph BODY["BODY"]
      direction TB
      B1["COLUMNS | Comparable quotations"]
      B2["PRICE | Unit and total price"]
      B3["CAPACITY | Capacity and lead time"]
      B4["QUALITY | Requirement fit"]
    end
    subgraph ACTIONS["PRIMARY ACTION AREA"]
      direction LR
      PRIMARY["[ Select Quote ]"]
      SECONDARY["[ Back ]"]
    end
    NAV["NAV | Home | B2C | B2B | Account"]
  end
  classDef app fill:#EDEDED,stroke:#333,stroke-width:2px;
  classDef body fill:#FFFFFF,stroke:#888,stroke-width:1px;
  classDef action fill:#F6F6F6,stroke:#555,stroke-width:2px;
  classDef nav fill:#E3E3E3,stroke:#444,stroke-width:1px;
  class APPBAR app
  class BODY body
  class ACTIONS action
  class NAV nav
```

**Priority:** P1. **Primary API:** `/v1/b2b/requirements/{id}/quotations`. **Primary tables:** `quotations, quotation_items, market_matches`.

## BB42 — Change Request
```mermaid
flowchart TB
  subgraph SCREEN["BB42 Change Request | Responsive Web"]
    direction TB
    APPBAR["HEADER | Change Request"]
    subgraph BODY["BODY"]
      direction TB
      B1["QUOTE | Current quotation"]
      B2["CHANGE | Structured requested changes"]
      B3["REASON | Why change is needed"]
      B4["SUBMIT | Send request"]
    end
    subgraph ACTIONS["PRIMARY ACTION AREA"]
      direction LR
      PRIMARY["[ Send Change Request ]"]
      SECONDARY["[ Cancel ]"]
    end
    NAV["NAV | Home | B2C | B2B | Account"]
  end
  classDef app fill:#EDEDED,stroke:#333,stroke-width:2px;
  classDef body fill:#FFFFFF,stroke:#888,stroke-width:1px;
  classDef action fill:#F6F6F6,stroke:#555,stroke-width:2px;
  classDef nav fill:#E3E3E3,stroke:#444,stroke-width:1px;
  class APPBAR app
  class BODY body
  class ACTIONS action
  class NAV nav
```

**Priority:** P1. **Primary API:** `/v1/b2b/quotations/{id}/reject`. **Primary tables:** `quotations, quotation_items`.

## BB50 — Converted Order
```mermaid
flowchart TB
  subgraph SCREEN["BB50 Converted Order | Responsive Web"]
    direction TB
    APPBAR["HEADER | Converted Order"]
    subgraph BODY["BODY"]
      direction TB
      B1["ORDER | Converted order"]
      B2["SOURCE | Accepted quotation"]
      B3["FULFILMENT | Next steps"]
      B4["TRACK | Order status"]
    end
    subgraph ACTIONS["PRIMARY ACTION AREA"]
      direction LR
      PRIMARY["[ View Order ]"]
      SECONDARY["[ B2B Home ]"]
    end
    NAV["NAV | Home | B2C | B2B | Account"]
  end
  classDef app fill:#EDEDED,stroke:#333,stroke-width:2px;
  classDef body fill:#FFFFFF,stroke:#888,stroke-width:1px;
  classDef action fill:#F6F6F6,stroke:#555,stroke-width:2px;
  classDef nav fill:#E3E3E3,stroke:#444,stroke-width:1px;
  class APPBAR app
  class BODY body
  class ACTIONS action
  class NAV nav
```

**Priority:** P1. **Primary API:** `/v1/orders/{id}`. **Primary tables:** `orders, order_items, fulfillments`.

## 7.1 B2B quotation state
```mermaid
stateDiagram-v2
  [*] --> Draft
  Draft --> Submitted: "seller submits"
  Submitted --> Viewed: "buyer opens"
  Viewed --> ChangesRequested: "buyer asks for changes"
  Viewed --> Accepted: "buyer accepts"
  ChangesRequested --> Resubmitted: "seller revises"
  Resubmitted --> Viewed: "buyer reviews"
  Accepted --> ConvertedToOrder: "deterministic conversion"
  Submitted --> Expired: "deadline reached"
```

---

# 8. Shared Visual States

## 8.1 Seller error
```mermaid
flowchart TB
  subgraph SCREEN["S-ERROR Seller Error State | 390 x 844"]
    direction TB
    APPBAR["HEADER | Seller Error State"]
    subgraph BODY["BODY"]
      direction TB
      B1["MESSAGE | What happened in plain language"]
      B2["SAFETY | What is safe and what is not committed"]
      B3["RECOVERY | Retry, save later or contact support"]
      B4["STATUS | Connectivity or server state"]
    end
    subgraph ACTIONS["PRIMARY ACTION AREA"]
      direction LR
      PRIMARY["[ Try Again ]"]
      SECONDARY["[ Save for Later ]"]
    end
    NAV["NAV | Sell | My Orders | My Money"]
  end
  classDef app fill:#EDEDED,stroke:#333,stroke-width:2px;
  classDef body fill:#FFFFFF,stroke:#888,stroke-width:1px;
  classDef action fill:#F6F6F6,stroke:#555,stroke-width:2px;
  classDef nav fill:#E3E3E3,stroke:#444,stroke-width:1px;
  class APPBAR app
  class BODY body
  class ACTIONS action
  class NAV nav
```

## 8.2 Buyer error
```mermaid
flowchart TB
  subgraph SCREEN["B-ERROR Buyer Error State | Responsive Web"]
    direction TB
    APPBAR["HEADER | Buyer Error State"]
    subgraph BODY["BODY"]
      direction TB
      B1["MESSAGE | What happened"]
      B2["PERSISTENCE | What user work was preserved"]
      B3["RECOVERY | Retry or continue shopping"]
      B4["HELP | Support path if needed"]
    end
    subgraph ACTIONS["PRIMARY ACTION AREA"]
      direction LR
      PRIMARY["[ Try Again ]"]
      SECONDARY["[ Continue Shopping ]"]
    end
    NAV["NAV | Home | B2C | B2B | Account"]
  end
  classDef app fill:#EDEDED,stroke:#333,stroke-width:2px;
  classDef body fill:#FFFFFF,stroke:#888,stroke-width:1px;
  classDef action fill:#F6F6F6,stroke:#555,stroke-width:2px;
  classDef nav fill:#E3E3E3,stroke:#444,stroke-width:1px;
  class APPBAR app
  class BODY body
  class ACTIONS action
  class NAV nav
```

## 8.3 Seller consequential confirmation
```mermaid
flowchart TB
  subgraph SCREEN["S-CONF Consequential Confirmation | 390 x 844"]
    direction TB
    APPBAR["HEADER | Consequential Confirmation"]
    subgraph BODY["BODY"]
      direction TB
      B1["BEFORE | Current value"]
      B2["PROPOSED | New value"]
      B3["IMPACT | What this changes"]
      B4["SOURCE | AI proposal or user action"]
    end
    subgraph ACTIONS["PRIMARY ACTION AREA"]
      direction LR
      PRIMARY["[ Confirm ]"]
      SECONDARY["[ Cancel ]"]
    end
    NAV["NAV | Sell | My Orders | My Money"]
  end
  classDef app fill:#EDEDED,stroke:#333,stroke-width:2px;
  classDef body fill:#FFFFFF,stroke:#888,stroke-width:1px;
  classDef action fill:#F6F6F6,stroke:#555,stroke-width:2px;
  classDef nav fill:#E3E3E3,stroke:#444,stroke-width:1px;
  class APPBAR app
  class BODY body
  class ACTIONS action
  class NAV nav
```

## 8.4 Buyer checkout revalidation
```mermaid
flowchart TB
  subgraph SCREEN["B-CONF Checkout Revalidation | Responsive Web"]
    direction TB
    APPBAR["HEADER | Checkout Revalidation"]
    subgraph BODY["BODY"]
      direction TB
      B1["CHANGE | Price stock or delivery changed"]
      B2["AFFECTED LINE | Exact item and value"]
      B3["TOTAL | Updated total"]
      B4["NEXT | Return to cart or continue if allowed"]
    end
    subgraph ACTIONS["PRIMARY ACTION AREA"]
      direction LR
      PRIMARY["[ Review Cart ]"]
      SECONDARY["[ Continue ]"]
    end
    NAV["NAV | Home | B2C | B2B | Account"]
  end
  classDef app fill:#EDEDED,stroke:#333,stroke-width:2px;
  classDef body fill:#FFFFFF,stroke:#888,stroke-width:1px;
  classDef action fill:#F6F6F6,stroke:#555,stroke-width:2px;
  classDef nav fill:#E3E3E3,stroke:#444,stroke-width:1px;
  class APPBAR app
  class BODY body
  class ACTIONS action
  class NAV nav
```

## 8.5 Empty states
```mermaid
flowchart TB
  subgraph SCREEN["S-EMPTY Seller Empty Products | 390 x 844"]
    direction TB
    APPBAR["HEADER | Seller Empty Products"]
    subgraph BODY["BODY"]
      direction TB
      B1["MESSAGE | No products yet"]
      B2["NEXT | Create first product"]
      B3["HELP | Audio explanation"]
    end
    subgraph ACTIONS["PRIMARY ACTION AREA"]
      direction LR
      PRIMARY["[ Create Product ]"]
      SECONDARY["[ Listen ]"]
    end
    NAV["NAV | Sell | My Orders | My Money"]
  end
  classDef app fill:#EDEDED,stroke:#333,stroke-width:2px;
  classDef body fill:#FFFFFF,stroke:#888,stroke-width:1px;
  classDef action fill:#F6F6F6,stroke:#555,stroke-width:2px;
  classDef nav fill:#E3E3E3,stroke:#444,stroke-width:1px;
  class APPBAR app
  class BODY body
  class ACTIONS action
  class NAV nav
```

```mermaid
flowchart TB
  subgraph SCREEN["B-EMPTY Buyer Empty Search | Responsive Web"]
    direction TB
    APPBAR["HEADER | Buyer Empty Search"]
    subgraph BODY["BODY"]
      direction TB
      B1["QUERY | Keep query and filter context"]
      B2["MESSAGE | No matching products"]
      B3["SUGGESTIONS | Related craft, region, categories"]
      B4["RECOVERY | Clear or edit filters"]
    end
    subgraph ACTIONS["PRIMARY ACTION AREA"]
      direction LR
      PRIMARY["[ Clear Filters ]"]
      SECONDARY["[ Browse Categories ]"]
    end
    NAV["NAV | Home | B2C | B2B | Account"]
  end
  classDef app fill:#EDEDED,stroke:#333,stroke-width:2px;
  classDef body fill:#FFFFFF,stroke:#888,stroke-width:1px;
  classDef action fill:#F6F6F6,stroke:#555,stroke-width:2px;
  classDef nav fill:#E3E3E3,stroke:#444,stroke-width:1px;
  class APPBAR app
  class BODY body
  class ACTIONS action
  class NAV nav
```

---

# 9. AI and Trust Presentation

```mermaid
flowchart LR
  A0["User input"] --> B0["AI extraction"]
  A1["AI extraction"] --> B1["AI proposal"]
  A2["AI proposal"] --> B2["Explicit confirmation"]
  A3["Explicit confirmation"] --> B3["Domain mutation"]
  A4["Domain mutation"] --> B4["Confirmed UI state"]
```

| Information state | Screen treatment |
|---|---|
| User-provided | Normal editable field |
| AI-extracted | AI understood indicator plus edit |
| AI-generated | Suggested or generated badge |
| Confirmed | Normal authoritative state |
| Verified | Verification indicator and evidence context |

---

# 10. Responsive Buyer Web

```mermaid
flowchart TB
  A0["Desktop 1440"] --> B0["12-column grid"]
  A1["Tablet 1024"] --> B1["8-column grid"]
  A2["Mobile 390"] --> B2["4-column grid"]
  A3["All"] --> B3["Shared component system"]
  A4["All"] --> B4["Same API and domain"]
```

| Area | Desktop | Tablet | Mobile |
|---|---|---|---|
| Header | Search plus full nav | Compact nav | Search plus menu |
| Product grid | 4 columns | 3 columns | 2 columns |
| Product detail | Gallery plus info | Split or stacked | Stacked |
| B2B comparison | Full comparison table | Scrollable table | Stacked cards |
| Checkout | Two-column | One or two column | Single column |

---

# 11. Attention and Notification

```mermaid
flowchart TB
  A0["Domain event"] --> B0["Critical action"]
  A1["Domain event"] --> B1["Important update"]
  A2["Domain event"] --> B2["Informational update"]
  A3["Critical"] --> B3["Blocking in-app attention"]
  A4["Important"] --> B4["Contextual banner"]
  A5["Informational"] --> B5["History or notification center"]
```

---

# 12. Navigation and State Rules

## 12.1 Seller
```mermaid
flowchart TB
  A0["Home"] --> B0["Sell"]
  A1["Home"] --> B1["My Orders"]
  A2["Home"] --> B2["My Money"]
  A3["Sell"] --> B3["Product creation"]
  A4["My Orders"] --> B4["Order detail"]
  A5["My Money"] --> B5["Earnings detail"]
  A6["Contextual"] --> B6["Voice assistant"]
```

## 12.2 Buyer
```mermaid
flowchart TB
  A0["Home"] --> B0["Discover"]
  A1["Discover"] --> B1["Search"]
  A2["Search"] --> B2["Product detail"]
  A3["Product detail"] --> B3["Cart"]
  A4["Cart"] --> B4["Checkout"]
  A5["Checkout"] --> B5["Order confirmation"]
  A6["B2B Workspace"] --> B6["Requirement"]
  A7["Requirement"] --> B7["Matches"]
  A8["Matches"] --> B8["Quotation"]
  A9["Quotation"] --> B9["Converted order"]
```

---

# 13. Screen-to-API Traceability

| Screen | Primary API capability |
|---|---|
| S01 Welcome | `See API contract for this screen` |
| S02 Language | `See API contract for this screen` |
| S03 Phone | `See API contract for this screen` |
| S04 PIN | `See API contract for this screen` |
| S05 Artisan Basics | `See API contract for this screen` |
| S06 Ready | `See API contract for this screen` |
| S10 Home | `See API contract for this screen` |
| S20 Voice Proposal | `See API contract for this screen` |
| S30 Start Product | `/v1/products` |
| S31 Camera Guidance | `/v1/products/{product_id}/media/upload-init` |
| S32 Photo Review | `See API contract for this screen` |
| S33 Voice Description | `/v1/voice/interactions` |
| S34 Extracted Facts | `/v1/ai/decisions/{decision_id}` |
| S35 Image Studio | `/v1/products/{product_id}/media/{media_id}/enhance` |
| S36 Catalog Draft | `/v1/products/{product_id}/catalog/generate` |
| S37 Price Advisor | `/v1/products/{product_id}/pricing/recommend` |
| S38 Final Confirmation | `/v1/voice/interactions/{interaction_id}/confirm` |
| S39 Publish Result | `/v1/products/{product_id}/catalog/{catalog_entry_id}/publish` |
| S40 Product Detail | `See API contract for this screen` |
| S41 Product Edit | `See API contract for this screen` |
| S42 Drafts | `See API contract for this screen` |
| S50 Order Inbox | `/v1/orders?owner=me` |
| S51 Order Detail | `/v1/orders/{order_id}` |
| S52 Fulfilment | `/v1/orders/{order_id}/fulfillment/events` |
| S53 Order Exception | `/v1/orders/{order_id}/cancel or return-requests or refunds` |
| S60 My Money | `/v1/earnings/summary` |
| S61 Earnings Detail | `/v1/earnings/ledger` |
| S70 Offline State | `/v1/sync/status` |
| S71 Sync Status | `/v1/sync/status and /v1/sync/pull` |
| S72 Conflict Review | `/v1/conflicts/{conflict_id}/comparison and /resolve` |
| D01 Didi Home | `See API contract for this screen` |
| D02 Artisan List | `See API contract for this screen` |
| D03 Assistance Session Setup | `See API contract for this screen` |
| D04 Active Assistance Session | `See API contract for this screen` |
| D05 Assistance History | `See API contract for this screen` |
| S80 Profile | `See API contract for this screen` |
| S81 Data & Privacy | `See API contract for this screen` |
| B10 B2C Home | `/v1/discovery/products` |
| B20 Search Results | `/v1/discovery/products` |
| B21 Filters | `/v1/discovery/products` |
| B30 Product Detail | `/v1/public/products/{product_id} and /v1/carts/{cart_id}/items` |
| B31 Craft Story | `/v1/products/{product_id}/stories` |
| B32 Artisan Profile | `/v1/public/artisans/{artisan_id}` |
| B40 Cart | `/v1/carts/current` |
| B50 Checkout | `/v1/checkout/validate and /v1/checkout/orders` |
| B51 Address | `/v1/checkout/validate` |
| B52 Order Confirmation | `/v1/orders/{order_id}` |
| B60 Order Tracking | `/v1/orders/{order_id}/fulfillment` |
| B61 Order History | `/v1/orders?mine=true` |
| B70 Account | `/v1/buyer/session and /v1/users/me` |
| BB10 B2B Home | `/v1/b2b/requirements?mine=true` |
| BB20 Requirement Step 1 | `/v1/b2b/requirements` |
| BB21 Requirement Step 2 | `/v1/b2b/requirements/{id}` |
| BB22 Requirement Step 3 | `/v1/b2b/requirements/{id}` |
| BB23 Requirement Review | `/v1/b2b/requirements/{id}/publish` |
| BB30 Match Results | `/v1/b2b/requirements/{id}/matches` |
| BB31 Match Detail | `/v1/b2b/matches/{id}` |
| BB32 Quote Request | `/v1/b2b/matches/{id}/quotation-requests` |
| BB40 Quotation Detail | `/v1/b2b/quotations/{id}` |
| BB41 Quote Comparison | `/v1/b2b/requirements/{id}/quotations` |
| BB42 Change Request | `/v1/b2b/quotations/{id}/reject` |
| BB50 Converted Order | `/v1/orders/{id}` |

---

# 14. Screen-to-Database Traceability

| Screen | Primary database coverage |
|---|---|
| S01 Welcome | `See database contract` |
| S02 Language | `See database contract` |
| S03 Phone | `See database contract` |
| S04 PIN | `See database contract` |
| S05 Artisan Basics | `See database contract` |
| S06 Ready | `See database contract` |
| S10 Home | `See database contract` |
| S20 Voice Proposal | `See database contract` |
| S30 Start Product | `products` |
| S31 Camera Guidance | `product_media, media_processing_jobs` |
| S32 Photo Review | `See database contract` |
| S33 Voice Description | `voice_interactions` |
| S34 Extracted Facts | `ai_decisions, product_field_sources` |
| S35 Image Studio | `media_derivatives, media_quality_checks` |
| S36 Catalog Draft | `catalog_entries, catalog_entry_versions` |
| S37 Price Advisor | `pricing_runs, pricing_factors, price_recommendations, market_comparables` |
| S38 Final Confirmation | `ai_decisions, catalog_entry_versions` |
| S39 Publish Result | `catalog_publications, market_readiness` |
| S40 Product Detail | `See database contract` |
| S41 Product Edit | `See database contract` |
| S42 Drafts | `See database contract` |
| S50 Order Inbox | `orders, order_items` |
| S51 Order Detail | `orders, order_items, order_addresses` |
| S52 Fulfilment | `fulfillments, fulfilment_events` |
| S53 Order Exception | `orders, payment_records, fulfillments` |
| S60 My Money | `earnings_ledger, payment_records` |
| S61 Earnings Detail | `earnings_ledger, payout_records` |
| S70 Offline State | `sync_queue_items, devices` |
| S71 Sync Status | `sync_queue_items, sync_conflicts` |
| S72 Conflict Review | `sync_conflicts, sync_conflict_resolutions` |
| D01 Didi Home | `See database contract` |
| D02 Artisan List | `See database contract` |
| D03 Assistance Session Setup | `See database contract` |
| D04 Active Assistance Session | `See database contract` |
| D05 Assistance History | `See database contract` |
| S80 Profile | `See database contract` |
| S81 Data & Privacy | `See database contract` |
| B10 B2C Home | `catalog_entries, catalog_publications, products` |
| B20 Search Results | `catalog_entries, products, inventory` |
| B21 Filters | `catalog_entries, products` |
| B30 Product Detail | `products, product_variants, inventory` |
| B31 Craft Story | `production_stories, craft_provenance_records` |
| B32 Artisan Profile | `artisan_profiles, craft_profiles, verifications` |
| B40 Cart | `buyer_sessions, carts, cart_items` |
| B50 Checkout | `orders, order_items, order_addresses, payment_records, inventory_reservations` |
| B51 Address | `order_addresses` |
| B52 Order Confirmation | `orders, fulfillments, payment_records` |
| B60 Order Tracking | `orders, fulfillments, fulfilment_events` |
| B61 Order History | `orders, fulfillments` |
| B70 Account | `customers, buyer_sessions, users` |
| BB10 B2B Home | `buyer_requirements, quotations` |
| BB20 Requirement Step 1 | `buyer_requirements, buyer_requirement_items` |
| BB21 Requirement Step 2 | `buyer_requirements, buyer_requirement_items` |
| BB22 Requirement Step 3 | `buyer_requirements` |
| BB23 Requirement Review | `buyer_requirements, buyer_requirement_items` |
| BB30 Match Results | `market_opportunities, market_matches, market_match_factors` |
| BB31 Match Detail | `market_matches, market_match_factors, cluster_capacity_snapshots` |
| BB32 Quote Request | `quotations, quotation_items` |
| BB40 Quotation Detail | `quotations, quotation_items` |
| BB41 Quote Comparison | `quotations, quotation_items, market_matches` |
| BB42 Change Request | `quotations, quotation_items` |
| BB50 Converted Order | `orders, order_items, fulfillments` |

```mermaid
flowchart LR
  A0["Seller UI"] --> B0["Seller domain tables"]
  A1["Didi UI"] --> B1["Assistance and audit tables"]
  A2["B2C UI"] --> B2["Catalog cart checkout order tables"]
  A3["B2B UI"] --> B3["Requirement match quote tables"]
  A4["All mutations"] --> B4["Audit and authorization boundary"]
```

---

# 15. Screen Inventory

## 15.1 Seller App
| ID | Screen | Priority |
|---|---|---|
| S01 | Welcome | P0 |
| S02 | Language | P0 |
| S03 | Phone | P0 |
| S04 | PIN | P0 |
| S05 | Artisan Basics | P0 |
| S06 | Ready | P0 |
| S10 | Home | P0 |
| S20 | Voice Proposal | P0 |
| S30 | Start Product | P0 |
| S31 | Camera Guidance | P0 |
| S32 | Photo Review | P0 |
| S33 | Voice Description | P0 |
| S34 | Extracted Facts | P0 |
| S35 | Image Studio | P0 |
| S36 | Catalog Draft | P0 |
| S37 | Price Advisor | P0 |
| S38 | Final Confirmation | P0 |
| S39 | Publish Result | P0 |
| S40 | Product Detail | P0 |
| S41 | Product Edit | P1 |
| S42 | Drafts | P0 |
| S50 | Order Inbox | P0 |
| S51 | Order Detail | P0 |
| S52 | Fulfilment | P0 |
| S53 | Order Exception | P1 |
| S60 | My Money | P0 |
| S61 | Earnings Detail | P1 |
| S70 | Offline State | P0 |
| S71 | Sync Status | P0 |
| S72 | Conflict Review | P0 |
| D01 | Didi Home | P0 |
| D02 | Artisan List | P0 |
| D03 | Assistance Session Setup | P0 |
| D04 | Active Assistance Session | P0 |
| D05 | Assistance History | P1 |
| S80 | Profile | P1 |
| S81 | Data & Privacy | P1 |

## 15.2 Buyer Web B2C
| ID | Screen | Priority |
|---|---|---|
| B10 | B2C Home | P0 |
| B20 | Search Results | P0 |
| B21 | Filters | P0 |
| B30 | Product Detail | P0 |
| B31 | Craft Story | P0 |
| B32 | Artisan Profile | P0 |
| B40 | Cart | P0 |
| B50 | Checkout | P0 |
| B51 | Address | P0 |
| B52 | Order Confirmation | P0 |
| B60 | Order Tracking | P0 |
| B61 | Order History | P1 |
| B70 | Account | P1 |

## 15.3 Buyer Web B2B
| ID | Screen | Priority |
|---|---|---|
| BB10 | B2B Home | P0 |
| BB20 | Requirement Step 1 | P0 |
| BB21 | Requirement Step 2 | P0 |
| BB22 | Requirement Step 3 | P0 |
| BB23 | Requirement Review | P0 |
| BB30 | Match Results | P0 |
| BB31 | Match Detail | P0 |
| BB32 | Quote Request | P0 |
| BB40 | Quotation Detail | P0 |
| BB41 | Quote Comparison | P1 |
| BB42 | Change Request | P1 |
| BB50 | Converted Order | P1 |

---

# 16. Figma Page Structure

```mermaid
flowchart TB
  A0["00 Rules"] --> B0["01 Seller Foundations"]
  A1["01 Seller Foundations"] --> B1["02 Seller Onboarding"]
  A2["02 Seller Onboarding"] --> B2["03 Seller Core"]
  A3["03 Seller Core"] --> B3["04 Seller Orders Money"]
  A4["04 Seller Orders Money"] --> B4["05 Seller Didi States"]
  A5["05 Seller Didi States"] --> B5["06 Buyer B2C"]
  A6["06 Buyer B2C"] --> B6["07 Buyer B2B"]
  A7["07 Buyer B2B"] --> B7["08 Shared States"]
  A8["08 Shared States"] --> B8["09 Responsive"]
  A9["09 Responsive"] --> B9["10 Components"]
  A10["10 Components"] --> B10["11 Prototype Flows"]
```

Every Figma frame must retain the screen ID from this document.

---

# 17. P0 End-to-End Coverage

```mermaid
flowchart LR
  A0["S01-S06"] -->|"seller onboarding"| B0["S10 Home"]
  A1["S10 Home"] -->|"create and publish"| B1["S30-S39"]
  A2["S39 Publish"] -->|"buyer discovery"| B2["B10-B30"]
  A3["B30"] -->|"B2C purchase"| B3["B40-B52"]
  A4["B52"] -->|"seller fulfilment"| B4["S50-S52"]
  A5["D01-D04"] -->|"assisted creation"| B5["S30-S39"]
  A6["BB10"] -->|"B2B requirement"| B6["BB20-BB23"]
  A7["BB23"] -->|"matching and quote request"| B7["BB30-BB32"]
  A8["BB40"] -->|"quotation conversion"| B8["BB50"]
```

---

# 18. Definition of Done

## Seller App
- All 37 Seller/Didi screen IDs have a dedicated screen composition.
- P0 screens have defined loading, empty, error and confirmation behavior where applicable.
- S30-S39 is fully wired as the primary product-creation journey.
- S50-S53 covers the seller order lifecycle and exceptions.
- S70-S72 covers offline, sync and conflict states.
- D03-D04 visually communicates assistance scope and confirmation boundaries.

## Buyer Web
- All 13 B2C and 12 B2B screen IDs have a dedicated screen composition.
- B10-B52 covers B2C discovery through order confirmation.
- BB10-BB50 covers requirement through quotation conversion.
- Desktop, tablet and mobile responsive behavior is specified.

## Cross-platform
- No screen creates its own product, inventory, order or quotation truth.
- Screen IDs map to API capabilities and database coverage.
- AI proposals are visually distinct from confirmed state.
- Consequential mutations require explicit confirmation.
- Buyer Web never exposes seller-side operational complexity.

---

# 19. Final Wireframe Principle

> **See the screen first, then see the flow. Kala Setu wireframes are the visual contract for what each user sees, what each action means, and how the two platforms move through one shared deterministic commerce system.**
