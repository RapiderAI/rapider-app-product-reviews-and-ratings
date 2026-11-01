<!-- Rapider Official App SOW & Specification | App #128: Product Reviews & Ratings (product-reviews-and-ratings) | Domain: COM -->

# #128 Product Reviews & Ratings (`product-reviews-and-ratings`) — Scope of Work (SOW)

- **App Number & Name:** `#128` — Product Reviews & Ratings
- **Proposed App Key:** `product-reviews-and-ratings`
- **Suite / Domain Key & Model Prefix:** `COM` (`Com*`)
- **Function Category:** 13. E-Commerce & Retail
- **Representative Product (`P1`):** Yotpo Reviews
- **Combined Feature Catalog:** [../combined-features/128-product-reviews-and-ratings-features.md](../combined-features/128-product-reviews-and-ratings-features.md)
- **Target Release Posture:** Verified Post-Delivery Reviews, Structured Fit/Attribute Sliders, Visual UGC, Community Q&A, FTC-Compliant Moderation & Search/Retail Syndication Microapplication

---

## 1. Executive Outcome & Target Audience

### 1.1 Business Problem Solved
E-commerce shoppers abandon high-consideration purchases when Product Detail Pages (PDPs) lack authentic buyer social proof, sizing/fit clarity, real-world customer photos, or timely answers to pre-purchase questions—while merchants face up to `\$53,088` per violation under the FTC Consumer Review Rule (16 CFR Part 465) if moderation tools suppress negative reviews or hide incentivized material connections. **Rapider Product Reviews & Ratings (`product-reviews-and-ratings`)** anchors verified review collection to physical parcel delivery (`#127`) and POS purchases (`pos`), captures structured fit sliders and photo/video UGC, enforces statutory FTC non-suppression guardrails, powers Community Q&A and grounded AI PDP summaries, and syndicates GTIN-matched star ratings to Google Shopping and retail marketplaces.

### 1.2 Target Personas & Progressive Disclosure
- **SME Fast-Path Defaults (DTC Brand Founders, Store Managers):** Enable automated review invitations 7 days after confirmed delivery (`com.shipment.delivered`), publish clean verified-buyer reviews automatically with profanity/PII screening, display star histograms + `Schema.org` rich snippets on `website`/`ecommerce` PDPs, and reply publicly from one inbox.
- **Enterprise Progressive Disclosure (VP E-Commerce, Brand Compliance Counsel, Quality/PLM Teams):** Configure category-specific centering sliders (`Runs Small <-> True to Size <-> Runs Large`), seeded sampling disclosures, multi-locale GTIN review sharing across `ComSite`s, Google Merchant Center XML & retail syndication feeds, star-neutral `#129` loyalty rewards, automatic low-star `SupTicket` service recovery, and SKU defect drift alerts.

### 1.3 Measurable Business Outcomes
- **18%–32% Lift in PDP Conversion Rate:** Surfacing verified buyer reviews, customer photo/video UGC galleries, fit-slider consensus, and balanced Pros/Cons AI summaries directly resolves buyer hesitation.
- **Zero Premature "In-Transit" Review Invites:** Anchoring `ComReviewRequest` triggers to `#127` `DELIVERED` events eliminates 1-star reviews caused by asking customers to rate unreceived parcels.
- **100% FTC 16 CFR Part 465 Audit Readiness:** Cryptographic provenance badging (`VERIFIED_BUYER`, `INCENTIVIZED_REWARD`, `SEEDED_FREE_SAMPLE`), star-neutral reward enforcement, and immutable statutory quarantine reason codes prevent regulatory exposure.

### 1.4 Benchmark Summary (`P1`–`P6`)
Synthesizes the Smart Prompts, AI Review Summaries, visual UGC, and Google Shopping syndication of **`P1` Yotpo Reviews**, enterprise fraud screening, Ask-an-Owner Q&A, and sampling rigor of **`P2` Bazaarvoice**, fit/attribute centering sliders and FTC non-suppression workflows of **`P3` Okendo**, cryptographic buyer verification and merchant/product separation of **`P4` Trustpilot**, 40-language automated PII/profanity moderation and omnichannel BI of **`P5` Microsoft Dynamics 365 Commerce**, and native storefront/support recovery integration of **`P6` Salesforce Commerce Cloud & Odoo eCommerce**.

---

## 2. In-Scope, Out-of-Scope & Suite Ownership Boundaries

### 2.1 In-Scope Capabilities
1. Delivery-anchored and POS-triggered review invitation scheduling (`ComReviewRequest`) with single-use cryptographic tokens, multi-item progressive submission forms, and return/cancellation auto-suppression.
2. Category-specific structured attribute & centering-slider templates (`ComReviewAttributeTemplate`) plus EXIF-scrubbed customer photo/video UGC management (`ComReviewMedia`).
3. FTC-compliant moderation workbench (`ComProductReview`) with automated PII/profanity/bot screening, immutable statutory rejection reason codes, provenance badges, and public merchant replies.
4. Storefront Community Q&A (`ComProductQuestion`) with instant FAQ search, brand expert answers, and optional past-buyer community routing.
5. Parent-product and variant-level rating/fit rollup engine (`ComProductRatingSummary`) with grounded Pros/Cons AI summaries, `Schema.org` JSON-LD output, and Google Shopping / retail XML syndication feeds (`ComReviewSyndicationFeed`).
6. Low-rating (`<= 2 stars`) `SupTicket` service recovery handoff and star-neutral `#129` loyalty point reward triggers.

### 2.2 Out-of-Scope Capabilities (Delegated to Adjacent Rapider Apps)
- **Master product/variant catalog, GTIN maintenance, and base merchandising copy:** Owned by `productcatalog` (`ComProduct`, `ComVariant`).
- **Storefront page builder and checkout cart execution:** Owned by `website` and `ecommerce` (`ComSite`, `ComPage`, `ComCart`), which embed `#128` widgets/APIs.
- **Email and SMS transport delivery infrastructure, sender domains, and TCPA opt-in ledgers:** Owned by `email` and `sms` (`MktEmailMessage`, `MktSmsMessage`).
- **Loyalty point ledger balances, VIP tiers, and reward coupon redemption:** Owned by `#129 Customer Loyalty & Rewards` (`customer-loyalty-and-rewards`), which consumes `com.reviews.review_verified_published`.
- **General customer NPS/CSAT survey questionnaires unrelated to catalog SKUs:** Owned by `surveys` (`MktSurvey`).

### 2.3 Cross-Suite Ownership & Integration Contract

| Domain / App | Owns (Canonical Models) | Integration Contract with `product-reviews-and-ratings` |
| :--- | :--- | :--- |
| **`product-reviews-and-ratings` (`#128` — This App)** | `ComReviewRequest`, `ComReviewAttributeTemplate`, `ComProductReview`, `ComReviewMedia`, `ComProductQuestion`, `ComProductRatingSummary`, `ComReviewSyndicationFeed` | Collects, verifies, moderates, aggregates, summarizes, and syndicates product reviews, visual UGC, and PDP Q&A. |
| **`productcatalog`, `website`, `ecommerce` (`COM`)** | `ComProduct`, `ComVariant`, `ComSite`, `ComPage` | Reads SKU/GTIN hierarchy for parent-variant rollups; serves sub-50ms `ComProductRatingSummary`, UGC galleries, Q&A, and JSON-LD to PDPs. |
| **`shipping-label-and-fulfillment-management` (`#127`) & `pos`** | `ComShipment`, `ComTrackingEvent`, `ComPosTransaction` | Consumes `com.shipment.delivered` and POS receipt events to schedule `ComReviewRequest` invitations strictly post-receipt. |
| **`ordermanagement` (`OPS`) & `#130 Returns`** | `OpsCustomerOrder`, `ComReturnRequest` | Verifies order line ownership for `VERIFIED_BUYER` status; cancels pending `ComReviewRequest`s if items are cancelled or returned prior to send. |
| **`customer-loyalty-and-rewards` (`#129`)** | `ComLoyaltyMember`, `ComLoyaltyTransaction` | Publishes star-neutral `com.reviews.review_verified_published` (`TEXT_ONLY` / `WITH_PHOTO` / `WITH_VIDEO`) and marks incentivized reviews with mandatory FTC disclosure badge. |
| **`email` & `sms` (`MKT` Suite)** | `MktEmailMessage`, `MktSmsMessage` | Dispatches multi-item review invitations, reminders, and Q&A answer notifications honoring `CrmContact` channel consent. |
| **`support` (`SUP`) & `quality` (`MFG`)** | `SupTicket`, `MfgQualityInspection` | Opens `SupTicket` automatically on `<= 2-star` reviews for service recovery; surfaces SKU quality/fit-drift spikes via Home `WorkItem`s. |

---

## 3. Primary Web Pages & Sidebar Navigation Architecture (12 Pages)

Every visible sidebar section strictly enforces Rapider's **Minimum 3 Visible Items Rule** (`Moderation & Q&A`: 3 items, `Collection & UGC`: 3 items, `Insights & Syndication`: 3 items; detail/inspector routes set `displayOnMenu: false`).

| # | Route | Page Title | Sidebar Section | Menu Visible | Key Components & UX |
| :--- | :--- | :--- | :--- | :--- | :--- |
| 1 | `dashboard` | Reviews & Trust Overview | `Moderation & Q&A` | `true` (Order 1) | `countSWR`/`findSWR` KPI cards (Published Avg Rating, Verified Buyer %, Photo/Video UGC %, Pending Moderation, Unanswered Q&A, Invitation Conversion %), rating trend & top-defect callouts. |
| 2 | `reviews` | Reviews Moderation Queue | `Moderation & Q&A` | `true` (Order 2) | Bulk `findSWR` table & `<rapider-kanban>` board (`PENDING_SCREENING`, `FLAGGED_COMPLIANCE`, `PUBLISHED`, `QUARANTINED_LEGAL`), `#activeFilterTagsBar`, star/provenance badges, FTC-guarded quarantine modal, clickable Review `#` -> `review-detail`. |
| 3 | `questions` | Community Product Q&A | `Moderation & Q&A` | `true` (Order 3) | `ComProductQuestion` inbox with SLA timer, instant `ComProduct` spec sidebar, `Ask Past Buyers` toggle, AI draft answer helper, and clickable Question `#` -> `question-detail`. |
| 4 | `invitations` | Review Requests & Sampling | `Collection & UGC` | `true` (Order 1) | `ComReviewRequest` schedule ledger (`AWAITING_DELIVERY`, `SCHEDULED`, `SENT`, `REMINDER_SENT`, `COMPLETED`, `CANCELLED_RETURNED`), post-delivery delay rules, and seeded sampling cohort creator. |
| 5 | `media-gallery` | Photo & Video UGC Studio | `Collection & UGC` | `true` (Order 2) | Filterable visual UGC grid (`ComReviewMedia`) linked to `ComProductReview` and `ComVariant`, storefront carousel pinning, rights/provenance status, and safety moderation toggles. |
| 6 | `attribute-templates` | Fit Sliders & Smart Prompts | `Collection & UGC` | `true` (Order 3) | `ComReviewAttributeTemplate` builder mapping `ComProduct` categories to centering sliders (`Runs Small <-> True to Size <-> Runs Large`), reviewer context fields, and Smart Prompt topic chips. |
| 7 | `product-ratings` | SKU Rating & Fit Rollups | `Insights & Syndication` | `true` (Order 1) | `ComProductRatingSummary` catalog scorecard showing Average Rating (`Decimal128`), review volume, fit drift indicator, AI Pros/Cons summary status, and clickable Product SKU -> `product-rating-detail`. |
| 8 | `syndication` | Google & Retail Syndication | `Insights & Syndication` | `true` (Order 2) | `ComReviewSyndicationFeed` manager generating Google Merchant Center Product Ratings XML and retail feeds, GTIN match-rate diagnostics, and incentivized-review exclusion compliance toggles. |
| 9 | `quality-insights` | Sentiment & Defect Analytics | `Insights & Syndication` | `true` (Order 3) | Category & SKU topic sentiment heatmaps correlating 1–2 star themes (sizing mismatch, packaging damage, defect) with `SupTicket` recovery rates and CSV import tools. |
| 10 | `review-detail` | Review, Media & FTC Audit Detail | *(Hidden)* | `false` | Full `ComProductReview` workspace: verified order/POS link, attribute slider responses, media preview, public brand reply editor, linked `SupTicket`, immutable FTC moderation log, and `Back to Reviews`. |
| 11 | `question-detail` | Product Q&A Thread Inspector | *(Hidden)* | `false` | Complete `ComProductQuestion` view with shopper question, brand official answer, verified past-buyer community answers, and `Back to Q&A` filter restoration. |
| 12 | `product-rating-detail` | Product Rollup & AI Summary View | *(Hidden)* | `false` | Per-`ComProduct` rating histogram, variant-by-variant comparison, fit centering bar chart, grounded AI Pros/Cons summary citations, and JSON-LD rich-snippet preview. |

**UI Standards Compliance:**
- All initial loads use `findSWR` / `countSWR` without redundant `isDeleted` API query filters, paired with defensive in-memory `item.isDeleted !== true` filtering.
- `#activeFilterTagsBar` with HTML-escaped chips and `Clear all` renders above all filtered tables, grids, and kanban boards.
- Tables never render a standalone `View` button in `Actions`; Review `#`, Question `#`, and Product SKU navigate to detail views; row actions strictly use icon+text `Edit` and icon-only `Delete`.
- Primary/danger buttons enforce `#ffffff !important` white text; SMS invitation test/contact inputs use `<rapider-phone-input>`; native `window.alert`/`confirm`/`prompt` dialogs are strictly banned.

---

## 4. Canonical Data Models (`Com*` — 7 Models)

All models omit `projectId`, use platform UUIDs (`00000000-xxxx-xxxx-xxxx-xxxxxxxxxxxx` in deterministic seeds), maintain optimistic `version`, and store aggregate/slider math as `Decimal128`.

| Model Name | Key Fields & Types | Relationships & External Refs | Invariants & Lifecycle |
| :--- | :--- | :--- | :--- |
| **`ComReviewAttributeTemplate`** | `id` (UUID), `templateCode` (String), `name` (String), `productCategoryCode` (String), `sliderDefinitionsJson` (JSON: code, label, minLabel, centerLabel, maxLabel), `reviewerContextFieldsJson` (JSON), `smartPromptTopics` (Array<String>), `postDeliveryDelayDays` (Int), `isActive` (Boolean), `version` (Int) | Maps to `ComProduct` category taxonomy | `postDeliveryDelayDays >= 0`; slider definitions enforce 3- or 5-point discrete scales mapped to `-1.00 .. +1.00`. |
| **`ComReviewRequest`** | `id` (UUID), `requestNumber` (String), `customerOrderId` (UUID, nullable), `posTransactionId` (UUID, nullable), `shipmentId` (UUID, nullable), `contactId` (UUID), `recipientEmail` (String), `recipientPhone` (E.164 String, nullable), `provenanceType` (Enum: `VERIFIED_ORDER`, `VERIFIED_POS`, `SEEDED_FREE_SAMPLE`), `rewardOffered` (Boolean), `tokenHash` (String), `scheduledSendAt` (DateTime), `sentAt` (DateTime, nullable), `completedAt` (DateTime, nullable), `status` (Enum: `AWAITING_DELIVERY`, `SCHEDULED`, `SENT`, `REMINDER_SENT`, `COMPLETED`, `CANCELLED_ORDER_RETURNED`, `EXPIRED`), `version` (Int) | `customerOrderId` -> `OpsCustomerOrder.id`, `shipmentId` -> `ComShipment.id`, `posTransactionId` -> `ComPosTransaction.id`, `contactId` -> `CrmContact.id` | Raw token never persisted (only `tokenHash`); single submission per `(requestId, variantId)` line; auto-cancels if order/item is returned before `SENT`. |
| **`ComProductReview`** | `id` (UUID), `reviewNumber` (String), `productId` (UUID), `variantId` (UUID, nullable), `reviewRequestId` (UUID, nullable), `contactId` (UUID, nullable), `siteId` (UUID), `starRating` (Int: `1..5`), `storeExperienceRating` (Int: `1..5`, nullable), `headline` (String), `bodyText` (String), `redactedBodyText` (String), `attributeResponsesJson` (JSON), `provenanceBadge` (Enum: `VERIFIED_BUYER`, `INCENTIVIZED_REWARD`, `SEEDED_FREE_SAMPLE`, `UNVERIFIED_OPEN`, `VERIFIED_IMPORTED`), `isIncentivized` (Boolean), `localeCode` (String), `helpfulCount` (Int), `reportedCount` (Int), `moderationStatus` (Enum: `PENDING_SCREENING`, `FLAGGED_COMPLIANCE`, `PUBLISHED`, `QUARANTINED_LEGAL`), `quarantineReasonCode` (Enum, nullable), `quarantineAuditNote` (String, nullable), `merchantReplyText` (String, nullable), `merchantRepliedAt` (DateTime, nullable), `linkedSupportTicketId` (UUID, nullable), `publishedAt` (DateTime, nullable), `version` (Int) | `productId` -> `ComProduct.id`, `variantId` -> `ComVariant.id`, `reviewRequestId` -> `ComReviewRequest.id`, `siteId` -> `ComSite.id`, `linkedSupportTicketId` -> `SupTicket.id` | `1 <= starRating <= 5`; FTC invariant: transitioning to `QUARANTINED_LEGAL` **requires** a non-null statutory `quarantineReasonCode` and appends an immutable moderation audit record; `SEEDED_FREE_SAMPLE` or `rewardOffered = true` forces `isIncentivized = true`. |
| **`ComReviewMedia`** | `id` (UUID), `reviewId` (UUID), `productId` (UUID), `variantId` (UUID, nullable), `mediaType` (Enum: `PHOTO`, `VIDEO`), `fileId` (UUID), `thumbnailFileId` (UUID), `durationSeconds` (Int, nullable), `exifScrubbed` (Boolean), `isFeaturedOnPdp` (Boolean), `mediaModerationStatus` (Enum: `PENDING`, `APPROVED`, `REJECTED_SAFETY`), `version` (Int) | `reviewId` -> `ComProductReview.id`, `fileId` -> Rapider Files | `exifScrubbed` must be `true` before `mediaModerationStatus` can become `APPROVED` to protect buyer home GPS coordinates. |
| **`ComProductQuestion`** | `id` (UUID), `questionNumber` (String), `productId` (UUID), `variantId` (UUID, nullable), `siteId` (UUID), `askerContactId` (UUID, nullable), `askerDisplayName` (String), `askerEmail` (String), `questionText` (String), `officialAnswerText` (String, nullable), `officialAnsweredByUserId` (UUID, nullable), `communityAnswersJson` (JSON: verified buyer answers), `status` (Enum: `OPEN`, `ROUTED_TO_OWNERS`, `ANSWERED_PUBLISHED`, `QUARANTINED_SPAM`), `askedAt` (DateTime), `answeredAt` (DateTime, nullable), `version` (Int) | `productId` -> `ComProduct.id`, `siteId` -> `ComSite.id` | Only answers with `ANSWERED_PUBLISHED` render on public storefront PDP widgets. |
| **`ComProductRatingSummary`** | `id` (UUID), `productId` (UUID), `publishedReviewCount` (Int), `verifiedBuyerCount` (Int), `averageStarRating` (`Decimal128`), `star1Count` (Int), `star2Count` (Int), `star3Count` (Int), `star4Count` (Int), `star5Count` (Int), `mediaItemCount` (Int), `attributeRollupsJson` (JSON: `Decimal128` mean centering index per slider), `aiSummaryPros` (Array<String>), `aiSummaryCons` (Array<String>), `aiCitedReviewIds` (Array<UUID>), `aiSummaryUpdatedAt` (DateTime, nullable), `version` (Int) | 1-to-1 with `ComProduct.id` | Deterministically recomputed strictly from `PUBLISHED` `ComProductReview`s (`star1Count + ... + star5Count == publishedReviewCount`); AI summary requires `publishedReviewCount >= 10`. |
| **`ComReviewSyndicationFeed`** | `id` (UUID), `feedName` (String), `channelTarget` (Enum: `GOOGLE_SHOPPING_RATINGS`, `BING_SHOPPING`, `RETAIL_SYNDICATION_XML`, `MARKETPLACE_EXPORT`), `siteId` (UUID, nullable), `excludeIncentivized` (Boolean), `minReviewCountThreshold` (Int), `includedReviewCount` (Int), `matchedGtinSkuCount` (Int), `lastGeneratedFileId` (UUID, nullable), `lastGeneratedAt` (DateTime, nullable), `status` (Enum: `ACTIVE`, `GENERATING`, `ERROR`), `version` (Int) | `siteId` -> `ComSite.id`, `lastGeneratedFileId` -> Rapider Files | For `GOOGLE_SHOPPING_RATINGS`, `excludeIncentivized` is locked `true` if strict merchant policy applies or explicitly emitted with `<is_incentivized_review>true</is_incentivized_review>` per Google spec. |

---

## 5. Core End-to-End User Journeys (6 Journeys)

### Journey 1: Delivery-Anchored Multi-Item Review Capture -> Star-Neutral Loyalty Points
1. `#127 Shipping Label & Fulfillment Management` emits `com.shipment.delivered` for `OpsCustomerOrder` `ORD-90412` (2 items: Trail Running Shoes + Merino Socks).
2. `product-reviews-and-ratings` schedules `ComReviewRequest` for `+7 days`. When dispatched via `email`, the buyer opens the single-use progressive form, rates the Shoes `5 stars` (`Fit: True to Size`), uploads 2 trail photos (EXIF GPS stripped automatically), and rates the Socks `4 stars`—all in 60 seconds without logging in.
3. Automated PII/profanity screening passes both reviews to `PUBLISHED`, updates `ComProductRatingSummary`, and emits `com.reviews.review_verified_published` (`WITH_PHOTO`) so `#129 Customer Loyalty & Rewards` credits `100` star-neutral points while tagging the review `INCENTIVIZED_REWARD`.

### Journey 2: Honest 1-Star Review -> Non-Suppression Publication + Instant `SupTicket` Care
1. Verified buyer submits a `1-star` review on `SKU-BLENDER-PRO`: *"Motor smoked on second smoothie—very disappointed."*
2. Per FTC 16 CFR Part 465 rules, the review passes automated profanity/PII checks and **publishes transparently** to the PDP (updating `ComProductRatingSummary`), while simultaneously opening high-priority `SupTicket` `SUP-4410` in `support` and notifying Customer Care.
3. Support Agent dispatches a warranty replacement and posts a public `Verified Brand Reply` on `ComProductReview` via `#/review-detail`, turning a negative incident into visible brand trust.

### Journey 3: Flagged PII / Bot Submission -> Statutory Quarantine via Home `WorkItem`
1. An incoming review contains the buyer's raw mobile number and home street address in `bodyText`.
2. Automated screening holds the review in `FLAGGED_COMPLIANCE`, stores masked text in `redactedBodyText`, and creates a Rapider Home `WorkItem` for the E-Commerce Moderator.
3. Moderator opens `#/review-detail`, either publishes the PII-redacted version or—if confirmed bot spam—selects mandatory statutory reason `SPAM_BOT_FAKE` in the custom quarantine modal, logging an immutable FTC compliance record.

### Journey 4: Pre-Purchase Shopper Q&A -> Grounded AI Draft -> Published FAQ
1. Shopper on the `ComSite` PDP asks: *"Is the inner carafe BPA-free stainless steel or Tritan plastic?"* (`ComProductQuestion`).
2. Merchandiser opens `#/questions`, clicks `Draft Grounded Answer` (which cites `ComProduct` specification `innerMaterial: "304 Food-Grade Stainless Steel"`), reviews and approves the answer, emailing the shopper immediately and publishing the Q&A onto the PDP.

### Journey 5: Google Shopping XML Syndication & Grounded Pros/Cons PDP Summary
1. Once `SKU-TRAIL-SHOE` reaches `25` published reviews, background workflow generates a grounded `ComProductRatingSummary` AI digest (*Pros: Grip on wet rock, roomy toe box [cites 18 reviews]; Cons: Laces run slightly long [cites 4 reviews]*).
2. Merchandiser opens `#/syndication`, runs `Google Shopping Product Ratings XML`, verifies 100% GTIN match against `ComVariant`, and publishes the feed endpoint for Google Merchant Center star snippet indexing.

### Journey 6: Click-Through Filtering & State Restoration
1. Moderator filters `#/reviews` by `Star Rating: "1-2 Stars"` and `Status: "FLAGGED_COMPLIANCE"`, rendering `#activeFilterTagsBar` with escaped chips and `Clear all`.
2. Moderator clicks `REV-2026-1108`, inspects media and order provenance on `#/review-detail?id=...`, posts a brand reply, and clicks `Back to Reviews`—returning with active filters intact.

---

## 6. Consolidated Feature Priorities & Work Packages (`P0`, `P1`, `P2`)

### 6.1 `P0` Foundation Work Packages (Must-Have Launch Core)
- **WP-P0-1 — Delivery-Aware Invitations & Structured Capture (`PRR-001`, `PRR-002`, `PRR-003`, `PRR-004`):** `ComReviewRequest` post-delivery scheduling, single-form multi-item mobile capture, `ComReviewAttributeTemplate` fit sliders, and EXIF-scrubbed `ComReviewMedia` uploads.
- **WP-P0-2 — FTC-Compliant Moderation & Provenance Engine (`PRR-007`, `PRR-008`, `PRR-009`):** `VERIFIED_BUYER`/`INCENTIVIZED_REWARD`/`SEEDED_FREE_SAMPLE` badges, statutory `quarantineReasonCode` enforcement, and automated PII/profanity/bot screening.
- **WP-P0-3 — Community Q&A & Merchant Replies (`PRR-012`, `PRR-013`):** `ComProductQuestion` capture/search and official brand replies across reviews and Q&A.
- **WP-P0-4 — Rollup Aggregates, Storefront Widgets & Low-Star Care (`PRR-015`, `PRR-016`, `PRR-019`):** `ComProductRatingSummary` `Decimal128` rollups, `Schema.org` JSON-LD / PDP widget APIs, and automatic `<= 2-star` `SupTicket` creation.

### 6.2 `P1` Scale & Enterprise Governance Work Packages
- **WP-P1-1 — Store-vs-Product Split, Loyalty Hooks & Helpful Votes (`PRR-005`, `PRR-010`, `PRR-011`, `PRR-024`):** Separate delivery/merchant score, star-neutral `#129` loyalty event emission, community helpful/abuse voting, and provenance-preserving CSV migration imports.
- **WP-P1-2 — Ask-an-Owner Q&A, Syndication & Multi-Store Sharing (`PRR-014`, `PRR-017`, `PRR-018`):** Past-buyer Q&A routing, `ComReviewSyndicationFeed` Google Shopping XML + retail exports, and GTIN-based cross-locale `ComSite` review sharing.
- **WP-P1-3 — SKU Defect & Fit-Drift Intelligence (`PRR-020`):** Cross-referencing negative topic surges with `#130` return reasons and raising `MfgQualityInspection` / merchandising Home `WorkItem`s.

### 6.3 `P2` Adjacent & Grounded AI Work Packages
- **WP-P2-1 — Smart Prompts, Grounded PDP Pros/Cons Summaries & Reply Copilot (`PRR-006`, `PRR-021`, `PRR-022`):** Real-time writing topic prompts, citation-backed Pros/Cons `ComProductRatingSummary` digests, and `ComProduct`-grounded reply/Q&A drafting in `projectChannelMessage`.
- **WP-P2-2 — Seeded Sampling Campaigns (`PRR-023`):** Complimentary product seeding invitations with mandatory FTC free-sample disclosure enforcement.

---

## 7. Roles, Permissions & Field-Level Governance

| Role | Permitted Actions | Restricted / Field-Level Boundaries |
| :--- | :--- | :--- |
| **Community / UGC Moderator** | Triage `FLAGGED_COMPLIANCE` reviews and `ComReviewMedia`, approve PII-redacted text, post public brand replies, and answer `ComProductQuestion`s. | Cannot quarantine a review without a statutory `quarantineReasonCode`; cannot edit a customer's `starRating` or rewrite buyer sentiment. |
| **E-Commerce Merchandising Manager** | Configure `ComReviewAttributeTemplate` sliders, post-delivery timing, featured UGC pins, `ComReviewSyndicationFeed` exports, and seeded sampling cohorts. | Cannot delete immutable FTC quarantine audit logs or configure star-gated loyalty incentives (blocked by schema/command validation). |
| **Brand Compliance / Legal Officer** | Audit all `QUARANTINED_LEGAL` decisions, review incentivized/sampling badge compliance, and process GDPR reviewer erasure requests. | Read-only on catalog syndication and merchandising template layout settings. |
| **Customer Support Agent (`SUP`)** | View linked `ComProductReview` context from `SupTicket`s and draft public brand follow-up replies. | Cannot alter moderation policies, syndication feeds, or category slider templates. |

---

## 8. Technical APIs, Guarded Commands, Events & `requiredWorkflows`

### 8.1 Guarded Domain Commands
All commands enforce `idempotencyKey`, `correlationId`, and `expectedVersion`:
- `com.reviews.schedulePostDeliveryRequest` — Creates/updates `ComReviewRequest` anchored to `com.shipment.delivered` or `ComPosTransaction`.
- `com.reviews.submitVerifiedReview` — Validates single-use `tokenHash`, strips EXIF GPS from `ComReviewMedia`, screens PII/profanity, and persists `ComProductReview`.
- `com.reviews.moderateReview` — Transitions `ComProductReview` to `PUBLISHED` or `QUARANTINED_LEGAL` (strictly rejecting quarantine calls missing a valid statutory `quarantineReasonCode`).
- `com.reviews.answerProductQuestion` — Publishes official brand answer on `ComProductQuestion` and notifies the asker.
- `com.reviews.generateSyndicationFeed` — Builds GTIN-matched XML/JSON `ComReviewSyndicationFeed` artifacts into Rapider Files.

### 8.2 Domain Events Published & Consumed (Kafka)
- **Published:** `com.reviews.request_dispatched`, `com.reviews.review_submitted`, `com.reviews.review_verified_published` (star-neutral payload for `#129`), `com.reviews.low_rating_escalated` (`SupTicket`), `com.reviews.summary_recomputed`, `com.reviews.quality_drift_alert`.
- **Consumed:** `com.shipment.delivered` (`#127`), `com.pos.transaction_completed` (`pos`), `ops.order.cancelled` (`ordermanagement`), `com.returns.rma_authorized` (`#130`), `com.catalog.product_updated` (`productcatalog`).

### 8.3 `requiredWorkflows` (Declared in `app.manifest.json`)
1. `wf-reviews-post-delivery-scheduler` — Listens to `com.shipment.delivered` and `com.returns.rma_authorized` to dispatch timely invitations and cancel returned-item invites.
2. `wf-reviews-screening-and-rollup` — Runs PII/profanity/bot checks, creates `SupTicket`s on `<= 2-star` submissions, recomputes `Decimal128` `ComProductRatingSummary`, and triggers `#129` reward events.
3. `wf-reviews-ai-pros-cons-refresher` — Periodically refreshes grounded Pros/Cons bullets on products crossing new verified-review milestones (`>= 10` reviews).
4. `wf-reviews-merchant-center-feed-builder` — Generates daily Google Shopping Product Ratings XML feeds matched against `ComVariant` GTINs.

### 8.4 Grounded Rapider AI Governance
- PDP AI Review Summaries **must** include both common Pros and honest Cons whenever negative themes exist in published reviews, citing exact `ComProductReview.id`s—never whitewashing defects.
- AI Q&A and reply drafters strictly cite `ComProduct` attributes and `SupKnowledgeArticle` records, and **never** auto-post public brand replies or quarantine user reviews without human confirmation.

---

## 9. Security, Privacy, Compliance & Non-Functional Requirements

- **FTC Consumer Review Rule (16 CFR Part 465) & EU Omnibus Directive Compliance:** Star-gated review gating ("if 4–5 stars go to public review, if 1–3 stars go to private form") and star-conditioned incentives are strictly prohibited at the architectural level. Every incentivized or seeded review carries a permanent, conspicuous disclosure badge, and every moderation quarantine decision is logged immutably with actor, timestamp, and statutory reason code.
- **Reviewer Privacy & EXIF Stripping (GDPR / CCPA):** Reviewer emails and phone numbers are never exposed in public storefront APIs (only formatted display name such as `Maya R.`); uploaded photos/videos automatically strip GPS/device EXIF metadata prior to storage; GDPR right-to-erasure anonymizes reviewer PII while optionally retaining de-identified aggregate product scores.
- **Performance, Accessibility & Responsive UX:** Public `ComProductRatingSummary` widget reads respond in `< 50ms` via SWR/CDN caching; WCAG 2.2 AA accessible star selectors, fit sliders, video captions/controls, and `#ffffff !important` primary/danger buttons across `390px`, `768px`, and `1440px` viewports.

---

## 10. Deterministic Acceptance Criteria (14 Testable Criteria)

1. **Sidebar Section Threshold:** All 3 visible sidebar sections (`Moderation & Q&A`, `Collection & UGC`, `Insights & Syndication`) contain >= 3 visible items (`3, 3, 3`), while `review-detail`, `question-detail`, and `product-rating-detail` set `displayOnMenu: false`.
2. **SWR & Clean Query Compliance:** Every page mount uses `findSWR` / `countSWR` with zero redundant `isDeleted` API query filters and defensive client `item.isDeleted !== true` filtering.
3. **Active Filter Tags & Clear All:** Filtering or searching on `reviews`, `questions`, `invitations`, `media-gallery`, `product-ratings`, or `syndication` displays `#activeFilterTagsBar` with escaped chips and working `Clear all`.
4. **Table Action & Click-Through Standard:** Zero `View` buttons exist in `Actions`; Review `#`, Question `#`, and Product SKU navigate to detail views and `Back` restores filter state; row actions use icon+text `Edit` and icon-only `Delete`.
5. **Drawer Lifecycle & `finally` Reset:** Saving/cancelling template, invitation, sampling, or reply drawers auto-closes the container, resets button state in `finally`, invalidates SWR caches, and updates UI state optimistically.
6. **Delivery-Anchored Invitation Guard:** `ComReviewRequest` remains `AWAITING_DELIVERY` until `com.shipment.delivered` arrives, and transitions to `CANCELLED_ORDER_RETURNED` if `#130` authorizes a return beforehand.
7. **FTC Statutory Quarantine Invariant:** Calling `com.reviews.moderateReview` with `QUARANTINED_LEGAL` and an empty `quarantineReasonCode` deterministically fails validation; customers' `starRating` (`1..5`) is immutable by staff.
8. **Star-Neutral Incentive & Disclosure Enforcement:** Submitting a `1-star` verified review on a `rewardOffered = true` request sets `isIncentivized = true` and emits the exact same `com.reviews.review_verified_published` reward tier event to `#129` as a `5-star` review.
9. **Low-Star Service Recovery Bridge:** Submitting a `<= 2-star` review automatically creates/links a `SupTicket` while allowing clean reviews to transition to `PUBLISHED`.
10. **EXIF Privacy Scrubbing:** No `ComReviewMedia` record can transition to `APPROVED` unless `exifScrubbed == true`.
11. **Exact `Decimal128` Rollup Integrity:** `ComProductRatingSummary.averageStarRating` equals exact `Decimal128` weighted mean of `PUBLISHED` reviews only, and `star1Count + ... + star5Count == publishedReviewCount`.
12. **Custom Elements & Dialog Ban:** `<rapider-kanban>` renders the review moderation board, `<rapider-phone-input>` normalizes SMS recipient phones, primary/danger buttons enforce `#ffffff !important` text, and zero `window.alert`/`confirm`/`prompt` calls exist.
13. **Dark & Light Theme Fidelity:** All 12 pages pass `validate_theme` contrast checks in dark and light themes.
14. **6-Pillar E2E Expectations Coverage:** `e2e/expectations.json` deterministically covers all clickable actions, `findSWR`/`countSWR` reads, click-through navigation, theme validation, form mutations, and zero runtime errors.
