# CyberDrop: Zero-Internet Cross-Platform File Share

CyberDrop is a featherweight, zero-internet offline file and data sharing application engineered to run natively across Windows and Mobile (Android/iOS) while keeping the final application binary within the **10–20MB target range** and supporting automated compilation via **GitHub Actions**. It leverages native WebRTC DataChannels, local Wi-Fi Direct signaling, and Web Bluetooth beacons with an immersive dark cyber-tech radar dashboard, real-time chunked transfer speed meters, multi-file batch queues, pre-accept file previews, offline clipboard synchronization, and persistent transfer history.

***

## User Review & Critical Decisions

> [!IMPORTANT]
> The architectural direction and interactive feature scope have been aligned with your latest constraints:

- **Confirmed 10–20MB Binary Size & Ultra-Lean Architecture**:
  - Zero heavy external dependencies. The core application bundle is ultra-compact (< 1.5MB gzip), avoiding heavyweight runtimes (no bloated 150MB+ Electron containers).
  - Uses native browser & OS primitives: native WebRTC `RTCPeerConnection` DataChannels, native `navigator.bluetooth`, native HTML5 Canvas for cyber radar animation, native `crypto.subtle` for SHA-256 checksums, and native `IndexedDB` for offline history.
  - Native packaging target: PWA standalone container / featherweight WebView2 (Windows) & Android PWA/TWA APK builder compatible with GitHub Actions producing lightweight ~12–18MB binaries.
- **Confirmed GitHub Actions Workflow**:
  - Integrated CI/CD workflow specification (`.github/workflows/build-and-release.yml`) for automated cross-platform compilation, asset bundling, and release artifact generation.
- **Confirmed Discovery & Pairing Mode**:
  - Dual mode combining active cyber radar beacon sweeps (Web Bluetooth & LAN beacon detection) with high-density QR code scanning and 6-digit local pairing codes for instantaneous, zero-internet handshake.
- **Confirmed Visual Aesthetic**:
  - Dark cyber-tech radar dashboard (`#070b12` deep void canvas, `#111a2e` structural surfaces, `#00f2fe` neon cyan radar sweep and `#10b981` emerald transfer signal accents, `#6366f1` indigo accents) with real animated SVG/Canvas pulse waves and radar sweeps.
- **Confirmed Feature Set**:
  1. Multi-file batch queues with pause, resume, and cancel controls.
  2. Instant file preview modal before receiver accepts incoming transfers (supporting images, audio, video, code, and documents).
  3. Direct offline clipboard text and snippet sharing.
  4. Persistent transfer history log stored in local device storage with SHA-256 verification and re-download capability.
  5. Native PWA installation manifest and offline service worker caching for 100% standalone offline operation on Windows and mobile devices.

***

## 1. Overview & Core Concept

- **What It Does**: Enables instant, high-speed transfer of large files, media batches, and text notes directly between nearby devices over local Wi-Fi / hotspot or Bluetooth without uploading a single byte to the internet or external cloud servers.
- **Target Audience / Persona**: Mobile and desktop users in zero-connectivity environments (flights, remote field work, secure offline networks, subways) needing fast, AirDrop-like zero-configuration sharing between dissimilar operating systems (e.g., Windows laptop to Android phone, iPhone to Windows PC).
- **Key Value**: 100% privacy, zero data consumption, minimal disk footprint (10–20MB binary), instant previews, and real-time chunked transfer metrics (MB/s, ETA, progress bar).

***

## 2. User Experience & Visual Design

### Key User Flows

1. **Radar Discovery View**:
   - The user opens CyberDrop and sees their own device profile (e.g. `Win-Desktop-09` or `Pixel-Mobile-4X`) in the center of an interactive circular radar sweep.
   - The radar periodically pings nearby discovered peers (with device icons, signal strength bars, and connection protocol tags: Wi-Fi Direct or Bluetooth).
   - A quick-action bar lets users toggle between **Radar Scan**, **Display My QR Code**, or **Enter 6-Digit Pairing Code**.
2. **File Selection & Batch Staging**:
   - The sender drags & drops files or clicks to select documents, photos, videos, or folders.
   - Staged files appear in an interactive batch queue displaying file sizes, mime icons, and total batch size.
   - Sender clicks on any discovered device or connects via QR code to dispatch the batch.
3. **Pre-Transfer Receiver Verification**:
   - The receiver device displays an incoming transfer modal with the sender's device name, protocol, and itemized file list.
   - Receiver can click **Preview** on any item to inspect full-resolution images, video thumbnails, audio snippets, or text contents before accepting.
   - Receiver clicks **Accept Transfer** or **Decline**.
4. **Active Transfer & Progress Dashboard**:
   - Both sender and receiver watch smooth progress bars with real-time transfer telemetry: chunk progress, current speed in MB/s, elapsed time, ETA countdown, and Pause / Resume / Cancel controls.
5. **Transfer History & Offline Clipboard**:
   - Dedicated history view listing past sent and received items with status badges, timestamps, file sizes, and download/open actions.
   - Dedicated offline clipboard tab allowing rapid copy-pasting of text snippets across devices with one click.

### Visual Identity & Theme

- **Aesthetic Direction**: High-precision cyber-tech radar console. Clean, professional, tactile, and dark without garish purple slop.
- **Color Palette**:
  - *Dominant Canvas (60%)*: Deep void slate `#070b12` and `#0b1120`.
  - *Structural Surfaces (30%)*: Card and panel surfaces `#111a2e` with razor-thin hairline borders (`border-cyan-500/20` and `border-slate-800`).
  - *Accents (10%)*: Neon cyber cyan (`#00f2fe` / `#06b6d4`) for radar sweeps and active signals, vivid emerald (`#10b981`) for completed transfers and connected state, electric amber (`#f59e0b`) for pending/paused states.
- **Typography & Hierarchy**:
  - *Display / Headings*: `Plus Jakarta Sans` / `Outfit` with crisp letter-spacing.
  - *Body Prose*: `Public Sans` / `DM Sans` for high legibility.
  - *Telemetry & Numbers*: `JetBrains Mono` / `tabular-nums` for speeds (`48.2 MB/s`), percentages (`78.4%`), and ETAs (`00:14s`).
- **Interactive Motion**:
  - Concentric radar sweep line rotating with smooth Canvas/SVG animation.
  - Rippling pulse circles that expand upon new peer discovery.
  - Fluid micro-interactions on drag-over, queue reordering, and transfer progress fill.

***

## 3. Key Product Decisions & Trade-Offs

- **Decision 1: Ultra-Lean Minimal Code Strategy (10–20MB Target)**:
  - *Chosen Approach*: Avoid heavy monolithic libraries. Use native web platform capabilities: native HTML5 Canvas for radar rendering, standard Web APIs for file chunking (`Blob.slice`), native `RTCPeerConnection`, and native `crypto.subtle`.
  - *Why*: Guarantees that compilation via GitHub Actions outputs binaries under 20MB while maintaining blistering 60fps performance and instantaneous startup.
- **Decision 2: Direct P2P Offline WebRTC + Local Signaling Fallback**:
  - *Chosen Approach*: WebRTC DataChannels transfer raw ArrayBuffers at native hardware network speeds (hundreds of megabits per second) directly between devices with zero cloud servers.
  - *Why*: Total privacy and true zero-internet functionality.
- **Decision 3: GitHub Actions CI/CD Pipeline**:
  - *Chosen Approach*: Provide a `.github/workflows/build-and-release.yml` template configured to build the optimized production PWA bundle and package lightweight cross-platform artifacts on every push/tag.
  - *Why*: Fulfills the user's specific request for automated GitHub Actions compilation.

***

## 4. Technical Architecture & Data Strategy

```
┌────────────────────────────────────────────────────────────────────────┐
│                        CYBERDROP APPLICATION                           │
│                 (Target Size: 10-20MB Compiled)                        │
│                                                                        │
│  ┌────────────────────────┐  ┌──────────────────────────────────────┐  │
│  │     Top Navigation     │  │          Device Identity             │  │
│  │ [Radar|Send|Clip|Hist] │  │ Device ID, Avatar, LAN/BLE Status    │  │
│  └───────────┬────────────┘  └──────────────────┬───────────────────┘  │
│              │                                  │                      │
│  ┌───────────▼──────────────────────────────────▼───────────────────┐  │
│  │                    Active View Router / Canvas                   │  │
│  │                                                                  │  │
│  │  ┌─────────────────────┐  ┌─────────────────┐ ┌───────────────┐ │  │
│  │  │  Radar Beacon Hub   │  │ Batch Queue &   │ │ Pre-Transfer  │ │  │
│  │  │  • Concentric Sweep │  │ File Staging    │ │ Preview Modal │ │  │
│  │  │  • Peer Ping Nodes  │  │ • Drag & Drop   │ │ • Media thumb │ │  │
│  │  │  • QR / Code Pair   │  │ • Multi-Select  │ │ • Size / Type │ │  │
│  │  └──────────┬──────────┘  └────────┬────────┘ └───────┬───────┘ │  │
│  │             │                      │                  │         │  │
│  │  ┌──────────▼──────────────────────▼──────────────────▼──────┐  │  │
│  │  │          Real-Time Transfer Engine (WebRTC DataChannel)   │  │  │
│  │  │  • 64KB ArrayBuffer Chunking  • Checksum Verification     │  │  │
│  │  │  • Real-time MB/s & ETA       • Pause / Resume / Cancel   │  │  │
│  │  └─────────────────────────────────┬─────────────────────────┘  │  │
│  │                                    │                            │  │
│  │  ┌─────────────────────────────────▼─────────────────────────┐  │  │
│  │  │    Local IndexedDB / Storage: Transfer History & Cache    │  │  │
│  │  └───────────────────────────────────────────────────────────┘  │  │
│  └──────────────────────────────────────────────────────────────────┘  │
└────────────────────────────────────────────────────────────────────────┘
```

### Data Model & State

- **`DevicePeer`**: `{ id: string, name: string, type: 'windows' | 'android' | 'ios' | 'mac', protocol: 'wifi' | 'bluetooth', signalStrength: number, status: 'available' | 'connecting' | 'connected' }`
- **`FileTransferItem`**: `{ id: string, name: string, size: number, type: string, previewUrl?: string, status: 'queued' | 'transferring' | 'paused' | 'completed' | 'declined' | 'failed', progress: number, speedMbps: number, etaSeconds: number, checksum: string, timestamp: number, direction: 'outgoing' | 'incoming', peerName: string, blob?: Blob }`
- **`ClipboardSnippet`**: `{ id: string, text: string, timestamp: number, peerName: string, isSelf: boolean }`

### Verification Plan

1. **Bundle & Binary Footprint Check**: Verify production build size using `compile_applet`, ensuring JS/CSS assets are under 500KB and zero heavy dependencies are added.
2. **GitHub Actions Workflow File**: Verify `.github/workflows/build-and-release.yml` syntax.
3. **PWA & Offline Execution**: Verify manifest and service worker load seamlessly with zero network requests.
4. **Transfer Engine & Interactive Controls**: Verify file batch staging, previews, pause/resume, chunked progress calculations, and history persistence.
