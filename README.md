# layer-notebook-finetuning

The Unsloth fine-tuning notebook collection, provisioned into the workspace
volume at deploy time, as a standalone OpenCharly layer repo.

This is a **data-only candy** — no packages, no services, no dependencies. It
uses the `data:` field in `charly.yml` to map a directory of notebooks to a named
volume with a subdirectory destination. The collection includes Unsloth_Setup,
Vision_Training, SFT, GRPO, DPO, Reward, RLOO, and QLoRA variants, and is
consumed by `jupyter-ml-notebook` for an Unsloth-ready JupyterLab image.

## What it provides

| Property | Value |
|---|---|
| Layer / candy | `notebook-finetuning` |
| Data | `data/finetuning` → `workspace` volume, dest `finetuning` |
| Contents | 37 Jupyter notebooks + a `notebooks.yaml` manifest |
| Packages | none |
| Service / port | none |

At build time the contents of `data/finetuning/` are staged into
`/data/workspace/finetuning/` inside the box. At deploy time, when the workspace
volume is bind-mounted (`charly config --bind workspace`), `charly config` copies
the staged data into `<workspace>/finetuning/`, seeding the volume with
ready-to-use training notebooks.

## How to use it

Compose the layer as a nested `candy:` list inside a named box body:

```yaml
unsloth-studio:
  candy:
    base: fedora
    candy:
      - '@github.com/opencharly/layer-notebook-finetuning:v2026.239.1606'
      - unsloth-studio
```

```bash
charly config unsloth-studio --bind workspace
charly start unsloth-studio
# open http://localhost:8888 → navigate to finetuning/
```

## Layout

- `charly.yml` — the `notebook-finetuning:` candy entity (the `data:` mapping,
  the `check:` assertions, and the embedded `notebook-finetuning-skill:` skill
  entity).
- `data/finetuning/` — the notebook collection + `notebooks.yaml` manifest.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — this user overview.

## Related

- Owning skill: `/charly-jupyter:notebook-finetuning` — the collection, its
  contents, and the `data:`/`dest:` provisioning.
- `/charly-jupyter:unsloth-studio` — the box that owns the workspace volume.
- `/charly-jupyter:notebook-templates` — sibling data candy.
- `/charly-image:layer` — candy authoring reference.
- [`opencharly/charly`](https://github.com/opencharly/charly) — the charly CLI and image builder
- [`opencharly/opencharly`](https://github.com/opencharly/opencharly) — the umbrella
