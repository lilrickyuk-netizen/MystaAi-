# MystaAI — V3 FINAL IMPLEMENTATION-LOCKED PRODUCT DESIGN & ZERO-GUESSWORK BUILD PLAN

**Version:** V3  
**Status:** FINAL / IMPLEMENTATION LOCKED  
**Research lock date:** 27 September 2026  
**Product name:** **MystaAI**  
**Positioning:** **MystaAI — Your Personal AI Mystic**  
**Product type:** Commercial AI-assisted divination, spiritual-reflection and personalised reading platform  
**Launch surfaces:** Web, iOS, Android  
**Build standard:** Full production product — **not an MVP, not a demo, not a prototype**  
**Internal slug:** `mystaai`  
**Application namespace:** `com.richardcurley.mystaai`  
**Paid production AI:** OpenAI GPT-5.6 Terra (`gpt-5.6-terra`) through the Responses API  
**Free-budget AI:** OpenAI GPT-5.6 Luna (`gpt-5.6-luna`) under the locked daily cost/token/generation budget; Free users cannot access Mysta  
**Production speech synthesis:** self-hosted Kokoro-82M v1.0 with Mysta-owned `mysta_voice_v1`; no metered external TTS API in the normal production path  
**Production speech recognition:** self-hosted Whisper large-v3-turbo through pinned faster-whisper/CTranslate2 on GPU-backed ECS/EC2 inference capacity; no metered external STT API in the normal production path  
**Live avatar provider:** LemonSlice Enterprise, BYO LLM/voice, Zero Data Retention enabled, rendering boundary only  
**Realtime media:** LiveKit Cloud, WebRTC transport only; MystaAI retains its own ASR/LLM/TTS/orchestration/conversation state  
**Cloud architecture:** AWS global edge + one authoritative `eu-west-2` application/data region at launch; one production cell (`cell-001`) with cell-ready expansion  
**Minimum launch age:** 18+  

> **This document is the single build authority for MystaAI.** Product design, architecture, monetisation, knowledge policy, implementation order, dependency/source research, acceptance criteria and launch gates are contained here. The build is executed Phase 0 through Phase 114. No phase is allowed to replace a locked decision with a developer guess.

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
  VOICE_MANIFEST.json
  VOICE_CAPACITY_MANIFEST.json
  AVATAR_PROVIDER_MANIFEST.json
  AVATAR_CAPACITY_MANIFEST.json
  REALTIME_MEDIA_MANIFEST.json
  MONETIZATION_MANIFEST.json
  FREE_ALLOWANCE_MANIFEST.json
  COST_ENVELOPE_MANIFEST.json
```

Each manifest has a JSON Schema and is validated in CI. Applicable production fields may not be `null`, `TBD`, `TODO`, `unknown` or an undocumented default.

## 0.5 Research-complete build workflow

V3 uses **research before implementation**, not open-ended research while coding.

The build plan already defines the required technology, package/model/service, official source, licence status, repository destination, knowledge acquisition path, integration boundary, tests and acceptance gate for every phase. The builder follows those instructions rather than choosing alternatives.

There are only three kinds of checks during execution:

1. **Mechanical freshness check** — confirm that an exact researched dependency is still obtainable and has not been yanked, critically compromised or deprecated. This is not a design exercise. If unchanged, install the V3-pinned value. If a critical external change makes it unusable, stop that phase and perform a controlled V3 change review.
2. **Account-specific value capture** — values that cannot exist until an account/resource exists, such as an AWS service quota, an OpenAI project rate limit, a store product ID or a signed LemonSlice commercial rate, are captured in the exact phase that creates or productionises that integration. They do not block starting the build.
3. **Pre-launch revalidation** — volatile rules/prices/quotas are rechecked before launch using the official source and pass/fail rule already specified by V3. This is verification of a researched design, not new architecture research.

**No phase contains a hidden instruction to “research what to use.”** If a phase needs a dependency, knowledge source, service, licence, credential, download, parser, repository path, test or acceptance criterion, the phase entry in Section 108 names it.

Phase 0 therefore performs build-readiness validation only. It does **not** require production enterprise contracts, final AWS quotas, GPU Capacity Reservations, final store commission values or production load tests before coding can begin.

A material external change may force a controlled V3 revision, but it never authorises silent substitution.

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

## 0.7 Freshness/preflight rule

Run:

```text
infra/scripts/preflight-lock.ts
```

at three lifecycle points:

```text
BUILD START        dependency/source availability only
INTEGRATION PHASE  account/API/service facts needed by that phase
PRE-LAUNCH         prices, quotas, store rules, deprecations, licences and production headroom
```

The script does not choose architecture. It compares live facts against V3's locked requirements and either passes or blocks.

At build start it verifies:

- pinned package/model/source exists;
- package/model is not yanked or critically compromised;
- required licence page/source remains available;
- Node/Expo/PostgreSQL/Python toolchains match the V3 compatibility matrix.

At the relevant integration phase it additionally captures the account-specific fields named by that phase.

Before launch it rechecks:

- OpenAI model availability/deprecation/pricing/project quotas;
- LemonSlice signed rate, billing quantum, ZDR, concurrency and expansion evidence;
- LiveKit project region/privacy/capacity/current charges;
- Apple/Google current digital-goods rules and account fee status;
- RevenueCat and Stripe current commercial terms;
- AWS applied quotas/AZ offerings/current prices/Capacity Reservations;
- Swiss Ephemeris commercial licence status;
- GeoNames production plan;
- current international-transfer/privacy obligations.

A failed preflight blocks the affected integration or launch gate. It does not send the builder back to invent a replacement.

## 0.8 Pre-delivery design audit — passed

V3 was audited before release against these invariants:

- exactly one subscriber product experience: Mysta; no parallel chatbot product;
- Free cannot enter Mysta through UI, API, WebSocket, wallet or route manipulation;
- Premium/Ultra have the same complete Mysta functionality and differ primarily by included avatar allowance;
- eligible subscriber core readings have no marketed monthly reading-count cap; normal-human-use abuse protection remains non-entitlement-breaking;
- only provider-ready avatar-active time debits `AVA_SEC`; failed/fallback intervals do not;
- audio-only and text-only continuity retain intelligence/tool/memory/evidence parity;
- monthly-only launch billing is consistent across pricing, store, acceptance and restore flows;
- deterministic tarot/numerology/astrology remain authoritative over AI synthesis;
- psychology evidence and mystical tradition are separate claim classes;
- launch architecture is one authoritative `eu-west-2` cell with a measured no-redesign scale path;
- avatar capacity assumes 100% avatar-start attempts for admitted Mysta sessions and does not hide under-provisioning behind fallback;
- every knowledge-install phase has a source/rights strategy, repository destination, ingestion/build script and publication gate;
- all 115 phase headings are present exactly once in the continuous range 0–114;
- no phase requires a developer to select an unnamed library/provider/knowledge source.

---

# 1. PRODUCT MISSION

MystaAI is a persistent personal AI mystic combining deterministic divination engines, controlled traditional knowledge, personal history, evidence-informed reflection, AI-generated interpretation, a consistent synthetic voice and a live visual Mysta presence.

Launch includes:

- Tarot;
- Zodiac / star signs;
- Pythagorean numerology;
- Natal astrology;
- Current transits and transit-to-natal interpretation;
- Synastry and compatibility;
- Dream interpretation and dream journal;
- **Mysta**, the subscriber-only embodied AI mystic experience, available to active Premium/Ultra subscribers only;
- Mysta accepts either voice or text input inside the same persistent Mysta session;
- **Mysta is visually embodied by the live real-time animated avatar at launch**, lip-synchronised to `mysta_voice_v1`, with approved listening/reflecting/reading/speaking/gesture states;
- audio-only and text-only presentation exist only as technical/accessibility fallbacks inside the same Mysta session and are never separate products, plans, entitlements or marketed modes;
- evidence-informed human-psychology knowledge for reflective conversation;
- Cross-system synthesis;
- Daily/weekly/monthly/annual readings;
- Personal mystical profile, history and recurring pattern tracking;
- Premium reports;
- Free, Premium and Ultra commercial states;
- a deliberately tiny configurable Free daily AI allowance for eligible non-chat features;
- pay-as-you-go Reading Credits purchasable by Free and paid users;
- subscriber-included live-avatar minutes plus subscriber avatar-minute top-ups;
- one-off report purchases;
- Web, iOS, Android and Admin/operations console;
- self-hosted speech recognition and synthesis with no per-use external speech API charge;
- single-region `eu-west-2` production infrastructure designed for the 2–3M-user launch-scale target with a clean same-product expansion path.

MystaAI is not:

```text
User → generic prompt → generic LLM answer
```

The authoritative conversational production chain is:

```text
USER TEXT / MICROPHONE
 ↓
AUTH + ENTITLEMENT / FREE-ALLOWANCE GATE
 ↓
SELF-HOSTED WHISPER (voice only)
 ↓
PRIVACY + SAFETY INPUT GATE
 ↓
REQUEST CLASSIFIER
 ↓
DIVINATION ORCHESTRATOR
 ↓
DETERMINISTIC DOMAIN ENGINES
 ↓
CONTROLLED KNOWLEDGE / PSYCHOLOGY RETRIEVAL
 ↓
OPENAI MODEL GATEWAY
   Free eligible non-chat → GPT-5.6 LUNA
   Paid / credit-backed / Mysta → GPT-5.6 TERRA
 ↓
TRUTH / EVIDENCE / TRADITION VALIDATOR
 ↓
SAFETY VALIDATOR
 ↓
VALIDATED TEXT
 ├─ text UI
 └─ self-hosted Kokoro → mysta_voice_v1
                         ↓
                    LemonSlice avatar render
                         ↓
                    LiveKit WebRTC video/audio
```

LemonSlice is a **presentation/rendering provider only**. It does not choose tarot, calculate astrology/numerology, hold Mysta's conversation memory, perform Mysta's LLM reasoning, transcribe the user, or synthesize Mysta's authoritative voice.

---

# 2. PERMANENT PRODUCT PRINCIPLES

1. Deterministic calculation outranks generated language.
2. AI never chooses authoritative tarot cards.
3. AI never calculates authoritative numerology or astrology.
4. Traditional meanings come from controlled knowledge; psychology evidence remains a separate evidence class.
5. AI synthesises; it does not invent hidden factual state.
6. Every completed reading stores reproducible evidence; unknown remains unknown.
7. External-service failure never becomes fabricated success.
8. Divination is positioned as reflection/entertainment, not guaranteed supernatural fact.
9. Personal data is minimised before model/provider calls.
10. User-facing design, Mysta visual identity and Mysta voice are original and version-controlled.
11. Competitors may be researched but never copied.
12. Psychology improves reflection; it never becomes diagnosis, therapy, covert clinical profiling or manipulative monetisation.
13. MystaAI launches as one production cell in `eu-west-2`; scaling quantity may change but product truth/security/privacy/UX may not weaken.
14. External provider/service quotas and contracted concurrency are architecture dependencies and are monitored before 70% consumption.
15. `mysta_voice_v1` is the production voice; no generic provider voice may silently replace it.
16. Production STT/TTS is self-hosted; live-avatar rendering may be externally metered but does not own intelligence or voice.
17. Raw microphone audio is transient by default and is never used for training.
18. Partial ASR is not authoritative user intent; TTS speaks only validated Mysta text.
19. **Mysta is subscriber-only. Free users never receive the Mysta experience through Reading Credits, the Free allowance, route manipulation or fallback-state access.**
20. Free AI usage is a promotional daily budget, not purchased currency; it resets daily, does not roll over and is adjustable without an app rebuild.
21. Free usage is protected simultaneously by generation, input-token, output-token and provider-cost ceilings; the first exhausted ceiling stops further free AI generation.
22. Purchased Reading Credits never expire and may be used by Free users for eligible non-chat paid features.
23. Live-avatar allowance is separate from Reading Credits. Premium/Ultra subscription grants may expire at the billing-cycle reset; purchased avatar minutes never expire.
24. Purchased avatar minutes remain owned if a subscription lapses, but cannot be spent until the user again has active Mysta entitlement.
25. Avatar minutes are charged only while the Mysta avatar is provider-ready and billable. Audio-only/text-only fallback states inside the same Mysta session do not consume avatar minutes.
26. If LemonSlice/LiveKit avatar rendering fails, the same Mysta session degrades explicitly to audio-only, then text-only if required, with **no avatar-minute debit during the failed interval**.
27. Store/platform billing rules outrank marketing copy: digital purchases inside iOS/Android use compliant store billing unless a jurisdiction-specific approved programme is separately implemented.
28. Purchased digital credits/currency never expire across any channel; MystaAI uses the strictest common rule to avoid platform-dependent ownership semantics.
29. Pricing/allowances may not be silently changed in code. Paid-price changes require an owner-approved commercial revision; Free allowance values are runtime configuration within the locked owner safety ceiling.
30. A launch price is not assumed profitable merely because gross price exceeds avatar cost; channel-specific fees and all variable costs are measured before launch.

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

## 7.5 Character provenance / consent record

Mysta is the user's original character, inspired by the user's mother rather than intended as a direct copy. The user has confirmed the mother's consent to that inspiration/likeness. MystaAI therefore does not treat character permission as an unresolved product blocker on the facts supplied.

Before launch, `ASSET_MANIFEST.json` / the legal evidence folder records the owner's authorship/provenance statement and the supplied consent record. If the production character is later changed to reproduce another identifiable person's likeness, that new likeness requires its own rights review before release.

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

## 7.7 Mysta voice identity — locked

Mysta has one production synthetic voice identity: `mysta_voice_v1`. It is created as an original synthetic voice and is **not** a clone or imitation of the user's mother, a public figure, a performer or any other identifiable person.

Locked qualities:

- feminine;
- warm;
- calm;
- reassuring;
- intelligent;
- natural rather than theatrical;
- subtly mystical without caricature;
- clear international/British-leaning English pronunciation;
- measured conversational pace;
- capable of gentle reflective pauses without sounding slow or artificial.

The final voice asset is approved in the dedicated voice-creation phase and frozen as:

```text
assets/brand/mysta/voice/mysta_voice_v1.pt
assets/brand/mysta/voice/mysta_voice_v1_reference.wav
assets/brand/mysta/voice/mysta_voice_v1_eval.json
```

`VOICE_MANIFEST.json` records source model revision, source voice embeddings used during design, creation procedure, licence/attribution notices, SHA-256 hashes, owner approval timestamp, pronunciation benchmark result and the statement that no identifiable-person reference recording was used.

The production voice asset is private product IP. It is not published to a public voice repository.

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
Mysta
Explore
Profile
```

Labels remain visible.

`Mysta` receives restrained gold emphasis when selected.

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

## 14.4 Mysta

```text
/mysta
/mysta/new
/mysta/:sessionId
/mysta/history
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

# 24. MYSTA — SINGLE EMBODIED SUBSCRIBER EXPERIENCE — LOCKED

Mysta is the subscriber-only relationship surface. MystaAI does **not** ship a separate chatbot product and a second avatar product. There is only Mysta: one embodied subscriber experience. There is one persistent Mysta session and one Mysta entitlement.

Mysta is visually embodied by the live avatar whenever avatar transport/capacity/allowance permit. The user may speak or type to the same Mysta. If the avatar cannot be rendered, the same session falls back to audio-only and, if necessary, text-only without changing intelligence, memory, tools, entitlement or conversation identity.

Access contract:

```text
Free                         DENIED
Premium ACTIVE/GRACE         ALLOWED
Ultra ACTIVE/GRACE           ALLOWED
expired/cancelled-after-end  DENIED
Reading Credits              NEVER unlock Mysta
```

A Free user selecting Mysta is routed to the subscription explanation/paywall. Free daily AI allowance and Reading Credits cannot bypass this gate.

## 24.1 One session, two input methods, bounded fallback states

There are **not three commercial modes**. The contract is:

```text
PRODUCT EXPERIENCE     MYSTA
DEFAULT PRESENTATION   AVATAR_ACTIVE
INPUT METHOD           VOICE or TEXT
FALLBACK 1             AUDIO_ONLY_FALLBACK
FALLBACK 2             TEXT_ONLY_FALLBACK
```

Rules:

- `AVATAR_ACTIVE` is the normal subscriber presentation and is promoted simply as Mysta, not as a separate “Mysta avatar” product.
- Voice and text are input methods inside the same session; switching input does not create a new conversation or entitlement.
- `AUDIO_ONLY_FALLBACK` is entered for accessibility, bandwidth, device limitation, avatar allowance exhaustion or avatar-provider failure.
- `TEXT_ONLY_FALLBACK` is entered when audio is unavailable/disabled, the user deliberately chooses text for accessibility/privacy, or audio cannot meet its SLO.
- fallback states retain the same Mysta intelligence, conversation ID, memory, tools, safety rules, deterministic evidence, reading actions, history, quick actions, citations/evidence surfaces and subscriber entitlement.
- **No intelligence, model-quality, tool, reading, memory, history or safety capability may be removed merely because avatar video or audio is unavailable.** The only removed capability is the unavailable presentation medium itself.
- only provider-ready avatar-active time consumes `AVA_SEC`.
- the UI must never present text interaction, voice interaction, audio-only fallback or avatar presentation as separate paid products; they are states/capabilities inside Mysta.
- internal state names may contain `FALLBACK`; ordinary accessibility selection must not be presented to the user as an error or inferior tier.
- when degradation is technical, user-facing copy is factual and calm (for example, “Video is unavailable — continuing with Mysta by voice”) and never implies the user has lost Mysta or been downgraded commercially.

## 24.1A First-class continuity presentation contract — hard lock

Audio-only and text-only presentations are **production-grade Mysta surfaces**, not emergency leftover screens.

Both presentations must retain:

```text
same Mysta identity/branding
same persistent session and transcript
same Terra intelligence and prompt/policy stack
same deterministic tarot/astrology/numerology/dream/compatibility tools
same knowledge + psychology retrieval
same truth/safety validation
same conversation memory/history
same quick actions and tool-status visibility
same evidence/citation access
same subscriber reading entitlements
same save/share/report/history actions where those actions exist in AVATAR_ACTIVE
```

`AUDIO_ONLY_FALLBACK` UX requirements:

- Mysta-branded low-bandwidth stage, not a generic phone-call or chatbot screen;
- `mysta_voice_v1`, captions/transcript, microphone state, mute, replay, playback speed, interruption/barge-in and tool-status states remain available;
- avatar video/renderer is not loaded while this state is intentionally active, so it can operate on materially lower bandwidth/compute;
- static Mysta artwork or the approved reduced-motion/low-bandwidth identity treatment may remain visible, but no fake animated avatar is shown;
- VoiceOver/TalkBack/keyboard operation does not depend on the missing video surface.

`TEXT_ONLY_FALLBACK` UX requirements:

- Mysta-branded conversational surface, not a generic bubble-only chatbot clone;
- full transcript, rich reading/tool result cards, quick actions, citations/evidence, typing state, persisted history and all non-audio controls remain available;
- all avatar/video/audio semantics have equivalent text/status semantics;
- screen-reader navigation order is explicitly designed and regression-tested;
- no autoplay/audio dependency and no hidden video requirement.

Transition requirements:

```text
AVATAR_ACTIVE → AUDIO_ONLY_FALLBACK: preserve active turn/session; stop AVA_SEC immediately at provider-ready loss
AUDIO_ONLY_FALLBACK → TEXT_ONLY_FALLBACK: preserve active turn/session; no duplicate model/tool execution
fallback → AVATAR_ACTIVE: reattach presentation to the existing session; no new conversation, no repeated response, no duplicate billing
```

Quality acceptance:

- zero lost accepted user turns across presentation transitions;
- zero duplicated Mysta turns caused by presentation transitions;
- zero lost deterministic evidence/tool result;
- zero entitlement change;
- zero AVA_SEC debit outside provider-ready avatar-active intervals;
- audio-only and text-only complete the same scripted functional acceptance journeys as avatar-active Mysta, excluding only capabilities physically dependent on the missing medium;
- web/iOS/Android each have approved visual/accessibility fixtures for all three presentation states;
- VoiceOver/TalkBack/keyboard/reduced-motion tests pass from session start through tool use, interruption, history and recovery;
- technical failover and restoration are automated integration/E2E tests, not manual-only checks.

## 24.2 Mysta visual state machine

Approved avatar states:

```text
READY
CONNECTING
LISTENING
REFLECTING
CHECKING_EVIDENCE
READING_CARDS
CHECKING_CHART
LOOKING_AT_NUMBERS
SPEAKING
GENTLE_SMILE
CELEBRATORY
CONCERNED
INTERRUPTED
RECONNECTING
UNAVAILABLE
```

Mysta's visible state is driven only by the authoritative application state machine. The LLM may not emit arbitrary animation commands or infer/display hidden mental-health/vulnerability states.

Approved visual behaviour includes:

- natural idle breathing, blinking and eye/head micro-movement;
- attentive listening posture;
- restrained reflective movement;
- subtle hand/upper-body gestures while speaking;
- safe card/chart/number interaction states;
- warm smile/celebratory response for appropriate low-risk moments;
- calm neutral concerned state for safety-sensitive content;
- accurate lip synchronisation to final `mysta_voice_v1` audio.

Forbidden:

- chain-of-thought visualisation;
- manipulative urgency/fear expressions;
- exaggerated distress;
- sexualised motion;
- uncontrolled provider-generated personality drift;
- provider voice substitution;
- avatar actions implying tool/calculation success before authoritative results exist.

## 24.3 Mysta session UI

Header:

```text
Mysta
current state
microphone / text-input control
avatar allowance remaining
conversation actions
```

The avatar remains the dominant visual identity while `AVATAR_ACTIVE`. Text entry opens without replacing Mysta with a conventional chat-bubble product layout. Transcript/history remains available for accessibility and review.

Quick actions:

```text
Love
Career
Tarot
Birth Chart
Dreams
Numerology
```

Tool status may show only factual state:

```text
Drawing your cards…
Checking your chart…
Calculating your numbers…
Checking evidence…
Reflecting on the pattern…
```

Internal chain-of-thought is never exposed.

## 24.4 Voice/input contract

Voice is first-class at launch; text input is equally valid user input to the same Mysta session.

```text
READY
→ CONNECTING
→ LISTENING or TEXT_INPUT
→ TRANSCRIBING when voice
→ UNDERSTANDING
→ TOOL_WORK / REFLECTING
→ VALIDATING
→ SYNTHESISING_VOICE when audio presentation enabled
→ SPEAKING / RENDERING_AVATAR
→ LISTENING or TEXT_INPUT
```

Additional states:

```text
MIC_PERMISSION_REQUIRED
MIC_PERMISSION_DENIED
NETWORK_RECONNECTING
TRANSCRIPT_CONFIRMATION_REQUIRED
VOICE_CAPACITY_BUSY
AVATAR_CONNECTING
AVATAR_CAPACITY_BUSY
AVATAR_ALLOWANCE_EXHAUSTED
AUDIO_ONLY_FALLBACK
TEXT_ONLY_FALLBACK
INTERRUPTED
ENDED
```

Microphone permission is requested only when invoked. Background recording is prohibited. A visible microphone-active indicator is mandatory.

## 24.5 Barge-in

If the user speaks while Mysta is speaking:

1. client playback stops;
2. avatar speaking state is cancelled/transitioned;
3. server receives `interrupt`;
4. queued/unemitted TTS chunks are cancelled;
5. delivered response prefix is marked `interrupted=true`;
6. unused future avatar seconds are not charged merely because the response had been generated;
7. the next turn runs through the normal privacy/tool/truth/safety pipeline in the same Mysta session.

## 24.6 Avatar-second metering

Internal avatar currency is `AVA_SEC` (integer seconds); UI displays minutes.

Metering starts only after:

```text
Mysta entitlement verified
AND
avatar provider session accepted
AND
avatar video/audio track ready
```

Metering stops at the first of:

```text
avatar presentation leaves AVATAR_ACTIVE
session.end accepted
provider session closes
avatar track fails beyond reconnect grace
30-minute session boundary
```

The provider's actual billable quantum is frozen in `AVATAR_PROVIDER_MANIFEST.json`. Reconciliation uses vendor usage evidence; the client is never the metering authority.

Spend order:

```text
1. subscription-included AVA_SEC with nearest expiry
2. purchased non-expiring AVA_SEC
```

When no `AVA_SEC` remains, the subscriber stays in the same Mysta session. The avatar presentation stops and Mysta offers:

```text
continue in AUDIO_ONLY_FALLBACK
continue in TEXT_ONLY_FALLBACK
buy avatar-minute top-up
wait for next subscription grant
```

No additional entitlement or separate chat product is created.

## 24.7 Voice/avatar privacy

User microphone frames go only through MystaAI's approved realtime/ASR path. LemonSlice receives only the validated Mysta output audio/data needed to render Mysta; it is not given separate conversation memory or deterministic-domain data unless technically required and explicitly allowlisted.

Production requirements:

- LemonSlice Enterprise ZDR enabled;
- LiveKit project data region = EU (Frankfurt);
- LiveKit Agent Observability recording/transcript capture disabled;
- no LiveKit Inference;
- no LiveKit Egress/recording for ordinary Mysta sessions;
- raw user microphone data not stored in S3/logs/analytics;
- raw avatar video not retained by MystaAI by default;
- persisted history stores accepted text transcript plus normal evidence/safety metadata;
- provider DPAs/sub-processors/international-transfer assessment completed before launch.

LiveKit media uses global shortest-path routing by default because media transport is transient/not retained and global routing reduces latency. Protocol region pinning remains available on Scale but is enabled only if the legal/data-residency gate requires it; enabling it is recorded in `REALTIME_MEDIA_MANIFEST.json` because it can increase non-European latency.

## 24.8 Avatar failure and fallback

An avatar-provider failure never changes products or loses the conversation:

```text
avatar fails
→ stop AVA_SEC debit
→ show explicit avatar unavailable state
→ preserve same Mysta session
→ continue AUDIO_ONLY_FALLBACK when speech stack healthy
→ otherwise continue TEXT_ONLY_FALLBACK
```

No failed avatar second is charged. Reconnect is bounded and may not create duplicate LemonSlice sessions.

## 24.9 Accessibility

Required:

- captions/transcript always available;
- voice and text input in the same Mysta session;
- audio-only presentation when avatar presentation is unsuitable/unavailable;
- text-only presentation when audio is unsuitable/unavailable;
- a user may intentionally select the accessible audio/text presentation without receiving an error-tier UI;
- mute Mysta;
- replay completed Mysta response;
- playback-speed preference within approved range;
- system headset/Bluetooth/audio routing;
- reduced-motion presentation preserving information;
- avatar never being the sole carrier of meaning;
- every avatar state/tool status having a semantic text equivalent;
- no autoplay outside an active user-started Mysta session;
- Section 24.1A parity/transition acceptance is mandatory on web, iOS and Android.

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

Paywalls are premium, clear and non-manipulative.

Forbidden:

- fake countdowns or scarcity;
- hidden close controls;
- fear-based spiritual messaging;
- implying Mysta will abandon/punish the user;
- misleading “unlimited” claims;
- hiding that live-avatar minutes are metered;
- presenting Reading Credits as a way to unlock subscriber-only Mysta.

Launch plans:

```text
Free
Premium — USD $7.99/month base price
Ultra   — USD $21.99/month base price
```

Only Free, Premium and Ultra launch. Premium and Ultra are monthly subscriptions only; no annual subscription or free trial launches.

Free-exhaustion surface states clearly:

```text
Your free AI insight allowance is used for today.
Come back after the daily reset,
use/buy Reading Credits for eligible readings,
or subscribe to Premium/Ultra.
```

Subscriber avatar-allowance exhaustion states clearly:

```text
Your included Mysta avatar time is used for this billing period.
Continue with the same Mysta session in audio-only or text-only fallback,
buy an avatar-minute top-up,
or wait for the next included grant.
```

Storefront prices may be localised by Apple/Google. The product UI reads actual store price strings from the store/RevenueCat SDK; it never hardcodes a converted native-store price.

---

# 29. PROFILE / SETTINGS UX — LOCKED

Sections:

```text
Your Mystical Profile
Birth Details
Reader Persona
Reading Preferences
Voice Preferences
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
MystaLiveAvatar
MystaAvatarStage
MystaAvatarModeControl
MystaAvatarAllowanceMeter
MystaTarotCard
MystaSpread
MystaChart
MystaNumberHero
MystaInsightCard
MystaEntitlementGate
MystaCreditBadge
MystaReportCard
MystaVoiceOrb
MystaVoiceControls
MystaTranscript
MystaAudioLevel
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
Mysta
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
Speaker
Mute
EndVoice
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

The following versions and compatibility choices were rechecked against current official sources on 27 September 2026.

## 35.1 Runtime/tooling

```text
Node.js             24.21.0 LTS
pnpm                12.5.1
Turborepo           2.11.2
TypeScript          6.0.2
ESLint              10.11.0
Prettier            3.9.9
Vitest              5.0.1
Playwright          1.63.0
```

Package manager: `pnpm 12.5.1`. Monorepo runner: `turbo 2.11.2`. Both are exact-pinned in the root manifest. Mechanical registry availability/security checks are run before install; a different major/minor is not selected automatically.

## 35.2 Web

```text
Next.js             16.3.6
React               19.3.0
React DOM           19.3.0
Tailwind CSS        4.3.3
Zod                 4.6.5
```

`Next.js 16.3.6` is the locked web-framework baseline because the current stable line includes the 22 September 2026 critical security update.

## 35.3 API

```text
Fastify             5.12.5
```

V3 fixes the required Fastify plugin identities. Phase 1 mechanically resolves each package's `dist-tags.latest`, verifies its published Fastify compatibility includes Fastify 5.x, records the exact returned version/integrity, and installs that exact version. No plugin may be substituted. Research-lock reference versions include `@fastify/cors 11.3.0`, `@fastify/helmet 13.1.1`, `@fastify/cookie 11.1.2`, `@fastify/rate-limit 11.2.0`, `@fastify/swagger 9.x`, `@fastify/swagger-ui 6.1.1`, `@fastify/sensible 6.0.5` and `@fastify/websocket 11.3.1`; the manifest value is whatever the deterministic stable resolver returns at Phase 1 if the compatibility check still passes.

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

Production relational engine at the V3 research lock:

```text
Amazon Aurora PostgreSQL-compatible  18.4.2
AWS-supported pgvector               0.8.2
Amazon RDS Proxy                     managed service
```

Aurora PostgreSQL 18.4.2 and AWS pgvector 0.8.2 are the researched production baseline. Phase 107 performs the mechanical AWS availability check immediately before provisioning and records the actual selected Aurora patch/extension in `DEPENDENCY_MANIFEST.json`; application SQL/schema features may not exceed that selected production capability.

Prisma is pinned to `7.10.0`. A prerelease/RC ORM is prohibited. `prisma`, `@prisma/client` and `@prisma/adapter-pg` must remain on the same exact version.

## 35.5 Durable queue/cache/routing

Production durable asynchronous work and routing use:

```text
Amazon SQS Standard queues   default durable queue
Amazon SQS FIFO queues       only where strict ordering/deduplication is required
Amazon ElastiCache Redis     cache/rate-limit/short-lived coordination
Aurora user_routing table    authoritative launch user→home_region/cell assignment
```

BullMQ/Redis is not the authoritative durable production job queue in V3. SQS Standard is at-least-once; every consumer is idempotent and every durable queue has retry, visibility timeout, DLQ, age/depth alarms and replay runbook.

No DynamoDB table is required for the V3 launch architecture. The routing abstraction remains isolated in `packages/routing`; if a later approved scale revision activates multiple independent cells, the backing routing store may be changed without changing user-facing contracts.

The required AWS SDK package identities are listed in Section 35.10 and are exact-resolved by the deterministic Phase-1 AWS-SDK rule.

## 35.6 Mobile

```text
Expo                57.0.25
React Native        0.86.3
React               19.2.3
```

Expo SDK 57 is the stable production line selected for this build. React Native is kept on Expo SDK 57's supported 0.86 line and React mobile on 19.2.3.

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
expo-audio
```

Real in-app purchase testing uses EAS development builds, not Expo Go.

LiveKit requires native WebRTC code and is therefore tested only in EAS development/production builds, never Expo Go. The researched package baseline is:

```text
livekit-client                         2.22.3
@livekit/react-native                 3.0.0
@livekit/react-native-expo-plugin     1.0.3
@livekit/react-native-webrtc          144.2.0
@config-plugins/react-native-webrtc   15.0.2
```

Install through `npx expo install` so Expo validates native compatibility, then freeze the exact resolved lockfile. Because the config-plugin compatibility table can lag new Expo SDK releases, Phase 81 must run `npx expo-doctor`, `npx expo prebuild --clean`, Android compile and iOS EAS compile before the set is accepted. LiveKit's official Expo quickstart remains the implementation authority. Web uses exact-locked `livekit-client@2.22.3`.

## 35.7 Authentication

```text
better-auth         1.7.6
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
react-native-purchases      10.10.2
react-native-purchases-ui   10.10.2
```

Stripe uses the official `stripe` Node SDK. Phase 1 resolves `npm view stripe dist-tags.latest`, rejects prerelease, records the exact version/integrity, and pins it before Phase 67. The Stripe account API version is an account-created value captured separately in `INTEGRATION_MANIFEST.json`.

## 35.9 Observability/analytics

```text
@sentry/react-native       exact Phase-1 resolved stable release
@sentry/nextjs             exact Phase-1 resolved stable release
@sentry/node               exact Phase-1 resolved stable release
posthog-react-native       exact Phase-1 resolved stable release
posthog-js                 exact Phase-1 resolved stable release
posthog-node               exact Phase-1 resolved stable release
```

These SDKs release frequently. V3 locks the package identities and the deterministic resolver: Phase 1 runs `npm view <package> dist-tags.latest`, rejects prerelease tags, verifies runtime/peer compatibility, writes the returned exact version and integrity to `DEPENDENCY_MANIFEST.json`, and installs that exact value. No developer chooses a version.

## 35.10 PDF/storage/email

```text
@react-pdf/renderer	4.9.0
@aws-sdk/client-s3
@aws-sdk/s3-request-presigner
@aws-sdk/client-sesv2
@aws-sdk/client-sqs
@aws-sdk/client-cloudwatch
@aws-sdk/client-application-auto-scaling
```

AWS SDK clients publish rapidly. V3 locks the package list and exact Phase-1 resolution rule: for each AWS package, run `npm view <package> dist-tags.latest`, require a non-prerelease Apache-2.0 release compatible with Node 24, store its exact version/integrity in `DEPENDENCY_MANIFEST.json`, and install exactly that version. This is mechanical dependency locking, not architecture research.

## 35.11 Python privacy/knowledge services

```text
Python                  3.14.7
PyTorch                 2.14.0
Transformers            5.17.0
Sentence Transformers   6.1.0
Presidio Analyzer       2.2.364
Presidio Anonymizer     2.2.364
FastAPI                 0.141.1
Uvicorn                 0.54.0
```

Knowledge/privacy services use `fastapi==0.141.1` and `uvicorn[standard]==0.54.0`; both support Python 3.14 and are exact-pinned in the Python lock file.

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

`geo-tz` is pinned to `8.1.9`.

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
paid model           gpt-5.6-terra
free-budget model    gpt-5.6-luna
```

The official `openai` TypeScript/JavaScript package is the only production OpenAI client dependency and only `packages/ai-gateway` may import it. Version `7.23.0` was the current official npm release at the V3 research lock; Phase 1 re-verifies availability/non-yanked/security status and freezes the exact dependency. Any substitution uses controlled dependency evidence.

`gpt-5-mini-2025-08-07` is explicitly prohibited for the launch implementation because OpenAI's current deprecation register schedules it for removal on **11 December 2026** and names `gpt-5.6-terra` as the recommended replacement. `gpt-5.6-luna` is the current cost-sensitive high-volume member of the same family and is used only for the locked Free budget.

## 35.16 Self-hosted voice runtime — locked

Production speech services do **not** call OpenAI Audio, ElevenLabs or another metered speech API.

Text-to-speech model:

```text
model                    hexgrad/Kokoro-82M
release                  v1.0
repository revision      f3ff357 (exact full revision and file hashes are frozen in Phase 85)
model SHA-256            496dba118d1a58f5f3db2efc88dbdc216e0483fc89fe6e47ee1f2c53f18ad1e4
licence                  Apache-2.0
production voice         mysta_voice_v1
runtime                  self-hosted Python service
external TTS API         prohibited in normal production path
output                    24 kHz mono PCM internally, streamed to clients as Opus
```

The voice-inference service uses a **dedicated Python 3.12.14 container** because the official `kokoro 0.9.4` package declares `Python >=3.10,<3.13`. This is intentionally separate from the Python 3.14.7 privacy/knowledge services.

Exact voice TTS runtime:

```text
Python        3.12.14
kokoro        0.9.4
misaki        0.9.4
model         hexgrad/Kokoro-82M v1.0
```

`espeak-ng` is installed in the container only as the approved English out-of-dictionary phonemisation fallback used by Misaki/Kokoro; no external network TTS service is introduced. The final production image digest is recorded in `VOICE_MANIFEST.json`.

Speech-to-text model:

```text
source model             openai/whisper-large-v3-turbo
source revision          7b51950f41f72ba7b9f619317023ebe15acc1900 (exact source revision is frozen in Phase 84)
licence                  MIT
runtime                  faster-whisper 1.2.1
CTranslate2              4.8.2
Python                   3.12.14
format                   CTranslate2 FP16 generated from the pinned source snapshot
external STT API         prohibited in normal production path
input                     16 kHz mono speech after decode/resample
```

`faster-whisper==1.2.1` and `ctranslate2==4.8.2` are exact-pinned in the Python 3.12.14 voice image. Phase 84 converts/downloads the pinned Whisper source snapshot during the controlled build, records model and converted-artifact SHA-256 values, and production containers never fetch model weights from the public internet at runtime.

Voice transport/runtime dependencies:

- `expo-audio` on mobile, installed with `npx expo install` and frozen;
- browser `MediaDevices/getUserMedia` + Web Audio APIs on web;
- authenticated WebSocket transport through the regional ALB;
- `@fastify/websocket` (research-lock 11.3.1), exact-resolved/frozen by the Phase-1 stable resolver;
- Opus codec support in a pinned container image/toolchain;
- server-side VAD/endpointing from the pinned speech runtime;
- no background microphone capture.

## 35.17 Voice compute — production capacity lock

Voice compute runs only in `eu-west-2` and is split into two independently scaled pools because ASR and TTS have materially different compute economics.

### 35.17.1 ASR production rule — GPU mandatory

Production Whisper large-v3-turbo ASR runs on GPU-backed Amazon ECS capacity on EC2. Fargate is not an allowed ASR production target because AWS Fargate does not provide GPU resources.

Phase 88 benchmarks only instance shapes that are actually offered to the production account in `eu-west-2`. Candidate families are:

```text
1. G6f fractional L4, only when the selected slice has sufficient VRAM and passes the full ASR SLO/concurrency test
2. G6 full L4
3. G5 A10G
```

The benchmark may reject a family/size for model-load failure, VRAM pressure, latency, concurrency, driver/runtime incompatibility, AZ coverage or cost. It may not select an unbenchmarked shape.

For every candidate that loads successfully, Phase 88 records:

```text
instance_type
GPU model/slice
vCPU
RAM
GPU memory available to task
Availability-Zone offerings
On-Demand USD/hour from AWS Price List API
container/AMI digest
CUDA/driver/runtime versions
model hash
warm model-load time
ASR real-time factor p50/p95/p99
speech-end-to-final-transcript p50/p95/p99
max concurrent streams that still meet Section 91
GPU utilisation/memory at max passing concurrency
CPU/RAM utilisation
error/OOM rate
cost_per_passing_concurrent_stream_hour
```

The selected ASR shape is the passing candidate with the lowest `cost_per_passing_concurrent_stream_hour`. Exact tie-break order is: lower p95 transcript latency → lower absolute hourly price → newer AWS GPU generation. The result is frozen in `VOICE_CAPACITY_MANIFEST.json`.

### 35.17.2 TTS production rule — CPU first

Kokoro-82M is not automatically placed on GPU capacity. Phase 88 benchmarks CPU inference first because MystaAI must not pay for accelerator capacity that is not required.

CPU candidates are the following x86-64 Linux ECS/Fargate task shapes, tested in this exact order after Phase 88 confirms that AWS supports the CPU/memory pair:

```text
2 vCPU / 4 GiB
4 vCPU / 8 GiB
8 vCPU / 16 GiB
16 vCPU / 32 GiB
```

The first shape is not automatically selected: all passing candidates are cost-scored. Exact tested shapes and current Fargate rates are recorded in `VOICE_CAPACITY_MANIFEST.json`. If AWS removes one of these combinations, Phase 0 records it as unavailable; adding a replacement shape requires a controlled spec/material revision.

A CPU profile passes only when it satisfies **all** Section 91 TTS first-audio/quality requirements at rated per-task concurrency with >=40% measured capacity headroom.

Only if every approved CPU candidate fails the SLO/cost acceptance may TTS use GPU. GPU TTS candidates are benchmarked in this order:

```text
G6f fractional L4 → G6 L4 → G5 A10G
```

The TTS selection algorithm is identical: lowest measured cost per passing concurrent synthesis stream, then lower p95 first-audio latency, then lower hourly price, then newer GPU generation.

The selected TTS target is frozen as exactly one of:

```text
CPU_FARGATE
GPU_ECS_EC2
```

No production deployment may choose between CPU and GPU ad hoc.

### 35.17.3 Availability-Zone and minimum-capacity rule

Phase 88 queries actual instance offerings by Availability Zone and selects two distinct `eu-west-2` Availability Zones that both offer the chosen ASR GPU instance type. The manifest stores both account-local AZ names and stable AZ IDs.

Production ASR minimum floor:

```text
minimum selected ASR GPU EC2 instances   2
minimum Availability Zones               2
minimum instances per selected AZ        1
Spot instances in minimum floor           prohibited
```

Before production voice is enabled, MystaAI creates an **On-Demand Capacity Reservation** for the minimum ASR GPU instance in each selected AZ. These reservations are part of the production floor and count toward the applicable On-Demand G/VT vCPU quota.

Additional autoscaling capacity may use ordinary On-Demand capacity. Spot capacity is prohibited from the V3 voice launch design so rated capacity does not depend on interruptible instances.

If TTS uses CPU Fargate, production maintains at least two healthy TTS tasks distributed across the selected application AZs. If TTS uses GPU, it follows the same two-AZ/Capacity-Reservation rule as ASR.

### 35.17.4 Account quota rule

`SERVICE_QUOTA_MANIFEST.json` records the **applied quota in the real production AWS account**, not a documented AWS default.

For GPU voice capacity, the required quota is the regional **Running On-Demand G and VT instances** vCPU quota. If TTS uses `CPU_FARGATE`, the relevant quota is **Fargate On-Demand vCPU resource count**, together with Fargate burst/sustained launch-rate quotas. No EC2 Standard-instance quota is substituted for the Fargate quota.

Required granted quota before launch:

```text
required_quota >= ceil(rated_voice_required_vCPU / 0.70)
```

This preserves >=30% quota headroom at rated voice capacity. A pending quota request does not satisfy the launch gate.

### 35.17.5 AWS price and cost lock

Static price numbers are deliberately not hard-coded into this long-lived specification. Phase 88 obtains the current benchmark-time prices directly from the AWS Price List Query API for `eu-west-2` and freezes the returned price evidence in the manifest.

For every selected or rejected benchmark candidate the manifest stores:

```text
pricing_service_code
AWS SKU/price dimension
region
effective_date
currency
on_demand_hourly_rate
price_list_response_hash
retrieved_at
```

The final selected profile additionally stores:

```text
minimum_floor_instances/tasks
rated_instances/tasks_for_voice_mix_v1
maximum_autoscale_instances/tasks
monthly_floor_compute_cost_730h
monthly_rated_compute_cost_730h_equivalent
capacity_reservation_floor_cost
EBS/model-storage_cost
incremental_ALB/data_processing_estimate
CloudWatch/logging_estimate
inter_AZ_transfer_estimate
total_monthly_floor_voice_infra_estimate
total_monthly_rated_voice_infra_estimate
measured_voice_compute_cost_per_1000_voice_minutes
measured_voice_compute_cost_per_nominal_session
```

Cost calculations use current AWS prices plus measured benchmark throughput; they are not inferred from marketing instance specifications.

Capacity Reservations guarantee compute availability but are not treated as a discount. Savings Plans/Reserved pricing may be evaluated after sustained utilisation is known, but launch economics and launch acceptance use the conservative On-Demand rate. Savings discounts never substitute for Capacity Reservations.

### 35.17.6 Benchmark protocol — deterministic

The Phase-88 benchmark harness is non-production tooling under:

```text
tools/preflight/voice-capacity/
```

It uses the exact immutable model/container artefacts intended for production and produces signed JSON/CSV evidence under:

```text
docs/operations/evidence/voice-capacity/<run-id>/
```

ASR benchmark:

1. Warm model fully.
2. Use the locked rights-safe ASR corpus with 5s, 15s and 30s clips.
3. Ramp concurrent streams `1 → 2 → 4 → 8 → ...` until an SLO/OOM/error threshold fails.
4. Hold each concurrency level for 10 minutes after a 2-minute warm-up.
5. Repeat the highest passing level three times.
6. `max_passing_concurrency` is the lowest result across the three repetitions.
7. `production_admitted_concurrency_per_instance = floor(max_passing_concurrency * 0.60)`.

TTS benchmark:

1. Warm model/voice asset fully.
2. Use the locked Mysta response corpus covering 1-sentence, ordinary and long responses.
3. Ramp simultaneous synthesis streams using the same sequence/rules.
4. Measure first-audio, real-time factor, audio quality failures, CPU/GPU/RAM and queue delay.
5. Repeat the highest passing level three times.
6. `production_admitted_concurrency_per_task = floor(max_passing_concurrency * 0.60)`.

A benchmark is invalid if the production model hash, voice hash, container digest, runtime, codec path or instance/task shape differs from the candidate being scored.

### 35.17.7 Fixed voice acceptance traffic mix

The scale programme uses `voice_mix_v1`; this is an engineering acceptance workload, not a user-growth forecast.

At **1,000 concurrent active voice sessions** the steady-state mix is:

```text
400 sessions actively sending speech to ASR
250 sessions receiving/generated TTS audio
250 sessions waiting on Mysta/LLM/tools
100 sessions idle/turn-transition/reconnect window
```

Additional stress windows:

```text
ASR spike                         600 simultaneous ASR streams for 60s
TTS spike                         400 simultaneous TTS streams for 60s
new voice-session burst           200 session starts/s for 10s
```

The rated fleet must be **pre-warmed** before these burst tests. Reactive cold instance launch is not counted as burst capacity.

### 35.17.8 Autoscaling/admission control

For each selected pool:

```text
normal admitted utilisation       <=60% of benchmarked passing concurrency
scale-out utilisation trigger      >65% for 60s
capacity-planning trigger           70%
capacity warning                    75%
hard admission-protection point     80%
scale-in utilisation trigger       <35% for 15m
```

Immediate scale-out is also triggered when either:

```text
ASR/TTS queue-age p95 >200ms for 30s
OR the relevant Section 91 latency SLO fails in two consecutive 1-minute windows
```

Scale-in never reduces ASR below the two-AZ reserved minimum floor. TTS never scales below its two-task/two-AZ minimum.

At the 80% hard protection point the gateway stops admitting additional voice work beyond the bounded queue and explicitly offers text fallback. It never overloads inference until transcripts or speech become unreliable.

Campaigns, app-store featuring or other known traffic events require scheduled pre-warming to the forecast capacity before the event.

### 35.17.9 Voice capacity manifest — no unresolved production fields

`VOICE_CAPACITY_MANIFEST.json` is launch-authoritative and must contain, at minimum:

```text
schema_version
benchmark_run_id
benchmark_timestamp
aws_region
selected_az_names[]
selected_az_ids[]

asr_model_hash
asr_container_digest
asr_compute_target=GPU_ECS_EC2
asr_instance_type
asr_gpu_type
asr_gpu_allocation
asr_max_passing_concurrency_per_instance
asr_admitted_concurrency_per_instance
asr_min_instances
asr_rated_instances_voice_mix_v1
asr_max_instances
asr_capacity_reservation_ids[]

tts_model_hash
mysta_voice_hash
tts_container_digest
tts_compute_target
tts_task_or_instance_shape
tts_max_passing_concurrency_per_task
tts_admitted_concurrency_per_task
tts_min_tasks_or_instances
tts_rated_tasks_or_instances_voice_mix_v1
tts_max_tasks_or_instances
tts_capacity_reservation_ids[]  // empty only for CPU_FARGATE

applied_g_vt_vcpu_quota
required_g_vt_vcpu_quota
applied_fargate_on_demand_vcpu_quota
fargate_burst_launch_rate_quota
fargate_sustained_launch_rate_quota
quota_headroom_percent

pricing_evidence[]
monthly_floor_voice_infra_estimate
monthly_rated_voice_infra_estimate
cost_per_1000_voice_minutes
cost_per_nominal_session

voice_mix_version=voice_mix_v1
load_test_evidence_paths[]
owner_cost_envelope_approval_at
approved_by
```

`null`, `TBD`, placeholder values or missing evidence in any applicable production field block Phase 1 or launch as specified by the relevant gate.

---

## 35.18 Live avatar / realtime media stack — locked

Primary live-avatar renderer:

```text
provider                 LemonSlice
plan                      Enterprise
model class               LemonSlice 2.1 / contracted launch model
integration               BYO LLM + BYO voice
intelligence              MystaAI-owned; LemonSlice is rendering only
voice                     mysta_voice_v1 from self-hosted Kokoro
required enterprise       Actions, Emotions, >=1,429 contracted concurrent sessions, ZDR, data-residency terms, dedicated support
max renderer rate gate    <= USD 0.16 / provider-billable minute
```

The current official LemonSlice pricing page states Enterprise includes Actions, Emotions, 1000+ concurrency, Lite/Pro/Flash access, 24-hour calls, Zero Data Retention and data-residency options, with advertised scale pricing as low as $0.039/min. MystaAI does **not** assume the advertised minimum; Phase 89 requires the actual signed rate and billing quantum before production avatar enablement.

Realtime media:

```text
provider                 LiveKit Cloud
launch plan              Scale
purpose                  WebRTC transport/video-track distribution only
project data region      European Union (Frankfurt)
LiveKit Inference         prohibited
Agent Observability       disabled in production Mysta sessions
ordinary recording       prohibited
region pinning            OFF by default; enable only when legal gate requires it
```

LiveKit Scale is selected because current official terms provide up to 5,000 concurrent connections and region-pinning capability. At the rated 1,000 avatar-active Mysta sessions, capacity planning assumes at least a user participant and avatar participant per session; the 5,000-connection plan therefore preserves material transport headroom. The actual account limit is frozen in `REALTIME_MEDIA_MANIFEST.json`.

Phase 1 exact-locks the stable compatible Node packages:

```text
@livekit/agents
@livekit/agents-plugin-lemonslice
livekit-client
```

and the React/React Native packages listed in Section 35.6. No LemonSlice/LiveKit SDK package version is guessed in this document: Phase 1 follows the existing exact registry-resolution procedure, writes exact versions to `DEPENDENCY_MANIFEST.json`, and blocks if the current stable versions do not satisfy the locked Node/Expo compatibility matrix.

Avatar source asset:

```text
assets/brand/mysta/avatar/mysta-avatar-source-v1.png
```

It is derived from the approved Mysta character direction, not a new person/identity. It must present a clear face/mouth and enough upper/full-body context for the approved gesture set. The final file SHA-256 is written to `ASSET_MANIFEST.json` and `AVATAR_PROVIDER_MANIFEST.json` before provider integration.

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
  avatar-worker/
  voice-gateway/
  voice-inference/

packages/
  ai-gateway/
  avatar-gateway/
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
  realtime-media/
  monetization/
  usage-metering/
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
  voice-contracts/
  voice-orchestrator/
  voice-client/
  zodiac/

assets/
  brand/
    logo/
    mysta/
      avatar/
    icons/
  tarot/
  zodiac/
  reports/
  voice/

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
    avatar/
  acceptance/
  fixtures/
  performance/
  security/
  ai-evals/
  visual/
  voice/
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
voice-gateway     8103
voice-inference   8104
PostgreSQL        5432
cache Redis       6379
```

Production has an edge plane and one authoritative application/data plane.

Realtime Mysta avatar rendering adds two external presentation/control dependencies without moving Mysta intelligence out of the authoritative data plane:

```text
client ↔ LiveKit Cloud WebRTC
              ↕
      MystaAI avatar-worker / avatar-gateway
              ↓ validated mysta_voice_v1 audio only
        LemonSlice Enterprise renderer
              ↓ avatar video track
        LiveKit room → client
```

Whisper, Kokoro, deterministic tools, OpenAI calls, safety, evidence, conversation history, entitlements and wallet ledgers remain under MystaAI authority.

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
voice gateway:    private/public-upgrade path behind ALB for authenticated WebSockets
voice inference:  private ECS-on-EC2 accelerator service, no public endpoint
object storage:   S3
routing metadata: Aurora `user_routing` table
```

The launch backend is authoritative in `eu-west-2`. No Global Accelerator, DynamoDB Global Tables, cross-Region database replication or second authoritative Region is required for V3 launch.

## 37.3 Routing abstraction

`packages/routing` exposes one contract for resolving `user_id → home_region/cell_id/routing_version`. The V3 implementation uses Aurora. Launch values are `home_region=eu-west-2` and `cell_id=cell-001` unless an approved later specification revision activates additional cells.

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
→ OpenAI (GPT-5.6 Terra) interpretation
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

# 46. MYSTA KNOWLEDGE CORE — LOCKED

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

## 47.5 Human psychology sourcing — V3 hard lock

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
BAAI/bge-m3                  (MIT)
BAAI/bge-reranker-v2-m3      (Apache-2.0)
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

OpenAI (GPT-5.6 Terra) is the launch LLM provider, but raw personal data is not sent unrestricted.

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

Raw values normally withheld from OpenAI (GPT-5.6 Terra):

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

Mysta and dream text, including accepted transcripts created from voice input, passes through:

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

## 51.4 Voice privacy boundary

Voice input is processed inside MystaAI-controlled infrastructure in the active AWS Region. Raw microphone audio is transient and must not be written to application logs, analytics, S3, database fields, crash attachments or provider payloads.

The speech-recognition service returns text plus bounded technical metadata only. The accepted text transcript then follows the normal Mysta privacy pipeline before any OpenAI request.

The speech-synthesis service receives only the final validated Mysta text plus approved voice-rendering controls. It receives no raw user audio and no unnecessary profile data.

Voice model artefacts are immutable, checksum-verified and read-only in production containers. User audio may never be used to adapt them.

## 51.5 UK international-transfer gate

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

# 52. OPENAI GPT-5.6 MODEL GATEWAY — LOCKED

Provider:

```text
OpenAI
```

Endpoint:

```text
POST https://api.openai.com/v1/responses
```

Create:

```text
packages/ai-gateway
```

No feature package calls OpenAI directly.

## 52.1 Model aliases

```text
mysta-free  → gpt-5.6-luna,  reasoning.effort=none
mysta-fast  → gpt-5.6-terra, reasoning.effort=low
mysta-deep  → gpt-5.6-terra, reasoning.effort=medium
```

Routing:

- `mysta-free`: Free eligible non-chat concise interpretation only, and only while all Free daily budgets remain available;
- `mysta-fast`: subscriber Mysta session, Reading-Credit-backed reading, ordinary paid/subscriber interpretation;
- `mysta-deep`: complex cross-system work, long compatibility, premium reports and difficult multi-domain synthesis.

Free users can never cause `mysta-free` or any other alias to enter `/mysta` routes or create a Mysta session.

## 52.2 Request contract

```text
API                   Responses API
store                  false
stream                 true where interactive UX benefits
structured output      strict JSON Schema
function/tool schemas  strict
conversation state     MystaAI database, not provider persistence
```

## 52.3 Current lock-date commercial baseline

Official current pricing at the V3 research lock:

```text
gpt-5.6-terra  input $2.00 / 1M   cached $0.20 / 1M   output $12.00 / 1M
gpt-5.6-luna   input $0.20 / 1M   cached $0.02 / 1M   output $1.20 / 1M
```

These prices are evidence inputs, not a promise of future pricing. Phase 39 records the integration-time model prices in `MODEL_MANIFEST.json`; Phase 102 recomputes the complete launch cost guard using then-current prices.

## 52.4 Free admission controller

The initial locked Free daily settings are:

```text
FREE_DAILY_GENERATION_CAP                 2
FREE_DAILY_INPUT_TOKEN_CAP             2500
FREE_DAILY_OUTPUT_TOKEN_CAP             250
FREE_DAILY_OPENAI_COST_CAP_USD        0.001
FREE_DAILY_OWNER_HARD_CEILING_USD     0.005
FREE_ALLOWANCE_RESET                    00:00 UTC
FREE_ALLOWANCE_ROLLOVER                 false
```

At the current Luna prices, exhausting both token caps costs at most approximately:

```text
2500 × $0.20 / 1,000,000 = $0.000500
 250 × $1.20 / 1,000,000 = $0.000300
total token-price maximum = $0.000800/day
```

Therefore the $0.001 provider-cost cap retains safety headroom while keeping direct Free LLM cost below one tenth of one US cent per fully exhausted Free user-day at current prices.

Admission rule:

1. calculate worst-case cost of the requested generation from remaining input/output limits and response cap;
2. reserve that capacity atomically before the provider call;
3. reject if generation, token or cost ceiling would be exceeded;
4. debit actual usage after completion and release unused reservation;
5. if price changes make existing token caps exceed the configured cost cap, the **cost cap wins** and effective token allowance shrinks automatically;
6. an operator may lower or raise normal Free limits at runtime, but no configuration may exceed `FREE_DAILY_OWNER_HARD_CEILING_USD` without owner-authorised audited configuration change.

The user sees **daily free allowance**, never raw token accounting.

## 52.5 AI responsibilities

Terra/Luna may perform natural-language interpretation/explanation/synthesis/conversation/report composition/tone rendering and approved evidence-informed psychology reflection. They may never independently draw tarot, calculate authoritative numerology/astrology, invent history/entitlements/provenance/tool success, diagnose users, or override deterministic results.

## 52.6 Structured output and validation

All model-generated readings use the Section 55 schema. Malformed, refused, incomplete or schema-invalid outputs are handled as explicit failures/retries; they are never silently coerced to success.

## 52.7 Timeouts / retries

```text
short generation       60s
deep reading           180s
report section         240s
full report            asynchronous SQS job
max retry count        2 for 429/5xx/network reset/timeout only
```

Respect `Retry-After`; otherwise use jittered exponential backoff. Authentication/validation 4xx is not retried.

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
commercial_access_source   // FREE_DAILY | SUBSCRIPTION | READ_CREDIT | ONE_OFF
user_id
reading_id/conversation_id
success
error_code
created_at
```

## 52.9 Model change gate

Any model/provider/alias/reasoning change requires AI factual/safety/psychology/tool/structured-output/latency/cost regression evidence plus updated manifests and explicit approval. A model with a shutdown date before planned launch is prohibited.

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

Tables/entities include:

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
store_transactions
webhook_events

free_ai_daily_usage
free_ai_reservations
wallet_ledger
wallet_spend_allocations
commercial_config_versions

avatar_sessions
avatar_usage_segments
avatar_provider_reconciliation
realtime_media_sessions

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
voice_sessions
voice_turns

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

`wallet_ledger` is immutable and supports exactly these launch currencies:

```text
READ       integer Reading Credits
AVA_SEC    integer Mysta avatar seconds
```

Each wallet event contains at minimum:

```text
id
user_id
currency_code
event_type       // GRANT | SPEND | EXPIRE | REVERSAL | ADJUSTMENT
source_type      // PURCHASE | SUBSCRIPTION | PROMO | ADMIN | REFUND
source_id
units_delta
expires_at       // null for purchased units
idempotency_key
metadata_version
created_at
```

Balance is the sum of ledger deltas; no mutable balance field is sole authority. Expiration is represented by an immutable negative `EXPIRE` event created idempotently. Purchased `READ` and purchased `AVA_SEC` events have `expires_at=null`.

`free_ai_daily_usage` is **not** a wallet/currency. It stores UTC date, generation/input/output/cost actuals plus atomic reservations against the runtime-configured Free allowance.

Every user-owned entity contains `user_id` where applicable. Routing/authorization rules remain unchanged.

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

Base: `/api/v1`.

Groups:

```text
/auth /profile /birth-profile
/tarot /numerology /zodiac /astrology /transits /compatibility /dreams
/readings /mysta /reports
/billing /credits /allowance /avatar
/notifications /privacy /admin
```

## 64.1 Reading create

```text
POST /api/v1/readings
```

The server resolves exactly one commercial access source before model execution:

```text
FREE_DAILY
SUBSCRIPTION
READ_CREDIT
ONE_OFF_PURCHASE
```

If `READ_CREDIT` is used, the required credit spend is atomically reserved/debited with idempotency; provider/model failure that yields no accepted paid output triggers the defined compensating credit event.

## 64.2 Free allowance

```text
GET /api/v1/allowance/free
```

Returns user-readable remaining/reset data, never pricing secrets or raw provider quota.

## 64.3 Reading Credits

```text
GET  /api/v1/credits/read
POST /api/v1/credits/read/spend   // internal/server-authorised flow, not arbitrary client delta
```

The client never supplies ledger deltas.

## 64.4 Mysta entitlement gate

Every `/mysta` HTTP/WebSocket token/session creation requires active `premium` or `ultra` entitlement. Free allowance and READ balance are irrelevant to this check. There is one Mysta entitlement and one Mysta session contract; avatar/audio/text states do not create separate entitlements.

## 64.5 Voice session

Authenticated realtime session creation is server-authorised. The existing voice protocol remains `mysta-voice-v1`; user audio enters self-hosted Whisper and validated assistant text enters self-hosted Kokoro.

## 64.6 Mysta avatar transport session

Create:

```text
POST /api/v1/avatar/session
```

Preconditions:

```text
active Premium/Ultra entitlement
MYSTA_AVATAR_ENABLED feature enabled
AVA_SEC > 0
no concurrent active avatar transport session for the same Mysta session/user
voice/avatar capacity admission succeeds
```

Server returns only short-lived LiveKit connection credentials scoped to one room/identity/permissions. LemonSlice and LiveKit service secrets never reach the client.

Control events include:

```text
avatar.session.ready
avatar.state
avatar.usage.started
avatar.usage.tick
avatar.allowance.low
avatar.allowance.exhausted
avatar.fallback.voice
avatar.fallback.text
avatar.session.ended
```

## 64.7 API error shape

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

No internal stack trace is returned.

## 64.8 Idempotency

Required for reading creation/spend, report orders, Reading Credit/AVA purchases, payment/webhooks, avatar session usage settlement, deletion and export jobs. Header: `Idempotency-Key`.

---

# 65. AUTHENTICATION — LOCKED

Provider:

```text
Better Auth 1.7.6
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

## 66.1 Free — $0

Free includes the product's deterministic/basic non-Mysta experiences plus the locked tiny daily AI insight allowance. It includes **no access to Mysta**.

Launch Free AI budget defaults:

```text
2 accepted AI generations/day maximum
2,500 input tokens/day maximum
250 output tokens/day maximum
USD $0.001/day OpenAI direct-cost cap
00:00 UTC reset
no rollover
```

When exhausted, deterministic content that does not require new LLM generation may remain available. Additional AI-personalised non-chat work requires Reading Credits or subscription access where the feature is included.

Free users may buy Reading Credits and one-off reports. Reading Credits do not unlock Mysta.

## 66.2 Premium — USD $7.99/month

Includes:

- full subscriber core feature entitlement;
- full Mysta access as one embodied subscriber experience;
- voice or text input inside the same Mysta session;
- Mysta avatar active by default when device/network/capacity/allowance permit;
- audio-only/text-only fallback inside the same Mysta session when required;
- **25 Mysta avatar minutes (1,500 `AVA_SEC`) per paid billing cycle**;
- full history/pattern experience;
- normal-human-use fair-use/abuse protection rather than a marketed Mystic message quota.

The 1,500 included `AVA_SEC` expires at the end of its paid billing cycle and does not roll over.

## 66.3 Ultra — USD $21.99/month

Ultra has the same launch feature capability as Premium and is the high-avatar-usage subscription:

- all Premium capabilities;
- **69 Mysta avatar minutes (4,140 `AVA_SEC`) per paid billing cycle**.

The 4,140 included `AVA_SEC` expires at billing-cycle end and does not roll over.

There is no artificial feature degradation of Premium merely to force Ultra; both tiers receive the same complete Mysta experience and Ultra differentiates through the materially larger included avatar allowance.

## 66.4 Subscription grant economics

Commercial allowance budgets remain owner-locked:

```text
Premium gross price              $7.99
Premium avatar allowance budget  $3.99
pre-fee spread                    $4.00

Ultra gross price                $21.99
Ultra avatar allowance budget    $10.99
pre-fee spread                    $11.00
```

The 25/69 minute grants are fixed launch customer promises. They are **not** silently reduced because a vendor/store price changes. Phase 102 must prove positive full-use channel contribution margin or block launch for an owner-approved commercial revision.

## 66.5 No annual/trial launch products

V3 launches monthly Premium/Ultra only. Annual plans/free trials require a later owner-approved pricing revision and complete store/margin retest.

## 66.6 Subscriber avatar top-ups

Available only while Premium/Ultra entitlement is active:

```text
Avatar Small     £10 UK base price     +1,800 AVA_SEC (30 minutes)
Avatar Large     £20 UK base price     +4,500 AVA_SEC (75 minutes)
```

Purchased `AVA_SEC` never expires. If the subscription later expires, the purchased balance remains but cannot be spent until the user again has active Mysta entitlement.

---

# 67. READING CREDIT SYSTEM — LOCKED

`READ` is the pay-as-you-go currency for eligible non-chat readings. Free users can purchase it.

Feature costs:

```text
standard tarot                1 READ
deep tarot                    2 READ
dream interpretation          2 READ
full numerology reading       3 READ
compatibility                 3 READ
cross-system reading          4 READ
```

Web/UK-base launch packs:

```text
20 READ      £2.99
60 READ      £6.99
150 READ     £14.99
```

Native stores use the corresponding configured local store price point and display the store-returned localized price.

Purchased READ never expires. There is no conversion between READ and AVA_SEC. READ cannot unlock Mysta or `/mysta`.

Premium/Ultra ordinary core readings are included under normal-human-use/fair-use controls and do not consume READ. Any READ previously purchased remains on the account and becomes useful again if the subscription lapses or for a future explicitly credit-priced non-core feature.

---

# 68. PREMIUM REPORTS — LOCKED

Launch catalogue/web target prices remain:

| Report | Price |
|---|---:|
| Full Numerology | £14.99 |
| Birth Chart | £19.99 |
| Relationship | £19.99 |
| Career | £14.99 |
| Annual Forecast | £24.99 |
| Complete Spiritual Profile | £29.99 |

Reports are separate one-off purchases at launch and do not silently consume READ or AVA_SEC. Native regional store prices may differ because stores localize price/tax/currency.

---

# 69. STORE PRODUCT MANIFEST — LOCKED

RevenueCat entitlements:

```text
premium
ultra
mysta_access
```

Subscription IDs:

```text
mystaai.premium.monthly
mystaai.ultra.monthly
```

Reading Credit consumables:

```text
mystaai.readcredits.20
mystaai.readcredits.60
mystaai.readcredits.150
```

Subscriber avatar-time consumables:

```text
mystaai.avatar.30m
mystaai.avatar.75m
```

Report product IDs remain versioned in `STORE_PRODUCT_MANIFEST.json`.

Apple/Google base-country/base-currency and all actual localized price points are recorded from the real store configuration. Apple can automatically generate storefront prices from a chosen base and Google supports local pricing/pricing templates; MystaAI does not calculate live FX inside the native app.

---

# 70. BILLING / WALLET AUTHORITY — LOCKED

Web:

```text
Stripe purchase/subscription
→ verified webhook
→ purchase_event
→ internal entitlement/wallet ledger grant
```

Mobile:

```text
Apple / Google purchase
→ RevenueCat verification/normalisation
→ authenticated RevenueCat webhook
→ purchase_event
→ internal entitlement/wallet ledger grant
```

MystaAI's backend entitlement + immutable wallet ledger is runtime application authority after a verified provider event. The client is never authority.

RevenueCat is **not called on every reading/avatar-second spend**. The current API v2 Customer In-App-Currency transaction endpoint belongs to a published 480-requests/minute rate-limit domain; RevenueCat documentation has changed these limits over time, so Phase 0 re-verifies the exact endpoint limit. Even the current limit is unsuitable as the synchronous high-throughput wallet path for the locked MystaAI service envelope. MystaAI therefore uses local transactional ledger spends and reconciles purchase/entitlement truth with RevenueCat/provider events.

Purchased digital currency never expires. Subscription-included AVA_SEC can expire/reset because it is a recurring subscription grant rather than purchased consumable currency.

---

# 71. BILLING STATES — LOCKED

```text
ACTIVE
GRACE
BILLING_ISSUE
CANCELLED_ACTIVE_UNTIL_END
EXPIRED
REFUNDED
REVOKED
```

Cancellation preserves access through the already-paid period. Included AVA_SEC grant expiry is tied to the billing-period boundary; no new grant occurs until a successful renewal/charge.

Refund/revocation creates idempotent ledger reversal events for remaining attributable purchased units. Balance is never driven below zero solely by a provider refund; already-consumed refunded value is recorded for fraud/risk review rather than inventing negative spendable currency.

---

# 72. STRIPE IMPLEMENTATION RULES

Stripe is web billing. Requirements: Customer/Checkout or Payment Element as approved, subscriptions, consumables/reports, webhook signature verification, persisted event IDs, idempotent asynchronous processing, Customer Portal where supported, no card data stored by MystaAI, and actual account API version recorded in the manifest.

Web launch base prices use the owner-locked subscription USD values and UK-base top-up/report values. Currency/localisation presentation is configured deliberately; server never trusts a client-submitted price.

---

# 73. REVENUECAT IMPLEMENTATION RULES

RevenueCat abstracts iOS/Android store purchase verification/entitlements.

Required:

- sandbox/production separation;
- stable MystaAI App User ID mapping;
- verified/authenticated webhooks with event-ID idempotency;
- fast acknowledgement + asynchronous processing;
- customer-info refresh after purchase/foreground where appropriate;
- restore subscription/non-consumable state;
- login restores MystaAI's server-side consumable balances;
- refund/revocation reconciliation;
- current RevenueCat commercial cost recorded in `COST_ENVELOPE_MANIFEST.json` (official researched Pro baseline: free to $2,500 MTR, then 1% of tracked revenue; Phase 68 records the current commercial terms and Phase 102 rechecks them).

MystaAI does not depend on RevenueCat virtual-currency spend API for the realtime wallet.

---

# 74. APP-STORE BILLING RULES — LOCKED

On iOS, subscriptions, Reading Credits, avatar-time consumables and reports are digital functionality/content and use Apple In-App Purchase unless an explicitly approved jurisdiction-specific programme applies. Apple currently requires IAP for in-app digital feature unlocks and states purchased credits/currencies may not expire.

On Google Play, subscriptions, digital features and virtual currency use Google Play Billing unless an approved programme exception is implemented. Current UK/EEA/US service fees introduced in 2026 are recorded at Phase 0 rather than assumed from historical 15%/30% rules.

Native UI always renders the actual store-provided localized price. No in-app Stripe checkout is used to bypass store digital-goods rules.

Cross-platform account balances are permitted only under the applicable store rules. On Apple platforms, READ/AVA or subscription value acquired on MystaAI web/another platform may be recognised in the signed-in account only because the same digital item/capability is also offered through IAP in the iOS app, consistent with Apple's current multi-platform-services rule. The native app must not steer users to external checkout unless an enrolled jurisdiction-specific programme expressly permits that flow. Google Play native purchase prompts use Play Billing unless an enrolled programme/market exception is explicitly configured.

Before submission:

- re-read current Apple/Google payment rules;
- verify product type/price/availability in each store;
- verify actual Apple commission/program status;
- verify actual Google fee/billing-fee status by launch market/install class;
- verify RevenueCat cost;
- rerun full-use contribution margin for Premium/Ultra and every consumable;
- block launch if a sold configuration has negative full-use variable contribution margin.

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

Provider: PostHog.

Allowed events include:

```text
signup_started signup_completed onboarding_completed
reading_started reading_completed reading_failed
free_allowance_used free_allowance_exhausted
read_credit_pack_purchased read_credit_spent
paywall_viewed subscription_started subscription_renewed subscription_cancelled
mysta_session_started mysta_text_input_used mysta_voice_input_used mysta_avatar_ready mysta_avatar_ended
avatar_allowance_low avatar_allowance_exhausted avatar_topup_purchased avatar_fallback_used
report_purchased report_completed
```

Allowed commercial properties use coarse identifiers/amounts such as tier, pack/product ID, currency code and metered seconds. Prohibited analytics payloads include raw dream/Mysta-conversation/transcript/audio, precise birth details, legal name, private narrative or sensitive medical/legal text.

Analytics must support conversion funnels:

```text
Free allowance use → exhaustion → READ purchase / subscription / next-day return
Free → Premium/Ultra
Premium → avatar top-up / Ultra
Mysta session start → avatar ready → completed avatar minutes → fallback/error
```

---

# 79. ADMIN CONSOLE — LOCKED

Admin supports:

- user/account and entitlement lookup;
- transaction/webhook status;
- immutable READ/AVA_SEC ledger inspection and audited adjustments;
- Free allowance configuration/current usage/aggregate cost;
- Free allowance enable/disable and limits within owner hard ceiling;
- subscription/plan configuration display (prices are store/manifest-controlled, not casual admin-editable);
- avatar allowance/usage/cost/concurrency/provider health;
- LemonSlice/LiveKit contract/quota/capacity alarms;
- reading/report/model/queue failures;
- model usage/cost;
- knowledge/source licence state;
- safety/feature flags/notifications/infrastructure health.

Changing normal Free limits requires admin MFA + reason + audit event. Raising the Free provider-cost cap above `FREE_DAILY_OWNER_HARD_CEILING_USD` is impossible through ordinary admin controls and requires an owner-authorised versioned configuration change.

Admin never provides casual unrestricted browsing of private reading/dream/Mysta-conversation content. High-risk access requires break-glass audit.

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
AI_FREE_MODEL=gpt-5.6-luna
AI_PRIMARY_MODEL=gpt-5.6-terra
AI_FREE_ALIAS=mysta-free
AI_FAST_ALIAS=mysta-fast
AI_DEEP_ALIAS=mysta-deep
AI_FREE_REASONING_EFFORT=none
AI_FAST_REASONING_EFFORT=low
AI_DEEP_REASONING_EFFORT=medium
OPENAI_STORE_RESPONSES=false

FREE_ALLOWANCE_ENABLED=true
FREE_DAILY_GENERATION_CAP=2
FREE_DAILY_INPUT_TOKEN_CAP=2500
FREE_DAILY_OUTPUT_TOKEN_CAP=250
FREE_DAILY_OPENAI_COST_CAP_USD=0.001
FREE_DAILY_OWNER_HARD_CEILING_USD=0.005
FREE_ALLOWANCE_RESET_TIME_UTC=00:00
FREE_ALLOWANCE_MANIFEST_PATH=manifests/FREE_ALLOWANCE_MANIFEST.json

VOICE_PROTOCOL_VERSION=mysta-voice-v1
VOICE_GATEWAY_URL=
VOICE_TTS_ENGINE=kokoro-82m
VOICE_TTS_MODEL_PATH=
VOICE_TTS_MODEL_SHA256=496dba118d1a58f5f3db2efc88dbdc216e0483fc89fe6e47ee1f2c53f18ad1e4
VOICE_TTS_VOICE_ID=mysta_voice_v1
VOICE_TTS_VOICE_PATH=
VOICE_STT_ENGINE=faster-whisper
VOICE_STT_MODEL_PATH=
VOICE_STT_SOURCE_REVISION=7b51950f41f72ba7b9f619317023ebe15acc1900
VOICE_RAW_AUDIO_RETENTION=false
VOICE_BACKGROUND_RECORDING=false
VOICE_MAX_SESSION_SECONDS=1800
VOICE_MAX_TURN_SECONDS=90
VOICE_CAPACITY_MANIFEST_PATH=manifests/VOICE_CAPACITY_MANIFEST.json

LEMONSLICE_API_KEY=
LEMONSLICE_AGENT_ID=
LEMONSLICE_ZDR_REQUIRED=true
LEMONSLICE_MAX_RENDER_RATE_USD_PER_MIN=0.16
AVATAR_PROVIDER_MANIFEST_PATH=manifests/AVATAR_PROVIDER_MANIFEST.json
AVATAR_CAPACITY_MANIFEST_PATH=manifests/AVATAR_CAPACITY_MANIFEST.json

LIVEKIT_URL=
LIVEKIT_API_KEY=
LIVEKIT_API_SECRET=
LIVEKIT_PROJECT_DATA_REGION=eu
LIVEKIT_USE_INFERENCE=false
LIVEKIT_AGENT_OBSERVABILITY=false
LIVEKIT_MEDIA_REGION_PINNING=false
REALTIME_MEDIA_MANIFEST_PATH=manifests/REALTIME_MEDIA_MANIFEST.json

STRIPE_SECRET_KEY=
STRIPE_WEBHOOK_SECRET=
STRIPE_PREMIUM_MONTHLY_PRICE_ID=
STRIPE_ULTRA_MONTHLY_PRICE_ID=
STRIPE_READ_20_PRICE_ID=
STRIPE_READ_60_PRICE_ID=
STRIPE_READ_150_PRICE_ID=
STRIPE_AVATAR_30M_PRICE_ID=
STRIPE_AVATAR_75M_PRICE_ID=

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

Every variable records secret/public classification, owner service, required environments, provisioning/rotation/failure behaviour. Secrets never enter Git. Paid price/allowance values are also mirrored in signed/versioned monetization/store manifests; environment variables are not allowed to become an unaudited pricing authority.

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

MystaAI does not use private reading/Mysta-conversation/dream content as an internal training dataset by default.

If product-improvement datasets are introduced later, they require a new privacy/data-governance specification.

---

# 84. DATA RETENTION — LOCKED BASELINE

```text
Active account profile          until deletion/account closure
Reading history                 until user deletion unless user removes it
Dream history                   until user deletion unless user removes it
Conversation history            user-controlled retention, default retained for product continuity
Voice accepted transcripts      same retention as conversation history
Raw voice microphone audio      not retained; transient processing only
Synthesised response audio      not persisted by default; regenerated from stored validated text/voice version when replay is requested
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

MystaAI retains the commercial objective of capturing a material share of the global paying spiritual-app market, while V3 deliberately avoids paying for 20–30M-user infrastructure before demand exists. The launch architecture is required to serve:

```text
2,000,000 paying users target
3,000,000 paying users engineering headroom
```

The 3M figure is a service-capacity target, not a forecast or a hard lifetime ceiling. Free/inactive accounts do not justify prebuilding multi-Region infrastructure; actual runtime capacity is governed by measured concurrent sessions, API RPS, AI concurrency, database load and queue throughput. If total free+paid traffic reaches 70% of the proven envelope, capacity expansion is triggered before the 3M paid-user objective is threatened.

Initial synthetic scale envelope for pre-production proving:

```text
peak concurrent app sessions             150,000
sustained authenticated API load           15,000 req/s
short API spike                            30,000 req/s
interactive AI concurrency                  1,500 requested generations
launch-provisioned avatar-active Mysta       1,000 concurrent MINIMUM verified floor
concurrent active voice sessions             1,000 engineering acceptance baseline
voice session start burst                       200 sessions/s
async work ingress                          3,000 jobs/s burst envelope
```

The `1,000` avatar figure is the minimum **launch-provisioned verified floor**, not a claim that only 1,000 avatar sessions will ever be needed and not a mixed-mode usage assumption. Phase 0 may increase it; it may not reduce it.

### 85.1A Avatar-first capacity model — hard lock

Because Mysta is avatar-first, capacity and cost models receive **no discount for an assumed voluntary text/audio-only user mix**.

```text
avatar_start_attempt_rate_for_capacity = 1.00
```

Meaning: every admitted Mysta session that is not already explicitly in an accessibility/technical non-video state is counted as an attempted avatar start for capacity planning. Successful avatar sessions alone are not the demand metric; rejected/queued/capacity-fallback attempts are also recorded.

Launch provision rule:

```text
launch_provisioned_avatar_concurrency >= 1,000
required_launch_contracted_avatar_sessions = ceil(launch_provisioned_avatar_concurrency / 0.70)
```

At the minimum floor:

```text
launch provisioned = 1,000
minimum contracted = 1,429
```

This preserves >=30% contracted-concurrency headroom at the verified launch floor.

The 2–3M paying-user design envelope must not rely on 1,000 concurrency. A deterministic full-allowance capacity sanity floor is derived from the locked subscriber allowances using a 30-day billing-month normalisation:

```text
average_avatar_concurrency = total_included_avatar_minutes_per_30d / 43,200
minimum_contract_floor_with_30pct_headroom = ceil(average_avatar_concurrency / 0.70)
```

Derived endpoints, before any purchased top-ups:

```text
2,000,000 Premium × 25 min = 50,000,000 min/month
average concurrency ≈ 1,158
30% headroom contract floor = 1,654

2,000,000 Ultra × 69 min = 138,000,000 min/month
average concurrency ≈ 3,195
30% headroom contract floor = 4,564

3,000,000 Premium × 25 min = 75,000,000 min/month
average concurrency ≈ 1,737
30% headroom contract floor = 2,481

3,000,000 Ultra × 69 min = 207,000,000 min/month
average concurrency ≈ 4,792
30% headroom contract floor = 6,846
```

These figures are **average full-allowance floors, not peak forecasts**. Therefore:

- Phase 95 must obtain provider/transport evidence that the architecture can expand contractually and operationally to at least `6,846` concurrent avatar sessions without application/protocol redesign; this proves there is no provider ceiling below the worst-case average full-allowance 3M/Ultra design endpoint;
- actual production peak requirement is based on observed `attempted_avatar_concurrency`, including capacity fallbacks, not on successful avatar sessions alone;
- once production data exists, `required_provisioned_avatar_concurrency = ceil(p99_attempted_avatar_concurrency_7d / 0.70)` with a hard minimum of 1,000; the larger of that value and any known campaign/event forecast is provisioned before the demand event;
- if p99 attempted demand, launch campaign forecast or sold/top-up minute behaviour implies a higher requirement, the provider contract, LiveKit capacity, voice TTS capacity and load-test envelope are increased **before** the 70% planning threshold is crossed;
- capacity-triggered audio/text continuity is a safety valve, not the normal product capacity strategy. Under normal non-outage operation, `avatar_capacity_fallback_rate` must remain <=0.5% of Mysta session starts over a rolling 24-hour window; exceeding that for 15 consecutive minutes triggers a mandatory capacity incident and expansion review;
- the cost model assumes 100% of included Premium/Ultra avatar minutes may be consumed and 100% of purchased avatar minutes may be redeemed. It may not assume cheaper audio/text presentation to make the commercial margin pass.

`AVATAR_CAPACITY_MANIFEST.json` and `REALTIME_MEDIA_MANIFEST.json` record the measured participants/session factor. The required LiveKit connection floor is:

```text
required_livekit_connections = ceil(required_avatar_sessions * measured_participants_per_session / 0.70)
```

A self-serve plan ceiling may satisfy launch while it fits; scaling beyond that ceiling uses a contracted LiveKit capacity tier without changing Mysta's application/session architecture.

Voice capacity remains separately benchmarked, but TTS planning assumes every avatar-active Mysta session may require `mysta_voice_v1`; ASR may not use a voluntary-text-input discount for hard overload protection until real post-launch input-method telemetry exists.

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

Within V3, capacity increase means one or more of:

- additional ECS tasks;
- larger/more Aurora instances/readers;
- RDS Proxy capacity tuning;
- larger/sharded ElastiCache where justified;
- more SQS consumers;
- higher AWS/OpenAI quotas;
- more voice-inference accelerator instances/tasks;
- ALB/CloudFront capacity planning.

A second cell or Region is not a V3 launch requirement. It becomes a controlled future scale revision only if measured demand requires it.

## 85.5 Global availability from one Region

Web and static assets are delivered globally through CloudFront. Dynamic MystaAI requests terminate in `eu-west-2`. This intentionally trades some distant-user latency for a materially simpler, faster and cheaper launch architecture.

No claim is made that a single Region eliminates regional-outage risk. A full `eu-west-2` outage can make dynamic MystaAI services unavailable until AWS recovers or the documented restore process is executed. That is an accepted V3 launch tradeoff.

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

Every externally enforced AWS/OpenAI/accelerator-capacity quota that could block the single-region traffic envelope is recorded in `SERVICE_QUOTA_MANIFEST.json`. Quotas are requested for `eu-west-2` and the real production account only; multi-Region quota requests are prohibited unless a later specification revision activates another Region.

---

# 87. AURORA / DATA POLICY — LOCKED

Production database at the V3 lock date:

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

Phase 107 re-verifies the exact compatible engine/extension patch before production provisioning.

Rules:

- bounded application pools connect through RDS Proxy;
- migrations use the approved direct administrative connection;
- read-only workloads use readers when consistency semantics allow;
- user-owned high-volume tables are indexed around `user_id`;
- `user_routing` is authoritative for launch region/cell assignment;
- append-heavy tables may use time partitioning only after benchmark evidence;
- schema/index changes are tested against the full 3M-user synthetic dataset;
- no cross-Region replication is a V3 launch dependency.

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

Cross-Region replication is not required for V3 launch.

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

No active-active, active-passive cross-Region failover or Aurora Global Database is required for V3. A full Region outage remains an accepted availability risk at launch.

---

# 91. SERVICE LEVEL OBJECTIVES — LOCKED TARGETS

At rated single-region capacity:

```text
core API monthly availability               >=99.95%
single-region routing/control target        >=99.95%
non-AI API p95                              <300ms
non-AI API p99                              <1s
tarot deterministic draw p95                <150ms
numerology calculation p95                  <100ms
astrology calculation p95                   <1s
knowledge retrieval p95                     <750ms
standard paid AI reading p95                <20s
deep reading p95                            <60s
Mysta first-token p95                      <6s while OpenAI healthy
voice session connect p95                   <1.5s
speech end → final transcript p95           <1.5s for <=30s English turn
validated text → first Kokoro audio p95     <1.0s ordinary reply
barge-in playback stop p95                  <250ms client-side
avatar provider session-ready p95           <5s while LemonSlice/LiveKit healthy
validated audio → first avatar video p95    <1.5s after avatar session ready
avatar audio/video absolute sync p95        <=250ms
avatar session technical failure            <=1.0% excluding user-network/permission failures
voice session technical failure             <=0.5% excluding user-network/permission failures
premium report generation                   <10 minutes
server 5xx at rated load                     <=0.1% excluding deliberate policy responses
```

Correctness, evidence integrity, privacy, entitlement and wallet accuracy outrank latency. Avatar/voice failure degrades honestly; it never permits unsafe or unvalidated output.

---

# 92. AI / AVATAR COST AND CAPACITY CONTROL — LOCKED

Routing:

```text
deterministic task                    → no LLM
Free eligible concise generation      → mysta-free / GPT-5.6 Luna / none
subscriber or READ-backed ordinary    → mysta-fast / GPT-5.6 Terra / low
complex synthesis/report              → mysta-deep / GPT-5.6 Terra / medium
speech recognition                    → self-hosted Whisper
speech synthesis                      → self-hosted Kokoro / mysta_voice_v1
live visual render                    → LemonSlice Enterprise only while Mysta avatar presentation is AVATAR_ACTIVE
media                                 → LiveKit Cloud Scale WebRTC
```

Track per user/plan/feature/channel:

- OpenAI tokens/cost;
- Free allowance reservations/actuals/exhaustion;
- READ purchases/spends;
- AVA_SEC included/purchased/spent/expired;
- self-hosted ASR/TTS compute cost;
- LemonSlice provider billable seconds/cost;
- LiveKit participant minutes/data transfer/fixed-plan allocation;
- RevenueCat/store/Stripe fees;
- failed/wasted provider spend;
- queue/latency/quota/concurrency headroom;
- contribution margin at actual full allowance use.

`COST_ENVELOPE_MANIFEST.json` is launch-authoritative. It contains separate Premium/Ultra calculations for Web, iOS and Android using the real channel fees and contracted vendor rates. A negative variable contribution margin at full included avatar allowance blocks sale of that plan on that channel until the owner approves a pricing/allowance/provider revision.

Interactive subscriber Mysta traffic is prioritised over asynchronous reports during OpenAI saturation. Free traffic is deprioritised and is the first AI workload shed when provider quota/cost protection requires it.

---

# 93. RATE LIMITS / ABUSE CONTROL — LOCKED BASELINE

Authentication:

```text
login:          10 attempts / 15 min / IP-account pair
password reset: 5 / hour / account/IP
signup:         10 / hour / IP
```

General API:

```text
authenticated: 120 requests/minute
anonymous:      30 requests/minute
```

Free AI:

```text
2 accepted generations/day
plus token and $0.001 direct OpenAI cost caps
one account must not obtain multiple same-day budgets by reinstall/device changes
```

Mysta subscriber abuse protection:

```text
concurrent Mysta sessions          1 / user
voice/live session starts           10 / 10 min / user
continuous voice/avatar session     30 min before transparent renewal boundary
single user speech turn             90s
interactive model requests          20 / minute / user burst ceiling
```

These are anti-automation/infrastructure controls, not marketed Mysta message quotas. Normal human use is not charged by message count.

Avatar provider/account admission stops before 80% of contracted or tested capacity. No client can create provider sessions directly.

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
AVATAR_UNAVAILABLE
AVATAR_ALLOWANCE_EXHAUSTED
FREE_ALLOWANCE_EXHAUSTED
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
- OpenAI Terra/Luna RPM/TPM/429/latency/cost;
- Free daily allowance cost/exhaustion/conversion;
- LemonSlice active sessions/billable seconds/errors/latency/contract headroom;
- LiveKit concurrent connections/participant minutes/data transfer/errors;
- READ/AVA_SEC ledger reconciliation drift;
- channel contribution margin;
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

Scale proof is based on the full single-region V3 architecture and the Section 85 workload envelope. The launch is not blocked on multi-cell or multi-Region testing because those systems are not part of V3 production.

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
15. self-hosted ASR concurrency/latency/accuracy test;
16. self-hosted TTS concurrency/latency/audio-integrity test using `mysta_voice_v1`;
17. 1,000-concurrent active voice-session test plus 200-session/s connection burst;
18. voice WebSocket reconnect, packet-loss and backpressure test;
19. barge-in/cancellation test proving no stale audio continues after interruption;
20. voice-inference task/AZ loss and autoscaling recovery test;
21. OpenAI 5xx/network outage test;
22. payment webhook burst test;
23. notification burst test;
24. one-AZ impairment test;
25. ECS service failure/replacement test;
26. Aurora writer failover/read-recovery test inside the Region;
27. service-quota exhaustion alarm test;
28. real Aurora backup/PITR restore test.

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
peak_voice_sessions
voice_session_start_burst
asr_tested_capacity
tts_tested_capacity
voice_mix_version
asr_instance_type
asr_instance_concurrency
asr_rated_instance_count
tts_compute_target
tts_task_or_instance_shape
tts_instance_concurrency
tts_rated_task_or_instance_count
voice_monthly_floor_cost
voice_monthly_rated_cost
voice_cost_per_1000_minutes
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
- `voice_mix_v1` is used exactly: 400 ASR + 250 TTS + 250 Mysta/tool-wait + 100 idle/transition sessions at 1,000 concurrent sessions;
- 600-stream ASR and 400-stream TTS 60-second stress windows pass;
- 1,000 concurrent active voice sessions and 200 new voice sessions/s handled at the declared voice traffic mix with the rated fleet pre-warmed;
- minimum ASR Capacity Reservations are active in two selected AZs;
- applied AWS G/VT quota is >= the Section 35.17 required quota and has >=30% rated-capacity headroom;
- current AWS Price List evidence and measured voice cost/session are frozen in `VOICE_CAPACITY_MANIFEST.json`;
- ASR and TTS p95 latency remain within Section 91 targets at rated voice load;
- barge-in stops stale playback and cancels pending audio without corrupting conversation state;
- loss of one voice-inference task/AZ degrades capacity but preserves text fallback and recovers without fabricated speech success;
- 3,000 jobs/s burst envelope handled/recovered;
- database connection storm does not exhaust Aurora;
- cache loss degrades performance without losing authoritative data;
- queue backlog drains without duplicate business effects;
- one-AZ impairment recovers;
- Aurora in-Region writer failover/recovery is proven;
- backup restore succeeds;
- AWS/OpenAI quota headroom is documented for the single active Region/provider project;
- no test weakens calculation correctness, evidence validation, privacy, billing or entitlement rules.

Future multi-cell/multi-Region tests are not part of V3 and must not appear as launch blockers. If a future specification activates them, it must add its own acceptance programme.

---

# 102. CI/CD — LOCKED

Pipeline:

```text
install frozen dependencies
→ validate manifests including SCALE/SERVICE_QUOTA/REGION_CELL/PSYCHOLOGY_EVIDENCE/VOICE/VOICE_CAPACITY
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
- microphone permission purpose strings and voice privacy disclosure;
- restore-purchases flow.

Store screenshots must depict real product UI, not unsupported features.

---

# 105. LEGAL / COMPLIANCE LAUNCH GATES

Before commercial launch:

- Swiss Ephemeris commercial licence complete;
- Mysta character authorship/provenance and supplied consent record archived;
- Mysta synthetic voice provenance/rights manifest complete and confirms no identifiable-person voice clone;
- Kokoro/Whisper/faster-whisper/CTranslate2 runtime licences and required notices archived;
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
- SCALE_MANIFEST, SERVICE_QUOTA_MANIFEST, REGION_CELL_MANIFEST, VOICE_MANIFEST and VOICE_CAPACITY_MANIFEST approved;
- single-region scale/resilience acceptance evidence complete for the declared architecture version.

This specification is technical/product design, not a substitute for jurisdiction-specific legal advice.

---

# 106. MYSTA VOICE SYSTEM — SELF-HOSTED / NO METERED SPEECH API

## 106.1 Product rule

Voice is part of the launch product. MystaAI owns the voice experience end to end.

The production path is:

```text
USER MICROPHONE
  ↓
CLIENT CAPTURE + LOCAL ECHO/PLAYBACK CONTROL
  ↓ encrypted WebSocket
VOICE GATEWAY
  ↓
SELF-HOSTED WHISPER ASR
  ↓
ACCEPTED TRANSCRIPT
  ↓
PRIVACY / SAFETY PREPROCESSING
  ↓
EXISTING MYSTA CONVERSATION ENGINE + TOOLS
  ↓
TRUTH / EVIDENCE / TRADITION VALIDATOR
  ↓
SAFETY VALIDATOR
  ↓
FINAL VALIDATED MYSTA TEXT
  ↓
SELF-HOSTED KOKORO + mysta_voice_v1
  ↓ streaming Opus
USER HEARS MYSTA
```

There is no normal production call to a third-party speech-to-text or text-to-speech API. OpenAI GPT-5.6 Terra remains the language/reasoning provider and therefore still has its normal model-token cost; speech recognition and speech synthesis incur only MystaAI infrastructure cost.

## 106.2 Mysta synthetic voice creation

The voice is created and frozen before production voice integration is accepted.

Creation constraints:

- no recording of the user's mother is used;
- no public figure, actor, narrator or other identifiable person's voice is used as a target;
- no external voice-cloning API is used;
- source embeddings/assets must come from the rights-cleared Kokoro distribution or other separately approved source recorded in `VOICE_MANIFEST.json`;
- the finished voice must remain recognisably the same across every sentence, device and session.

Creation procedure:

1. Vendor the pinned Kokoro v1.0 model and its included voice assets after licence/hash verification.
2. Generate a controlled candidate set using an internal voice-design script that operates only in the licensed Kokoro voice-embedding space. Candidate creation may blend/interpolate approved source embeddings and approved model-supported prosody controls; it must not fit to human reference audio.
3. Render every candidate against the same locked evaluation corpus covering ordinary dialogue, mystical vocabulary, names, numbers, dates, astrology terms, tarot terms, safety language and difficult English phonemes.
4. Reject candidates with clipping, unstable timbre, pronunciation failures above the benchmark threshold or materially inconsistent identity.
5. Human product approval selects the final Mysta voice for warmth, calmness, naturalness, intelligence and brand fit.
6. Freeze the selected embedding as `mysta_voice_v1.pt`; record SHA-256, evaluation results and reference audio in `VOICE_MANIFEST.json`.
7. Production can load only the manifest-approved voice hash. A changed voice requires `mysta_voice_v2` plus a controlled specification/asset revision.

The candidate-search implementation is a build tool, not a runtime dependency. The final production service requires only Kokoro plus the frozen Mysta voice asset.

## 106.3 Speech recognition

The production ASR service uses the pinned Whisper large-v3-turbo source model through the frozen faster-whisper/CTranslate2 runtime.

Rules:

- English is the launch voice language unless a later language pack is explicitly added;
- client audio is resampled to 16 kHz mono for ASR;
- server endpointing/VAD determines speech segments;
- partial transcripts are display-only;
- only a final accepted transcript enters the Mysta conversation engine;
- low-confidence/noisy/ambiguous input follows `TRANSCRIPT_CONFIRMATION_REQUIRED` rather than guessing;
- no raw audio is retained after the turn is resolved;
- the ASR service has no public internet endpoint.

The exact confidence/confirmation thresholds are calibrated against the locked voice-ASR evaluation corpus in the voice acceptance phase and then frozen in `VOICE_MANIFEST.json`; they are not arbitrary magic numbers chosen during runtime coding.

## 106.4 Speech synthesis

The TTS service receives only validated Mysta response text and these bounded controls:

```text
voice_id = mysta_voice_v1
locale = en-GB / approved neutral-English mapping
speech_mode = conversational | reflective | reading | safety
playback_speed = approved profile value
request_id
conversation_id
message_id
```

Speech modes alter only approved pacing/chunking; they may not change the voice identity or content.

Default rendering profiles:

```text
conversational   normal pace, shortest natural pause profile
reflective       slightly slower pacing and longer sentence-boundary pauses
reading          measured ceremonial pacing without theatrical exaggeration
safety           clear neutral pacing; no mystical dramatization
```

TTS is sentence/chunk streamed so playback can begin before the entire audio file is rendered. The text is already validated before synthesis; chunking cannot rewrite it.

## 106.5 Audio transport

Transport is authenticated WebSocket over TLS through the regional ALB. Voice inference services remain private.

Input:

```text
client capture → Opus frames → voice gateway → decode/resample → ASR
```

Output:

```text
Kokoro 24 kHz mono PCM → Opus encoder → ordered audio chunks → client jitter buffer/player
```

Every audio chunk carries session/message/sequence metadata. Out-of-order or stale chunks are discarded. Client reconnect uses the last acknowledged sequence; raw microphone frames are never replayed automatically after reconnect.

## 106.6 Barge-in

Barge-in is mandatory.

While Mysta is speaking, the client continues foreground speech detection with echo-cancellation/voice-communication capture settings. When genuine user speech is detected:

```text
stop local Mysta playback
→ send interrupt
→ cancel remaining TTS chunks
→ mark assistant delivery interrupted
→ capture new user turn
```

An interrupted response is never replayed automatically. The text transcript visibly marks the interruption boundary.

## 106.7 Permissions and device behaviour

Mobile uses `expo-audio`; web uses standard browser media APIs.

Rules:

- foreground voice only at launch;
- microphone permission on demand;
- denied permission leaves full text-input Mysta functionality intact;
- app backgrounding ends/pause-safe-closes microphone capture;
- Bluetooth/headset route changes are handled without silently switching to device microphone in a way that surprises the user;
- phone-call/audio-focus interruptions stop or pause voice safely;
- microphone indicator is visible while capture is active.

## 106.8 Voice data model

Add:

```text
VoiceSession
  id
  user_id
  conversation_id
  protocol_version
  started_at
  ended_at
  state
  client_platform
  stt_model_version
  tts_model_version
  voice_asset_version
  total_input_ms
  total_output_ms
  interruption_count
  reconnect_count
  failure_code

VoiceTurn
  id
  voice_session_id
  message_id
  direction
  accepted_transcript_text   // user turns only
  spoken_text_hash           // assistant turns; canonical text remains Message
  input_duration_ms
  output_duration_ms
  interrupted
  stt_latency_ms
  tts_first_audio_ms
  created_at
```

No raw audio/blob column exists in the production schema.

## 106.9 Voice compute topology

```text
regional ALB
  → voice-gateway service (ECS/Fargate)
      → private ASR service
           → GPU ECS/EC2 capacity provider
           → 2-AZ reserved minimum floor
      → existing Mysta/API services
      → private TTS service
           → CPU ECS/Fargate by default
           → GPU ECS/EC2 only when the Phase-88 benchmark selects GPU_TTS
```

ASR and TTS scale independently. ASR is GPU-backed. Kokoro TTS is CPU-first and is not allowed to consume GPU merely because a GPU path exists. Exact compute targets, AZs, quotas, reservations, throughput and prices come from the measured Section 35.17 Phase-88 evidence and are rechecked before Phase 107 provisioning.

Accelerator instances are not pre-provisioned for the theoretical 3M account count. The minimum HA floor is reserved; capacity above that floor scales against measured concurrent voice use and the fixed `voice_mix_v1` acceptance envelope.

No public model downloads occur in production. Models are downloaded/verified in the build pipeline, baked into immutable images or approved immutable model artefacts, and deployed from MystaAI-controlled registries/storage.

## 106.10 Failure behaviour

```text
microphone denied             → text mode remains available
voice gateway unavailable     → text fallback
ASR unavailable               → keep audio transient, fail turn, ask user to type/retry; do not fabricate transcript
ASR ambiguous                 → transcript confirmation/retry
Mysta/OpenAI unavailable     → existing provider-unavailable behaviour; no TTS fabrication
TTS unavailable               → show validated text immediately and offer retry voice playback
client disconnect             → 30s reconnect grace, then clean session end
voice capacity saturated      → bounded queue/explicit busy state, text fallback
```

The voice layer can degrade to text. It cannot degrade truth, safety or privacy.

## 106.11 Security

- authenticated session required before WebSocket upgrade;
- entitlement checked server-side;
- short-lived voice session token derived from authenticated session, audience-bound to voice gateway;
- per-user one-active-session invariant;
- binary frame size/rate limits;
- malformed Opus/frame/protocol input rejected;
- no user-controlled filesystem paths/model identifiers;
- inference containers run non-root, read-only model volumes, restricted IAM and no public ingress;
- raw audio excluded from traces/error payloads/Sentry/PostHog;
- voice model hashes verified at process start;
- dependency/container SBOM and vulnerability scan required.

## 106.12 Voice observability

Metrics:

```text
active sessions
session starts/s
connect/reconnect latency
input audio seconds
accepted turns
ASR real-time factor
ASR p50/p95/p99 latency
transcript confirmation rate
TTS real-time factor
TTS time-to-first-audio p50/p95/p99
output audio seconds
barge-in stop latency
ASR/TTS queue depth and age
GPU utilisation/memory
per-instance concurrent sessions
voice fallbacks/errors by code
voice compute cost/session
voice compute cost/1,000 voice minutes
ASR reserved-floor utilisation
AWS G/VT quota utilisation
AWS Fargate On-Demand vCPU quota utilisation
Capacity Reservation utilisation
fleet desired/running/pending instance/task counts
cold scale-out time
```

No metric label contains transcript text or raw audio.

## 106.13 Voice quality acceptance corpus

The repository contains a rights-safe synthetic/text evaluation corpus under:

```text
tests/voice/corpus/
```

It covers:

- ordinary UK/international English dialogue;
- tarot card names;
- zodiac/planet/house/aspect terms;
- numerology terminology;
- dates, times and numbers;
- common personal names and place names;
- emotionally sensitive but non-diagnostic safety phrasing;
- punctuation, questions, lists and long sentences;
- noisy-room and headset microphone fixtures generated/recorded with rights clearance.

Acceptance requires:

- approved Mysta identity in blind internal voice review;
- no material clipping/corruption;
- no systematic pronunciation defect in locked domain vocabulary;
- ASR word-error benchmark meets the threshold frozen during Phase 88 voice calibration;
- Section 91 latency targets at rated load;
- barge-in works on physical iOS and Android devices plus supported web browsers;
- no raw audio persists after test retention cleanup;
- no speech API billing credential exists in production configuration.

## 106.14 Cost rule

Mysta voice is **not billed per API character/token**. Cost comes from the AWS compute used to run the installed ASR/TTS models, networking and ordinary storage/observability. Capacity scales with demand and can be optimised by batching, quantisation only after quality acceptance, caching of deterministic/static spoken phrases, and accelerator selection.

No optimisation may replace `mysta_voice_v1` with a generic provider voice or send user audio to a third-party speech API without a future controlled specification revision.

---

# 107. LIVING MYSTA AVATAR SYSTEM — LAUNCH LOCK

## 107.1 Purpose

The live avatar makes Mysta visually present during subscriber conversation at launch. It is not a second intelligence system.

## 107.2 Provider contract

Primary provider is **LemonSlice Enterprise**. Required contract/manifest fields:

```text
provider_account_id
contract_id/effective_date
launch_model
BYO_voice=true
BYO_LLM=true
ZDR=true
Actions=true
Emotions=true
data_residency_terms
contracted_concurrency >= 1429
render_rate_usd_per_billable_minute <= 0.16
billing_quantum_seconds
usage_export/reconciliation_method
support/SLA contacts
DPA/subprocessors reviewed
```

Any missing/failed field blocks Phase 1. There is no automatic provider substitution.

## 107.3 Realtime transport

LiveKit Cloud Scale is the transport. New project must be created with **EU (Frankfurt)** project data region. Production does not use LiveKit Inference or ordinary Agent Observability recordings/transcripts. The system uses short-lived room tokens with minimum permissions.

Launch-rated design floor:

```text
avatar-first demand attempt rate                      100% of admitted Mysta sessions unless already accessibility/technical non-video
launch-provisioned avatar-active Mysta                >=1,000 concurrent
minimum LemonSlice contract at 1,000 floor            >=1,429 concurrent
3M/Ultra full-allowance average contract path floor   >=6,846 concurrent without application/protocol redesign
LiveKit launch connections                            >=ceil(launch_avatar_concurrency × measured_participants/session / 0.70)
LiveKit design-envelope expansion path                >=ceil(6,846 × measured_participants/session / 0.70)
```

The public/self-serve LiveKit plan limit is not treated as a permanent architectural ceiling. Actual account/contract values, participants/session and provider session-start/ramp limits are frozen in manifests and tested. If the required design-envelope expansion cannot be contracted without changing the Mysta session protocol, the 2–3M design claim fails Phase 0.

## 107.4 Provider isolation

LemonSlice receives only what is necessary to render Mysta. Its avatar participant is not authorised to call Mysta tools, retrieve user databases, access birth profile, inspect billing or write conversation history.

## 107.5 State/action controller

Create `packages/avatar-gateway` with a strict finite-state controller. LLM output can request semantic response tone only through validated structured metadata; the controller maps approved application states to whitelisted avatar prompts/actions. Untrusted free-form LLM animation instructions are prohibited.

## 107.6 Asset lock

`mysta-avatar-source-v1.png` and any alternate approved crop are versioned/hashes locked. Provider-generated identity drift is tested against approved human visual review fixtures. No real-person identity is introduced by avatar generation.

## 107.7 Usage settlement

The server creates `avatar_usage_segments` only after avatar media readiness. Every segment records provider session ID, LiveKit room, start/stop monotonic timestamps, provider usage evidence, ledger debit event and reconciliation status. Client timers are display-only.

## 107.8 Capacity/economics

`AVATAR_CAPACITY_MANIFEST.json` contains:

```text
avatar_start_attempt_rate_for_capacity=1.00
launch_provisioned_avatar_concurrency
launch_rated_test_sessions
contracted_sessions
required_contract_sessions_at_30pct_headroom
provider_documented_scaling_path_sessions
full_allowance_3m_ultra_average_floor=4792
design_envelope_contract_path_floor=6846
contract_headroom_percent
provider_session_start_limit
provider_ramp_limit
attempted_avatar_concurrency_p50_p95_p99
capacity_fallback_rate
LiveKit_connection_limit
estimated_participants_per_session
required_LiveKit_connections_launch
required_LiveKit_connections_design_envelope
provider_p95_start_latency
provider_failure_rate
provider_rate
billing_quantum
LiveKit_fixed_monthly_price
LiveKit_participant_minute_price
LiveKit_data_transfer_price
full-use Premium/Ultra variable-cost model by channel
purchased_avatar_minute_full-redemption_cost_model
load-test evidence
```

Launch rated acceptance runs at **the full `launch_provisioned_avatar_concurrency`**, never less than 1,000, against a production-equivalent environment under an agreed provider load-test window. The test models every admitted Mysta session as an avatar-start attempt; it may not manufacture a cheaper/passable mixed-mode split. Session-start/ramp tests stay inside the signed provider contract and record the maximum accepted start rate. Cost evidence assumes full consumption of included allowances and full redemption of purchased avatar minutes.

## 107.9 Failure acceptance

Test provider timeout, join failure, media loss, LiveKit disconnect, duplicate webhook/usage event, stale room token, entitlement loss, AVA exhaustion and service recovery. Required result is Section 24.1A first-class audio/text continuity, no double debit, no lost conversation state, no lost tool/evidence state, no duplicated Mysta response and successful reattachment of the avatar to the existing session.

## 107.10 Visual acceptance

On web/iOS/Android, approved recordings demonstrate:

- recognisable consistent Mysta face/character;
- natural idle/listening/speaking/reflection behaviour;
- lip sync within Section 91 target;
- no clipping/warping that materially breaks identity;
- safe concerned state;
- reduced-motion/caption/text fallbacks;
- no generic chatbot presentation.

---

# 108. COMPLETE ZERO-GUESSWORK BUILD PLAN — PHASE 0–114

This section is the executable build order. The research required to choose the implementation is already represented here and in the locked design sections above. **No phase instructs the builder to decide what technology, model, source, licence, database, queue, provider or algorithm to use.**

When an exact third-party value is volatile, V3 supplies: (a) the researched baseline, (b) the official source to query, (c) the exact lifecycle phase that captures the live value, and (d) the pass/block rule. That is a deterministic verification step, not open-ended research.

Every phase follows the same contract: outcome → locked inputs → repository targets → commands/acquisition → implementation → credentials → verification → done gate. Do not start the next phase until the current done gate passes.


## Phase 0 — Build-readiness research lock and execution map

**Outcome**  
Create the V3 execution-control artefacts from this document so the repository starts with the researched dependency/source/rights map rather than blank TODOs.

**Locked inputs / researched sources**
- This V3 document is authoritative.
- Current baseline registry/source matrix in Section 35 and source register in Section 112.
- No production contract, AWS quota increase, Capacity Reservation, App Store product or LemonSlice commitment is required to finish this phase.

**Repository targets**
- `docs/build/V3_IMPLEMENTATION_INDEX.md`
- `manifests/DEPENDENCY_MANIFEST.json`
- `manifests/KNOWLEDGE_SOURCE_MANIFEST.json`
- `manifests/ENVIRONMENT_MANIFEST.json`
- `docs/operations/evidence/build-start/`

**Exact commands / acquisition**
```bash
node --version
npm --version
git --version
docker --version
# install pnpm exactly if not already present
npm install --global pnpm@12.5.1
pnpm --version
```

**Implementation**
- Copy the V3 phase index into `docs/build/V3_IMPLEMENTATION_INDEX.md`.
- Populate dependency/source manifest entries with the exact researched baselines from V3; mark only genuinely account-created values as `lifecycle_capture`, never as developer-choice TODOs.
- Create `infra/scripts/preflight-lock.ts` contract: `build-start`, `integration`, `prelaunch` modes.
- Record the workstation OS/tool versions used for the build.

**Credentials / paid items at this phase**
- None. This phase must not force purchase of production services.

**Verification commands**
```bash
node -e "if(process.versions.node.split('.')[0] !== '24') process.exit(1)"
pnpm --version
git status --short
```

**Phase complete only when**
- Node major 24 is active and pnpm 12.5.1 is available.
- No dependency/source row lacks owner, official URL, licence status or target phase.
- All account-specific unknowns name the exact phase where they will be captured.


## Phase 1 — Dependency manifest and exact lock preparation

**Outcome**  
Freeze the exact JavaScript/Python/tooling baselines used to bootstrap the repo.

**Locked inputs / researched sources**
- Node 24.21.0 LTS; pnpm 12.5.1; Turbo 2.11.2; TypeScript 6.0.2; ESLint 10.11.0; Prettier 3.9.9; Vitest 5.0.1; Playwright 1.63.0.
- Python knowledge/privacy: 3.14.7. Voice container: Python 3.12.14 because kokoro 0.9.4 requires <3.13.
- Prisma trio 7.10.0; no prerelease ORM.

**Repository targets**
- `manifests/DEPENDENCY_MANIFEST.json`
- `tools/lock/verify-dependencies.mjs`
- `tools/lock/resolve-stable.mjs`
- `tools/lock/install-resolved.mts`
- `tools/lock/python-constraints.txt`

**Exact commands / acquisition**
```bash
npm view pnpm@12.5.1 version
npm view turbo@2.11.2 version
npm view typescript@6.0.2 version
npm view prisma@7.10.0 version
npm view openai@7.23.0 version
npm view @fastify/cors dist-tags.latest
npm view @fastify/helmet dist-tags.latest
npm view @fastify/cookie dist-tags.latest
npm view @fastify/rate-limit dist-tags.latest
npm view @fastify/swagger dist-tags.latest
npm view @fastify/swagger-ui dist-tags.latest
npm view @fastify/sensible dist-tags.latest
npm view @fastify/websocket dist-tags.latest
npm view stripe dist-tags.latest
npm view @sentry/nextjs dist-tags.latest
npm view @sentry/node dist-tags.latest
npm view @sentry/react-native dist-tags.latest
npm view posthog-js dist-tags.latest
npm view posthog-node dist-tags.latest
npm view posthog-react-native dist-tags.latest
npm view @aws-sdk/client-s3 dist-tags.latest
npm view @aws-sdk/s3-request-presigner dist-tags.latest
npm view @aws-sdk/client-sesv2 dist-tags.latest
npm view @aws-sdk/client-sqs dist-tags.latest
npm view @aws-sdk/client-cloudwatch dist-tags.latest
npm view @aws-sdk/client-application-auto-scaling dist-tags.latest
python3 --version
```

**Implementation**
- Create `tools/lock/resolve-stable.mjs` and `tools/lock/install-resolved.mts`; these implement the deterministic `dist-tags.latest` resolution for the explicitly listed fast-moving SDKs, verify engines/peer compatibility/licence, and write exact values before install.
- Record resolved tarball integrity/license metadata.
- Reject prerelease tags and any exact baseline that is yanked/critically compromised.
- For V3 hard-pinned packages, do not upgrade merely because a newer release exists; only a critical security/deprecation event triggers controlled review.

**Credentials / paid items at this phase**
- None.

**Verification commands**
```bash
node tools/lock/verify-dependencies.mjs
python -m json.tool manifests/DEPENDENCY_MANIFEST.json >/dev/null
```

**Phase complete only when**
- Every baseline exists and has an approved licence/security state.
- The manifest contains no floating `latest`, caret or tilde runtime dependency.


## Phase 2 — Repository bootstrap

**Outcome**  
Create the complete pnpm/Turborepo monorepo and root quality commands.

**Locked inputs / researched sources**
- Monorepo names are `@mystaai/<package>`; apps are private.
- No empty production package is considered complete.

**Repository targets**
- root config files
- all `apps/*`, `packages/*`, `assets/*`, `knowledge/*`, `vendor/*`, `infra/*`, `store/*`, `docs/*`, `tests/*` directories from Section 36

**Exact commands / acquisition**
```bash
mkdir mystaai && cd mystaai
git init
pnpm init
pnpm add -Dw turbo@2.11.2 typescript@6.0.2 eslint@10.11.0 prettier@3.9.9 vitest@5.0.1 @playwright/test@1.63.0
```

**Implementation**
- Create `pnpm-workspace.yaml`, `turbo.json`, `tsconfig.base.json`, `eslint.config.mjs`, `.prettierrc`, `.gitignore`, `.node-version`.
- Create the complete Section 36 directory tree.
- Root scripts: `lint`, `typecheck`, `test`, `test:e2e`, `format:check`, `build`.

**Credentials / paid items at this phase**
- None.

**Verification commands**
```bash
pnpm install --frozen-lockfile
pnpm lint
pnpm typecheck
pnpm test
```

**Phase complete only when**
- Fresh-clone bootstrap is documented and passes.
- No production dependency uses a floating range.


## Phase 3 — Manifest schemas

**Outcome**  
Create JSON Schema + TypeScript validators for every required manifest.

**Locked inputs / researched sources**
- Required manifest list in Section 0.4.
- Zod 4.6.5 is the runtime validation layer.

**Repository targets**
- `manifests/schemas/*.schema.json`
- `packages/validation/src/manifests/*`
- `tools/validate-manifests.ts`

**Exact commands / acquisition**
```bash
pnpm --filter @mystaai/validation add zod@4.6.5
pnpm add -Dw ajv@8.20.0 ajv-formats@3.0.1
```

**Implementation**
- Define required fields, enums and `additionalProperties:false` where appropriate.
- Disallow `TBD`, `TODO`, `unknown` for applicable production fields; lifecycle-capture fields must instead use a typed `{status:"capture_at_phase", phase:number}` object.
- Wire manifest validation into root CI.

**Credentials / paid items at this phase**
- None.

**Verification commands**
```bash
pnpm tsx tools/validate-manifests.ts
pnpm test --filter manifest
```

**Phase complete only when**
- All valid manifests pass.
- Deliberately invalid fixtures fail CI.


## Phase 4 — Brand asset foundation

**Outcome**  
Install the approved Mysta/brand references and licensed fonts with provenance.

**Locked inputs / researched sources**
- Mysta character is original and consent/provenance record is retained.
- Inter OFL: https://github.com/rsms/inter
- Playfair Display OFL: https://github.com/google/fonts/tree/main/ofl/playfairdisplay

**Repository targets**
- `assets/brand/mysta/avatar/mysta-reference-v1.*`
- `assets/brand/fonts/`
- `manifests/BRAND_MANIFEST.json`
- `manifests/FONT_MANIFEST.json`
- `docs/brand/provenance.md`

**Exact commands / acquisition**
```bash
mkdir -p assets/brand/{mysta/avatar,fonts,logo,icons}
sha256sum assets/brand/mysta/avatar/* || true
```

**Implementation**
- Store supplied approved image locally; do not regenerate identity implicitly.
- Download font release files from official repositories, archive OFL text, hash files.
- Create the exact colour/type token JSON from Section 8.

**Credentials / paid items at this phase**
- No paid licence required for Inter/Playfair Display; retain OFL notices.

**Verification commands**
```bash
pnpm test --filter brand
pnpm test --filter design-tokens
```

**Phase complete only when**
- All asset hashes/provenance recorded.
- No unlicensed visual/font asset exists.


## Phase 5 — MystaAI logo, sigil and icon

**Outcome**  
Create the original MystaAI logo/sigil/app-icon family without copying competitors.

**Locked inputs / researched sources**
- Locked brand colours/type/character rules.
- Original artwork only.

**Repository targets**
- `assets/brand/logo/`
- `assets/brand/icons/`
- `manifests/ASSET_MANIFEST.json`

**Exact commands / acquisition**
```bash
pnpm tsx tools/assets/validate-brand-assets.ts
```

**Implementation**
- Export SVG master + PNG/web app/mobile icon sizes.
- Record creator/source/date/hash/rights.
- Run small-size and dark/light surface checks.

**Credentials / paid items at this phase**
- None unless a commissioned designer is used; if so, written assignment/licence must be archived.

**Verification commands**
```bash
pnpm test --filter visual -- --grep "logo|icon"
pnpm tsx tools/assets/validate-brand-assets.ts
```

**Phase complete only when**
- Originality review passes.
- All required icon sizes render legibly.


## Phase 6 — Shared configuration

**Outcome**  
Build shared typed config, validation, logging, IDs, errors, region/cell and idempotency contracts.

**Locked inputs / researched sources**
- Zod 4.6.5; Node crypto; V3 environment/source manifests.

**Repository targets**
- `packages/config`
- `packages/shared`
- `packages/validation`
- `packages/logging`
- `packages/design-tokens`

**Exact commands / acquisition**
```bash
pnpm --filter @mystaai/config add zod@4.6.5
pnpm --filter @mystaai/logging add pino@10.3.1
```

**Implementation**
- Implement env schema and redacted config loader.
- Use UUIDv7/ULID strategy selected in shared constants; never IDs from Math.random.
- Define `home_region`, `cell_id`, `routing_version`, queue idempotency key and error taxonomy.

**Credentials / paid items at this phase**
- No secrets committed; `.env.example` contains names only.

**Verification commands**
```bash
pnpm --filter @mystaai/config test
pnpm --filter @mystaai/shared test
pnpm --filter @mystaai/logging test
```

**Phase complete only when**
- Invalid env fails startup.
- PII/secrets are redacted in log fixtures.


## Phase 7 — UI component foundation

**Outcome**  
Build the reusable original design system used by web/mobile.

**Locked inputs / researched sources**
- Web: React 19.3.0, Tailwind 4.3.3. Mobile consumes platform-neutral tokens.
- WCAG 2.2 AA.

**Repository targets**
- `packages/ui`
- `packages/design-tokens`
- `tests/visual/components`

**Exact commands / acquisition**
```bash
pnpm --filter @mystaai/ui add react@19.3.0 zod@4.6.5
pnpm --filter @mystaai/ui add -D tailwindcss@4.3.3
```

**Implementation**
- Create buttons, cards, input, modal/sheet, tabs, reading cards, evidence panels, loading/error/empty states, Mysta status surfaces.
- Keyboard/focus/reduced-motion semantics built in, not added later.

**Credentials / paid items at this phase**
- None.

**Verification commands**
```bash
pnpm --filter @mystaai/ui test
pnpm playwright test tests/visual/components
```

**Phase complete only when**
- All component states have accessibility + visual baselines.


## Phase 8 — Local infrastructure

**Outcome**  
Run local PostgreSQL/pgvector/Redis and approved dev queue adapter with reproducible containers.

**Locked inputs / researched sources**
- PostgreSQL 18.6; pgvector 0.8.6; Redis current stable container pinned by digest.
- Production queue remains SQS; local adapter may use ElasticMQ/LocalStack only for development/tests.

**Repository targets**
- `infra/docker/docker-compose.local.yml`
- `infra/scripts/local-up.sh`
- `infra/scripts/local-down.sh`

**Exact commands / acquisition**
```bash
docker compose -f infra/docker/docker-compose.local.yml up -d
docker compose -f infra/docker/docker-compose.local.yml ps
```

**Implementation**
- Create healthchecks and persistent dev volumes.
- Enable pgvector extension.
- Expose only local ports 5432/6379 and dev queue endpoint.

**Credentials / paid items at this phase**
- None.

**Verification commands**
```bash
pnpm tsx infra/scripts/check-local.ts
```

**Phase complete only when**
- One command starts all local dependencies and all healthchecks pass.


## Phase 9 — Database foundation

**Outcome**  
Create the authoritative relational schema/migrations.

**Locked inputs / researched sources**
- Prisma 7.10.0 trio; PostgreSQL 18.6 local; production compatibility with Aurora PostgreSQL 18.x + pgvector.

**Repository targets**
- `packages/database/prisma/schema.prisma`
- `packages/database/prisma/migrations/`
- `packages/database/src/client.ts`
- `packages/database/src/seed.ts`

**Exact commands / acquisition**
```bash
pnpm --filter @mystaai/database add prisma@7.10.0 @prisma/client@7.10.0 @prisma/adapter-pg@7.10.0 pg@8.23.0
pnpm --filter @mystaai/database exec prisma generate
pnpm --filter @mystaai/database exec prisma migrate dev --name foundation
```

**Implementation**
- Model users, auth/session linkage, birth profiles, readings/evidence, knowledge/version/source/chunks, wallets/ledger, subscriptions/entitlements, Mysta sessions/turns, voice/avatar usage, reports, notifications, audit events, `user_routing`.
- Add encryption metadata and deletion/export status fields.

**Credentials / paid items at this phase**
- Local `DATABASE_URL`; production secret is Phase 108.

**Verification commands**
```bash
pnpm --filter @mystaai/database exec prisma migrate reset --force
pnpm --filter @mystaai/database test
```

**Phase complete only when**
- Empty DB → migrate → seed → query passes.
- All monetary/credit fields use integer minor units/ledger deltas, never float.


## Phase 10 — Authentication

**Outcome**  
Implement shared secure authentication for web/mobile/admin.

**Locked inputs / researched sources**
- better-auth 1.7.6; email/password, verification, reset, Google, Apple; admin MFA.

**Repository targets**
- `packages/auth`
- `apps/api/src/routes/auth/*`
- `apps/web/app/(auth)/*`
- `apps/mobile/app/(auth)/*`

**Exact commands / acquisition**
```bash
pnpm --filter @mystaai/auth add better-auth@1.7.6
pnpm --filter @mystaai/api add @fastify/cookie @fastify/rate-limit
```

**Implementation**
- Server-authoritative sessions; secure/httpOnly cookies on web; SecureStore-backed token/session material mobile only where required by Better Auth adapter design.
- CSRF/origin protections, login throttling, session revocation, audit.

**Credentials / paid items at this phase**
- `GOOGLE_CLIENT_ID`, `GOOGLE_CLIENT_SECRET`, Apple Sign In identifiers/keys when social auth is enabled; email provider credentials later Phase 74.

**Verification commands**
```bash
pnpm --filter @mystaai/auth test
pnpm playwright test tests/acceptance/auth.spec.ts
```

**Phase complete only when**
- Password/reset/verification/social/session-revocation tests pass.
- Admin MFA enforced.


## Phase 11 — Profile and encrypted birth data

**Outcome**  
Implement profile/birth data with strict privacy boundaries.

**Locked inputs / researched sources**
- AES-GCM/KMS envelope design in security section; date/time/place/time-confidence model.

**Repository targets**
- `packages/database` models
- `apps/api/src/routes/profile/*`
- `packages/shared/src/birth-profile.ts`

**Exact commands / acquisition**
```bash
pnpm --filter @mystaai/api test -- --grep birth
```

**Implementation**
- Persist name/preferences separately from encrypted sensitive birth fields where practical.
- Model birth-time confidence: exact/approximate/unknown; never fabricate houses/Ascendant if unknown.
- Add consent timestamps and delete/export ownership.

**Credentials / paid items at this phase**
- Local dev encryption key only in `.env.local`; production KMS key in Phase 108.

**Verification commands**
```bash
pnpm test --filter profile
pnpm test --filter privacy -- --grep birth
```

**Phase complete only when**
- Sensitive values never appear in logs.
- Cross-user access tests fail closed.


## Phase 12 — Onboarding UI

**Outcome**  
Build and persist the complete onboarding flow.

**Locked inputs / researched sources**
- Locked MystaAI visual system; profile/birth schemas from Phase 11.

**Repository targets**
- `apps/web/app/onboarding/*`
- `apps/mobile/app/onboarding/*`
- `packages/ui/src/onboarding/*`

**Exact commands / acquisition**
```bash
pnpm playwright test tests/acceptance/onboarding.web.spec.ts
```

**Implementation**
- Implement welcome, age gate, account/profile, birth data/time confidence, consent/privacy, interests/preferences, first-profile completion.
- Resume safely after app/browser interruption.

**Credentials / paid items at this phase**
- None beyond auth account.

**Verification commands**
```bash
pnpm playwright test tests/acceptance/onboarding.web.spec.ts
pnpm --filter @mystaai/mobile test -- --grep onboarding
```

**Phase complete only when**
- Onboarding persists, resumes and passes accessibility/visual regression.


## Phase 13 — GeoNames integration

**Outcome**  
Integrate production-grade place search/geocoding for birth location.

**Locked inputs / researched sources**
- GeoNames Premium Web Services Best Availability; initial 1,000,000 credits/year researched price €250/year; official source https://www.geonames.org/commercial-webservices.html.

**Repository targets**
- `packages/timezone/src/geonames.ts`
- `apps/api/src/routes/location/*`
- `manifests/INTEGRATION_MANIFEST.json`

**Exact commands / acquisition**
```bash
curl -sS "https://secure.geonames.net/searchJSON?q=London&maxRows=1&username=$GEONAMES_USERNAME" | jq .
```

**Implementation**
- Implement server-side search proxy, canonical `geonameId`, lat/lon/country/admin/timezone fields, caching, timeout/retry and Best Availability failover host support.
- Never expose GeoNames username as a client-side production secret.

**Credentials / paid items at this phase**
- Acquire GeoNames production plan before launch; dev can use a permitted development account. Env: `GEONAMES_USERNAME`, `GEONAMES_PRIMARY_HOST`, `GEONAMES_FAILOVER_HOST`.

**Verification commands**
```bash
pnpm --filter @mystaai/timezone test -- --grep geonames
pnpm --filter @mystaai/api test -- --grep location
```

**Phase complete only when**
- Known-place fixtures resolve canonically.
- Rate/failure path returns explicit unavailable state, not invented coordinates.


## Phase 14 — Timezone engine

**Outcome**  
Implement deterministic coordinate/timezone and historical civil-time handling.

**Locked inputs / researched sources**
- IANA tzdb 2026d, released 11 Sep 2026: https://www.iana.org/time-zones/releases/2026d
- `timezonecomplete 5.15.1`, `tzdata 1.0.51`, exact-compatible `geo-tz` lock from dependency manifest.

**Repository targets**
- `packages/timezone`
- `vendor/tzdb/2026d/`
- `manifests/DEPENDENCY_MANIFEST.json`

**Exact commands / acquisition**
```bash
mkdir -p vendor/tzdb/2026d
curl -L https://data.iana.org/time-zones/releases/tzdata2026d.tar.gz -o vendor/tzdb/2026d/tzdata2026d.tar.gz
sha256sum vendor/tzdb/2026d/tzdata2026d.tar.gz
```

**Implementation**
- Coordinate→IANA zone deterministic mapping; local civil datetime→UTC with DST ambiguous/nonexistent states surfaced.
- Persist tzdb version with each deterministic natal calculation.

**Credentials / paid items at this phase**
- None.

**Verification commands**
```bash
pnpm --filter @mystaai/timezone test
```

**Phase complete only when**
- DST boundary fixtures and historical dates pass; no server-local timezone leakage.


## Phase 15 — Swiss Ephemeris commercial licence

**Outcome**  
Acquire and archive the commercial Swiss Ephemeris licence required for proprietary production use.

**Locked inputs / researched sources**
- Swiss Ephemeris official licensing: https://www.astro.com/swisseph/
- Research baseline: Professional unlimited licence CHF 700; verify invoice amount only when purchasing.

**Repository targets**
- `docs/legal/licences/swisseph/`
- `manifests/SWISSEPH_MANIFEST.json`

**Exact commands / acquisition**
```bash
mkdir -p docs/legal/licences/swisseph
```

**Implementation**
- Store signed agreement/invoice/reference outside public repo as appropriate; commit non-secret licence metadata and rights decision.

**Credentials / paid items at this phase**
- Paid Swiss Ephemeris Professional licence before proprietary public release.

**Verification commands**
```bash
pnpm tsx tools/legal/verify-swisseph-manifest.ts
```

**Phase complete only when**
- Commercial-use authority is documented; public launch remains blocked if absent.


## Phase 16 — Swiss source vendoring

**Outcome**  
Vendor the exact Swiss Ephemeris source and required ephemeris files reproducibly.

**Locked inputs / researched sources**
- Repository https://github.com/aloistr/swisseph ; tag `v2.10.3bfinal`, researched short SHA `f4dcd18`.

**Repository targets**
- `vendor/swisseph/`
- `manifests/SWISSEPH_MANIFEST.json`

**Exact commands / acquisition**
```bash
git clone --depth 1 --branch v2.10.3bfinal https://github.com/aloistr/swisseph.git vendor/swisseph/source
cd vendor/swisseph/source && git rev-parse HEAD
find . -type f -print0 | sort -z | xargs -0 sha256sum > ../SHA256SUMS.txt
```

**Implementation**
- Vendor only files needed by the wrapper/runtime; preserve licence notices.
- Record full commit and every shipped file hash.

**Credentials / paid items at this phase**
- Swiss licence from Phase 15.

**Verification commands**
```bash
sha256sum -c vendor/swisseph/SHA256SUMS.txt
```

**Phase complete only when**
- Full hash verification passes.


## Phase 17 — Native Swiss wrapper

**Outcome**  
Create a typed Node-API Swiss Ephemeris wrapper; no uncontrolled astrology wrapper.

**Locked inputs / researched sources**
- Vendored Swiss source from Phase 16; Node-API.

**Repository targets**
- `packages/swiss-ephemeris-native`
- `packages/swiss-ephemeris-native/native/*`
- `tests/fixtures/astrology/swisseph/*`

**Exact commands / acquisition**
```bash
pnpm --filter @mystaai/swiss-ephemeris-native add node-addon-api
pnpm --filter @mystaai/swiss-ephemeris-native build
```

**Implementation**
- Expose typed wrappers only for required `swe_*` functions; validate numeric inputs and error codes.
- Pin compiler/build container.

**Credentials / paid items at this phase**
- None.

**Verification commands**
```bash
pnpm --filter @mystaai/swiss-ephemeris-native test
```

**Phase complete only when**
- Wrapper outputs match `swetest` fixtures within locked tolerances.


## Phase 18 — Astrology core

**Outcome**  
Implement deterministic natal calculation core.

**Locked inputs / researched sources**
- Swiss native wrapper; V3 tropical/geocentric/True Node/Placidus defaults.

**Repository targets**
- `packages/astrology/src/core/*`
- `tests/fixtures/astrology/core/*`

**Exact commands / acquisition**
```bash
# no new external dependency; implement against already locked packages
```

**Implementation**
- Implement planets/speed/retrograde/signs/houses/Asc/MC/aspects.
- Unknown birth time suppresses houses/Ascendant rather than fabricating them.

**Credentials / paid items at this phase**
- None.

**Verification commands**
```bash
pnpm --filter @mystaai/astrology test
```

**Phase complete only when**
- All deterministic fixtures pass.
- No LLM is authoritative for this phase.


## Phase 19 — Natal chart persistence

**Outcome**  
Persist canonical natal charts with engine/data versions.

**Locked inputs / researched sources**
- Astrology core; timezone/GeoNames canonical input.

**Repository targets**
- `packages/astrology/src/natal/*`
- database natal tables/migration

**Exact commands / acquisition**
```bash
# no new external dependency; implement against already locked packages
```

**Implementation**
- Canonicalize output JSON; store input hash, Swiss version, tzdb version and result hash.
- Replay from stored evidence.

**Credentials / paid items at this phase**
- None.

**Verification commands**
```bash
pnpm --filter @mystaai/astrology test
```

**Phase complete only when**
- All deterministic fixtures pass.
- No LLM is authoritative for this phase.


## Phase 20 — Transit engine

**Outcome**  
Implement current and upcoming transit calculations.

**Locked inputs / researched sources**
- Astrology core and deterministic current-time input.

**Repository targets**
- `packages/astrology/src/transits/*`

**Exact commands / acquisition**
```bash
# no new external dependency; implement against already locked packages
```

**Implementation**
- Calculate transit body positions and transit→natal aspects using versioned orb rules.

**Credentials / paid items at this phase**
- None.

**Verification commands**
```bash
pnpm --filter @mystaai/astrology test
```

**Phase complete only when**
- All deterministic fixtures pass.
- No LLM is authoritative for this phase.


## Phase 21 — Synastry engine

**Outcome**  
Implement deterministic synastry calculations.

**Locked inputs / researched sources**
- Two canonical natal charts.

**Repository targets**
- `packages/astrology/src/synastry/*`

**Exact commands / acquisition**
```bash
# no new external dependency; implement against already locked packages
```

**Implementation**
- Calculate cross-chart aspects and house overlays only when birth times permit.

**Credentials / paid items at this phase**
- None.

**Verification commands**
```bash
pnpm --filter @mystaai/astrology test
```

**Phase complete only when**
- All deterministic fixtures pass.
- No LLM is authoritative for this phase.


## Phase 22 — Solar return engine

**Outcome**  
Implement exact solar return calculation.

**Locked inputs / researched sources**
- Swiss ephemeris root-finding around natal solar longitude.

**Repository targets**
- `packages/astrology/src/solar-return/*`

**Exact commands / acquisition**
```bash
# no new external dependency; implement against already locked packages
```

**Implementation**
- Solve return time deterministically and persist evidence.

**Credentials / paid items at this phase**
- None.

**Verification commands**
```bash
pnpm --filter @mystaai/astrology test
```

**Phase complete only when**
- All deterministic fixtures pass.
- No LLM is authoritative for this phase.


## Phase 23 — Astrology validation gate

**Outcome**  
Prove astrology correctness independently before interpretation AI is allowed to use it.

**Locked inputs / researched sources**
- `swetest` from vendored Swiss distribution.
- JPL Horizons API: https://ssd.jpl.nasa.gov/api/horizons.api for independent planetary validation.

**Repository targets**
- `tests/fixtures/astrology/validation/*`
- `tools/astrology/validate-jpl.ts`
- `docs/operations/evidence/astrology/`

**Exact commands / acquisition**
```bash
python tools/astrology/build-fixtures.py
pnpm tsx tools/astrology/validate-jpl.ts
pnpm --filter @mystaai/astrology test
```

**Implementation**
- Run fixed-date/location corpus, DST cases, poles/edge cases, known retrogrades, house-system fixtures.
- Compare selected planetary longitudes against JPL within documented tolerance.

**Credentials / paid items at this phase**
- No API key required for JPL Horizons public API; obey service etiquette.

**Verification commands**
```bash
pnpm test --filter astrology-validation
```

**Phase complete only when**
- Every required fixture passes; discrepancies are explained and within tolerance.


## Phase 24 — Numerology engine

**Outcome**  
Implement Pythagorean numerology exactly as Section 40.

**Locked inputs / researched sources**
- V3 formula table; no external numerology library.

**Repository targets**
- `packages/numerology/src/*`
- `tests/fixtures/numerology/*`

**Exact commands / acquisition**
```bash
# no new external dependency; implement against already locked packages
```

**Implementation**
- Implement NFKD normalisation, A–Z map, master numbers 11/22/33, Life Path, name numbers, cycles, pinnacles/challenges/karmic fields.
- Unsupported scripts require explicit transliteration; AI cannot invent it.

**Credentials / paid items at this phase**
- None.

**Verification commands**
```bash
pnpm --filter @mystaai/numerology test -- --coverage
```

**Phase complete only when**
- All deterministic fixtures pass.
- No LLM is authoritative for this phase.


## Phase 25 — Zodiac engine

**Outcome**  
Implement zodiac facts/profile layer.

**Locked inputs / researched sources**
- Authoritative solar sign comes from astrology longitude when birth data exists.

**Repository targets**
- `packages/zodiac/src/*`

**Exact commands / acquisition**
```bash
# no new external dependency; implement against already locked packages
```

**Implementation**
- Implement 12-sign static schema, elements/modalities/rulers/decans and deterministic longitude→sign mapping.

**Credentials / paid items at this phase**
- None.

**Verification commands**
```bash
pnpm --filter @mystaai/zodiac test
```

**Phase complete only when**
- All deterministic fixtures pass.
- No LLM is authoritative for this phase.


## Phase 26 — Tarot static domain

**Outcome**  
Create canonical 78-card domain and versioned spreads.

**Locked inputs / researched sources**
- V3 tarot schema and spread list.

**Repository targets**
- `packages/tarot/src/cards.ts`
- `packages/tarot/src/spreads.ts`
- `knowledge/processed/tarot/schema/*`

**Exact commands / acquisition**
```bash
# no new external dependency; implement against already locked packages
```

**Implementation**
- Create exactly 22 Major + 56 Minor IDs; reversals default OFF; spreads include ordered position semantics.

**Credentials / paid items at this phase**
- None.

**Verification commands**
```bash
pnpm --filter @mystaai/tarot test
```

**Phase complete only when**
- All deterministic fixtures pass.
- No LLM is authoritative for this phase.


## Phase 27 — Tarot secure draw

**Outcome**  
Implement cryptographically secure authoritative tarot draws and immutable evidence.

**Locked inputs / researched sources**
- Node `crypto.randomInt`; Fisher–Yates.

**Repository targets**
- `packages/tarot/src/draw.ts`
- `packages/tarot/src/repository.ts`

**Exact commands / acquisition**
```bash
# no new external dependency; implement against already locked packages
```

**Implementation**
- Server-only draw; transactionally persist card/orientation/position/rng method; replay stored draw only.
- Prohibit `Math.random`, timestamp seeds, client authority and LLM card selection.

**Credentials / paid items at this phase**
- None.

**Verification commands**
```bash
pnpm --filter @mystaai/tarot test
```

**Phase complete only when**
- All deterministic fixtures pass.
- No LLM is authoritative for this phase.


## Phase 28 — Knowledge source registry

**Outcome**  
Create the licence/provenance registry that every knowledge install must pass.

**Locked inputs / researched sources**
- Source classes A–E and fields in Section 47.
- Licence validators include CC BY, CC0/public domain, approved commercial licence; unknown/NC is blocked for commercial publishing.

**Repository targets**
- `packages/knowledge/src/source-registry/*`
- `knowledge/manifests/`
- `manifests/KNOWLEDGE_SOURCE_MANIFEST.json`

**Exact commands / acquisition**
```bash
pnpm --filter @mystaai/knowledge add zod@4.6.5
pnpm --filter @mystaai/knowledge test
```

**Implementation**
- Implement source registration, SHA-256, rights status, parser version, retrieval timestamp, immutable raw-storage key and publication gate.
- Separate `rights_status` from `evidence_grade`.

**Credentials / paid items at this phase**
- None.

**Verification commands**
```bash
pnpm --filter @mystaai/knowledge test -- --grep source
```

**Phase complete only when**
- Unknown licence and prohibited source fixtures cannot publish.


## Phase 29 — Tarot knowledge installation

**Outcome**  
Install tarot historical/reference knowledge and transform it into original MystaAI structured tarot knowledge.

**Locked inputs / researched sources**
- Historical source: A. E. Waite, *The Pictorial Key to the Tarot* (1911), https://www.sacred-texts.com/tarot/pkt/pkttp.htm ; archive states pre-1923 US public-domain status.
- Use historical text as a provenance source; production wording is original MystaAI-authored normalized meaning. Do not copy modern commercial tarot apps/books.
- Tarot artwork is original MystaAI artwork; historical scans are not shipped as production art.

**Repository targets**
- `knowledge/raw/tarot/waite-pictorial-key/`
- `knowledge/processed/tarot/cards/*.json`
- `knowledge/manifests/tarot-waite.json`
- `packages/knowledge/src/importers/tarot-waite.ts`
- `tools/knowledge/fetch_tarot_waite.py`
- `tools/knowledge/build-tarot.ts`

**Exact commands / acquisition**
```bash
mkdir -p knowledge/raw/tarot/waite-pictorial-key knowledge/processed/tarot/cards
python tools/knowledge/fetch_tarot_waite.py --source https://www.sacred-texts.com/tarot/pkt/pkttp.htm --out knowledge/raw/tarot/waite-pictorial-key
pnpm tsx tools/knowledge/build-tarot.ts
```

**Implementation**
- Implement respectful fetch with cached raw HTML/text, source hash and chapter URLs.
- Map historical concepts into the canonical 78-card schema; author unique concise MystaAI text for all required fields.
- Validate 78 cards and source IDs.

**Credentials / paid items at this phase**
- None.

**Verification commands**
```bash
pnpm test --filter knowledge-tarot
```

**Phase complete only when**
- Exactly 78 active cards; every field populated and source/provenance resolves; plagiarism/originality check passes.


## Phase 30 — Numerology knowledge installation

**Outcome**  
Install numerology interpretation knowledge tied to the deterministic formulas already implemented.

**Locked inputs / researched sources**
- Arithmetic rules are V3-owned implementation facts.
- Historical/traditional reference: Sepharial, *The Kabala of Numbers* (1911), University of Illinois Brittle Books source `https://brittlebooks.library.illinois.edu/brittlebooks_open/Books2012-04/sephar0001kabofn/`; author died 1929 and the underlying 1911 text is rights-reviewed as public-domain source material in launch jurisdictions before ingestion.
- Supporting public-domain reference: Sepharial, *Cosmic Symbolism* (1912), Project Gutenberg eBook 70749.
- Production interpretation copy is newly authored MystaAI content; no modern proprietary numerology text is copied.

**Repository targets**
- `knowledge/raw/numerology/sepharial/`
- `knowledge/processed/numerology/*.json`
- `knowledge/manifests/numerology-sepharial.json`
- `tools/knowledge/fetch-numerology-sources.py`
- `tools/knowledge/build-numerology.ts`

**Exact commands / acquisition**
```bash
pnpm tsx tools/knowledge/build-numerology.ts
```

**Implementation**
- Create meanings for 1–9, 11/22/33, Life Path, Expression, Soul Urge, Personality, cycles, pinnacles/challenges and required compatibility themes.
- Each interpretation explicitly references deterministic number evidence rather than recalculating with AI.

**Credentials / paid items at this phase**
- None.

**Verification commands**
```bash
pnpm test --filter knowledge-numerology
```

**Phase complete only when**
- Every deterministic output token has an interpretation record; no orphan records.


## Phase 31 — Zodiac knowledge installation

**Outcome**  
Install original zodiac/sign knowledge.

**Locked inputs / researched sources**
- Twelve signs/elements/modalities/rulers from deterministic schema.
- Historical reference: Sepharial, *Astrology: How to Make and Read Your Own Horoscope* (1920), Project Gutenberg eBook 46963; underlying text is public domain in the USA and author-death/publication dates are recorded for launch-jurisdiction rights review.
- Interpretive language is newly authored MystaAI TRADITION content; no competitor/app copy.

**Repository targets**
- `knowledge/raw/zodiac/sepharial/`
- `knowledge/processed/zodiac/*.json`
- `knowledge/manifests/zodiac-sepharial.json`
- `tools/knowledge/fetch-astrology-reference.py`
- `tools/knowledge/build-zodiac.ts`

**Exact commands / acquisition**
```bash
pnpm tsx tools/knowledge/build-zodiac.ts
```

**Implementation**
- Create sign themes, strengths/challenges, relationships, work/money/family, decans and planet-in-sign records.
- Tag every interpretive claim as TRADITION, not empirical fact.

**Credentials / paid items at this phase**
- None.

**Verification commands**
```bash
pnpm test --filter knowledge-zodiac
```

**Phase complete only when**
- 12 signs complete; all cross-references resolve.


## Phase 32 — Astrology knowledge installation

**Outcome**  
Install astrology factual validation sources plus original interpretive knowledge.

**Locked inputs / researched sources**
- Calculation authority: Swiss Ephemeris commercial install.
- Independent factual validation: JPL Horizons API https://ssd.jpl.nasa.gov/api/horizons.api.
- Timezone authority: IANA 2026d.
- Historical interpretive reference: Sepharial, *Astrology: How to Make and Read Your Own Horoscope* (1920), Project Gutenberg eBook 46963. Production planet/sign/house/aspect/transit/synastry language is newly authored MystaAI TRADITION content, not copied text.

**Repository targets**
- `knowledge/raw/astrology/jpl/fixtures/`
- `knowledge/processed/astrology/*.json`
- `knowledge/manifests/astrology-*.json`
- `tools/knowledge/fetch-astrology-reference.py`
- `tools/knowledge/build-astrology.ts`
- `tools/astrology/validate-jpl.ts`

**Exact commands / acquisition**
```bash
pnpm tsx tools/knowledge/build-astrology.ts
pnpm tsx tools/astrology/validate-jpl.ts
```

**Implementation**
- Separate astronomical CALCULATION records from interpretive TRADITION records in schema/index.
- No AI-generated astronomical fact may override Swiss output.

**Credentials / paid items at this phase**
- Swiss licence from Phase 15; no JPL key required.

**Verification commands**
```bash
pnpm test --filter knowledge-astrology
```

**Phase complete only when**
- Every interpretation links to a known deterministic concept; evidence classes remain separated.


## Phase 33 — Dream knowledge installation

**Outcome**  
Install a rights-safe dream-reflection framework without scraping a modern dream dictionary.

**Locked inputs / researched sources**
- Dream knowledge is original MystaAI-authored reflective framework: possible personal association, emotional context, common metaphorical reading, reflection questions.
- No symbol is presented as objective diagnosis or universal prediction.

**Repository targets**
- `knowledge/processed/dream/frameworks.json`
- `knowledge/processed/dream/symbol-seeds.json`
- `knowledge/manifests/dream-original.json`
- `tools/knowledge/build-dream.ts`

**Exact commands / acquisition**
```bash
pnpm tsx tools/knowledge/build-dream.ts
```

**Implementation**
- Create generic symbol categories and promptable reflection structures; keep user dream text outside public knowledge corpus.
- Mark all symbolic content TRADITION/SYNTHESIS-compatible, never EVIDENCE unless separately psychology-sourced.

**Credentials / paid items at this phase**
- None.

**Verification commands**
```bash
pnpm test --filter knowledge-dream
```

**Phase complete only when**
- Safety fixtures reject diagnostic/medical certainty; schema complete.


## Phase 34 — Human psychology knowledge installation

**Outcome**  
Install the evidence-informed psychology layer from rights-approved sources with article-level provenance.

**Locked inputs / researched sources**
- NIMH text reuse policy: https://www.nimh.nih.gov/site-info/policies — text/publications public domain unless indicated; images excluded.
- PLOS API: https://api.plos.org/search ; PLOS TDM policy permits mining/reuse; ingest article-level CC BY-compatible content and attribution.
- PMC automated access: https://pmc.ncbi.nlm.nih.gov/tools/developers/ and OAI-PMH https://pmc.ncbi.nlm.nih.gov/tools/oai/ ; only articles whose individual licence permits commercial reuse.
- Crossref metadata: https://api.crossref.org/works ; use `mailto`, do not assume exposed abstracts are reusable.
- OpenStax Psychology is blocked unless explicit written commercial/AI permission is archived.

**Repository targets**
- `knowledge/psychology/raw/{nimh,plos,pmc,crossref}/`
- `knowledge/psychology/processed/`
- `knowledge/psychology/manifests/`
- `packages/psychology/src/ingest/*`
- `knowledge/psychology/manifests/query-policy.json`
- `apps/knowledge-ml/scripts/fetch_nimh.py`
- `apps/knowledge-ml/scripts/fetch_plos.py`
- `apps/knowledge-ml/scripts/fetch_pmc_oa.py`
- `apps/knowledge-ml/scripts/fetch_crossref_metadata.py`
- `manifests/PSYCHOLOGY_EVIDENCE_MANIFEST.json`

**Exact commands / acquisition**
```bash
mkdir -p knowledge/psychology/raw/{nimh,plos,pmc,crossref} knowledge/psychology/processed knowledge/psychology/manifests
python apps/knowledge-ml/scripts/fetch_nimh.py
python apps/knowledge-ml/scripts/fetch_plos.py
python apps/knowledge-ml/scripts/fetch_pmc_oa.py
python apps/knowledge-ml/scripts/fetch_crossref_metadata.py
```

**Implementation**
- `query-policy.json` fixes the candidate-query families: `emotion regulation`, `cognitive reappraisal`, `motivation goal pursuit`, `habit formation behavior change`, `decision making cognitive bias`, `interpersonal communication conflict`, `adult attachment relationship`, `social support`, `stress coping`, `grief bereavement`, `values meaning goals`, `uncertainty tolerance`, and `expressive/reflective writing`. Synonyms may expand recall only through the committed policy file; domains outside the approved list cannot enter production retrieval.
- Whitelisted domains: emotion/regulation, motivation, habits/behaviour change, decisions/biases, communication/conflict, relationship dynamics, attachment concepts, social/personality research, stress/coping, grief/transitions, goals/values, uncertainty, reflective questioning.
- Store evidence grade, study type, population/context, limitations, causal status, DOI/source IDs, licence, correction/retraction status.
- Create original summaries; never diagnose users or use vulnerability for monetisation.

**Credentials / paid items at this phase**
- `CROSSREF_MAILTO` required; The public PLOS Search API does not require an API key at the research lock; respect documented service limits. `NCBI_API_KEY` is optional but recommended if E-Utilities is used above unauthenticated rate limits. No OpenStax ingestion without written permission.

**Verification commands**
```bash
python -m pytest apps/knowledge-ml/tests/test_psychology_ingest.py
pnpm test --filter psychology-rights
```

**Phase complete only when**
- Every published claim has rights + scientific review fields; blocked/NC/unknown/retracted sources cannot publish.


## Phase 35 — Local knowledge ML service

**Outcome**  
Build the local Python knowledge/privacy ML service for embedding, reranking and PII support.

**Locked inputs / researched sources**
- Python 3.14.7; FastAPI 0.141.1; Uvicorn 0.54.0; PyTorch 2.14.0; Transformers 5.17.0; Sentence Transformers 6.1.0; Presidio Analyzer/Anonymizer 2.2.364.
- Embedding `BAAI/bge-m3` (MIT), pin exact HF revision/hash.
- Reranker `BAAI/bge-reranker-v2-m3` (Apache-2.0), pin exact HF revision/hash.

**Repository targets**
- `apps/knowledge-ml/pyproject.toml`
- `apps/knowledge-ml/src/*`
- `infra/docker/knowledge-ml.Dockerfile`

**Exact commands / acquisition**
```bash
python3.14 -m venv .venv-knowledge
. .venv-knowledge/bin/activate
pip install "fastapi==0.141.1" "uvicorn[standard]==0.54.0" "torch==2.14.0" "transformers==5.17.0" "sentence-transformers==6.1.0" "presidio-analyzer==2.2.364" "presidio-anonymizer==2.2.364"
pip freeze > apps/knowledge-ml/requirements.lock
```

**Implementation**
- Implement `/health`, internal `/embed`, `/rerank`, `/pii/analyse`, `/pii/anonymise`; private-network only.
- Download model snapshots at build time, hash all model artefacts; production does not float `main`.

**Credentials / paid items at this phase**
- No external inference key. HF access token only if needed for controlled snapshot download; do not persist in image.

**Verification commands**
```bash
python -m pytest apps/knowledge-ml/tests
```

**Phase complete only when**
- Embedding dimensions/stability fixtures pass; PII tests pass; no public endpoint.


## Phase 36 — pgvector and hybrid retrieval

**Outcome**  
Create hybrid pgvector + lexical retrieval with deterministic filtering/reranking.

**Locked inputs / researched sources**
- PostgreSQL/pgvector; Phase 35 embedding/reranker; V3 chunk sizes 150–700 target 450 overlap 80.

**Repository targets**
- `packages/knowledge/src/retrieval/*`
- database vector migrations
- `tests/fixtures/retrieval/*`

**Exact commands / acquisition**
```bash
pnpm --filter @mystaai/knowledge test -- --grep retrieval
```

**Implementation**
- Store source/domain/tradition/evidence-grade/population/limitations/version metadata with every vector.
- Query: filters → vector top30 + lexical → merge/dedupe → rerank → top8.
- Structured records are not arbitrarily split.

**Credentials / paid items at this phase**
- Internal knowledge service only.

**Verification commands**
```bash
pnpm test --filter retrieval-eval
```

**Phase complete only when**
- Golden retrieval corpus meets precision/recall thresholds; rights-inactive records never surface.


## Phase 37 — Knowledge graph

**Outcome**  
Build the typed knowledge graph and validated edges.

**Locked inputs / researched sources**
- Node/edge types in Section 50.

**Repository targets**
- `packages/knowledge/src/graph/*`
- database graph tables/migrations

**Exact commands / acquisition**
```bash
pnpm --filter @mystaai/knowledge test -- --grep graph
```

**Implementation**
- Persist graph nodes/edges in PostgreSQL, not a new graph database.
- Keep empirical psychology edges/evidence metadata separate from traditional correspondences.

**Credentials / paid items at this phase**
- None.

**Verification commands**
```bash
pnpm test --filter knowledge-graph
```

**Phase complete only when**
- No orphan node/edge; no tradition→evidence promotion.


## Phase 38 — Privacy-safety service

**Outcome**  
Build privacy/safety preprocessing service used before external AI/provider calls.

**Locked inputs / researched sources**
- Presidio service from Phase 35; V3 data-classification/retention rules; UK GDPR minimisation.

**Repository targets**
- `apps/privacy-safety`
- `packages/safety/src/privacy/*`
- `manifests/DATA_RETENTION_MANIFEST.json`

**Exact commands / acquisition**
```bash
pnpm --filter @mystaai/privacy-safety test
```

**Implementation**
- Classify/redact/minimise data before OpenAI; preserve deterministic IDs instead of raw unnecessary profile data.
- Define raw microphone audio transient-only rule.
- Add export/delete hooks.

**Credentials / paid items at this phase**
- No new external provider.

**Verification commands**
```bash
pnpm test --filter privacy-safety
```

**Phase complete only when**
- PII fixtures redact as designed; no prohibited data appears in outbound-request snapshots.


## Phase 39 — OpenAI Terra/Luna gateway

**Outcome**  
Implement the only OpenAI boundary with paid Terra and budgeted Luna routing.

**Locked inputs / researched sources**
- Official `openai@7.23.0` Apache-2.0.
- Responses API https://platform.openai.com/docs/api-reference/responses.
- Paid `gpt-5.6-terra`: researched $2/M input, $0.20/M cached input, $12/M output; 1.05M context, 128k output.
- Free `gpt-5.6-luna`: researched $0.20/M input, $0.02/M cached input, $1.20/M output.
- Structured Outputs/function calling supported; `store:false` unless explicitly approved.

**Repository targets**
- `packages/ai-gateway`
- `manifests/MODEL_MANIFEST.json`
- `tests/ai-evals/gateway/*`

**Exact commands / acquisition**
```bash
pnpm --filter @mystaai/ai-gateway add openai@7.23.0 zod@4.6.5
```

**Implementation**
- Only this package imports `openai`.
- Implement provider timeout/retry/backoff, request IDs, usage capture, model alias routing, structured output validation and hard Free budget admission.
- Never send raw unnecessary PII; deterministic tools remain authority.

**Credentials / paid items at this phase**
- Create OpenAI project/key when integration is reached: `OPENAI_API_KEY`, `OPENAI_PROJECT_ID`; record actual project limits then, not at build start.

**Verification commands**
```bash
pnpm --filter @mystaai/ai-gateway test
pnpm test --filter ai-gateway-contract
```

**Phase complete only when**
- Mocked contract tests pass; one controlled real dev request per model passes Structured Output/tool contract; costs/usage recorded.


## Phase 40 — Tool registry

**Outcome**  
Expose deterministic tools through a strict internal typed registry.

**Locked inputs / researched sources**
- Astrology, tarot, numerology, zodiac, retrieval, profile/history tools.

**Repository targets**
- `packages/reading-engine/src/tools/*`

**Exact commands / acquisition**
```bash
# no new external dependency
```

**Implementation**
- Define tool name/version/input/output schema/timeout/permission/evidence output.
- LLM may request tools but cannot invent tool success or overwrite deterministic results.

**Credentials / paid items at this phase**
- None.

**Verification commands**
```bash
pnpm test --filter tool-registry
```

**Phase complete only when**
- Every tool has schema, timeout, error contract and evidence ID.


## Phase 41 — Reading orchestrator

**Outcome**  
Build the durable reading state machine/orchestrator.

**Locked inputs / researched sources**
- Reading state machine REQUESTED→CALCULATING→RETRIEVING_KNOWLEDGE→PRIVACY_CHECK→GENERATING→VALIDATING→COMPLETE; SQS for durable async long jobs.

**Repository targets**
- `packages/reading-engine/src/orchestrator/*`
- `apps/worker/src/jobs/readings/*`

**Exact commands / acquisition**
```bash
# uses existing packages
```

**Implementation**
- Implement idempotency, retries, terminal failure states, transaction boundaries and evidence persistence.
- Long reports go to worker/SQS; ordinary interactive readings may complete inline under timeout.

**Credentials / paid items at this phase**
- AWS SQS dev credentials only if using real dev queue; local adapter otherwise.

**Verification commands**
```bash
pnpm test --filter reading-orchestrator
```

**Phase complete only when**
- Failure cannot be rendered COMPLETE; retry cannot duplicate charges/readings.


## Phase 42 — Truth / Evidence / Tradition validator

**Outcome**  
Implement claim-class truth validation and evidence attachment.

**Locked inputs / researched sources**
- Authority order FACT > CALCULATION > EVIDENCE > TRADITION > SYNTHESIS.

**Repository targets**
- `packages/validation/src/truth/*`

**Exact commands / acquisition**
```bash
# no new dependency
```

**Implementation**
- Validate output sentence/section metadata against deterministic evidence and approved knowledge IDs.
- UNKNOWN remains UNKNOWN.
- Block psychology claims that exceed evidence/population/causality limits.

**Credentials / paid items at this phase**
- None.

**Verification commands**
```bash
pnpm test --filter truth-validator
```

**Phase complete only when**
- Contradiction/adversarial fixtures fail; valid mixed-class responses pass.


## Phase 43 — Safety engine

**Outcome**  
Implement high-stakes safety classifier/policy layer.

**Locked inputs / researched sources**
- High-stakes categories in V3; 18+; divination is reflection/entertainment, no supernatural guarantee.

**Repository targets**
- `packages/safety/src/policy/*`
- `tests/ai-evals/safety/*`

**Exact commands / acquisition**
```bash
# no new provider required; use deterministic rules + validated AI classification through ai-gateway where configured
```

**Implementation**
- Prioritise self-harm/crisis, medical/pregnancy/death, legal/investment/gambling, abuse/crime/missing persons, psych diagnosis/trauma/personality disorders.
- Safety outranks persona and monetisation.

**Credentials / paid items at this phase**
- None.

**Verification commands**
```bash
pnpm test --filter safety-evals
```

**Phase complete only when**
- Unsafe certainty/diagnosis/manipulative prompts meet rejection/redirect thresholds.


## Phase 44 — Tarot AI readings

**Outcome**  
Implement evidence-backed tarot readings.

**Locked inputs / researched sources**
- Stored cryptographic tarot draw.
- Tarot knowledge from Phase 29.
- OpenAI gateway + truth/safety validator.

**Repository targets**
- `packages/tarot/src/interpretation/*`
- `tests/ai-evals/tarot/*`

**Exact commands / acquisition**
```bash
# no new external dependency
```

**Implementation**
- Construct tool/evidence payload first; retrieve approved domain knowledge; call Terra; validate; persist evidence and model/prompt/knowledge versions.
- AI cannot alter deterministic inputs.

**Credentials / paid items at this phase**
- OpenAI dev key from Phase 39.

**Verification commands**
```bash
pnpm test --filter tarot-ai
```

**Phase complete only when**
- Golden/eval suite passes; deterministic contradictions are blocked.


## Phase 45 — Numerology AI readings

**Outcome**  
Implement evidence-backed numerology readings.

**Locked inputs / researched sources**
- Deterministic numerology result.
- Numerology knowledge from Phase 30.
- OpenAI gateway + truth/safety validator.

**Repository targets**
- `packages/numerology/src/interpretation/*`
- `tests/ai-evals/numerology/*`

**Exact commands / acquisition**
```bash
# no new external dependency
```

**Implementation**
- Construct tool/evidence payload first; retrieve approved domain knowledge; call Terra; validate; persist evidence and model/prompt/knowledge versions.
- AI cannot alter deterministic inputs.

**Credentials / paid items at this phase**
- OpenAI dev key from Phase 39.

**Verification commands**
```bash
pnpm test --filter numerology-ai
```

**Phase complete only when**
- Golden/eval suite passes; deterministic contradictions are blocked.


## Phase 46 — Zodiac daily/weekly/monthly

**Outcome**  
Implement evidence-backed zodiac daily/weekly/monthly forecasts.

**Locked inputs / researched sources**
- Actual solar sign + transits/personal cycles where available.
- Zodiac/astrology knowledge from Phases 31–32.
- OpenAI gateway + truth/safety validator.

**Repository targets**
- `packages/zodiac/src/interpretation/*`
- `tests/ai-evals/zodiac/*`

**Exact commands / acquisition**
```bash
# no new external dependency
```

**Implementation**
- Construct tool/evidence payload first; retrieve approved domain knowledge; call Terra; validate; persist evidence and model/prompt/knowledge versions.
- AI cannot alter deterministic inputs.

**Credentials / paid items at this phase**
- OpenAI dev key from Phase 39.

**Verification commands**
```bash
pnpm test --filter zodiac-ai
```

**Phase complete only when**
- Golden/eval suite passes; deterministic contradictions are blocked.


## Phase 47 — Natal-chart interpretation

**Outcome**  
Implement evidence-backed natal-chart interpretation.

**Locked inputs / researched sources**
- Canonical natal evidence.
- Astrology knowledge Phase 32.
- OpenAI gateway + truth/safety validator.

**Repository targets**
- `packages/astrology/src/interpretation/*`
- `tests/ai-evals/astrology/*`

**Exact commands / acquisition**
```bash
# no new external dependency
```

**Implementation**
- Construct tool/evidence payload first; retrieve approved domain knowledge; call Terra; validate; persist evidence and model/prompt/knowledge versions.
- AI cannot alter deterministic inputs.

**Credentials / paid items at this phase**
- OpenAI dev key from Phase 39.

**Verification commands**
```bash
pnpm test --filter astrology-ai
```

**Phase complete only when**
- Golden/eval suite passes; deterministic contradictions are blocked.


## Phase 48 — Transit interpretation

**Outcome**  
Implement evidence-backed transit interpretation.

**Locked inputs / researched sources**
- Transit/transit-to-natal evidence.
- Astrology knowledge Phase 32.
- OpenAI gateway + truth/safety validator.

**Repository targets**
- `packages/astrology/src/interpretation/*`
- `tests/ai-evals/astrology/*`

**Exact commands / acquisition**
```bash
# no new external dependency
```

**Implementation**
- Construct tool/evidence payload first; retrieve approved domain knowledge; call Terra; validate; persist evidence and model/prompt/knowledge versions.
- AI cannot alter deterministic inputs.

**Credentials / paid items at this phase**
- OpenAI dev key from Phase 39.

**Verification commands**
```bash
pnpm test --filter astrology-ai
```

**Phase complete only when**
- Golden/eval suite passes; deterministic contradictions are blocked.


## Phase 49 — Cross-system synthesis

**Outcome**  
Implement evidence-backed cross-system synthesis.

**Locked inputs / researched sources**
- Deterministic evidence from selected systems.
- Domain knowledge plus psychology evidence kept in separate namespaces.
- OpenAI gateway + truth/safety validator.

**Repository targets**
- `packages/compatibility/src/interpretation/*`
- `tests/ai-evals/compatibility/*`

**Exact commands / acquisition**
```bash
# no new external dependency
```

**Implementation**
- Construct tool/evidence payload first; retrieve approved domain knowledge; call Terra; validate; persist evidence and model/prompt/knowledge versions.
- AI cannot alter deterministic inputs.

**Credentials / paid items at this phase**
- OpenAI dev key from Phase 39.

**Verification commands**
```bash
pnpm test --filter compatibility-ai
```

**Phase complete only when**
- Golden/eval suite passes; deterministic contradictions are blocked.


## Phase 50 — Compatibility

**Outcome**  
Implement evidence-backed compatibility/synastry interpretation.

**Locked inputs / researched sources**
- Synastry + numerology + optional tarot evidence.
- No arbitrary numeric compatibility score.
- OpenAI gateway + truth/safety validator.

**Repository targets**
- `packages/compatibility/src/interpretation/*`
- `tests/ai-evals/compatibility/*`

**Exact commands / acquisition**
```bash
# no new external dependency
```

**Implementation**
- Construct tool/evidence payload first; retrieve approved domain knowledge; call Terra; validate; persist evidence and model/prompt/knowledge versions.
- AI cannot alter deterministic inputs.

**Credentials / paid items at this phase**
- OpenAI dev key from Phase 39.

**Verification commands**
```bash
pnpm test --filter compatibility-ai
```

**Phase complete only when**
- Golden/eval suite passes; deterministic contradictions are blocked.


## Phase 51 — Dream journal and interpretation

**Outcome**  
Build encrypted dream journal, extraction, retrieval and safe interpretation.

**Locked inputs / researched sources**
- Dream framework Phase 33; psychology evidence may support general reflection but not diagnosis.

**Repository targets**
- `packages/dream`
- `apps/api/src/routes/dreams/*`

**Exact commands / acquisition**
```bash
# no new external dependency
```

**Implementation**
- Encrypt raw dream text; derive sanitised text/emotions/symbols/themes; retrieve prior recurring patterns; Terra interpretation through safety/truth.
- Pattern counts are deterministic.

**Credentials / paid items at this phase**
- OpenAI key already configured.

**Verification commands**
```bash
pnpm test --filter dream
```

**Phase complete only when**
- Replay/history works; medical/diagnostic certainty blocked.


## Phase 52 — Mysta intelligence and conversation orchestration

**Outcome**  
Build the single subscriber-only Mysta intelligence/session system.

**Locked inputs / researched sources**
- Entitlement `mysta_access`; Terra; deterministic tools; history/memory/evidence; no separate chatbot product.

**Repository targets**
- `packages/voice-orchestrator`
- `apps/api/src/routes/mysta/*`
- database Mysta session/turn tables

**Exact commands / acquisition**
```bash
# no new external dependency
```

**Implementation**
- Create one persistent session ID; accept TEXT or VOICE input event types; same intelligence regardless presentation.
- Build bounded memory retrieval and summarisation; never hidden psychological vulnerability profile.
- Store validated final turns, not chain-of-thought.

**Credentials / paid items at this phase**
- OpenAI key.

**Verification commands**
```bash
pnpm test --filter mysta-session
```

**Phase complete only when**
- Free cannot create session; Premium/Ultra can; same session survives presentation changes.


## Phase 53 — Reading timeline and pattern engine

**Outcome**  
Build deterministic reading timeline and recurring-pattern engine.

**Locked inputs / researched sources**
- Stored evidence/versioned readings only.

**Repository targets**
- `packages/reading-engine/src/history/*`

**Exact commands / acquisition**
```bash
# no new dependency
```

**Implementation**
- Timeline, recurring tarot cards/themes, dream symbols/emotions, cycle markers, transit patterns; label statistical recurrence without supernatural certainty.

**Credentials / paid items at this phase**
- None.

**Verification commands**
```bash
pnpm test --filter patterns
```

**Phase complete only when**
- Counts/windows deterministic and reproducible.


## Phase 54 — Today/Home experience

**Outcome**  
Build the production Today/Home experience on web first with shared components ready for mobile parity.

**Locked inputs / researched sources**
- `packages/ui` brand/accessibility system.
- Daily tarot/personal-day/transit/profile/history inputs.

**Repository targets**
- `apps/web/app/(app)/today/page.tsx`
- shared Today components

**Exact commands / acquisition**
```bash
# use existing Next/React/UI dependencies
```

**Implementation**
- Implement loading/empty/error/success, evidence expansion, responsive layout, keyboard/focus, reduced motion and analytics events.
- No copied competitor layout/art.

**Credentials / paid items at this phase**
- Authenticated dev account where route requires it.

**Verification commands**
```bash
pnpm playwright test tests/acceptance --grep "Today/Home"
```

**Phase complete only when**
- Functional + visual + accessibility tests pass.


## Phase 55 — Readings hub

**Outcome**  
Build the production Readings hub experience on web first with shared components ready for mobile parity.

**Locked inputs / researched sources**
- `packages/ui` brand/accessibility system.
- Available reading products/entitlements/history.

**Repository targets**
- `apps/web/app/(app)/readings/page.tsx`

**Exact commands / acquisition**
```bash
# use existing Next/React/UI dependencies
```

**Implementation**
- Implement loading/empty/error/success, evidence expansion, responsive layout, keyboard/focus, reduced motion and analytics events.
- No copied competitor layout/art.

**Credentials / paid items at this phase**
- Authenticated dev account where route requires it.

**Verification commands**
```bash
pnpm playwright test tests/acceptance --grep "Readings"
```

**Phase complete only when**
- Functional + visual + accessibility tests pass.


## Phase 56 — Tarot UI

**Outcome**  
Build the production Tarot experience on web first with shared components ready for mobile parity.

**Locked inputs / researched sources**
- `packages/ui` brand/accessibility system.
- Tarot domain/draw/AI endpoints.

**Repository targets**
- `apps/web/app/(app)/readings/tarot/*`

**Exact commands / acquisition**
```bash
# use existing Next/React/UI dependencies
```

**Implementation**
- Implement loading/empty/error/success, evidence expansion, responsive layout, keyboard/focus, reduced motion and analytics events.
- No copied competitor layout/art.

**Credentials / paid items at this phase**
- Authenticated dev account where route requires it.

**Verification commands**
```bash
pnpm playwright test tests/acceptance --grep "Tarot"
```

**Phase complete only when**
- Functional + visual + accessibility tests pass.


## Phase 57 — Numerology UI

**Outcome**  
Build the production Numerology experience on web first with shared components ready for mobile parity.

**Locked inputs / researched sources**
- `packages/ui` brand/accessibility system.
- Numerology result/AI endpoints.

**Repository targets**
- `apps/web/app/(app)/readings/numerology/*`

**Exact commands / acquisition**
```bash
# use existing Next/React/UI dependencies
```

**Implementation**
- Implement loading/empty/error/success, evidence expansion, responsive layout, keyboard/focus, reduced motion and analytics events.
- No copied competitor layout/art.

**Credentials / paid items at this phase**
- Authenticated dev account where route requires it.

**Verification commands**
```bash
pnpm playwright test tests/acceptance --grep "Numerology"
```

**Phase complete only when**
- Functional + visual + accessibility tests pass.


## Phase 58 — Zodiac UI

**Outcome**  
Build the production Zodiac experience on web first with shared components ready for mobile parity.

**Locked inputs / researched sources**
- `packages/ui` brand/accessibility system.
- Zodiac daily/weekly/monthly endpoints.

**Repository targets**
- `apps/web/app/(app)/readings/zodiac/*`

**Exact commands / acquisition**
```bash
# use existing Next/React/UI dependencies
```

**Implementation**
- Implement loading/empty/error/success, evidence expansion, responsive layout, keyboard/focus, reduced motion and analytics events.
- No copied competitor layout/art.

**Credentials / paid items at this phase**
- Authenticated dev account where route requires it.

**Verification commands**
```bash
pnpm playwright test tests/acceptance --grep "Zodiac"
```

**Phase complete only when**
- Functional + visual + accessibility tests pass.


## Phase 59 — Astrology UI and chart renderer

**Outcome**  
Build the production Astrology chart experience on web first with shared components ready for mobile parity.

**Locked inputs / researched sources**
- `packages/ui` brand/accessibility system.
- Natal deterministic chart + interpretation.

**Repository targets**
- `apps/web/app/(app)/readings/astrology/*`
- `packages/ui/src/chart/*`

**Exact commands / acquisition**
```bash
# use existing Next/React/UI dependencies
```

**Implementation**
- Implement loading/empty/error/success, evidence expansion, responsive layout, keyboard/focus, reduced motion and analytics events.
- No copied competitor layout/art.

**Credentials / paid items at this phase**
- Authenticated dev account where route requires it.

**Verification commands**
```bash
pnpm playwright test tests/acceptance --grep "Astrology"
```

**Phase complete only when**
- Functional + visual + accessibility tests pass.


## Phase 60 — Transit UI

**Outcome**  
Build the production Transit experience on web first with shared components ready for mobile parity.

**Locked inputs / researched sources**
- `packages/ui` brand/accessibility system.
- Transit evidence/interpretation.

**Repository targets**
- `apps/web/app/(app)/readings/transits/*`

**Exact commands / acquisition**
```bash
# use existing Next/React/UI dependencies
```

**Implementation**
- Implement loading/empty/error/success, evidence expansion, responsive layout, keyboard/focus, reduced motion and analytics events.
- No copied competitor layout/art.

**Credentials / paid items at this phase**
- Authenticated dev account where route requires it.

**Verification commands**
```bash
pnpm playwright test tests/acceptance --grep "Transit"
```

**Phase complete only when**
- Functional + visual + accessibility tests pass.


## Phase 61 — Compatibility UI

**Outcome**  
Build the production Compatibility experience on web first with shared components ready for mobile parity.

**Locked inputs / researched sources**
- `packages/ui` brand/accessibility system.
- Compatibility/synastry evidence.

**Repository targets**
- `apps/web/app/(app)/readings/compatibility/*`

**Exact commands / acquisition**
```bash
# use existing Next/React/UI dependencies
```

**Implementation**
- Implement loading/empty/error/success, evidence expansion, responsive layout, keyboard/focus, reduced motion and analytics events.
- No copied competitor layout/art.

**Credentials / paid items at this phase**
- Authenticated dev account where route requires it.

**Verification commands**
```bash
pnpm playwright test tests/acceptance --grep "Compatibility"
```

**Phase complete only when**
- Functional + visual + accessibility tests pass.


## Phase 62 — Dream UI

**Outcome**  
Build the production Dream experience on web first with shared components ready for mobile parity.

**Locked inputs / researched sources**
- `packages/ui` brand/accessibility system.
- Encrypted dream journal + interpretation.

**Repository targets**
- `apps/web/app/(app)/dreams/*`

**Exact commands / acquisition**
```bash
# use existing Next/React/UI dependencies
```

**Implementation**
- Implement loading/empty/error/success, evidence expansion, responsive layout, keyboard/focus, reduced motion and analytics events.
- No copied competitor layout/art.

**Credentials / paid items at this phase**
- Authenticated dev account where route requires it.

**Verification commands**
```bash
pnpm playwright test tests/acceptance --grep "Dream"
```

**Phase complete only when**
- Functional + visual + accessibility tests pass.


## Phase 63 — Mysta embodied interface, subscriber gate, input and continuity states

**Outcome**  
Build the one Mysta embodied subscriber surface with avatar-first layout and first-class audio/text continuity states.

**Locked inputs / researched sources**
- Route is `/mysta`, entitlement `mysta_access`.
- Presentations: AVATAR_ACTIVE default, AUDIO_ONLY_FALLBACK, TEXT_ONLY_FALLBACK; voice/text are input methods.

**Repository targets**
- `apps/web/app/(app)/mysta/*`
- `packages/ui/src/mysta/*`
- `manifests/UI_ROUTE_MANIFEST.json`

**Exact commands / acquisition**
```bash
# no avatar provider dependency yet; UI works with local presentation-state fixtures
```

**Implementation**
- No standalone chat screen.
- Audio/text fallback surfaces retain full tools, reading cards, evidence, history, captions/transcript, quick actions and session identity.
- Accessibility preference is not described as an error/downgrade; technical failure messaging is factual.

**Credentials / paid items at this phase**
- Subscriber fixture account for acceptance tests.

**Verification commands**
```bash
pnpm playwright test tests/acceptance/mysta-shell.spec.ts
```

**Phase complete only when**
- All state transitions preserve session and feature parity; Free route/API guard tested.


## Phase 64 — Cross-System UI

**Outcome**  
Build the production Cross-System experience on web first with shared components ready for mobile parity.

**Locked inputs / researched sources**
- `packages/ui` brand/accessibility system.
- Cross-system synthesis endpoint with separated evidence classes.

**Repository targets**
- `apps/web/app/(app)/readings/cross-system/*`

**Exact commands / acquisition**
```bash
# use existing Next/React/UI dependencies
```

**Implementation**
- Implement loading/empty/error/success, evidence expansion, responsive layout, keyboard/focus, reduced motion and analytics events.
- No copied competitor layout/art.

**Credentials / paid items at this phase**
- Authenticated dev account where route requires it.

**Verification commands**
```bash
pnpm playwright test tests/acceptance --grep "Cross-System"
```

**Phase complete only when**
- Functional + visual + accessibility tests pass.


## Phase 65 — History/Patterns UI

**Outcome**  
Build the production History/Patterns experience on web first with shared components ready for mobile parity.

**Locked inputs / researched sources**
- `packages/ui` brand/accessibility system.
- Timeline/pattern engine.

**Repository targets**
- `apps/web/app/(app)/history/*`

**Exact commands / acquisition**
```bash
# use existing Next/React/UI dependencies
```

**Implementation**
- Implement loading/empty/error/success, evidence expansion, responsive layout, keyboard/focus, reduced motion and analytics events.
- No copied competitor layout/art.

**Credentials / paid items at this phase**
- Authenticated dev account where route requires it.

**Verification commands**
```bash
pnpm playwright test tests/acceptance --grep "History/Patterns"
```

**Phase complete only when**
- Functional + visual + accessibility tests pass.


## Phase 66 — Monetisation, Free allowance and immutable wallets

**Outcome**  
Implement pricing catalogue, Free admission control, subscription/READ/AVA ledgers and immutable accounting model.

**Locked inputs / researched sources**
- Free $0; Premium $7.99/month + 1500 AVA_SEC; Ultra $21.99/month + 4140 AVA_SEC; no annual/trial at launch.
- READ packs: 20 £2.99, 60 £6.99, 150 £14.99.
- AVA topups active subscribers: £10/1800 sec; £20/4500 sec. Purchased READ/AVA do not expire.
- Free: 2 accepted generations/day, 2500 input tokens, 250 output tokens, $0.001 direct provider cap/day, owner ceiling $0.005, reset 00:00 UTC.

**Repository targets**
- `packages/monetization`
- `packages/usage-metering`
- `packages/billing`
- database ledger migrations
- `manifests/MONETIZATION_MANIFEST.json`
- `manifests/FREE_ALLOWANCE_MANIFEST.json`

**Exact commands / acquisition**
```bash
# no payment SDK yet
```

**Implementation**
- Use append-only double-entry-style event ledger/idempotency keys; balances derived/atomically updated server-side.
- Included AVA expires at cycle boundary and spends before purchased AVA.
- READ never unlocks Mysta.

**Credentials / paid items at this phase**
- None.

**Verification commands**
```bash
pnpm test --filter monetization -- --runInBand
```

**Phase complete only when**
- Concurrency/overspend tests pass; Free budget cannot race past cap; no annual product exists.


## Phase 67 — Stripe web subscriptions, READ, AVA and reports

**Outcome**  
Integrate Stripe for web purchases/subscriptions.

**Locked inputs / researched sources**
- Stripe official Node SDK: pin current stable exact release when installing this phase; API version recorded separately.
- UK researched card baseline 1.5% + 20p for standard UK cards; EU card baseline 2.5% + 20p; Stripe Billing may add 0.7% billing volume. Current values are rechecked here and again Phase 102.

**Repository targets**
- `packages/billing/src/stripe/*`
- `apps/api/src/webhooks/stripe.ts`
- `store/stripe/`

**Exact commands / acquisition**
```bash
STRIPE_VERSION=$(npm view stripe version)
pnpm --filter @mystaai/billing add "stripe@$STRIPE_VERSION"
echo "$STRIPE_VERSION"
```

**Implementation**
- Create monthly Premium/Ultra prices, READ/AVA consumables and reports in Stripe test mode; product IDs recorded in store manifest.
- Verify webhook signatures; idempotently convert paid events to internal entitlement/ledger events.
- Never trust client success redirect.

**Credentials / paid items at this phase**
- `STRIPE_SECRET_KEY`, `STRIPE_WEBHOOK_SECRET`, publishable key for web client.

**Verification commands**
```bash
pnpm test --filter stripe
stripe trigger checkout.session.completed
```

**Phase complete only when**
- Test purchase/renew/cancel/refund/webhook replay all reconcile exactly once.


## Phase 68 — RevenueCat, Apple and Google subscriptions/consumables

**Outcome**  
Integrate Apple/Google in-app purchases through RevenueCat for native digital goods.

**Locked inputs / researched sources**
- `react-native-purchases@10.10.2`, `react-native-purchases-ui@10.10.2`.
- Apple App Review 3.1.1: digital subscriptions/features/currencies use IAP.
- Google Play payments policy: digital goods/virtual currency use Play Billing unless a permitted programme exception applies.
- RevenueCat researched Pro baseline: free to $2,500 monthly tracked revenue then 1%; capture actual terms here.

**Repository targets**
- `packages/billing/src/revenuecat/*`
- `apps/api/src/webhooks/revenuecat.ts`
- `store/apple/*`
- `store/google/*`
- `store/revenuecat/*`

**Exact commands / acquisition**
```bash
cd apps/mobile
npx expo install react-native-purchases@10.10.2 react-native-purchases-ui@10.10.2
```

**Implementation**
- Create matching monthly subscriptions, READ/AVA/report consumables using current store product identifiers; map RevenueCat entitlements to server validation.
- RevenueCat transports purchase/entitlement events; internal DB is high-throughput spend authority.
- Webhook signature/auth + idempotency.

**Credentials / paid items at this phase**
- Apple Developer/App Store Connect, Google Play Console and RevenueCat accounts when this phase is reached; sandbox/service credentials only then.

**Verification commands**
```bash
pnpm test --filter revenuecat
npx expo-doctor
```

**Phase complete only when**
- Sandbox purchase/restore/refund/revocation passes on real dev builds; current store rules documented.


## Phase 69 — Entitlement, Free allowance, READ and AVA enforcement

**Outcome**  
Enforce entitlements and every Free/READ/AVA spend at server boundaries.

**Locked inputs / researched sources**
- Server entitlement is authoritative.

**Repository targets**
- `packages/monetization/src/enforcement/*`
- API guards
- WebSocket/Mysta admission guards

**Exact commands / acquisition**
```bash
# no new dependency
```

**Implementation**
- Guard route/API/WebSocket/tool spend; atomic Free counter/cost reservation; atomic READ/AVA debit and rollback on failed operation.
- Free cannot create/enter Mysta by any route, token or credit manipulation.

**Credentials / paid items at this phase**
- None.

**Verification commands**
```bash
pnpm test --filter entitlement-security
```

**Phase complete only when**
- Race/adversarial tests cannot overspend or bypass plan.


## Phase 70 — Free exhaustion, paywall, Reading Credits and avatar top-up UI

**Outcome**  
Build the production Monetisation/paywall experience on web first with shared components ready for mobile parity.

**Locked inputs / researched sources**
- `packages/ui` brand/accessibility system.
- Locked plans, Free exhaustion, READ packs, AVA topups, report products.

**Repository targets**
- `apps/web/app/(app)/upgrade/*`
- `apps/mobile/app/upgrade/*`
- `packages/ui/src/paywall/*`

**Exact commands / acquisition**
```bash
# use existing Next/React/UI dependencies
```

**Implementation**
- Implement loading/empty/error/success, evidence expansion, responsive layout, keyboard/focus, reduced motion and analytics events.
- No copied competitor layout/art.

**Credentials / paid items at this phase**
- Authenticated dev account where route requires it.

**Verification commands**
```bash
pnpm playwright test tests/acceptance --grep "Monetisation/paywall"
```

**Phase complete only when**
- Functional + visual + accessibility tests pass.


## Phase 71 — Report engine

**Outcome**  
Generate purchased/subscriber premium PDF reports asynchronously with evidence/provenance.

**Locked inputs / researched sources**
- `@react-pdf/renderer@4.9.0`; S3 signed URLs; Terra report sections validated independently.

**Repository targets**
- `packages/pdf` if present or `packages/reading-engine/src/reports/*`
- `apps/worker/src/jobs/reports/*`
- S3 report storage

**Exact commands / acquisition**
```bash
pnpm --filter @mystaai/reading-engine add @react-pdf/renderer@4.9.0
```

**Implementation**
- Queue report job; calculate deterministic evidence; generate/validate sections; render PDF; encrypt/private S3 object; store report version/hash.

**Credentials / paid items at this phase**
- AWS dev S3 credentials if testing real S3; local object-store adapter otherwise.

**Verification commands**
```bash
pnpm test --filter reports
```

**Phase complete only when**
- Failed section prevents COMPLETE report; generated report contains required evidence/disclaimer/provenance.


## Phase 72 — Report UI

**Outcome**  
Build the production Reports experience on web first with shared components ready for mobile parity.

**Locked inputs / researched sources**
- `packages/ui` brand/accessibility system.
- Report catalogue/purchase/status/download.

**Repository targets**
- `apps/web/app/(app)/reports/*`
- `apps/mobile/app/reports/*`

**Exact commands / acquisition**
```bash
# use existing Next/React/UI dependencies
```

**Implementation**
- Implement loading/empty/error/success, evidence expansion, responsive layout, keyboard/focus, reduced motion and analytics events.
- No copied competitor layout/art.

**Credentials / paid items at this phase**
- Authenticated dev account where route requires it.

**Verification commands**
```bash
pnpm playwright test tests/acceptance --grep "Reports"
```

**Phase complete only when**
- Functional + visual + accessibility tests pass.


## Phase 73 — Notifications

**Outcome**  
Implement consented push notifications.

**Locked inputs / researched sources**
- Expo Notifications installed with Expo SDK via `npx expo install`; platform push credentials.

**Repository targets**
- `packages/notifications`
- `apps/worker/src/jobs/notifications/*`
- mobile notification handlers

**Exact commands / acquisition**
```bash
cd apps/mobile && npx expo install expo-notifications
```

**Implementation**
- Permission only in context; token registration/revocation; daily/reading/report events; timezone-aware schedules; no sensitive reading content on lock screen by default.

**Credentials / paid items at this phase**
- Expo/EAS push credentials and Apple/Google push credentials when configuring native builds.

**Verification commands**
```bash
pnpm test --filter notifications
```

**Phase complete only when**
- Opt-out works; invalid/revoked token cleanup; privacy-safe payloads.


## Phase 74 — Email

**Outcome**  
Implement transactional email.

**Locked inputs / researched sources**
- Amazon SES v2 via exact-pinned AWS SDK; templates for verify/reset/receipt/report-ready/security.

**Repository targets**
- `packages/email`
- `apps/worker/src/jobs/email/*`

**Exact commands / acquisition**
```bash
pnpm tsx tools/lock/install-resolved.mts --workspace @mystaai/email --package @aws-sdk/client-sesv2
```

**Implementation**
- Template IDs/versioning; unsubscribe only where legally applicable; no sensitive divination detail in subject lines.

**Credentials / paid items at this phase**
- SES verified domain/identity and production access before launch.

**Verification commands**
```bash
pnpm test --filter email
```

**Phase complete only when**
- Rendering/snapshot/bounce handling passes.


## Phase 75 — Profile/settings/privacy UI

**Outcome**  
Build the production Profile/settings/privacy experience on web first with shared components ready for mobile parity.

**Locked inputs / researched sources**
- `packages/ui` brand/accessibility system.
- Profile, birth data, consent, notification, reduced-motion, audio, privacy settings.

**Repository targets**
- `apps/web/app/(app)/profile/*`
- `apps/mobile/app/profile/*`

**Exact commands / acquisition**
```bash
# use existing Next/React/UI dependencies
```

**Implementation**
- Implement loading/empty/error/success, evidence expansion, responsive layout, keyboard/focus, reduced motion and analytics events.
- No copied competitor layout/art.

**Credentials / paid items at this phase**
- Authenticated dev account where route requires it.

**Verification commands**
```bash
pnpm playwright test tests/acceptance --grep "Profile/settings/privacy"
```

**Phase complete only when**
- Functional + visual + accessibility tests pass.


## Phase 76 — Data export/deletion

**Outcome**  
Implement user data export and account deletion end-to-end.

**Locked inputs / researched sources**
- UK GDPR data-subject rights; retention schedule.

**Repository targets**
- `packages/shared/src/privacy-jobs/*`
- `apps/worker/src/jobs/export-delete/*`
- API routes

**Exact commands / acquisition**
```bash
# no new dependency
```

**Implementation**
- Generate user export archive; delete/anonymise according to legal/financial retention rules; revoke sessions/tokens/push; delete raw private content and provider-side data where applicable.

**Credentials / paid items at this phase**
- Provider admin/API credentials already configured by integration phases.

**Verification commands**
```bash
pnpm test --filter data-rights
```

**Phase complete only when**
- Export complete; deletion is idempotent and leaves only documented legally retained records.


## Phase 77 — Admin console

**Outcome**  
Build role-protected admin/ops console.

**Locked inputs / researched sources**
- Admin MFA; immutable audit log.

**Repository targets**
- `apps/admin`
- `packages/auth/src/admin/*`

**Exact commands / acquisition**
```bash
# existing Next/React/auth
```

**Implementation**
- Dashboards: users/entitlements/ledger reconciliations, knowledge publish/retract, safety incidents, provider health, cost/quotas, support tools.
- No arbitrary edit of immutable ledger/evidence; corrective events only.

**Credentials / paid items at this phase**
- Admin accounts/MFA.

**Verification commands**
```bash
pnpm playwright test tests/acceptance/admin.spec.ts
```

**Phase complete only when**
- RBAC and audit tests pass; destructive actions require explicit confirmation/permissions.


## Phase 78 — Analytics

**Outcome**  
Implement privacy-controlled product analytics.

**Locked inputs / researched sources**
- PostHog SDK exact stable versions from dependency manifest; no raw sensitive reading/dream text.

**Repository targets**
- `packages/analytics` or `packages/observability/src/analytics/*`

**Exact commands / acquisition**
```bash
# install exact manifest-pinned PostHog SDKs
```

**Implementation**
- Event taxonomy uses IDs/categories, plan/funnel/performance—not mystical content.
- Consent/opt-out by jurisdiction/config.

**Credentials / paid items at this phase**
- PostHog project key/host when enabled.

**Verification commands**
```bash
pnpm test --filter analytics-privacy
```

**Phase complete only when**
- No sensitive payload fixture reaches analytics.


## Phase 79 — Observability

**Outcome**  
Implement logs, metrics, traces, errors and SLO dashboards.

**Locked inputs / researched sources**
- Sentry exact stable compatible SDKs pinned in manifest; AWS CloudWatch metrics/alarms; OpenTelemetry where used.

**Repository targets**
- `packages/observability`
- `infra/terraform/modules/observability`
- `docs/runbooks/*`

**Exact commands / acquisition**
```bash
# install manifest-pinned Sentry/OpenTelemetry packages
```

**Implementation**
- Trace reading IDs/provider request IDs without sensitive body.
- Dashboards for API, DB, queue, AI, Free spend, wallet, voice, avatar, provider failures.

**Credentials / paid items at this phase**
- Sentry DSN/CloudWatch IAM in environments.

**Verification commands**
```bash
pnpm test --filter observability
```

**Phase complete only when**
- Alert fixtures and redaction tests pass.


## Phase 80 — Full web integration

**Outcome**  
Integrate the complete web application against real local/staging APIs.

**Locked inputs / researched sources**
- Next.js 16.3.6; React/DOM 19.3.0; Tailwind 4.3.3.

**Repository targets**
- `apps/web`
- `tests/acceptance/web/*`

**Exact commands / acquisition**
```bash
pnpm --filter @mystaai/web build
pnpm playwright test tests/acceptance/web
```

**Implementation**
- Wire auth/onboarding/Today/readings/Mysta/payments/reports/history/settings/admin links; server/client boundaries and CSP.

**Credentials / paid items at this phase**
- Dev service credentials from prior phases.

**Verification commands**
```bash
pnpm --filter @mystaai/web build
pnpm playwright test tests/acceptance/web
```

**Phase complete only when**
- Supported-browser acceptance path passes without mocked business services.


## Phase 81 — Mobile foundation

**Outcome**  
Create the Expo SDK 57 native app and prove the complete native dependency set compiles.

**Locked inputs / researched sources**
- Expo 57.0.25; React Native 0.86.3; React 19.2.3; TypeScript selected through `npx expo install` compatibility check.
- LiveKit researched set: livekit-client 2.22.3; @livekit/react-native 3.0.0; expo plugin 1.0.3; RN WebRTC 144.2.0; config plugin 15.0.2.
- LiveKit official Expo quickstart requires `expo-dev-client`, config plugins and `registerGlobals()`; Expo Go is prohibited for final integration.

**Repository targets**
- `apps/mobile`
- `apps/mobile/app.config.ts`
- `apps/mobile/eas.json`
- `apps/mobile/metro.config.js`

**Exact commands / acquisition**
```bash
cd apps/mobile
npx expo install expo@57.0.25 react@19.2.3 react-native@0.86.3 expo-router expo-dev-client expo-notifications expo-secure-store expo-font expo-linking expo-constants expo-application expo-device expo-haptics expo-image expo-audio
npx expo install livekit-client@2.22.3 @livekit/react-native@3.0.0 @livekit/react-native-expo-plugin@1.0.3 @livekit/react-native-webrtc@144.2.0 @config-plugins/react-native-webrtc@15.0.2
npx expo-doctor
npx expo prebuild --clean
```

**Implementation**
- Configure bundle/package ID `com.richardcurley.mystaai`, permissions, LiveKit plugins and `registerGlobals()`.
- Build EAS development clients; if any native compatibility set fails compile, this phase is blocked—do not substitute randomly.

**Credentials / paid items at this phase**
- Expo account/EAS project and Apple/Google development signing when building device clients.

**Verification commands**
```bash
cd apps/mobile && npx expo-doctor
npx expo prebuild --clean
```

**Phase complete only when**
- Android dev build installs/boots; iOS EAS dev build compiles/boots; LiveKit globals initialize.


## Phase 82 — Full mobile integration

**Outcome**  
Port/wire all user experiences to mobile with parity.

**Locked inputs / researched sources**
- All completed API contracts and mobile foundation.

**Repository targets**
- `apps/mobile/app/*`
- `packages/voice-client`
- shared UI/domain clients

**Exact commands / acquisition**
```bash
# existing Expo dependencies
```

**Implementation**
- Implement Today/Readings/Mysta/Explore/Profile navigation, onboarding, readings, payments, reports, history/settings.
- Preserve mobile accessibility, safe areas, keyboard, offline/transient states.

**Credentials / paid items at this phase**
- Sandbox accounts from integrations.

**Verification commands**
```bash
pnpm --filter @mystaai/mobile test
cd apps/mobile && npx expo-doctor
```

**Phase complete only when**
- Feature parity acceptance passes on physical Android/iOS devices.


## Phase 83 — Mysta synthetic voice creation and asset freeze

**Outcome**  
Create and freeze the original synthetic `mysta_voice_v1` without cloning an identifiable person.

**Locked inputs / researched sources**
- Kokoro-82M v1.0 Apache-2.0; `kokoro==0.9.4`, `misaki==0.9.4`, Python 3.12.14.
- Target: feminine, warm, calm, wise, natural, subtly mystical, clear international/British-leaning, measured pace.

**Repository targets**
- `assets/voice/mysta_voice_v1.*`
- `manifests/VOICE_MANIFEST.json`
- `tools/voice/design/*`
- `tests/voice/corpus/*`

**Exact commands / acquisition**
```bash
python3.12 -m venv .venv-voice
. .venv-voice/bin/activate
pip install "kokoro==0.9.4" "misaki[en]==0.9.4" soundfile
```

**Implementation**
- Generate candidates only from licensed Kokoro voice embedding space/prosody controls; no human-reference fitting.
- Evaluate fixed corpus; freeze chosen voice asset SHA/provenance.

**Credentials / paid items at this phase**
- No external voice API; no mother/person voice recording used.

**Verification commands**
```bash
python tools/voice/evaluate_voice.py --voice assets/voice/mysta_voice_v1.*
```

**Phase complete only when**
- Voice identity/provenance/pronunciation/safety corpus passes and hash is immutable.


## Phase 84 — Self-hosted speech recognition service

**Outcome**  
Build self-hosted Whisper large-v3-turbo ASR.

**Locked inputs / researched sources**
- Python 3.12.14; faster-whisper 1.2.1; CTranslate2 4.8.2; source model `openai/whisper-large-v3-turbo` MIT pinned to V3 source revision.

**Repository targets**
- `apps/voice-inference/asr/*`
- `infra/docker/voice-asr.Dockerfile`
- `assets/voice/models/whisper-large-v3-turbo/`

**Exact commands / acquisition**
```bash
python3.12 -m venv .venv-voice
. .venv-voice/bin/activate
pip install "faster-whisper==1.2.1" "ctranslate2==4.8.2"
```

**Implementation**
- Controlled build downloads/converts the pinned model, hashes source+converted files, then bakes/attaches immutable artefact.
- 16k mono decode/resample, VAD/endpointing, partial vs final transcript contract.
- Raw mic audio never retained by default.

**Credentials / paid items at this phase**
- HF token only if snapshot tooling requires it; not runtime.

**Verification commands**
```bash
python -m pytest apps/voice-inference/tests/asr
```

**Phase complete only when**
- WER/latency functional corpus passes locally; production GPU scale proved Phase 88.


## Phase 85 — Self-hosted Mysta speech synthesis service

**Outcome**  
Build self-hosted Kokoro `mysta_voice_v1` TTS.

**Locked inputs / researched sources**
- Python 3.12.14; kokoro 0.9.4; misaki 0.9.4; espeak-ng fallback; frozen voice asset.

**Repository targets**
- `apps/voice-inference/tts/*`
- `infra/docker/voice-tts.Dockerfile`

**Exact commands / acquisition**
```bash
python3.12 -m venv .venv-voice
. .venv-voice/bin/activate
pip install "kokoro==0.9.4" "misaki[en]==0.9.4" soundfile
# Linux container
apt-get update && apt-get install -y --no-install-recommends espeak-ng
```

**Implementation**
- Stream 24k mono PCM internally; validated text only; chunk for Opus transport; cache only safe reusable system phrases if policy permits.

**Credentials / paid items at this phase**
- None.

**Verification commands**
```bash
python -m pytest apps/voice-inference/tests/tts
```

**Phase complete only when**
- Exact voice hash loads; pronunciation/latency corpus passes; no external TTS call.


## Phase 86 — Voice gateway and realtime protocol

**Outcome**  
Build authenticated realtime voice session protocol and orchestration.

**Locked inputs / researched sources**
- Fastify 5.12.5 + `@fastify/websocket@11.3.1`; Opus; ASR/TTS services.

**Repository targets**
- `apps/voice-gateway`
- `packages/voice-contracts`
- `packages/voice-orchestrator`
- `packages/voice-client`

**Exact commands / acquisition**
```bash
pnpm --filter @mystaai/voice-gateway add @fastify/websocket@11.3.1
pnpm --filter @mystaai/voice-gateway add -D @types/ws
```

**Implementation**
- Protocol `mysta-voice-v1`: authenticate, session bind, audio frames, VAD/final transcript, turn start/end, interrupt/cancel, TTS chunks, errors/reconnect.
- Backpressure/size/rate limits; no unauthenticated mic stream.

**Credentials / paid items at this phase**
- Existing auth/session and internal service credentials.

**Verification commands**
```bash
pnpm test --filter voice-protocol
```

**Phase complete only when**
- Barge-in cancels synthesis quickly; reconnect does not duplicate turns.


## Phase 87 — Voice UX integration — web/iOS/Android

**Outcome**  
Wire microphone/playback/captions/barge-in on web/iOS/Android.

**Locked inputs / researched sources**
- Web MediaDevices/Web Audio; Expo `expo-audio`; voice protocol.

**Repository targets**
- `packages/voice-client`
- web Mysta client
- mobile Mysta client

**Exact commands / acquisition**
```bash
# no new dependency beyond Expo audio already installed
```

**Implementation**
- Explicit microphone permission; headset/Bluetooth/system routing; captions/transcript always visible; replay/mute/playback speed; interruption.

**Credentials / paid items at this phase**
- OS microphone permission only when user starts voice.

**Verification commands**
```bash
pnpm test --filter voice-client
pnpm playwright test --grep voice
```

**Phase complete only when**
- Functional device/browser voice flows pass; no background capture.


## Phase 88 — Voice quality, privacy, resilience and scale gate

**Outcome**  
Benchmark and select real production voice compute, then prove privacy/resilience/scale.

**Locked inputs / researched sources**
- AWS `eu-west-2`; ASR candidates G6f fractional L4 where viable, G6 L4, G5 A10G; TTS Fargate CPU first.
- Target voice mix: 1,000 active sessions; 400 ASR/250 TTS/250 LLM-tools/100 idle; spike 600 ASR/400 TTS/200 starts/s.

**Repository targets**
- `tools/preflight/voice-capacity/*`
- `manifests/VOICE_CAPACITY_MANIFEST.json`
- `tests/voice/load/*`
- `docs/operations/evidence/voice/`

**Exact commands / acquisition**
```bash
aws ec2 describe-instance-type-offerings --location-type availability-zone --region eu-west-2 > docs/operations/evidence/voice/instance-offerings.json
aws service-quotas list-service-quotas --service-code ec2 --region eu-west-2 > docs/operations/evidence/voice/ec2-quotas.json
```

**Implementation**
- Benchmark exact images on candidate shapes; choose cheapest passing ASR profile and cheapest passing TTS profile.
- Record throughput/p50/p95/p99/VRAM/CPU/memory/cost; test disconnect/retry/privacy/no raw-audio persistence.
- Do not buy long-lived Capacity Reservations until production provisioning requires them.

**Credentials / paid items at this phase**
- AWS account required now for real benchmark; temporary benchmark spend.

**Verification commands**
```bash
python tools/preflight/voice-capacity/run.py
pnpm test --filter voice-scale
```

**Phase complete only when**
- SLOs pass; selected shape offered in two AZs; manifest has measured capacity/cost.


## Phase 89 — LemonSlice Enterprise production contract/provider gate

**Outcome**  
Obtain LemonSlice Enterprise production terms only now, when the avatar integration is ready.

**Locked inputs / researched sources**
- Official LemonSlice API/pricing: https://lemonslice.com/avatar-api and https://lemonslice.com/pricing.
- Required: BYO LLM/voice, Actions/Emotions, ZDR, data-residency/DPA terms, production API, >=1,429 launch-contracted sessions at 1,000 provisioned floor, <=$0.16 effective billable renderer minute, documented expansion path to >=6,846 concurrent sessions without application-protocol redesign.

**Repository targets**
- `manifests/AVATAR_PROVIDER_MANIFEST.json`
- `docs/legal/providers/lemonslice/`

**Exact commands / acquisition**
```bash
# commercial account/contract step; no source-code install
```

**Implementation**
- Capture exact billing quantum, session/ramp limits, API/auth docs, subprocessors, support path, ZDR configuration evidence.
- Provider cannot access Mysta databases/tools/history directly; renderer payload is minimum necessary.

**Credentials / paid items at this phase**
- Paid LemonSlice Enterprise production contract/API credentials.

**Verification commands**
```bash
pnpm tsx tools/providers/validate-avatar-provider-manifest.ts
```

**Phase complete only when**
- Every required commercial/privacy/capacity field evidenced; otherwise avatar production path is blocked.


## Phase 90 — Mysta avatar production asset

**Outcome**  
Create/finalise the production Mysta avatar asset against the approved character reference.

**Locked inputs / researched sources**
- Original Mysta reference/provenance; LemonSlice custom-avatar requirements.

**Repository targets**
- `assets/brand/mysta/avatar/mysta-avatar-source-v1.*`
- `manifests/ASSET_MANIFEST.json`
- visual fixtures

**Exact commands / acquisition**
```bash
# provider asset upload/config performed through contracted account
```

**Implementation**
- Prepare exact crop/resolution/background/source asset; create approved expression/state reference set.
- Hash lock identity; human review for drift.

**Credentials / paid items at this phase**
- LemonSlice account from Phase 89.

**Verification commands**
```bash
pnpm test --filter mysta-avatar-visual
```

**Phase complete only when**
- Identity consistency/originality/rights pass.


## Phase 91 — LiveKit realtime media foundation

**Outcome**  
Create LiveKit Cloud production media foundation and native/web connection clients.

**Locked inputs / researched sources**
- Official LiveKit Cloud; create project with **European Union (Frankfurt)** project data region because region is immutable at creation.
- Client packages: livekit-client 2.22.3; mobile set proved in Phase 81.
- Agent Observability recording/transcript capture disabled for production Mysta sessions; no LiveKit Inference/STT/LLM/TTS.

**Repository targets**
- `packages/realtime-media`
- `apps/api/src/routes/realtime/*`
- `manifests/REALTIME_MEDIA_MANIFEST.json`

**Exact commands / acquisition**
```bash
pnpm --filter @mystaai/realtime-media add livekit-client@2.22.3
```

**Implementation**
- Server issues short-lived scoped room tokens after auth/entitlement/capacity checks.
- Measure actual participants/connections per Mysta session; do not confuse LiveKit Agent quotas with raw media-room capacity.

**Credentials / paid items at this phase**
- LiveKit Cloud account/project/API key/secret.

**Verification commands**
```bash
pnpm test --filter realtime-media
```

**Phase complete only when**
- EU project evidence stored; token scope/expiry tests pass; web + mobile join controlled test room.


## Phase 92 — Avatar gateway and self-managed pipeline

**Outcome**  
Build the Mysta-owned avatar gateway that joins LiveKit and controls LemonSlice rendering.

**Locked inputs / researched sources**
- LemonSlice contract/API; LiveKit transport; `mysta_voice_v1` audio.

**Repository targets**
- `packages/avatar-gateway`
- `apps/avatar-worker`

**Exact commands / acquisition**
```bash
# use provider SDK/API version documented in AVATAR_PROVIDER_MANIFEST; do not add provider LLM/voice
```

**Implementation**
- Create session only after entitlement/capacity check; send only validated audio/state metadata; receive avatar video into same room.
- Timeout/retry/cancel/health and duplicate-session prevention.

**Credentials / paid items at this phase**
- `LEMONSLICE_API_KEY`, `LIVEKIT_API_KEY`, `LIVEKIT_API_SECRET` in secret stores only.

**Verification commands**
```bash
pnpm test --filter avatar-gateway
```

**Phase complete only when**
- Provider timeout/join/media-loss fixtures degrade cleanly without losing session.


## Phase 93 — Living Mysta state/action/emotion controller

**Outcome**  
Implement whitelisted living-Mysta state/action/emotion finite-state controller.

**Locked inputs / researched sources**
- States READY/CONNECTING/LISTENING/REFLECTING/CHECKING_EVIDENCE/READING_CARDS/CHECKING_CHART/LOOKING_AT_NUMBERS/SPEAKING/GENTLE_SMILE/CELEBRATORY/CONCERNED/INTERRUPTED/RECONNECTING/UNAVAILABLE.

**Repository targets**
- `packages/avatar-gateway/src/state-machine/*`

**Exact commands / acquisition**
```bash
# no new dependency
```

**Implementation**
- Map application/tool events and validated semantic tone metadata to whitelisted provider Actions/Emotions.
- LLM cannot emit arbitrary animation commands; no chain-of-thought visualisation.

**Credentials / paid items at this phase**
- None.

**Verification commands**
```bash
pnpm test --filter avatar-state-machine
```

**Phase complete only when**
- Every transition valid; invalid/free-form action rejected.


## Phase 94 — Mysta avatar presentation and AVA_SEC metering

**Outcome**  
Make avatar-first Mysta live on all clients and meter AVA_SEC server-authoritatively.

**Locked inputs / researched sources**
- Meter starts only after entitlement + LemonSlice accepted + LiveKit avatar media ready.
- Included AVA spends before purchased; text/audio-only never consumes AVA.

**Repository targets**
- web/mobile Mysta presentation
- `packages/usage-metering/src/avatar/*`
- avatar usage DB tables

**Exact commands / acquisition**
```bash
# no new dependency
```

**Implementation**
- Open usage segment on provider-ready media; monotonic server timestamps; close on disconnect/failure/allowance exhaustion; reconcile provider usage.
- Failure immediately stops debit and same session continues audio-only then text-only if needed; reconnect reattaches without duplicate turn/session/debit.

**Credentials / paid items at this phase**
- Provider/media credentials already configured.

**Verification commands**
```bash
pnpm test --filter avatar-metering
pnpm playwright test --grep avatar-fallback
```

**Phase complete only when**
- No failed second charged; concurrency/race/idempotency tests pass; fallback parity preserved.


## Phase 95 — Avatar privacy, quality, economics and scale gate

**Outcome**  
Prove avatar privacy, visual quality, economics and capacity for launch and scale path.

**Locked inputs / researched sources**
- Capacity assumes `avatar_start_attempt_rate_for_capacity=1.00` for admitted Mysta sessions unless already explicit non-video accessibility/technical state.
- Launch verified floor 1,000 concurrent avatar-active sessions; contract floor `ceil(provisioned/0.70)` = 1,429 at 1,000.
- 3M Ultra full-included-minutes average capacity floor ≈4,792 concurrent; 30% headroom contract path >=6,846 before peak/top-ups.

**Repository targets**
- `tests/scale/avatar/*`
- `manifests/AVATAR_CAPACITY_MANIFEST.json`
- `docs/operations/evidence/avatar/*`

**Exact commands / acquisition**
```bash
# execute approved provider load-test procedure during contracted window
```

**Implementation**
- Measure attempted vs successful avatar concurrency, start latency, failure/fallback rate, provider billing, LiveKit participants/session and transport capacity.
- Normal capacity fallback target <=0.5%; derive post-launch provisioning from p99 attempted concurrency / 0.70.

**Credentials / paid items at this phase**
- Load-test/provider spend and approved test window.

**Verification commands**
```bash
pnpm test --filter avatar-scale
```

**Phase complete only when**
- 1,000 rated test passes; no privacy/billing defect; provider/LiveKit expansion path documented to >=6,846 without application redesign.


## Phase 96 — Accessibility audit

**Outcome**  
Run full WCAG 2.2 AA + mobile accessibility audit including first-class fallback states.

**Locked inputs / researched sources**
- WCAG 2.2 AA; screen readers, keyboard, reduced motion, captions, contrast.

**Repository targets**
- `tests/accessibility/*`
- evidence report

**Exact commands / acquisition**
```bash
pnpm playwright test tests/accessibility
```

**Implementation**
- Test every launch route plus Mysta avatar/audio/text transitions; avatar is never sole carrier of meaning.

**Credentials / paid items at this phase**
- Physical iOS/Android accessibility testing devices.

**Verification commands**
```bash
pnpm playwright test tests/accessibility
pnpm test --filter mobile-a11y
```

**Phase complete only when**
- No material accessibility blocker.


## Phase 97 — Originality audit

**Outcome**  
Prove production UI/art/audio originality and provenance.

**Locked inputs / researched sources**
- Original Mysta art/voice; rights records.

**Repository targets**
- `docs/brand/originality-audit.md`
- ASSET/BRAND/VOICE manifests

**Exact commands / acquisition**
```bash
# manual + automated perceptual hash checks against internal references
```

**Implementation**
- Review logo, avatar, tarot art, report templates, screens, sounds; remove copied/watermarked/stock-unlicensed material.

**Credentials / paid items at this phase**
- None.

**Verification commands**
```bash
pnpm tsx tools/assets/originality-audit.ts
```

**Phase complete only when**
- All production assets have provenance and pass review.


## Phase 98 — Knowledge rights audit

**Outcome**  
Re-run rights/licence audit over every knowledge/model/font/source before production publish.

**Locked inputs / researched sources**
- NIMH/PLOS/PMC/Crossref rules; Swiss licence; BGE licences; font licences; original content.

**Repository targets**
- `docs/knowledge/rights-audit.md`
- knowledge manifests

**Exact commands / acquisition**
```bash
# manifest-driven audit
```

**Implementation**
- Article-level PMC/PLOS licences checked; blocked source classes absent; OpenStax absent unless explicit permission.

**Credentials / paid items at this phase**
- None beyond licences already acquired.

**Verification commands**
```bash
pnpm tsx tools/knowledge/rights-audit.ts
```

**Phase complete only when**
- Zero unknown/NC/prohibited production knowledge.


## Phase 99 — AI acceptance

**Outcome**  
Run complete AI truth/safety/persona/psychology evaluation suite.

**Locked inputs / researched sources**
- Terra/Luna gateway, deterministic tools, truth/safety.

**Repository targets**
- `tests/ai-evals/*`
- frozen prompt/model manifests

**Exact commands / acquisition**
```bash
pnpm test --filter ai-evals
```

**Implementation**
- Evaluate contradictions, unsupported certainty, high-stakes, diagnosis, mystical-vs-evidence separation, tool failure, prompt injection, persona consistency.

**Credentials / paid items at this phase**
- OpenAI staging project/key.

**Verification commands**
```bash
pnpm test --filter ai-evals
```

**Phase complete only when**
- All launch thresholds pass; prompts/model aliases frozen.


## Phase 100 — Security hardening

**Outcome**  
Execute security hardening against web/mobile/API/cloud/provider threat model.

**Locked inputs / researched sources**
- OWASP ASVS 5.0.0; MASVS 2.1.0/MASWE; dependency/container/IaC scanning.

**Repository targets**
- `tests/security/*`
- `docs/security/threat-model.md`
- SECURITY_CONTROL_MANIFEST

**Exact commands / acquisition**
```bash
# run configured SAST/SCA/DAST/container/IaC tools
```

**Implementation**
- Auth/session, IDOR, rate limit, SSRF, injection, webhook replay, wallet races, secret scanning, mobile storage, provider tokens, WebSocket abuse.

**Credentials / paid items at this phase**
- Staging environment.

**Verification commands**
```bash
pnpm test --filter security
```

**Phase complete only when**
- No unresolved Critical/High exploitable launch blocker.


## Phase 101 — Performance/load hardening

**Outcome**  
Prove the single-region 2–3M design envelope with load/soak/spike/failure tests.

**Locked inputs / researched sources**
- 150k concurrent sessions; 15k sustained auth API RPS; 30k short spike; 1500 interactive AI generations; 3000 async jobs/s burst; voice/avatar gates separately included.

**Repository targets**
- `tests/scale/*`
- SCALE_MANIFEST
- SERVICE_QUOTA_MANIFEST

**Exact commands / acquisition**
```bash
# deploy production-equivalent staging then execute k6/approved load harness
```

**Implementation**
- Measure ALB/ECS/Aurora/RDS Proxy/Redis/SQS/provider saturation/autoscaling; 3M synthetic account routing/data-volume; single-cell saturation/isolation; soak/restore/quota alarms.

**Credentials / paid items at this phase**
- AWS staging/prod-like account and temporary load-test spend.

**Verification commands**
```bash
pnpm test --filter scale
```

**Phase complete only when**
- All SLO/headroom targets pass or launch blocked.


## Phase 102 — Financial/billing/allowance/wallet acceptance

**Outcome**  
Recalculate current full-use economics and prove all billing/ledger lifecycles.

**Locked inputs / researched sources**
- Current OpenAI prices, LemonSlice/LiveKit signed costs, AWS measured voice costs, Stripe/RevenueCat/store fees are pulled now using official/account sources.
- Assume 100% included avatar allowance consumption and 100% purchased-minute redemption for margin safety.

**Repository targets**
- COST_ENVELOPE_MANIFEST
- STORE_PRODUCT_MANIFEST
- billing evidence

**Exact commands / acquisition**
```bash
# run billing acceptance + cost model scripts
```

**Implementation**
- Test monthly Premium/Ultra only, upgrades/downgrades/cancel/renew/grace/restore/refund/revocation; READ/AVA/report purchases; web/native reconciliation.
- Verify no annual/trial product exposed.

**Credentials / paid items at this phase**
- Live sandbox/test store/payment accounts.

**Verification commands**
```bash
pnpm test --filter billing-acceptance
pnpm tsx tools/cost/recompute-launch-economics.ts
```

**Phase complete only when**
- Positive variable contribution margin on every launch channel/product; ledgers reconcile exactly.


## Phase 103 — Disaster recovery

**Outcome**  
Perform real backup/PITR restore and application reconnection drill.

**Locked inputs / researched sources**
- Aurora snapshot/PITR, S3 backup/versioning policies.

**Repository targets**
- `docs/operations/evidence/dr/`
- runbook

**Exact commands / acquisition**
```bash
# AWS restore commands are generated from Terraform outputs and runbook
```

**Implementation**
- Restore isolated replacement cluster; validate critical invariants; point staging-equivalent stack at restore; record RTO/RPO.

**Credentials / paid items at this phase**
- AWS production/staging privileges.

**Verification commands**
```bash
pnpm tsx tools/dr/validate-restored-db.ts
```

**Phase complete only when**
- Restore proven; RTO/RPO within targets.


## Phase 104 — Privacy/legal gate

**Outcome**  
Complete privacy/legal/terms/provider-transfer/age/licence gate.

**Locked inputs / researched sources**
- UK GDPR/ICO international transfers current guidance; OpenAI/LiveKit/LemonSlice DPAs; terms/privacy; 18+; entertainment/reflection framing.

**Repository targets**
- `docs/legal/*`
- `docs/privacy/*`
- DATA_RETENTION_MANIFEST

**Exact commands / acquisition**
```bash
# legal review/document finalisation
```

**Implementation**
- Map processors/subprocessors/transfers/safeguards; consent/retention/export/delete; no medical/therapy claims; character consent/provenance; all paid licences.

**Credentials / paid items at this phase**
- Legal review where required.

**Verification commands**
```bash
pnpm tsx tools/legal/prelaunch-check.ts
```

**Phase complete only when**
- No material privacy/legal/licence blocker.


## Phase 105 — App Store preparation

**Outcome**  
Prepare and submit iOS App Store production release.

**Locked inputs / researched sources**
- Current Apple App Review Guidelines and IAP rules rechecked immediately before submission; Expo SDK 57 iOS requirements satisfied.

**Repository targets**
- `store/apple/*`
- App Store Connect metadata/screenshots/review notes

**Exact commands / acquisition**
```bash
cd apps/mobile && eas build --platform ios --profile production
eas submit --platform ios --profile production
```

**Implementation**
- Create final IAP/subscription metadata/localisations, privacy nutrition labels, age rating, reviewer account/notes explaining Mysta/AI/divination/subscriptions.

**Credentials / paid items at this phase**
- Apple Developer/App Store Connect production access/signing.

**Verification commands**
```bash
cd apps/mobile && npx expo-doctor
```

**Phase complete only when**
- Build accepted by App Store; purchase restore and production receipt validation pass.


## Phase 106 — Google Play preparation

**Outcome**  
Prepare and submit Google Play production release.

**Locked inputs / researched sources**
- Current Google Play billing/digital-goods/data-safety/target API rules rechecked immediately before submission.

**Repository targets**
- `store/google/*`
- Play Console metadata/screenshots/data-safety

**Exact commands / acquisition**
```bash
cd apps/mobile && eas build --platform android --profile production
eas submit --platform android --profile production
```

**Implementation**
- Create subscriptions/consumables, data safety, content rating, review instructions.

**Credentials / paid items at this phase**
- Google Play Console/service account/signing.

**Verification commands**
```bash
cd apps/mobile && npx expo-doctor
```

**Phase complete only when**
- Play review accepted; billing/restore production validation passes.


## Phase 107 — Production Terraform

**Outcome**  
Provision the complete production AWS edge + one `eu-west-2` cell from versioned Terraform.

**Locked inputs / researched sources**
- Route53/CloudFront/WAF/ALB/ECS Fargate/RDS Proxy/Aurora PostgreSQL/ElastiCache/SQS/S3/SES/CloudWatch/KMS/Secrets Manager; GPU ECS/EC2 ASR pool selected Phase 88.
- No Global Accelerator, second authoritative region, DynamoDB Global Tables or Aurora Global Database at launch.

**Repository targets**
- `infra/terraform/edge`
- `infra/terraform/region`
- `infra/terraform/cell`
- `manifests/SERVICE_QUOTA_MANIFEST.json`
- `REGION_CELL_MANIFEST.json`

**Exact commands / acquisition**
```bash
terraform -chdir=infra/terraform init
terraform -chdir=infra/terraform fmt -check
terraform -chdir=infra/terraform validate
terraform -chdir=infra/terraform plan -out=tfplan
```

**Implementation**
- Query actual eu-west-2 service quotas/AZ offerings/current Aurora patch; request increases needed for measured target.
- Activate minimum two-AZ ASR On-Demand Capacity Reservations before voice production enablement.
- Apply only reviewed plan; tags/budgets/alarms/backups.

**Credentials / paid items at this phase**
- AWS production account/IAM, domain/DNS ownership.

**Verification commands**
```bash
terraform -chdir=infra/terraform validate
terraform -chdir=infra/terraform show tfplan >/dev/null
```

**Phase complete only when**
- Applied infrastructure matches manifests; quota/headroom and two-AZ voice capacity prerequisites pass.


## Phase 108 — Production secrets

**Outcome**  
Load production secrets through Secrets Manager/KMS; prove none exist in source/history.

**Locked inputs / researched sources**
- All integration env names from V3.

**Repository targets**
- AWS Secrets Manager/KMS
- ENVIRONMENT_MANIFEST
- secret rotation runbooks

**Exact commands / acquisition**
```bash
# use aws secretsmanager put-secret-value for each generated/provided secret; never echo values into logs
```

**Implementation**
- Store DB/OpenAI/Stripe/RevenueCat/Apple/Google/GeoNames/LiveKit/LemonSlice/SES/etc credentials with least-privilege task roles.
- Rotate any credential ever exposed in development logs/chat/source.

**Credentials / paid items at this phase**
- All production credentials.

**Verification commands**
```bash
git grep -nE "(sk-|secret|api[_-]?key)" -- . ":(exclude).env.example" || true
pnpm test --filter secret-scan
```

**Phase complete only when**
- Secret scan/history scan clean; tasks can access only required secrets.


## Phase 109 — Production database

**Outcome**  
Create/migrate production Aurora and supporting data services.

**Locked inputs / researched sources**
- Terraform outputs; Prisma 7.10.0 migrations; AWS-supported pgvector version selected Phase 107.

**Repository targets**
- Production Aurora/RDS Proxy/Redis/SQS/DLQ/S3 resources
- migration evidence

**Exact commands / acquisition**
```bash
pnpm --filter @mystaai/database exec prisma migrate deploy
```

**Implementation**
- Enable pgvector; migrate schema; seed only approved static data; verify routing row defaults and DB encryption/backups.

**Credentials / paid items at this phase**
- Production DB secret through runtime role/secret store.

**Verification commands**
```bash
pnpm --filter @mystaai/database exec prisma migrate status
pnpm tsx tools/prod/db-smoke.ts
```

**Phase complete only when**
- Migrations clean; no dev/test fake data.


## Phase 110 — Production knowledge publish

**Outcome**  
Publish the immutable initial production knowledge version.

**Locked inputs / researched sources**
- Only rights-audited knowledge from Phases 29–37/98.

**Repository targets**
- Production knowledge tables/vector index/S3 raw archive
- knowledge release manifest

**Exact commands / acquisition**
```bash
pnpm tsx tools/knowledge/publish.ts --environment production
```

**Implementation**
- Upload immutable raw archives where required; load structured records/embeddings/graph; set one active version atomically.

**Credentials / paid items at this phase**
- Production DB/S3 service role.

**Verification commands**
```bash
pnpm tsx tools/knowledge/verify-published.ts
```

**Phase complete only when**
- All source IDs/hashes/licences resolve; no orphan chunks/edges; active version reproducible.


## Phase 111 — Production smoke test

**Outcome**  
Smoke every real production integration and critical failure path.

**Locked inputs / researched sources**
- Real production auth, GeoNames, Swiss, Terra/Luna, Whisper/Kokoro, knowledge, email/push, payments, LiveKit, LemonSlice.

**Repository targets**
- `docs/operations/evidence/prod-smoke/`

**Exact commands / acquisition**
```bash
# execute controlled production-smoke script with non-destructive test accounts/products
```

**Implementation**
- Test Terra paid + Luna Free cap, LiveKit EU connectivity, LemonSlice avatar ready, server AVA metering, forced provider/media failure debit stop, audio/text continuity and recovery with no duplicate turn/session/debit.

**Credentials / paid items at this phase**
- Controlled production test users/store sandbox-or-approved test mechanism.

**Verification commands**
```bash
pnpm tsx tools/prod/smoke.ts
```

**Phase complete only when**
- All smoke checks pass and evidence archived.


## Phase 112 — Full acceptance journey

**Outcome**  
Run the complete real-user acceptance journey on web/iOS/Android.

**Locked inputs / researched sources**
- All production systems complete.

**Repository targets**
- `tests/acceptance/full-journey/*`
- acceptance evidence videos/logs

**Exact commands / acquisition**
```bash
# execute Playwright + physical-device checklist
```

**Implementation**
- Flow: open → account → birth profile → initial profile → welcome → tarot → numerology → natal → transit → while Free attempt Mysta and verify paywall → exercise Free cap → subscribe → enter avatar-first Mysta → speak → type same session → interrupt → force avatar failure/audio parity → force audio unavailable/text parity → restore avatar no duplicates/debit → dream → compatibility → cross-system → subscriber reading → report → history → restore purchase → export data.

**Credentials / paid items at this phase**
- Real/sandbox test purchases according to store rules.

**Verification commands**
```bash
pnpm playwright test tests/acceptance/full-journey
```

**Phase complete only when**
- Journey passes on web and physical iOS/Android with exact entitlement/metering evidence.


## Phase 113 — Launch

**Outcome**  
Release Web, iOS and Android only after every hard blocker is clear.

**Locked inputs / researched sources**
- All Phases 0–112 green; prelaunch `infra/scripts/preflight-lock.ts prelaunch` green.

**Repository targets**
- production deployments/store releases
- launch dashboard/runbook

**Exact commands / acquisition**
```bash
pnpm tsx infra/scripts/preflight-lock.ts prelaunch
# deploy web/api/workers through CI
# release approved iOS/Android store builds
```

**Implementation**
- Enable production traffic gradually within approved deployment strategy; monitor auth/payments/AI/queues/voice/avatar/cost/security/privacy.

**Credentials / paid items at this phase**
- Production deployment and store release permissions.

**Verification commands**
```bash
pnpm tsx tools/prod/post-deploy-check.ts
```

**Phase complete only when**
- Web live; stores released; SLO dashboards healthy; no hard launch blocker.


## Phase 114 — Stabilisation gate

**Outcome**  
Hold product scope stable while launch telemetry proves reliability/economics.

**Locked inputs / researched sources**
- Crash/error/payment/READ-AVA reconciliation/Free spend/AI validation/queue/voice/avatar metrics.

**Repository targets**
- stabilisation dashboard
- incident/capacity/cost runbooks

**Exact commands / acquisition**
```bash
# continuous monitoring and incident runbooks
```

**Implementation**
- No major feature work until crash/error/security/privacy/payment/validator/queue/voice/avatar gates are stable.
- Track attempted-avatar p99 and expand provisioned/contracted capacity before 70% utilisation; capacity fallback <=0.5% normal operation.
- Reconcile avatar provider usage vs AVA ledger; investigate any failed-interval debit.

**Credentials / paid items at this phase**
- Operational provider/cloud accounts.

**Verification commands**
```bash
pnpm tsx tools/ops/stabilisation-check.ts
```

**Phase complete only when**
- All stabilisation targets green for the configured observation window.


---

# 109. LAUNCH BLOCKERS — HARD LOCK

Launch is additionally blocked by any of:

- GPT-5.6 Terra/Luna model or quota mismatch;
- Mysta accessible to Free users;
- Free allowance exceeding its cost ceiling or lacking atomic admission control;
- purchased READ/AVA expiry or wallet overspend/reconciliation defect;
- LemonSlice Enterprise contract missing, renderer rate >$0.16/min, concurrency <1,429, ZDR unavailable or required DPA/data-residency evidence incomplete;
- LiveKit project not using EU data region, connection capacity below rated need, or production observability recording unexpectedly enabled;
- live avatar not available on web/iOS/Android;
- avatar usage can debit before readiness, after failure, or twice;
- Premium/Ultra full-use variable contribution margin negative on any launch sales channel;
- Apple/Google product types/prices/fees not reverified;
- Free/paid native digital purchases bypassing required store billing.

MystaAI cannot launch while any of the following remains unresolved:

```text
Swiss Ephemeris commercial licence absent
Mysta character authorship/provenance or supplied consent record missing
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
VOICE_MANIFEST incomplete or voice asset hash mismatch
VOICE_CAPACITY_MANIFEST incomplete or contains applicable TBD/null/placeholder capacity/cost fields
selected ASR GPU instance not offered in both locked eu-west-2 Availability Zones
minimum ASR Capacity Reservation absent/inactive in either selected AZ
applied G/VT quota below required quota/headroom
AWS price evidence stale/missing for selected voice compute profile
voice_mix_v1 or ASR/TTS spike capacity test failed
voice infrastructure rated/monthly cost envelope not owner-approved
external metered STT/TTS dependency present in normal production path
voice ASR accuracy/latency gate failed
voice TTS identity/pronunciation/latency gate failed
voice barge-in/reconnect/privacy gate failed
raw microphone audio persistence detected
```

---

# 110. FINAL PRODUCTION ACCEPTANCE CRITERIA

## Functional

Every launch feature specified in this document exists and works.

## Visual

Every launch screen uses the locked MystaAI brand, passes visual regression, accessibility and originality review.

## Mysta

Mysta is consistent with the approved visual reference. The character is the user's original character inspired by the user's mother with consent already established; the product retains the required provenance record. `mysta_voice_v1` is an original synthetic voice and is not a clone of the mother or another identifiable person.

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

## Voice

Mysta's approved synthetic voice is installed and used consistently. Live voice conversation works end-to-end on web, iOS and Android, with self-hosted ASR/TTS, transcripts, interruption/barge-in, privacy controls, reconnects and text fallback. Normal production speech contains no metered STT/TTS API dependency.

Whisper ASR runs on the benchmark-selected GPU profile with active two-AZ minimum Capacity Reservations. Kokoro TTS runs on the benchmark-selected CPU-first profile unless the Phase-88 evidence selected GPU. `VOICE_CAPACITY_MANIFEST.json` contains the exact production AZs, compute shapes, passing concurrency, applied quotas, current AWS price evidence, autoscale limits, floor/rated monthly costs and measured cost/session. The rated `voice_mix_v1` and spike tests pass with >=30% quota headroom.

## Reliability

Retries, queues, webhook idempotency, provider outages and rollback paths are tested.

## Infrastructure / single-region scale

Backup restore succeeds. Cell capacity is measured; single-region routing/control plane passes; 3M synthetic paying-account routing/data-volume validation passes; single-cell saturation and failure isolation pass; provider/service/voice-compute capacity headroom is documented for the active Region; adding capacity or a second Region later does not require product redesign.

## Mobile

Physical iOS/Android acceptance passes.

## Web

Supported-browser acceptance passes.

- Free users cannot create or enter a Mysta session by route/API/WebSocket/credit manipulation;
- Free daily allowance defaults are <=$0.001 provider cost/day and adjustable without app release under the owner ceiling;
- Free user can purchase/use non-expiring READ for eligible non-chat readings;
- Premium $7.99 and Ultra $21.99 monthly products and 25/69 included avatar-minute grants reconcile across web/iOS/Android;
- £10/30m and £20/75m purchased avatar top-ups never expire and cannot be spent without active subscriber entitlement;
- live Mysta avatar is present at launch on web/iOS/Android and passes state/visual/lip-sync/fallback tests;
- 1,000 concurrent avatar-active Mysta rated test passes or launch capacity claim is not accepted;
- LemonSlice/LiveKit/OpenAI/voice/store costs and quotas match the signed launch manifests.

---

# 111. PERMANENT ARCHITECTURE RULES

1. OpenAI (GPT-5.6 Terra) never calculates authoritative astrology.
2. OpenAI (GPT-5.6 Terra) never calculates authoritative numerology.
3. OpenAI (GPT-5.6 Terra) never selects authoritative tarot cards.
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
23. MystaAI V3 launches as one `eu-west-2` production cell; capacity is increased inside that Region first, and later cell/Region expansion must preserve product contracts.
24. The Aurora writer is allowed to be the V3 authoritative transactional writer; its measured capacity may not be exceeded without an approved capacity increase or later cell split.
25. Every user has an authoritative `home_region`, `cell_id` and `routing_version`; V3 launch values are `eu-west-2` / `cell-001`.
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
36. `mysta_voice_v1` is the production Mysta voice until a controlled versioned replacement is approved.
37. Production STT/TTS is self-hosted; normal voice use must not require a metered external speech API.
38. Raw microphone audio is transient by default and may not become an undeclared stored dataset.
39. Voice transcripts are user input only after final acceptance/confirmation; partial ASR is not authoritative intent.
40. TTS may speak only validated Mysta text; it cannot bypass truth/safety validation.
41. Voice inference model artefacts are pinned, hashed and unavailable for silent runtime replacement.
42. Voice failure must degrade to explicit retry/text fallback, never fabricated transcript or fabricated audio success.

43. LemonSlice is a replaceable presentation boundary only through an owner-approved future spec revision; no provider can own Mysta's intelligence/voice/evidence.
44. Mysta is embodied by the live avatar at launch; the avatar is not an optional second product or post-launch bootstrap item.
45. Free users never receive Mysta; READ cannot buy access to it.
46. Free AI direct cost is runtime-capped and the cost ceiling outranks nominal token caps.
47. Purchased READ and AVA_SEC never expire.
48. Subscription-included AVA_SEC expires/reset only at the billing-cycle boundary and spends before purchased AVA_SEC.
49. Text input and audio-only/text-only fallback inside Mysta do not consume AVA_SEC; only provider-ready avatar-active Mysta time does.
50. Store purchase receipts/webhooks grant internal ledger value exactly once; client state and RevenueCat API balances are not the high-throughput spend authority.
51. LemonSlice/LiveKit outage degrades to voice/text without charging failed avatar time.
52. Paid prices/allowances are owner-locked; Free allowance values are operationally adjustable only within the owner hard cost ceiling.
53. There is exactly one subscriber Mysta product surface and entitlement. No standalone chatbot/chat product, separate voice product or separately marketed avatar product may be implemented.
54. Voice/text are input methods; audio-only/text-only are fallback presentation states; all remain inside the same Mysta session and conversation identity.
55. Audio-only/text-only continuity is feature/intelligence parity, not a stripped-down product; only unavailable media presentation may differ.
56. Avatar capacity planning assumes 100% avatar-start attempts for admitted Mysta sessions unless a session is already explicitly accessibility/technical non-video; no mixed-mode discount is allowed in Phase-95 capacity/economic acceptance.
57. The 1,000-avatar figure is a minimum launch verified floor, not the 2–3M design ceiling; provider/transport scaling must satisfy Section 85.1A without application/protocol redesign and production capacity expands from attempted-demand telemetry before 70% utilisation.

---

# 112. RESEARCH / SOURCE REGISTER — LOCK-DATE REFERENCES

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
- OpenAI GPT-5.6 Terra model page: https://developers.openai.com/api/docs/models/gpt-5.6-terra
- OpenAI GPT-5.6 Luna model page: https://developers.openai.com/api/docs/models/gpt-5.6-luna
- OpenAI deprecations: https://developers.openai.com/api/docs/deprecations
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
- RevenueCat pricing: https://www.revenuecat.com/pricing
- RevenueCat virtual currency/expiration reference: https://www.revenuecat.com/docs/offerings/virtual-currency/expiring-currencies
- RevenueCat virtual-currency API rate limit reference: https://www.revenuecat.com/docs/offerings/virtual-currency
- RevenueCat webhooks: https://www.revenuecat.com/docs/integrations/webhooks
- Apple App Review Guidelines: https://developer.apple.com/app-store/review/guidelines/
- Apple IAP pricing/localization: https://developer.apple.com/help/app-store-connect/manage-in-app-purchases/set-a-price-for-an-in-app-purchase
- Google Play service fees: https://support.google.com/googleplay/android-developer/answer/112622
- Google Play payments policy: https://support.google.com/googleplay/android-developer/answer/9858738

## Live avatar / realtime media

- LemonSlice pricing/Enterprise capabilities: https://lemonslice.com/pricing
- LemonSlice Avatar API: https://lemonslice.com/avatar-api
- LiveKit LemonSlice plugin: https://docs.livekit.io/agents/models/avatar/plugins/lemonslice/
- LiveKit Expo quickstart: https://docs.livekit.io/transport/sdk-platforms/expo/
- LiveKit pricing/capacity: https://livekit.com/pricing
- LiveKit EU data residency: https://docs.livekit.io/deploy/admin/regions/data-residency/
- LiveKit region pinning: https://docs.livekit.io/deploy/admin/regions/region-pinning/

## AWS / single-region scale

- Aurora PostgreSQL updates: https://docs.aws.amazon.com/AmazonRDS/latest/AuroraPostgreSQLReleaseNotes/AuroraPostgreSQL.Updates.html
- Aurora PostgreSQL supported extensions: https://docs.aws.amazon.com/AmazonRDS/latest/AuroraPostgreSQLReleaseNotes/AuroraPostgreSQL.Extensions.html
- RDS Proxy for Aurora: https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/rds-proxy.html
- SQS Standard queues: https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/standard-queues.html
- ECS service autoscaling: https://docs.aws.amazon.com/AmazonECS/latest/developerguide/service-auto-scaling.html
- ECS/Fargate quotas: https://docs.aws.amazon.com/AmazonECS/latest/developerguide/service-quotas.html
- CloudFront quotas: https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/cloudfront-limits.html
- ElastiCache Redis OSS support: https://docs.aws.amazon.com/AmazonElastiCache/latest/dg/engine-versions.html

## Self-hosted voice

- Kokoro-82M model card/release/licence: https://huggingface.co/hexgrad/Kokoro-82M
- Kokoro source repository: https://github.com/hexgrad/kokoro
- faster-whisper repository/releases/licence: https://github.com/SYSTRAN/faster-whisper
- OpenAI Whisper large-v3-turbo model/licence: https://huggingface.co/openai/whisper-large-v3-turbo
- Expo Audio (`expo-audio`) documentation: https://docs.expo.dev/versions/latest/sdk/audio/

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


## V3 additional implementation research lock

- OpenAI official Node/TypeScript SDK 7.23.0: https://www.npmjs.com/package/openai

- pg 8.23.0: https://www.npmjs.com/package/pg
- Pino 10.3.1: https://www.npmjs.com/package/pino
- Ajv 8.20.0 / ajv-formats 3.0.1: https://www.npmjs.com/package/ajv and https://www.npmjs.com/package/ajv-formats
- Fastify plugin compatibility/package pages: https://www.npmjs.com/package/@fastify/cors ; https://www.npmjs.com/package/@fastify/helmet ; https://www.npmjs.com/package/@fastify/cookie ; https://www.npmjs.com/package/@fastify/rate-limit ; https://www.npmjs.com/package/@fastify/swagger ; https://www.npmjs.com/package/@fastify/swagger-ui ; https://www.npmjs.com/package/@fastify/sensible ; https://www.npmjs.com/package/@fastify/websocket
- FastAPI 0.141.1: https://pypi.org/project/fastapi/
- Uvicorn 0.54.0: https://pypi.org/project/uvicorn/
- geo-tz 8.1.9: https://www.npmjs.com/package/geo-tz
- Project Gutenberg astrology reference (Sepharial, 1920): https://www.gutenberg.org/ebooks/46963
- University of Illinois 1911 numerology reference (Sepharial): https://brittlebooks.library.illinois.edu/brittlebooks_open/Books2012-04/sephar0001kabofn/
- Project Gutenberg Cosmic Symbolism (Sepharial, 1912): https://www.gutenberg.org/ebooks/70749
- Expo SDK compatibility table: https://docs.expo.dev/versions/latest/
- Expo TypeScript install guidance: https://docs.expo.dev/guides/typescript/
- LiveKit Expo quickstart: https://docs.livekit.io/transport/sdk-platforms/expo/
- LiveKit React Native Expo plugin: https://www.npmjs.com/package/@livekit/react-native-expo-plugin
- LiveKit React Native SDK: https://www.npmjs.com/package/@livekit/react-native
- LiveKit React Native WebRTC: https://www.npmjs.com/package/@livekit/react-native-webrtc
- LiveKit JS client: https://www.npmjs.com/package/livekit-client
- Fastify WebSocket: https://www.npmjs.com/package/@fastify/websocket
- Kokoro runtime 0.9.4 (Python >=3.10,<3.13): https://pypi.org/project/kokoro/
- Misaki G2P: https://pypi.org/project/misaki/
- Python 3.12.14 release: https://www.python.org/doc/versions/
- faster-whisper releases: https://github.com/SYSTRAN/faster-whisper/releases
- CTranslate2 4.8.2: https://pypi.org/project/ctranslate2/
- BAAI/bge-m3 model card/licence: https://huggingface.co/BAAI/bge-m3
- BAAI/bge-reranker-v2-m3: https://huggingface.co/BAAI/bge-reranker-v2-m3
- GeoNames Premium Web Services: https://www.geonames.org/commercial-webservices.html
- IANA tzdb 2026d: https://www.iana.org/time-zones/releases/2026d
- JPL Horizons API: https://ssd-api.jpl.nasa.gov/doc/horizons.html
- A. E. Waite, The Pictorial Key to the Tarot historical source: https://www.sacred-texts.com/tarot/pkt/pkttp.htm
- NIMH reuse policy: https://www.nimh.nih.gov/site-info/policies
- PLOS API and TDM: https://api.plos.org/ and https://api.plos.org/text-and-data-mining.html
- PMC automated-retrieval rules: https://pmc.ncbi.nlm.nih.gov/tools/developers/
- PMC OAI-PMH: https://pmc.ncbi.nlm.nih.gov/tools/oai/
- Crossref REST API: https://www.crossref.org/documentation/retrieve-metadata/rest-api/

---

# 113. FINAL STATUS

**V3 status:** final, implementation-locked product design and zero-guesswork build plan.

V3 defines the complete production MystaAI product and the implementation path required to build it Phase 0 through Phase 114. Stable architecture and researched dependencies/sources are specified in advance. Values that can only exist at account/provisioning time are captured in the exact integration phase that needs them and are checked against V3 pass/block rules; they are never left to developer invention.

The product model is explicit: **Mysta is one subscriber-only embodied experience, avatar-first at launch, with voice or text input in the same persistent session and audio-only/text-only presentations as first-class technical/accessibility continuity states; Premium $7.99/month; Ultra $21.99/month; 25/69 included avatar minutes; subscriber avatar top-ups; configurable near-zero-cost Free daily AI allowance; Free-user purchasable non-expiring Reading Credits; and GPT-5.6 Terra/Luna routing.**

MystaAI is built **start to finish**. Difficult features are not silently omitted, substituted or deferred merely to meet a date.

**END OF MYSTAAI V3 FINAL IMPLEMENTATION-LOCKED PRODUCT DESIGN & ZERO-GUESSWORK BUILD PLAN**
