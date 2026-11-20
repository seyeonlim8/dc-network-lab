# P0 - Lab environment (OrbStack + containerlab + FRR)

**Status:** in progress. Lab host is ready; Docker, containerlab and FRR are not installed yet.

## Goal
- 2 FRR nodes with eBGP in state Established
- Ping between the nodes
- Reproducible from scratch using `docs/setup.md`

## Done so far
- Isolated Ubuntu 24.04 (arm64) VM on OrbStack
- Base packages (git, curl), GitHub access via a repo-scoped deploy key

## Layout
- `docs/setup.md` - step-by-step setup with verification
- `configs/` - FRR configs (not yet)
- `evidence/` - command outputs used as proof
