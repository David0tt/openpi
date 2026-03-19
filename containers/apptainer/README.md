# AI GENERATED!

# Singularity GPU Container For openpi

This gives you a newer userspace (including newer libc) than the host, while keeping your project and uv workflow unchanged.

## 1) Build the image

Run from repo root:

```bash
singularity build --fakeroot openpi.sif containers/apptainer/openpi.def
```

Why --fakeroot?

- Building from a definition file needs root-like privileges to run package manager steps in `%post`.
- `--fakeroot` provides those privileges for unprivileged users.
- You only need this for build time, not for running the container.

If your cluster does not allow --fakeroot, ask for one of:

- admin-built SIF from the same def file
- Singularity configured with fakeroot/subuid/subgid
- `singularity build --remote openpi.sif containers/apptainer/openpi.def` (if remote builder is enabled)

## 2) Enter container with GPU and project mount

```bash
singularity shell --nv openpi.sif
cd /workspace
```

## 3) Verify libc and GPU inside container

```bash
ldd --version
nvidia-smi
python --version
uv --version
```

## 4) Run your original command

```bash
cd /workspace
uv sync
uv run scripts/compute_norm_stats.py --config-name pi0_mypolicy
```

## Notes

- Keep all installs and `uv run` inside the container.
- Keep `.venv` inside the mounted project directory as usual.
- Always pass --nv for CUDA/GPU access.
