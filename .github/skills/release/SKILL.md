---
name: release
description: "Use when cutting a new revision/release of the APPN protocol suite: bumping the revision in publish.yaml, updating last_revised, writing the CHANGELOG entry, rebuilding the wiki and PDFs with publish_to_wiki.py, tagging, and creating the formal GitHub release. Trigger phrases: cut a release, new revision, bump the revision, publish the wiki, release process."
---

# APPN Protocol Suite — Release Process

Cuts a new locked revision of the protocol suite: version bump, changelog,
wiki + PDF regeneration, git tag, and a formal GitHub release.

## Prerequisites (verify before starting)

1. **Conda env** — `appn-sop` must exist and activate cleanly:
   `conda run -n appn-sop python -c "import yaml"` and
   `conda run -n appn-sop pandoc --version`. If missing, build it per
   [Scripts/README.md](../../../Scripts/README.md).
2. **Wiki checkout** — `../APPN-Field-Protocols-and-Pipelines.wiki` must be a
   clean git working copy (`git -C ../APPN-Field-Protocols-and-Pipelines.wiki status`).
   Pull it first.
3. **Clean main repo** — all content changes for this revision are committed
   (or at least staged and reviewed); `git status` shows no surprises.
4. **`gh` CLI authenticated** — `GH_PAGER=cat gh auth status`. Always set
   `GH_PAGER=cat` when running `gh` so output is not swallowed by a pager.

## Versioning rules (`MAJOR.YYx`)

- **MAJOR** — annual baseline, locked at the start of the season.
- **`.YY` minor** (two digits, leading zero) — in-season change to *what you
  do* or new scope: procedure correction, stub finished, new page,
  APEx-confirmed parameter. Announced at a Field EWG meeting.
- **`x` letter patch** — editorial-only (typo, clarification, photo); no
  procedure change. No letter = no patch.

**STOP and ask the user** which bump applies if it is not obvious. Procedure
corrections (e.g. changed overlap guidance) are a **minor** bump, not a patch.

## Step 1 — Collect changes since the last revision

```bash
git log --oneline v<LAST_REV>..HEAD -- Protocols/
git diff --stat v<LAST_REV>..HEAD -- Protocols/
```

If no `v<LAST_REV>` tag exists, find the last release commit in
`Protocols/CHANGELOG.md` or `GH_PAGER=cat gh release list`.

Build a per-document list of what changed and classify each as procedural or
editorial. Cross-reference open GitHub issues
(`GH_PAGER=cat gh issue list --json number,title`) — note any that this
revision resolves.

## Step 2 — Bump the revision

In [publish.yaml](../../../publish.yaml):

1. Set `revision: "<NEW_REV>"` (e.g. `"1.01"`).
2. Optionally set/uncomment `revision_date` (defaults to today UTC).
3. For **every page whose content changed**, set `last_revised: "<NEW_REV>"`.
   Unchanged pages keep their old `last_revised`.
4. If any Approved documents are being locked for the first time, ask the
   user whether to add `adopted` to their `status` list.

## Step 3 — Write the changelog entry

Add a new `## [<NEW_REV>] — <YYYY-MM-DD>` section at the **top** of the
release list in [Protocols/CHANGELOG.md](../../../Protocols/CHANGELOG.md),
matching the existing style. Include:

- A one-line summary of the revision.
- Per-document bullets of what changed and why.
- References to resolved GitHub issues as `(#N)`.

Show the draft to the user before committing.

## Step 4 — Rebuild the wiki and PDFs

```bash
conda run -n appn-sop python Scripts/publish_to_wiki.py --dry-run   # preview
conda run -n appn-sop python Scripts/publish_to_wiki.py --pdf       # real run
```

- Run from the repository root.
- `--pdf` renders snapshots into `releases/Rev<NEW_REV>/`. First tectonic run
  is slow (package cache download) — this is normal.
- Verify: `git -C ../APPN-Field-Protocols-and-Pipelines.wiki diff --stat`
  touches the expected pages; the "Locked revision" banners show `<NEW_REV>`;
  `Protocols/STATUS.md`, wiki `Home.md` and `_Sidebar.md` regenerated.

## Step 5 — Commit and push (ASK BEFORE PUSHING)

Main repo:

```bash
git add -A
git commit -m "Rev <NEW_REV>: <one-line summary>"
git push
git tag -a v<NEW_REV> -m "Rev <NEW_REV>"
git push --tags
```

Wiki repo:

```bash
cd ../APPN-Field-Protocols-and-Pipelines.wiki
git add -A && git commit -m "Publish Rev <NEW_REV>" && git push
```

## Step 6 — Formal GitHub release

Create a release from the tag, with the changelog entry as the body and the
PDF snapshots attached:

```bash
GH_PAGER=cat gh release create v<NEW_REV> \
  --title "Rev <NEW_REV>" \
  --notes-file /tmp/release-notes.md \
  releases/Rev<NEW_REV>/*.pdf
```

- Write the CHANGELOG section for this revision into `/tmp/release-notes.md`
  first (strip the `## [x.yz]` heading; gh supplies the title).
- Attach every PDF in `releases/Rev<NEW_REV>/` (and `checklists/` if present).

## Step 7 — Close resolved issues

For each issue fixed in this revision:

```bash
GH_PAGER=cat gh issue close <N> --comment "Fixed in Rev <NEW_REV> (<link to release>)"
```

Ask the user before closing.

## Step 8 — Post-release checks

- Wiki pages render correctly on GitHub (spot-check one changed page).
- `Protocols/STATUS.md` reflects the new revision.
- Remind the user: **minor releases must be announced at a Field EWG
  meeting**.
