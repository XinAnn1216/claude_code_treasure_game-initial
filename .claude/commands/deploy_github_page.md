---
description: Push this project to GitHub and deploy it to GitHub Pages, then report the live URL
argument-hint: "[repo-name]"
allowed-tools: PowerShell, Bash, Read, Write, Edit
---

Deploy this Vite + React project to GitHub Pages and give me the public URL.

Repo name: use `$ARGUMENTS` if given, otherwise the existing `origin` repo, otherwise the project folder name.

Steps:

1. **Check tools and login.**
   - If `gh` is not on PATH, use `& "C:\Program Files\GitHub CLI\gh.exe"` (installed via winget). If it is missing entirely, install it with `winget install --id GitHub.cli -e --silent`.
   - Run `gh auth status`. If not logged in, stop and tell me to run `! gh auth login` myself (it is interactive), then re-run `/deploy_github_page`.
   - Get my GitHub username with `gh api user -q .login`.

2. **Make sure the code is on GitHub.**
   - If this is not a git repo, run `git init -b main`. Check `.gitignore` covers `node_modules/`, `build/`, `.vercel/` and `.env*` before staging, and never commit secrets.
   - Commit any pending changes with a short descriptive message.
   - If there is no `origin` remote, create the repo with `gh repo create <repo> --public --source=. --remote=origin --push` (GitHub Pages on a free account requires a public repo). Otherwise `git push origin HEAD`.

3. **Build with the Pages base path.** A GitHub project site is served from `/<repo>/`, so run:
   `npx vite build --base=/<repo>/`
   Pass `--base` on the command line, and don't set it in `vite.config.ts`, so the Vercel deploy (served from `/`) keeps working. If the build fails, show me the error and stop.

4. **Publish the build to the `gh-pages` branch.**
   `npx --yes gh-pages -d build -t -m "Deploy to GitHub Pages"`

5. **Enable GitHub Pages** (skip if already enabled; a 409 error means it already is):
   `gh api -X POST repos/<user>/<repo>/pages -f "source[branch]=gh-pages" -f "source[path]=/"`

6. **Wait for it to go live.** Poll `gh api repos/<user>/<repo>/pages/builds/latest -q .status` every ~15s, for up to about 5 minutes, until it is `built`, then confirm `https://<user>.github.io/<repo>/` returns HTTP 200.

7. **Report.** Give me:
   - The **live URL**: `https://<user>.github.io/<repo>/`
   - The repo URL: `https://github.com/<user>/<repo>`
   - If something failed, the relevant error lines and a suggested fix
