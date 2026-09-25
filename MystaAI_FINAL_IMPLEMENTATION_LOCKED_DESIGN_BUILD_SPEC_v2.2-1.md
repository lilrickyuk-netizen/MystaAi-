# MystaAI — FINAL IMPLEMENTATION-LOCKED PRODUCT DESIGN & BUILD SPECIFICATION

**Version:** 2.2  
**Status:** FINAL / IMPLEMENTATION LOCKED  
**Lock date:** 25 September 2026  
**Research basis:** v2.0/v2.1 verified design retained; v2.2 rescopes the production AI provider to OpenAI GPT-5 mini and the launch infrastructure to a single-region 2–3M-user capacity target, reverified 25 September 2026 against current official OpenAI/AWS sources  
**Product name:** **MystaAI**  
**Positioning:** **MystaAI — Your Personal AI Mystic**  
**Product type:** Commercial AI-assisted divination, spiritual-reflection and personalised reading platform  
**Launch surfaces:** Web, iOS, Android  
**Build standard:** Full production product — **not an MVP, not a demo, not a prototype**  
**Internal slug:** `mystaai`  
**Application namespace:** `com.richardcurley.mystaai`  
**Primary launch AI provider:** OpenAI GPT-5 mini (`gpt-5-mini`) through the OpenAI Responses API; DeepSeek is not permitted in the v2.2 production design  
**Cloud architecture:** AWS global web edge + single-region application/data plane in `eu-west-2`; one production cell at launch, with cell-ready expansion retained but not built as a launch requirement  
**Minimum launch age:** 18+

> **This document is the single build authority for MystaAI. Version 2.2 supersedes v2.1 and v2.0 and incorporates the human-psychology knowledge layer, the OpenAI GPT-5 mini production gateway, and the single-region 2–3M-user scale architecture.** It supersedes every earlier generic “AI Divination Platform” design and every partial MystaAI specification. No merging is required. If implementation conflicts with this document, implementation is wrong unless a formally approved specification revision exists.

---

# 0. DOCUMENT AUTHORITY, CHANGE CONTROL AND ZERO-GUESSWORK RULE

## 0.1 Authority hierarchy

1. This implementation-locked specification.
2. Machine-readable manifests committed to the repository.
3. Exact dependency lockfiles, infrastructure state and database migrations.
4. Production implementation.
5. Generated documentation.

No developer, coding assistant, agent or contractor may silently change technology, algorithms, layouts, features, pricing, licensing assumptions, model providers, visual identity or acceptance criteria.

## 0.2 Definition of “zero guesswork”

Before any production subsystem begins, the specification or a referenced machine-readable manifest must define:

- purpose;
- repository path;
- exact dependency/service or exact managed-version selection rule;
- official source;
- licence and commercial-use status;
- runtime requirements;
- input contract;
- output contract;
- algorithm;
- database ownership;
- environment variables;
- credentials/permissions;
- timeout;
- retry behaviour;
- caching behaviour;
- failure state;
- rollback/recovery behaviour;
- security boundary;
- privacy classification;
- tests;
- fixtures;
- acceptance criteria;
- visual acceptance criteria where applicable.

If one of these is unresolved, that phase is blocked. The builder does not “pick something suitable.”

## 0.3 No placeholders

Completed production phases may not contain:

- empty production directories;
- TODO-only production files;
- fake services;
- mocked production integrations;
- stub calculations represented as working;
- unimplemented routes represented as complete;
- hard-coded fake data;
- fake validation evidence;
- test-only payment state in production;
- unlicensed assets;
- copied competitor designs.

Mocks are permitted only in explicit automated tests and local fixtures.

## 0.4 Required manifests

```text
manifests/
  DEPENDENCY_MANIFEST.json
  INTEGRATION_MANIFEST.json
  KNOWLEDGE_SOURCE_MANIFEST.json
  MODEL_MANIFEST.json
  ENVIRONMENT_MANIFEST.json
  ASSET_MANIFEST.json
  FONT_MANIFEST.json
  BRAND_MANIFEST.json
  STORE_PRODUCT_MANIFEST.json
  SWISSEPH_MANIFEST.json
  SECURITY_CONTROL_MANIFEST.json
  DATA_RETENTION_MANIFEST.json
  PROMPT_MANIFEST.json
  UI_ROUTE_MANIFEST.json
  FEATURE_ENTITLEMENT_MANIFEST.json
  SCALE_MANIFEST.json
  SERVICE_QUOTA_MANIFEST.json
  REGION_CELL_MANIFEST.json
  PSYCHOLOGY_EVIDENCE_MANIFEST.json
```

Each manifest has a JSON Schema and is validated in CI.

## 0.5 Phase 0 specification-control gate

Before Phase 1 begins, execute **Phase 0 — v2.2 design/control verification**. Phase 0 does not author production code. It verifies that this v2.2 document, its manifests and its official-source assumptions still describe the current external world.

Phase 0 must reverify:

- the psychology source/licence rules in Sections 46–50;
- OpenAI (GPT-5 mini) model names, quotas and capacity-expansion path;
- AWS single-region Aurora PostgreSQL, RDS Proxy, SQS, ElastiCache, CloudFront, WAF, ALB, ECS/Fargate and service quotas;
- the active Region/cell plan and legal/data-residency constraints;
- the scale envelope and load-test tooling;
- every dependency/security/store rule already covered by the existing preflight.

A material external change changes the implementation material, not MystaAI's locked product outcome. A replacement is accepted only if it preserves or improves functionality, security, correctness, privacy, scale and acceptance criteria.

---

## 0.6 Dependency rule

Production package manifests use exact versions, not floating ranges.

Forbidden for production runtime dependencies:

```text
^x.y.z
~x.y.z
latest
next
beta
rc
```

Exception: Expo-managed native packages are resolved with `npx expo install` for the locked SDK because Expo owns native compatibility; the resolved versions are immediately frozen in `pnpm-lock.yaml` and `DEPENDENCY_MANIFEST.json`.

## 0.7 Preflight rule

Before Phase 1 and again before production launch, run:

```text
infra/scripts/preflight-lock.ts
```

The script and human checklist verify:

- package/version availability;
- security advisories;
- licence status;
- OpenAI API/model availability;
- Swiss Ephemeris release/licence;
- GeoNames availability;
- AWS support;
- Apple/Google billing rules;
- RevenueCat behaviour;
- Stripe API compatibility;
- privacy/international-transfer obligations.

A material external change causes a controlled spec revision. It does not authorise silent substitution.

---

# 1. PRODUCT MISSION

MystaAI is a persistent personal AI mystic combining deterministic divination engines, controlled traditional knowledge, personal history and AI-generated interpretation.

Launch includes:

- Tarot;
- Zodiac / star signs;
- Pythagorean numerology;
- Natal astrology;
- Current transits;
- Transit-to-natal interpretation;
- Synastry;
- Compatibility;
- Dream interpretation;
- Dream journal;
- AI Mystic conversation;
- evidence-informed human-psychology knowledge for reflective conversation;
- Cross-system synthesis;
- Daily readings;
- Weekly readings;
- Monthly readings;
- Annual forecasts;
- Personal mystical profile;
- Reading history;
- Recurring-card tracking;
- Recurring-symbol tracking;
- Recurring-theme tracking;
- Premium reports;
- Subscription tiers;
- One-off purchases;
- Credit top-ups;
- Web app;
- iOS app;
- Android app;
- Admin/operations console;
- single-region production infrastructure designed for the 2–3M-user launch-scale target, with a clean same-product expansion path when measured demand requires more capacity.

MystaAI is not:

```text
User → generic prompt → generic LLM answer
```

The production chain is:

```text
USER
 ↓
AUTH + PERSONAL PROFILE
 ↓
REQUEST CLASSIFIER
 ↓
DIVINATION ORCHESTRATOR
 ↓
DETERMINISTIC DOMAIN ENGINES
 ↓
CONTROLLED KNOWLEDGE RETRIEVAL
 ↓
CROSS-SYSTEM SYNTHESIS
 ↓
PRIVACY / SAFETY GATE
 ↓
OPENAI GPT-5 MINI
 ↓
TRUTH / EVIDENCE / TRADITION VALIDATOR
 ↓
SAFETY VALIDATOR
 ↓
READING
 ↓
EVIDENCE STORE
 ↓
PERSONAL READING TIMELINE
```

---

# 2. PERMANENT PRODUCT PRINCIPLES

1. Deterministic calculation outranks generated language.
2. AI never chooses authoritative tarot cards.
3. AI never calculates authoritative numerology.
4. AI never calculates authoritative astrology.
5. Traditional meanings come from controlled knowledge.
6. AI synthesises; it does not invent hidden factual state.
7. Every completed reading stores evidence.
8. Old readings remain reproducible.
9. Unknown remains unknown.
10. External-service failure never becomes fabricated success.
11. Divination content is presented as reflection/entertainment, not guaranteed supernatural fact.
12. Personal data is minimised before model calls.
13. User-facing design is original to MystaAI.
14. Competitors may be researched but never copied.
15. Every feature must feel like one coherent MystaAI product.
16. Empirical psychology and mystical tradition remain separate evidence classes.
17. Psychology improves reflection and conversational quality; it never becomes diagnosis, therapy or covert clinical profiling.
18. Psychological vulnerability is never used to manipulate pricing, paywalls, retention or advertising.
19. MystaAI launches as one production cell in `eu-west-2`; growth within the 2–3M target is handled by vertical/horizontal capacity scaling, and later cell/Region expansion must not require product redesign.
20. Failure of a service/AZ inside the launch Region must be contained and recoverable; if additional cells are activated later, failure of one cell must not cascade into another.
21. The launch architecture may use one authoritative Aurora writer cluster in `eu-west-2`; database scaling limits are measured before capacity is exhausted and later cells receive independent transactional capacity.
22. External provider/service quotas are explicit architecture dependencies and are monitored before demand reaches them.

---

# 3. TRUTH / EVIDENCE / TRADITION CLAIM MODEL

Every generated claim is classified as exactly one of:

## 3.1 FACT

Examples:

- stored birth date;
- resolved latitude/longitude;
- card actually drawn;
- active entitlement;
- persisted reading history.

## 3.2 CALCULATION

Examples:

- Life Path 8;
- Sun longitude;
- Sun sign;
- Ascendant;
- house cusp;
- aspect;
- Personal Day;
- reversed/upright orientation.

Calculations come only from deterministic code.

## 3.3 EVIDENCE

Evidence is an empirical psychology/behavioural proposition supported by an approved, rights-cleared source record.

Examples:

- a finding from an approved systematic review or meta-analysis;
- an evidence-based statement about habit formation, cognitive bias, communication, motivation or emotional regulation;
- an approved public-health psychology statement from a public-domain authoritative source.

Every EVIDENCE claim requires approved psychology knowledge IDs, evidence grade, population/context metadata and known limitations. Evidence does **not** scientifically validate tarot, astrology, numerology, dream symbolism or other mystical traditions.

## 3.4 TRADITION

Examples:

- traditional meaning of The Star;
- traditional symbolism associated with Scorpio;
- traditional interpretation of Life Path 8;
- traditional meaning of a square aspect.

Traditional claims require approved knowledge IDs.

## 3.5 SYNTHESIS

AI-generated language combining evidence.

Valid:

> “Taken together, these themes could place greater emphasis on structure and renewal.”

Invalid:

> “This guarantees you will get a promotion.”

## 3.6 Authority order

```text
FACT
>
DETERMINISTIC CALCULATION
>
APPROVED EMPIRICAL PSYCHOLOGY EVIDENCE
>
APPROVED TRADITIONAL KNOWLEDGE
>
AI SYNTHESIS
```

Where empirical psychology and mystical tradition address different kinds of claims, MystaAI preserves that distinction rather than pretending one validates the other.

---

# 4. AGE, SAFETY AND POSITIONING

## 4.1 Launch age

```text
18+
```

## 4.2 Positioning

MystaAI is:

> spiritual reflection, entertainment and personal insight.

It does not guarantee future events.

## 4.3 High-stakes categories

Special handling is mandatory for:

- self-harm;
- suicide;
- serious illness;
- medical diagnosis;
- medication;
- pregnancy;
- death prediction;
- legal outcomes;
- investment decisions;
- gambling;
- abuse;
- criminal allegations;
- missing persons;
- psychological/psychiatric diagnosis;
- mental-health crisis;
- trauma diagnosis/inference;
- personality-disorder diagnosis.

Mysta may provide symbolic reflection and evidence-informed general psychology, but cannot substitute professional advice, diagnose a user, infer a hidden disorder/trauma as fact, or present dangerous certainty.

---

# 5. BRAND IDENTITY — LOCKED

## 5.1 Product name

```text
MystaAI
```

## 5.2 Positioning line

```text
MystaAI — Your Personal AI Mystic
```

## 5.3 Brand promise

> A private celestial sanctuary where Mysta helps the user explore tarot, astrology, numerology, dreams and personal patterns through one intelligent, consistent experience.

## 5.4 Brand qualities

MystaAI is:

- premium;
- mystical;
- elegant;
- warm;
- feminine;
- intelligent;
- calm;
- cinematic;
- trustworthy;
- modern;
- celestial;
- original.

MystaAI is not:

- cheap;
- carnival-like;
- horror-themed;
- childish;
- tacky;
- neon-gaming;
- cluttered;
- a generic chatbot;
- a clone of a competitor.

---

# 6. ORIGINALITY / NO-COPY RULE — HARD LOCK

Competitors may be reviewed only for:

- category conventions;
- common user expectations;
- information architecture patterns;
- accessibility patterns;
- feature coverage;
- store positioning.

Forbidden:

- tracing competitor screens;
- copying competitor layouts screen-for-screen;
- copying card artwork;
- copying icons;
- copying logos;
- copying illustrations;
- copying exact marketing wording;
- copying distinctive animations;
- reproducing a competitor’s visual identity with different colours;
- deliberately using competitor screenshots as style-transfer targets for production artwork.

Every launch screen requires an originality review stored in:

```text
docs/design/originality-review/
```

Checklist:

```text
[ ] Layout follows MystaAI route/component specification.
[ ] Icons are MystaAI-owned or appropriately licensed.
[ ] Illustrations are MystaAI-owned or appropriately licensed.
[ ] Copy is original.
[ ] Tarot artwork is rights-cleared.
[ ] No distinctive competitor visual signature is reproduced.
[ ] Brand tokens match MystaAI.
[ ] Mysta matches the approved character reference.
```

---

# 7. MYSTA CHARACTER — LOCKED

Mysta is the visual and conversational face of MystaAI.

## 7.1 Approved reference

The user-approved reference supplied in the design conversation is authoritative for Mysta’s visual direction.

Repository targets:

```text
assets/brand/mysta/mysta-reference-v1.png
apps/mobile/assets/brand/mysta/mysta-reference-v1.png
apps/web/public/brand/mysta/mysta-reference-v1.png
```

Reference properties:

```text
Dimensions: 1229 × 1536
SHA-256: a19bbc98ee064fa6212bf8b5d66742f13b667b128641dccb398ee392cb951046
```

## 7.2 Character attributes

Mysta remains:

- warm;
- kind;
- welcoming;
- wise;
- feminine;
- reassuring;
- elegant;
- calm;
- trustworthy;
- spiritually attuned;
- approachable;
- premium;
- cinematic.

## 7.3 Visual attributes

Preserve:

- rich dark wavy hair;
- friendly recognisable face;
- warm smile;
- celestial mystical clothing;
- gold moon/star detailing;
- elegant jewellery;
- crescent/celestial motifs;
- premium painterly-cartoon finish;
- deep indigo/navy/violet environment;
- warm gold illumination.

## 7.4 Character drift prohibited

Mysta must not become:

- childish cartoon;
- generic anime;
- hypersexualised fantasy character;
- horror fortune teller;
- generic stock mystic;
- materially different face across screens;
- visually copied from another app/personality.

## 7.5 Commercial likeness gate

If the approved reference retains the recognisable likeness of a real person, commercial release is blocked until MystaAI holds written permission/model-release rights sufficient for commercial use. If permission is unavailable, a legally distinct original Mysta face must be created and explicitly approved before launch.

## 7.6 Required expression set

```text
mysta-neutral
mysta-welcome
mysta-listening
mysta-reflecting
mysta-reading
mysta-gentle-smile
mysta-celebratory
mysta-concerned
mysta-unavailable
```

Expressions remain subtle and character-consistent.

---

# 8. COLOUR SYSTEM — LOCKED

## 8.1 Core tokens

```text
--mysta-midnight-void:   #0B1020
--mysta-cosmic-navy:     #141B33
--mysta-mystic-indigo:   #23245A
--mysta-nebula-violet:   #4A3C7A
--mysta-moon-gold:       #D6A85F
--mysta-soft-gold:       #F0C979
--mysta-ivory:           #F5EBDD
--mysta-star-white:      #FFF9F0
```

Supporting accents:

```text
--mysta-amethyst:        #8A6BD8
--mysta-celestial-rose:  #C98BB8
--mysta-aura-blue:       #6A8DFF
```

Functional colours:

```text
--mysta-success:         #6DBA8B
--mysta-warning:         #E3B85C
--mysta-danger:          #D86A74
--mysta-info:            #6A8DFF
```

## 8.2 Surface hierarchy

```text
App background        Midnight Void
Primary surface       Cosmic Navy
Raised surface        Mystic Indigo
Premium emphasis      Moon Gold / Soft Gold
Primary text          Star White
Secondary text        Ivory
```

Gold is an accent, not a full-screen background.

Purple is restrained; the product must not become undifferentiated purple-on-purple UI.

---

# 9. TYPOGRAPHY — LOCKED

## 9.1 Display / editorial

**Playfair Display**

Use for:

- major headings;
- reading titles;
- tarot card names;
- report covers;
- premium editorial moments.

Licence: SIL Open Font License 1.1.

## 9.2 Product/UI

**Inter**

Use for:

- body;
- buttons;
- inputs;
- navigation;
- data;
- settings;
- charts;
- accessibility-critical copy.

Licence: SIL Open Font License 1.1.

## 9.3 Font handling

Approved font files are bundled locally and recorded in `FONT_MANIFEST.json` with:

- upstream source;
- pinned commit/version;
- filename;
- weight;
- licence;
- SHA-256.

No untracked runtime font CDN dependency.

## 9.4 Mobile type scale

```text
Display XL   40 / 46
Display L    34 / 40
H1           30 / 36
H2           26 / 32
H3           22 / 28
Title        19 / 25
Body L       17 / 26
Body         15 / 23
Body S       14 / 21
Caption      12 / 17
Micro        11 / 15
```

Dynamic Type / text scaling is supported.

---

# 10. LOGO, SIGIL AND APP ICON — LOCKED DIRECTION

## 10.1 Wordmark

```text
MystaAI
```

Use a bespoke wordmark treatment derived from the locked typography system.

## 10.2 MystaAI Sigil

Custom-draw:

- crescent;
- central guiding star;
- subtle orbital geometry.

The sigil must be original and not copied from another astrology/divination brand.

## 10.3 App icon

```text
Background: Midnight Void
Mark: Moon Gold / Soft Gold MystaAI Sigil
```

Requirements:

- readable at small launcher size;
- no tiny zodiac wheel;
- no text inside icon;
- passes iOS and Android masks;
- recognisable in monochrome.

## 10.4 Required exports

```text
wordmark-dark
wordmark-light
sigil-dark
sigil-light
horizontal-lockup
stacked-lockup
app-icon
monochrome-mark
```

---

# 11. UI SHAPE LANGUAGE — LOCKED

Spacing scale:

```text
4 8 12 16 20 24 32 40 48 64
```

Radius scale:

```text
xs 8
sm 12
md 16
lg 22
xl 28
pill 999
```

Borders:

```text
Default: 1px
Ordinary: low-opacity ivory
Premium: low-opacity Moon Gold
```

Elevation uses soft navy shadow, subtle inner border and restrained glow. Heavy generic drop shadows are avoided.

Glass effects are decorative only; content must remain readable without blur.

---

# 12. MOTION LANGUAGE — LOCKED

```text
instant       120ms
fast          180ms
standard      260ms
slow          420ms
ceremonial    700ms
```

Allowed:

- subtle shimmer;
- star twinkle;
- gentle card lift;
- tarot flip;
- soft glow pulse;
- fade/scale;
- calm page transition;
- restrained parallax.

Forbidden:

- aggressive bouncing;
- repeated shaking;
- flashing/strobing;
- excessive particles;
- manipulative attention motion.

Reduced-motion settings are honoured.

---

# 13. MAIN NAVIGATION — LOCKED

Mobile bottom navigation:

```text
Today
Readings
Mystic
Explore
Profile
```

Labels remain visible.

`Mystic` receives restrained gold emphasis when selected.

Desktop uses the same information architecture adapted to a persistent side/top navigation.

---

# 14. COMPLETE SCREEN / ROUTE INVENTORY

No launch route family below may be omitted.

## 14.1 Authentication and onboarding

```text
/auth/welcome
/auth/age
/auth/sign-in
/auth/sign-up
/auth/verify-email
/auth/forgot-password
/auth/reset-password

/onboarding/name
/onboarding/birth-date
/onboarding/birth-time
/onboarding/birth-place
/onboarding/birth-time-confidence
/onboarding/interests
/onboarding/reader-persona
/onboarding/notifications
/onboarding/initial-profile
/onboarding/welcome-reading
```

## 14.2 Today

```text
/today
/today/daily-tarot
/today/daily-zodiac
/today/personal-day
/today/transits
/today/insight
```

## 14.3 Readings

```text
/readings
/readings/tarot
/readings/tarot/select
/readings/tarot/draw
/readings/tarot/reveal
/readings/tarot/result
/readings/numerology
/readings/numerology/profile
/readings/zodiac
/readings/astrology
/readings/astrology/natal
/readings/astrology/transits
/readings/compatibility
/readings/compatibility/profile
/readings/compatibility/result
/readings/dream
/readings/cross-system
```

## 14.4 Mystic

```text
/mystic
/mystic/new
/mystic/:conversationId
/mystic/history
```

## 14.5 Explore

```text
/explore
/explore/tarot
/explore/numerology
/explore/zodiac
/explore/astrology
/explore/dreams
/explore/patterns
/explore/reports
/explore/learn
```

## 14.6 Dreams

```text
/dreams
/dreams/new
/dreams/:id
/dreams/:id/interpretation
/dreams/patterns
```

## 14.7 History/patterns

```text
/history
/history/readings
/history/tarot
/history/astrology
/history/numerology
/history/dreams
/history/patterns
```

## 14.8 Reports

```text
/reports
/reports/numerology
/reports/birth-chart
/reports/relationship
/reports/career
/reports/annual
/reports/spiritual-profile
/reports/:id
```

## 14.9 Billing

```text
/billing
/billing/plans
/billing/paywall
/billing/credits
/billing/purchases
/billing/manage
```

## 14.10 Profile/settings/privacy

```text
/profile
/profile/birth-profile
/profile/preferences
/profile/reader-persona
/profile/notifications
/profile/privacy
/profile/export
/profile/delete
/profile/security
/profile/about
```

## 14.11 Required system states for every major route

```text
loading
empty
offline
retryable-error
nonretryable-error
maintenance
entitlement-required
privacy-blocked
provider-unavailable
```

---

# 15. TODAY / HOME SCREEN — LOCKED UX

The Today screen is the primary re-entry surface.

## 15.1 Header

Required:

- time-aware greeting;
- preferred user name;
- short “energy today” line;
- small Mysta presence/avatar;
- notification control.

## 15.2 Daily constellation row

Four compact modules:

```text
Daily Tarot
Zodiac
Personal Day
Notable Transit
```

Each displays deterministic state and opens a detail screen.

## 15.3 Mysta’s Insight hero

Contains:

- Mysta portrait/illustration;
- “Mysta’s Insight”;
- one combined short insight;
- evidence-derived theme tags;
- CTA to explore the full reading.

## 15.4 Continue your journey

Contextual modules may include:

- unfinished reading;
- new report;
- recurring pattern;
- dream journal;
- compatibility profile;
- weekly/monthly reading.

## 15.5 Home visual rules

- dark celestial background;
- Mysta prominent once, not repeated excessively;
- gold used for hierarchy;
- generous spacing;
- important content before upsell;
- no dense SaaS dashboard grid.

---

# 16. READINGS HUB — LOCKED UX

Feature cards:

```text
Tarot
Numerology
Star Signs
Birth Chart
Transits
Compatibility
Dreams
Cross-System
```

Each has:

- original MystaAI icon;
- title;
- one-line description;
- entitlement indicator if needed;
- restrained domain accent.

---

# 17. TAROT UX — LOCKED

Flow:

```text
reading type
→ question
→ spread
→ optional reversals
→ draw ceremony
→ reveal
→ interpretation
→ evidence/history
```

Draw ceremony:

```text
deck appears
→ subtle shuffle
→ fan
→ user taps draw
→ server commits authoritative draw
→ face-down cards settle
→ user reveals
```

The animation cannot change the server draw.

Result contains:

- cards;
- spread positions;
- orientation;
- core meaning;
- Mysta interpretation;
- expandable “Why this interpretation?” evidence;
- save;
- ask Mysta deeper;
- explicit share action.

Tarot artwork is original or rights-cleared. Every asset is in `ASSET_MANIFEST.json`.

---

# 18. NUMEROLOGY UX — LOCKED

Main view includes:

- Life Path;
- Personal Day;
- Personal Month;
- Personal Year;
- Expression;
- Soul Urge;
- Personality;
- Maturity.

Visual style:

- large elegant numbers;
- celestial geometry;
- restrained gold line work;
- no casino/gambling aesthetic.

Detailed views show:

```text
calculation
traditional meaning
personal interpretation
related cycle
```

Users can inspect the calculation path.

---

# 19. ZODIAC UX — LOCKED

Screens include:

- Sun;
- Moon;
- Rising when available;
- daily;
- weekly;
- monthly;
- yearly themes;
- sign reference;
- compatibility entry.

Exact calculated solar position outranks approximate date tables.

---

# 20. ASTROLOGY / BIRTH CHART UX — LOCKED

MystaAI uses an original chart renderer.

Required layers:

- 12 signs;
- 12 houses;
- planet glyphs;
- aspect lines;
- Ascendant;
- MC.

Tabs:

```text
Overview
Planets
Houses
Aspects
Transits
Insights
```

Chart base:

```text
Midnight Void / Cosmic Navy
```

Primary chart lines/text:

```text
Ivory / Moon Gold
```

When birth time is unknown, Ascendant/houses are disabled or marked uncertain rather than fabricated.

---

# 21. TRANSITS UX — LOCKED

Shows:

- significant current transit;
- exact aspect;
- exact date/window;
- natal placement involved;
- traditional theme;
- Mysta interpretation.

Timeline:

```text
Now
Next 7 days
Next 30 days
```

No fake urgency.

---

# 22. COMPATIBILITY UX — LOCKED

A relationship profile stores an alias/name and birth details where available.

Result sections:

- communication;
- emotional;
- attraction;
- practical;
- strengths;
- friction themes;
- reflection prompts.

No arbitrary “soulmate percentage” at launch.

---

# 23. DREAM UX — LOCKED

New dream inputs:

- free text;
- optional title;
- date;
- emotions.

Interpretation shows:

- detected symbols;
- emotional themes;
- traditional possibilities;
- personal context;
- Mysta reflection;
- recurring-pattern links.

Dream art uses moonlit indigo/violet atmosphere without horror styling.

---

# 24. MYSTIC CHAT — LOCKED

Mystic Chat must not resemble a generic support chat.

Header:

```text
Mysta
state
conversation actions
```

States:

```text
Ready
Listening
Reflecting
Reading the cards
Checking your chart
Looking at your numbers
Responding
Unavailable
```

Mysta uses the approved portrait/avatar.

Quick-action chips:

```text
Love
Career
Tarot
Birth Chart
Dreams
Numerology
```

When deterministic tools execute, UI may show factual tool status such as:

```text
Drawing your cards…
Checking your chart…
Calculating your numbers…
```

Internal chain-of-thought is never exposed.

When psychology knowledge is relevant, Mystic Chat may show a restrained factual status such as:

```text
Checking evidence…
Reflecting on the pattern…
```

The final response may expose a user-readable **Evidence-informed reflection** disclosure with source/provenance links where appropriate. It must not imply that psychology evidence proves a mystical interpretation.

---

# 25. CROSS-SYSTEM UX — LOCKED

Cross-System is a signature premium experience.

Flow:

```text
question
→ selected domains or “Mysta choose”
→ deterministic calculations
→ knowledge retrieval
→ evidence summary
→ synthesis
```

Result visibly separates source domains:

```text
Tarot
Numerology
Astrology
Current Transits
Personal Patterns
```

When empirical psychology evidence is relevant, it is displayed separately as:

```text
Evidence-informed reflection
```

Psychology is not presented as another divination system and is never used to claim scientific validation of the mystical source cards.

Then displays:

```text
Mysta’s Synthesis
```

---

# 26. HISTORY / PATTERN UX — LOCKED

Views:

```text
Timeline
Cards
Themes
Dream Symbols
Astrology Events
Numerology Cycles
```

Examples of deterministic pattern claims:

- most drawn card;
- most frequent suit;
- repeated Major Arcana;
- recurring dream symbol;
- recurring theme;
- category frequency.

All counts come from database queries, never model invention.

---

# 27. REPORT UX — LOCKED

Reports use MystaAI editorial styling.

Cover:

- MystaAI wordmark;
- report title;
- preferred name/alias;
- generation date;
- celestial motif.

Content:

- Playfair Display headings;
- Inter body;
- readable layout;
- restrained gold;
- charts/diagrams;
- evidence appendix where appropriate.

Reports are viewable in-app and downloadable as PDF.

---

# 28. PAYWALL / BILLING UX — LOCKED

Paywalls are premium but non-manipulative.

Forbidden:

- fake countdowns;
- false scarcity;
- hidden close controls;
- fear-based spiritual messaging;
- misleading “unlimited” technical claims.

Plans:

```text
Free
Starter
Premium
Mystic Unlimited
```

Premium is the main value tier.

---

# 29. PROFILE / SETTINGS UX — LOCKED

Sections:

```text
Your Mystical Profile
Birth Details
Reader Persona
Reading Preferences
Notifications
Subscription
Credits
Purchases
Privacy
Security
Data Export
Delete Account
About MystaAI
```

Destructive actions require confirmation/re-authentication where appropriate.

---

# 30. COMPONENT LIBRARY — LOCKED

Create:

```text
packages/ui
```

Components:

```text
MystaButton
MystaIconButton
MystaCard
MystaPremiumCard
MystaInput
MystaTextArea
MystaSelect
MystaTabs
MystaSegmentedControl
MystaChip
MystaBadge
MystaBottomNav
MystaTopBar
MystaModal
MystaSheet
MystaToast
MystaSkeleton
MystaEmptyState
MystaErrorState
MystaAvatar
MystaTarotCard
MystaSpread
MystaChart
MystaNumberHero
MystaInsightCard
MystaEntitlementGate
MystaCreditBadge
MystaReportCard
```

Every component has:

- interaction states;
- accessibility labels;
- visual fixture;
- screenshot baseline;
- disabled/loading/error behaviour.

---

# 31. ICONOGRAPHY — LOCKED

MystaAI uses an original line-icon family.

Required symbols:

```text
Today
Readings
Mystic
Explore
Profile
Tarot
Astrology
Numerology
Zodiac
Dreams
Compatibility
Cross-System
Reports
Journal
Patterns
Moon
Sun
Star
Planet
Calendar
Notification
Settings
Privacy
Security
Credits
Premium
Back
Close
Share
Save
Search
Send
Microphone
```

Universal actions may follow platform conventions; domain/brand icons must be original.

---

# 32. ACCESSIBILITY — LOCKED

Target:

```text
WCAG 2.2 AA
```

Required:

- accessible contrast;
- Dynamic Type/font scaling;
- VoiceOver/TalkBack;
- keyboard navigation on web;
- logical focus order;
- 44×44 minimum touch target;
- reduced motion;
- chart textual alternatives;
- tarot-image descriptions;
- no meaning conveyed only by colour.

---

# 33. RESPONSIVE RULES

Web breakpoints:

```text
xs <480
sm 480–767
md 768–1023
lg 1024–1439
xl ≥1440
```

Desktop may use:

- persistent side navigation;
- wider reading canvas;
- side evidence panel;
- multi-column history.

Desktop must not simply stretch a mobile screen.

---

# 34. VISUAL ACCEPTANCE / REGRESSION

Every major screen has an approved screenshot fixture.

CI visual checks cover:

- mobile viewport;
- tablet viewport;
- desktop viewport;
- dark theme;
- text scaling;
- reduced motion where relevant.

A screen is not complete merely because it functions. It must also:

- match brand tokens;
- use correct spacing/typography;
- use approved Mysta representation;
- use correct navigation;
- pass accessibility;
- pass originality review.

---

# 35. LOCKED PRODUCTION STACK

The following versions were verified on 22 September 2026 where stated.

## 35.1 Runtime/tooling

```text
Node.js             24.21.0 LTS
ESLint              10.11.0
Prettier            3.9.8
Vitest              5.0.1
Playwright          1.63.0
```

Package manager: `pnpm`.

For `pnpm` and `Turborepo`, Phase 1 runs official registry lookup, selects the current stable non-prerelease version compatible with Node 24.21.0, writes it to `packageManager`/root dependencies with an exact version, and records it in `DEPENDENCY_MANIFEST.json` before any app package is installed. This is the only allowed selection procedure.

## 35.2 Web

```text
Next.js             16.3.6
React               19.3.0
React DOM           19.3.0
Tailwind CSS        4.3.3
Zod                 4.6.5
```

`Next.js 16.3.6` supersedes the previously discussed 16.3.3 because an out-of-band critical security update was published on 22 September 2026.

## 35.3 API

```text
Fastify             5.12.5
```

Required Fastify plugins are installed at the exact latest stable version compatible with Fastify 5.12.5 during the Phase 1 dependency preflight and immediately locked; no prerelease plugin is allowed.

Required plugins:

```text
@fastify/cors
@fastify/helmet
@fastify/cookie
@fastify/rate-limit
@fastify/swagger
@fastify/swagger-ui
@fastify/sensible
```

## 35.4 Database

Local/test relational engine:

```text
PostgreSQL          18.6
pgvector            0.8.6
Prisma              7.10.0
@prisma/client      7.10.0
@prisma/adapter-pg  7.10.0
```

Production relational engine at the v2.2 lock date:

```text
Amazon Aurora PostgreSQL-compatible  18.4.2
AWS-supported pgvector               0.8.2
Amazon RDS Proxy                     managed service
```

Aurora 18.4.2 was the current Aurora PostgreSQL 18.4 patch verified on 24 September 2026. Phase 0/1 rechecks the exact production patch before provisioning; no silent downgrade is allowed. Application SQL/schema features must remain compatible with the production Aurora version.

Prisma 8 is not used at the original lock date because it was release-candidate software; Phase 1 rechecks release status.

## 35.5 Durable queue/cache/routing

Production durable asynchronous work and routing use:

```text
Amazon SQS Standard queues   default durable queue
Amazon SQS FIFO queues       only where strict ordering/deduplication is required
Amazon ElastiCache Redis     cache/rate-limit/short-lived coordination
Aurora user_routing table    authoritative launch user→home_region/cell assignment
```

BullMQ/Redis is not the authoritative durable production job queue in v2.2. SQS Standard is at-least-once; every consumer is idempotent and every durable queue has retry, visibility timeout, DLQ, age/depth alarms and replay runbook.

No DynamoDB table is required for the v2.2 launch architecture. The routing abstraction remains isolated in `packages/routing`; if a later approved scale revision activates multiple independent cells, the backing routing store may be changed without changing user-facing contracts.

The exact stable AWS SDK packages required for SQS and CloudWatch are registry-verified during Phase 1, locked to exact versions and recorded in `DEPENDENCY_MANIFEST.json`.

## 35.6 Mobile

```text
Expo                57.0.22
React Native        0.86.3
React               19.2.3
```

Expo 58 is beta at lock date and is excluded.

Expo-managed native libraries are installed using `npx expo install` and exact resolved versions are frozen.

Required:

```text
expo-router
expo-dev-client
expo-notifications
expo-secure-store
expo-font
expo-linking
expo-constants
expo-application
expo-device
expo-haptics
expo-image
```

Real in-app purchase testing uses EAS development builds, not Expo Go.

## 35.7 Authentication

```text
better-auth         1.7.5
```

Methods:

- email/password;
- email verification;
- password reset;
- Google;
- Apple;
- admin MFA.

## 35.8 Billing

```text
react-native-purchases      10.10.1
react-native-purchases-ui   10.10.1
```

Stripe Node SDK is pinned to the current stable release at the billing-phase preflight and written to the manifest before billing code is authored. The API version used by the Stripe account is recorded separately in `INTEGRATION_MANIFEST.json`.

## 35.9 Observability/analytics

```text
@sentry/react-native       8.27.0
posthog-react-native       4.75.0
```

Corresponding web/server Sentry and PostHog SDKs are exact-version locked during Phase 1 compatibility preflight.

## 35.10 PDF/storage/email

```text
@react-pdf/renderer          4.9.0
@aws-sdk/client-s3           3.1137.0
@aws-sdk/s3-request-presigner 3.1136.0
@aws-sdk/client-sesv2        3.1136.0
```

Also required and exact-version frozen during Phase 1 after compatibility verification:

```text
@aws-sdk/client-sqs
@aws-sdk/client-cloudwatch
@aws-sdk/client-application-auto-scaling
```

## 35.11 Python privacy/knowledge services

```text
Python                  3.14.7
PyTorch                 2.14.0
Transformers            5.17.0
Sentence Transformers   6.1.0
Presidio Analyzer       2.2.364
Presidio Anonymizer     2.2.364
```

FastAPI/ASGI server versions are locked to the current stable Python-3.14-compatible releases during Phase 1 preflight and recorded before service implementation.

## 35.12 Astrology

```text
Swiss Ephemeris         v2.10.3bfinal
Release short SHA       f4dcd18
Licence                 Professional Edition
Professional unlimited  CHF 700 at lock date
```

## 35.13 Location/timezone

```text
GeoNames                Best Availability
Initial plan            1,000,000 credits/year
Price                   €250/year at lock date
timezonecomplete        5.15.1
tzdata package          1.0.51
IANA database           2026d
```

The exact compatible `geo-tz` stable version is registry-verified and frozen in Phase 1.

## 35.14 Security standards

```text
OWASP ASVS           5.0.0
OWASP MASVS          2.1.0
```

Mobile testing also follows the current OWASP MASWE catalogue referenced by MASVS at implementation time.

## 35.15 OpenAI SDK / AI transport

```text
openai               7.23.0
licence              Apache-2.0
API                  Responses API
base URL             https://api.openai.com/v1
provider model       gpt-5-mini
```

The official `openai` TypeScript/JavaScript package is the only production OpenAI client dependency. Feature packages never import it directly; only `packages/ai-gateway` may do so. Phase 1 rechecks that 7.23.0 remains available/non-yanked and has no launch-blocking advisory; any newer substitution requires the normal controlled dependency revision and frozen lockfile evidence.

---

# 36. MONOREPO — LOCKED

```text
apps/
  web/
  mobile/
  api/
  worker/
  admin/
  privacy-safety/
  knowledge-ml/

packages/
  ai-gateway/
  astrology/
  auth/
  billing/
  compatibility/
  config/
  database/
  design-tokens/
  dream/
  email/
  knowledge/
  logging/
  numerology/
  observability/
  notifications/
  reading-engine/
  safety/
  psychology/
  routing/
  scale/
  shared/
  swiss-ephemeris-native/
  tarot/
  timezone/
  ui/
  validation/
  zodiac/

assets/
  brand/
    logo/
    mysta/
    icons/
  tarot/
  zodiac/
  reports/

knowledge/
  psychology/
    raw/
    processed/
    manifests/
    schemas/
  raw/
  manifests/
  processed/
  schemas/

vendor/
  swisseph/

infra/
  terraform/
    edge/
    region/
    cell/
    modules/
  docker/
  scripts/

store/
  apple/
  google/
  revenuecat/
  stripe/

docs/
  architecture/
  api/
  brand/
  design/
  knowledge/
  operations/
  privacy/
  legal/
  runbooks/

tests/
  scale/
    component/
    cell/
    single-region/
    soak/
    spike/
    failure/
    restore/
    quota/
  acceptance/
  fixtures/
  performance/
  security/
  ai-evals/
  visual/
```

Empty production packages are prohibited.

---

# 37. SERVICE TOPOLOGY — SINGLE-REGION / CELL-READY

Local defaults:

```text
web               3000
api               3001
admin             3002
privacy-safety    8101
knowledge-ml      8102
PostgreSQL        5432
cache Redis       6379
```

Production has an edge plane and one authoritative application/data plane.

## 37.1 Global web edge

```text
DNS:              Route 53
web/static:       CloudFront → web origin
security:         AWS WAF on public entry points
```

CloudFront is used for global web/static delivery. It does not replicate the authoritative MystaAI application database.

## 37.2 Single-region application/data plane

```text
API traffic:      Route 53 → regional ALB (`eu-west-2`) → ECS Fargate API
workers:          ECS Fargate workers
privacy/safety:   private ECS service
knowledge:        private ECS service
database:         RDS Proxy → Aurora PostgreSQL writer/readers
cache:            ElastiCache Redis
async:            SQS + DLQs
object storage:   S3
routing metadata: Aurora `user_routing` table
```

The launch backend is authoritative in `eu-west-2`. No Global Accelerator, DynamoDB Global Tables, cross-Region database replication or second authoritative Region is required for v2.2 launch.

## 37.3 Routing abstraction

`packages/routing` exposes one contract for resolving `user_id → home_region/cell_id/routing_version`. The v2.2 implementation uses Aurora. Launch values are `home_region=eu-west-2` and `cell_id=cell-001` unless an approved later specification revision activates additional cells.

## 37.4 Scale invariant

MystaAI must support the locked 2–3M-user service envelope without product redesign. Within that envelope, capacity grows by adding ECS tasks, Aurora reader capacity/instance size, RDS Proxy capacity, Redis capacity, SQS consumers and provider quota. A later second cell/Region is an expansion step, not a launch dependency.

---

# 38. READING STATE MACHINE

```text
REQUESTED
→ CALCULATING
→ RETRIEVING_KNOWLEDGE
→ PRIVACY_CHECK
→ GENERATING
→ VALIDATING
→ COMPLETE
```

Failure states:

```text
FAILED_INPUT
FAILED_CALCULATION
FAILED_KNOWLEDGE
FAILED_PRIVACY_POLICY
FAILED_AI
FAILED_VALIDATION
FAILED_PERSISTENCE
```

The client must render the real state. Failure may not be visually presented as completion.

---

# 39. TAROT ENGINE — LOCKED

## 39.1 Deck model

Canonical 78-card structure:

- 22 Major Arcana;
- 56 Minor Arcana;
- Wands;
- Cups;
- Swords;
- Pentacles;
- Ace–10;
- Page;
- Knight;
- Queen;
- King.

IDs:

```text
major_00_fool
major_01_magician
...
major_21_world

wands_ace
wands_02
...
pentacles_king
```

## 39.2 Card schema

```text
id
canonical_name
arcana
suit
rank
number
upright_keywords[]
reversed_keywords[]
upright_core_meaning
reversed_core_meaning
symbolism[]
visual_elements[]
element
astrological_correspondence[]
numerological_correspondence[]
archetypes[]
love_meaning
career_meaning
money_meaning
family_meaning
spiritual_meaning
decision_meaning
advice_meaning
warning_meaning
past_position_meaning
present_position_meaning
future_position_meaning
obstacle_position_meaning
outcome_position_meaning
source_ids[]
content_version
```

## 39.3 Authoritative draw

Server only.

Algorithm:

```text
Fisher–Yates shuffle
for i from n - 1 down to 1:
  j = crypto.randomInt(0, i + 1)
  swap(deck[i], deck[j])
```

Forbidden:

```text
Math.random()
LLM card selection
timestamp-seeded pseudo-draw
client-only authoritative draw
```

## 39.4 Reversals

Default:

```text
OFF
```

If enabled:

```text
crypto.randomInt(0, 2)
```

for each drawn card independently.

## 39.5 Launch spreads

```text
single_insight
past_present_future
situation_action_outcome
mind_body_spirit
love_five
relationship_five
career_five
money_five
decision_five
challenge_advice_outcome
celtic_cross_v1
custom_question_three
```

Each spread is versioned and defines:

- number of cards;
- ordered positions;
- position names;
- position interpretive role;
- allowed reading categories.

## 39.6 Persisted draw evidence

```text
reading_id
spread_id
spread_version
position_index
position_key
card_id
orientation
rng_method
created_at
```

Immutable after transaction commit.

## 39.7 Replay rule

Viewing an old reading retrieves the stored draw; it never re-draws cards.

## 39.8 Tarot acceptance

- exactly 78 unique active cards;
- exactly 22 Major Arcana;
- exactly 56 Minor Arcana;
- no duplicate draw inside a spread;
- spread count matches schema;
- persisted draw equals displayed draw;
- replay equals original draw;
- AI cannot substitute another card;
- reversal statement is impossible without reversed evidence.

---

# 40. NUMEROLOGY ENGINE — LOCKED

Tradition:

```text
PYTHAGOREAN_V1
```

## 40.1 Letter mapping

```text
1 A J S
2 B K T
3 C L U
4 D M V
5 E N W
6 F O X
7 G P Y
8 H Q Z
9 I R
```

## 40.2 Name normalisation

1. Unicode NFKD.
2. Remove combining marks.
3. Uppercase.
4. Use A–Z for arithmetic.
5. Preserve the original supplied name separately.
6. Apostrophes, spaces and hyphens are ignored mathematically but retained for display.
7. Unsupported scripts require explicit transliteration input or an approved deterministic transliteration module.
8. AI may not invent transliteration.
9. Y is consonantal by default unless a later version introduces a documented phonetic rule set.

## 40.3 Reduction functions

```text
reduceMaster(n):
  while n > 9:
    if n in [11, 22, 33]: return n
    n = sumDigits(n)
  return n
```

```text
reduceDigit(n):
  while n > 9:
    n = sumDigits(n)
  return n
```

## 40.4 Production calculations

- Life Path;
- Birthday Number;
- Expression/Destiny;
- Soul Urge/Heart’s Desire;
- Personality;
- Maturity;
- Personal Year;
- Personal Month;
- Personal Day;
- Pinnacles;
- Challenges;
- Karmic Debt;
- Karmic Lessons;
- Hidden Passion;
- Balance;
- Cornerstone;
- Capstone;
- First Vowel.

## 40.5 Life Path

```text
monthPart = reduceMaster(monthNumber)
dayPart   = reduceMaster(dayNumber)
yearPart  = reduceMaster(sumDigits(fourDigitYear))
lifePath  = reduceMaster(monthPart + dayPart + yearPart)
```

## 40.6 Personal cycles

```text
Personal Year  = reduceDigit(month + day + sumDigits(calendarYear))
Personal Month = reduceDigit(personalYear + calendarMonth)
Personal Day   = reduceDigit(personalMonth + calendarDay)
```

## 40.7 Pinnacles

```text
P1 = reduceMaster(month + day)
P2 = reduceMaster(day + yearPart)
P3 = reduceMaster(P1 + P2)
P4 = reduceMaster(month + yearPart)
```

## 40.8 Challenges

```text
C1 = abs(reduceDigit(day) - reduceDigit(month))
C2 = abs(reduceDigit(yearPart) - reduceDigit(day))
C3 = abs(C1 - C2)
C4 = abs(reduceDigit(yearPart) - reduceDigit(month))
```

## 40.9 Name-number functions

Expression uses all mapped letters.

Soul Urge uses vowels:

```text
A E I O U
```

Y is excluded in V1.

Personality uses consonants.

Maturity:

```text
reduceMaster(lifePath + expression)
```

## 40.10 Tests

Fixtures must cover:

- ordinary single-digit outputs;
- 11;
- 22;
- 33;
- Karmic Debt 13/14/16/19;
- accented Latin names;
- apostrophes;
- hyphens;
- leap-day birthdays;
- transliteration-required names.

Calculation engine target:

```text
100% branch coverage
```

---

# 41. ZODIAC ENGINE — LOCKED

Signs:

```text
Aries
Taurus
Gemini
Cancer
Leo
Virgo
Libra
Scorpio
Sagittarius
Capricorn
Aquarius
Pisces
```

When accurate birth data exists, actual solar longitude is authoritative.

```text
normalized = ((longitude % 360) + 360) % 360
signIndex = floor(normalized / 30)
degreeInSign = normalized % 30
```

Knowledge per sign includes:

- element;
- modality;
- ruler;
- polarity;
- themes;
- strengths;
- challenges;
- love;
- friendship;
- communication;
- work;
- money;
- family;
- compatibility;
- decans;
- planet-in-sign interpretations.

Approximate date tables may be used only for lightweight unauthenticated marketing content, never as authoritative profile calculation when ephemeris calculation is available.

---

# 42. ASTROLOGY ENGINE — LOCKED

## 42.1 Production engine

Use **Swiss Ephemeris Professional Edition**.

Commercial closed-source launch is blocked until the required commercial licence is purchased and archived.

## 42.2 Source lock

```text
Repository: aloistr/swisseph
Tag:        v2.10.3bfinal
Short SHA:  f4dcd18
```

Before vendoring, record:

- full commit SHA;
- every vendored file SHA-256;
- licence text;
- commercial agreement reference;
- acquisition date.

## 42.3 Native wrapper

Create:

```text
packages/swiss-ephemeris-native
```

Use Node-API rather than an uncontrolled third-party astrology wrapper.

Expose only typed functions required by MystaAI, including wrappers around:

```text
swe_set_ephe_path
swe_julday
swe_calc_ut
swe_houses_ex2
swe_close
swe_get_planet_name
```

Native build dependencies are exact-version locked in `DEPENDENCY_MANIFEST.json` before implementation.

## 42.4 Production convention

```text
Western tropical
Geocentric
Swiss Ephemeris
SEFLG_SWIEPH
SEFLG_SPEED
True Node default
South Node = North Node + 180° normalized
Placidus default
Whole Sign optional user setting
```

Launch bodies:

- Sun;
- Moon;
- Mercury;
- Venus;
- Mars;
- Jupiter;
- Saturn;
- Uranus;
- Neptune;
- Pluto;
- True Node;
- South Node.

Deferred unless a future spec activates them:

- Chiron;
- Lilith;
- asteroids.

## 42.5 Retrograde

```text
retrograde = longitudeSpeed < 0
```

## 42.6 Location/time pipeline

```text
birth-place text
→ GeoNames
→ latitude / longitude
→ IANA timezone resolution
→ historical timezone conversion
→ UTC birth instant
→ Julian Day UT
→ Swiss Ephemeris
```

Raw user-entered location and resolved canonical place are stored separately.

## 42.7 DST ambiguity

Fall-back ambiguous local time:

```text
calculate both UTC candidates
→ ask user when possible
→ otherwise mark AMBIGUOUS_DST
```

Nonexistent spring-forward local time:

```text
reject value
→ explain invalid civil time
→ request correction
```

MystaAI never silently shifts a birth time.

## 42.8 Birth-time confidence

```text
EXACT
APPROXIMATE
UNKNOWN
AMBIGUOUS_DST
```

`UNKNOWN` disables or marks unavailable:

- Ascendant;
- MC;
- houses;
- house-dependent interpretations.

Moon certainty must be qualified where a date spans a sign transition and time is unknown.

## 42.9 Aspects

| Aspect | Exact angle | Base orb |
|---|---:|---:|
| Conjunction | 0° | 8° |
| Opposition | 180° | 8° |
| Trine | 120° | 7° |
| Square | 90° | 7° |
| Sextile | 60° | 5° |
| Quincunx | 150° | 3° |
| Semi-sextile | 30° | 2° |
| Semi-square | 45° | 2° |
| Sesquiquadrate | 135° | 2° |

If Sun or Moon participates:

```text
+1° up to a maximum 9°
```

Angular separation:

```text
d = abs(a - b) % 360
separation = min(d, 360 - d)
```

If more than one aspect could match an edge case, choose the aspect with the lowest normalised angular error. Exact tie priority:

```text
conjunction > opposition > square > trine > sextile > quincunx > semi-square > sesquiquadrate > semi-sextile
```

## 42.10 Launch chart types

- natal;
- current transits;
- transit-to-natal aspects;
- synastry;
- solar return.

Deferred:

- secondary progressions;
- composite charts.

## 42.11 Independent validation

Primary conformance oracle:

```text
official swetest built from the exact vendored source/tag
```

Wrapper tolerance:

```text
±0.000001°
```

Independent astronomical cross-check:

```text
NASA/JPL Horizons
Sun–Pluto: ±0.01°
Moon:      ±0.02°
```

Houses/Ascendant are validated against `swetest`, not JPL.

If the engine is unavailable:

```text
ASTROLOGY_UNAVAILABLE
```

AI must not estimate positions.

---

# 43. COMPATIBILITY ENGINE — LOCKED

Inputs can include:

- Sun;
- Moon;
- Ascendant;
- Venus;
- Mars;
- synastry aspects;
- house overlays when birth times permit;
- Life Path;
- Expression;
- Soul Urge;
- user-selected relationship context;
- optional tarot relationship spread.

Output sections:

```text
communication themes
emotional themes
attraction themes
practical themes
strengths
friction themes
reflection questions
```

No arbitrary numeric compatibility percentage at launch.

If a scoring model is introduced later, it requires its own deterministic versioned specification and validation data.

---

# 44. DREAM ENGINE — LOCKED

Dream record:

```text
id
user_id
occurred_at
title
raw_text_encrypted
sanitised_text
emotions[]
symbols[]
themes[]
interpretation_id
created_at
```

Pipeline:

```text
dream text
→ privacy gateway
→ symbol extraction
→ emotion extraction
→ knowledge retrieval
→ previous-dream pattern lookup
→ OpenAI (GPT-5 mini) interpretation
→ truth/safety validation
→ save
```

MystaAI does not present dream symbolism as medical diagnosis or universal objective fact.

Pattern engine deterministically calculates:

- recurring symbols;
- recurring locations/setting tags;
- recurring emotional themes;
- recurring relationship-role placeholders;
- frequency over time.

---

# 45. DAILY / WEEKLY / MONTHLY / ANNUAL ENGINE

## Daily

Combines:

```text
daily tarot
Personal Day
Sun-sign context
significant current transits
recent deterministic patterns
```

## Weekly

Combines:

- transit window;
- zodiac themes;
- numerology cycle;
- optional weekly tarot spread.

## Monthly

Combines:

- Personal Month;
- significant transit events;
- monthly tarot;
- reading-history patterns.

## Annual

Combines:

- Personal Year;
- solar return;
- major transits;
- annual tarot spread;
- major historical themes.

Annual long-form output is a premium report.

---

# 46. MYSTIC KNOWLEDGE CORE — LOCKED

Architecture:

```text
SOURCE
 ↓
LICENCE GATE
 ↓
IMMUTABLE RAW ARCHIVE
 ↓
NORMALISATION
 ↓
STRUCTURED EXTRACTION
 ↓
VALIDATION
 ↓
KNOWLEDGE RECORDS
 ↓
EMBEDDINGS
 ↓
VECTOR INDEX
 ↓
KNOWLEDGE GRAPH
 ↓
PUBLISHED KNOWLEDGE VERSION
```

Required domains:

```text
tarot.cards
tarot.symbolism
tarot.spreads
tarot.history
tarot.combinations

numerology.pythagorean
numerology.calculations
numerology.meanings
numerology.cycles
numerology.compatibility

zodiac.signs
zodiac.elements
zodiac.modalities
zodiac.relationships

astrology.planets
astrology.signs
astrology.houses
astrology.aspects
astrology.transits
astrology.synastry
astrology.solar_return

dream.symbols
dream.frameworks

psychology.emotion
psychology.emotion_regulation
psychology.motivation
psychology.habits
psychology.behaviour_change
psychology.decision_making
psychology.cognitive_biases
psychology.communication
psychology.conflict
psychology.relationship_dynamics
psychology.attachment_concepts
psychology.social_psychology
psychology.personality_research
psychology.stress_coping
psychology.grief_transitions
psychology.goals_values
psychology.uncertainty
psychology.reflective_questioning

reading.methodology
compatibility.rules
safety.boundaries
```

---


Psychology domain rules:

- psychology records are evidence records, not divination meanings;
- every psychology retrieval result carries evidence grade, population/context, limitations and source IDs;
- Mysta may use psychology to improve reflection, questions, communication and behavioural framing;
- psychology may never be used to prove a mystical claim, diagnose a user or create covert vulnerability targeting.

# 47. KNOWLEDGE SOURCE POLICY — LOCKED

Source classes:

```text
A = authoritative factual source
B = public-domain historical primary source
C = licensed reference
D = original MystaAI-authored knowledge
E = prohibited / unverified
```

Class E never publishes.

Source manifest fields:

```text
source_id
title
author
publisher
source_type
domain
tradition
publication_year
jurisdiction
copyright_status
licence
commercial_use_allowed
ai_use_allowed
evidence_grade
study_type
population_context
known_limitations
causal_status
retraction_status
correction_status
source_class
source_url
retrieved_at
sha256
raw_storage_key
parser_version
knowledge_version
notes
```

Publication requires:

```text
licence approved
AND source hash recorded
AND parser passes
AND schema passes
AND provenance exists
AND content validation passes
AND psychology evidence manifest passes when domain=psychology
```

## 47.1 Tarot sourcing

Historical base may include a verified public-domain/rights-cleared edition of:

```text
A. E. Waite — The Pictorial Key to the Tarot
```

Modern tarot applications/books may be researched but their proprietary wording is not copied into the product.

Production interpretation copy is:

- public-domain material where legally usable;
- licensed content where explicitly licensed;
- original MystaAI-authored content.

## 47.2 Numerology sourcing

The arithmetic formulas are owned by this specification.

Interpretive language is original MystaAI-authored content or separately licensed.

## 47.3 Astrology sourcing

Astronomical/calculation facts originate from:

- Swiss Ephemeris;
- JPL validation;
- GeoNames;
- IANA timezone data.

Interpretive language is original MystaAI-authored or separately licensed.

## 47.4 Dream sourcing

No unlicensed modern “dream dictionary” is scraped.

Dream records are structured as:

```text
traditional_symbolic_theme
possible_personal_association
emotional_context
common_metaphorical_reading
reflection_questions
```

---

## 47.5 Human psychology sourcing — v2.2 hard lock

Purpose: provide Mysta with evidence-informed general psychology/behavioural knowledge that improves reflection, communication and conversational usefulness without turning MystaAI into a clinical product.

Approved initial source classes:

1. **NIMH public-domain text** — approved where the NIMH page/publication carries the public-domain reuse policy. Attribution is retained. NIMH images are not copied. No wording implies NIMH endorsement or individual medical advice.
2. **PLOS article content under CC BY 4.0 or equally permissive commercial-use terms** — approved with article-level attribution/provenance. Third-party material inside an article is excluded unless its own rights are compatible.
3. **PMC Open Access Subset** — conditional. Automated retrieval uses only PMC-approved services. Each article's licence is validated individually; only licences allowing MystaAI's commercial reuse are ingested.
4. **Crossref metadata** — approved for bibliographic/DOI/licence/post-publication metadata. Abstract text is not assumed reusable merely because Crossref exposes it.
5. **Original MystaAI summaries** — approved only when written from approved evidence sources, reviewed, provenance-linked and non-derivative beyond permitted rights.

Blocked unless explicit written commercial/AI permission is archived:

- OpenStax Psychology 2e;
- APA copyrighted books, tests, scales or proprietary content;
- commercial psychology/therapy websites;
- unlicensed books;
- random blogs/social posts;
- scraped proprietary databases;
- proprietary psychometric instruments.

OpenStax Psychology 2e is specifically blocked because its current terms are non-commercial and require prior written permission for LLM/generative-AI ingestion.

Psychology source publication requires **both** scientific/evidence approval and rights approval. Scientific quality never overrides copyright/licensing; licensing never upgrades weak science.

### 47.5.1 Evidence grades

```text
A = authoritative public-health synthesis, systematic review, meta-analysis, guideline-level evidence or equivalent high-quality synthesis
B = multiple convergent peer-reviewed approved sources
C = one peer-reviewed approved study with suitable methodology
D = exploratory/early/limited evidence; must be explicitly qualified
```

Every psychology evidence record stores:

```text
knowledge_id
domain
subdomain
proposition
evidence_grade
study_type
population_context
known_limitations
causal_status
source_ids[]
doi[]
licence
commercial_use_allowed
ai_use_allowed
retraction_status
correction_status
source_hash
knowledge_version
reviewed_at
prohibited_use_tags[]
```

### 47.5.2 Retraction/correction control

A supporting source that becomes retracted, materially corrected, licence-revoked or superseded enters `REVIEW_REQUIRED`. Affected evidence is removed from active retrieval until re-reviewed. Crossref/post-publication metadata is used as one signal; source-level verification remains authoritative.

### 47.5.3 Prohibited psychology uses

Psychology knowledge may not be used to:

- diagnose a mental disorder;
- diagnose or assert trauma;
- infer a personality disorder;
- present an attachment style as a clinical diagnosis;
- claim certainty about a user's subconscious state;
- infer psychological vulnerability for sales;
- alter pricing/paywall pressure based on vulnerability;
- target fear, grief or crisis states for monetisation.

---

# 48. KNOWLEDGE INGESTION — LOCKED

Pipeline:

```text
source acquisition
→ SHA-256
→ licence validation
→ psychology evidence/retraction validation when domain=psychology
→ immutable raw archive
→ parser
→ normalisation
→ structured extraction
→ semantic segmentation
→ human/rule validation
→ embedding
→ graph edge generation
→ automated tests
→ publish knowledge version
```

Failed licence, rights, retraction, evidence-quality or validation checks prevent publication.

Knowledge version format:

```text
mysta-knowledge-MAJOR.MINOR.PATCH
```

A new published version never silently mutates the evidence behind old readings.

---

# 49. VECTOR RETRIEVAL — LOCKED

Primary store:

```text
PostgreSQL + pgvector
```

No separate vector database at launch.

Embedding and reranker models:

```text
BAAI/bge-m3
BAAI/bge-reranker-v2-m3
```

Before model files enter production they require:

- exact Hugging Face revision pin;
- model-file SHA;
- licence/commercial-use review;
- security scan;
- retrieval evaluation;
- `MODEL_MANIFEST.json` entry.

Narrative chunking:

```text
target: 450 tokens
minimum: 150
maximum: 700
overlap: 80
```

Structured card/sign/planet records are not arbitrarily split.

Retrieval flow:

```text
query
→ domain filter
→ tradition/evidence-class filter
→ evidence-grade filter when domain=psychology
→ active knowledge version
→ vector retrieval top 30
→ lexical retrieval
→ merge/dedupe
→ rerank
→ top 8 context records
```

Vector records contain:

```text
chunk_id
source_id
domain
tradition
evidence_grade
population_context
known_limitations
text
embedding
metadata
content_version
```

---

# 50. KNOWLEDGE GRAPH — LOCKED

Node types:

```text
TarotCard
Concept
Number
ZodiacSign
Planet
Element
Modality
House
Aspect
DreamSymbol
Theme
PsychologyConcept
BehaviouralMechanism
EvidenceClaim
EvidencePopulation
```

Example edges:

```text
Emperor → associated_with → structure
Emperor → traditional_correspondence → Aries
Aries → ruler → Mars
Aries → element → Fire
Aries → modality → Cardinal
```

Psychology graph edges additionally preserve evidence grade, population/context and limitation metadata. Mystical/traditional correspondence edges are never promoted into empirical psychology evidence merely because they share a concept label.

Every edge stores:

```text
source_id
knowledge_version
confidence_class
created_at
```

Unsupported correspondence is not inferred into permanent graph state merely because an LLM mentions it.

---

# 51. PRIVACY-SAFETY GATE — LOCKED

OpenAI (GPT-5 mini) is the launch LLM provider, but raw personal data is not sent unrestricted.

Create:

```text
apps/privacy-safety
```

Responsibilities:

- PII detection;
- pseudonymisation;
- sensitive-domain classification;
- high-stakes classification;
- provider-send/provider-block decision;
- prompt minimisation.

Core local dependencies:

```text
Python                3.14.7
Presidio Analyzer     2.2.364
Presidio Anonymizer   2.2.364
```

Optional local ML classifiers use only pinned, reviewed models.

## 51.1 Default provider minimisation

Raw values normally withheld from OpenAI (GPT-5 mini):

- legal name;
- email;
- phone;
- postal address;
- payment data;
- precise current location;
- raw birth location where derived chart data is sufficient;
- raw birth date/time where derived chart data is sufficient;
- unnecessary health/legal/political/religious/sexual information;
- inferred mental-health/trauma/personality/vulnerability labels.

Preferred model packet:

```text
PERSON_A
Sun = Scorpio
Moon = Capricorn
Ascendant = Virgo
Life Path = 8
Personal Year = 1
Cards = [...]
Relevant approved knowledge = [...]
```

## 51.2 User free-text

Mystic and dream text passes through:

```text
PII detection
→ sensitive-category classification
→ placeholder substitution
→ provider-policy decision
```

## 51.3 Psychology privacy boundary

MystaAI does not create a hidden clinical or vulnerability profile. The following may not be persisted as ordinary user facts merely because the AI inferred them:

```text
mental disorder
trauma
personality disorder
addiction
suicidality
attachment diagnosis
psychological vulnerability
```

User-confirmed goals/preferences and explicitly saved reflections may be stored under the normal user-controlled memory rules. Derived conversational themes remain marked as derived and deletable.

## 51.4 UK international-transfer gate

Before public launch involving UK personal data and a non-UK processor:

- map data flow;
- identify controller/processor roles;
- review contractual terms;
- identify transfer mechanism;
- complete required transfer assessment;
- determine DPIA requirement;
- update privacy notice;
- record legal approval.

This is a hard launch gate.

---

# 52. OPENAI GPT-5 MINI AI GATEWAY — LOCKED

Provider:

```text
OpenAI
```

Provider model:

```text
gpt-5-mini
```

Provider endpoint:

```text
POST https://api.openai.com/v1/responses
```

Create:

```text
packages/ai-gateway
```

No feature package calls OpenAI directly. All model traffic goes through the provider-independent gateway.

## 52.1 OpenAI production request contract

Default request controls:

```text
API: Responses API
model: gpt-5-mini
store: false
stream: true for interactive Mystic/readings where UX benefits
structured output: strict JSON Schema
function/tool schemas: strict=true
```

The gateway rejects direct use of provider-generated persistent conversation state. MystaAI owns conversation/history state in its own database and sends the minimum required context for each request.

`store:false` is mandatory unless a later privacy-approved specification revision explicitly changes it. OpenAI Zero Data Retention/Modified Abuse Monitoring eligibility is reviewed before launch but is not falsely assumed merely because `store:false` is set.

## 52.2 Internal model aliases

```text
mysta-fast  → gpt-5-mini, reasoning.effort=minimal
mysta-deep  → gpt-5-mini, reasoning.effort=medium
```

`mysta-fast` is used for:

- daily insight;
- short tarot interpretation;
- ordinary Mystic reply;
- concise numerology/zodiac explanation;
- low-complexity rewriting/summarisation.

`mysta-deep` is used for:

- complex cross-system reading;
- long compatibility reading;
- premium report sections;
- difficult multi-domain synthesis.

Reasoning effort may only change after the AI evaluation suite proves quality/latency/cost impact and `MODEL_MANIFEST.json` is updated.

## 52.3 AI responsibilities

OpenAI GPT-5 mini may perform:

- natural-language tarot interpretation;
- numerology explanation;
- astrology explanation;
- cross-system narrative synthesis;
- dream interpretation;
- Mystic conversation;
- premium report composition;
- summarisation;
- tone/persona rendering;
- evidence-informed general psychology explanation/reflection using approved retrieved evidence.

## 52.4 AI prohibited responsibilities

OpenAI GPT-5 mini must never independently:

- draw authoritative tarot cards;
- calculate authoritative numerology;
- calculate planetary positions;
- calculate houses;
- calculate aspects;
- invent user history;
- invent entitlements;
- invent source provenance;
- override deterministic results;
- claim a failed tool succeeded;
- diagnose a user;
- assert hidden trauma/mental disorder/personality disorder;
- use psychology as proof that a mystical interpretation is scientifically true.

## 52.5 Structured output/tool contract

Every model-generated reading uses the Section 55 schema through OpenAI Structured Outputs. Tool calling uses strict function schemas. `additionalProperties:false` is used for strict tool objects and all required properties are declared according to current OpenAI schema rules.

Malformed, refused, incomplete or schema-invalid outputs are surfaced to the gateway and handled by the validator/retry policy; they are never silently coerced into a successful reading.

## 52.6 Timeouts

```text
short generation       60s
deep reading           180s
report section         240s
full report            asynchronous MystaAI SQS job
```

## 52.7 Retries and throttling

Retry only:

```text
429
5xx
network reset
timeout
```

Maximum:

```text
2 retries
```

Rules:

- obey `Retry-After` when present;
- otherwise exponential backoff with jitter;
- first fallback delay approximately 1s;
- second fallback delay approximately 4s;
- never retry ordinary authentication/validation 4xx errors;
- a provider-capacity failure becomes `CAPACITY_EXHAUSTED`/`FAILED_AI`, never fabricated success.

## 52.8 Usage ledger

Every call records:

```text
provider
resolved_model
model_alias
reasoning_effort
input_tokens
output_tokens
cached_input_tokens
latency_ms
estimated_cost
feature
user_id
reading_id
success
error_code
created_at
```

## 52.9 Current lock-date commercial/capacity baseline

At the 25 September 2026 lock date, the official GPT-5 mini model page lists a baseline token price of USD $0.25 / 1M input tokens, $0.025 / 1M cached input tokens and $2.00 / 1M output tokens, and published usage-tier rate limits up to Tier 5. These values are planning inputs only: Phase 0/1 records the account's actual granted RPM/TPM/batch limits and current prices in `MODEL_MANIFEST.json` / `SERVICE_QUOTA_MANIFEST.json`.

MystaAI does not assume published maximums have been granted to its OpenAI project.

## 52.10 Model change gate

Any change to `gpt-5-mini`, provider behavior, model alias, reasoning effort or provider requires:

- AI regression suite;
- factual-fidelity suite;
- psychology/safety suite;
- structured-output/tool-call suite;
- latency comparison;
- cost comparison;
- updated `MODEL_MANIFEST.json`;
- explicit approval.

---

# 53. AI TOOL REGISTRY — LOCKED

Mysta can call only registered tools:

```text
draw_tarot
calculate_numerology
calculate_natal_chart
calculate_transits
calculate_synastry
calculate_solar_return
retrieve_knowledge
retrieve_user_profile
retrieve_reading_history
retrieve_patterns
retrieve_psychology_knowledge
retrieve_psychology_evidence
create_reading
save_user_note
```

Every tool defines:

- Zod input schema;
- Zod output schema;
- permission requirement;
- timeout;
- failure codes;
- audit event.

Unregistered tool invocation is rejected.

Explicitly prohibited tool capabilities:

```text
diagnose_user
infer_mental_disorder
infer_trauma
infer_psychological_vulnerability
target_vulnerability_for_sales
```

---

# 54. CROSS-SYSTEM SYNTHESIS — LOCKED

Pipeline:

```text
1 classify request
2 select required deterministic engines
3 execute calculations
4 collect validated outputs
5 map concepts
6 retrieve graph correspondences
7 retrieve source knowledge
8 retrieve approved psychology evidence when relevant
9 calculate repeated personal themes
10 identify disagreements between systems
11 build evidence packet with separate mystical/psychology namespaces
12 privacy sanitise
13 send to OpenAI
14 validate claims
15 persist evidence
```

Evidence weighting:

```text
Tier 1 deterministic facts/calculations
Tier 2 deterministic user-history patterns
Tier 3 approved empirical psychology evidence
Tier 4 direct traditional meanings
Tier 5 documented traditional graph correspondences
Tier 6 AI synthesis
```

Lower tiers cannot contradict higher tiers.

The system must not force multiple traditions to agree. Empirical psychology evidence is carried in a separate namespace and must not be described as scientific validation of divination.

---

# 55. STRUCTURED AI OUTPUT — LOCKED

Internal schema:

```text
ReadingNarrative {
  title
  summary
  sections[]
  themes[]
  reflectiveQuestions[]
  claims[]
  sourceKnowledgeIds[]
  safetyClass
}
```

Each claim:

```text
Claim {
  text
  class: FACT | CALCULATION | EVIDENCE | TRADITION | SYNTHESIS
  evidenceIds[]
}
```

Rules:

- FACT requires factual evidence;
- CALCULATION requires deterministic calculation evidence;
- EVIDENCE requires approved psychology evidence IDs and evidence-grade/context metadata;
- TRADITION requires approved knowledge IDs;
- SYNTHESIS requires supporting evidence IDs.

Malformed output is rejected.

---

# 56. TRUTH / EVIDENCE / TRADITION VALIDATOR — LOCKED

Validation sequence:

```text
schema
→ deterministic-value validation
→ tarot validation
→ numerology validation
→ astrology validation
→ history validation
→ psychology evidence/grade/population validation
→ provenance/retraction validation
→ unsupported-claim scan
→ safety validation
```

Example:

```text
Stored: The Star
Generated: “You drew The Moon.”
Result: REJECT
```

```text
Stored Life Path: 8
Generated: “Your Life Path is 7.”
Result: REJECT
```

First failure:

```text
regenerate once with explicit correction packet
```

Second failure:

```text
FAILED_VALIDATION
```

No infinite retry loop. Psychology claims that diagnose, over-generalise beyond the cited population, confuse correlation with causation, rely on inactive/retracted evidence or claim mystical validation are rejected.

---

# 57. SAFETY ENGINE — LOCKED

Categories:

```text
MEDICAL
MENTAL_HEALTH_CRISIS
PSYCHOLOGICAL_DIAGNOSIS
SELF_HARM
PREGNANCY
DEATH
LEGAL
FINANCIAL
GAMBLING
ABUSE
CRIMINAL_ALLEGATION
MISSING_PERSON
```

Safety rules are evaluated independently of Mysta’s persona.

Mystical reflection and general psychology may continue only when they do not create high-stakes certainty, diagnosis or unsafe advice. Crisis/self-harm handling outranks persona, psychology retrieval and mystical interpretation.

---

# 58. READER PERSONAS — LOCKED

Launch personas:

```text
The Oracle
The Guide
The Straight Talker
The Romantic
The Astrologer
The Numerologist
```

Persona controls:

- tone;
- vocabulary;
- mystical intensity;
- directness;
- verbosity.

Persona cannot alter:

- card draw;
- calculation;
- evidence;
- safety;
- entitlement;
- factual state.

Mysta remains the visual identity across personas; personas are communication modes, not separate characters.

---

# 59. PERSONAL MYSTICAL PROFILE — LOCKED

Stores:

```text
preferred_name
birth_name
current_name
birth_date
birth_time
birth_place_text
birth_place_canonical
latitude
longitude
timezone
birth_time_confidence
sun_sign
moon_sign
ascendant
numerology_profile_version
reader_persona
reversal_preference
reading_preferences
```

Sensitive birth fields are encrypted at rest with application-level field encryption or equivalent KMS-backed protection defined in the security manifest.

---

# 60. PERSONAL MEMORY — LOCKED

Three classes:

## Immutable events

- card draws;
- astrology calculations;
- numerology calculations;
- purchases;
- report generations.

## User-editable facts

- relationship context;
- career context;
- goals;
- preferred name;
- notes.

## Derived memory

- AI summaries;
- recurring-theme summaries;
- trend summaries.

Derived memory never overwrites source facts. Derived psychology themes are never promoted into diagnoses or vulnerability labels and remain user-deletable.

---

# 61. READING TIMELINE — LOCKED

Tracks:

- previous readings;
- repeated cards;
- repeated suits;
- Major Arcana frequency;
- recurring themes;
- dream symbols;
- numerology cycles;
- notable astrology events.

Example:

> “The Star appeared in three of your last five career readings.”

must come from a deterministic query over stored events.

---

# 62. CORE DATABASE MODEL — LOCKED

Tables/entities:

```text
users
accounts
sessions
profiles
birth_profiles
preferences

subscriptions
entitlements
purchase_events
credit_ledger
store_transactions
webhook_events

TarotCard
tarot_spreads
tarot_spread_positions
tarot_readings
tarot_draws

numerology_profiles
numerology_calculations

natal_charts
planet_positions
house_positions
aspects
transit_snapshots
synastry_results
solar_return_results

compatibility_profiles
compatibility_readings

dreams
dream_symbols
dream_interpretations

readings
reading_sections
reading_themes
reading_evidence
reading_claims

conversations
conversation_messages

generated_reports
report_orders

knowledge_sources
knowledge_documents
knowledge_chunks
knowledge_edges
knowledge_versions
psychology_evidence
psychology_evidence_sources
psychology_evidence_reviews

model_executions
model_usage

notifications
notification_preferences

audit_events
security_events
privacy_requests

feature_flags
system_configuration
user_routing_assignments
cell_migration_jobs
```

Every user-owned entity contains `user_id` where applicable. Every user also has authoritative `home_region`, `cell_id` and `routing_version` control-plane assignment and is protected by service-level authorization.

## 62.1 Reading evidence object

Every completed reading stores an immutable evidence snapshot:

```json
{
  "readingId": "...",
  "readingType": "career",
  "createdAt": "...",
  "engineVersions": {
    "tarot": "...",
    "numerology": "...",
    "astrology": "..."
  },
  "knowledgeVersion": "...",
  "modelAlias": "mysta-fast",
  "resolvedModel": "...",
  "cards": [],
  "numerology": {},
  "astrology": {},
  "retrievedKnowledgeIds": [],
  "psychologyEvidenceIds": [],
  "promptVersion": "...",
  "validatorVersion": "..."
}
```

Old evidence is not silently rewritten when engines/models change.

---

# 63. AUTHORIZATION — LOCKED

Consumer resource access requires:

```text
authenticated user
AND resource.user_id == authenticated user.id
```

No client-submitted `user_id` is trusted as authorization.

Admin access requires:

```text
admin role
AND MFA
AND audited action
```

Break-glass access to private user content additionally requires:

```text
operator identity
reason
timestamp
case/ticket reference when applicable
audit event
```

IDOR tests are launch blockers.

---

# 64. API DESIGN — LOCKED

Base:

```text
/api/v1
```

Groups:

```text
/auth
/profile
/birth-profile
/tarot
/numerology
/zodiac
/astrology
/transits
/compatibility
/dreams
/readings
/mystic
/reports
/billing
/credits
/notifications
/privacy
/admin
```

## 64.1 Reading create

```text
POST /api/v1/readings
```

Example request:

```json
{
  "type": "career",
  "systems": ["tarot", "numerology", "astrology"],
  "question": "What themes should I reflect on in my career?",
  "persona": "oracle"
}
```

The client never submits authoritative calculated values.

## 64.2 API error shape

```json
{
  "error": {
    "code": "FAILED_VALIDATION",
    "message": "The reading could not be validated.",
    "requestId": "...",
    "retryable": false
  }
}
```

No internal stack trace is returned to clients.

## 64.3 Idempotency

Required for:

- reading create retries;
- report orders;
- credit purchases;
- payment processing;
- webhooks;
- account deletion;
- data export jobs.

Header:

```text
Idempotency-Key
```

---

# 65. AUTHENTICATION — LOCKED

Provider:

```text
Better Auth 1.7.5
```

Customer methods:

- email/password;
- email verification;
- password reset;
- Google OAuth;
- Apple Sign In.

Admin:

- password/OAuth according to organization policy;
- mandatory MFA;
- short session lifetime;
- re-authentication for high-risk actions.

Password storage is delegated to the auth framework’s supported secure password hashing implementation; MystaAI does not invent its own password hash scheme.

Mobile auth credentials/tokens are stored only in secure native storage using Expo SecureStore or the auth SDK’s secure storage adapter.

---

# 66. PRICING AND ENTITLEMENTS — LOCKED

## 66.1 Free

```text
£0
```

Includes:

- account;
- onboarding profile;
- one complete welcome reading;
- daily tarot;
- daily star-sign reading;
- basic Life Path;
- Personal Day;
- 3 standard reading credits/month;
- 30-day reading history;
- no unrestricted Mystic Chat.

## 66.2 Starter

```text
£2.99/month
£24.99/year
```

Includes:

- all Free features;
- 20 reading credits/month;
- 50 Mystic messages/month;
- complete core numerology;
- 5 dream interpretations/month;
- 1 compatibility reading/month;
- 90-day history.

## 66.3 Premium

```text
£7.99/month
£69.99/year
```

Includes:

- all Starter features;
- full tarot;
- full numerology;
- natal astrology;
- transits;
- synastry;
- compatibility;
- dream journal;
- cross-system readings;
- full history;
- pattern tracking;
- 300 Mystic messages/month;
- generous standard readings under fair-use protection;
- 1 premium deep report/month.

Internal standard-reading abuse threshold:

```text
20 generated standard readings/day
```

This is not marketed as a normal quota.

## 66.4 Mystic Unlimited

```text
£22.00/month
£199.99/year
```

Includes:

- all Premium features;
- Mystic marketed as unlimited normal human use;
- priority AI queue;
- higher reasoning mode when useful;
- 3 premium deep reports/month;
- priority report generation;
- highest history/pattern access.

Internal abuse/automation controls:

```text
100 Mystic messages/day
2,000 Mystic messages/month soft-review threshold
50 standard readings/day soft-review threshold
```

These are fair-use protections, not customer-facing token limits.

## 66.5 Raw LLM tokens

Raw AI token units are never exposed as customer allowances.

---

# 67. READING CREDIT SYSTEM — LOCKED

Credit costs:

```text
standard tarot                1
deep tarot                    2
dream interpretation          2
full numerology reading       3
compatibility                 3
cross-system reading          4
```

Top-ups:

```text
20 credits     £2.99
60 credits     £6.99
150 credits    £14.99
```

Subscription-issued credits expire at the next renewal/reset unless consumer law or store rules require otherwise.

Purchased credits do not silently disappear; expiry behaviour, if ever introduced, requires an explicit spec/legal revision.

The source of truth is an immutable ledger:

```text
credit_ledger
  id
  user_id
  source_type
  source_id
  delta
  currency_type
  created_at
```

Balance:

```text
SUM(delta)
```

No mutable balance column is the sole authority.

---

# 68. PREMIUM REPORTS — LOCKED

Launch catalogue and web target prices:

| Report | Price |
|---|---:|
| Full Numerology | £14.99 |
| Birth Chart | £19.99 |
| Relationship | £19.99 |
| Career | £14.99 |
| Annual Forecast | £24.99 |
| Complete Spiritual Profile | £29.99 |

Prices are configuration/store data, not hard-coded business logic.

Native regional store prices may differ due to store tiers, tax and currency rules.

---

# 69. STORE PRODUCT MANIFEST — LOCKED

RevenueCat entitlements:

```text
starter
premium
mystic_unlimited
```

Subscription product IDs:

```text
mystaai.starter.monthly
mystaai.starter.annual
mystaai.premium.monthly
mystaai.premium.annual
mystaai.mystic.monthly
mystaai.mystic.annual
```

Consumables:

```text
mystaai.credits.20
mystaai.credits.60
mystaai.credits.150
```

Reports:

```text
mystaai.report.numerology
mystaai.report.birthchart
mystaai.report.relationship
mystaai.report.career
mystaai.report.annual
mystaai.report.spiritual
```

Store-specific identifiers map through `STORE_PRODUCT_MANIFEST.json`.

---

# 70. BILLING AUTHORITY — LOCKED

Web flow:

```text
Stripe purchase/subscription
→ verified Stripe webhook
→ internal billing event
→ internal entitlement
```

Mobile flow:

```text
Apple / Google purchase
→ RevenueCat
→ signed/authenticated RevenueCat webhook
→ internal billing event
→ internal entitlement
```

The MystaAI backend entitlement store is the application authority after verified provider events.

The client is never entitlement authority.

---

# 71. BILLING STATES — LOCKED

```text
TRIAL
ACTIVE
GRACE
BILLING_ISSUE
CANCELLED_ACTIVE_UNTIL_END
EXPIRED
REFUNDED
REVOKED
```

Cancellation does not immediately remove paid access when the paid period remains active.

Refund/revocation follows verified store/provider state.

---

# 72. STRIPE IMPLEMENTATION RULES

Stripe is used for web billing.

Requirements:

- Checkout or Payment Element according to final verified Stripe product flow;
- Customer records;
- subscription products/prices;
- webhook signature verification;
- idempotent webhook processing;
- Customer Portal for supported account/subscription management;
- report/top-up purchases;
- no raw card data stored by MystaAI.

Webhook processing pattern:

```text
verify raw payload/signature
→ persist event ID
→ return success when safely queued/persisted
→ process idempotently
→ reconcile entitlement
```

MystaAI records the Stripe account API version in `INTEGRATION_MANIFEST.json` and does not assume the SDK version equals the account API version.

---

# 73. REVENUECAT IMPLEMENTATION RULES

Use RevenueCat for iOS/Android purchase abstraction and entitlement events.

Required:

- development builds for real IAP testing;
- production and sandbox separation;
- webhook authorization;
- HMAC signature verification when enabled;
- raw request body verification;
- event-ID idempotency;
- fast webhook acknowledgement;
- asynchronous processing;
- restore purchases;
- customer-info refresh on app foreground/after purchase where appropriate.

RevenueCat retries unsuccessful webhook deliveries up to five times with increasing delays; MystaAI must therefore be idempotent.

---

# 74. APP-STORE BILLING RULES — LOCKED

Native digital subscriptions/features use platform-compliant in-app purchase mechanisms unless a jurisdiction-specific permitted flow is explicitly approved by a future compliance update.

The build does not use an in-app Stripe checkout to bypass Apple/Google digital-content rules.

Before store submission:

- re-read Apple App Review Guidelines;
- re-read Google Play Payments policy/service-fee rules;
- update `STORE_PRODUCT_MANIFEST.json` if required;
- rerun financial model.

MystaAI never assumes headline subscription price equals net revenue.

---

# 75. REPORT GENERATION ENGINE — LOCKED

Pipeline:

```text
entitlement/purchase
→ report request
→ deterministic calculations
→ approved knowledge retrieval
→ section generation
→ claim validation
→ report assembly
→ PDF rendering
→ encrypted/private object storage
→ signed delivery URL
→ notification
```

Report evidence stores:

```text
calculation_versions
knowledge_version
model_alias
resolved_model
prompt_versions
source_ids
section_hashes
validation_result
created_at
```

Reports are reproducible from stored evidence where provider model determinism does not guarantee byte-identical prose; the historical generated output itself is stored as the authoritative delivered artifact.

---

# 76. NOTIFICATIONS — LOCKED

Supported notifications:

- daily reading;
- weekly reading;
- monthly reading;
- personal-year/cycle reminder;
- notable transit;
- report ready;
- subscription billing issue;
- purchase restoration result.

Scheduling respects user timezone.

Lock-screen notifications do not reveal sensitive reading text unless the user explicitly opts in.

---

# 77. EMAIL — LOCKED

Provider:

```text
AWS SES v2
```

Transactional use:

- email verification;
- password reset;
- account security;
- report ready;
- data export ready;
- billing issue where appropriate.

Marketing requires separate lawful consent/opt-out handling.

---

# 78. ANALYTICS — LOCKED

Product analytics provider:

```text
PostHog
```

Allowed events include:

```text
signup_started
signup_completed
onboarding_completed
reading_started
reading_completed
reading_failed
mystic_message_sent
dream_created
paywall_viewed
subscription_started
subscription_renewed
subscription_cancelled
report_purchased
report_completed
credit_pack_purchased
```

Prohibited default analytics payloads:

- raw dream text;
- raw Mystic conversation;
- precise birth details;
- legal name;
- private reading narrative;
- medical/legal sensitive text.

Analytics properties are allowlisted.

---

# 79. ADMIN CONSOLE — LOCKED

Admin supports:

- user/account lookup;
- entitlement status;
- transaction status;
- webhook status;
- report failures;
- reading failures;
- model usage/cost;
- queue health;
- knowledge versions;
- knowledge ingestion;
- source licence status;
- safety events;
- feature flags;
- notification status;
- infrastructure health.

Admin does not provide casual unrestricted browsing of private reading/dream/chat content.

High-risk access requires break-glass audit.

---

# 80. ENVIRONMENT MANIFEST — LOCKED

Minimum `.env.example` keys:

```text
NODE_ENV=
APP_URL=
API_URL=
ADMIN_URL=
MOBILE_SCHEME=mystaai

DATABASE_URL=
DATABASE_DIRECT_URL=

CACHE_REDIS_URL=
SQS_AI_QUEUE_URL=
SQS_REPORT_QUEUE_URL=
SQS_NOTIFICATION_QUEUE_URL=
SQS_EXPORT_QUEUE_URL=

BETTER_AUTH_SECRET=
BETTER_AUTH_URL=

GOOGLE_CLIENT_ID=
GOOGLE_CLIENT_SECRET=

APPLE_CLIENT_ID=
APPLE_CLIENT_SECRET=

OPENAI_API_KEY=
OPENAI_BASE_URL=https://api.openai.com/v1
OPENAI_PROJECT_ID=
AI_PRIMARY_MODEL=gpt-5-mini
AI_FAST_ALIAS=mysta-fast
AI_DEEP_ALIAS=mysta-deep
AI_FAST_REASONING_EFFORT=minimal
AI_DEEP_REASONING_EFFORT=medium
OPENAI_STORE_RESPONSES=false

STRIPE_SECRET_KEY=
STRIPE_WEBHOOK_SECRET=
STRIPE_STARTER_MONTHLY_PRICE_ID=
STRIPE_STARTER_ANNUAL_PRICE_ID=
STRIPE_PREMIUM_MONTHLY_PRICE_ID=
STRIPE_PREMIUM_ANNUAL_PRICE_ID=
STRIPE_MYSTIC_MONTHLY_PRICE_ID=
STRIPE_MYSTIC_ANNUAL_PRICE_ID=

REVENUECAT_IOS_API_KEY=
REVENUECAT_ANDROID_API_KEY=
REVENUECAT_WEBHOOK_SECRET=
REVENUECAT_WEBHOOK_HMAC_SECRET=

GEONAMES_USERNAME=
SWISS_EPHEMERIS_PATH=

AWS_REGION=eu-west-2
AWS_HOME_REGION=eu-west-2
MYSTA_REGION_ID=
MYSTA_CELL_ID=
ROUTING_TABLE_NAME=
ROUTING_HMAC_KEY_ID=
SCALE_MANIFEST_PATH=manifests/SCALE_MANIFEST.json
SERVICE_QUOTA_MANIFEST_PATH=manifests/SERVICE_QUOTA_MANIFEST.json
REGION_CELL_MANIFEST_PATH=manifests/REGION_CELL_MANIFEST.json
S3_REPORTS_BUCKET=
S3_KNOWLEDGE_RAW_BUCKET=
S3_KNOWLEDGE_PROCESSED_BUCKET=
SES_FROM_ADDRESS=

SENTRY_DSN=
SENTRY_AUTH_TOKEN=

POSTHOG_KEY=
POSTHOG_HOST=

DATA_ENCRYPTION_KEY_ID=
```

Every variable records:

- secret/public;
- owner service;
- required environments;
- provisioning method;
- rotation method;
- failure behaviour.

Secrets never enter Git.

---

# 81. SECURITY BASELINE — LOCKED

Security standard:

```text
OWASP ASVS 5.0.0
OWASP MASVS 2.1.0
current OWASP MASWE catalogue
```

Controls include:

- TLS everywhere;
- encryption at rest;
- KMS-backed key management;
- Secrets Manager;
- least privilege;
- MFA for admin;
- CSRF protection;
- secure cookie configuration;
- CSP;
- rate limiting;
- brute-force controls;
- input/schema validation;
- webhook signature verification;
- audit logs;
- SAST;
- dependency scanning;
- secret scanning;
- container scanning;
- IDOR tests;
- SSRF tests;
- XSS tests;
- SQL-injection tests;
- mobile secret extraction review.

An exploitable Critical or High vulnerability blocks launch.

---

# 82. SECRET MANAGEMENT — LOCKED

Production secrets live in:

```text
AWS Secrets Manager
```

KMS protects relevant encrypted values.

Secrets are prohibited from:

```text
Git
mobile bundle
frontend source
analytics events
plain logs
error messages
```

Only explicitly public client keys may appear in clients.

---

# 83. PRIVACY AND DATA RIGHTS — LOCKED

UK launch is designed around UK GDPR/Data Protection Act obligations.

Required user controls:

- privacy notice;
- data export;
- account deletion;
- marketing consent management;
- analytics/cookie controls where required;
- notification preferences;
- correction of profile data.

Deletion flow:

```text
request
→ re-authenticate
→ mark deletion pending
→ revoke sessions
→ queue deletion
→ delete/anonymise active application data
→ delete user object-store artefacts
→ remove provider identifiers where supported/required
→ retain only legally required financial/security records
→ completion record
```

MystaAI does not use private reading/chat/dream content as an internal training dataset by default.

If product-improvement datasets are introduced later, they require a new privacy/data-governance specification.

---

# 84. DATA RETENTION — LOCKED BASELINE

```text
Active account profile          until deletion/account closure
Reading history                 until user deletion unless user removes it
Dream history                   until user deletion unless user removes it
Conversation history            user-controlled retention, default retained for product continuity
Raw AI provider payload logs    disabled by MystaAI
Application logs                30 days default
Security/audit logs             90 days minimum, longer where justified
Data-export archives            7 days after download-ready notification
Deletion job metadata           minimal proof retained as legally appropriate
Backups                         expire under backup lifecycle
Financial records               statutory retention as legally required
```

Exact legal financial retention is documented by jurisdiction in `DATA_RETENTION_MANIFEST.json` before launch.

---

# 85. PRODUCTION AWS ARCHITECTURE — SINGLE-REGION SCALE LOCK

## 85.1 Business/engineering scale target

MystaAI retains the commercial objective of capturing a material share of the global paying spiritual-app market, while v2.2 deliberately avoids paying for 20–30M-user infrastructure before demand exists. The launch architecture is required to serve:

```text
2,000,000 paying users target
3,000,000 paying users engineering headroom
```

The 3M figure is a service-capacity target, not a forecast or a hard lifetime ceiling. Free/inactive accounts do not justify prebuilding multi-Region infrastructure; actual runtime capacity is governed by measured concurrent sessions, API RPS, AI concurrency, database load and queue throughput. If total free+paid traffic reaches 70% of the proven envelope, capacity expansion is triggered before the 3M paid-user objective is threatened.

Initial synthetic scale envelope for pre-production proving:

```text
peak concurrent sessions                150,000
sustained authenticated API load         15,000 req/s
short API spike                          30,000 req/s
interactive AI concurrency                1,500 requested generations
async work ingress                        3,000 jobs/s burst envelope
```

These are engineering acceptance targets. Phase 0/88 may increase them after a documented workload model; they may not be reduced merely to make testing easier.

## 85.2 Launch topology

```text
GLOBAL WEB/STATIC
Route 53
  → CloudFront
  → WAF
  → web origin

DYNAMIC API — SINGLE REGION
Route 53
  → regional ALB (`eu-west-2`)
  → WAF
  → ECS Fargate API/services
  → RDS Proxy
  → Aurora PostgreSQL writer/readers
  → ElastiCache
  → SQS + DLQs
  → S3
```

The launch system has one authoritative home Region: `eu-west-2`. It has one logical production cell: `cell-001`. AWS Multi-AZ capabilities inside that Region are used where defined. No launch-time Global Accelerator, DynamoDB Global Tables, Aurora Global Database or active second Region is required.

## 85.3 User routing

Launch routing is stored in Aurora:

```text
user_routing
  user_id
  home_region
  cell_id
  routing_version
  routing_status
  created_at
  updated_at
```

Launch values are normally:

```text
home_region = eu-west-2
cell_id      = cell-001
```

The application accesses routing only through `packages/routing`, not direct ad-hoc SQL from feature packages. Rich user state remains in the same authoritative Aurora data plane.

## 85.4 Capacity thresholds

Capacity is measured, not inferred from cloud branding. `SCALE_MANIFEST.json` records the tested capacity of the deployed architecture.

```text
normal target                 <=60% tested envelope
capacity planning trigger       70%
capacity warning                75%
mandatory capacity increase     80%
```

Within v2.2, capacity increase means one or more of:

- additional ECS tasks;
- larger/more Aurora instances/readers;
- RDS Proxy capacity tuning;
- larger/sharded ElastiCache where justified;
- more SQS consumers;
- higher AWS/OpenAI quotas;
- ALB/CloudFront capacity planning.

A second cell or Region is not a v2.2 launch requirement. It becomes a controlled future scale revision only if measured demand requires it.

## 85.5 Global availability from one Region

Web and static assets are delivered globally through CloudFront. Dynamic MystaAI requests terminate in `eu-west-2`. This intentionally trades some distant-user latency for a materially simpler, faster and cheaper launch architecture.

No claim is made that a single Region eliminates regional-outage risk. A full `eu-west-2` outage can make dynamic MystaAI services unavailable until AWS recovers or the documented restore process is executed. That is an accepted v2.2 launch tradeoff.

---

# 86. NETWORK / EDGE — LOCKED

Public:

- Route 53;
- CloudFront for web/static distribution;
- regional ALB for dynamic API traffic;
- AWS WAF on public entry points;
- approved AWS DDoS protections.

Private:

- ECS internal services;
- Aurora/RDS Proxy;
- ElastiCache;
- workers;
- ML/privacy services.

Database/cache endpoints are never public. Admin access is restricted and audited.

Every externally enforced AWS/OpenAI quota that could block the single-region traffic envelope is recorded in `SERVICE_QUOTA_MANIFEST.json`. Quotas are requested for `eu-west-2` and the real production account only; multi-Region quota requests are prohibited unless a later specification revision activates another Region.

---

# 87. AURORA / DATA POLICY — LOCKED

Production database at the v2.2 lock date:

```text
Aurora PostgreSQL-compatible 18.4.2
AWS-supported pgvector 0.8.2
Multi-AZ cluster
RDS Proxy
writer endpoint
reader capacity
encrypted
PITR/backups
deletion protection
performance monitoring
```

Phase 0/1 reverify the exact compatible engine/extension patch before provisioning.

Rules:

- bounded application pools connect through RDS Proxy;
- migrations use the approved direct administrative connection;
- read-only workloads use readers when consistency semantics allow;
- user-owned high-volume tables are indexed around `user_id`;
- `user_routing` is authoritative for launch region/cell assignment;
- append-heavy tables may use time partitioning only after benchmark evidence;
- schema/index changes are tested against the full 3M-user synthetic dataset;
- no cross-Region replication is a v2.2 launch dependency.

---

# 88. S3 STORAGE — LOCKED

Separate buckets/prefixes by sensitivity and purpose:

```text
knowledge-raw
knowledge-processed
model-artifacts
reports
exports
audit-archive
scale-test-artifacts
```

Requirements:

- block public access;
- server-side encryption;
- KMS where appropriate;
- least-privilege IAM;
- versioning for immutable knowledge/manifests where appropriate;
- lifecycle rules;
- signed URLs for private downloads.

Cross-Region replication is not required for v2.2 launch.

---

# 89. DURABLE ASYNC WORK / CACHE — LOCKED

Amazon SQS is the authoritative durable production queue.

Standard queues default for:

- AI asynchronous generations;
- reports;
- notifications;
- email;
- exports;
- knowledge processing;
- maintenance jobs;
- non-order-sensitive webhook follow-up.

SQS Standard's at-least-once semantics require idempotent consumers. Every queue has:

```text
visibility timeout
retry policy
max receive count
DLQ
idempotency key strategy
depth alarm
oldest-message-age alarm
replay/runbook
```

FIFO is used only where strict ordering/deduplication is a documented business requirement.

ElastiCache remains for cache, rate limits, short-lived coordination and hot derived state. Cache loss may reduce performance but must not corrupt authoritative purchases, entitlements, credits, readings, draws or profile data.

---

# 90. BACKUP / DISASTER RECOVERY — SINGLE-REGION LOCK

Aurora:

```text
point-in-time recovery
automated backups
35-day target retention where service/settings permit
manual pre-migration snapshots
restore into a clean replacement cluster as the primary DR procedure
```

Knowledge/manifests:

```text
versioned S3
immutable manifests where defined
```

Required restore tests:

```text
quarterly minimum
before major database migration
before major infrastructure migration
before production launch
```

Acceptance:

- restore a production-equivalent Aurora snapshot/PITR point into an isolated replacement cluster;
- run migrations/schema verification;
- verify critical row counts and invariants;
- verify reading/history/entitlement integrity;
- reconnect a staging-equivalent application stack;
- record achieved RTO/RPO in the runbook.

No active-active, active-passive cross-Region failover or Aurora Global Database is required for v2.2. A full Region outage remains an accepted availability risk at launch.

---

# 91. SERVICE LEVEL OBJECTIVES — LOCKED TARGETS

At rated single-region capacity:

```text
core API monthly availability           >=99.95%
single-region routing/control target    >=99.95%
non-AI API p95                          <300ms
non-AI API p99                          <1s
tarot deterministic draw p95            <150ms
numerology calculation p95              <100ms
astrology calculation p95               <1s
knowledge retrieval p95                 <750ms
standard AI reading p95                 <20s
deep reading p95                        <60s
Mystic first-token p95                  <6s while provider healthy
premium report generation               <10 minutes
server 5xx at rated load                 <=0.1% excluding deliberate policy responses
```

Correctness, evidence integrity, privacy and entitlement accuracy outrank latency. External provider outages are reported honestly; they never permit fabricated results.

---

# 92. AI COST / CAPACITY CONTROL — LOCKED

Routing rule:

```text
deterministic task → no LLM
simple interpretation → mysta-fast → GPT-5 mini / minimal reasoning
complex synthesis → mysta-deep → GPT-5 mini / medium reasoning
premium report → mysta-deep → GPT-5 mini / medium reasoning / asynchronous
```

Track:

- cost/user;
- cost/plan;
- cost/feature;
- cost/reading;
- cost/report;
- input/output/cached tokens;
- cache hit rate;
- latency;
- failed spend;
- provider RPM/TPM consumption;
- 429/slow-down rate;
- capacity headroom.

`SERVICE_QUOTA_MANIFEST.json` records the OpenAI project's actual limits, not only public maximums. Capacity increases are requested before forecast traffic exceeds 70% of the granted provider envelope.

Interactive Mystic traffic is prioritised above asynchronous reports during provider saturation. Reports remain queued and explicit. No fake completion is shown.

---

# 93. RATE LIMITS / ABUSE CONTROL — LOCKED BASELINE

Authentication:

```text
login:          10 attempts / 15 min / IP-account pair
password reset: 5 / hour / account/IP
signup:         10 / hour / IP
```

General API baseline:

```text
authenticated: 120 requests/minute
anonymous:      30 requests/minute
```

AI/reading limits are additionally governed by tier/fair-use/provider-capacity rules. Rate-limit enforcement cannot create account-enumeration leaks. Admin limits are stricter and combined with MFA/audit.

---

# 94. FAILURE / OBSERVABILITY / QUOTA POLICY — HARD LOCK

Failure states remain explicit:

```text
ASTROLOGY_UNAVAILABLE
FAILED_KNOWLEDGE
FAILED_AI
FAILED_VALIDATION
FAILED_PERSISTENCE
ROUTING_UNAVAILABLE
CAPACITY_EXHAUSTED
REGION_UNAVAILABLE
```

Unknown remains unknown. Capacity exhaustion never becomes fabricated success.

All operational metrics can be segmented by:

```text
service
route
feature
subscription tier
provider/model
queue
database cluster
```

Required metrics include:

- active/free/paid users;
- API RPS and p50/p90/p95/p99 latency;
- 5xx;
- ECS task saturation;
- Aurora/RDS Proxy connections;
- writer/reader utilisation and replica lag;
- cache hit rate/memory;
- SQS depth/oldest age/DLQ depth;
- OpenAI RPM/TPM/429/latency/cost;
- AWS quota utilisation;
- billing/webhook failures;
- validator failures;
- privacy-gateway blocks;
- mobile crash rate.

Raw sensitive user text is excluded from telemetry by default.

`SERVICE_QUOTA_MANIFEST.json` fields:

```text
provider
service
region
quota_name
current_quota
required_capacity
headroom
adjustable
increase_requested
request_reference
verified_at
owner
alarm_threshold
```

For OpenAI, `region` is recorded as `external-provider` and RPM/TPM/batch/usage-limit values are recorded from the actual project.

A required quota below planned demand with no approved mitigation is a scale blocker.

---

# 95. KNOWLEDGE TESTS — LOCKED

Required automated assertions:

```text
78 tarot cards
22 Major Arcana
56 Minor Arcana
all card IDs unique
all spread position counts valid
12 zodiac signs
12 houses
all required planets
all required aspects
all knowledge source IDs resolve
no orphan knowledge chunks
no orphan graph edges
no prohibited source class published
no missing licence/commercial-use field
embedding dimension matches selected model
knowledge version immutable after publish
all psychology evidence has evidence grade
all psychology evidence has rights/commercial-use approval
no active psychology evidence depends only on retracted/inactive source
psychology population/context and limitations are preserved
no prohibited psychology source publishes
```

---

# 96. ASTROLOGY TESTS — LOCKED

Fixture classes:

- ordinary UK birth;
- ordinary US birth;
- eastern/western longitude;
- leap day;
- historical DST;
- ambiguous fall-back time;
- nonexistent spring-forward time;
- unknown birth time;
- high latitude;
- sign-boundary Sun;
- Moon sign boundary;
- retrograde/direct boundary;
- aspect orb boundary;
- synastry;
- solar return.

Raw planetary output must meet Section 42 tolerance.

---

# 97. NUMEROLOGY TESTS — LOCKED

Tests cover:

- all calculations;
- master numbers;
- karmic-debt values;
- name normalisation;
- punctuation;
- accents;
- unsupported script handling;
- leap day;
- Personal Year/Month/Day date transitions.

Target:

```text
100% branch coverage for deterministic numerology package
```

---

# 98. TAROT TESTS — LOCKED

Tests prove:

- 78 cards;
- uniqueness;
- secure RNG path;
- no duplicate card in a draw;
- reversal behaviour;
- spread position count;
- immutable persistence;
- replay consistency;
- validator rejects wrong-card/wrong-orientation claims.

---

# 99. AI EVALUATION SUITE — LOCKED

Stored under:

```text
tests/ai-evals/
```

Evaluation categories:

```text
tarot fidelity
numerology fidelity
astrology fidelity
history fidelity
knowledge provenance
cross-system synthesis
contradiction handling
unsupported certainty
privacy leakage
high-stakes safety
persona adherence
psychology fidelity
psychology evidence provenance
evidence-grade accuracy
population/generalisation handling
correlation-vs-causation handling
retraction handling
diagnosis avoidance
trauma-inference avoidance
attachment-label avoidance
mysticism-vs-evidence separation
psychology privacy
non-manipulation
```

A model/prompt/validator change cannot enter production if regression exceeds the approved threshold recorded in `MODEL_MANIFEST.json` / `PROMPT_MANIFEST.json`.

---

# 100. SECURITY TESTING — LOCKED

Required before launch:

- SAST;
- dependency CVE scan;
- secret scan;
- container scan;
- DAST;
- authentication bypass tests;
- IDOR tests;
- privilege escalation tests;
- webhook forgery/replay tests;
- SQL injection;
- XSS;
- CSRF;
- SSRF;
- rate-limit bypass;
- admin break-glass tests;
- mobile secret extraction review;
- deep-link validation;
- insecure local storage review;
- privacy-gateway leakage tests.

Unresolved exploitable Critical/High issue blocks launch.

---

# 101. PERFORMANCE / RESILIENCE TESTING — HARD LOCK (SINGLE REGION / 3M PAID-USER HEADROOM)

Scale proof is based on the full single-region v2.2 architecture and the Section 85 workload envelope. The launch is not blocked on multi-cell or multi-Region testing because those systems are not part of v2.2 production.

Required test programme:

1. endpoint/component microbenchmarks;
2. deterministic engine benchmarks;
3. knowledge retrieval benchmark;
4. Aurora query/index benchmark;
5. RDS Proxy connection-storm test;
6. ElastiCache saturation/loss test;
7. SQS burst/backlog/recovery/DLQ test;
8. single-region rated-capacity load test;
9. isolated saturation-to-failure test;
10. 3,000,000 synthetic paying-account routing/data-volume test;
11. sustained Section 85.1 peak-envelope test;
12. short 10x spike against normal operating load;
13. minimum 24-hour soak test;
14. OpenAI RPM/TPM/429/slow-down/capacity-exhaustion test;
15. OpenAI 5xx/network outage test;
16. payment webhook burst test;
17. notification burst test;
18. one-AZ impairment test;
19. ECS service failure/replacement test;
20. Aurora writer failover/read-recovery test inside the Region;
21. service-quota exhaustion alarm test;
22. real Aurora backup/PITR restore test.

Load generation uses non-production synthetic data and a locked k6/JMeter/Locust toolchain or AWS Distributed Load Testing where approved.

`SCALE_MANIFEST.json` records:

```text
architecture_version
target_paying_users
design_headroom_paying_users
traffic_model_version
peak_sessions
peak_api_rps
peak_ai_concurrency
api_tested_capacity
database_tested_capacity
cache_tested_capacity
queue_tested_capacity
provider_capacity
capacity_planning_threshold
capacity_warning_threshold
last_load_test
evidence_paths[]
```

Scale acceptance requires:

- 3M synthetic paying-account dataset success;
- 150k peak-session model supported at the documented traffic mix;
- sustained 15k authenticated API RPS;
- 30k short API spike;
- 1,500 requested AI-generation concurrency handled through granted OpenAI capacity/admission control without fabricated success;
- 3,000 jobs/s burst envelope handled/recovered;
- database connection storm does not exhaust Aurora;
- cache loss degrades performance without losing authoritative data;
- queue backlog drains without duplicate business effects;
- one-AZ impairment recovers;
- Aurora in-Region writer failover/recovery is proven;
- backup restore succeeds;
- AWS/OpenAI quota headroom is documented for the single active Region/provider project;
- no test weakens calculation correctness, evidence validation, privacy, billing or entitlement rules.

Future multi-cell/multi-Region tests are not part of v2.2 and must not appear as launch blockers. If a future specification activates them, it must add its own acceptance programme.

---

# 102. CI/CD — LOCKED

Pipeline:

```text
install frozen dependencies
→ validate manifests including SCALE/SERVICE_QUOTA/REGION_CELL/PSYCHOLOGY_EVIDENCE
→ licence + psychology evidence/retraction checks
→ lint
→ typecheck
→ unit tests
→ integration tests
→ calculation fixtures
→ knowledge tests
→ security scan
→ AI eval subset
→ visual regression
→ build
→ container scan
→ deploy staging
→ staging smoke
→ scale smoke / routing-cell health checks
→ end-to-end tests
→ manual production approval
→ production deployment
→ production smoke
```

Production deployment cannot use a skip-tests override.

---

# 103. ENVIRONMENTS — LOCKED

```text
local
test
staging
production
```

Production has separate:

- AWS resources;
- database;
- Redis;
- secrets;
- payment keys;
- RevenueCat environment;
- Sentry environment;
- analytics environment.

Production user data never populates local/test environments unless irreversibly anonymised under a specifically approved process.

---

# 104. APP STORE / PLAY STORE PREPARATION — LOCKED

Required assets/data:

- app icon;
- screenshots;
- description;
- keywords/metadata;
- privacy declarations;
- age rating;
- subscription products;
- review notes;
- support URL;
- privacy-policy URL;
- terms URL;
- account deletion path;
- restore-purchases flow.

Store screenshots must depict real product UI, not unsupported features.

---

# 105. LEGAL / COMPLIANCE LAUNCH GATES

Before commercial launch:

- Swiss Ephemeris commercial licence complete;
- Mysta character commercial-rights gate complete;
- tarot artwork rights complete;
- font licences archived;
- knowledge licences complete;
- privacy notice approved;
- terms approved;
- cookie/analytics consent implemented where required;
- UK GDPR data map completed;
- OpenAI Data Processing Addendum (DPA/SCC) execution completed;
- data retention schedule approved;
- Apple/Google compliance review passed;
- trademark/name availability review performed for MystaAI before major brand spend/launch;
- psychology source/rights/evidence audit complete;
- no blocked psychology source in published knowledge;
- SCALE_MANIFEST, SERVICE_QUOTA_MANIFEST and REGION_CELL_MANIFEST approved;
- single-region scale/resilience acceptance evidence complete for the declared architecture version.

This specification is technical/product design, not a substitute for jurisdiction-specific legal advice.

---

# 106. COMPLETE BUILD PLAN — IMPLEMENTATION ORDER

The following order is locked. A later phase may not be declared complete by bypassing an earlier acceptance gate on which it depends.

## Phase 0 — v2.2 design-control / live verification gate

Before production code:

- reverify every v2.2 official-source assumption;
- reverify OpenAI (GPT-5 mini) model/quota/capacity behavior;
- reverify the single-region `eu-west-2` architecture: Aurora PostgreSQL/pgvector, RDS Proxy, SQS, ElastiCache, CloudFront, WAF, ALB, ECS/Fargate and quotas;
- reverify psychology source rights and automated-retrieval rules;
- approve `SCALE_MANIFEST.json`, `SERVICE_QUOTA_MANIFEST.json`, `REGION_CELL_MANIFEST.json` and `PSYCHOLOGY_EVIDENCE_MANIFEST.json` schemas;
- update implementation materials only where external services changed, preserving locked product behavior.

Acceptance:

- zero unresolved architecture/material choice;
- no scale or psychology-source ambiguity carried into Phase 1;
- v2.2 remains the single build authority.

## Phase 1 — External preflight and dependency freeze

Actions:

- verify all core versions;
- verify Next.js security patch line;
- verify Expo stable/beta status;
- verify Prisma release status;
- verify OpenAI GPT-5 mini model/pricing/API;
- verify Swiss release/licence;
- verify RevenueCat/Stripe rules;
- verify app-store rules;
- verify OpenAI granted rate limits/tier and capacity increase route;
- verify Aurora PostgreSQL/pgvector/RDS Proxy support;
- verify SQS/CloudFront/ALB/ECS/Fargate/Aurora/ElastiCache quotas for `eu-west-2`;
- verify psychology source policies/licences;
- freeze exact package-manager/Turborepo/Fastify-plugin/web-SDK versions;
- populate `DEPENDENCY_MANIFEST.json`.

Acceptance:

- every required dependency has exact version, source, licence and owner;
- no prerelease foundation dependency;
- no unresolved critical advisory.

## Phase 2 — Repository bootstrap

Create:

```text
package.json
pnpm-workspace.yaml
turbo.json
tsconfig.base.json
eslint.config.*
.prettierrc
.gitignore
.node-version
```

Create complete directory tree from Section 36.

Acceptance:

```text
fresh clone
→ install --frozen-lockfile
→ lint
→ typecheck
→ test
```

passes.

## Phase 3 — Manifest schemas

Create JSON schemas and validators for all required manifests.

Acceptance:

- invalid manifest fails CI;
- every core integration represented;
- scale/quota/region-cell/psychology-evidence manifests validate in CI.

## Phase 4 — Brand asset foundation

Actions:

- copy approved Mysta reference into repository;
- verify SHA-256;
- complete commercial-likeness rights gate or replace with approved legally distinct Mysta;
- create `BRAND_MANIFEST.json`;
- obtain/pin Playfair Display and Inter;
- archive OFL licences;
- create design-token files.

Acceptance:

- character/brand/font provenance complete;
- colour/typography tokens compile on web/mobile.

## Phase 5 — MystaAI logo, sigil and icon

Create original:

- sigil;
- wordmark;
- icon variants;
- favicon/app icons.

Acceptance:

- originality review passed;
- small-size readability passed;
- assets hashed/manifests updated.

## Phase 6 — Shared configuration

Build:

```text
packages/config
packages/shared
packages/validation
packages/logging
packages/design-tokens
```

Define:

- environment schema;
- ID strategy;
- error taxonomy;
- date/time conventions;
- logging redaction;
- region/cell identity;
- routing-version contract;
- queue idempotency contract;
- scale/quota config readers.

## Phase 7 — UI component foundation

Build `packages/ui` components from Section 30.

Acceptance:

- accessibility states;
- visual baselines;
- brand tokens only;
- no copied competitor component design.

## Phase 8 — Local infrastructure

Provision local:

- PostgreSQL 18.6;
- pgvector;
- cache Redis;
- local/test SQS adapter or approved dev SQS resources;
- Terraform module skeletons for `edge`, `region`, `cell`.

Acceptance:

- one documented start command;
- health checks pass.

## Phase 9 — Database foundation

Install Prisma 7.10.0 stack.

Create schema/migrations including psychology evidence tables and the authoritative `user_routing` table (`home_region`, `cell_id`, `routing_version`, `routing_status`). No cell-migration job system is required in v2.2.

Acceptance:

```text
empty DB → migrate → seed static minimum → query
```

passes.

## Phase 10 — Authentication

Implement:

- email/password;
- verification;
- reset;
- Google;
- Apple;
- admin MFA;
- secure sessions.

Acceptance:

- web/mobile share identity;
- auth security tests pass.

## Phase 11 — Profile and encrypted birth data

Implement profile/birth-profile/preferences and encryption boundaries.

Acceptance:

- sensitive fields never appear in logs;
- access-control tests pass.

## Phase 12 — Onboarding UI

Implement all `/onboarding/*` screens exactly from the route/design specification.

Acceptance:

- accessibility;
- visual regression;
- complete onboarding state persistence.

## Phase 13 — GeoNames integration

Acquire production plan/account.

Implement:

- search;
- canonical place;
- coordinate storage;
- request caching;
- rate handling;
- client failover if plan supports it.

## Phase 14 — Timezone engine

Implement coordinate-to-IANA mapping and historical civil-time conversion using locked timezone data.

Acceptance:

- DST fixtures pass;
- ambiguous/nonexistent times handled explicitly.

## Phase 15 — Swiss Ephemeris commercial licence

Purchase/record licence before commercial implementation is considered releasable.

Acceptance:

- contract/licence reference in manifest;
- source-use decision approved.

## Phase 16 — Swiss source vendoring

Vendor exact source/tag and required ephemeris files.

Acceptance:

- full SHA recorded;
- every vendored file hash matches manifest.

## Phase 17 — Native Swiss wrapper

Implement Node-API wrapper.

Acceptance:

- wrapper vs `swetest` tolerance passes.

## Phase 18 — Astrology core

Implement:

- planets;
- nodes;
- signs;
- speed/retrograde;
- houses;
- Ascendant/MC;
- aspects.

No AI.

## Phase 19 — Natal chart persistence

Persist deterministic natal result and version.

Acceptance:

- same inputs produce same canonical chart under same engine/version.

## Phase 20 — Transit engine

Implement current/upcoming transit calculations.

## Phase 21 — Synastry engine

Implement deterministic inter-chart aspect calculations.

## Phase 22 — Solar return engine

Calculate exact solar return using Swiss Ephemeris.

## Phase 23 — Astrology validation gate

Run `swetest` and JPL fixture suite.

No AI astrology phase starts until this passes.

## Phase 24 — Numerology engine

Implement all formulas from Section 40.

Acceptance:

- fixture suite;
- 100% branch coverage.

## Phase 25 — Zodiac engine

Implement longitude-based sign resolution and sign domain models.

## Phase 26 — Tarot static domain

Create 78-card canonical records, spread schemas and validation.

## Phase 27 — Tarot secure draw

Implement server cryptographic draw/reversal and immutable storage.

## Phase 28 — Knowledge source registry

Build source/licence/provenance storage and publication gates.

## Phase 29 — Tarot knowledge installation

Populate complete 78-card approved knowledge corpus.

Acceptance:

- every required card field populated;
- every imported claim has source/original-author marker.

## Phase 30 — Numerology knowledge installation

Install approved meanings for:

- 1–9;
- 11/22/33;
- cycles;
- calculations;
- challenges;
- compatibility traditions.

## Phase 31 — Zodiac knowledge installation

Populate all 12 signs, elements, modalities, rulers and domain meanings.

## Phase 32 — Astrology knowledge installation

Populate planets, signs, houses, aspects, transits, synastry and solar-return traditions.

## Phase 33 — Dream knowledge installation

Create original/licensed symbolic framework; no scraped commercial dictionary.

## Phase 34 — Human psychology knowledge installation

Implement the v2.2 psychology evidence layer from Sections 46–50.

Actions:

- create psychology schemas/manifest;
- ingest only approved rights-cleared source records;
- populate evidence grades, population/context, limitations and causal status;
- implement retraction/correction review state;
- create original MystaAI evidence summaries linked to sources;
- install prohibited-use tags.

Acceptance:

- no blocked source publishes;
- every evidence claim has source/rights/evidence-grade metadata;
- retracted/inactive evidence cannot retrieve as approved;
- psychology test corpus passes.

## Phase 35 — Local knowledge ML service

Install pinned embedding/reranker models after licence verification.

Acceptance:

- hashes pinned;
- offline/local inference works;
- no runtime branch download.

## Phase 36 — pgvector/hybrid retrieval

Implement vector + lexical + reranking flow.

Acceptance:

- retrieval benchmark suite passes.

## Phase 37 — Knowledge graph

Implement nodes, edges and provenance.

Acceptance:

- no orphan edges;
- every correspondence has provenance.

## Phase 38 — Privacy-safety service

Implement Presidio redaction, pseudonymisation, sensitive-domain routing and the no-hidden-clinical/vulnerability-profile boundary.

Acceptance:

- privacy leakage evaluation passes.

## Phase 39 — OpenAI GPT-5 mini gateway

Implement:

- official `openai@7.23.0` client inside `packages/ai-gateway` only;
- Responses API calls with `store:false`;
- model registry;
- `mysta-fast` / `mysta-deep` reasoning-effort routing;
- strict Structured Outputs;
- strict function/tool schemas;
- retries honoring `Retry-After`;
- timeout;
- usage/cost/RPM/TPM tracking;
- provider-capacity admission control;
- 429/slow-down/circuit-breaker behavior;
- streaming where used.

Acceptance:

- automated mocked transport tests;
- live calls through a dedicated OpenAI staging project with bounded spend/rate limits;
- `store:false` verified in request fixtures;
- strict schema/tool-call tests pass;
- provider failure/refusal/rate-limit is correctly surfaced;
- no feature package imports the OpenAI SDK directly.

## Phase 40 — Tool registry

Implement all approved Mysta tools with schemas and audit, including `retrieve_psychology_knowledge` and `retrieve_psychology_evidence`; prohibited diagnostic/vulnerability tools must not exist.

## Phase 41 — Reading orchestrator

Build complete deterministic → mystical retrieval + optional psychology evidence retrieval → privacy → AI → validation pipeline with separate evidence namespaces.

Acceptance:

- every completed reading stores evidence.

## Phase 42 — Truth / Evidence / Tradition validator

Implement claim-level FACT/CALCULATION/EVIDENCE/TRADITION/SYNTHESIS checks, including psychology population/generalisation/retraction/diagnosis rules.

Acceptance:

- intentionally wrong card/number/planet/history outputs rejected.

## Phase 43 — Safety engine

Implement high-stakes categories and refusal/redirect behaviour.

## Phase 44 — Tarot AI readings

Wire deterministic draw to knowledge and interpretation.

## Phase 45 — Numerology AI readings

Wire deterministic numerology to approved knowledge and interpretation.

## Phase 46 — Zodiac daily/weekly/monthly

Implement calculated sign context and scheduled content.

## Phase 47 — Natal-chart interpretation

Implement chart-to-knowledge-to-AI pipeline.

## Phase 48 — Transit interpretation

Implement transit reading pipeline.

## Phase 49 — Cross-system synthesis

Implement Section 54 algorithm.

Acceptance:

- conflicting traditions are not falsely merged.

## Phase 50 — Compatibility

Implement synastry + numerology + context synthesis.

## Phase 51 — Dream journal/interpretation

Implement dream storage, privacy, symbols, interpretation and recurrence.

## Phase 52 — Mystic conversation

Implement persistent conversation and tool use with bounded psychology evidence retrieval, provenance and non-diagnostic safeguards.

Acceptance:

- cannot bypass tool authority;
- cannot invent tool success.

## Phase 53 — Reading timeline/pattern engine

Implement deterministic pattern aggregation.

## Phase 54 — Today/Home experience

Build exact Section 15 UI.

Acceptance:

- real daily data only;
- visual/originality/accessibility checks pass.

## Phase 55 — Readings hub

Build exact Section 16 UI.

## Phase 56 — Tarot UI

Build draw/reveal/result screens and animations.

## Phase 57 — Numerology UI

Build profile and calculation explanation screens.

## Phase 58 — Zodiac UI

Build sign/daily/weekly/monthly experience.

## Phase 59 — Astrology UI/chart renderer

Build original MystaAI chart renderer and detailed tabs.

## Phase 60 — Transit UI

Build Now/7-day/30-day timeline.

## Phase 61 — Compatibility UI

Build profile and result screens.

## Phase 62 — Dream UI

Build journal, interpretation and patterns.

## Phase 63 — Mystic Chat UI

Build original Mysta interaction room and states.

## Phase 64 — Cross-System UI

Build signature evidence-separated combined reading.

## Phase 65 — History/Patterns UI

Build timeline and domain pattern views.

## Phase 66 — Billing data model

Implement plans, entitlements, purchase state, credit ledger and reconciliation records.

## Phase 67 — Stripe web billing

Configure products/prices/webhooks/portal and one-off purchases.

Acceptance:

- test-mode full lifecycle.

## Phase 68 — RevenueCat / Apple / Google

Configure products, offerings, entitlements, webhook auth/HMAC and restore flow.

Acceptance:

- physical-device sandbox purchase;
- renewal;
- cancellation;
- billing issue;
- restore;
- refund/revocation path.

## Phase 69 — Entitlement enforcement

Enforce exact Free/Starter/Premium/Mystic feature matrix server-side.

## Phase 70 — Paywall / credits UI

Build original, non-manipulative plan/credit/purchase screens.

## Phase 71 — Report engine

Implement all six launch reports, PDF generation and evidence.

## Phase 72 — Report UI

Build report catalogue, viewer, purchase state and download.

## Phase 73 — Notifications

Implement push scheduling and privacy-safe content.

## Phase 74 — Email

Configure SES and transactional templates.

## Phase 75 — Profile/settings/privacy UI

Build complete Section 29 surfaces.

## Phase 76 — Data export/deletion

Implement actual export/deletion workflows.

## Phase 77 — Admin console

Implement operations surfaces and break-glass controls.

## Phase 78 — Analytics

Implement allowlisted PostHog events only.

## Phase 79 — Observability

Implement Sentry, metrics, dashboards and alerts with mandatory Region/cell/provider/queue/database dimensions plus quota-saturation and cell-admission alerts.

## Phase 80 — Full web integration

Ensure every route and feature is wired end-to-end on web.

No placeholder pages.

## Phase 81 — Mobile foundation

Build Expo 57 native project using development builds from the start.

Integrate:

- router;
- secure storage;
- notifications;
- RevenueCat;
- Sentry;
- fonts/assets.

## Phase 82 — Full mobile integration

Implement all customer journeys on iOS/Android.

## Phase 83 — Accessibility audit

Audit WCAG 2.2 AA, VoiceOver, TalkBack, keyboard and reduced motion.

## Phase 84 — Originality audit

Review every major screen/art asset against Section 6.

Any copied/distinctively derivative asset/layout is replaced.

## Phase 85 — Knowledge rights audit

Verify source/licence/commercial-use manifest and asset provenance.

## Phase 86 — AI acceptance

Run complete AI evaluation suite and freeze model/prompt/validator manifests.

## Phase 87 — Security hardening

Execute Section 100.

No unresolved launch-blocking vulnerabilities.

## Phase 88 — Performance/load hardening

Execute the complete Section 101 single-region scale programme; populate SCALE/SERVICE_QUOTA manifests and record measured autoscaling/database/provider values.

## Phase 89 — Financial/billing acceptance

Test:

- all subscription tiers;
- annual/monthly;
- upgrades/downgrades;
- cancellation;
- renewal;
- grace/billing issue;
- restore;
- refund/revocation;
- credits;
- reports;
- Stripe/RevenueCat reconciliation.

## Phase 90 — Disaster recovery

Execute Section 90 exactly: perform a real Aurora snapshot/PITR restore into an isolated replacement cluster, validate critical data/invariants, reconnect a staging-equivalent stack and record RTO/RPO evidence. No cross-Region failover is required.

## Phase 91 — Privacy/legal gate

Complete privacy/terms/transfer/licence/character-rights/store-rule gates.

## Phase 92 — App Store preparation

Configure metadata, screenshots, privacy, age rating, subscriptions and review notes.

## Phase 93 — Google Play preparation

Configure equivalent Play assets/products/compliance.

## Phase 94 — Production Terraform

Provision tracked AWS web-edge resources plus the complete single-region `eu-west-2` application/data infrastructure from versioned Terraform modules. Provision exactly one launch cell (`cell-001`). Do not provision Global Accelerator, DynamoDB Global Tables, Aurora Global Database or a second authoritative Region.

## Phase 95 — Production secrets

Provision Secrets Manager/KMS values; verify zero secrets in source/history.

## Phase 96 — Production database

Create the production Aurora PostgreSQL cluster, RDS Proxy, supported pgvector extension, migrations, Aurora `user_routing` table, SQS/DLQs, ElastiCache and approved static seed data in `eu-west-2`.

## Phase 97 — Production knowledge publish

Publish immutable initial knowledge version.

## Phase 98 — Production smoke test

Test real production-path integrations without creating unsupported fake state:

- auth;
- GeoNames;
- Swiss;
- OpenAI (GPT-5 mini);
- knowledge retrieval;
- report storage;
- email;
- push;
- permitted payment validation.

## Phase 99 — Full acceptance journey

Execute real-user flow:

```text
install/open
→ account
→ birth profile
→ initial profile calculation
→ welcome reading
→ tarot
→ numerology
→ natal chart
→ transit
→ Mystic
→ dream
→ compatibility
→ cross-system
→ subscribe
→ premium reading
→ report
→ history/patterns
→ purchase restore
→ export data
```

## Phase 100 — Launch

Release:

```text
Web
iOS
Android
```

Monitor continuously during launch.

## Phase 101 — Stabilisation gate

No major new feature work until:

```text
crash rate within target
payment reconciliation healthy
error rate within SLO
no security blocker
no privacy blocker
AI cost understood
validator pass rate stable
queue health stable
```

---

# 107. LAUNCH BLOCKERS — HARD LOCK

MystaAI cannot launch while any of the following remains unresolved:

```text
Swiss Ephemeris commercial licence absent
Mysta character commercial rights unresolved where required
Tarot/brand asset rights unresolved
Knowledge-source licence uncertainty
OpenAI DPA/SCC execution incomplete
Critical/High exploitable security issue
Incorrect tarot deck/draw
Incorrect numerology fixtures
Incorrect astrology fixtures
AI contradicts deterministic evidence above threshold
Payment purchase flow broken
Purchase restoration broken
Stripe reconciliation broken
RevenueCat reconciliation broken
Credit ledger incorrect
Account deletion broken
Data export broken
Backup restoration unproven
Production secret exposure
Native crash blocker
App Store rejection
Google Play rejection
Material privacy defect
Material accessibility blocker
Originality audit failure on production assets
Psychology source/rights/evidence audit incomplete
Blocked/retracted psychology evidence published
Psychology diagnosis/vulnerability eval above allowed threshold
SCALE_MANIFEST incomplete
SERVICE_QUOTA_MANIFEST shows inadequate unmitigated quota
3M routing/data-volume test failed (single-region)
single-cell saturation/isolation test failed
required provider capacity not approved
```

---

# 108. FINAL PRODUCTION ACCEPTANCE CRITERIA

## Functional

Every launch feature specified in this document exists and works.

## Visual

Every launch screen uses the locked MystaAI brand, passes visual regression, accessibility and originality review.

## Mysta

Mysta is consistent with the approved reference and commercial-rights gate.

## Calculation truth

All deterministic engine fixtures pass.

## Knowledge

Every production knowledge item has provenance and approved rights status.

## Psychology

Every production psychology evidence item has rights approval, evidence grade, population/context and limitations; diagnosis, hidden-vulnerability profiling, mystical-validation and manipulative-monetisation evaluations pass.

## AI

FACT/CALCULATION/EVIDENCE/TRADITION/SYNTHESIS claims obey their authority, provenance and validation rules.

## Billing

All subscription/purchase lifecycles pass.

## Security

No unresolved launch-blocking vulnerability.

## Privacy

Consent, minimisation, export, deletion and provider-transfer gates pass.

## Reliability

Retries, queues, webhook idempotency, provider outages and rollback paths are tested.

## Infrastructure / single-region scale

Backup restore succeeds. Cell capacity is measured; single-region routing/control plane passes; 3M synthetic paying-account routing/data-volume validation passes; single-cell saturation and failure isolation pass; provider/service quota headroom is documented for the active Region; adding capacity or a second Region later does not require product redesign.

## Mobile

Physical iOS/Android acceptance passes.

## Web

Supported-browser acceptance passes.

---

# 109. PERMANENT ARCHITECTURE RULES

1. OpenAI (GPT-5 mini) never calculates authoritative astrology.
2. OpenAI (GPT-5 mini) never calculates authoritative numerology.
3. OpenAI (GPT-5 mini) never selects authoritative tarot cards.
4. Every reading stores evidence.
5. Every imported knowledge item has provenance.
6. Every visual production asset has provenance/rights.
7. Unsupported knowledge is not silently invented.
8. Every external integration has an explicit failure state.
9. Unknown remains unknown.
10. Failed tooling is never represented as success.
11. Personal data is minimised before external AI calls.
12. Reader persona cannot bypass safety.
13. Native digital purchases remain store-compliant.
14. Server entitlement state is authoritative for app access.
15. Historical readings remain viewable/reproducible from stored evidence.
16. Model changes require evaluation.
17. Knowledge changes require a new knowledge version.
18. Calculation-engine changes require versioned fixtures.
19. No prerelease foundation dependency enters production without formal spec revision.
20. No unlicensed knowledge/art/font enters production.
21. MystaAI UI must remain original; competitor imitation is prohibited.
22. A launch blocker cannot be waived merely to meet a date.
23. MystaAI v2.2 launches as one `eu-west-2` production cell; capacity is increased inside that Region first, and later cell/Region expansion must preserve product contracts.
24. The Aurora writer is allowed to be the v2.2 authoritative transactional writer; its measured capacity may not be exceeded without an approved capacity increase or later cell split.
25. Every user has an authoritative `home_region`, `cell_id` and `routing_version`; v2.2 launch values are `eu-west-2` / `cell-001`.
26. Routing is isolated in `packages/routing` and contains no MystaAI divination/business logic.
27. Service/AZ failure must be contained inside the launch Region; future cell isolation becomes mandatory if more than one cell is activated.
28. Durable asynchronous work is idempotent and safe under at-least-once delivery.
29. External provider/service quotas are architecture dependencies and are measured/monitored.
30. The 2M paying-user target/3M paying-user headroom requires measured scale evidence, not the statement "cloud scales automatically."
31. Human psychology exists for evidence-informed education/reflection, not diagnosis or treatment.
32. Empirical psychology and mystical tradition remain separate evidence classes.
33. Psychological vulnerability may never drive manipulative monetisation.
34. Every psychology source requires scientific/evidence approval and commercial-rights approval.
35. Scale may increase infrastructure quantity/cost; it may not weaken functionality, truth, security, privacy or UX.

---

# 110. RESEARCH / SOURCE REGISTER — LOCK-DATE REFERENCES

The following official/high-authority references were used to establish this specification and must be rechecked at the relevant preflight gates.

## Runtime/frameworks

- Node.js release index: https://nodejs.org/en/blog/release
- Next.js release/security blog: https://nextjs.org/blog
- React 19.3: https://react.dev/blog/2026/09/09/react-19-3
- Expo SDK 57: https://expo.dev/changelog/sdk-57
- Expo SDK 58 beta: https://expo.dev/changelog/sdk-58-beta
- Expo SDK reference: https://docs.expo.dev/versions/latest/
- Prisma release status: https://www.prisma.io/docs/orm/release-status
- PostgreSQL 18.6: https://www.postgresql.org/docs/release/18.6/
- Fastify npm package: https://www.npmjs.com/package/fastify
- Tailwind CSS v4.3: https://tailwindcss.com/blog/tailwindcss-v4-3
- Zod 4.6: https://zod.dev/blog/zod-4-6
- Vitest 5: https://vitest.dev/blog/vitest-5
- Playwright releases: https://github.com/microsoft/playwright/releases

## AI

- OpenAI API changelog: https://platform.openai.com/docs/changelog
- OpenAI GPT-5 mini model page: https://developers.openai.com/api/docs/models/gpt-5-mini
- OpenAI official Node/TypeScript SDK (`openai`): https://www.npmjs.com/package/openai
- OpenAI pricing: https://openai.com/api/pricing/
- OpenAI rate limits: https://developers.openai.com/api/docs/guides/rate-limits
- OpenAI Responses API migration/state/storage: https://developers.openai.com/api/docs/guides/migrate-to-responses
- OpenAI Structured Outputs: https://developers.openai.com/api/docs/guides/structured-outputs
- OpenAI function calling: https://developers.openai.com/api/docs/guides/function-calling
- OpenAI API data controls: https://developers.openai.com/api/docs/guides/your-data
- OpenAI DPA: https://openai.com/policies/data-processing-addendum/

## Astrology/time/location

- Swiss Ephemeris releases: https://github.com/aloistr/swisseph/releases
- Swiss Ephemeris official site/licensing: https://www.astro.com/swisseph/
- JPL Horizons API: https://ssd-api.jpl.nasa.gov/doc/horizons.html
- GeoNames commercial web services: https://www.geonames.org/commercial-webservices.html
- IANA timezone data: https://www.iana.org/time-zones

## Billing/stores

- RevenueCat React Native package: https://www.npmjs.com/package/react-native-purchases
- RevenueCat webhooks: https://www.revenuecat.com/docs/integrations/webhooks
- Apple App Review Guidelines: https://developer.apple.com/app-store/review/guidelines/
- Google Play service fees: https://support.google.com/googleplay/android-developer/answer/112622

## AWS / single-region scale

- Aurora PostgreSQL updates: https://docs.aws.amazon.com/AmazonRDS/latest/AuroraPostgreSQLReleaseNotes/AuroraPostgreSQL.Updates.html
- Aurora PostgreSQL supported extensions: https://docs.aws.amazon.com/AmazonRDS/latest/AuroraPostgreSQLReleaseNotes/AuroraPostgreSQL.Extensions.html
- RDS Proxy for Aurora: https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/rds-proxy.html
- SQS Standard queues: https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/standard-queues.html
- ECS service autoscaling: https://docs.aws.amazon.com/AmazonECS/latest/developerguide/service-auto-scaling.html
- ECS/Fargate quotas: https://docs.aws.amazon.com/AmazonECS/latest/developerguide/service-quotas.html
- CloudFront quotas: https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/cloudfront-limits.html
- ElastiCache Redis OSS support: https://docs.aws.amazon.com/AmazonElastiCache/latest/dg/engine-versions.html

## Security/privacy

- OWASP ASVS: https://owasp.org/projects/asvs
- OWASP MASVS: https://github.com/OWASP/masvs/releases
- ICO international transfers: https://ico.org.uk/for-organisations/uk-gdpr-guidance-and-resources/international-transfers/

## Psychology knowledge / evidence rights

- NIMH website/publication reuse policy: https://www.nimh.nih.gov/site-info/policies
- PLOS licences/copyright (CC BY commercial reuse): https://journals.plos.org/plosone/s/licenses-and-copyright
- PMC Open Access Subset/licence-by-article/retrieval rules: https://pmc.ncbi.nlm.nih.gov/tools/openftlist/
- Crossref REST metadata/licence/post-publication data: https://www.crossref.org/documentation/retrieve-metadata/rest-api/
- OpenStax Psychology 2e reuse/AI restriction (blocked without permission): https://openstax.org/books/psychology-2e/pages/preface

## Fonts

- Inter: https://github.com/rsms/inter
- Playfair Display OFL: https://github.com/google/fonts/blob/main/ofl/playfairdisplay/OFL.txt

---

# 111. FINAL STATUS

**v2.2 status:** the human-psychology knowledge layer, OpenAI GPT-5 mini gateway and single-region 2–3M-user production architecture are integrated into the main specification; this is not an addendum and no merge with an earlier version is required.

This document defines the launch product, its architecture, calculations, AI boundaries, mystical and human-psychology knowledge installation, privacy, billing, single-region scale infrastructure, security, tests, visual identity, Mysta character, complete UI surface and implementation order.

MystaAI is to be built **start to finish**.

It is not reduced to an MVP during construction.

Difficult features are not silently omitted; they are implemented through the locked phases and accepted only when their objective gates pass.

Future major additions or deliberate architectural changes require a new specification version rather than silently altering this document.

**END OF MYSTAAI FINAL IMPLEMENTATION-LOCKED PRODUCT DESIGN & BUILD SPECIFICATION v2.2**
