# Setup Guide

This profile is built from real GitHub-rendering constraints, not assumptions — a couple of things
are worth understanding before you deploy it.

## Why it's built this way

GitHub strips inline `style=""` attributes and `<style>` blocks from README HTML for security. True
CSS glassmorphism (`backdrop-filter`, layered `rgba` blur) cannot run live inside a README. So the
"glass card" hero, the animated dividers, the footer wave, and the patent seal are all **self-contained
SVG images** — animation and blur effects are built with SMIL (`<animate>`) and SVG filters inside the
SVG file itself, which *do* render, because the browser loads them as an image resource rather than
running them through GitHub's HTML sanitizer.

The About / Experience / Achievements / Projects / Tech Stack sections use tables, blockquote accents,
and colour-matched shields.io badges — real, supported GitHub markdown — to keep the same premium
language without relying on CSS that would be silently stripped.

## 1. Repository structure

Create a repository with the **exact same name as your GitHub username** (this is what makes a
README show up on your profile page). Then drop these files in exactly this structure:

```
YOUR_GITHUB_USERNAME/
├── README.md
├── SETUP.md
├── assets/
│   ├── banner.svg
│   ├── divider.svg
│   ├── footer-wave.svg
│   ├── patent-badge.svg
│   └── snake-dark.svg        ← generated automatically by the workflow, don't create manually
└── .github/
    └── workflows/
        └── snake.yml
```

## 2. Installation steps

1. Go to `https://github.com/new` and create a public repository named exactly `YOUR_GITHUB_USERNAME`
   (e.g. if your handle is `kanishkgupta`, the repo is named `kanishkgupta`).
2. Upload every file from this package into that repository, preserving the folder structure above.
3. Go to **Settings → Actions → General** in that repo and set workflow permissions to
   **"Read and write permissions"** — the snake workflow needs this to publish the generated SVG.
4. Go to the **Actions** tab and manually run **"Generate Contribution Snake"** once
   (`Run workflow`). It will also run automatically every day at midnight UTC afterward.
5. Open `README.md` and replace every placeholder listed below with your real information.
6. Commit. Your profile page (`github.com/YOUR_GITHUB_USERNAME`) now shows the README.

## 3. Placeholder checklist (find & replace)

| Placeholder | Replace with |
|---|---|
| `YOUR_GITHUB_USERNAME` | Your GitHub handle (appears in badge URLs, stats APIs, repo links) |
| `YOUR_EMAIL@example.com` | Your contact email |
| `YOUR_LINKEDIN_HANDLE` | Your LinkedIn username |
| `YOUR_PORTFOLIO_URL` | Your personal site (remove the badge entirely if you don't have one) |
| `your-demo-link.com` | Each project's live demo URL (or delete the "Live Demo" badge if unhosted) |
| `YOUR_WAKATIME_USER_ID` | Your WakaTime user ID (see step below) |

## 4. Optional external services

These are real, widely-used, free services — none require you to trust a third party with credentials
beyond what's described:

- **GitHub Readme Stats / Streak Stats / Activity Graph / Trophies** — work immediately once you swap
  in your username. No account needed beyond GitHub itself.
- **Contribution Snake** — handled by the included workflow. Nothing else to configure beyond step 4 above.
- **WakaTime Coding Activity** — create a free account at [wakatime.com](https://wakatime.com), install
  the editor plugin for VS Code / JetBrains, code for a day so it collects data, then copy your badge
  from **wakatime.com → your profile → the "Embed" / badge section**. Paste that badge URL in place of
  the WakaTime line in the Analytics section.
- **Spotify Now Playing** *(optional, not included by default)* — deploy
  [kittinan/spotify-github-profile](https://github.com/kittinan/spotify-github-profile) to Vercel with
  your own Spotify app credentials, then add the resulting image URL under the Analytics section.
- **Visitor Counter** — powered by `komarev.com/ghpvc`, already wired to your username placeholder,
  no setup required.
- **Developer quote** — powered by `readme-typing-svg`, no setup required.

## 5. Customizing the palette

All custom SVGs use this token set — change the hex values inside `assets/*.svg` if you want to shift
the theme:

| Token | Hex | Used for |
|---|---|---|
| Void (background) | `#05070D` | Base background |
| Navy | `#0B1220` | Secondary background / card fill |
| Violet | `#8B5CF6` | Primary accent |
| Blue | `#3B82F6` | Secondary accent |
| Cyan | `#22D3EE` | Highlight / glow |
| Muted text | `#94A3B8` | Captions, secondary text |
| Primary text | `#F1F5F9` | Headlines |

Shields.io badges reuse the same hex values via their `color=` and `logoColor=` query parameters, so
the palette stays consistent between the custom SVGs and the markdown-based sections.

## 6. Updating content later

- **Projects** — duplicate one `<td>` project card block in `README.md` and edit the text/links.
- **Experience** — duplicate a timeline `<tr>` block.
- **Patent / Achievements** — these are static; edit the text directly, no external service involved.
