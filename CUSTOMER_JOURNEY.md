# QueryPic Customer Journey Mapping

This document details the step-by-step user experience and touchpoints across the QueryPic product lifecycle.

---

## Lifecycle Phases

```
┌─────────────────┐     ┌─────────────────┐     ┌─────────────────────┐     ┌─────────────────┐     ┌──────────────────────┐
│  1. Onboarding  │ ──► │   2. Initial    │ ──► │ 3. Core Daily Search│ ──► │ 4. Monetization │ ──► │  5. Background Sync  │
│    & Trust      │     │    Indexing     │     │        Loop         │     │    & Upgrades   │     │    & Maintenance     │
└─────────────────┘     └─────────────────┘     └─────────────────────┘     └─────────────────┘     └──────────────────────┘
```

---

### Phase 1: Onboarding & First-Time Setup (Building Trust)

* **Objective:** Establish immediate trust regarding privacy and zero cloud transfer, while giving users granular folder control.
* **Touchpoints & UX Flow:**
  1. **Welcome Screen:** Highlighting the core promise: *"100% On-Device & Air-Gapped Search"*.
  2. **Privacy & Telemetry Consent:** Explicit breakdown of zero-knowledge architecture with optional anonymous crash diagnostics consent.
  3. **Folder & Directory Picker:** User explicitly grants access to specific media folders (e.g., Camera Roll, WhatsApp Downloads, Scans) rather than full-disk indexing.

---

### Phase 2: Initial Indexing (Managing Expectations)

* **Objective:** Ensure background indexing is transparent, unobtrusive, and delivers rapid initial value.
* **Touchpoints & UX Flow:**
  1. **Progress Feedback:** Dynamic home screen banner displaying real-time indexing status (e.g., *"Indexing 1,240 of 5,000 files..."*).
  2. **Resource Guardrail Messaging:** Clear indicator when indexing pauses due to battery level (<20%) or device thermal limits.
  3. **Immediate Utility:** Search is unlocked immediately for already-indexed media while remaining processing continues in background idle mode.
  4. **Starter Prompts:** Dynamic search chips based on indexed content (e.g., *"Try searching for 'Receipt' or 'Dog on grass'"*).

---

### Phase 3: Core Daily Search Loop (The Main Experience)

* **Objective:** Enable sub-500ms discovery across natural language, OCR text, spoken video dialogue, and visual similarity.

#### Scenario A: Quick Search via Home Screen
* **Action:** User enters search bar on launch screen or selects a dynamic filter chip (*"Receipts"*, *"Videos with Speech"*, *"Sunsets"*).
* **Input:** Natural language query like *"Receipt from Home Depot"* or *"Beach sunset from last summer"*.
* **Output:** Instant thumbnail grid in the expanded media workspace with visual match confidence scores.

#### Scenario B: Deep Video & Speech Search (Pro Feature)
* **Action:** User types spoken phrase query like *"Who said happy birthday"*.
* **Output:** Video search results showing timestamped dialogue snippets.
* **Handoff:** Tapping a result launches the OS native video player seeking directly to the exact timestamp.

#### Scenario C: Visual Similarity Search
* **Action:** User selects a reference image and taps *"Find Similar"*.
* **Output:** Media gallery filtered by visual feature vector similarity.

---

### Phase 4: Upgrade & Monetization Touchpoints (Free to Pro)

* **Objective:** Convert engaged free users seamlessly at natural usage milestones.
* **Touchpoints & Triggers:**
  1. **Index Capacity Nudge:** Friendly prompt when approaching or reaching the 10,000 free local file index limit.
  2. **Pro Feature Discovery:** Inline preview when attempting timed video speech search or fuzzy temporal queries, with a 1-tap subscription action.

---

### Phase 5: Long-Term Engagement & Maintenance

* **Objective:** Silent, effortless background maintenance without user overhead.
* **Touchpoints & Flow:**
  1. **Idle Processing:** Automatic incremental background indexing of new media when connected to power and idle.
  2. **Directory Management:** Easy settings toggle to manage permitted folders, recalculate indices, or clear storage cache.
  3. **Continuous Reassurance:** Air-gapped privacy badge confirming 0 bytes uploaded to external servers.
