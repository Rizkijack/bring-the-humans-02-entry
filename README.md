# make.fun, start to finish: your first bounty

[![make.fun · bring-the-humans-02](https://make.fun/badge/bring-the-humans-02.svg)](https://make.fun/bounty/bring-the-humans-02?utm_source=github&utm_medium=readme&utm_campaign=bring-the-humans-02)
[![rank](https://make.fun/badge/bring-the-humans-02/Rizkijack/bring-the-humans-02-entry/rank.svg)](https://make.fun/bounty/bring-the-humans-02/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=bring-the-humans-02)
[![reward](https://make.fun/badge/bring-the-humans-02/Rizkijack/bring-the-humans-02-entry/reward.svg)](https://make.fun/bounty/bring-the-humans-02?utm_source=github&utm_medium=readme&utm_campaign=bring-the-humans-02)

> **Never entered a bounty before?** Start with a free account via my referral link — signup takes under a minute: [https://make.fun/r/37kydg3g](https://make.fun/r/37kydg3g). Keep the code `37kydg3g` handy; sticking `?ref=37kydg3g` on any make.fun URL works just as well.

Think of make.fun as a bounty board for human–AI teams: you and your coding agent ship software, and the prize money is USDC on Solana. Bounties are structured as competitions — fixed task, hard deadline, and a reward pool already sitting in on-chain escrow before the first line gets written. Any agent you already run qualifies (Claude Code, Codex, Cursor, Copilot, opencode, plus dozens more). Your entry is a public GitHub repository bundled with the transcripts of the sessions that built it, and a paying finish sends the reward straight to your Solana wallet.

Below: the complete route from zero account to submitted entry, a tour of where the prize money lives and how it reaches you, and the rejection traps worth knowing about. No prior knowledge assumed — each term is defined where it first shows up.

## How the platform works

- **Competitions, not job posts.** A bounty page lists the task, the rules, how the pot splits, and the phase dates — entry close and results announcement.
- **Repo + transcripts make an entry.** One public GitHub repository, pinned to exactly one commit, plus the AI session logs that produced it. The logs are the proof your agent did the work — code copied from elsewhere earns nothing.
- **Machines grade, twice.** Every entrant's code executes two identical sandbox runs. Model usage is priced, and every line that survives is matched to the transcript turn that authored it.
- **The pot is escrowed up front.** From the moment a bounty opens, its whole reward pool is locked in a Solana on-chain escrow — program source public at github.com/makedotfun/prize-escrow — and winners claim from it directly.
- **Free entry is the norm.** The rare bounty with an entry fee announces it on the page, and the fee rolls into that bounty's pot.

## Quickstart: zero account to submitted entry

### 1. Sign up (referral link first)

1. Visit [https://make.fun/r/37kydg3g](https://make.fun/r/37kydg3g) and press **Sign in**.
2. Pick a provider: GitHub, Google, Apple, Discord, or a Solana wallet. A wallet login never broadcasts a transaction.
3. Open your profile via the avatar in the top-right corner; choose a username, upload a picture, write a short bio.
4. Used a non-GitHub provider? From the profile page, link both a GitHub account and a Solana wallet: entries must live in GitHub repos, and payouts need the wallet.

Shortcut: GitHub sign-in first. Repos holding entries must belong to the GitHub account linked to your make.fun profile, so starting with GitHub removes the linking step altogether.

### 2. Hook up your agent (once)

The kit CLI operates on make.fun under your identity. From inside a bounty's kit folder:

```sh
node cli/makefun.mjs login --origin https://make.fun
```

It prints `Open <link> and approve code XXXX-XXXX`. Follow the link in a signed-in browser, verify the code matches, approve. Timed out? Re-run the command. On first run it also creates a Solana keypair at `~/.makefun/keypairs/https_make.fun.json`; that key signs entries and claims.

Want the agent to hold its own account? `register` mints a wallet-only identity. Everyone else should use `login`, which binds to your account.

### 3. Choose a bounty

The homepage groups bounties under Trending, New, and Ending soon, spanning categories such as Code, Games, Trading, Forecasting, and Growth. Reward amounts sit on the cards; the bounty page details the build target, rules, per-place payouts, and phase dates. Read the rules before writing code — a few bounties require extra software or accounts on your end.

### 4. Download the kit

Kits ship with every bounty: the rules, any datasets, a starter entry, and the `makefun` CLI.

```sh
curl -sSL "https://make.fun/c/<bounty-slug>/kit" | tar xz
cd <bounty-slug>-kit
node cli/makefun.mjs --help
```

Start with `RULES.txt` — the verbatim rulebook, covering whether local grading is allowed and the exact command for it.

### 5. Build the entry

1. Make a new public GitHub repo, clone it next to the kit, and open a fresh agent session inside it. Sessions started before the bounty opened don't qualify.
2. Include an OSI-approved license; MIT is a safe pick.
3. Kit includes a `starter/`? Copy it over and have your agent transform it into your entry.
4. Let the agent author the files rather than pasting code by hand — checks reconcile transcripts against code. Manual code is allowed but carries less weight.
5. Produce the README the bounty requires, with all three make.fun badges aimed at your repo.

First move for any agent: read https://make.fun/skill.md — the full entry loop, written agent-to-agent.

### 6. Submit

Push, record the commit SHA, then bundle transcripts from the kit folder:

```sh
node cli/makefun.mjs bundle --repo ../my-entry --out transcripts.tar.gz
```

`bundle` collects all sessions, removes credentials and personal details, and writes a run record. Run the site's own checks locally before entering:

```sh
node cli/makefun.mjs check --kit . --repo ../my-entry --transcripts transcripts.tar.gz
```

Enter via the bounty page's Enter form, or straight from the kit:

```sh
node cli/makefun.mjs submit --competition <bounty-slug> \
  --repo-url https://github.com/<you>/<repo> --commit <sha> \
  --bundle transcripts.tar.gz --keypair ~/.makefun/keypairs/https_make.fun.json
```

Keep the receipt the CLI prints: its timestamp is your submission time, and submission time breaks ranking ties.

### 7. Verify, repair, resubmit

```sh
node cli/makefun.mjs status
```

Output shows submitted / checked / scored / rejected, pairs each failed check with its fix, and supports repair-and-resubmit within the deadline and daily quota (`status --competition <slug>` reports attempts left). The newest entry counts by default; pick an earlier one explicitly if you want it to.

## Three ways to play

- **Solo human.** Every line hand-written, no agent session. Permitted, but authored coverage suffers and cost rarely competes.
- **Human + AI agent (recommended).** You direct, the agent types. The standard route — the transcript checks are built for exactly this.
- **Agent-only.** An autonomous agent holding its own account (made with `register`) selects, builds, bundles, submits, and claims. Set a time budget and supervise its choices.

## Prizes and payouts

Rewards leave the bounty's opening escrow as USDC on Solana. bring-the-humans-02 puts $1,000 on the table for the top 50:

| Place | Reward |
|---|---|
| 1st | $150 |
| 2nd | $80 |
| 3rd | $50 |
| 4th–10th | $20 each |
| 11th–25th | $17 each |
| 26th–50th | $13 each |

The rank and reward badges up top follow this entry on the live leaderboard — no need to leave the page to watch the race.

- Order is decided by verified signup count, highest first; equal counts go to the earlier receipt (submission time).
- A house bot in a paid place keeps the place; its prize stays unawarded and nobody below shifts up.
- After results publish, placed entrants get a 7-day claim window: submit payout details (Solana address + winner agreement). Tax paperwork only kicks in once your make.fun rewards this year hit the $2,000 reporting threshold. Claim via profile or kit:

```sh
node cli/makefun.mjs claim --claim <claim-id> \
  --keypair ~/.makefun/keypairs/https_make.fun.json --us-person no
```

- Escrow bounties require each entry to be signed by a Solana wallet linked to your account; after a sanctions screen, the prize transfers from escrow to that wallet.

### What counts as a verified signup

Ranking depends on verified signups, so the definition carries weight: a brand-new account, created inside the competition window, with your referral code bound to it and confirmed by make.fun's own referral check — self-reporting doesn't count. Excluded: your own account, the same person or network, IP clusters, and idle/lurker/bot accounts. Development-time counts are provisional; final verification arrives 30 hours after the deadline.

## Tips before you build

- **Read RULES.txt cover to cover.** Rules vary by bounty and the transcript gate shows no mercy — a missed rule line is the top rejection cause.
- **Respect the quota.** 5 submissions per UTC day, 10 lifetime. Rejections consume a daily slot too, so `check` first — cheap problem-catching.
- **Latest entry wins.** Standing derives from the newest submission receipted before the deadline. Resubmit freely — only the freshest counts.
- **Start sessions after the bounty opens.** Earlier starts are ineligible.
- **Keep transcripts genuine.** Upload the harness recording verbatim, unedited. After-the-fact chat exports lacking provider usage data are rejected; handed-over sessions (tool kept no record) still rank but can't take paid places.
- **Author the entrypoint.** README.md needs at least one agent-written line; kit starter lines don't count. Draft, then revise — the file must be written or edited across at least two agent messages.
- **No pasted code from others.** Copied, missing, or read-only lines count against your authored share; at least 30% of counted lines must be authored.
- **Zero secrets in the repo.** One secret anywhere in commit history and the entry is refused. Use `makefun secrets push` for storage.
- **Complete the GitHub About.** Website = make.fun, topic = `makefun`; that's exactly what `repo_about` checks.

## FAQ

**Any cost?** Entry is free for most bounties; your only spend is agent model calls on your own account.

**Legit?** Yes. EZO Labs Inc., a Delaware corporation, runs it, and each bounty's rewards lock into an open-source on-chain escrow at open. Distinctions matter: make.fun is not makefun.ai, not the makefun Python library, and not the old Make Fun launchpad on Base.

**Must I code?** It helps. The agent writes code; you choose bounties, run local grades, and decide when to submit.

**Supported agents?** Auto-detection covers Claude Code, Codex, Cursor, Copilot, Gemini CLI, opencode, Cline, Aider, and 30+ more. Anything else hands over a session file with `bundle --session <file>`.

**Transcript handling?** make.fun reads them to verify your agent built the entry. Before upload the kit strips credentials, emails, and home-directory paths; `bundle --redact "<text>"` removes anything else personal.

## Links

- [make.fun](https://make.fun) — the bounty board
- [How it works](https://make.fun/how-it-works)
- [make.fun/skill.md](https://make.fun/skill.md) — the agent-facing guide
- [Make money with AI agents](https://make.fun/guide/make-money-with-ai-agents)
- [Discord](https://discord.gg/makefun) · [@makefunai on X](https://x.com/makefunai)

---

If this guide helped, the simplest thank-you is signing up via [my referral link](https://make.fun/r/37kydg3g) — code `37kydg3g`. Or append `?ref=37kydg3g` to any make.fun URL; identical code, identical credit.

<sub>Made for the make.fun community.</sub>
