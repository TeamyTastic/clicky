# Pulling upstream updates from farzaa/clicky

This repo (`Camekazi/clicky`) is a personal fork of [farzaa/clicky](https://github.com/farzaa/clicky).
It has diverged significantly — custom Cloudflare Worker proxy, AssemblyAI streaming, ElevenLabs TTS,
multi-monitor cursor overlay, etc. Upstream merges require careful conflict resolution.

## Remotes

```
origin    https://github.com/Camekazi/clicky.git   (your fork)
upstream  https://github.com/farzaa/clicky.git     (farzaa's original)
```

## How to pull an upstream update

1. **Fetch upstream**
   ```bash
   git fetch upstream
   ```

2. **Review what changed upstream**
   ```bash
   git log upstream/main..main --oneline          # your commits not in upstream
   git log main..upstream/main --oneline          # upstream commits not in yours
   git diff main upstream/main -- <file>          # diff a specific file
   ```

3. **Cherry-pick specific commits** (recommended)
   If upstream fixed a bug or added a feature you want, cherry-pick individual commits
   rather than merging the whole branch — your architecture is different enough that
   a full merge will produce more conflicts than value.
   ```bash
   git cherry-pick <commit-sha>
   ```

4. **Or merge and resolve** (for larger updates)
   ```bash
   git merge upstream/main
   # resolve conflicts in Xcode or your editor
   git add .
   git commit
   git push
   ```

## What to watch out for

- **ClaudeAPI.swift** — upstream may use direct Anthropic API calls; your version routes
  through the Cloudflare Worker proxy. Don't let upstream overwrite this.
- **ElevenLabsTTSClient.swift** — same: proxied via Worker, not direct.
- **AssemblyAIStreamingTranscriptionProvider.swift** — your version uses a shared URLSession
  and token-fetch pattern; upstream may differ.
- **worker/** — entirely yours; upstream doesn't have this directory.
- **Info.plist / AppBundleConfiguration.swift** — your proxy URL and config keys live here.

## Checking for upstream changes periodically

```bash
git fetch upstream && git log main..upstream/main --oneline
```

If output is empty, you're up to date. If there are commits, read them before deciding
whether to pull anything in.
