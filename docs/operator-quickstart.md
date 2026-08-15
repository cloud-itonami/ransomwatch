# Operator quickstart

**There is nothing here to run.** Six tracked files, none of them code:
`CLAUDE.md`, `OWNERS`, `PROJECT.jsonld`, `README.edn`, `kotodama.jsonld`,
`migration.edn`. No `src/`, no appview, no worker, no lexicons.

That is the useful fact, and it is not obvious from the documentation, which
describes a running monitor with a sanitization guarantee. This document is
repository orientation: what the metadata claims, what is actually here, and what
therefore cannot be concluded from a deploy of this tree. It does not describe how
to monitor anything.

Steps marked ✅ were run against this tree on 2026-08-15.

---

## 1. The whole repository, in one command ✅

```bash
git ls-files
#   CLAUDE.md
#   OWNERS
#   PROJECT.jsonld
#   README.edn
#   kotodama.jsonld
#   migration.edn

git ls-files | grep -c '\.ts$\|\.cljc$\|\.py$\|\.clj$'   # 0
ls src                                                    # No such file or directory
```

## 2. What the documents claim, and what is here ✅

| Claim | Measured here | |
|---|---|---|
| `CLAUDE.md`: "Phase 1 (this commit): scaffold mirror + **4 lexicons**" | **0 lexicon files** | ✗ |
| `kotodama.jsonld`: `component: {path: "src/app.ts"}` | **no `src/` exists** | ✗ |
| `kotodama.jsonld`: `build.guestLanguage: "ts"` | **0 TypeScript files** | ✗ |
| Identity table: NSID prefix `com.etzhayyim.apps.ransomwatch.*` | — | see §3 |
| Substrate table: this repo writes `ai.etzhayyim.apps.ransomwatch.*` | — | see §3 |
| `kotodama.jsonld`: `atStandard: false` | consistent — nothing is standardised yet | ✓ |
| Worker + LangGraph pod are Phase 2 | consistent — neither is here | ✓ |

**The missing lexicons are not this repository's quirk.** Measured across the 483
repositories in `cloud-itonami` and `etzhayyim` that carry a `CLAUDE.md`: 19 claim
that some number of lexicons landed, and **16 of those 19 track zero lexicon
files**. The claims range from 4 to 14. So roughly a hundred lexicons are
documented as delivered and are not in the repositories that say so, and reading
any one of those Phase-1 paragraphs as an inventory will mislead.

## 3. `com.` and `ai.` are different authorities, and this document uses both ✅

`CLAUDE.md`'s Identity table gives the NSID prefix as
`com.etzhayyim.apps.ransomwatch.*`; its Substrate table says this side writes
`ai.etzhayyim.apps.ransomwatch.*`. Those are two different namespace authorities,
so they are two different collections, not two spellings of one.

Unlike §2 this one **is** local: measured over the same 483 documents, exactly
**2** contain both forms — this repository and its sister `public-malak`. Which of
the two is intended is a decision for the app's owner; this document does not
decide it, and does not assume the pair is a typo.

## 4. ⚠ Nothing here enforces TLP:WHITE or the no-victim-PII rule

`CLAUDE.md` states the governance plainly: TLP:WHITE only, AMBER and RED not
exposed at this surface, no victim PII — only sector, country and impact
descriptors — and it describes the TLP filter as "structural, no change".

**Structural on the vendor side.** In this repository there is no filter, because
there is no code. The string TLP appears in prose only. So:

- **A deploy of this tree is not a sanitized feed**, and there is in any case
  nothing to deploy — `component.path` points at a file that does not exist.
- The guarantee to verify before anything is exposed publicly lives where the
  orchestrator lives, which `kotodama.jsonld` names as an in-cluster LangServer
  (`ransomwatch-langgraph.murakumo.svc.cluster.local:8000`) reachable from neither
  this tree nor this machine.

This is stated because the gap between a documented guarantee and an empty
repository is exactly the gap in which someone assumes the guarantee holds.

## 5. `migration.edn` here has a different shape ✅

```bash
cat migration.edn
```

It declares `:schema "etzhayyim.migration/extracted-v1"` — not the
`etzhayyim.migration/v1` its sister repositories use — with `:status :extracted`,
`:go-files-created 0`, `:tinygo-files-created 0`, and **no
`:identity/:allowed-additions`**. So unlike the `/v1` seeds there is no declared
allow-list of additions for this document to be added to, and none was invented.

Source: `etzhayyim/root` at `60-apps/etzhayyim-project-ransomwatch`, revision
`691c245d`, tree `c801b979`, 4 tracked files, 9,033 bytes.

---

## 6. What would make this repository do something

Nothing in this tree; recorded so the next reader does not go looking. Per
`CLAUDE.md` and `kotodama.jsonld`: the 4 lexicons under
`00-contracts/lexicons/com/etzhayyim/apps/ransomwatch/` (`seedGroup`,
`listGroups`, `listPosts`, `getStats`), the worker and the LangGraph pod, and the
write path — all Phase 2, all elsewhere. `backendDependencies` names
`atproto.etzhayyim.com` and `dispatcher.etzhayyim.com`. `OWNERS` lists
`junkawasaki` as sole approver and reviewer.
