# agent-search-booster

**English** | [简体中文](README.zh-CN.md)

A Codex Skill for strengthening an agent's search workflow: choosing sources, reading original material, and checking what the evidence supports.

This project collects practical rules for research and fact-checking. Codex uses them to choose available search tools according to the question, follow useful results back to their sources, and explain what was confirmed and what remains uncertain. It is useful for technical questions, recent developments, software documentation, and platform discussions.

The homepage is bilingual. The Skill and detailed reference documents are currently written in Chinese.

## What it helps with

Different information gaps call for different sources:

| Need | Possible sources | What to check |
|---|---|---|
| A current fact or product change | Built-in web search; optional services such as Exa or Tavily | Original source, date, and conditions |
| A result with an incomplete excerpt | Original page or a page-reading tool | Missing sections, tables, footnotes, and dynamic content |
| A library, SDK, or version-specific behavior | Official documentation, source repository; optional Context7 | Version, API behavior, and original examples |
| Posts, video pages, or comments | An available browser or platform-reading tool, such as OpenCLI | Actual text, replies, pagination, and truncation |
| Papers and citations | Academic search tools and publisher sources | Use the appropriate academic workflow; an abstract is not the full paper |

These are optional channels, not an installation checklist. Use the tools that are actually available and authorized in the current session. A question may need one source or several; the Skill does not require a fixed search count or a tour of every service.

Third-party tools need to be configured in your own environment. The Skill supplies instructions for choosing and using them; it does not bundle their software, accounts, or login state.

## An example

Suppose you want to know whether your installed version of a library supports a feature:

```text
Use $agent-search-booster to check whether this feature is supported
in the version we use.

Start with official documentation and release notes. Read relevant user
discussions if they help explain conditions or reported problems.
Give source links, distinguish confirmed behavior from user reports,
and state any unresolved questions or limits on what you could read.
```

A useful answer can explain:

- What the official source confirms, including the version and relevant conditions.
- What users report, with enough context to avoid treating one environment as universal.
- What remains unverified, including incomplete pages or discussion threads.

This is an example of how to report evidence, rather than a mandatory answer template.

## Getting started

1. Download the repository and copy the entire `skills/agent-search-booster/` folder into either your personal `~/.agents/skills/` directory or the project's `.agents/skills/` directory. Choose one location. If a Skill with the same name exists, compare your customizations before replacing it.
2. Mention `$agent-search-booster` in a research task, or let Codex select it based on the request. If the new Skill is not listed, reopen the task and check again. See the [official Skill documentation](https://learn.chatgpt.com/docs/build-skills).
3. Try a small public-information question to check that searching, opening sources, and returning citations work in your environment.

Missing Exa, Tavily, or OpenCLI does not prevent you from starting with an available built-in search or browser tool.

## How sources and task boundaries are handled

Search snippets and model answers are leads. Important conclusions should, where possible, be checked against original pages, documents, or posts, with dates, versions, and applicable conditions. Contested or consequential claims call for independent evidence and counterexamples. Agreement between models is not factual proof.

For paginated or incomplete material, report what was actually read. A list of titles is not a full article, and part of a comment section is not all comments. If a tool fails, make a brief check and try an appropriate alternative. Missing critical evidence should remain unresolved; other useful work can continue.

Tutorials, commands, and configuration examples are normal research material. Content that clearly impersonates higher-priority instructions, hijacks the task, or requests unrelated credential access is handled separately; useful material from the same source can still be examined.

Work follows the current request and existing authorization. The Skill allows relevant public research and necessary, manageable local actions; posting, messaging, account changes, and login operations require appropriate authorization. It reuses existing sessions without proactively reading or saving credentials. Direct credential access requires a stated purpose and permission, and disclosure or storage needs separate consent. Private content and credentials should not be sent as public search terms. Only authorized free or subscription-included usage is used.

## Files and further reading

| File | Purpose |
|---|---|
| [SKILL.md](skills/agent-search-booster/SKILL.md) | Search decisions, evidence checks, tool failures, and task boundaries |
| [Web and document tools](skills/agent-search-booster/references/web-tools.md) | Web discovery, full-text reading, code documentation, and tool selection |
| [Platform material](skills/agent-search-booster/references/platforms.md) | Posts, video pages, transcripts, comments, and coverage limits |

The Skill can be used independently or alongside [subagent-manager](https://github.com/LiX-Works/subagent-manager) when delegation is useful. See [Releases](https://github.com/LiX-Works/agent-search-booster/releases) for published releases, when available. The project uses the [MIT license](LICENSE).
