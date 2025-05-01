# The FNDTN Protocol
### A Framework for a Permission‑less Decentralised Open Trust‑Minimised Scientific Publishing with Bitcoin-Based Peer-Review and Reproducibility Monetization

*dr. Kazimieras Bagdonas*

*kazimieras.bagdonas@ktu.lt*

*Version 0.1  |  1 May 2025*

---

## Abstract

The FNDTN Protocol is an open, permission‑less architecture that re‑imagines the scholarly publishing stack around cryptographic integrity and Bitcoin‑denominated incentives.  By anchoring every manuscript, review, and revision into a Bitcoin‑timestamped time‑chain, and transporting all interactions through the resilient nostr messaging fabric, FNDTN eliminates single points of failure in science communication.  A game‑theoretic reward schedule executed over the Lightning Network aligns the interests of authors, reviewers, evaluators, and archivists—transforming peer review from an unpaid chore into a verifiable, scarce labor market.  The result is a censorship‑resistant, transparent, and economically sustainable research commons.

---

## 1  Motivation

Modern science suffers from a cascade of market failures. Journals erect paywalls despite publicly funded research; opaque editorial decisions distort citation economies; reviewers donate unpaid labor to for‑profit publishers; and reproducibility crises erode public trust. Blockchain experiments have promised remedies, yet many replicate the Web2 gatekeepers under new tokens or introduce complexity incompatible with academic workflows.

FNDTN takes a leaner path.  We do **not** build a new blockchain or create new crypto *tokens*.  We leverage the Bitcoin network — the original open monetary network, the most secure public ledger — to enable the monetization of review activities and timestamp published content.  We expose an open protocol that any interface, DAO, or university repository can plug into.  We do **not** mint an inflationary governance coin.  Reputation is earned through provable scholarly action and measured on a logarithmic scale from 1 to 10.

---

## 2  Protocol Overview

At its core, FNDTN is three interlocking layers:

1. **Integrity Layer** Submitted publications are integrated in the preprint mempool, where the network initiates the search for compatible reviewers and evaluators. Every accepted revision yields a Merkle root immutably embedded in a sidechain OpenTimestamps commitment.  This root links to a linear Git history of the manuscript and its annexed artifacts (data, code, reviews).  Anyone can reproduce the hash locally to verify authenticity. When the review process is completed and the publication is greenlighted, the publication is included in the hash link chain. If the publication is rejected, the authors can initiate a new review process with the submission of additional funds and a new call. Original reviewers and evaluators are excluded from the next pool of review candidates.

2. **Messaging Layer** All protocol events—submission, reviewer invitations, evaluation scores, treasury votes—are signed nostr events.  Clients relay these messages through various volunteer relays; no single relay is authoritative.  Private reviewer discussions are encrypted sealed boxes that only decrypt after decision release, enabling double‑blind review without sacrificing auditability.

3. **Economic Layer** Authors escrow satoshis into a smart contract when submitting the manuscript for review. The funds can be retrieved if no reviewers are assigned during the specified time period. Once the reviewers are selected and have agreed on the review assignment, the funds are locked until the end of the review process. Evaluators subsequently judge the quality of reviews, and if the work is considered sub-par, the evaluator can reject the reviewer and initiate the search for an alternative. This decision can be appealed by the reviewer, and a panel of secondary evaluators is selected to provide independent judgment. The funds are to be distributed in an adversarial game theory based manner to minimize the abuse of the system, with significant penalties to the reputation of the reviewers and evaluators who are determined to abuse the system.

 The contract logic (expressed as a Taproot‑enabled DLC) selects reviewers whose declared fee and minimum reputation thresholds match the escrow.  Reviewers and higher‑reputation evaluators are randomly chosen by hash lottery, paid automatically in Lightning as soon as evaluations converge.  A 0.33 % tithe flows to the protocol treasury from publishing and a 3.33 % from bounty activities, which funds open‑source maintenance and bug bounties.

---

## 3  Actors & Incentives

* **Scholar Node** — A pseudonymous Ed25519 key used to author papers, review others and vote on proposals.  Reputation *R* increases logarithmically with citations, accepted reviews, and meta‑review scores.  High‑R scholars gain priority for evaluator roles and larger revenue share.

* **Library Node** — University‑run (or community‑run) servers replicating the whole history and serving large files through IPFS or Arweave.  They post bonded collateral; downtime slashes the bond, disincentivizing data loss.

* **Org Node** — Funding bodies or DAOs that post reproduction bounties.  The first *N* independent teams to replicate key findings receive automatic Lightning payouts, verifiably linking their replication dataset to the original Merkle root.

* **Evaluators** — Drawn from the 90th percentile of reputation, they grade each review on a 100‑point rubric.  Because both reviewers and evaluators are pseudonymous, their dominant strategy under the payment schedule is honest work: collusion would require revealing identity and jeopardizing future income.

Game‑theoretic analysis shows that for rational agents, the Nash equilibrium is: (i) authors submit genuine scholarship to maximize citation‑driven reputation; (ii) reviewers invest effort proportional to escrow size; (iii) evaluators grade truthfully; (iv) libraries maintain constant uptime to avoid bond slashing.

---

## 4  Workflow Walkthrough

1. **Submission**: The author broadcasts a `PaperSubmitted` nostra event, attaching keywords, the requested minimum reviewer reputation, the Lightning escrow invoice, and a git commit.
2. **Matching**: A deterministic TF‑IDF routine executed by library nodes matches candidate reviewers; a hash‑seed lottery selects 3 reviewers and 6 evaluators.
3. **Double‑Blind Review**: Reviewers clone the repo and open pull requests with inline comments. All traffic is boxed until the verdict.
4. **Evaluation**: Evaluators score each review.  If the median score ≥ 60, the manuscript passes; else the author revises and resubmits without extra fee for the first *N* rounds.
5. **Publication**: Library nodes anchor the Merkle root, Treasury splits the escrow via Lightning, and the review packets decrypt for the public record.
6. **Replication Phase (optional)**: The Org Node posts a `ReproCall` event with a bounty; replications referencing the original DOI compete for the Lightning payout.

---

## 5  Governance (TBF)

Protocol upgrades follow FNDTN Improvement Proposals (FIPs).  The voting weight is `log₂(R × stake_sats).`  This blend balances merit (reputation) and commitment (economic stake) while dampening whale dominance with the logarithm.

A two‑thirds super‑majority of bonded Library nodes can trigger an emergency fork (e.g., to patch critical vulnerabilities).  Day‑to‑day parameter tuning—like fee splits or reviewer count—passes with the simple majority over a one‑week nostr voting window.

*The exact governance model TBD after the minimum viable technological demonstration prototype is developed*

---

## 6  Implementation Roadmap

| Quarter | Milestone |
|---------|-----------|
| **Q3 2025** | Reference nostr client & basic Lightning escrow contract on Signet. |
| **Q4 2025** | Library node alpha: Git+IPFS integration, Merkle anchoring, public API v0.1. |
| **Q1 2026** | Blind‑review workflow, reputation oracle, and evaluator grading UI. |
| **Q2 2026** | Reproduction bounty module, DAO treasury, and federation with existing preprint servers. |

---

## 7  Conclusion

FNDTN eliminates rent‑seeking intermediaries from scientific publishing by fusing the world’s most secure ledger with a permission‑less review marketplace.  Anyone—tenured professor, independent researcher, or citizen scientist—can contribute under a pseudonym, build a provable track record, and earn Bitcoin for advancing knowledge.  Transparency is the default; reproducibility is rewarded; censorship is technically impossible without shutting down nostr and Bitcoin.  We hope scientists, researchers, universities, and open‑source contributors can benefit from participating in such a network.

---

*For discussion, specifications, and code: <https://github.com/DDAHHDP/fndtn>*