# Reddit Feed Search Builder

A free, static, mobile-friendly tool for building Reddit post searches using **different flair and title filters in each subreddit**. Supports include/exclude rules, Boolean logic, configurable sorting, an optimized search query, and JSON import/export.

- **Website:** https://jeffthegoalie.github.io/CustomeRedditSearchGenerator/
- **Source:** https://github.com/jeffthegoalie/CustomeRedditSearchGenerator
- **License:** [MIT](LICENSE)

This is an independent search-link generator. **It does not use the Reddit API, retrieve Reddit posts, create an account, or retain your rules between visits.**

## What it can do

- Enter multiple subreddits, each with its own **Flair** or **Title** inclusion rules.
- Combine inclusion rules within a subreddit using **Any (OR)** or **All (AND)**.
- Exclude additional flair/title matches using **NOT**.
- Combine all configured subreddit searches with **OR**.
- Choose Reddit's **New** (default), **Relevance**, **Hot**, **Top**, or **Most comments** sort.
- **Optimize** the generated search by removing unnecessary quotes/grouping, deduplicating identical rules, and grouping subreddits that have the same filter conditions.
- Show a **live query-length counter** and warning against Reddit's documented 512-character API search-query limit.
- **Open or copy the combined search**, plus **open or copy individual subreddit search links** for manual testing.
- **Export JSON** and **Import JSON** to transfer your configuration explicitly without saving it in the browser.
- Work on desktop and mobile browsers with light/dark mode support.

## Use

1. Enter a subreddit, for example `hockey` or `r/hockey`.
2. Add **Include** rules. Choose Flair or Title and enter **one word or phrase per rule**. Example: `Flair = News`, `Title = goalie`.
3. Choose **Any (OR)** if any inclusion rule may match, or **All (AND)** if every inclusion rule must match for that subreddit.
4. Add optional **Exclude** rules, such as `Title = giveaway`. All exclusions within a subreddit are applied as `NOT (A OR B …)`.
5. Add more subreddits. They can have different rules or share rules.
6. Select the sort order. Review the **optimized search query** and its character count.
7. Click **Open combined search**, **Copy combined URL**, or use the links under **Test individual subreddit searches**. Reddit handles the actual search results.
8. To reuse the form later, click **Export JSON** and save the file. Next time, **Import JSON** and select it. Alternatively, bookmark the *generated Reddit search URL* to return to that exact query.

### Example: different conditions in different subreddits

- `r/hockey`: allow flair `News` OR flair `Discussion`
- `r/hockeygoalies`: allow titles containing `Warrior` OR `Bauer`; exclude titles containing `sale`

The builder produces a query along these lines:

```text
(subreddit:hockey AND (flair:News OR flair:Discussion)) OR (subreddit:hockeygoalies AND (title:Warrior OR title:Bauer) NOT title:sale)
```

The `sort` selection is added to the **Reddit URL**, not to the search expression.

### Example: automatic query optimization

If `r/hockey` and `r/nhl` both have the *same* conditions (for example, `Flair = News`), the tool groups their subreddit scopes:

```text
(subreddit:hockey OR subreddit:nhl) AND flair:News
```

If those subreddits have **different** rules, they stay in separate groups, preserving their original matching logic. Matching rules entered in different orders can still be grouped. Repeated identical rule terms are deduplicated.

**This optimization is syntax-only.** It does not query Reddit, use AI, remove unique user criteria, or change the intended Boolean logic. Reddit may nevertheless handle complex expressions inconsistently.

## Query length and Reddit limitations

Reddit's [API search reference](https://www.reddit.com/dev/api/) documents a **512-character maximum for the `q` query parameter**. This is not simply a limit on the URL's encoded length. We use it as a **warning threshold**; the regular Reddit website can have different behavior and may also impose other restrictions.

The builder:

- Counts the characters of the **optimized** expression as you type.
- Shows approximately how many characters were removed by optimization.
- Warns when the query passes 512 characters.
- **Does not automatically split the combined query**. This is intentional: automatically splitting would create separate results that are not merged into one newest-first feed.
- Allows you to **try opening or copying an over-limit search anyway**, so you can test how Reddit handles it.
- Provides **one search link per configured subreddit**, subject to that subreddit's own query complexity. These links are explicitly for manual testing, **not an automatic split or a merged feed**.

To shorten an overlong search, remove less important rules, reduce the number of different rule groups, or give several subreddits identical criteria where appropriate. A long expression cannot be made valid by URL encoding alone.

Reddit's search indexing is not guaranteed to be exhaustive or immediate. Sorting by **New** requests newest-first *search results* but doesn't guarantee every new matching post will appear. Multiword phrases and Boolean expressions may behave differently across Reddit's web and mobile interfaces.

## JSON import/export and privacy

The exported file uses the **existing version 1 format**, compatible with exported configurations from the previous version of this builder:

```json
{
  "format": "reddit-feed-search-builder",
  "version": 1,
  "sort": "new",
  "subreddits": [
    {
      "name": "hockey",
      "logic": "ANY",
      "rules": [
        { "field": "flair", "mode": "include", "term": "News" }
      ]
    }
  ]
}
```

Import confirms before replacing your current form. The file is read locally in the browser; it is **not uploaded** to this site. Imports are validated (file size, version, sort value, and structure). Export generates a JSON download on demand.

**No configuration is saved** in cookies, localStorage, sessionStorage, a database, or a server. Reloading the page resets the form and sets sorting back to **New**. There are no analytics or external JavaScript dependencies. Opening a generated search sends the query to Reddit, where Reddit's policies and account settings apply. An exported JSON file or bookmarked Reddit URL can contain your subreddit and keyword choices, so share those intentionally.

## Publish with GitHub Pages

The GitHub repository root contains only:

```text
index.html
README.md
LICENSE
```

1. Commit `index.html` and `README.md` to the root of [`jeffthegoalie/CustomeRedditSearchGenerator`](https://github.com/jeffthegoalie/CustomeRedditSearchGenerator), replacing the existing versions. Keep `LICENSE` unchanged.
2. In **Settings → Pages**, select **Deploy from a branch**.
3. Set the branch to `main` and directory to `/(root)`.
4. Enable GitHub Actions if your repository displays a message that Actions is disabled; GitHub Pages uses its own deployment workflow. You do **not** need to write a custom workflow or Jekyll configuration.
5. Once deployment succeeds, open https://jeffthegoalie.github.io/CustomeRedditSearchGenerator/.

The entire application is contained in **`index.html`** (HTML, CSS, JavaScript). No build step, API key, GitHub Actions configuration file, package manager, database, or paid hosting is required. `LICENSE` is a separate standard text file for licensing, not a runtime dependency.

## License and credit

Copyright (c) 2026 **jeffthegoalie**. Released under the [MIT License](LICENSE). You may use, modify, and redistribute the code, including commercially, provided the original copyright and license notices are retained as the MIT License requires.

The site's footer links to the original repository and license. This is an **unofficial** project and is not affiliated with or endorsed by Reddit.

## References

- [Reddit: Available search features](https://support.reddithelp.com/hc/en-us/articles/19696541895316-Available-search-features)
- [Reddit: Search API reference and `q` parameter](https://www.reddit.com/dev/api/)
- [GitHub Pages: Publishing from a branch](https://docs.github.com/en/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site)
