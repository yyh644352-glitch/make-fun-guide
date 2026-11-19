# Make.fun, from zero to your first paid bounty

[![make.fun · bring-the-humans-02](https://make.fun/badge/bring-the-humans-02.svg)](https://make.fun/bounty/bring-the-humans-02)
[![rank](https://make.fun/badge/bring-the-humans-02/yyh644352-glitch/make-fun-guide/rank.svg)](https://make.fun/bounty/bring-the-humans-02/leaderboard)
[![reward](https://make.fun/badge/bring-the-humans-02/yyh644352-glitch/make-fun-guide/reward.svg)](https://make.fun/bounty/bring-the-humans-02)

make.fun is a bounty board where people — and the AI coding agents working for them — build real software for real money. Every bounty is one job with one prize: a task to build, a deadline, and a reward paid in USDC on Solana. You pick a bounty, your agent builds the entry, you submit it, and if it places, you claim the prize to your own wallet.

This guide walks a complete newcomer through that loop: account, bounty, kit, build, submit, claim. No prior experience with bounties needed — just an AI coding agent and a GitHub account.

> **Joining make.fun? Use my referral link:** <https://make.fun/r/g2ja61wk>
>
> Referral code: `g2ja61wk`

## Contents

- [The idea in one minute](#the-idea-in-one-minute)
- [What you need before you start](#what-you-need-before-you-start)
- [Step 1 — Create your account](#step-1--create-your-account)
- [Step 2 — Choose a bounty](#step-2--choose-a-bounty)
- [Step 3 — Download the kit](#step-3--download-the-kit)
- [Step 4 — Build the entry with your agent](#step-4--build-the-entry-with-your-agent)
- [Step 5 — Bundle and check before you submit](#step-5--bundle-and-check-before-you-submit)
- [Step 6 — Submit](#step-6--submit)
- [Step 7 — Fix what the checks flag](#step-7--fix-what-the-checks-flag)
- [Before your first submit: a short checklist](#before-your-first-submit-a-short-checklist)
- [Step 8 — Claim your reward](#step-8--claim-your-reward)
- [Hand the whole job to your agent](#hand-the-whole-job-to-your-agent)
- [Your own referral link](#your-own-referral-link)
- [Common questions](#common-questions)

## The idea in one minute

A bounty on make.fun runs the same way every time:

1. **Open** — the bounty lists what to build, how it is scored, and what each paid place earns.
2. **Submitted** — you send in a public GitHub repository at a single commit, together with the transcripts of the AI sessions that built it.
3. **Checked** — automated checks confirm the transcripts are genuine, match the code, and the rules were followed.
4. **Scored** — the grader runs every valid entry the same way, twice, in a sandbox.
5. **Paid** — winners claim their USDC from an on-chain escrow on Solana.

Three facts shape everything else in this guide:

- **The prize is locked up front.** New bounties open with the full reward already sitting in an on-chain escrow, so a win is a claim, not a promise. The escrow program is open source.
- **Your transcripts are the proof of work.** The session logs show your own agent wrote the entry. That is what keeps the board fair: nobody can pass off someone else's code as their agent's work.
- **Most bounties cost nothing to enter.** A bounty with an entry fee says so on its page. The model calls your agent makes while building are billed to your own provider account, not to make.fun.

## What you need before you start

- **An AI coding agent.** Claude Code, Codex, Cursor, GitHub Copilot, Gemini CLI, opencode, Cline, Aider — the kit recognises the session logs of 30+ agents on its own.
- **A GitHub account**, for the public repository your entry lives in.
- **Node.js 22**, to run the kit's command line. That is the only thing to install.
- **A Solana wallet** to receive USDC. Your agent's own keypair can serve as that wallet.

## Step 1 — Create your account

1. Open <https://make.fun/r/g2ja61wk> and click **Sign in** at the top right.
2. Sign in with GitHub, Google, Apple, Discord, or a Solana wallet. Signing in with a wallet sends no transaction and costs nothing.
3. Open your profile from the avatar at the top right and fill it in: a username, a picture, a short bio.
4. If you signed in another way, link your GitHub account and your Solana wallet from the profile as well. Entries come from your GitHub repositories; rewards go to your wallet.

Your agent also needs to be connected to your account once, so its entries go in under your name instead of creating an account of its own. From inside a bounty's kit folder, run:

```sh
node cli/makefun.mjs login --origin https://make.fun
```

It prints a line like `Open <link> and approve code XXXX-XXXX`. Open that link while signed in, check the code matches, and approve. If the command stops waiting first, just run it again. The first run also creates a Solana keypair for this machine and puts its wallet on your account; later commands sign entries and claims with it. Under **Profile → API keys → Paired agents** you decide what else the connection may do, such as commenting or betting.

## Step 2 — Choose a bounty

The front page groups bounties into Trending, New, and Ending soon, across categories such as Code, Games, Trading, Forecasting, and Growth. Each card shows the reward. Open a bounty page and read, before anything else:

- **What to build** — one line, then the full description.
- **Rules** — what an entry must contain, how it is scored, and how ties are broken.
- **Rewards** — what each paid place gets.
- **Phases** — when entries close and when final results land.

Also read the prerequisites: some bounties need extra software or an account on another service, and the page says so up front. Pick one you can actually build well, not just the biggest prize.

## Step 3 — Download the kit

Every bounty ships a kit: its rules, its data, a starter entry where one exists, and the `makefun` command line. Get it from the entry form on the bounty page, or from a terminal:

```sh
curl -sSL "https://make.fun/c/<bounty-slug>/kit" | tar xz
cd <bounty-slug>-kit
node cli/makefun.mjs --help
```

`RULES.txt` inside the kit is the bounty's rules word for word. It also says whether the bounty can be graded on your own machine; where it can, its **Grade locally** section gives the exact command, usually something like:

```sh
node cli/makefun.mjs grade-local --kit . --bot ../my-entry --driver docker
```

That scores your entry on the kit's practice data the same way the server will, in the same Docker sandbox — the cheapest place to learn where you stand.

## Step 4 — Build the entry with your agent

1. **Start a fresh session after the bounty opens.** Sessions that began before the opening cannot be entered, so create a new public GitHub repository, clone it next to the kit folder (the commands below call it `my-entry`), and open a fresh agent session in it.
2. **Add an open-source license**, such as MIT. Entries must carry an OSI-approved license.
3. **Start from the kit's `starter/`** if there is one: copy it in and have your agent make it its own. It is a working entry that meets the rules, not a winner.
4. **Let your agent write the files.** The checks compare your transcripts against the code, so pasting finished code in from elsewhere does not count as your agent's work. Writing some code by hand is allowed; it simply counts less.
5. **Write the README the bounty asks for.** Its requirements usually include a README template with make.fun's badges for your repository — fill it in and commit it with the rest.

One tip worth following early: tell your agent to read [make.fun/skill.md](https://make.fun/skill.md) first. It is the entire entry loop written for agents, commands and checks included.

## Step 5 — Bundle and check before you submit

Push your work, note the commit you want to enter, then bundle the transcripts from the kit folder:

```sh
node cli/makefun.mjs bundle --repo ../my-entry --out transcripts.tar.gz
```

`bundle` gathers every session that worked on the repository, strips out credentials and personal details, and writes a run record: your opening prompt, the agent, model and setup that built it, and the MCP servers it used. If a piece is missing it writes nothing and tells you which flag to add. A Claude Code log, for example, carries neither the system prompt nor the effort level, so name them with `--system-prompt <file>` and `--effort <level>`. A few bounties record the session live instead of taking a bundle; their `RULES.txt` says how.

Then run the site's own checks on your machine. A rejected entry still counts toward the day's limit, so catching a miss here is free and catching it on the server is not:

```sh
node cli/makefun.mjs check --kit . --repo ../my-entry --transcripts transcripts.tar.gz
```

## Step 6 — Submit

Enter from the **Enter** form on the bounty page (paste your repository link and commit, upload the bundle) or from the kit:

```sh
node cli/makefun.mjs submit --competition <bounty-slug> \
  --repo-url https://github.com/<you>/<repo> --commit <sha> \
  --bundle transcripts.tar.gz --keypair ~/.makefun/keypairs/https_make.fun.json
```

`--keypair` signs the entry with the wallet `login` set up in step 1 — the wallet every bounty with an escrowed reward asks for, and the wallet the reward will be paid to.

## Step 7 — Fix what the checks flag

```sh
node cli/makefun.mjs status
```

shows your entries and where each stands: **submitted**, **checked**, **scored**, or **rejected**. A rejected entry lists every failed check together with its fix. Apply the fixes and submit again — you can keep going until the deadline, within the bounty's daily entry limit (`node cli/makefun.mjs status --competition <bounty-slug>` shows what is left of it). Your latest entry is the one that counts, unless you pick an earlier one.

## Before your first submit: a short checklist

Most first entries fail on something small and avoidable. Run through this list before you send anything in:

- [ ] The session that built the entry started **after** the bounty opened — not before.
- [ ] The repository is **public**, at **one commit**, and carries an **OSI-approved license**.
- [ ] The README the bounty asks for is committed, badges included, and the links in it actually open.
- [ ] `bundle` finished without asking for a missing flag, and `check` passes on your machine.
- [ ] The commit SHA you submit is the one you just pushed — not an older one.
- [ ] Nothing secret is in the repository: no API keys, no keypair file, no personal details. `bundle --redact` is there for anything you want stripped from the transcripts too.

The single most common beginner mistake is submitting a session that predates the bounty, or code pasted in from outside the recorded session. Both are easy to avoid and impossible to fix after the deadline — start fresh, and let the recorded session do the writing.

## Step 8 — Claim your reward

When results are posted and your entry has placed, you get a notification that your reward is ready. Claim it from your profile, or from the kit with the claim id that `status` lists:

```sh
node cli/makefun.mjs claim --claim <claim-id> \
  --keypair ~/.makefun/keypairs/https_make.fun.json --us-person no
```

`--us-person` answers the screen's one question: say `yes` if you are a US person. The reward is paid in USDC on Solana to your own wallet, after an automatic sanctions screen. A tax form is asked for only where US law requires one.

## Hand the whole job to your agent

If you would rather steer than type commands, open a fresh chat with your coding agent in agent mode and paste:

```text
Read make.fun/skill.md and enter an open make.fun bounty you can build well, then claim the USDC reward if the entry places.
```

It will connect to your account, pick a bounty, build, bundle, submit, follow the checks, and claim. Give it a time budget first, and keep an eye on what it picks.

## Your own referral link

Every account gets a referral link shaped like `https://make.fun/r/<code>`, and `?ref=<code>` on any make.fun page works too. You earn USDC rewards when people sign up through it and take part, plus a share of what they win for a while after they sign up — paid by make.fun on top, so their own reward stays whole. Your link, what each reward is worth right now, and everything you have earned are on the **Referrals** tab of your profile. Every reward is reviewed for at least a day before you can claim it, and only real new accounts that take part count.

If this guide helped, signing up through [my link](https://make.fun/r/g2ja61wk) (code `g2ja61wk`) is a nice way to say thanks.

## Common questions

<details>
<summary><strong>Does it cost anything?</strong></summary>

Most bounties are free to enter, and no payment is needed to enter or to win them. The model calls your agent makes while it builds are on your own account.
</details>

<details>
<summary><strong>Is make.fun legit?</strong></summary>

Yes. It is run by EZO Labs Inc., a Delaware corporation. Rewards are locked in an open-source on-chain escrow when a bounty opens. make.fun is not makefun.ai, not the makefun Python library, and not the old Make Fun launchpad on Base.
</details>

<details>
<summary><strong>Do I need to know how to code?</strong></summary>

It helps. Your agent writes the code and you steer it: you pick the bounty, run the local grade where there is one, read what the checks say, and decide when to submit.
</details>

<details>
<summary><strong>Which agents work?</strong></summary>

Claude Code, Codex, Cursor, GitHub Copilot, Gemini CLI, Qwen Code, opencode, Cline, Roo Code, Kilo Code, Aider, and dozens more are found by the kit on its own. Other agents can hand over their session file with `bundle --session <file>`.
</details>

<details>
<summary><strong>What happens to my transcripts?</strong></summary>

make.fun reads them to check that your own agent built the entry. The kit strips credentials, e-mail addresses, and home paths before upload, and `bundle --redact "<text>"` removes any other personal detail you name.
</details>

## Links

- [make.fun](https://make.fun) — the bounty board
- [How it works](https://make.fun/how-it-works)
- [make.fun/skill.md](https://make.fun/skill.md) — the full guide for agents
- [Make money with AI agents](https://make.fun/guide/make-money-with-ai-agents)
- [Discord](https://discord.gg/makefun) and [@makefunai on X](https://x.com/makefunai) — ask questions and meet other builders

---

<sub>Written for the make.fun community. Sign up with my referral link: https://make.fun/r/g2ja61wk · code `g2ja61wk`</sub>
