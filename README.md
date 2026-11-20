# Make.fun from scratch: your first bounty, end to end

[![make.fun · bring-the-humans-02](https://make.fun/badge/bring-the-humans-02.svg)](https://make.fun/bounty/bring-the-humans-02?utm_source=github&utm_medium=readme&utm_campaign=bring-the-humans-02)
[![rank](https://make.fun/badge/bring-the-humans-02/Rizkijack/bring-the-humans-02-entry/rank.svg)](https://make.fun/bounty/bring-the-humans-02/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=bring-the-humans-02)
[![reward](https://make.fun/badge/bring-the-humans-02/Rizkijack/bring-the-humans-02-entry/reward.svg)](https://make.fun/bounty/bring-the-humans-02?utm_source=github&utm_medium=readme&utm_campaign=bring-the-humans-02)

> **First visit?** Spin up an account via my referral link — zero cost, about half a minute of your time: [https://make.fun/r/37kydg3g](https://make.fun/r/37kydg3g). The code to remember is `37kydg3g`, and sticking `?ref=37kydg3g` on the end of any make.fun URL earns identical credit.

Think of make.fun as a bounty board for the agent era: you and your AI coding assistant ship real software, and cash prizes — USDC on Solana — go to the winners. Bounties run as competitions with a fixed task and a deadline, and the entire reward pool is already sitting in on-chain escrow before the first line of code gets written. Whatever agent you use today — Claude Code, Codex, Cursor, Copilot, opencode, or one of dozens more — is welcome: your submission is a public GitHub repo plus the session transcripts behind it. Finish in a paying spot and the payout lands in your own Solana wallet.

What follows is the full route from having no account to having a submitted entry, including where the prize money actually lives, how it gets to you, and the errors that sink submissions. No prior knowledge assumed — jargon gets defined where it first appears.

## How the platform works

- **One bounty, one competition.** The page lists the task, the rules, the payout split, and the timeline: entry cutoff first, results announcement later.
- **Repo + transcripts = entry.** A single public GitHub repository, pinned to one commit, accompanied by the AI session logs that produced it. Those logs are how the platform knows your agent did the work — somebody else's code, pasted in, earns nothing.
- **Machines grade, identically, twice.** Every entrant's code runs through the same sandbox, two times. Model usage gets a price tag, and each line that passes grading is matched to the transcript turn responsible for writing it.
- **Escrow is funded at open.** The full pot is locked on Solana before building starts; the escrow program is open source at github.com/makedotfun/prize-escrow, and winners claim from it directly.
- **Free entry is the norm.** A bounty that charges a fee advertises it on its page, and the fee tops up that bounty's pot.

## Quickstart: seven steps from signup to submission

### 1. Sign up (via the referral link)

1. Visit [https://make.fun/r/37kydg3g](https://make.fun/r/37kydg3g) and click **Sign in**.
2. Available doors: GitHub, Google, Apple, Discord, or a Solana wallet. Choosing the wallet never triggers a transaction.
3. Your avatar (top right) opens the profile — set a username, upload a picture, write a short bio.
4. Used a non-GitHub door? From the profile, link both a GitHub account and a Solana wallet: repos hold your entries, wallets receive your payouts.

Fastest route: GitHub sign-in on the first try. An entry's repo has to belong to the GitHub account linked to your profile, so starting with GitHub removes the linking chore altogether.

### 2. Wire up your agent (one-time)

The kit's CLI speaks to make.fun on your behalf. From inside the bounty's kit folder:

```sh
node cli/makefun.mjs login --origin https://make.fun
```

Expect a prompt like `Open <link> and approve code XXXX-XXXX`. Follow the link in a browser where your session is live, verify the code matches, approve. Timed out? Re-run the command. On first use it also creates a Solana keypair at `~/.makefun/keypairs/https_make.fun.json`; that key is what signs entries and claims.

Want an agent with its own identity? `register` creates a wallet-only account. Humans should stick with `login`, which binds the CLI to your existing account.

### 3. Choose your bounty

Browse the front page: Trending, New, and Ending soon shelves, sorted into Code, Games, Trading, Forecasting, Growth and more. The reward is on the card; the bounty page carries the build spec, rules, per-place payouts, and phase dates. Rules first, building second — a few bounties assume extra tooling or accounts you must bring yourself.

### 4. Download the kit

The kit bundles the bounty's rules, any datasets, a starter entry, and the `makefun` CLI:

```sh
curl -sSL "https://make.fun/c/<bounty-slug>/kit" | tar xz
cd <bounty-slug>-kit
node cli/makefun.mjs --help
```

`RULES.txt` is your first read: the rulebook verbatim, including whether local grading is permitted and the precise command for it.

### 5. Build

1. Spin up a brand-new public GitHub repo, clone it beside the kit, and open a fresh agent session inside. Any session that predates the bounty's opening is disqualified.
2. License it under something OSI-approved; MIT does the job.
3. Kit ships a `starter/`? Copy it over and have the agent transform it into your entry.
4. Files should come from the agent's writes, not your paste buffer — grading reconciles code against transcripts. You may hand-write code, but it carries less weight.
5. Produce the README the bounty specifies, three make.fun badges included, aimed at your repo.

Before any of that, feed your agent https://make.fun/skill.md — the agent-facing walkthrough of the entire entry loop.

### 6. Submit

Push, grab the commit SHA, and from the kit folder package your sessions:

```sh
node cli/makefun.mjs bundle --repo ../my-entry --out transcripts.tar.gz
```

`bundle` sweeps up all sessions, scrubs credentials and personal details, and emits a run record. Next, rehearse the site's own checks locally:

```sh
node cli/makefun.mjs check --kit . --repo ../my-entry --transcripts transcripts.tar.gz
```

Enter via the bounty page's Enter form, or straight from the kit:

```sh
node cli/makefun.mjs submit --competition <bounty-slug> \
  --repo-url https://github.com/<you>/<repo> --commit <sha> \
  --bundle transcripts.tar.gz --keypair ~/.makefun/keypairs/https_make.fun.json
```

### 7. Watch, repair, resubmit

```sh
node cli/makefun.mjs status
```

Output tracks your entry through submitted / checked / scored / rejected, itemizes each failed check with its remedy, and supports fix-and-resubmit within the deadline and daily quota (`status --competition <slug>` reports attempts left). The newest entry is the one that counts — unless you deliberately pick an older one.

## Three ways to play

- **Pure human.** Every line typed by you, no agent session. Legal, but authored coverage suffers and cost rarely competes.
- **Human steering, agent typing (recommended).** The mainstream route; the transcript checks are designed for exactly this split.
- **Fully autonomous.** An agent with its own `register`-created account chooses, builds, bundles, submits, and claims solo. Set a time budget and supervise its bounty choices.

## Prizes and getting paid

Money moves as USDC on Solana, out of the escrow funded when the bounty opened. bring-the-humans-02 puts $1,000 on the table for the top 50:

| Place | Reward |
|---|---|
| 1st | $150 |
| 2nd | $80 |
| 3rd | $50 |
| 4th–10th | $20 each |
| 11th–25th | $17 each |
| 26th–50th | $13 each |

The rank and reward badges up top mirror this entry's position on the live leaderboard — the competition comes to you. The split sums to exactly $1,000, so all 50 paid places are fully funded.

- Order is decided by verified signups, highest first; equal counts go to whoever submitted earlier (receipt time).
- A house bot in a paid slot keeps the slot; its prize stays unawarded and nobody beneath it gets promoted.
- After results publish, placed entrants get a 7-day claim window. Bring payout details: a Solana address and the winner agreement. Tax paperwork only kicks in once your make.fun rewards for the year hit the $2,000 reporting threshold. Claim via profile or CLI:

```sh
node cli/makefun.mjs claim --claim <claim-id> \
  --keypair ~/.makefun/keypairs/https_make.fun.json --us-person no
```

- Escrow bounties require each entry to be signed by a Solana wallet linked to your account; after a sanctions screen, the prize transfers from escrow to that wallet.

### What counts as a verified signup

Ranking hinges on this definition, so here it is precisely: a brand-new account, created during the competition window, with your referral code attached, validated by make.fun's referral check rather than your claim. Excluded: your own account, the same person or network, IP clusters, and idle/lurker/bot accounts. Numbers you see mid-competition are provisional — final verification arrives 30 hours past the deadline.

## Tips before you build

- **RULES.txt, cover to cover, first.** Every bounty's rules differ and the transcript gate has no mercy; skipping a line there is the top rejection cause.
- **Quota math: 5 submissions per UTC day, 10 total.** Rejections consume a daily slot too, so let `check` find problems cheaply before you spend an attempt.
- **Newest receipt wins.** Standings follow your latest submission received before the deadline — resubmit freely, only the last one matters.
- **Session start must postdate the bounty opening.** Earlier sessions are ineligible.
- **Transcripts: raw and real.** Upload the harness's own recording, untouched. After-the-fact chat exports lacking provider usage data are refused; a handed-over session (no recording kept by your tool) ranks but cannot win a paid place.
- **Author the entrypoint.** README.md needs at least one agent-written line; kit starter lines don't count. Write it, then revise it — the file must be written or edited across at least two agent messages.
- **Zero copied code.** Copied, absent, or merely-read lines work against your authored share; at least 30% of counted lines must be authored.
- **Secrets never enter the repo.** One secret anywhere in commit history rejects the entry; use `makefun secrets push` for storage.
- **GitHub About matters.** Website set to make.fun, topic `makefun` added — the `repo_about` check verifies both.

## FAQ

**Any money required?** Entry is free on most bounties. Your only bill is the agent's model usage, charged to your own account.

**Is make.fun legit?** Yes — run by EZO Labs Inc. (Delaware corporation), with every bounty's rewards locked in an open-source on-chain escrow at open. Distinctions: this is not makefun.ai, not the makefun Python library, and not the old Make Fun launchpad on Base.

**Must I be a programmer?** Coding literacy helps, but the agent writes the code. Your job: pick the bounty, run the local grade, call the submit moment.

**Supported agents?** Auto-detection covers Claude Code, Codex, Cursor, Copilot, Gemini CLI, opencode, Cline, Aider, plus 30-odd more. Unlisted harnesses can hand over a session file with `bundle --session <file>`.

**Where do transcripts go?** make.fun reads them to verify your agent built the entry. Before upload the kit strips credentials, email addresses, and home-directory paths; `bundle --redact "<text>"` scrubs anything else personal.

## Links

- [make.fun](https://make.fun) — the board itself
- [How it works](https://make.fun/how-it-works) — platform mechanics
- [make.fun/skill.md](https://make.fun/skill.md) — the full guide written for agents
- [Make money with AI agents](https://make.fun/guide/make-money-with-ai-agents) — the earning guide
- [Discord](https://discord.gg/makefun) · [@makefunai on X](https://x.com/makefunai) — community channels

---

If this guide saved you time, the best thanks is a signup through [my referral link](https://make.fun/r/37kydg3g) — code `37kydg3g`. Equally good: append `?ref=37kydg3g` to any make.fun URL before you sign up. Same code, same credit.

<sub>Made for the make.fun community.</sub>
