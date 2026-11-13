# Video skills for Claude and Codex

Twelve reusable skills for planning, capturing, and finishing videos. The UI
and branding workflows are included alongside the general production skills.

| Skill | Use it for |
|---|---|
| `creator-video` | A person presenting or demonstrating a product on camera |
| `feature-video` | A short video of a real software feature |
| `ui-component-clip` | A seamless loop of one interface component |
| `silent-product-demo` | A designed UI demo without narration |
| `ui-tutorial-video` | A step-by-step tutorial in the real interface |
| `narrated-product-film` | A voice-led film with visual proof for each line |
| `brand-film` | A narrative launch or campaign film |
| `walkie-voiceover` | A voiceover for any of these films, narrated by the user's own Walkie app |
| `brand-grid-video` | A moving grid of UI and brand elements |
| `video-brand-system` | Shared color, type, motion, sound, and framing rules |
| `video-thumbnails` | Still covers and short animated loops |
| `social-video-hooks` | Openings, scripts, and timed beat sheets for social clips |

Each folder contains a standard `SKILL.md` file with `name` and `description` frontmatter. These are guidance skills: they use the capture, editing, and rendering tools available in the user's environment. They do not contain a rendering app, API integration, or credentials.

## Give a film a voice

`walkie-voiceover` pairs with `brand-film` and `narrated-product-film`. Ask your assistant to make the film and give it a voice: it writes the script, asks [Walkie](https://trywalkie.com) on your computer to narrate it, and cuts the picture to the audio file that comes back. It needs the Walkie app open and signed in on a plan that includes assistant connections (Walkie Work or above), Walkie connected to your assistant ([setup guide](https://trywalkie.com/mcp/setup)), and the setting "Let your assistant add recordings and narrate" turned on in Walkie's Settings, under MCP. Narration counts toward your Read Aloud time, and the voice is AI-generated, so say so where you publish. You can hear the voices at [trywalkie.com/mcp/voices](https://trywalkie.com/mcp/voices). If Walkie is not connected, the skill tells you what to switch on.

## Install

Install every skill globally for both Claude Code and Codex with one command:

```bash
npx --yes skills add adamperlis/video-skills -g -a claude-code -a codex -s '*' -y
```

This uses the open `skills` CLI through npm; no npm package for this repository is needed. To install manually instead, clone the repository and copy the folders:

```bash
git clone https://github.com/adamperlis/video-skills.git
mkdir -p ~/.claude/skills ~/.codex/skills
cp -R video-skills/skills/. ~/.claude/skills/
cp -R video-skills/skills/. ~/.codex/skills/
```

Copy only to the assistant you use if you do not need both. Restart the assistant or start a new chat if the new skills do not appear immediately. Invoke a skill by name, or describe the task and let the assistant select it.

## Publication review

This bundle was rewritten from internal video workflows. The `ui-component-clip`, `silent-product-demo`, `ui-tutorial-video`, `brand-film`, `narrated-product-film`, and `video-brand-system` skills preserve the reusable methods from the product-specific recipes. The `brand-grid-video` skill comes from the grid workflow in `feature-video`. It excludes project-specific studios, source footage, brand assets, Drive paths, private URLs, and API credentials.

This standalone bundle is published under the MIT license.
