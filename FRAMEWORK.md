# TPAMI Journal Paper Framework

## Paper Title
**EDISCO: E(2)-Equivariant Diffusion and Partition Networks for Geometric Combinatorial Optimization**

---

## Key Differences from ICLR Conference Paper

| Aspect | ICLR (Conference) | TPAMI (Journal Extension) |
|--------|-------------------|---------------------------|
| **Focus** | TSP with CVRP/ESTP in appendix | TSP + Large-Scale CVRP (200-1000+) as main contribution |
| **CVRP Scale** | 20-100 customers (appendix) | 200-1000+ customers (main section) |
| **Novel Method** | None for large CVRP | E(2)-Equivariant Partition Network |
| **Theory** | Proposition + equivariance proof | Extended analysis on partition efficiency |
| **Experiments** | Standard benchmarks | + Real-world logistics datasets |
| **Page Count** | ~10 pages main + appendix | 14 double-column pages |

---

## Proposed 14-Page Structure

### 1. Introduction (1.5 pages)
- Geometric COPs and E(2) symmetry
- Limitations of existing methods (lack of equivariance)
- **NEW**: Challenge of scaling to large CVRP
- Contributions:
  1. E(2)-equivariant continuous-time diffusion (from ICLR)
  2. **NEW**: E(2)-equivariant partition network for large-scale CVRP
  3. Comprehensive experimental validation

### 2. Related Work (1 page)
- Neural CO solvers (TSP, CVRP)
- Geometric deep learning and equivariance
- Continuous-time diffusion
- **NEW**: Partition-based methods for large-scale routing (GLOP, HGS)

### 3. Preliminary (0.5 pages)
- Problem formulation (TSP, CVRP)
- E(2) equivariance definition
- Continuous-time Markov chains

### 4. Methodology (3 pages)

#### 4.1 E(2)-Equivariant Graph Neural Network
- EGNN architecture with stability mechanisms
- Coordinate updates, message passing
- Feature separation (geometric vs. invariant)

#### 4.2 Continuous-Time Categorical Diffusion
- Forward/reverse process
- Adaptive mixing strategy
- Solver flexibility (PNDM, DEIS, etc.)

#### 4.3 **NEW: E(2)-Equivariant Partition Network for Large-Scale CVRP**
- Motivation: GLOP uses polar angle θ → breaks equivariance
- Our approach: Replace GLOP's GNN with EGNN
- Node features: (demand/capacity, distance_from_depot) - NO polar angle
- Edge features: (distance, affinity)
- Sequential sampling with capacity constraints
- Integration with GLOP's pretrained revisers

### 5. Theoretical Analysis (0.5 pages)
- Proposition on quotient manifold dimension
- E(2) preservation during diffusion
- **NEW**: Partition equivariance preservation

### 6. Experiments (5 pages)

#### 6.1 TSP Experiments (2 pages)
- TSP-50/100/500/1000/10000 results
- Solver comparison
- Generalization and robustness
- TSPLIB real-world instances

#### 6.2 **NEW: Large-Scale CVRP Experiments (2.5 pages)**
- CVRP-200, CVRP-500, CVRP-1000 benchmarks
- Comparison with GLOP, HGS, LKH-3
- Ablation: EGNN vs. GLOP's polar-angle GNN
- Equivariance verification (rotation test)
- Cross-scale generalization

#### 6.3 Euclidean Steiner Tree (0.5 pages)
- Results from ICLR appendix (condensed)

### 7. Ablation Studies and Analysis (1 page)
- Component ablation (EGNN, continuous-time, mixing)
- Training data efficiency
- Noise schedule analysis
- Partition quality analysis

### 8. Discussion and Conclusion (0.5 pages)
- Summary of contributions
- Limitations (ATSP, non-geometric problems)
- Future directions

### Appendix (~2 pages supplementary)
- Implementation details
- Additional experimental results
- Algorithm pseudocode

---

## Major Novel Contributions for Journal

### 1. E(2)-Equivariant Partition Network
This is the key new contribution using the `edisco-partition` codebase:
- Replaces GLOP's polar-angle GNN with EGNN
- Maintains E(2)-equivariance for partition task
- Enables scaling to CVRP-1000+

**Key Technical Insight:**
GLOP uses polar angle θ as a node feature, which breaks E(2)-equivariance. Our approach uses only invariant features:
- Node features: `(demand/capacity, distance_from_depot)`
- Edge features: `(distance, affinity)`

### 2. Large-Scale CVRP Experiments
New benchmarks beyond the 20-100 in ICLR appendix:
- CVRP-200, CVRP-500, CVRP-1000
- Comparison with state-of-the-art partition methods (GLOP, HGS)

### 3. Unified Framework
Single E(2)-equivariant architecture for both:
- Direct edge prediction (TSP, small CVRP)
- Partition-based solving (large CVRP)

---

## Implementation Resources

### Codebase Structure
```
edisco-partition/
├── edisco_partition/
│   ├── models/
│   │   ├── egnn.py           # E(2)-equivariant GNN
│   │   └── partition_net.py  # Full partition network
│   ├── data/
│   │   ├── instance.py       # CVRP generation
│   │   └── graph.py          # E(2)-equivariant graph construction
│   ├── sampler/
│   │   └── sequential.py     # Route sampler (from GLOP)
│   └── evaluation/
│       ├── nn_routing.py     # Nearest neighbor heuristic
│       └── lkh_routing.py    # LKH-3 integration
├── scripts/
│   ├── train.py              # Training script
│   └── evaluate.py           # Evaluation script
├── pretrained/               # GLOP's pretrained revisers
└── checkpoints/              # Saved models
```

### Pretrained Revisers (from GLOP)
Located in `edisco-partition/pretrained/`:
- `reviser_10/` - For segments up to 10 nodes
- `reviser_20/` - For segments up to 20 nodes
- `reviser_50/` - For segments up to 50 nodes
- `reviser_100/` - For segments up to 100 nodes

---

## Experimental Plan

### TSP Experiments (from ICLR)
- [x] TSP-50, TSP-100 (dense graph)
- [x] TSP-500, TSP-1000 (sparse graph, k-NN)
- [x] TSP-10000 (sparse graph)
- [x] TSPLIB real-world instances
- [x] Cross-distribution generalization (Cluster, Explosion, Implosion)

### CVRP Experiments (NEW for TPAMI)
- [ ] CVRP-200 with partition network
- [ ] CVRP-500 with partition network
- [ ] CVRP-1000 with partition network
- [ ] Comparison with GLOP (polar-angle baseline)
- [ ] Comparison with HGS, LKH-3
- [ ] Equivariance verification test
- [ ] Cross-scale generalization

### Ablation Studies
- [ ] EGNN vs. standard GNN for partition
- [ ] Effect of removing polar angle θ
- [ ] Partition quality metrics
- [ ] Training data efficiency for partition

---

## Timeline and Milestones

1. **Complete edisco-partition implementation**
   - Verify training works correctly
   - Run experiments on CVRP-200/500/1000

2. **Collect experimental results**
   - TSP results (can reuse from ICLR)
   - New CVRP partition results
   - Ablation studies

3. **Write manuscript sections**
   - Methodology (Section 4.3 is new)
   - Experiments (Section 6.2 is new)
   - Update Introduction and Related Work

4. **Polish and submit**
   - Final proofreading
   - Ensure proper differentiation from ICLR
   - Submit to TPAMI

---

## References

### Key Papers
1. GLOP (Ye et al., 2024) - Partition-based neural CVRP solver
2. HGS (Vidal et al., 2012) - Hybrid genetic search for CVRP
3. DIFUSCO (Sun et al., 2023) - Diffusion for TSP
4. DISCO (Zhao et al., 2024) - Decoupled diffusion for TSP
5. E(n)-EGNN (Satorras et al., 2021) - Equivariant GNN architecture

### ICLR Manuscript Location
`d:\Codes\EDISCO Journal\ICLR manuscript\iclr2026_conference.tex`

---

## Notes

- TPAMI requires "substantial revision" for conference extensions (typically ~30% new content)
- The large-scale CVRP partition work provides the major new contribution
- Keep the unified theme: E(2)-equivariance benefits geometric CO problems
- The key insight: polar angle θ breaks equivariance; our invariant features preserve it
