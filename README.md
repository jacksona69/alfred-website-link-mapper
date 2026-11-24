# Website Link Mapper for Alfred 6

MacOS · MIT licence

Crawl a website and see which pages link to each other. Use **Graph** to follow links and inspect their anchor text. Use **Audit** to find pages with few incoming links from page content and export the results to CSV.

<img width="1509" height="857" alt="CleanShot 2026-11-24 at 16 47 08" src="https://github.com/user-attachments/assets/778e85df-22b2-41df-abb0-474e514e615e" />

## Installation

You’ll need macOS 12 Monterey or later, [**Alfred 6 with the Powerpack**](https://www.alfredapp.com/help/workflows/), Python 3.9 or later, and a modern browser with JavaScript enabled.

1. Download `Website-Link-Mapper-v0.5.0.alfredworkflow` from the release assets.
2. Double-click the file and choose **Import** in Alfred.
3. Type `linkmap` in Alfred and enter a starting URL. If you omit the scheme, the crawler uses HTTPS.
4. Set **Maximum depth** and **Page limit**, or keep the defaults of 10 clicks and 350 page requests. Click **Start crawl**. A successful crawl opens the report in your default browser.

Run `python3 --version` in Terminal to check Python. The workflow detects standard Homebrew installations and uses Python’s standard library. If you install Python from [**python.org**](https://www.python.org/downloads/macos/), keep the default command-line tool and symlink options and run **Install Certificates.command** from the Python folder in **Applications**.

## Graph

| View | What it shows |
| --- | --- |
| **Crawl depth** | Pages grouped by the shortest click distance discovered from the starting URL, with discovery links and connections around the selected page. |
| **All links** | Every recorded connection between visible nodes that matches the filters. |
| **Focus on page** | The selected page and up to 12 direct neighbours in each displayed group. The sidebar lists the remaining matches. |

Search by title or URL, then select a page to inspect its HTTP status, crawl depth, canonical URL, redirects and links. Switch between **Incoming only**, **Outgoing only** and **Both directions**. The sidebar lists anchor text and link placement. Blue marks incoming links, orange marks outgoing links and purple marks reciprocal links.

Use **Link placement** to distinguish Content, Navigation, Header, Footer and Unclassified links present in the crawl. Choose **Content** to inspect recognised contextual links, or **Exclude menus, header & footer** to include Unclassified links as well. Unclassified links may still appear in page content.

Drag the background to pan and drag a node to move it. Use **+**, **−** and **Fit** to change the zoom. Choose **Export SVG** to save the graph.

## Audit

Select **Audit** in the report header. The table starts with HTML pages and places those with the fewest recognised incoming Content links first. It uses this order:

1. **Incoming Content**, lowest first.
2. **Outgoing Content**, lowest first when incoming counts tie.
3. **Page title**, A–Z ignoring case when both counts tie.

Click a column heading to sort by that column. Click again to reverse the order. **Reset filters** clears your filters and restores the default sort. Use **Columns** to choose which fields to display.

Search by title or URL. Use **Filters** to narrow by crawl depth, status, link counts or recorded issues, or to find pages with zero incoming Content links. The zero-Content filter follows the active placement and target settings. Choose **All placements** to check for zero incoming Content links across the current target scope.

Use **Show in Graph** to open a page with the current Graph filters. Audit retains its search, sort, filters, visible columns and scroll position when you switch views within the open report. **Export CSV** uses the visible data columns and includes every matching row in the current sort order, with titles and URLs in separate fields.

### How link counts work

- **Incoming** and **Outgoing** count distinct source and destination URLs under the active placement and target filters. Graph and Audit agree when those filters match.
- If page A links to page B three times, the overall count is one URL pair. That pair can count once under Content and once under Unclassified if it contains links of both types. The Graph sidebar also shows the number of matching link occurrences.
- Self-links stay in the crawl data and are excluded from relationship counts. Redirect URLs count as separate records.
- By default, relationships can include HTML, redirects and error records. Turn on **Include unverified targets** to also count records such as skipped assets and pages blocked by robots.txt or crawl limits. In Audit, select **All recorded types** to see those records in the table.

## SEO findings and saved crawls

In Graph, open **Issues** to inspect broken links, redirects, chains and loops. **Review flags** highlights pages with few incoming or Content links, pages three or more clicks deep, pages with at least 50 outgoing HTML destinations, fetch problems and sitemap URLs absent from the link crawl. Issue counts use the full crawl. **View crawl errors** shows page, robots.txt and sitemap request errors when present.

Use **Saved crawls → Open map** in Alfred to reopen a report. The current viewer can open an older crawl with Audit while preserving the original files and link classifications. **Open results folder** takes you to the dated crawl folder under `~/Library/Application Support/Website Link Mapper/Crawls/`. It contains the HTML map, `crawl.json` and the raw link list, `links.csv`. **Download crawl JSON** works in both report views.

**Keep saved crawls** sets a limit across all sites. The default of `0` keeps every crawl. After a successful crawl, the workflow moves excess eligible results to the macOS Trash if `/usr/bin/trash` is available.

## Crawl scope

The crawler follows HTTP and HTTPS links on the starting host and port. Start at the final address if the site redirects to a different host, such as a `www` variant. The crawler respects robots.txt and your crawl limits, records canonical URLs, and compares discovered URLs with same-host XML sitemap entries. Canonical URLs do not merge page records. Redirects on the same host count as requests and add no click to the depth. Recognised asset URLs can appear without a request.

The crawler reads server-returned HTML. Links added by JavaScript, pages behind sign-in and pages behind anti-bot checks may be missing. Only crawl sites you’re authorised to crawl.

## Support

When reporting an issue, include your macOS, Alfred and Python versions, the crawl limits and steps to reproduce it. Check any shared crawl data for private URLs and query values.

**Created by Adam Jackson** · [**jaxmore.com**](https://jaxmore.com/) · [**MIT licence**](LICENSE)
