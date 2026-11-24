# Submission package — Cortensor Hackathon #5

Project: **Repo Triager** — a repeatable GitHub issue triage agent with a human approval gate
Built on **openminion 0.1.24** (OpenMinion 0.1 Python SDK), submitted to
[DoraHacks #2418](https://dorahacks.io/hackathon/2418).

Deadline: **2026-12-19 16:59 KST** (2026/12/19 07:59 UTC).

---

## 1. Where to submit

The hackathon asks for a pull request to the dedicated Hackathon #5 repository. That
repository is **`cortensor/community-projects`**. Re-checked with `gh` on 2026-11-24
against the open #5 PRs #100–#104:

* No separate `cortensor/hackathon-5` / `cortensor/hackathons` repository exists; the docs
  page never names one, but this hub is where the other #5 submissions land.
* Every open #5 PR adds one directory under **`apps/<slug>/`** and uses the title
  `[Hackathon #5] feat(apps): add <name> (v0.1.0, MIT, OpenMinion 0.1.24)` — #100 and #101
  drop the parenthetical. Ours:
  `[Hackathon #5] feat(apps): add repo-triager (v0.1.0, MIT, OpenMinion 0.1.24)`.
* The repository root holds only `.gitignore`, `LICENSE.md`, `README.md`, `apps/`,
  `infras/`, `tools/` — **there is no root-level `PROJECTS.yml`**, although the README
  still describes one. Do not add one at the root. Each app carries its **own
  `PROJECTS.yml`**: #100, #102, #103 and #104 all do, and so does the
  `apps/hello-world-bot` template.
* Per-app file pattern in the current PRs (#100 minion-radar, #102 minionbench-ops,
  #103 fixbrief-agent, #104 cortensor-task-sentinel):

  | File | #100 | #101 | #102 | #103 | #104 | Repo Triager |
  | --- | --- | --- | --- | --- | --- | --- |
  | `README.md` | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ (`README.md`) |
  | `STATUS.md` | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ (`docs/community-projects/STATUS.md`) |
  | `RELEASE.md` | ✓ | — | ✓ | ✓ | ✓ | ✓ (`docs/community-projects/RELEASE.md`) |
  | `PROJECTS.yml` | ✓ | — | ✓ | ✓ | ✓ | ✓ (`docs/community-projects/PROJECTS.yml`) |
  | `LICENSE` / `LICENSE.md` | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ (`docs/community-projects/LICENSE`) |
  | `BUILDING_BLOCKS_TESTED.md` | ✓ (`docs/`) | — | ✓ | ✓ | ✓ | ✓ (root) |
  | `WHAT_WE_LEARNED.md` | ✓ (`docs/`) | — | ✓ | ✓ | ✓ | ✓ (root) |
  | `.env.example` / `.gitignore` | ✓ | — | ✓ | ✓ | ✓ | ✓ (root) |
  | demo video | `demo/build_demo_video.py` + logs | `demo/*.mp4` in-repo | — | `record_demo.py` | linked in the PR body | `demo.mp4` in-repo (5.2 MB) |

  So our package is complete once the four files in `docs/community-projects/` move to
  `apps/repo-triager/` alongside the repository root files (`README.md`,
  `BUILDING_BLOCKS_TESTED.md`, `WHAT_WE_LEARNED.md`, `LICENSE`, `pyproject.toml`, `src/`,
  `tests/`, `findings/`, `scripts/`, `demo/`, `demo.mp4`).

Two channels, do both:

1. **DoraHacks BUIDL** for `https://dorahacks.io/hackathon/2418` — project page, repo link,
   demo video, and the two notes.
2. **PR to `cortensor/community-projects`** under `apps/repo-triager/`. Title:
   `[Hackathon #5] feat(apps): add repo-triager (v0.1.0, MIT, OpenMinion 0.1.24)`.

Checklist the hackathon gives (all of it is preparation below):

- [x] public source repository with a README and setup/run instructions → `README.md`
- [x] short explanation of the OpenMinion SDK workflow → `docs/SDK_WORKFLOW.md`
- [x] one working example → `repo-triager --provider scripted demo`
- [x] 1–5 minute demo video → `demo.mp4` (2:34, H.264 + AAC, 1280×720, 5.3 MB); shot list
      in `docs/DEMO.md`, build with `python demo/build_demo_video.py`, check with
      `python demo/verify_demo.py`
- [x] Building Blocks Tested note → `BUILDING_BLOCKS_TESTED.md`
- [x] What We Learned note → `WHAT_WE_LEARNED.md`

## 2. Repository to publish

Suggested name: `repo-triager` (public, MIT). Contents: this directory, with `var/`,
`.venv/`, `__pycache__/` ignored (see `.gitignore`).

Publish steps (needs the operator's approval — they create a public repository in the
user's name):

```bash
cd cortensor-triager
git init -b main
git add -A
git commit -m "Repo Triager: GitHub issue triage agent on the OpenMinion 0.1 SDK"
gh repo create dorithesky/repo-triager --public --source=. --push
```

**Checked 2026-11-24:** the connected GitHub credential is a GitHub App installation token
(`X-Oauth-Client-Id: Iv23liO7kHg65WeC9NFS`, no OAuth scopes). `POST /user/repos` from it
returns `403 Resource not accessible by integration`, so **the repository cannot be created
on the user's behalf** — the same limitation as `reactive-sentinel` earlier today. Creating
the empty public repository (no README, no .gitignore) from the web UI takes ten seconds and
unblocks everything after it: push, release asset, PR and DoraHacks link.

## 3. DoraHacks BUIDL text (draft)

**Title:** Repo Triager — a GitHub issue triage agent with a human approval gate (OpenMinion 0.1 SDK)

**One line:** Reads an open issue, classifies severity/component against the repository's
own labels, finds duplicates, drafts the maintainer reply, and stages the plan only after
OpenMinion's policy gate is approved — nothing is ever written to GitHub.

**Description:**

> Repo Triager is a maintainer tool built on the OpenMinion 0.1 Python SDK. It runs a
> four-step workflow on one `APIRuntime`: a read-only analysis turn that produces a
> pydantic-validated `TriageDecision`, an optional bounded `subagent()` that double-checks
> duplicates, a staging turn whose single write tool (`stage_triage`) is a bare
> `@tool` — so OpenMinion's action policy requires an operator confirmation before it can
> run — and a memory turn. Approved plans are written to a local dry-run outbox with the reply draft; denied
> plans write nothing. Decisions are stored with `memory.write` so the next run stays
> consistent, and the rubric is an ingested, admitted OpenMinion skill. Every run produces
> a Markdown/JSON report with the policy audit line and model call counts.
>
> It ships with an offline scripted provider, so the whole demo and a 69-test suite run
> without a credential or network access; switching to Cortensor Portal or any hosted
> model is one flag.
>
> Testing the SDK at this depth surfaced six reproducible issues — instructions truncated
> at ~860 characters, `output_type` never telling the model the schema, a policy denial
> breaking structured output, session ids with `/` silently disabling memory, operator CLIs
> crashing without a passwd entry, and `memory.search` hiding record content. Each has a
> runnable reproduction under `findings/`, and the operator-CLI crash has a two-line fix
> patch.

**Demo video:** `demo.mp4` in the repository (2:34, offline walkthrough; shot list in `docs/DEMO.md`).

## 4. PR body (draft for `cortensor/community-projects`)

```markdown
## Repo Triager — issue triage agent on OpenMinion 0.1 (Hackathon #5)

Adds `apps/repo-triager/`: a repeatable GitHub issue triage agent with a human approval
gate, built on the OpenMinion 0.1 Python SDK.

- **Workflow:** read issue → classify (severity, `area:*` component, type) → duplicate
  candidates → maintainer reply draft → **policy-gated staging** → memory record.
- **SDK surface:** `APIRuntime`/`Agent` (3 phases), `@tool` read/write risk split,
  `approval_callback` + pause/`thread_id` resume, `output_type`, `forced_tools`,
  `subagent()`, sessions, `memory.write`/`memory.search`, skills (ingest + admit),
  `task.schedule`/cron, capability/posture diagnostics.
- **Safety:** read-only GitHub client (GET only); the only write path is a local dry-run
  outbox behind the runtime's confirmation; labels validated against the repo taxonomy.
- **Reproducibility:** offline scripted provider + 44-issue snapshot ⇒ `repo-triager
  --provider scripted demo` and `python -m pytest` (69 tests) need no key and no network.
- **Findings:** `BUILDING_BLOCKS_TESTED.md` (with `findings/repro_*.py`) and
  `WHAT_WE_LEARNED.md` — six reproducible 0.1.24 issues, two of them P0.

DoraHacks BUIDL: <BUIDL page URL — fill in after submitting>
Demo video (1–5 min): `apps/repo-triager/demo.mp4` (also linked from the DoraHacks BUIDL)
```

## 5. Open questions for the operator before submitting

1. **Create the repository.** Checked 2026-11-24: the connected GitHub credential cannot
   create repositories (`403 Resource not accessible by integration`). Create an empty
   public `dorithesky/repo-triager` (no README, no .gitignore, no licence) and the push,
   the PR and the video link follow.
2. **Video upload.** `demo.mp4` (2:34, 5.3 MB) is built and verified. DoraHacks needs it
   uploaded to the BUIDL page; the file also goes into the repository so the PR body can
   link it.
3. **Bug-report bonus.** The hackathon asks to report non-security OpenMinion bugs as GitHub
   issues *first*, wait for acknowledgement, then submit a substantive fix PR. Our six
   findings are ready to file; the operator-CLI crash (#5) already has a two-line patch
   (`findings/patches/operator_id_fallback.patch`). Filing issues and opening PRs on
   `openminion/openminion` requires explicit approval — it is the user's public identity.
4. **Bug reports.** `docs/openminion-issue-reports.md` holds six ready-to-file issue bodies
   with versions, reproduction steps and expected/actual. Filing them is a public write
   under the user's handle, so it waits for approval; #5 already has a patch.
5. **Live-model footage.** Everything recorded here runs on the scripted provider (no
   Portal key). If a key turns up, the live path is `repo-triager triage --source live
   --issue 859 --apply ask`; the current video labels itself as offline, which is honest and
   acceptable.
6. **License.** MIT for our code, matching the hub's shared license. `LICENSE` is copied
   into `docs/community-projects/` because the template and every current #5 PR ship one
   inside the app directory.
