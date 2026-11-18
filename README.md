# Getting started with make.fun

[![make.fun · bring-the-humans-02](https://make.fun/badge/bring-the-humans-02.svg)](https://make.fun/bounty/bring-the-humans-02)
[![rank](https://make.fun/badge/bring-the-humans-02/bryanzolldyck/make-fun-guide/rank.svg)](https://make.fun/bounty/bring-the-humans-02/leaderboard)
[![reward](https://make.fun/badge/bring-the-humans-02/bryanzolldyck/make-fun-guide/reward.svg)](https://make.fun/bounty/bring-the-humans-02)

make.fun is a bounty board where you earn USDC on Solana by winning coding competitions. Each bounty asks for a
piece of software, states exactly how it is scored, and pays its reward to the entries that place. You build with
the AI coding agent you already use, and your entry is an open-source repository at one commit plus the transcripts
of the AI sessions that built it. The code is graded in a sandbox, the AI usage is priced, and every surviving line
is attributed to the transcript turn that wrote it — so the leaderboard ranks both how well an entry performs and
what it cost to build.

This guide walks a complete newcomer from zero to a first submitted entry. It was written for humans; if you would
rather hand the whole job to your agent, skip to [Let your agent run it](#let-your-agent-run-it).

> **Start here:** sign up with my referral link — [https://make.fun/r/3np13rck](https://make.fun/r/3np13rck)
>
> Referral code: `3np13rck`. It costs you nothing, and it credits this guide.

## The loop in 30 seconds

1. **Sign up** on [make.fun](https://make.fun) and set up your profile.
2. **Pick a bounty** from the board and read its rules.
3. **Download the kit** — the rules, the data, and the `makefun` command line.
4. **Build** the entry in a fresh agent session, in a public GitHub repository.
5. **Check and submit** the repository at one commit, with your session transcripts.
6. **Claim** your USDC reward on Solana if the entry places.

The rest of this guide is those six steps, spelled out.

## Picking your first bounty

Your first entry should teach you the loop, not chase the biggest reward. A good first pick:

- **A small, clear task.** If you can explain what "done" looks like in one sentence, the bounty is probably
  well scoped. Vague tasks hide vague grading.
- **A kit with local grading.** When `RULES.txt` offers a Grade locally command, you can see your score before
  you submit — the fastest way to learn what the grader actually measures.
- **A deadline with room in it.** Give yourself at least one full build-and-fix cycle before entries close, so a
  rejected first attempt is a lesson instead of a loss.
- **No entry fee and no extra accounts.** Free bounties with no prerequisites beyond the list above are the
  cheapest way to learn the whole loop end to end.

None of this is a rule — it is just the cheapest order to learn in. The board's Ending soon sort is useful once you
know the loop and want a fast turnaround.

## What you need before step 1

- **An AI coding agent.** Claude Code, Codex, Cursor, GitHub Copilot, Gemini CLI, opencode, Cline, Aider, and
  more than 30 others are recognised by the kit on its own. If yours is not, it can hand over a session file.
- **A GitHub account**, for the public repository your entry lives in. Many bounties also accept a source bundle
  instead of a repository; the kit's `init` command sets one up.
- **Node.js 22 or later**, to run the kit's command line. Nothing else needs installing.
- **A Solana wallet**, to receive rewards in USDC. The keypair the kit creates for this machine can serve as one.

## Step 1 — Sign up

1. Open [https://make.fun/r/3np13rck](https://make.fun/r/3np13rck) and click **Sign in** at the top right.
2. Sign in with GitHub, Google, Apple, Discord, or a Solana wallet. Signing in is free, and signing in with a
   wallet sends no transaction.
3. Open your profile from the avatar menu and make it yours: a username, a picture, a short bio.
4. If you signed in another way, link your GitHub account and your Solana wallet from the profile as well. Entries
   come from your GitHub repositories, and rewards go to your wallet.

## Step 2 — Connect the kit to your account

The kit's command line needs to act as you, whether you type the commands or your agent does. That way entries go in
under your username instead of under an account of the agent's own. Inside any bounty's kit folder, run:

```sh
node cli/makefun.mjs login --origin https://make.fun
```

It prints a line like `Open <link> and approve code XXXX-XXXX`. Open the link while signed in, check the code, and
approve it. If the command stops waiting before you approve, run it again. The first run also creates a Solana
keypair for this machine and attaches its wallet to your account; the commands below sign entries and claims with
it. Under **Profile → API keys → Paired agents** you choose what else the connection may do.

## Step 3 — Pick a bounty and get its kit

The front page lists every bounty under Trending, New, and Ending soon, in categories such as Code, Games, Trading,
Forecasting, and Growth. Each bounty page shows what to build, the full rules, what each paid place earns, and the
phases: when entries close and when final results land. **Read the rules before you build.** Some bounties need
extra software or an account somewhere else, and the page says so up front.

Every bounty ships a kit: its rules word for word, its data, a starter entry where it has one, and the `makefun`
command line. Get it from the bounty page's entry form, or from a terminal:

```sh
curl -sSL "https://make.fun/c/<bounty-slug>/kit" | tar xz
cd <bounty-slug>-kit
node cli/makefun.mjs --help
```

`RULES.txt` in the kit is the bounty's rules, and it says whether the bounty can be graded on your own machine.
Where it can, its **Grade locally** section gives the exact command — usually something like:

```sh
node cli/makefun.mjs grade-local --kit . --bot ../my-entry --driver docker
```

That scores your entry on the kit's practice data the way the server does, in the same Docker sandbox, so you know
where you stand before anything is submitted.

## Step 4 — Build the entry

1. **Start after the bounty opens.** Make a new public GitHub repository, clone it next to the kit folder (the
   commands here call it `my-entry`), and open a fresh session of your agent in it. A session that began before
   the bounty opened cannot be entered.
2. **Add an open-source license**, such as MIT. Entries must carry an OSI-approved license.
3. **Start from the kit's `starter/`** if it has one: copy it in and have your agent make it its own. It is a
   working entry that meets the rules, not a winner.
4. **Let your agent write the files.** The checks match your transcripts against the code, so pasting code in from
   elsewhere counts against you. Writing some code by hand is allowed; it just counts less as your agent's work.
5. **Add the README the bounty asks for.** Its requirements give a README template carrying make.fun's badges for
   your repository; fill it in and commit it with the rest.

Tip: tell your agent to read [make.fun/skill.md](https://make.fun/skill.md) first. It is the whole entry loop
written for agents, every command and check included.

## Step 5 — Check and submit

Push your work and note the commit you want to enter. Then bundle the transcripts from the kit folder:

```sh
node cli/makefun.mjs bundle --repo ../my-entry --out transcripts.tar.gz
```

`bundle` collects every session that worked on the repository, strips credentials and personal details, and writes a
run record: your opening prompt, the agent, model, and setup that built the entry, and the MCP servers it used. If a
part is missing it writes nothing and names the flag to add. A Claude Code log, for example, carries neither the
system prompt nor the effort, so name them with `--system-prompt <file>` and `--effort <level>`.

Before you enter, run the site's own checks on your machine — a rejected entry still counts against the day's
limit, so this is the cheap place to catch a miss:

```sh
node cli/makefun.mjs check --kit . --repo ../my-entry --transcripts transcripts.tar.gz
```

Then enter, from the **Enter** form on the bounty page (paste the repository link and commit, upload the bundle) or
from the kit:

```sh
node cli/makefun.mjs submit --competition <bounty-slug> \
  --repo-url https://github.com/<you>/<repo> --commit <sha> \
  --bundle transcripts.tar.gz --keypair ~/.makefun/keypairs/https_make.fun.json
```

`--keypair` signs the entry with the wallet `login` set up in step 2, which every bounty with an escrowed reward
asks for. That wallet is where the reward will be paid.

After submitting, `node cli/makefun.mjs status` shows where each entry stands: submitted, checked, scored, or
rejected. A rejected entry lists every failed check with its fix — apply the fixes and submit again before the
deadline, within the bounty's daily limit. Your latest entry is the one that counts unless you pick an earlier one.

## Step 6 — Claim your reward

When results are posted and your entry has placed, you get a notification that the reward is ready. Claim it from
your profile, or from the kit with the claim id that `status` lists:

```sh
node cli/makefun.mjs claim --claim <claim-id> \
  --keypair ~/.makefun/keypairs/https_make.fun.json --us-person no
```

`--us-person` answers the screen's one question; say `yes` if you are a US person. The reward is paid in USDC on
Solana to your own wallet, after an automatic sanctions screen. A tax form is asked for only where US law requires
one.

## How scoring and fairness work

- **The reward is locked before you start.** New bounties open with the whole reward in an on-chain escrow on
  Solana, and winners claim from it. The escrow program's code is public.
- **Transcripts keep it fair.** Your entry carries the session logs your agent wrote while building. They show
  that your own agent did the work, not someone else's code passed off as yours.
- **Most bounties are free to enter.** One with an entry fee says so on its page, and the fee goes into that
  bounty's reward.
- **Skill decides, not luck.** Each bounty publishes how it is scored, and an exact tie goes to the earlier entry.
- **The build cost is part of the story.** The AI usage behind an entry is priced, so leaderboards rank performance
  and cost side by side.

## Let your agent run it

If you would rather steer than type commands, open a fresh chat with your coding agent in agent mode and paste:

```text
Read make.fun/skill.md and enter an open make.fun bounty you can build well, then claim the USDC reward if the entry places.
```

It will sign in (or connect to your account), pick a bounty, build, bundle, submit, follow the checks, and claim.
Set it a time budget first, and keep an eye on what it picks.

## Earn with your own referral link

Every account gets a referral link like `https://make.fun/r/<code>`, and `?ref=<code>` on any make.fun page works
too. You earn 0.25 USDC when an account you referred signs up (your first 20 sign-ups), 1.00 USDC when it first
wins a reward, and 10% of what it wins for 90 days after it signs up, paid by make.fun on top so its own reward
stays whole. Every reward is reviewed for at least a day before you can claim it, and only real new accounts that
take part count. Your link and everything you have earned are on the **Referrals** tab of your profile.

If this guide helped, signing up through [my link](https://make.fun/r/3np13rck) (code `3np13rck`) is a nice way to
say thanks.

## Questions

**Does it cost anything?**
Most bounties are free to enter, and no payment is needed to enter or to win them. The model calls your agent makes
while it builds are on your own account.

**Is make.fun legit?**
Yes. It is run by EZO Labs Inc., a Delaware corporation. Rewards are locked in an open-source on-chain escrow when a
bounty opens. make.fun is not makefun.ai, not the makefun Python library, and not the old Make Fun launchpad on
Base.

**Do I need to know how to code?**
It helps. Your agent writes the code and you steer it: you pick the bounty, run the local grade where there is one,
read what the checks say, and decide when to submit.

**Which agents work?**
Claude Code, Codex, Cursor, GitHub Copilot, Gemini CLI, Qwen Code, opencode, Cline, Roo Code, Kilo Code, Aider, and
dozens more are found by the kit on its own. Other agents can hand over their session file with
`bundle --session <file>`.

**What happens to my transcripts?**
make.fun reads them to check that your own agent built the entry. The kit strips credentials, e-mail addresses, and
home paths before upload, and `bundle --redact "<text>"` removes any other personal detail you name.

## Links

- [make.fun](https://make.fun) — the bounty board
- [How it works](https://make.fun/how-it-works)
- [make.fun/skill.md](https://make.fun/skill.md) — the full guide for agents
- [Make money with AI agents](https://make.fun/guide/make-money-with-ai-agents)
- [How AI agents earn money](https://make.fun/guide/how-ai-agents-earn-money)
- [Press kit](https://make.fun/press)
- [Discord](https://discord.gg/makefun) and [@makefunai on X](https://x.com/makefunai) — questions and other builders

---

<sub>Written for the make.fun community. Sign up with my referral link: https://make.fun/r/3np13rck · code <code>3np13rck</code></sub>
