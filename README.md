# ai-artifacts

Binary artifacts published by the auto-pr pipeline
([plugin-central](https://github.com/ritense/plugin-central)). **Everything here is machine-written.
None of it is documentation, none of it is code, and nothing in it should ever be executed.**

## What is in here

```
verification/<owner>-<repo>-<n>/     proof that a pull request's change was actually seen working
  run.gif                            the sequence, where the fix is a sequence
  before.png                         the symptom, on the base branch
  after.png                          the same view, fixed
```

One directory per issue, named with the full `<owner>-<repo>-<n>` slug — a bare issue number collides
across the 187 boards in `config/issues.allow`, where `#96` exists on most of them.

The files are written by `/verify-locally` E3, published here by `/work-ticket` B6, and embedded in the
pull request body by B8 as `raw.githubusercontent.com` URLs **pinned to the commit SHA**, never to a
branch name.

## Two rules

**Nothing is ever deleted.** Every link in every pull request body ever opened by the pipeline points at
a SHA in this repository. Removing a directory breaks the proof on merged pull requests retroactively,
including ones closed months ago. The size is bounded at the other end instead: `/verify-locally` E3
caps a single issue's artifacts at 5 MB.

**Nothing but artifacts lives here.** The pipeline pushes to `main` directly, with no pull request and
no review. That is safe only because there is nothing here for a push to break. No CI, no workflows, no
`.claude/`, no source. `config/repos.allow` in plugin-central explains why an artifact repository is a
repository at all rather than a gist: it is the one gate standing between model-authored text and a
`git push`.

## Visibility

Private, deliberately. Some of what gets photographed here is an unreleased internal product —
`valtimo-platform/deployer`, `ritense/pdca-service`, `generiekzaakafhandelcomponent/atlas-internal`. The
cost is that images in a pull request on a *public* repository render as broken for anyone outside
Ritense; the `## Proof` table in the body still reads on its own, which is why that table is written to
stand without the pictures.

This repository therefore cannot serve `/propose-mockup`, which links images into comments that public
reporters must be able to see. That needs a separate, public artifact repository.
