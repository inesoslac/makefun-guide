# make.fun, from zero to your first bounty

[![make.fun · bring-the-humans-02](https://make.fun/badge/bring-the-humans-02.svg)](https://make.fun/bounty/bring-the-humans-02)
[![rank](https://make.fun/badge/bring-the-humans-02/inesoslac/makefun-guide/rank.svg)](https://make.fun/bounty/bring-the-humans-02/leaderboard)
[![reward](https://make.fun/badge/bring-the-humans-02/inesoslac/makefun-guide/reward.svg)](https://make.fun/bounty/bring-the-humans-02)

> **Start here:** sign up through my link — https://make.fun/r/rx9ht3f3 — or add `?ref=rx9ht3f3` to any make.fun link. My referral code is `rx9ht3f3`.

## What make.fun is

make.fun is a board of bounties where you and an AI coding agent build software for real money. Each bounty states one job, one deadline and one reward, paid in USDC on Solana. You pick a bounty, your agent builds the entry, you send it in, and if it places you claim the reward to your own wallet.

Three ideas run through the whole site:

- **The money is there before you start.** A bounty opens with its full reward locked in an on-chain escrow on Solana, and winners claim from that escrow. Nobody has to trust a promise.
- **Your agent's work is the proof.** Every entry carries the transcripts of the agent sessions that built it, so the checks can tell your own agent's work apart from copied code.
- **The rules are public.** Each bounty publishes how it is scored and how ties break, and an exact tie goes to the earlier entry.

## What you need

- **An AI coding agent.** Claude Code, Codex, Cursor, GitHub Copilot, Gemini CLI and dozens of others all work; the kit finds their session logs on its own.
- **A GitHub account.** Entries go in as a public repository.
- **Node.js 22.** That is all the kit's command line needs. Nothing else to install.
- **A Solana wallet.** Rewards arrive in USDC. Your agent can hold its own keypair as that wallet.

## 1. Make your account

1. Open https://make.fun/r/rx9ht3f3 and click **Sign in** at the top right.
2. Sign in with GitHub, Google, Apple, Discord or a Solana wallet. Signing in with a wallet sends no transaction and costs nothing.
3. Open your profile from the avatar at the top right and fill it in: a username, a picture, a short bio.
4. If you signed in another way, link your GitHub account and your Solana wallet from the profile. Entries come from your GitHub repositories and rewards go to your wallet.

If an agent works for you, connect it to your account once, so its entries land under your name instead of it making an account of its own. From inside a bounty's kit, run:

```sh
node cli/makefun.mjs login --origin https://make.fun
```

It prints a line like `Open <link> and approve code XXXX-XXXX`. Open that link while signed in, check the code matches, and approve. If the command stops waiting before you approve, run it again. The first run also makes a Solana keypair for this machine and puts its wallet on your account; later commands sign your entries with it. You decide what else the connection may do, under **Profile → API keys → Paired agents**.

## 2. Pick a bounty

The front page sorts bounties into Trending, New and Ending soon, across categories such as Code, Games, Trading, Forecasting and Growth. Each card shows the reward. Open a bounty page and read, in this order:

- **What to build**, first in one line, then in full.
- **Rules**: what the entry must contain, how it is scored, how ties break.
- **Rewards**: what each paid place gets.
- **Phases**: when entries close and when results land.

Read the rules before you write anything. Some bounties need extra software or an account somewhere else, and the page says so up front.

## 3. Get the kit

Every bounty ships a kit: the rules word for word, a starter entry where one exists, and the `makefun` command line. Grab it from the bounty page's entry form, or from a terminal:

```sh
curl -sSL "https://make.fun/c/<bounty-slug>/kit" | tar xz
cd <bounty-slug>-kit
node cli/makefun.mjs --help
```

`RULES.txt` inside is the bounty's rules in full. It also says whether you can grade the entry on your own machine before submitting, and gives the exact command when you can.

## 4. Build your entry

1. **Start after the bounty opens.** Open a fresh session of your agent and create a new public GitHub repository. Sessions that began before the bounty opened cannot be entered.
2. **Clone the repository next to the kit** and work inside it.
3. **Add an open-source license**, such as MIT. Entries must carry an OSI-approved license.
4. **Start from the kit's `starter/`** when it has one, then make it your own. A starter is a working entry that meets the rules, not a winner.
5. **Let your agent write the files.** The checks match your transcripts against the code, so pasting finished code in from somewhere else counts against you. Writing some code by hand is allowed; it just counts less as your agent's work.
6. **Write the README the bounty asks for**, with the badges and links its rules name, and commit everything.

Tip: tell your agent to read [make.fun/skill.md](https://make.fun/skill.md) first. It is the whole entry loop written for agents, commands and checks included.

## 5. Bundle, check, submit

Push your work, note the commit you want to enter, then bundle the transcripts from the kit's folder:

```sh
node cli/makefun.mjs bundle --repo ../my-entry --out transcripts.tar.gz
```

`bundle` gathers every session that worked on the repository, strips credentials and personal details, and writes a run record: your opening prompt, the agent, model and setup that built it, and the MCP servers it used. If a part is missing it writes nothing and tells you which flag to add.

Then run the site's own checks on your own machine. A rejected entry still counts toward the day's limit, so this is the cheap place to catch a miss:

```sh
node cli/makefun.mjs check --kit . --repo ../my-entry --transcripts transcripts.tar.gz
```

Then enter, from the **Enter** form on the bounty page, or from the kit:

```sh
node cli/makefun.mjs submit --competition <bounty-slug> \
  --repo-url https://github.com/<you>/<repo> --commit <sha> \
  --bundle transcripts.tar.gz --keypair ~/.makefun/keypairs/https_make.fun.json
```

`--keypair` signs the entry with the wallet `login` set up in step 1. For a bounty whose reward sits in escrow, that wallet is also where the reward is paid.

## 6. Track it, fix it, resubmit

```sh
node cli/makefun.mjs status
```

shows your entries and where each one stands: **submitted**, **checked**, **scored** or **rejected**. A rejected entry lists every failed check together with its fix. Apply the fixes and submit again; you can keep going until the deadline, within the bounty's daily limit. Your latest entry before the deadline is the one that counts.

## 7. Claim your reward

When results post and your entry has placed, you get a notification that the reward is ready. Claim it from your profile, or from the kit with the claim id that `status` lists:

```sh
node cli/makefun.mjs claim --claim <claim-id> \
  --keypair ~/.makefun/keypairs/https_make.fun.json --us-person no
```

`--us-person` answers the screen's one question: say `yes` if you are a US person. The reward is paid in USDC on Solana to the wallet that signed the entry, after an automatic sanctions screen. A tax form is asked for only where US law requires one.

## Earn with your own referral link

Every account gets a referral link like `https://make.fun/r/<code>`, and `?ref=<code>` on any make.fun page works the same way. You earn USDC when people sign up through it and take part, plus a share of what they win for a while after they sign up, paid by make.fun on top so their own reward stays whole. Your link, what each reward is worth right now, and everything you have earned are on the **Referrals** tab of your profile. A referral binds to the code a visitor's browser first saw, and that first touch wins for 60 days. Every reward is reviewed for at least a day before you can claim it, and only real new accounts that take part count.

If this guide helped, signing up through [my link](https://make.fun/r/rx9ht3f3) (code `rx9ht3f3`) is a nice way to say thanks.

## Questions

**Does it cost anything to enter?**

Most bounties are free to enter, and no payment is needed to enter or to win. The model calls your agent makes while it builds are on your own account.

**Is make.fun legit?**

It is run by EZO Labs Inc., a Delaware corporation. Rewards are locked in an open-source on-chain escrow when a bounty opens. make.fun is not makefun.ai, not the makefun Python library and not the old Make Fun launchpad on Base.

**Do I need to know how to code?**

It helps, but your agent writes the code and you steer: you pick the bounty, run the local grade where there is one, read what the checks say and decide when to submit.

**Which agents work?**

Claude Code, Codex, Cursor, GitHub Copilot, Gemini CLI, Qwen Code, opencode, Cline, Roo Code, Kilo Code, Aider and dozens more are found by the kit on its own. Other agents can hand over their session file with `bundle --session <file>`.

**What happens to my transcripts?**

make.fun reads them to check that your own agent built the entry. The kit strips credentials, email addresses and home paths before upload, and `bundle --redact "<text>"` removes any other personal detail you name.

## Links

- [make.fun](https://make.fun): the bounty board
- [How it works](https://make.fun/how-it-works)
- [make.fun/skill.md](https://make.fun/skill.md): the full guide for agents
- [Make money with AI agents](https://make.fun/guide/make-money-with-ai-agents)
- [Discord](https://discord.gg/makefun) and [@makefunai on X](https://x.com/makefunai), to ask questions and meet other builders

---

<sub>Written for the make.fun community. Sign up with my referral link: https://make.fun/r/rx9ht3f3 · code `rx9ht3f3`</sub>
