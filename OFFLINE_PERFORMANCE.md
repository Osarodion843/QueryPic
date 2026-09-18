# QueryPic Offline Performance Expectations

QueryPic operates on a **100% offline, air-gapped architecture**. This document outlines the technical performance benchmarks, model requirements, hardware constraints, resource utilization, and guardrails that ensure an optimal user experience without relying on any external cloud or network connectivity.

---

## 🔌 1. Zero-Connectivity Architecture

Since QueryPic does not utilize any cloud-based API endpoints for indexing or search, its offline behavior is identical to its online behavior.

*   **No Network Fallbacks:** If the device goes into airplane mode or completely loses connectivity, search quality, indexing speed, and accuracy remain 100% unaffected.
*   **Zero Latency Overhead from Network:** Network roundtrips are eliminated, meaning performance is purely constrained by local hardware capabilities (CPU/GPU/NPU and RAM).

---

## 📦 2. Model Footprints & Memory Bounds

To run locally on standard consumer mobile devices without crashing or causing OS-level memory termination (OOM), QueryPic utilizes highly optimized, quantized models.

### Target Model Sizes (On-Device Storage)
| Model Function | Base Model | Optimization | Disk Footprint |
| :--- | :--- | :--- | :--- |
| **Multi-Modal/Vision Search** | MobileNetV4 / CLIP-ViT-B | INT8 Quantized | ~45 MB |
| **OCR Text Recognition** | MobileNetV3-OCR / EasyOCR | INT8 Quantized | ~15 MB |
| **Speech-to-Text (STT)** | Whisper-Tiny (English/Multi) | GGUF/CoreML FP16 | ~75 MB |
| **Vector Engine / SQLite** | HNSW/SQLite (Vector extension) | Embedded library | < 5 MB |
| **Total Model Footprint** | — | — | **~140 MB** |

### Memory (RAM) Boundaries
Mobile operating systems (particularly iOS) strictly limit the maximum active RAM a background process or main app can consume before termination.
*   **Search Mode (Foreground):** `< 150 MB` active RAM. Models are loaded on-demand and fully released when the search interface is minimized.
*   **Indexing Mode (Background):** `< 300 MB` active RAM. Background operations are metered to prevent spikes that would trigger OS-level termination.

---

## ⚡ 3. Indexing & Processing Latencies (Offline CPU)

QueryPic does not require a dedicated Neural Processing Unit (NPU), making it fully optimized for standard dual-core and quad-core mobile CPUs.

### Processing Speeds
*   **Photo Indexing:** `200–400 ms` per photo on midrange processors (e.g., Apple A13 Bionic or Snapdragon 778G).
    *   *Throughput:* ~1,200 photos per hour.
*   **Video Frame Extraction:** Extracts and analyzes 1 keyframe every 2 to 3 seconds of video runtime.
    *   *Throughput:* A 60-second video is fully analyzed in `6–8 seconds` on CPU.
*   **Audio Speech-to-Text:** `1.0x–1.5x` real-time speed.
    *   *Throughput:* A 10-minute video audio track takes `6–10 minutes` to fully transcribe.

---

## 🔎 4. Instant Search Latency

Because the index is structured as a local SQLite database containing vector embeddings and raw text indices (FTS5), query matching is extremely fast.

*   **Query Match Latency:** `< 500 ms` across a local database of `20,000` indexed media items.
*   **Scroll & Preview Latency:** 0ms lag during media grid scrolling, as image thumbnails are loaded from local OS cache.

---

## 🔋 5. Thermal, Battery, & Storage Guardrails

Executing machine learning locally on a mobile device requires defensive resource management to protect hardware health and user comfort.

### Battery Guardrail
*   **Background Indexing Pauses:** Automatically halts if the device battery falls below **20%** unless the device is plugged into power.
*   **Charging Optimizations:** High-intensity tasks (video frame analysis and audio transcriptions) are queued and run primarily when the device is **idle, fully charged, and connected to power**.

### Thermal Guardrail
*   **Thermal Monitoring:** The background process polls system thermal status (using iOS `thermalState` / Android `ThermalStatus`).
*   **Dynamic Throttling:**
    *   `Normal / Fair`: Max processing speed.
    *   `Serious`: Throttles background processing to 30% speed (introduces sleep gaps between items).
    *   `Critical`: Suspends all on-device indexing immediately.

### Storage Optimization
*   **Low Storage Buffer:** If device storage is below **1 GB**, QueryPic stops all background indexing and prompts the user to manage their local storage.
*   **Index Size Constraint:** The total index size (database + embeddings) is capped at `< 1.5%` of the raw size of the indexed media (e.g., an index for 10,000 photos typically takes up less than `300 MB`).
