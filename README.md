# Website Link Mapper for Alfred 6

macOS · MIT licence

Crawl a website, see which pages link to each other and identify opportunities to improve your internal linking.

Use **Graph** to follow links and inspect their anchor text. Use **Audit** to find pages with few incoming links from page content. Use **Suggestions** to identify potential new contextual linking opportunities using local text analysis, without AI, API keys or external analysis services.

<img width="1506" height="858" alt="CleanShot 2026-11-28 at 10 50 59" src="https://github.com/user-attachments/assets/36e0254a-35af-4784-8659-48717e854c04" />

## Installation

You’ll need macOS 12 Monterey or later, [**Alfred 6 with the Powerpack**](https://www.alfredapp.com/help/workflows/), Python 3.9 or later, and a modern browser with JavaScript enabled.

1. Download `Website-Link-Mapper-v0.6.0.alfredworkflow` from the release assets.
2. Double-click the file and choose **Import** in Alfred. If you already have Website Link Mapper installed, this upgrades your existing workflow.
3. Type `linkmap` in Alfred and enter a starting URL. If you omit the scheme, the crawler uses HTTPS.
4. Set **Maximum depth** and **Page limit**, or keep the defaults of 10 clicks and 350 page requests. Click **Start crawl**. A successful crawl opens the report in your default browser.

Run `python3 --version` in Terminal to check Python. The workflow detects standard Homebrew installations and uses Python’s standard library. If you install Python from [**python.org**](https://www.python.org/downloads/macos/), keep the default command-line tool and symlink options and run **Install Certificates.command** from the Python folder in **Applications**.

Existing saved crawls remain compatible with Graph and Audit. To use Suggestions with a crawl from an earlier version, run a fresh crawl to collect the additional page content required for contextual link analysis.

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

## Suggestions

Select **Suggestions** in the report header and click **Generate Suggestions** to identify potential contextual internal linking opportunities.

Website Link Mapper analyses page titles, headings, editorial content, distinctive phrases and existing internal links to identify related pages and suitable passages where an additional link could help readers.

All analysis takes place locally on your Mac. The workflow uses traditional text analysis techniques and requires no AI, API keys, subscriptions or external analysis services.

### Suggested Links and Connections Requiring Review

The analysis produces two separate categories of results.

| Category | What it shows |
| --- | --- |
| **Suggested Links** | Potential opportunities to add a contextual link between related pages, with supporting evidence and a suggested source passage. |
| **Connections Requiring Review** | Existing links whose editorial role the crawler could not confidently establish. These links may already provide an appropriate connection between the pages. |

The direction of each relationship is significant. A link from page A to page B does not prevent the workflow from identifying a suitable opportunity to link from page B to page A.

Existing navigation, header and footer links do not necessarily prevent a contextual link suggestion. The analysis checks for recognised contextual connections and separates uncertain existing connections for review.

### Reviewing a suggestion

Each suggested link identifies the source page, destination page, shared subject and relevant source section.

Click **Details** to inspect the supporting evidence, including:

- **Why these pages were matched:** An explanation of the terminology or phrases connecting the pages.
- **Where to consider a link:** The relevant source heading and an excerpt from the actual page content, with significant matching phrases highlighted.
- **What the destination covers:** The destination title, meta description and relevant content excerpt, where available.
- **Existing relationship:** Information about any existing links between the pages.
- **Pages linking contextually to this destination:** The number of distinct pages with recognised contextual links to the destination.
- **Technical details:** Relevance scores, priority scores and supporting analysis.

Use **Open source page** and **Open destination page** to inspect the live website. Use **Show source in Graph** or **Show destination in Graph** to inspect either page within the wider internal linking structure.

The workflow identifies existing passages for consideration. It does not generate replacement paragraphs, invent anchor text or modify your website.

### Reviewing existing connections

Open **Connections Requiring Review** to inspect existing links whose editorial role remains uncertain.

The expanded details identify the existing anchor text, recorded classification, location and surrounding content, where available. They also explain why the analysis considers the pages related.

Check the existing link in context before considering another. The link may already serve an appropriate purpose.

A connection requiring review does not necessarily indicate a missing link, classification error or SEO problem.

### Editorial restraint

Adding more internal links does not automatically improve a website.

Website Link Mapper applies several safeguards to avoid excessive or repetitive recommendations:

- Excludes existing recognised contextual connections.
- Separates uncertain existing connections for review.
- Considers the number and proximity of existing links.
- Avoids recommending multiple additional links within the same paragraph or editorial content block.
- Considers alternative placements where appropriate.
- Filters repeated promotional content and article teasers.
- Requires supporting textual evidence.
- Stops when no suitable opportunities remain.

A website may produce very few suggestions or none at all. This is a valid outcome.

### Filtering and excluding pages

Search by page title or URL, filter by source or destination page, or change the sort order to review the results.

Use **Exclude pages from analysis** to omit individual pages from recommendation generation. This can be useful for pages where you have no intention of adding contextual links or encouraging additional incoming links.

Exclusions apply to both source and destination pages. Existing links involving excluded pages remain part of the crawl evidence, and the pages remain available in Graph and Audit.

Click **Regenerate Suggestions** to apply changes to your exclusions.

Search, filtering and sorting allow you to review existing results without repeating the analysis.

### Exporting suggestions

Choose **Export CSV** to save the current result category and matching filtered results.

The export includes the supporting recommendation evidence and technical information, allowing you to review opportunities outside the interactive report or share them with website editors and colleagues.

Suggested Links and Connections Requiring Review have separate exports.

### About these suggestions

Recommendations use automated text analysis and your website's existing internal link structure. They may include unsuitable opportunities or overlook relevant connections.

Always review suggestions in context and add links where they provide genuine value to your readers. Relevance scores indicate textual similarity and do not predict SEO benefits.

A relevance score is a relative measure of textual similarity between pages. It does not represent a probability, an SEO rating or the expected benefit of adding a link.

Related pages may use different terminology, while unrelated pages may share similar words. The analysis cannot replace human editorial judgement.

Suggestions use the content captured during the crawl. If you subsequently change your website, run a fresh crawl before generating updated recommendations.

Suggested links do not appear as existing connections in Graph or contribute to Audit link counts.

## SEO findings and saved crawls

In Graph, open **Issues** to inspect broken links, redirects, chains and loops. **Review flags** highlights pages with few incoming or Content links, pages three or more clicks deep, pages with at least 50 outgoing HTML destinations, fetch problems and sitemap URLs absent from the link crawl. Issue counts use the full crawl. **View crawl errors** shows page, robots.txt and sitemap request errors when present.

Use **Saved crawls → Open map** in Alfred to reopen a report. The current viewer can open older crawls with Graph and Audit while preserving the original files and link classifications.

Crawls from v0.6.0 onwards collect the additional editorial content required for Suggestions. Older crawls require a fresh crawl before the recommendation engine can analyse them.

**Open results folder** takes you to the dated crawl folder under `~/Library/Application Support/Website Link Mapper/Crawls/`. It contains the HTML map, `crawl.json` and the raw link list, `links.csv`. **Download crawl JSON** works in all three report views.

**Keep saved crawls** sets a limit across all sites. The default of `0` keeps every crawl. After a successful crawl, the workflow moves excess eligible results to the macOS Trash if `/usr/bin/trash` is available.

## Crawl scope

The crawler follows HTTP and HTTPS links on the starting host and port. Start at the final address if the site redirects to a different host, such as a `www` variant. The crawler respects robots.txt and your crawl limits, records canonical URLs, and compares discovered URLs with same-host XML sitemap entries. Canonical URLs do not merge page records. Redirects on the same host count as requests and add no click to the depth. Recognised asset URLs can appear without a request.

The crawler reads server-returned HTML. Links added by JavaScript, pages behind sign-in and pages behind anti-bot checks may be missing. Only crawl sites you’re authorised to crawl.

Contextual link analysis uses the editorial content collected during the crawl. Website structure, repeated content, incomplete page extraction and link classification can affect the results.

Larger websites require more processing time and memory for contextual link analysis. The workflow generates suggestions on demand, so you can continue to use Graph and Audit without running the additional analysis.

## Privacy

Website Link Mapper requests pages from the website you choose to crawl. It stores crawl results and performs link analysis locally on your Mac.

Contextual link analysis does not send your website content to AI providers or external analysis services.

Saved crawls may contain page content, URLs, query values, anchor text and other information extracted from the website. Check the contents before sharing crawl files or exported results.

## Support

When reporting an issue, include your Website Link Mapper, macOS, Alfred and Python versions, the crawl limits and steps to reproduce it.

For issues involving the report interface, include your browser and browser version.

For issues involving contextual link suggestions, include the relevant source and destination URLs, recommendation details and any applicable page exclusions.

Check any shared crawl data for private URLs, query values and sensitive page content.

**Created by Adam Jackson** · [**jaxmore.com**](https://jaxmore.com/) · [**MIT licence**](LICENSE)
