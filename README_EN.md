# ♟️ Chinese Chess Pro17 (Xiangqi AI Engine)

![Version](https://img.shields.io/badge/Version-V17_Ultimate-success?style=for-the-badge)
![Tech Stack](https://img.shields.io/badge/Tech-HTML5%20%7C%20Canvas%20%7C%20Web_Workers-blue?style=for-the-badge)
![Concurrency](https://img.shields.io/badge/Concurrency-Lazy_SMP_SharedArrayBuffer-orange?style=for-the-badge)

A high-performance Chinese Chess (Xiangqi) AI engine built purely on the frontend (HTML/JS/CSS). 

This project pushes the physical limits of the browser environment. By combining **Zobrist Hashing, Transposition Tables (TT), a Web Workers cluster**, and a hardcore **Nginx cross-origin isolation + SharedArrayBuffer** setup, it achieves Grandmaster-level opening repertoires and blazing-fast multi-core Lazy SMP concurrency right in the browser.

---

## 🚀 Core Evolution History (Changelog: V10 ➡️ V17)

From a basic single-core search to a lock-free concurrent memory-sharing cluster, here is the evolution of the engine's core:

### [V17 Ultimate] The Final Polish: Bulletproof Locks & Graceful Degradation
- **🛡️ Absolute Concurrency Locks:** Introduced hardware-level state locks (`isAnimating`, `isInterrupted`). Completely eliminated "time-travel" glitches and UI crashes caused by frantic clicking during AI calculation or piece animation.
- **✨ Graceful Degradation:** Added an environment probe. When detached from an Nginx/HTTPS environment (e.g., running locally via a `file:///` double-click), it smartly abandons `SharedArrayBuffer` requests. Using Blob memory injection, it bypasses CORS restrictions and falls back seamlessly to an "Independent Multi-Core Mode" for out-of-the-box playability.

### [V16] Compute Purification: Eradicating Bottlenecks & Lazy SMP Enhancement
- **⚡ 0ms Main-Thread Opening Book:** Shifted the `checkOpeningBook` logic directly to the main thread. Fixed the "wooden barrel effect" where Worker 0's instant opening hit was bottlenecked by Workers 1-5 executing deep searches.
- **🎲 Synergistic Noise Algorithm:** Injected micro-random noise into different cores during Lazy SMP. This forces the search trees of the 6 cores to differentiate, drastically multiplying the pruning efficiency of the shared Transposition Table.
- **🐛 Interruption Interception Rewrite:** Fixed the "Wrongful AI Surrender" bug caused by unresolved Promises returning `null` when a user forces an "Undo" (terminating the threads).

### [V15] Epic Architectural Leap: Physical Shared Memory Pool Unlocked
- **🔥 SharedArrayBuffer Unsealed:** Paired with Nginx's `COOP/COEP` cross-origin isolation headers, successfully broke through browser security seals to activate a massive 104MB physical shared memory pool.
- **🧠 True Lazy SMP Synergy:** Scrapped inefficient static root-node splitting. 6 Web Worker cores now aggressively read/write to the same TT via Lock-free `Int16Array/BigInt64Array` views. Achieved cross-thread "answer-copying" for 0ms pruning, causing NPS (Nodes Per Second) to skyrocket.

### [V14] Opening Repertoire: Full-Spectrum Master Responses
- **📖 State-Pattern Detection:** Refactored the one-way Red opening book to include Black's passive response mechanisms. When the player uses standard openings (Central Cannon, Elephant Opening, etc.), the AI instantly responds within 1ms with battle-tested master repertoires (e.g., Screen Horse Defense), ending the blind waste of compute power in the early game.

### [V13] Cyber Aesthetics & Anti-Deadlock Refactoring
- **🎨 Geek-Tier UI Overhaul:** Redesigned the sidebar into three glassmorphism modules: [Tactical Status], [Command Center], and [System Log].
- **🧹 Memory Leak Eradication:** Fixed zombie Web Worker accumulation and closure memory leaks caused by pending Promises during high-frequency clicks. Introduced a rapid garbage collection queue (`pendingResolves`).

### [V12] Kernel Surgery: Hash Pollution & "Coward Bug" Fix
- **🦠 Lethal Logic Fix:** Hunted down and fixed a deeply hidden reverse Zobrist Hash sequence bug (Unmake Move pollution). Cured the "AI refuses to capture / cowardly pacing" phenomenon caused by TT read errors.
- **⚔️ Bloodlust Rekindled:** Stripped out early pseudo-SEE evaluations that caused misjudgments. Returned to and refactored pure MVV-LVA (Most Valuable Victim - Least Valuable Attacker) static capture sorting. Fixed the Cannon's overvaluation bug.

### [V11] Tactical Engine Foundation Solidified
- **⚖️ Advanced Pawn Structure & Space Eval:** Massively boosted the static evaluation weights for "Rook controlling open files", "Blocked Horses", and "Connected River-crossing Pawns".
- **🔄 Async Engine Upgrade:** Upgraded `autoNext` to a non-blocking `async/await` flow, preventing the browser's main thread from freezing.
- **🪞 Mirrored Coordinate System:** Rewrote underlying mouse mapping and rendering layers to perfectly support real-time "board flipping" without misalignments.

---

## ⚙️ Extreme Performance Deployment Guide (Docker + Nginx)

To unlock the engine's ultimate form (V15+ 104MB Shared Memory) locally, a trusted secure context and specific response headers are strictly required. HomeLab / NAS deployment is highly recommended:

1. **Deploy an Nginx Container**
2. **Configure Core Reverse Proxy Headers in `nginx.conf`**:
   ```nginx
   server {
       listen 80;
       location / {
           root /usr/share/nginx/html;
           index index.html;
           
           # MUST include these two lines to bypass browser multi-core memory locks
           add_header Cross-Origin-Opener-Policy "same-origin" always;
           add_header Cross-Origin-Embedder-Policy "require-corp" always;
       }
   }
