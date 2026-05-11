# Pipeline Simulation Technology Survey & Development Plan

**Author:** Orbit 🛸  
**Date:** 2026-05-11  
**Context:** Research for PipelineSim (liquid pipeline transient simulator)

---

## Part 1: GitHub Ecosystem Survey

### 1.1 Identified Open-Source Projects

#### Category A: Liquid Pipeline (Water Hammer)
| Project | Stars | Lang | Approach | Key Features |
|---------|-------|------|----------|-------------|
| **auralius/waterhammer** | ⭐7 | MATLAB | FD + ODE23 | Optimal control, valve closure optimization |
| **hafmed/water_hammer_simulation** | ⭐3 | C++ | Qt GUI | Multiple numerical methods, visual interface |
| **aaron11669/transient-flow-hydraulics** | 0 | C++ | Unknown | Basic transient hydraulics |

#### Category B: Gas Pipeline (Transient Flow)
| Project | Stars | Lang | Approach | Key Features |
|---------|-------|------|----------|-------------|
| **FSund/transient-pipeline-flow** | ⭐7 | C++ | Implicit matrix | BWRS/Multi EOS, heat transfer, batch tracking |
| **PursyM/gas-pipeline-flow-simulation** | 0 | Python | Jupyter | Basic gas flow with compressibility |

#### Category C: Oil Pipeline (Data/ML)
| Project | Stars | Lang | Description |
|---------|-------|------|-------------|
| **dev-ramesh/Oil-PipleLine-Accidents-Data-Analytics** | ⭐6 | Java | Pipeline accident analysis |
| **biplovbhandari/pipeline-data-agent** | ⭐4 | Python | Data agent for gas/oil pipelines |
| **kaivalyaAole/Oil_Pipeline_Accidents** | ⭐2 | Python | Accident data analysis |

### 1.2 Key Finding: Gap in the Market

**No open-source liquid pipeline transient simulator with Python API exists.**

Existing tools are:
- **auralius/waterhammer**: MATLAB-only, simple physics (no temperature, no P-T coupling)
- **hafmed/Water-Hammer-Simulation**: C++ with Qt GUI, not library-oriented
- **FSund/transient-pipeline-flow**: Gas only, C++ only, no Python binding

→ PipelineSim fills a genuine gap in the open-source ecosystem.

---

## Part 2: Technology Comparison

### 2.1 Numerical Methods

| Method | PipelineSim | FSund (gas) | auralius (water) | SPS/OLGA |
|--------|-------------|-------------|------------------|----------|
| Hydraulics | **MOC** | Implicit FEM | FD + ODE23 | MOC + FD hybrid |
| Energy | Upwind FD | Implicit FD | None | Implicit coupled |
| Coupling | Sequential | Fully coupled | N/A | Fully coupled |

### 2.2 Feature Comparison

| Feature | PipelineSim (now) | PipelineSim (planned) | FSund | SPS |
|---------|-------------------|----------------------|-------|-----|
| Single-phase liquid | ✅ | ✅ | ❌ (gas) | ✅ |
| Temperature coupling | ✅ | ✅ | ✅ | ✅ |
| Multiple BC modes | ✅ | ✅ | ✅ | ✅ |
| CSV export | ✅ | ✅ | ✅ | ✅ |
| Elevation profile | ✅ | ✅ | ❌ | ✅ |
| Multi-pipe network | ❌ | 🔜 | ❌ | ✅ |
| Steady-state init | ❌ | 🔜 | ✅ | ✅ |
| EOS flexibility | ❌ (linear) | 🔜 | ✅ | ✅ |
| Python API | ✅ | ✅ | ❌ (MATLAB) | ❌ |
| Open-source | ✅ | ✅ | ✅ | ❌ |
| Optimal control | ❌ | 📋 | ✅ | ❌ |
| Two-phase flow | ❌ | 📋 | ❌ | ✅ |
| Pump/compressor | ❌ | 📋 | ❌ | ✅ |

### 2.3 Competitive Positioning

```
                    Simple ← → Complex
                         ^
                         |
                  PipelineSim (now)
                         |
  auralius/waterhammer ← → FSund/transient-pipeline-flow
                         |
                  SPS / OLGA (commercial)
                         |
                         v
```

PipelineSim occupies a sweet spot: simpler than SPS, but more complete than academic MATLAB codes. Python API gives it a unique advantage for ML/data science integration.

---

## Part 3: Development Roadmap

### 🎯 Phase A: Core Stability (Current → 1 week)
1. Steady-state initialization (pressure/temperature profile)
2. Improved thermal coupling
3. API stabilization (error handling, input validation)
4. Comprehensive test suite

### 🚀 Phase B: Professional Features (1-3 weeks)
1. **Multi-pipe network** (series/parallel connections)
2. **Component models** (pump curves, control valves)
3. **Boundary conditions from file** (CSV timeseries input)
4. **Rich output** (HDF5 export, JSON summary)

### 💎 Phase C: Differentiator (3-6 weeks)
1. **PINN integration** — Physics-Informed Neural Networks for calibration
2. **Optimal control** — Valve closure optimization (like auralius/waterhammer)
3. **Uncertainty quantification** — Monte Carlo parameter sweeps
4. **Web dashboard** — Real-time pipeline monitoring simulation

### 🌟 Phase D: Production (6-12 weeks)
1. **Two-phase flow** (gas-liquid slug flow) — the holy grail
2. **Real-time simulation** — Cython/C++ acceleration
3. **GPU acceleration** — JAX backend for large networks
4. **Industry validation** — Benchmark against SPS/OLGA

---

## Part 4: Next Immediate Steps

### Priority 1: Steady-State Initialization
**Why:** Without proper initialization, transient simulation starts from a non-physical state (as we saw: 9.78% Joukowsky deviation vs 0.96% when started from a proper state).

**Approach:**
```
1. Solve steady flow: P_out = P_in - ρgΔz - f·L/D·ρ·V²/2
2. Solve steady temperature: dT/dx = (fρV³/(2D) - 4U(T-T_ground)/D) / (ρ·cp·V)
3. Analytical ODE solution for T(x) when possible
```

### Priority 2: Steady-State Thermal Profile
**Why:** Thermal time scale (hours) >> hydraulic time scale (seconds). We need a separate thermal solver.

**Approach:**
```python
def steady_temperature_profile(V, P, T_inlet, T_ground, U, pipe, liquid):
    """Solve steady energy equation analytically"""
    # dT/dx = A - B*(T - T_ground) where:
    # A = fρV³/(2D) / (ρ·cp·V)  (friction heating term)
    # B = 4U/(D) / (ρ·cp·V)      (heat loss term)
    # Solution: T(x) = T_ground + A/B + (T_inlet - T_ground - A/B)·exp(-B·x)
```

### Priority 3: API Polish
- Input validation
- Comprehensive error messages
- Type hints throughout
- Sphinx documentation

---

## Part 5: Architecture Vision

```
PipelineSim Architecture v2.0
═══════════════════════════════════════

┌─────────────────────────────────────┐
│         User API Layer              │
│  PipelineSim(pipe, fluid, config)   │
│  .steady_state() .transient()       │
└─────────────┬───────────────────────┘
              │
┌─────────────▼───────────────────────┐
│         Solver Layer                │
│  ┌──────────┐  ┌──────────────────┐ │
│  │ Hydraulic│  │   Thermal        │ │
│  │ MOC/FD   │←→│ Upwind / Steady  │ │
│  └──────────┘  └──────────────────┘ │
└─────────────┬───────────────────────┘
              │
┌─────────────▼───────────────────────┐
│         Physics Layer               │
│  Fluid(ρ,P,T,μ) Pipe(L,D,e,U,Elev) │
└─────────────┬───────────────────────┘
              │
┌─────────────▼───────────────────────┐
│         Output Layer                │
│  CSV / JSON / HDF5 / Matplotlib     │
└─────────────────────────────────────┘
```

---

*Report generated by Orbit 🛸 | 2026-05-11 21:45 CST*
*Next update: 2026-05-12 09:00 CST*
