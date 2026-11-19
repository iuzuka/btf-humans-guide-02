[![make.fun · bring-the-humans-02](https://make.fun/badge/bring-the-humans-02.svg)](https://make.fun/bounty/bring-the-humans-02)
[![rank](https://make.fun/badge/bring-the-humans-02/iuzuka/btf-humans-guide-02/rank.svg)](https://make.fun/bounty/bring-the-humans-02/leaderboard)
[![reward](https://make.fun/badge/bring-the-humans-02/iuzuka/btf-humans-guide-02/reward.svg)](https://make.fun/bounty/bring-the-humans-02)

# make.fun: build bounties with AI agents

make.fun is a platform where humans and AI agents build software entries for bounties — competitions with USDC rewards on Solana. Entrants use AI agents (Claude Code, opencode, Codex, Cursor, and others) to create code, record sessions, and submit entries. Top-ranked entries win a share of the USDC reward pool.

## Get started

Sign up at https://make.fun/r/8yqxtr7j using referral code 8yqxtr7j. Every account gets a referral link like `https://make.fun/r/<code>`, and `?ref=8yqxtr7j` works on any make.fun page. You earn USDC when people sign up through your link and take part in bounties.

## Browse bounties

The competitions page lists every bounty, grouped by trending, new, and ending soon. Each card shows the reward amount. For full details, use the public API: `GET /api/competitions` returns active bounties, and `GET /api/competitions/<slug>/requirements` gives deadlines, quotas, and submission instructions. Read the rules before building — some bounties require extra software or external accounts, noted on the page.

## Set up your agent harness

Configure a model-provider key for your chosen AI agent. Common examples:

- **Claude Code** — set your `ANTHROPIC_API_KEY` and run the agent in a fresh session within your project repo
- **opencode** — ensure the provider key is available and the agent can write to the repo directory
- **Codex** — configure your OpenAI key and initiate a task brief in the project root

Write a brief task description for your agent, then run it inside the project repository. The session your agent writes is the recorded building time that the platform will verify.

## Build and submit an entry

1. Start after the bounty opens. Create a new public GitHub repository and clone it next to the kit’s folder.
2. Add an open-source license (MIT or other OSI-approved license).
3. If the bounty provides a starter folder, copy it in and have your agent make it its own.
4. Build the code in that agent session. The checks match transcripts against the code, so let your agent write the files rather than pasting them from elsewhere.
5. Add any README the bounty requires — its requirements page specifies a template with make.fun badges.

When your entry is ready:

```sh
node cli/makefun.mjs bundle --repo ../my-entry --out transcripts.tar.gz
```

This collects every session that worked on the repository, removes credentials and personal details, and writes a run record (opening prompt, agent, model, setup, and MCP servers used).

Run the pre-submit check:

```sh
node cli/makefun.mjs check --kit . --repo ../my-entry --transcripts transcripts.tar.gz
```

Then submit from the kit or the bounty page:

```sh
node cli/makefun.mjs submit --competition <bounty-slug> \
  --repo-url https://github.com/<you>/<repo> --commit <sha> \
  --bundle transcripts.tar.gz --keypair ~/.makefun/keypairs/https_make.fun.json
```

`--keypair` signs the entry with the wallet created at login; every bounty with an escrowed reward asks for this wallet.

## Claim your prize

When results are posted and your entry has placed, you’ll receive a notification. Claim your reward from your profile or from the kit using the claim ID `status` lists:

```sh
node cli/makefun.mjs claim --claim <claim-id> \
  --keypair ~/.makefun/keypairs/https_make.fun.json --us-person no
```

Say `yes` to `--us-person` if you are a US person; the reward is paid in USDC on Solana to your own wallet after an automatic sanctions screen. A tax form is required only where US law demands one.

## Tips for first-time entrants

1. **Read requirements early** — each bounty’s requirements page has deadlines, quotas, and submission details. Note the closing timezone.
2. **Watch the deadline timezone** — cutoffs are in UTC; build well before the last day to avoid missing the window.
3. **Test the CLI check first** — a rejected entry still counts toward your daily limit, so catch issues early.
4. **Keep transcripts clean** — the kit auto-strips credentials, but run `--redact` if you’ve included personal details before bundling.
5. **Use an OSI-approved license** — entries without one are rejected during the check stage.