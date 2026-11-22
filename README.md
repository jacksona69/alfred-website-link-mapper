# Website Link Mapper for Alfred 6

**v0.3.10** · macOS · MIT licence

An interactive website crawler and internal link visualiser for Alfred 6. Explore how pages connect, investigate incoming and outgoing links, identify poorly connected content and spot potential SEO issues.

<img width="1760" height="1041" alt="link-map" src="https://github.com/user-attachments/assets/5a4c798d-5e7b-4f17-ab46-ae5a350fb83c" />

## Requirements

- macOS 12 Monterey or later and [**Alfred 6 with the Powerpack**](https://www.alfredapp.com/help/workflows/).
- Python 3.9 or later. If you need Python, use the macOS installer from [**python.org**](https://www.python.org/downloads/macos/) and keep its default command-line tools and symlink options. Open **Install Certificates.command** in the installed Python folder under **Applications** so HTTPS requests can verify certificates. Run `python3 --version` in Terminal to check. The workflow also detects Homebrew Python at the standard Apple Silicon and Intel paths. It uses the Python standard library, so there are no `pip` packages to install.
- Safari or another modern default browser with JavaScript enabled, plus access to the site you want to crawl. The report itself opens from disk.

## Install and run

1. Download `Website-Link-Mapper-v0.3.10.alfredworkflow` from the release assets.
2. Double-click the file and choose **Import** in Alfred.
3. Open Alfred, type `linkmap` and enter a starting URL. Scheme-less input uses HTTPS.
4. Set **Maximum depth** and **Page limit**, or keep the defaults of 10 clicks and 350 page requests. Click **Start crawl**.
5. When the crawl finishes, click **Open map**. Use **Saved crawls** to reopen an earlier result.

## Explore the map

| View | What it shows |
| --- | --- |
| **Crawl depth** | Places pages by their shortest discovered click distance from the starting URL and shows discovery links and connections around the selected page. |
| **All links** | Shows every recorded connection between the visible nodes, subject to the filters. |
| **Focus on page** | Shows only the selected page and its direct connections, with up to 12 neighbours in each displayed group. The sidebar lists the matching neighbours even if the graph omits some. |

- Search by page title or URL. Select a node or a sidebar entry to follow its connections. The details show the HTTP status, crawl depth, canonical URL, redirects and counts of distinct linking and linked URLs.
- Choose **Incoming only**, **Outgoing only** or **Both directions**. The sidebar shows the source or target URL, anchor text and placement for each connection. Repeated occurrences can share one arrow.
- **Link placement** distinguishes recognised content, navigation, header and footer links. **Unclassified** means the HTML offered no recognised placement. **Exclude menus, header & footer** includes unclassified links.
- Switch on **Include unverified targets** to see targets left unchecked because of scope limits, robots.txt or recognised asset extensions. Search can also select a recorded target directly.
- The selected node uses a green outline. Blue, orange and purple identify incoming, outgoing and reciprocal links around it. The small dots indicate response states, including HTML pages in green, redirects in amber and errors in red. Under an active filter, greyed-out nodes give site context and still accept clicks.
- Drag a node to reposition it or drag the background to pan. Use **+**, **−** and **Fit** to change the zoom. The browser can remember the view, filters and node positions for each saved result when it permits local storage for the report file.

## Review SEO findings

Open **Issues** in the sidebar. **Observed responses** includes HTTP 404 and 410 link targets, links via redirects, redirect chains, loops and links that end at a 404 or 410 after a redirect. Choose a finding and expand its source pages to see which links led to it. A recorded redirect can be intentional.

**Review flags** identifies pages with one or fewer incoming source pages, or one or fewer recognised content-link sources, pages at least three clicks deep, pages with at least 50 distinct outgoing HTML destinations, fetch problems, and sitemap URLs absent from the recorded link crawl. Check these in context. Unclassified links may appear within the main content, and missed sitemap URLs can result from crawl limits, blocked access or JavaScript navigation.

<img width="725" height="690" alt="seo-issues" src="https://github.com/user-attachments/assets/1ca7a3ed-b9e1-4a22-a717-7173aa64898a" />

## Downloads and saved results

- **Download crawl JSON** saves the report data. **Export SVG** saves the displayed graph. **View crawl errors** opens the details when a page, robots.txt or sitemap request reports an error.
- Alfred saves crawls in dated folders under `~/Library/Application Support/Website Link Mapper/Crawls/`. Click **Open results folder** in Alfred to find the HTML map, `crawl.json` and `links.csv`. The picker shows up to 50 successful results.
- **Keep saved crawls** applies across sites. The default `0` keeps all results. Set a limit to move excess eligible result folders to the macOS Trash after a successful crawl. This option uses the system `/usr/bin/trash` if available. The workflow can still crawl and save results on Macs without it.

## Crawl behaviour and limitations

- The crawler follows HTTP and HTTPS hyperlinks on the starting host and port. Other hosts and ports sit outside the crawl, including the `www` variant if you start at the bare domain. Start at the final host if the starting address redirects across hosts. Redirects within scope cost a request and add no click to crawl depth. Identical normalised URLs are fetched once. URL fragments are removed from fetch keys. Path case, trailing slashes and query strings can identify distinct URLs.
- The crawl checks robots.txt per origin and applies matching rules, including wildcard rules and crawl delays. A 404 or 410 for robots.txt allows crawling. If access to the rules fails in another way, the crawler records the error and skips that origin. The workflow spaces request starts by at least a quarter second per origin, uses a 12-second request deadline, and stops at the depth and page-request limits you set. Robots and sitemap requests sit outside the page budget. They have separate bounds.
- Canonical URLs found in HTML are recorded. The crawler does not treat them as navigational links or merge pages automatically. Recognised asset links may appear as targets without a fetch. Links identified as Cloudflare email obfuscation are excluded.
- The report compares links with same-host URLs in the XML sitemaps declared in robots.txt, or at `/sitemap.xml` if no sitemap was declared. It reads up to 32 sitemap or index files. Sitemap entries are an inventory for comparison and do not expand the page crawl.
- The crawler reads server-returned HTML. Links created after page load by JavaScript, pages requiring sign-in, and pages behind anti-bot challenges can be missing or incomplete. Large crawls can take time to fetch and display. Only start a crawl on a site you are authorised to crawl.

## Report an issue or contribute

Open a project issue with your macOS, Alfred and Python versions, the crawl limits, the affected URL if public, and steps to reproduce. Share only the relevant part of a report after checking it for private URLs and query values.

Workflow source and assets live in `src/`. Run `python3 -S src/test_crawler.py` for the recorded-response checks. The workflow archive uses `src/` as its root, with `info.plist`, `icon.png`, the crawler, the viewer and `LinkMapper.alfredui/` at the top level. Keep `LinkMapper.alfredui/bridge` executable. The test script belongs in the repository source.

## Author

**Created by Adam Jackson** · [**jaxmore.com**](https://jaxmore.com/)

The source is available under the [**MIT licence**](LICENSE).
