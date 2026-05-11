# PipelineSim Development Plan

## Phase A: Core Infrastructure ✅ COMPLETE

| Module | Status | Description |
|--------|--------|-------------|
| `sim/solver.py` | ✅ | MOC hydraulics + FD temperature coupling |
| `sim/fluid.py` | ✅ | P/T-dependent liquid properties |
| `sim/pipe.py` | ✅ | Pipe geometry, wave speed, elevation |
| `sim/steady.py` | ✅ | Analytical steady-state initialization |
| `sim/network.py` | ✅ | Multi-pipe series network |
| `sim/pump.py` | ✅ | Centrifugal pump with affinity laws |
| `sim/export.py` | ✅ | CSV/HDF5/JSON export |
| `sim/validation.py` | ✅ | Input validation with error messages |
| CLI interface | ✅ | 4 subcommands (analyze/run/water-hammer/network) |
| setup.py | ✅ | pip-installable package |
| Tests | ✅ | 9/9 passing |
| GitHub | ✅ | Repo on main branch, 32 files |

## Phase B: Professional Features (Next Priority)

| Priority | Feature | Description | Effort |
|----------|---------|-------------|--------|
| 🔴 P0 | **Control valve model** | CV curves, Cv vs opening, choke flow | 1-2 days |
| 🔴 P0 | **Valve dynamics** | Quick-closing, relief valve | 1-2 days |
| 🟡 P1 | **Batch simulation** | Parameter sweep runner | 1 day |
| 🟡 P1 | **Input validation** | JSON schema for config files | 0.5 day |
| 🟡 P1 | **Sphinx docs** | API documentation site | 1 day |
| 🟢 P2 | **Transient visualization** | Animated pressure wave propagation | 1 day |
| 🟢 P2 | **Controller models** | PID (pressure/flow/temperature) | 2-3 days |
| 🟢 P2 | **HDF5 export** | Full metadata support | 0.5 day |

## Phase C: Differentiators

| Feature | Description | Impact |
|---------|-------------|--------|
| Leak detection | Transient-based inverse MOC | ⭐⭐⭐ |
| Parameter calibration | Estimate roughness, K from data | ⭐⭐⭐ |
| PyPI release | `pip install pipeline-sim` | ⭐⭐⭐ |
| Real-time dashboard | WebSocket + Plotly/Dash | ⭐⭐ |
| Machine learning surrogate | Fast NN for real-time optimization | ⭐⭐⭐ |

## Commercial Validation Targets

To compete with SPS/OLGA for simple cases:
1. ✅ Water hammer (instant closure, <1% vs Joukowsky)
2. [ ] Pump trip (pump rundown + check valve slam)
3. [ ] Line break / leak detection
4. [ ] Valve sequencing (batch of operations)
5. [ ] Surge analysis report

## Timeline

```
Phase A: Core (DONE) ──────────────────────────────────────✔️
Phase B: Pro   (May 12-18)    ──🔄────────▶ 
Phase C: Diff  (May 19-31)    ────────▶
```

Night Build Complete
- Mon May 11: 22:00 ✓  GitHub repos created
- Mon May 11: 23:00 ✓  9/9 tests, pump, network, CLI, export
- Tue May 12: 09:00 🎯 Summary document ready
