# albatesuk-skills

Add-ons for [Claude Code](https://claude.com/claude-code) that teach Claude new jobs. Install the collection once, then pick the add-ons you want.

## What's here

| Add-on | What it does |
| --- | --- |
| [Explainer video](explainer-video) | Give Claude your website and it makes a 30–60 second video explaining your product, with captions, your brand colours and music. |

## How to install

You need Claude Code first. If you don't have it, follow [Anthropic's quickstart guide](https://code.claude.com/docs/en/quickstart).

### The easy way: ask Claude

Open Claude Code and paste this message:

> Please install the explainer-video plugin from the GitHub repository albatesuk/albatesuk-skills.

Claude will run the install for you. Then close Claude Code and open it again.

### Or type it yourself

Open a terminal. On a Mac, that's the **Terminal** app; on Windows, it's **PowerShell**. Then paste these two lines, one at a time, pressing Enter after each:

```bash
claude plugin marketplace add albatesuk/albatesuk-skills
```

```bash
claude plugin install explainer-video@albatesuk-skills
```

Then close Claude Code if it's open, and open it again.

### Or install from a downloaded copy

If you've downloaded this repository with git, open a terminal in the folder that contains it and run:

```bash
claude plugin marketplace add ./albatesuk-skills
```

```bash
claude plugin install explainer-video@albatesuk-skills
```

To update later, run `git pull` inside the `albatesuk-skills` folder, then the update command below.

### Getting updates

To get the latest version, ask Claude to "update the albatesuk-skills plugins", or run:

```bash
claude plugin marketplace update albatesuk-skills
```

### Removing an add-on

```bash
claude plugin uninstall explainer-video@albatesuk-skills
```

## License

MIT
