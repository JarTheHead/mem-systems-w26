# Cache Coherence Protocol Visualization

An interactive educational web app for visualizing cache coherence protocols in multiprocessor systems.

## Features

- **Three Protocols**: MSI, MESI, and Piranha (directory-based)
- **Animated Message Passing**: Watch coherence messages flow between caches
- **Configurable**: 2-8 processors for MSI/MESI, 2-chip architecture for Piranha
- **Pre-built Workloads**: Common patterns like producer-consumer, ping-pong, false sharing
- **Step-by-step Execution**: Play, pause, step forward/backward through operations

## Quick Start

### Prerequisites

- Node.js 18+
- npm

### Install Dependencies

```bash
npm install
```

### Run Development Server

```bash
npm run dev
```

Then open http://localhost:5173 in your browser.

### Build for Production

```bash
npm run build
```

The built files will be in the `dist/` directory, ready for static hosting (e.g., GitHub Pages).

### Preview Production Build

```bash
npm run preview
```

## Deploy to GitHub Pages

This app is deployed automatically as part of the `mem-systems-w26` site.

**Automatic deployment**: Push to `main` branch triggers the GitHub Actions workflow which:
1. Builds cc-viz with `npm run build`
2. Copies output to `static/cc-viz/`
3. Builds the Zola site (which includes cc-viz)
4. Deploys to gh-pages branch

**Live URL**: `https://khale.github.io/mem-systems-w26/cc-viz/`

**Manual local build**:
```bash
npm run build
# Output is in dist/
```

## Protocols

### MSI (Bus-based Snooping)
- **States**: Modified (M), Shared (S), Invalid (I)
- **Topology**: Shared bus with snooping
- **Use case**: Simple multiprocessor systems

### MESI (Bus-based Snooping)
- **States**: Modified (M), Exclusive (E), Shared (S), Invalid (I)
- **Topology**: Shared bus with snooping
- **Advantage**: Silent upgrade from E→M (no bus traffic)

### Piranha (Directory-based)
- **Architecture**: 2 chips × 2 cores = 4 total cores
- **L1 States**: Invalid, Valid-Clean, Valid-Dirty
- **Features**:
  - L2 cache with directory slices per chip
  - Intra-chip coherence (L1↔L2)
  - Inter-chip coherence (directory messages across ring)
  - Home chip determined by address (addr % numChips)

## Workloads

### General Workloads
- **Basic Read Sharing**: Demonstrates shared state establishment
- **Write Invalidation**: Shows how writes invalidate other copies
- **Ping-Pong**: Cache line bouncing between processors
- **Producer-Consumer**: One writer, multiple readers
- **False Sharing**: Unnecessary invalidations on same cache line
- **Read-Modify-Write**: Common atomic operation pattern
- **MESI Exclusive**: Demonstrates silent E→M upgrade

### Piranha-Specific Workloads
- **Intra-Chip Sharing**: Local L2 traffic only (same chip)
- **Inter-Chip Access**: Directory lookup across chips
- **Cross-Chip Ping-Pong**: Inter-chip invalidation cost
- **Local vs Remote Home**: Compare local vs remote directory access

## Project Structure

```
src/
├── components/          # React UI components
│   ├── Visualization.tsx    # Main visualization canvas
│   ├── PiranhaVisualization.tsx  # Chip-based Piranha view
│   ├── Node.tsx             # CPU/Cache node
│   ├── Memory.tsx           # Main memory display
│   ├── Message.tsx          # Animated message circles
│   └── Controls.tsx         # Playback controls
├── protocols/           # Protocol implementations
│   ├── msi.ts              # MSI state machine
│   ├── mesi.ts             # MESI state machine
│   ├── piranha.ts          # Piranha directory protocol
│   └── types.ts            # Shared type definitions
├── simulation/          # Simulation engine
│   ├── engine.ts           # Event-driven simulation
│   └── workloads.ts        # Pre-defined workloads
├── hooks/               # React hooks
│   ├── useSimulation.ts    # Simulation state management
│   └── useAnimation.ts     # Message animation timing
└── styles/
    └── main.css            # Global styles
```

## License

Educational use for memory systems courses.
