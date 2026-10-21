---
description: Build and deploy this project to Vercel production, then report the live URL
allowed-tools: PowerShell, Bash, Read, Write
---

Deploy this Vite + React project to Vercel production and give me the public URL.

Steps:

1. **Check login.** Run `npx --yes vercel@latest whoami`. If it says logged out, stop and tell me to run `! npx vercel login` myself (it is interactive), then re-run `/deploy_vercel`.

2. **Check config.** Make sure `vercel.json` exists at the project root with `"outputDirectory": "build"` (this project builds to `./build`, not `./dist`). Create it if missing:
   ```json
   {
     "framework": "vite",
     "buildCommand": "npm run build",
     "outputDirectory": "build"
   }
   ```

3. **Verify the build locally.** Run `npm run build`. If it fails, show me the error and stop. Do not deploy a broken build.

4. **Deploy.** Run `npx --yes vercel@latest deploy --prod --yes`. On first run this creates and links a Vercel project (writes `.vercel/`); make sure `.vercel` is in `.gitignore`.

5. **Report.** From the CLI output, give me:
   - The **production URL** (the stable `https://<project>.vercel.app` alias); this is the one to share
   - The unique deployment URL
   - If the deploy failed, the relevant error lines and a suggested fix

$ARGUMENTS
