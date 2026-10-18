# Foundation-0 Repository Standard

Every Foundation-0 repository must follow the shape of
[incentre-lattices](https://github.com/Foundation-0/incentre-lattices),
to the degree its maturity tier requires. The pattern, in one sentence:
**an open research repo is an interactive page, a paper, and a machine that
re-checks its own claims** — with every claim labelled, every number
replayable, and every debt written down.

Changes to this standard go through a pull request against this file,
approved by the `core` team.

## Maturity tiers

| Tier | Meaning | Required sections below |
|---|---|---|
| Seed | Placeholder or early exploration (`astr0`, `plate-eclipses`, `point-charges` today) | A, B, C |
| Active | Research underway, claims forming (`f0`, `physics0`, `math0`, `fractals` once populated) | A, B, C, D |
| Published | Public claims with a preprint (`incentre-lattices`) | A–E |

A repo moves up a tier by meeting the checklist, not by asking. The `core`
team may grant a written exemption in the repo's README when an item genuinely
does not apply (for example, the private `ion-tracker` has no public site).

## A. Identity (every repo, every tier)

- **Description:** one real sentence. No "placeholder" text, no trailing
  whitespace. If the repo is scaffolding, say so:
  "Astronomy explorations (scaffolding — content to come)."
- **Topics:** always include `foundation-0`, plus subject tags
  (`mathematics`, `physics`, `astronomy`, `geometry`, `fractals`, ...).
- **Website:** link the Pages site once it exists (section E).
- **README** opens with, in order:
  1. Title and Zenodo DOI badge (section E; omit before the first archive).
  2. A social/OG image (`og.jpg`) and one-paragraph hook: what the
     construction or question is, in plain words.
  3. Links to the live site, the research notes, and the open tracks.
  4. "What is known and what is ours": which parts are classical, which are
     this project's. A diagram helps; keep prior art named with citations.
  5. Paper section (section E; omit before there is one).
  6. Results table (section D; omit at Seed tier).
- **Honesty rules, everywhere:** prior-art entries mean "not found in the
  searches listed", never "new". Nothing is called peer reviewed unless it
  has been. Finite checks are labelled as such.

## B. Credit and licence (every repo, every tier)

- **LICENSE:** code under MIT; text, figures and site content under
  CC BY 4.0. State both at the top of the file, then the full MIT text.
- **NOTICE:** every third-party dataset, file or construction the repo builds
  on, with source, licence, and what must be preserved (headers, attribution).
  Flag anything that must NOT be assumed freely reusable.
- **CONTRIBUTORS.md:** who originated and directed the work; which AI models
  did what (ideas, proofs, code, checking, writing); prior work credited by
  name; how to get added.
- **CONTRIBUTING.md:** the autoresearch loop, adapted to the repo:
  - Open tracks with a **locked checker** — the checker is the only judge,
    its hash is recorded, and checker edits are reviewed separately, never
    merged together with a result.
  - One pull request per result: certificate or path file, checker output,
    exact reproduction command, failed attempts, prior-art search, honest
    novelty statement.
  - Never merge your own result. Disclose AI assistance and models.
  - Claim labels: **PROVEN** (written proof), **VERIFIED** (finite check,
    not a proof), **KNOWN** (cited), **CONJECTURE**.
  - No hype. Exact statements, honest limits.
  - Include the copy-paste agent prompt so a newcomer (human or agent) can
    join a track from a fork.

## C. Branch policy and security (every repo, every tier)

Enforced per repo today; to be enforced org-wide (including future repos),
an owner runs the ruleset command at the end of this section once from a
token with the `admin:org` scope. Per-repo settings must never weaken it:

- `main` requires a pull request with **1 approval**, stale-review
  dismissal, and resolved conversations. Applies to admins.
- No force pushes, no deletions.
- Secret scanning, push protection, and Dependabot security updates on
  (free for public repos; enable where the plan allows).
- No required status checks until a repo has CI — then section D applies.
- One-time org ruleset (covers future repos; needs a token with
  `admin:org` scope — `gh auth refresh -s admin:org` — then run):

```bash
echo '{"name":"default-branch-protection","target":"branch","enforcement":"active","conditions":{"ref_name":{"include":["~DEFAULT_BRANCH"],"exclude":[]}},"rules":[{"type":"deletion"},{"type":"non_fast_forward"},{"type":"required_pull_request_reviews","parameters":{"required_approving_review_count":1,"dismiss_stale_reviews_on_push":true,"require_code_owner_review":false,"require_last_push_approval":false}},{"type":"required_review_thread_resolution","parameters":{}}],"bypass_actors":[]}' \
  | gh api orgs/Foundation-0/rulesets -X POST --input -
```

## D. Replayable results (Active tier and above)

- **`results/<name>/` per result:** `PROOF.md` (statement, scope, evidence
  label, proof, prior art, open obligations), deterministic `verify.py`,
  `certificate.json`.
- **`Makefile`:** `verify` (every verifier), `verify-fast`,
  `verify-<result>`, `certificates` (regenerate after deliberate change).
  After `make verify`, `git diff --exit-code -- results certificates
  commitment` must be empty. Pin single-threaded numerics
  (`OPENBLAS_NUM_THREADS=1`, `OMP_NUM_THREADS=1`).
- **Pinned `requirements.txt`** (plus `requirements-dev.txt` for
  figure tooling). README figures are generated from the committed
  certificates by a script, never hand-drawn from memory.
- **CI (`.github/workflows/verify.yml`):** reruns every verifier on a
  Python matrix; fails if any regenerated certificate differs from the
  committed one. State in the workflow header what a green run does and
  does not prove.
- **Hash-pinned artefacts:** `certificates/` (selected results, SHA256SUMS)
  and `commitment/` (SHA256SUMS, ROOT) recording exactly what was checked.
- **`docs/`:** `audit.md` (what each check does and does not show),
  `status.md`, `review-ledger.md`.
- **`.github/`:** pull-request template (scope, validation, prior art, AI
  disclosure, credit checklist) and a `research-review` issue template
  (error, gap, failed replay, missing prior art, wrong label).

## E. Publication (Published tier)

- **`papers/`:** the preprint PDF plus LaTeX source; superseded versions
  kept unchanged (`papers/<name>-v1.pdf`) with their hashes committed.
- **`.zenodo.json`** and **`CITATION.cff`** with matching title, authors,
  DOI and licence; the preferred citation is the preprint, the repo is
  cited for code and data.
- **Zenodo–GitHub integration on**, one DOI per release plus an
  all-versions DOI; the DOI badge opens the README.
- **Versioned GitHub releases** (`v1.0.0`, `v2.0.0`, ...) with notes saying
  what changed and what was corrected. Corrections get their own release;
  old claims are corrected in public, not edited away.
- **GitHub Pages site** served from `main` root: an entry page plus one page
  per thread of the work, shared CSS/JS, dark/light figure variants, and a
  claim ledger page labelling every claim PROVEN / VERIFIED / CONJECTURE /
  KNOWN with evidence links.

## Manual steps (cannot be done by API)

These need a human with the right logins; do them when a repo enters the
tier that needs them:

1. Enable GitHub Pages (repo Settings → Pages → Deploy from `main` root).
2. On Zenodo, flip the GitHub integration switch for the repo so releases
   archive automatically; record the DOIs in `CITATION.cff`, `.zenodo.json`
   and the README badge.
3. Set the repo website field to the Pages URL.

## Rollout checklist (bringing an existing repo into the standard)

1. Confirm the tier the repo should meet.
2. Identity: description, topics, website; README sections per tier.
3. Credit: LICENSE, NOTICE, CONTRIBUTORS.md, CONTRIBUTING.md.
4. Protection: PR + 1 approval, no force push/deletion on `main` — verify, do not relax.
5. Active: first `results/` folder with PROOF.md, verify.py, certificate;
   Makefile; CI; templates; docs ledgers.
6. Published: papers, CITATION.cff, .zenodo.json, Zenodo flip, releases,
   Pages site, DOI badge.

