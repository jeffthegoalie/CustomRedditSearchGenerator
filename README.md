# Reddit Feed Search Builder

A free, static, mobile-friendly tool that generates Reddit post searches using **per-subreddit flair and/or title allowlists**, optional exclusions, and **New** sorting.

**Live site:** After enabling GitHub Pages, your URL will be `https://YOUR-USERNAME.github.io/YOUR-REPOSITORY/`.

## Features

- Any number of subreddits, each with its own rules.
- Include posts by **flair** or **title** phrase. Combine inclusion rules with **Any (OR)** or **All (AND)** within each subreddit.
- Exclude additional flairs or title phrases.
- Different rules for different subreddits; subreddit groups are joined with **OR**.
- Builds a Reddit search URL with `type=posts&sort=new`.
- Preview the query and copy the query or URL.
- Responsive design for desktop and phone; follows device light/dark mode.
- **No API key, Reddit authentication, database, dependencies, cookies, tracking, local storage, or saved user configuration.** Forms start blank on every fresh page load.

## Publish free with GitHub Pages

1. Sign into GitHub and create a **public** repository, for example `reddit-feed-builder`.
2. Upload `index.html` and `README.md` into the repository **root** (not inside another folder), or commit/push them with Git.
3. Open the repository's **Settings > Pages**.
4. Under **Build and deployment**, choose **Deploy from a branch**.
5. Select branch **main**, folder **/(root)**, then **Save**.
6. When published, open `https://YOUR-USERNAME.github.io/reddit-feed-builder/` (adjust for your actual repo name).
7. On iPhone, open the site in Safari; optionally use **Share > Add to Home Screen** for quicker access.

No build command, hosting bill, or GitHub Actions workflow is required.

## Use

1. Enter a subreddit name, e.g. `hockey` or `r/hockey`.
2. Add Include rules such as `Flair = News`, `Flair = Discussion`, or `Title = goalie`. Enter **one word or phrase per rule**.
3. Select **Any (OR)** to match any included rule, or **All (AND)** to require every included rule within that subreddit.
4. Optionally add Exclude rules, e.g. `Title = giveaway`. Exclusions are joined with OR and negated as a group.
5. Add more subreddits, each with their own conditions.
6. Use **Open search**. Reddit opens in a new tab with sorting set to **New**.

### Example

- `r/hockey`: Include flair `News` OR flair `Discussion`
- `r/hockeygoalies`: Include title `Warrior` OR title `Bauer`; exclude title `sale`

Produces a grouped query along the lines of:

```text
(subreddit:hockey AND (flair:"News" OR flair:"Discussion")) OR (subreddit:hockeygoalies AND (title:"Warrior" OR title:"Bauer") NOT (title:"sale"))
```

## Privacy and session behavior

This application contains no persistence code. No configuration is remembered on page refresh or a later visit. Entries exist only in the open page's memory while in use. The only external destination for a generated search is Reddit when you click **Open search**. Copying and bookmarking a generated **Reddit search URL** preserves rules in the URL itself, not in this builder. Like other public GitHub Pages sites, the page and its source code are publicly accessible.

## Important limitations

- This is **not** a Reddit feed reader and does **not** query Reddit's API or display Reddit posts directly.
- Reddit's indexing and Boolean search implementation can omit or inconsistently match results, especially for complex queries. `sort=new` asks Reddit for newest-first results but does not guarantee exhaustive, instant coverage.
- Search field phrases follow Reddit's documented syntax. Test multiword flair/title values against actual search results.
- Reddit may apply its account and Safe Search settings to results.
- Some mobile browsers or Reddit app deep links may handle search URLs differently. Safari and desktop browser links are the most predictable.

## Editing

Everything is in `index.html`: HTML, CSS, and vanilla JavaScript. No packages, bundlers, install steps, or ongoing hosting service integration. Edit the file, commit to GitHub, and Pages redeploys.

## References

- [Reddit search operators and Boolean grouping](https://support.reddithelp.com/hc/en-us/articles/19696541895316-Available-search-features)
- [Reddit search sorting](https://support.reddithelp.com/hc/en-us/articles/19695706914196-What-filters-and-sorts-are-available)
- [GitHub Pages quickstart](https://docs.github.com/en/pages/quickstart)

**Unofficial project. Not affiliated with Reddit.**
