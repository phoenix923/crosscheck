# Crosscheck

Compares what trusted newsrooms report with what is trending on X.

## How it works

1. `sources.json` lists the newsroom RSS feeds and the X trends location.
2. `scripts/build-data.mjs` reads the feeds, groups reports about the same event into stories, matches X trends to them, and writes `data.json`.
3. `index.html` loads `data.json`. If the file is missing it shows demo data.
4. `.github/workflows/update-data.yml` re-runs step 2 every 30 minutes and commits the new `data.json`.

## Set up

1. Create a GitHub repository and upload everything in this folder.
2. Turn on GitHub Pages (Settings, Pages, deploy from the main branch).
3. For X trends, add a repository secret named `X_BEARER_TOKEN` (Settings, Secrets and variables, Actions). Without it the site shows press coverage only.
4. Open the Actions tab and run "Update news data" once by hand. Check the log: each feed prints `ok` or `FAILED`.
5. Replace any feed that fails, and edit `sources.json` to choose your own outlets.

## Run it on your computer

```
npm test
X_BEARER_TOKEN=your_token node scripts/build-data.mjs
```

Needs Node 18 or newer. There are no dependencies to install.

## Known limits

- Story grouping and trend matching use headline keywords, so some matches will be wrong. Tune `minShared` and `minJaccard` in `scripts/lib.mjs`.
- X trends give a total post count, not a history, so live stories have no hourly chart yet.
- Claims are not checked automatically yet, so live stories show no claims section.
