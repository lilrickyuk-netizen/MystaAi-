# MystaAI — FINAL IMPLEMENTATION-LOCKED PRODUCT DESIGN & BUILD SPECIFICATION

**Version:** 2.5  
**Status:** FINAL / IMPLEMENTATION LOCKED  
**Lock date:** 26 September 2026  
**Research basis:** v2.5 preserves the complete v2.4 product, single-region 2–3M-user architecture and hardened self-hosted voice design; integrates the live Mysta avatar as a launch feature; replaces obsolete GPT-5 mini with the current supported GPT-5.6 Terra/Luna routing; replaces the former four-tier pricing with the owner-locked Free/Premium/Ultra commercial model; makes Mystic Chat subscriber-only; adds a negligible-cost configurable Free daily AI allowance; permits Free users to buy non-expiring Reading Credits; and hardens Apple/Google/RevenueCat/LemonSlice/LiveKit economics, metering, privacy, concurrency and store-compliance rules. External assumptions were deep-reverified on 26 September 2026 against current official OpenAI, Apple, Google Play, RevenueCat, LemonSlice, LiveKit, AWS, Kokoro and Whisper sources.  
**Product name:** **MystaAI**  
**Positioning:** **MystaAI — Your Personal AI Mystic**  
**Product type:** Commercial AI-assisted divination, spiritual-reflection and personalised reading platform  
**Launch surfaces:** Web, iOS, Android  
**Build standard:** Full production product — **not an MVP, not a demo, not a prototype**  
**Internal slug:** `mystaai`  
**Application namespace:** `com.richardcurley.mystaai`  
**Paid production AI:** OpenAI GPT-5.6 Terra (`gpt-5.6-terra`) through the Responses API  
**Free-cost AI:** OpenAI GPT-5.6 Luna (`gpt-5.6-luna`) under the locked daily cost/token/generation budget; Free users cannot access Mystic Chat  
**Production speech synthesis:** self-hosted Kokoro-82M v1.0 with Mysta-owned `mysta_voice_v1`; no metered external TTS API  
**Production speech recognition:** self-hosted Whisper large-v3-turbo through pinned faster-whisper/CTranslate2 on GPU-backed ECS/EC2 inference capacity; no metered external STT API  
**Live avatar provider:** LemonSlice Enterprise, BYO LLM/voice mode, Zero Data Retention option enabled, contracted concurrency/cost gate mandatory before Phase 1  
**Realtime media:** LiveKit Cloud Scale, WebRTC transport only; MystaAI retains its own ASR/LLM/TTS/orchestration and conversation state  
**Cloud architecture:** AWS global web edge + single-region application/data plane in `eu-west-2`; one production cell at launch, cell-ready expansion retained  
**Minimum launch age:** 18+

> **This document is the single build authority for MystaAI. Version 2.5 supersedes v2.4, v2.3, v2.2, v2.1, v2.0 and the separate `MystaAI_Monetization_Addendum_v1`. No merge is required.** The live avatar, final launch pricing, Free allowance, Reading Credits, subscriber avatar allowance and store/payment rules are part of the core launch design. If implementation conflicts with this document, implementation is wrong unless a formally approved specification revision exists.

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

## 0.5 Phase 0 specification-control gate

Before Phase 1 begins, execute **Phase 0 — v2.5 design/control verification**. Phase 0 does not author production application code. It resolves and freezes every external fact that cannot honestly be hard-coded months in advance.

Phase 0 must reverify and record evidence for:

- psychology source/licence rules in Sections 46–50;
- OpenAI `gpt-5.6-terra` and `gpt-5.6-luna` availability, Responses API, Structured Outputs/tool calling, current token prices and the production project's actual rate/usage limits;
- the OpenAI deprecation register; a model scheduled to disappear before the expected launch window is prohibited;
- official `openai` SDK availability/security for the pinned version;
- Kokoro-82M v1.0 model/repository hash, Apache-2.0 status and compatible runtime;
- Whisper large-v3-turbo source revision/licence plus faster-whisper/CTranslate2 compatibility;
- `expo-audio`, LiveKit Expo/React Native native-module requirements and the locked Expo SDK;
- LemonSlice Enterprise: signed commercial terms, BYO LLM/voice support, ZDR option, data-residency terms, Action/Emotion capability, contract concurrency and billable-usage granularity;
- **LemonSlice effective contracted avatar-render rate <= USD 0.16 per billable minute at the launch commitment/volume**; if this cannot be obtained, Phase 1 is blocked and the owner must approve a v2.5 commercial/provider revision;
- LemonSlice contracted concurrent-session capacity >= `ceil(1000 / 0.70) = 1,429` so the 1,000-session rated avatar test retains >=30% contractual headroom;
- LiveKit Cloud Scale current plan terms, >=5,000 concurrent WebRTC connections, EU project-data region availability, DPA/sub-processors and current WebRTC/data-transfer charges;
- LiveKit project creation with **European Union (Frankfurt)** project-data region; this choice is immutable and therefore must be correct before implementation;
- LiveKit Agent Observability recording/transcript capture disabled for production; MystaAI does not use LiveKit Inference, managed STT, managed LLM or managed TTS;
- current Apple App Review §3.1.1 digital-purchase/credit rules, App Store price-point/localisation rules and the account's actual commission/program status;
- current Google Play billing/digital-goods rules and actual service/billing fee structure for launch markets;
- current RevenueCat pricing, webhook behaviour and rate limits; RevenueCat is purchase/entitlement transport, not the high-throughput per-reading wallet authority;
- Stripe API/version/current fees for web billing;
- current `eu-west-2` G6f/G6/G5 availability, exact AZ offerings, production account G/VT quota, Fargate quota/launch rates, AWS Price List values and minimum ASR Capacity Reservations;
- AWS Aurora/RDS Proxy/SQS/ElastiCache/CloudFront/WAF/ALB/ECS/Fargate support and account quotas;
- active Region/cell/legal/data-residency plan;
- the scale envelope and load-test tooling;
- all store product IDs, base-country/base-currency price points and sandbox products;
- complete channel-aware contribution-margin evidence in `COST_ENVELOPE_MANIFEST.json` at **full included avatar allowance use**, including store/processor fees, RevenueCat fees, LemonSlice, LiveKit, OpenAI, self-hosted voice compute and attributable variable infrastructure.

A material external change changes implementation material, not MystaAI's locked product outcome. No substitute provider, pricing tier, model or entitlement may be selected silently.

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
- OpenAI Terra/Luna API/model availability, deprecation status, prices and actual project quotas;
- LemonSlice Enterprise contract/ZDR/concurrency/rate evidence;
- LiveKit Scale plan/project-data-region/privacy/capacity evidence;
- Apple/Google current digital-goods, subscription and purchased-credit rules;
- actual store commission/service-fee status and RevenueCat pricing;
- self-hosted Kokoro/Whisper model availability, hashes and licence status;
- voice runtime/container/GPU-instance availability in `eu-west-2`;
- actual account-level G/VT and Fargate On-Demand vCPU quota headroom;
- current AWS Price List API rates for the locked voice compute profile;
- minimum ASR Capacity Reservations are active in two selected Availability Zones before production enablement;
- `VOICE_CAPACITY_MANIFEST.json` contains no unresolved/TBD/null production-capacity or cost field;
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

MystaAI is a persistent personal AI mystic combining deterministic divination engines, controlled traditional knowledge, personal history, evidence-informed reflection, AI-generated interpretation, a consistent synthetic voice and a live visual Mysta presence.

Launch includes:

- Tarot;
- Zodiac / star signs;
- Pythagorean numerology;
- Natal astrology;
- Current transits and transit-to-natal interpretation;
- Synastry and compatibility;
- Dream interpretation and dream journal;
- AI Mystic conversation by text for active Premium/Ultra subscribers only;
- live two-way Mystic voice conversation for active Premium/Ultra subscribers only;
- **live real-time animated Mysta avatar at launch**, lip-synchronised to `mysta_voice_v1`, with approved listening/reflecting/reading/speaking/gesture states;
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
   Paid / credit-backed / Mystic → GPT-5.6 TERRA
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
19. **Mystic Chat is subscriber-only. Free users never receive text, voice or avatar Mystic Chat through credits or the Free allowance.**
20. Free AI usage is a promotional daily budget, not purchased currency; it resets daily, does not roll over and is adjustable without an app rebuild.
21. Free usage is protected simultaneously by generation, input-token, output-token and provider-cost ceilings; the first exhausted ceiling stops further free AI generation.
22. Purchased Reading Credits never expire and may be used by Free users for eligible non-chat paid features.
23. Live-avatar allowance is separate from Reading Credits. Premium/Ultra subscription grants may expire at the billing-cycle reset; purchased avatar minutes never expire.
24. Purchased avatar minutes remain owned if a subscription lapses, but cannot be spent until Mystic Chat entitlement becomes active again.
25. Live avatar minutes are charged only while a billable avatar session is actually active. Text-only and voice-only fallback do not consume avatar minutes.
26. If LemonSlice/LiveKit avatar rendering fails, Mysta degrades explicitly to subscriber voice-only/text with **no avatar-minute debit during the failed interval**.
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

# 24. MYSTIC CHAT / LIVING MYSTA — LOCKED

Mystic Chat is the subscriber-only relationship surface and must feel like interaction with Mysta, not a generic support chatbot.

Access contract:

```text
Free                         DENIED
Premium ACTIVE/GRACE         ALLOWED
Ultra ACTIVE/GRACE           ALLOWED
expired/cancelled-after-end  DENIED
Reading Credits              NEVER unlock Mystic Chat
```

A Free user selecting Mystic is routed to the subscription explanation/paywall; Free daily AI allowance and Reading Credits cannot bypass this gate.

## 24.1 Launch interaction modes

Every active subscriber can use the same conversation in three presentation modes:

```text
TEXT          Mysta conversation, no avatar-minute debit
VOICE_ONLY    self-hosted Whisper + mysta_voice_v1, no avatar-minute debit
LIVE_MYSTA    voice + real-time LemonSlice Mysta avatar, avatar-minute debit
```

`LIVE_MYSTA` is the default promoted premium experience when the device/network and allowance support it. Text and voice-only remain deliberate accessibility/bandwidth/failure fallbacks, not separate lower-intelligence products.

## 24.2 Living Mysta visual state machine

Approved states:

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

- natural idle breathing, blinking, eye/head micro-movement;
- attentive listening posture;
- restrained reflective motion;
- subtle hand/upper-body gestures while speaking;
- domain-specific safe gestures/states for cards/chart/numbers;
- warm smile/celebratory response for appropriate low-risk moments;
- calm neutral concerned state for safety-sensitive content;
- accurate lip synchronisation to the final `mysta_voice_v1` audio stream.

Forbidden:

- chain-of-thought visualisation;
- manipulative urgency/fear expressions;
- exaggerated distress;
- sexualised motion;
- uncontrolled provider-generated personality drift;
- provider voice substitution;
- avatar actions that imply a tool/calculation succeeded before the authoritative tool result exists.

## 24.3 Conversation UI

Header:

```text
Mysta
state
Live Mysta / Voice / Text mode control
remaining included/purchased avatar time when Live Mysta is selected
conversation actions
```

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

## 24.4 Live voice contract

Voice is first-class at launch.

```text
READY
→ CONNECTING
→ LISTENING
→ TRANSCRIBING
→ UNDERSTANDING
→ TOOL_WORK / REFLECTING
→ VALIDATING
→ SYNTHESISING_VOICE
→ SPEAKING
→ LISTENING
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
AVATAR_UNAVAILABLE_VOICE_FALLBACK
VOICE_UNAVAILABLE_TEXT_FALLBACK
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
5. the delivered response prefix is marked `interrupted=true`;
6. unused future avatar seconds are not charged merely because the response had been generated;
7. the next turn runs through the normal privacy/tool/truth/safety pipeline.

## 24.6 Avatar-minute metering

Internal currency is `AVA_SEC` (integer seconds); the UI displays minutes. Metering starts only after:

```text
entitlement verified
AND avatar provider session accepted
AND avatar video/audio track ready
```

Metering stops at the first of:

```text
user leaves Live Mysta mode
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

When no `AVA_SEC` remains, the active subscriber remains in Mystic Chat and is offered:

```text
continue in Voice Only
continue in Text
buy avatar-minute top-up
wait for next subscription grant
```

## 24.7 Voice/avatar privacy

User microphone frames go only through MystaAI's approved realtime/ASR path. LemonSlice receives the validated Mysta output audio required to render Mysta; it is not given a separate copy of conversation memory or deterministic-domain data unless technically required and explicitly allowlisted.

Production requirements:

- LemonSlice Enterprise ZDR option enabled;
- LiveKit project data region = EU (Frankfurt);
- LiveKit Agent Observability recording/transcript capture disabled;
- no LiveKit Inference;
- no LiveKit Egress/recording for ordinary Mystic sessions;
- raw user microphone data not stored in S3/logs/analytics;
- raw avatar video not retained by MystaAI by default;
- persisted history stores accepted text transcript plus normal evidence/safety metadata;
- provider DPAs/sub-processors/international-transfer assessment completed before launch.

LiveKit media uses global shortest-path routing by default because LiveKit states media transport is transient/not retained and global routing reduces latency. Protocol region pinning remains available on Scale but is enabled only if the legal/data-residency gate requires it; enabling it is recorded in `REALTIME_MEDIA_MANIFEST.json` because it can increase non-European latency.

## 24.8 Avatar failure and fallback

A live-avatar provider failure never ends the conversation unnecessarily:

```text
avatar fails
→ stop AVA_SEC debit
→ show explicit avatar unavailable state
→ preserve same conversation
→ continue Voice Only when speech stack healthy
→ otherwise continue Text
```

No failed avatar minute is charged. Reconnect is bounded and may not create duplicate LemonSlice sessions.

## 24.9 Accessibility

Required:

- captions/transcript always available;
- full Text mode;
- Voice Only mode;
- mute Mysta;
- replay completed Mysta response;
- playback-speed preference within approved range;
- system headset/Bluetooth/audio routing;
- reduced-motion presentation that preserves information;
- avatar is never the sole carrier of meaning;
- no autoplay outside an active user-started voice/live session.

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
- presenting Reading Credits as a way to unlock subscriber-only Mystic Chat.

Launch plans:

```text
Free
Premium — USD $7.99/month base price
Ultra   — USD $21.99/month base price
```

No Starter, Mystic Unlimited or annual subscription launches in v2.5.

Free-exhaustion surface states clearly:

```text
Your free AI insight allowance is used for today.
Come back after the daily reset,
use/buy Reading Credits for eligible readings,
or subscribe to Premium/Ultra.
```

Subscriber Live Mysta exhaustion states clearly:

```text
Live Mysta time is used for this billing period.
Continue with Voice Only or Text,
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

Production relational engine at the v2.5 lock date:

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

BullMQ/Redis is not the authoritative durable production job queue in v2.5. SQS Standard is at-least-once; every consumer is idempotent and every durable queue has retry, visibility timeout, DLQ, age/depth alarms and replay runbook.

No DynamoDB table is required for the v2.5 launch architecture. The routing abstraction remains isolated in `packages/routing`; if a later approved scale revision activates multiple independent cells, the backing routing store may be changed without changing user-facing contracts.

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
expo-audio
```

Real in-app purchase testing uses EAS development builds, not Expo Go.

LiveKit requires native WebRTC code and is therefore also tested only in EAS development/production builds, never Expo Go. Phase 1 registry-verifies and exact-locks the current stable compatible versions of:

```text
@livekit/react-native
@livekit/react-native-expo-plugin
@livekit/react-native-webrtc
@config-plugins/react-native-webrtc
livekit-client
```

Web uses exact-locked `livekit-client` plus the current stable compatible LiveKit React components package where used.

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
paid model           gpt-5.6-terra
free-budget model    gpt-5.6-luna
```

The official `openai` TypeScript/JavaScript package is the only production OpenAI client dependency and only `packages/ai-gateway` may import it. Version `7.23.0` was the current official npm release at the v2.5 research lock; Phase 1 re-verifies availability/non-yanked/security status and freezes the exact dependency. Any substitution uses controlled dependency evidence.

`gpt-5-mini-2025-08-07` is explicitly prohibited for the launch implementation because OpenAI's current deprecation register schedules it for removal on **11 December 2026** and names `gpt-5.6-terra` as the recommended replacement. `gpt-5.6-luna` is the current cost-sensitive high-volume member of the same family and is used only for the locked Free budget.

## 35.16 Self-hosted voice runtime — locked

Production speech services do **not** call OpenAI Audio, ElevenLabs or another metered speech API.

Text-to-speech model:

```text
model                    hexgrad/Kokoro-82M
release                  v1.0
repository revision      f3ff357 (Phase 0 re-verifies/full revision is frozen)
model SHA-256            496dba118d1a58f5f3db2efc88dbdc216e0483fc89fe6e47ee1f2c53f18ad1e4
licence                  Apache-2.0
production voice         mysta_voice_v1
runtime                  self-hosted Python service
external TTS API         prohibited in normal production path
output                    24 kHz mono PCM internally, streamed to clients as Opus
```

For voice-capacity benchmarking, Phase 0 first resolves the current stable non-prerelease `kokoro`/`misaki` runtime versions compatible with Kokoro v1.0, freezes them in `tools/preflight/voice-capacity/VOICE_BENCHMARK_LOCK.json`, and builds the immutable benchmark image. Phase 1 copies those exact already-benchmarked versions into the production Python lockfile and `DEPENDENCY_MANIFEST.json`; it may not silently choose different voice-runtime versions. If the current runtime is not Python-3.14 compatible, Phase 1 uses a dedicated voice-service Python runtime version supported by the packages and records that exact container runtime; the rest of MystaAI remains on the locked Python runtime. No silent substitution is allowed.

Speech-to-text model:

```text
source model             openai/whisper-large-v3-turbo
source revision          7b51950f41f72ba7b9f619317023ebe15acc1900 (Phase 0 re-verifies)
licence                  MIT
runtime                  faster-whisper 1.2.1
format                   CTranslate2 FP16 generated from the pinned source snapshot
external STT API         prohibited in normal production path
input                     16 kHz mono speech after decode/resample
```

The faster-whisper/CTranslate2 package versions and converter toolchain used by the benchmark are compatibility-verified and frozen in Phase 0 `VOICE_BENCHMARK_LOCK.json`. Phase 1 promotes those exact versions into the production dependency lock. The converted model is hashed and stored as an immutable build artefact; production containers never download model weights from the public internet at runtime.

Voice transport/runtime dependencies:

- `expo-audio` on mobile, installed with `npx expo install` and frozen;
- browser `MediaDevices/getUserMedia` + Web Audio APIs on web;
- authenticated WebSocket transport through the regional ALB;
- `@fastify/websocket` at the exact stable version compatible with the locked Fastify release, frozen in Phase 1;
- Opus codec support in a pinned container image/toolchain;
- server-side VAD/endpointing from the pinned speech runtime;
- no background microphone capture.

## 35.17 Voice compute — production capacity lock

Voice compute runs only in `eu-west-2` and is split into two independently scaled pools because ASR and TTS have materially different compute economics.

### 35.17.1 ASR production rule — GPU mandatory

Production Whisper large-v3-turbo ASR runs on GPU-backed Amazon ECS capacity on EC2. Fargate is not an allowed ASR production target because AWS Fargate does not provide GPU resources.

Phase 0 benchmarks only instance shapes that are actually offered to the production account in `eu-west-2`. Candidate families are:

```text
1. G6f fractional L4, only when the selected slice has sufficient VRAM and passes the full ASR SLO/concurrency test
2. G6 full L4
3. G5 A10G
```

The benchmark may reject a family/size for model-load failure, VRAM pressure, latency, concurrency, driver/runtime incompatibility, AZ coverage or cost. It may not select an unbenchmarked shape.

For every candidate that loads successfully, Phase 0 records:

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

Kokoro-82M is not automatically placed on GPU capacity. Phase 0 benchmarks CPU inference first because MystaAI must not pay for accelerator capacity that is not required.

CPU candidates are the following x86-64 Linux ECS/Fargate task shapes, tested in this exact order after Phase 0 reconfirms that AWS still supports the CPU/memory pair:

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

Phase 0 queries actual instance offerings by Availability Zone and selects two distinct `eu-west-2` Availability Zones that both offer the chosen ASR GPU instance type. The manifest stores both account-local AZ names and stable AZ IDs.

Production ASR minimum floor:

```text
minimum selected ASR GPU EC2 instances   2
minimum Availability Zones               2
minimum instances per selected AZ        1
Spot instances in minimum floor           prohibited
```

Before production voice is enabled, MystaAI creates an **On-Demand Capacity Reservation** for the minimum ASR GPU instance in each selected AZ. These reservations are part of the production floor and count toward the applicable On-Demand G/VT vCPU quota.

Additional autoscaling capacity may use ordinary On-Demand capacity. Spot capacity is prohibited from the v2.5 voice launch design so rated capacity does not depend on interruptible instances.

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

Static price numbers are deliberately not hard-coded into this long-lived specification. Phase 0 obtains the effective production prices directly from the AWS Price List Query API for `eu-west-2` and freezes the returned price evidence in the manifest.

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

The Phase-0 benchmark harness is non-production tooling under:

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
250 sessions waiting on Mystic/LLM/tools
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

The current official LemonSlice pricing page states Enterprise includes Actions, Emotions, 1000+ concurrency, Lite/Pro/Flash access, 24-hour calls, Zero Data Retention and data-residency options, with advertised scale pricing as low as $0.039/min. MystaAI does **not** assume the advertised minimum; Phase 0 requires the actual signed rate and billing quantum.

Realtime media:

```text
provider                 LiveKit Cloud
launch plan              Scale
purpose                  WebRTC transport/video-track distribution only
project data region      European Union (Frankfurt)
LiveKit Inference         prohibited
Agent Observability       disabled in production Mystic sessions
ordinary recording       prohibited
region pinning            OFF by default; enable only when legal gate requires it
```

LiveKit Scale is selected because current official terms provide up to 5,000 concurrent connections and region-pinning capability. At the rated 1,000 Live Mysta sessions, capacity planning assumes at least a user participant and avatar participant per session; the 5,000-connection plan therefore preserves material transport headroom. The actual account limit is frozen in `REALTIME_MEDIA_MANIFEST.json`.

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

Realtime Live Mysta adds two external presentation/control dependencies without moving Mysta intelligence out of the authoritative data plane:

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

The launch backend is authoritative in `eu-west-2`. No Global Accelerator, DynamoDB Global Tables, cross-Region database replication or second authoritative Region is required for v2.5 launch.

## 37.3 Routing abstraction

`packages/routing` exposes one contract for resolving `user_id → home_region/cell_id/routing_version`. The v2.5 implementation uses Aurora. Launch values are `home_region=eu-west-2` and `cell_id=cell-001` unless an approved later specification revision activates additional cells.

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

## 47.5 Human psychology sourcing — v2.5 hard lock

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

Mystic and dream text, including accepted transcripts created from voice input, passes through:

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

The speech-recognition service returns text plus bounded technical metadata only. The accepted text transcript then follows the normal Mystic privacy pipeline before any OpenAI request.

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
- `mysta-fast`: subscriber Mystic Chat, Reading-Credit-backed reading, ordinary paid/subscriber interpretation;
- `mysta-deep`: complex cross-system work, long compatibility, premium reports and difficult multi-domain synthesis.

Free users can never cause `mysta-free` or any other alias to enter `/mystic` routes.

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

Official current pricing at the v2.5 research lock:

```text
gpt-5.6-terra  input $2.00 / 1M   cached $0.20 / 1M   output $12.00 / 1M
gpt-5.6-luna   input $0.20 / 1M   cached $0.02 / 1M   output $1.20 / 1M
```

These prices are evidence inputs, not a promise of future pricing. Phase 0 writes the current values and retrieval timestamp into `MODEL_MANIFEST.json` and recomputes every cost guard before Phase 1.

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
AVA_SEC    integer Live Mysta avatar seconds
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
/readings /mystic /reports
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

## 64.4 Mystic entitlement gate

Every `/mystic` HTTP/WebSocket token/session creation requires active `premium` or `ultra` entitlement. Free allowance and READ balance are irrelevant to this check.

## 64.5 Voice session

Authenticated realtime session creation is server-authorised. The existing voice protocol remains `mysta-voice-v1`; user audio enters self-hosted Whisper and validated assistant text enters self-hosted Kokoro.

## 64.6 Live Mysta avatar session

Create:

```text
POST /api/v1/avatar/session
```

Preconditions:

```text
active Premium/Ultra entitlement
LIVE_MYSTA feature enabled
AVA_SEC > 0
no concurrent active Live Mysta session for user
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

## 66.1 Free — $0

Free includes the product's deterministic/basic non-chat experiences plus the locked tiny daily AI insight allowance. It includes **no Mystic Chat of any kind**.

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

Free users may buy Reading Credits and one-off reports. Reading Credits do not unlock Mystic Chat.

## 66.2 Premium — USD $7.99/month

Includes:

- full subscriber core feature entitlement;
- Mystic Chat by text;
- Voice Only Mystic Chat;
- Live Mysta avatar mode;
- **25 Live Mysta avatar minutes (1,500 `AVA_SEC`) per paid billing cycle**;
- full history/pattern experience;
- normal-human-use fair-use/abuse protection rather than a marketed Mystic message quota.

The 1,500 included `AVA_SEC` expires at the end of its paid billing cycle and does not roll over.

## 66.3 Ultra — USD $21.99/month

Ultra has the same launch feature capability as Premium and is the high-avatar-usage subscription:

- all Premium capabilities;
- **69 Live Mysta avatar minutes (4,140 `AVA_SEC`) per paid billing cycle**.

The 4,140 included `AVA_SEC` expires at billing-cycle end and does not roll over.

There is no artificial feature degradation of Premium merely to force Ultra; the launch differentiator is the materially larger included Live Mysta allowance.

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

The 25/69 minute grants are fixed launch customer promises. They are **not** silently reduced because a vendor/store price changes. Phase 0 must instead prove positive full-use channel contribution margin or block launch for an owner-approved commercial revision.

## 66.5 No annual/trial launch products

v2.5 launches monthly Premium/Ultra only. Annual plans/free trials require a later owner-approved pricing revision and complete store/margin retest.

## 66.6 Subscriber avatar top-ups

Available only while Premium/Ultra entitlement is active:

```text
Avatar Small     £10 UK base price     +1,800 AVA_SEC (30 minutes)
Avatar Large     £20 UK base price     +4,500 AVA_SEC (75 minutes)
```

Purchased `AVA_SEC` never expires. If the subscription later expires, the purchased balance remains but cannot be spent until the user again has active Mystic Chat entitlement.

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

Purchased READ never expires. There is no conversion between READ and AVA_SEC. READ cannot unlock `/mystic`.

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
mystic_chat
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
- current RevenueCat commercial cost recorded in `COST_ENVELOPE_MANIFEST.json` (official current Pro terms are free to $2,500 MTR, then 1% of tracked revenue; Phase 0 re-verifies).

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
mystic_text_started mystic_voice_started live_mysta_started live_mysta_ended
avatar_allowance_low avatar_allowance_exhausted avatar_topup_purchased avatar_fallback_used
report_purchased report_completed
```

Allowed commercial properties use coarse identifiers/amounts such as tier, pack/product ID, currency code and metered seconds. Prohibited analytics payloads include raw dream/chat/transcript/audio, precise birth details, legal name, private narrative or sensitive medical/legal text.

Analytics must support conversion funnels:

```text
Free allowance use → exhaustion → READ purchase / subscription / next-day return
Free → Premium/Ultra
Premium → avatar top-up / Ultra
Live Mysta start → successful avatar ready → completed minutes → fallback/error
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

Admin never provides casual unrestricted browsing of private reading/dream/chat content. High-risk access requires break-glass audit.

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

MystaAI does not use private reading/chat/dream content as an internal training dataset by default.

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

MystaAI retains the commercial objective of capturing a material share of the global paying spiritual-app market, while v2.5 deliberately avoids paying for 20–30M-user infrastructure before demand exists. The launch architecture is required to serve:

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
live Mysta avatar sessions                 1,000 concurrent rated test
concurrent active voice sessions           1,000 engineering acceptance baseline
voice session start burst                     200 sessions/s
async work ingress                        3,000 jobs/s burst envelope
```

These are engineering acceptance targets. Phase 0/94 may increase them after a documented workload model; they may not be reduced merely to make testing easier.

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

Within v2.5, capacity increase means one or more of:

- additional ECS tasks;
- larger/more Aurora instances/readers;
- RDS Proxy capacity tuning;
- larger/sharded ElastiCache where justified;
- more SQS consumers;
- higher AWS/OpenAI quotas;
- more voice-inference accelerator instances/tasks;
- ALB/CloudFront capacity planning.

A second cell or Region is not a v2.5 launch requirement. It becomes a controlled future scale revision only if measured demand requires it.

## 85.5 Global availability from one Region

Web and static assets are delivered globally through CloudFront. Dynamic MystaAI requests terminate in `eu-west-2`. This intentionally trades some distant-user latency for a materially simpler, faster and cheaper launch architecture.

No claim is made that a single Region eliminates regional-outage risk. A full `eu-west-2` outage can make dynamic MystaAI services unavailable until AWS recovers or the documented restore process is executed. That is an accepted v2.5 launch tradeoff.

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

Production database at the v2.5 lock date:

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
- no cross-Region replication is a v2.5 launch dependency.

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

Cross-Region replication is not required for v2.5 launch.

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

No active-active, active-passive cross-Region failover or Aurora Global Database is required for v2.5. A full Region outage remains an accepted availability risk at launch.

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
Mystic first-token p95                      <6s while OpenAI healthy
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
live visual render                    → LemonSlice Enterprise only while LIVE_MYSTA active
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

Interactive subscriber Mystic traffic is prioritised over asynchronous reports during OpenAI saturation. Free traffic is deprioritised and is the first AI workload shed when provider quota/cost protection requires it.

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

Mystic subscriber abuse protection:

```text
concurrent Mystic sessions          1 / user
voice/live session starts           10 / 10 min / user
continuous voice/avatar session     30 min before transparent renewal boundary
single user speech turn             90s
interactive model requests          20 / minute / user burst ceiling
```

These are anti-automation/infrastructure controls, not marketed Mystic message quotas. Normal human use is not charged by message count.

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

Scale proof is based on the full single-region v2.5 architecture and the Section 85 workload envelope. The launch is not blocked on multi-cell or multi-Region testing because those systems are not part of v2.5 production.

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
- `voice_mix_v1` is used exactly: 400 ASR + 250 TTS + 250 Mystic/tool-wait + 100 idle/transition sessions at 1,000 concurrent sessions;
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

Future multi-cell/multi-Region tests are not part of v2.5 and must not appear as launch blockers. If a future specification activates them, it must add its own acceptance programme.

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
EXISTING MYSTIC CONVERSATION + TOOLS
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
- only a final accepted transcript enters Mystic conversation;
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
- denied permission leaves full text Mystic functionality intact;
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
      → existing Mystic/API services
      → private TTS service
           → CPU ECS/Fargate by default
           → GPU ECS/EC2 only when Phase-0 benchmark selects GPU_TTS
```

ASR and TTS scale independently. ASR is GPU-backed. Kokoro TTS is CPU-first and is not allowed to consume GPU merely because a GPU path exists. Exact compute targets, AZs, quotas, reservations, throughput and prices come only from the locked Section 35.17 Phase-0 evidence.

Accelerator instances are not pre-provisioned for the theoretical 3M account count. The minimum HA floor is reserved; capacity above that floor scales against measured concurrent voice use and the fixed `voice_mix_v1` acceptance envelope.

No public model downloads occur in production. Models are downloaded/verified in the build pipeline, baked into immutable images or approved immutable model artefacts, and deployed from MystaAI-controlled registries/storage.

## 106.10 Failure behaviour

```text
microphone denied             → text mode remains available
voice gateway unavailable     → text fallback
ASR unavailable               → keep audio transient, fail turn, ask user to type/retry; do not fabricate transcript
ASR ambiguous                 → transcript confirmation/retry
Mystic/OpenAI unavailable     → existing provider-unavailable behaviour; no TTS fabrication
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
- ASR word-error benchmark meets the threshold frozen during Phase 0/voice calibration;
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

Rated design:

```text
Live Mysta sessions                         1,000 concurrent
minimum contracted LemonSlice headroom      1,429 sessions
estimated LiveKit participants at 1,000     <=2,500 including control/worker overhead
LiveKit Scale current account connection cap must be >=5,000
```

Actual account values are frozen in manifests and tested.

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
rated_sessions=1000
contracted_sessions
contract_headroom_percent
LiveKit_connection_limit
estimated_participants_per_session
provider_p95_start_latency
provider_failure_rate
provider_rate
billing_quantum
LiveKit_fixed_monthly_price
LiveKit_participant_minute_price
LiveKit_data_transfer_price
full-use Premium/Ultra variable-cost model by channel
load-test evidence
```

The rated acceptance test runs 1,000 concurrent avatar sessions against a production-equivalent environment under an agreed provider load-test window; it must not violate provider terms or create uncontrolled spend.

## 107.9 Failure acceptance

Test provider timeout, join failure, media loss, LiveKit disconnect, duplicate webhook/usage event, stale room token, entitlement loss, AVA exhaustion and service recovery. Required result is explicit voice/text fallback, no double debit and no lost conversation state.

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

# 108. COMPLETE BUILD PLAN — IMPLEMENTATION ORDER

The following order is locked. A later phase may not be declared complete by bypassing an earlier acceptance gate on which it depends.

## Phase 0 — v2.5 design-control / live verification and commercial contract gate

No production application code.

Complete and archive evidence for:

1. OpenAI Terra/Luna model/deprecation/pricing/quota verification.
2. All v2.5 existing dependency/security/knowledge/store preflight rules.
3. Voice benchmark lock and AWS ASR/TTS capacity/economics from v2.5 Sections 35.16–35.17.
4. LemonSlice Enterprise signed terms satisfying BYO voice/LLM, ZDR, Actions/Emotions, >=1,429 concurrency and <=$0.16 effective renderer minute.
5. LemonSlice exact billing quantum, usage/reconciliation mechanism, DPA/subprocessor/data-residency evidence.
6. LiveKit Scale production account/project created with EU (Frankfurt) project data region; actual connection limit >=5,000; Inference disabled; production observability recording disabled; DPA reviewed.
7. Apple/Google exact product/billing rules and account-specific fee/program status.
8. RevenueCat current pricing/webhook behaviour; Stripe current API/fees.
9. Complete `MONETIZATION_MANIFEST`, `FREE_ALLOWANCE_MANIFEST`, `AVATAR_PROVIDER_MANIFEST`, `AVATAR_CAPACITY_MANIFEST`, `REALTIME_MEDIA_MANIFEST` and `COST_ENVELOPE_MANIFEST` schemas/data.
10. Channel-aware full-use margin check for $7.99 Premium/25 min and $21.99 Ultra/69 min plus every READ/AVA/report consumable.
11. Store product/base-country/base-currency plan with actual sandbox IDs.
12. Legal/privacy transfer/DPA review for OpenAI, LiveKit, LemonSlice and existing providers.

Acceptance:

- every applicable Phase-0 manifest production field concrete and evidenced;
- no deprecated model scheduled to disappear before launch;
- avatar contract/capacity/rate gates pass;
- Free daily worst-case provider cost <=$0.001 at current locked settings;
- all sold products have positive variable contribution margin at full allowance/consumption under the actual launch-channel fee assumptions;
- signed Phase-0 evidence index committed under `docs/operations/evidence/phase-0-v2.5/`.

## Phase 1 — External preflight and dependency freeze

Actions:

- verify all core versions;
- verify that the Phase-0 `VOICE_BENCHMARK_LOCK.json` remains valid and promote its exact Kokoro/Misaki/faster-whisper/CTranslate2/container versions into production dependency locks; no version used in the accepted benchmark may change without rerunning Section 35.17;
- verify/freeze Opus tooling and `@fastify/websocket`;
- fetch voice model source snapshots by pinned revision, verify licences/hashes and create immutable internal build artefacts;
- verify Next.js security patch line;
- verify Expo stable/beta status;
- verify Prisma release status;
- verify OpenAI GPT-5.6 Terra model/pricing/API;
- verify Swiss release/licence;
- verify RevenueCat/Stripe rules;
- verify app-store rules;
- verify OpenAI granted rate limits/tier and capacity increase route;
- verify Aurora PostgreSQL/pgvector/RDS Proxy support;
- verify SQS/CloudFront/ALB/ECS/Fargate/Aurora/ElastiCache quotas for `eu-west-2`;
- verify psychology source policies/licences;
- freeze exact package-manager/Turborepo/Fastify-plugin/web-SDK versions;
- populate `DEPENDENCY_MANIFEST.json`.

Also exact-lock LiveKit web/RN/agent packages and `@livekit/agents-plugin-lemonslice`; promote the Phase-0 benchmarked voice runtime versions unchanged.

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
- record the locked Mysta authorship/provenance statement and the mother's confirmed consent record described in Section 7.5;
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

Create schema/migrations including psychology evidence tables and the authoritative `user_routing` table (`home_region`, `cell_id`, `routing_version`, `routing_status`). No cell-migration job system is required in v2.5.

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

Implement the v2.5 psychology evidence layer from Sections 46–50.

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

## Phase 39 — OpenAI GPT-5.6 Terra/Luna gateway

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

## Phase 52 — Subscriber-only Mystic conversation

Implement persistent conversation and tool use with bounded psychology evidence retrieval, provenance and non-diagnostic safeguards.

Acceptance:

- cannot bypass tool authority;
- cannot invent tool success;
- conversation API is modality-neutral so accepted voice transcripts enter the identical tool/privacy/safety path as typed turns.

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

## Phase 63 — Mystic Chat shell / subscriber gate / mode switching

Build original Mysta interaction room and text states, plus the voice-mode entry/control surfaces and state placeholders defined by Section 24.1. Full speech services are integrated in the dedicated voice phases after full mobile integration.

## Phase 64 — Cross-System UI

Build signature evidence-separated combined reading.

## Phase 65 — History/Patterns UI

Build timeline and domain pattern views.

## Phase 66 — Monetization, Free allowance and immutable wallet data model

Implement `free_ai_daily_usage`, atomic reservations, `wallet_ledger`, READ/AVA_SEC grants/spends/expiry/reversal, commercial config versioning and all Section 66–74 invariants.

Acceptance:
- concurrent spend cannot overspend;
- purchased credits never expire;
- included AVA expires only at billing reset;
- Free budget cannot enter Mystic routes.

Implement plans, entitlements, purchase state, credit ledger and reconciliation records.

## Phase 67 — Stripe web subscriptions / READ / AVA / reports

Configure Premium $7.99, Ultra $21.99, READ packs, active-subscriber AVA top-ups and reports using server-defined price IDs/webhooks.

Configure products/prices/webhooks/portal and one-off purchases.

Acceptance:

- test-mode full lifecycle.

## Phase 68 — RevenueCat / Apple / Google subscriptions and consumables

Create Premium/Ultra monthly subscriptions, READ consumables, AVA consumables and report products in store sandboxes. Map through RevenueCat. Use store-returned localized price strings and provider webhooks to grant internal ledger units.

Configure products, offerings, entitlements, webhook auth/HMAC and restore flow.

Acceptance:

- physical-device sandbox purchase;
- renewal;
- cancellation;
- billing issue;
- restore;
- refund/revocation path.

## Phase 69 — Entitlement, Free allowance, READ and AVA enforcement

Enforce subscriber-only `/mystic`; Free daily cost/token/generation gate; READ for eligible non-chat pay-as-you-go; AVA spend only for active subscriber Live Mysta sessions.

Enforce the exact v2.5 Free/Premium/Ultra entitlement matrix server-side; no legacy Starter or Mystic Unlimited entitlement may grant access.

## Phase 70 — Free exhaustion / paywall / Reading Credits / avatar top-up UI

Build exact Free exhaustion and subscriber avatar exhaustion choices from Sections 28 and 66. No dark patterns.

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
- fonts/assets;
- `expo-audio` microphone/playback foundation with background recording disabled.

## Phase 82 — Full mobile integration

Implement all customer journeys on iOS/Android.

## Phase 83 — Mysta synthetic voice creation and asset freeze

Actions:

- vendor/verify the pinned Kokoro model artefact and included voice assets;
- implement the internal non-cloning voice-design tool described in Section 106.2;
- generate the locked candidate set against the voice evaluation corpus;
- run technical pronunciation/audio checks;
- obtain owner approval of the final Mysta voice;
- freeze `mysta_voice_v1.pt`, reference WAV, evaluation JSON and SHA-256 in `VOICE_MANIFEST.json`;
- prove no identifiable-person reference recording was used.

Acceptance:

- `mysta_voice_v1` approved;
- voice hash reproducibly loads in the pinned Kokoro runtime;
- rights/provenance manifest complete;
- no generic or temporary production voice remains.

## Phase 84 — Self-hosted speech recognition service

Build `apps/voice-inference` ASR path using the pinned Whisper large-v3-turbo snapshot and faster-whisper/CTranslate2 conversion artefact.

Implement:

- model preload/hash verification;
- Opus decode/resample;
- VAD/endpointing;
- partial/final transcript events;
- confidence/confirmation policy calibrated to the evaluation corpus;
- cancellation/timeouts/backpressure;
- zero raw-audio persistence;
- metrics and health endpoints.

Acceptance:

- locked ASR corpus passes accuracy threshold;
- Section 91 transcript-latency target passes at declared per-instance concurrency;
- malformed/empty/noisy audio follows explicit failure/confirmation paths;
- container runs without public model download.

## Phase 85 — Self-hosted Mysta speech synthesis service

Build Kokoro production inference with `mysta_voice_v1`.

Implement:

- immutable model/voice hash verification at boot;
- approved speech-mode text segmentation/pacing;
- 24 kHz mono generation;
- streaming chunk encoder;
- cancellation on interruption;
- bounded queue/backpressure;
- metrics and health endpoints.

Acceptance:

- domain pronunciation suite passes;
- Mysta identity remains consistent across the full corpus;
- Section 91 first-audio target passes at declared per-instance concurrency;
- no external TTS credential/call exists.

## Phase 86 — Voice gateway and real-time protocol

Build `apps/voice-gateway` and `mysta-voice-v1` protocol.

Implement:

- authenticated WebSocket upgrade;
- entitlement validation;
- one active voice session/user;
- binary audio/control frame sequencing;
- ASR routing;
- accepted transcript → existing Mystic conversation orchestration;
- validated text → TTS routing;
- ordered audio streaming;
- reconnect grace;
- barge-in/cancellation;
- text fallback;
- audit/observability without audio/transcript leakage.

Acceptance:

- complete protocol conformance suite passes;
- stale/out-of-order frames cannot produce speech;
- interrupted output stops/cancels correctly;
- no direct client access to inference services.

## Phase 87 — Voice UX integration — web/iOS/Android

Implement the complete Section 24.1 experience.

Required:

- on-demand microphone permission;
- live Mysta states;
- transcript/captions;
- audio-level indicator;
- start/end/mute/replay controls;
- Bluetooth/headset/audio-focus behaviour;
- foreground-only recording;
- interruption/barge-in;
- reconnect state;
- transcript confirmation state;
- voice-unavailable text fallback;
- accessibility labels and keyboard/screen-reader support where applicable.

Acceptance:

- physical iOS and Android devices pass;
- supported web browsers pass;
- denied microphone permission never breaks text chat;
- raw audio is absent from persisted storage/logs.

## Phase 88 — Voice quality, privacy, resilience and scale gate

Execute Section 106 and Section 101 voice acceptance.

Required evidence:

- approved voice identity/provenance;
- ASR accuracy benchmark;
- TTS pronunciation/audio-quality benchmark;
- 1,000-concurrent voice-session test;
- 200-session/s connection burst;
- Section 91 latency SLOs;
- barge-in p95;
- reconnect/packet-loss/backpressure tests;
- task/AZ-loss recovery;
- ASR minimum floor is backed by active Capacity Reservations in both selected AZs;
- autoscaling evidence including scale-out/scale-in thresholds and cold-scale timing;
- `voice_mix_v1`, ASR spike and TTS spike all pass;
- current production-account quota evidence shows >=30% headroom at rated voice capacity;
- cost evidence reconciles benchmark instance/task-hours to `VOICE_CAPACITY_MANIFEST.json`;
- `VOICE_CAPACITY_MANIFEST.json` complete with no applicable null/TBD fields;
- no raw audio retention;
- no external speech API billing credential/call in production build.

No later acceptance phase may waive this gate.

## Phase 89 — LemonSlice Enterprise provider freeze

Promote Phase-0 contract values into production manifests/secrets/configuration; implement server-side provider client boundary and health/capacity admission checks.

Acceptance:
- no client provider secret;
- BYO LLM/voice path only;
- ZDR configuration evidenced;
- contract rate/concurrency alarms wired.

## Phase 90 — Mysta avatar production asset

Create/approve `mysta-avatar-source-v1.png`, exact crop(s), asset hash, visual-quality fixture and provider test agent from the locked Mysta character design.

Acceptance:
- owner-approved Mysta identity;
- face/mouth/gesture suitability;
- no character drift or third-party asset contamination.

## Phase 91 — LiveKit realtime media foundation

Create production-equivalent LiveKit project/config, short-lived token service, room/participant permission model, web renderer and Expo development-build integration.

Acceptance:
- EU project-data region evidenced;
- no LiveKit Inference;
- observability recording disabled;
- iOS/Android/web join/leave/reconnect pass;
- only scoped identities/tracks allowed.

## Phase 92 — Avatar gateway / self-managed pipeline

Wire validated `mysta_voice_v1` audio into LemonSlice AvatarSession through LiveKit while preserving MystaAI Whisper/Terra/Kokoro/tool pipeline.

Acceptance:
- LemonSlice never becomes LLM/STT/TTS authority;
- validated text/audio only reaches avatar renderer;
- provider failure preserves conversation.

## Phase 93 — Living Mysta state/action/emotion controller

Implement the Section 24 finite-state mapping and whitelisted avatar affect/action controls.

Acceptance:
- no free-form model animation execution;
- tool-status animation cannot precede tool truth;
- safety-sensitive states remain calm/non-manipulative.

## Phase 94 — Live Mysta UX and AVA_SEC metering

Implement Live Mysta mode on web/iOS/Android, allowance display, server-authoritative provider-session usage segments, included-before-purchased spend order, low-balance warnings and immediate Voice/Text fallback.

Acceptance:
- no debit before avatar-ready;
- no debit after failure/end;
- concurrent/reconnect events cannot double-charge;
- exhausted AVA leaves subscriber text/voice chat usable.

## Phase 95 — Avatar privacy, quality, economics and scale gate

Run privacy/DPA checks, visual/AV-sync acceptance, failure matrix, provider reconciliation tests and production-equivalent rated load up to 1,000 concurrent Live Mysta sessions under a controlled vendor load-test arrangement.

Acceptance:
- Section 91 avatar SLOs pass;
- contracted/provider/LiveKit capacity retains required headroom;
- cost evidence matches manifests;
- ZDR/privacy evidence captured;
- all web/iOS/Android golden recordings approved.

## Phase 96 — Accessibility audit

Audit WCAG 2.2 AA, VoiceOver, TalkBack, keyboard and reduced motion.

## Phase 97 — Originality audit

Review every major screen/art asset against Section 6.

Any copied/distinctively derivative asset/layout is replaced.

## Phase 98 — Knowledge rights audit

Verify source/licence/commercial-use manifest and asset provenance.

## Phase 99 — AI acceptance

Run complete AI evaluation suite and freeze model/prompt/validator manifests.

## Phase 100 — Security hardening

Execute Section 100.

No unresolved launch-blocking vulnerabilities.

## Phase 101 — Performance/load hardening

Execute the complete Section 101 single-region scale programme including voice; populate SCALE/SERVICE_QUOTA/VOICE_CAPACITY manifests and record measured autoscaling/database/provider values. Re-run AWS Price List and applied-quota capture immediately before the rated test; invalidate the run if the deployed compute profile differs from the Phase-0 selected profile without a controlled revision.

## Phase 102 — Financial/billing/allowance/wallet acceptance

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

## Phase 103 — Disaster recovery

Execute Section 90 exactly: perform a real Aurora snapshot/PITR restore into an isolated replacement cluster, validate critical data/invariants, reconnect a staging-equivalent stack and record RTO/RPO evidence. No cross-Region failover is required.

## Phase 104 — Privacy/legal gate

Complete privacy/terms/transfer/licence/character-rights/store-rule gates.

## Phase 105 — App Store preparation

Configure metadata, screenshots, privacy, age rating, subscriptions and review notes.

## Phase 106 — Google Play preparation

Configure equivalent Play assets/products/compliance.

## Phase 107 — Production Terraform

Provision tracked AWS web-edge resources plus the complete single-region `eu-west-2` application/data infrastructure from versioned Terraform modules. Provision exactly one launch cell (`cell-001`) plus the private self-hosted voice gateway/inference compute defined by Section 106. Do not provision Global Accelerator, DynamoDB Global Tables, Aurora Global Database or a second authoritative Region.

## Phase 108 — Production secrets

Provision Secrets Manager/KMS values; verify zero secrets in source/history.

## Phase 109 — Production database

Create the production Aurora PostgreSQL cluster, RDS Proxy, supported pgvector extension, migrations, Aurora `user_routing` table, voice session/turn tables, SQS/DLQs, ElastiCache and approved static seed data in `eu-west-2`.

## Phase 110 — Production knowledge publish

Publish immutable initial knowledge version.

## Phase 111 — Production smoke test

Test real production-path integrations without creating unsupported fake state:

- auth;
- GeoNames;
- Swiss;
- OpenAI (GPT-5.6 Terra);
- self-hosted Whisper ASR on the locked GPU ECS/EC2 profile;
- the two locked ASR On-Demand Capacity Reservations, one in each selected AZ, before voice is enabled;
- self-hosted Kokoro TTS with `mysta_voice_v1` on the locked CPU_FARGATE or GPU_ECS_EC2 profile;
- live voice WebSocket/barge-in path;
- knowledge retrieval;
- report storage;
- email;
- push;
- permitted payment validation.

## Phase 112 — Full acceptance journey

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
→ Mystic text
→ Mystic live voice conversation
→ interrupt Mysta and continue naturally
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

## Phase 113 — Launch

Release:

```text
Web
iOS
Android
```

Monitor continuously during launch.

## Phase 114 — Stabilisation gate

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
voice ASR/TTS latency stable
voice capacity headroom healthy
voice fallback/error rate within target
```

---

# 109. LAUNCH BLOCKERS — HARD LOCK

Launch is additionally blocked by any of:

- GPT-5.6 Terra/Luna model or quota mismatch;
- Mystic Chat accessible to Free users;
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

Whisper ASR runs on the benchmark-selected GPU profile with active two-AZ minimum Capacity Reservations. Kokoro TTS runs on the benchmark-selected CPU-first profile unless the Phase-0 evidence selected GPU. `VOICE_CAPACITY_MANIFEST.json` contains the exact production AZs, compute shapes, passing concurrency, applied quotas, current AWS price evidence, autoscale limits, floor/rated monthly costs and measured cost/session. The rated `voice_mix_v1` and spike tests pass with >=30% quota headroom.

## Reliability

Retries, queues, webhook idempotency, provider outages and rollback paths are tested.

## Infrastructure / single-region scale

Backup restore succeeds. Cell capacity is measured; single-region routing/control plane passes; 3M synthetic paying-account routing/data-volume validation passes; single-cell saturation and failure isolation pass; provider/service/voice-compute capacity headroom is documented for the active Region; adding capacity or a second Region later does not require product redesign.

## Mobile

Physical iOS/Android acceptance passes.

## Web

Supported-browser acceptance passes.

- Free users cannot reach text/voice/avatar Mystic Chat by route/API/WebSocket/credit manipulation;
- Free daily allowance defaults are <=$0.001 provider cost/day and adjustable without app release under the owner ceiling;
- Free user can purchase/use non-expiring READ for eligible non-chat readings;
- Premium $7.99 and Ultra $21.99 monthly products and 25/69 included avatar-minute grants reconcile across web/iOS/Android;
- £10/30m and £20/75m purchased avatar top-ups never expire and cannot be spent without active subscriber entitlement;
- live Mysta avatar is present at launch on web/iOS/Android and passes state/visual/lip-sync/fallback tests;
- 1,000 concurrent Live Mysta rated test passes or launch capacity claim is not accepted;
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
23. MystaAI v2.5 launches as one `eu-west-2` production cell; capacity is increased inside that Region first, and later cell/Region expansion must preserve product contracts.
24. The Aurora writer is allowed to be the v2.5 authoritative transactional writer; its measured capacity may not be exceeded without an approved capacity increase or later cell split.
25. Every user has an authoritative `home_region`, `cell_id` and `routing_version`; v2.5 launch values are `eu-west-2` / `cell-001`.
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
44. Live Mysta is a launch requirement, not a post-launch bootstrap item.
45. Free users never receive Mystic Chat; READ cannot buy access to it.
46. Free AI direct cost is runtime-capped and the cost ceiling outranks nominal token caps.
47. Purchased READ and AVA_SEC never expire.
48. Subscription-included AVA_SEC expires/reset only at the billing-cycle boundary and spends before purchased AVA_SEC.
49. Text/Voice-only subscriber Mystic does not consume AVA_SEC; only provider-ready Live Mysta time does.
50. Store purchase receipts/webhooks grant internal ledger value exactly once; client state and RevenueCat API balances are not the high-throughput spend authority.
51. LemonSlice/LiveKit outage degrades to voice/text without charging failed avatar time.
52. Paid prices/allowances are owner-locked; Free allowance values are operationally adjustable only within the owner hard cost ceiling.

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

---

# 113. FINAL STATUS

**v2.5 status:** final, implementation-locked launch design.

v2.5 preserves the complete production MystaAI system and adds the owner-locked launch changes directly into the core authority: **Living Mysta real-time avatar at launch; subscriber-only Mystic Chat; Premium $7.99/month; Ultra $21.99/month; 25/69 included live-avatar minutes; purchased avatar top-ups; configurable near-zero-cost Free daily AI allowance; Free-user purchasable non-expiring Reading Credits; and current GPT-5.6 Terra/Luna model routing.**

The former post-launch Avatar Bootstrap rule and the separate monetization addendum are superseded. There is no later merge step.

The remaining values that cannot truthfully be known before access to production accounts/contracts—AWS account quotas/current prices, LemonSlice contracted rate/concurrency/billing quantum, LiveKit account limits/current prices, Apple/Google account fee status and OpenAI project quotas—are not left to developer choice. They are mandatory Phase-0 evidence gates with deterministic pass/block rules before Phase 1.

MystaAI is to be built **start to finish**. It is not reduced to an MVP. Difficult features are not silently omitted.

**END OF MYSTAAI FINAL IMPLEMENTATION-LOCKED PRODUCT DESIGN & BUILD SPECIFICATION v2.5**

