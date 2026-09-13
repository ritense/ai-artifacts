# ai-artifacts

Binary artifacts published by the auto-pr pipeline
([plugin-central](https://github.com/ritense/plugin-central)). **Everything here is machine-written.
None of it is documentation, none of it is code, and nothing in it should ever be executed.**

## What is in here

Two producers, two prefixes, and they never overlap:

```
verification/<owner>-<repo>-<n>/     proof that a pull request's change was actually seen working
  run.gif                            the sequence, where the fix is a sequence
  before.png                         the symptom, on the base branch
  after.png                          the same view, fixed

mockups/<owner>-<repo>-<n>/          presentational options for a directional issue, for the
  option-a.png                       reporter to choose between before any code is written
  option-b.png
  README.md                          the issue URL, the date, one line per option
```

One directory per issue, named with the full `<owner>-<repo>-<n>` slug — a bare issue number collides
across the 187 boards in `config/issues.allow`, where `#96` exists on most of them.

`verification/` is written by `/verify-locally` E3, published here by `/work-ticket` B6, and embedded in
the pull request body by B8. `mockups/` is written and published by `/propose-mockup`, and linked from
the one comment it posts on the issue. Both embed as `raw.githubusercontent.com` URLs **pinned to the
commit SHA**, never to a branch name: the next issue's artifacts move `main`, and a URL naming a branch
would silently start pointing at whatever is there now.

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

## Visibility — public, and not by preference

Private was tried first and does not work. GitHub renders a `raw.githubusercontent.com` image as a
direct `<img src>` rather than through its camo proxy, so the **viewer's browser** fetches it — and that
host answers `404` without an `Authorization` header, which a browser never sends on an image load. A
private artifact repository therefore renders as broken images for *everyone*, Ritense staff included.
Measured, not assumed: `404` unauthenticated, `200` with a token, and `404` in a logged-in browser.

So the leak this repository would otherwise cause is prevented one level up instead, by a single rule
that both producers obey:

> **Nothing rendered from a private repository is published here.**
>
> `/work-ticket` B6 publishes proof only when the pull request's own repository is public. A change
> landing in `valtimo-platform/deployer`, `ritense/pdca-service` or any other private repository gets its
> `## Proof` table and its test output and **no images at all**.
>
> `/propose-mockup` is refused outright for an issue on a private tracker, before the unit is spent —
> there is nowhere to put the pictures, and the whole point of that skill is the pictures.

A recording is not worth making an unreleased internal product's UI world-readable. This is why the
`## Proof` table is written to stand on its own without them.

This was originally going to be two repositories, on the theory that mockups and proof wanted opposite
visibility. They do not. A mockup is rendered from the real frontend with seed data, which is the same
exposure as a screenshot of it. One repository, one rule. Two-thirds of the backlog sits on
`generiekzaakafhandelcomponent/gzac-issues`, which is public, so the rule costs less than it sounds like.
