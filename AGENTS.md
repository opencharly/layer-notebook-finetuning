# AGENTS.md — layer-notebook-finetuning

Standalone candy repo for the `notebook-finetuning` layer — the Unsloth
fine-tuning notebook collection provisioned into the workspace volume. The candy
lives in `charly.yml` at the repo root: the `data:` mapping, the `check:`
assertions, and the embedded `skill:` entity projected into the marketplace
corpus as `/charly-jupyter:notebook-finetuning`.

Canonical files:

- `charly.yml` — the `notebook-finetuning:` candy entity and the
  `notebook-finetuning-skill:` skill entity.
- `data/finetuning/` — the notebook collection + `notebooks.yaml` manifest.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — user overview only; never agent guidance.

## Load these skills first (R0)

- `/charly-jupyter:notebook-finetuning` — the owning skill. The collection, its
  contents, the `data:`/`dest:` provisioning, and the notebook compatibility
  fixes. Load before editing or troubleshooting the layer.
- `/charly-image:layer` — the candy authoring reference (`charly.yml` schema,
  the `data:` field, `plan:` step verbs incl. `check:`, and service
  declarations). Load before editing any entity field or plan step.

## Build / validate / test

- The merge gate is the **org-wide** `charly/pr-validator` (required check
  `validate / validate`, defined in `opencharly/.github`); this repo has **no
  per-repo candy gate** and ships only `.github/workflows/tag-on-merge.yml`.
- The candy's `plan:` `check:` steps are the functional evidence; the staged
  tree is asserted at build scope under `/data/workspace/finetuning/`, and the
  runtime checks (with `context: [runtime]`) assert the provisioned volume.
- This is a data-only candy — it declares no packages and no `require:`. Do not
  add a runtime dependency to satisfy a check.

## Modify this repo

- Edit the `notebook-finetuning:` candy entity AND the
  `notebook-finetuning-skill:` skill entity in `charly.yml` together. The skill
  is the projected usage source, so a content change not mirrored in the skill
  leaves the corpus stale.
- The `check:` assertions pin concrete notebook filenames
  (`00_Unsloth_Setup.ipynb`, `03_SFT_Training_Qwen.ipynb`,
  `08_QLoRA_Multi_Adapter_Qwen_Think.ipynb`); renaming a notebook means updating
  the corresponding `check:` in the same change.
- New behaviour claims belong in the `plan:` as an observable `check:` step, and
  in the skill body.

## Landing

- PR-only. Every change lands through a pull request; the org-required
  `charly/pr-validator` validates the diff and body and arms native auto-merge
  on PASS. Direct pushes to `main` are blocked.
- History lives in `CHANGELOG/` (written by `tag-on-merge` at merge time); the
  PR body IS the changelog.
- The authoritative rulebook is the umbrella `AGENTS.md` in
  `opencharly/opencharly` and `charly/AGENTS.md` in the charly repo. Do not
  restate its rules here.
