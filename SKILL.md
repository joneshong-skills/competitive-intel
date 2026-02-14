---
name: competitive-intel
description: >-
  This skill should be used when the user asks to "analyze competitors", "competitive research",
  "extract competitor ads", "compare competitors", "market positioning analysis",
  "competitive landscape", "ad analysis", "competitor strategies",
  "competitor messaging", "competitor positioning",
  "競爭分析", "競品研究", "市場定位分析", "廣告分析", "競爭對手",
  mentions competitive intelligence,
  or discusses competitor strategies, market positioning, ad analysis, or competitive landscape research.
version: 0.2.0
tools: Read, Write, Glob, Bash, WebSearch, WebFetch
---

# Competitive Intelligence

Research and analyze competitor strategies, advertising, messaging, and market positioning. Gather data from public sources, identify patterns in successful campaigns, and produce actionable competitive insights.

## Workflow

1. **Define scope** -- Confirm target competitors, industry, channels, and time frame with the user.
2. **Gather data** -- Collect ads, landing pages, pricing, and messaging from public sources.
3. **Analyze patterns** -- Apply the analysis framework below to each competitor.
4. **Synthesize insights** -- Compare across competitors, surface trends and gaps.
5. **Report** -- Deliver findings in the requested output format with clear recommendations.

## Tool Strategy

### Primary route: WebSearch + WebFetch
These tools are always available and should be the default approach.
- **WebSearch** -- discover competitor pages, reviews, news, social profiles, and ad mentions
- **WebFetch** -- extract and analyze content from specific URLs (landing pages, blog posts, pricing pages, review listings)

This combination covers the majority of competitive intelligence needs without requiring a browser automation stack.

### Enhanced route: Playwright (when available)
If Playwright MCP tools are connected, use them for richer data collection:
- Interacting with dynamic/JS-heavy pages that WebFetch cannot fully render
- Navigating multi-step flows (signup funnels, pricing calculators)
- Capturing screenshots for visual competitive comparisons

If Playwright is **not** available or a page fails to load, fall back to WebSearch + WebFetch. Do not block the research workflow on browser availability.

## Data Gathering Methods

### Web Search
Use WebSearch to find competitor information:
- Company announcements, press releases, blog posts
- Review sites (G2, Capterra, TrustRadius) for positioning and user sentiment
- Job postings (reveal strategic priorities)
- Funding/partnerships (reveal growth direction)

### Ad Library Research

> **Note:** Ad libraries from Meta, LinkedIn, and Google frequently use CAPTCHAs, login walls,
> and heavy client-side rendering that block automated access. Automated scraping of these
> platforms is unreliable and should not be the primary strategy.

**Recommended approach:**
- Use **WebSearch** to find third-party reports, screenshots, and analyses of competitor ads (e.g., searches like `"CompanyName" ad library site:reddit.com OR site:twitter.com`, `"CompanyName" facebook ads examples`)
- Use **WebFetch** to extract ad-related content from blog posts, case study sites, and ad intelligence summaries
- If the user has manual access to an ad library, ask them to export or screenshot relevant ads for analysis
- Focus automated effort on **publicly accessible data**: landing pages linked from ads, SEO/SEM keyword overlap, and review-site mentions of ad campaigns

**If Playwright is available and the user requests direct ad library access:**
- **Meta Ad Library** (`facebook.com/ads/library`) -- search by advertiser name, filter by country/platform/date
- **LinkedIn Ad Library** (`linkedin.com/ad-library`) -- B2B ad creative and targeting
- **Google Ads Transparency Center** (`adstransparency.google.com`) -- search and display ads
- Expect failures due to bot detection; inform the user and pivot to the recommended approach above

### Website & Landing Page Analysis
Use WebFetch (or Playwright when available) to visit competitor sites:
- Homepage hero messaging and value proposition
- Pricing page structure and packaging
- Feature comparison pages
- Signup/demo flow and CTAs
- Trust signals (logos, testimonials, case studies)

### Social Media Content
Use WebSearch to find public social profiles and content. Use WebFetch to extract post content from public profile pages when possible.

#### Analysis Dimensions
- **Posting frequency** -- Posts per week/month per platform; consistency and scheduling patterns
- **Engagement metrics** -- Likes, comments, shares, reposts; engagement rate relative to follower count; which post types drive the most interaction
- **Content themes** -- Recurring topics (product updates, thought leadership, customer stories, industry news, culture/hiring); ratio of educational vs promotional vs community content
- **Platform strategy** -- Which platforms are active vs dormant; platform-specific content adaptation (e.g., short-form video on TikTok/Reels, long-form on LinkedIn); cross-posting vs native content
- **Tone and voice** -- Formal/casual, technical/accessible, brand personality cues
- **Organic vs paid signals** -- Sponsored post indicators, boosted content patterns, dark post traces from ad libraries
- **Community engagement** -- Response rate to comments, UGC reposts, community building tactics
- **Hashtag and keyword strategy** -- Branded vs industry hashtags, SEO-aligned captions

## Analysis Framework

For each competitor, evaluate:

### Messaging Analysis
- **Problems highlighted** -- What pain points do they lead with?
- **Value propositions** -- What outcomes do they promise?
- **Positioning statement** -- How do they differentiate from alternatives?
- **Proof points** -- What evidence supports their claims?

### Creative Patterns
- **Visual style** -- Colors, imagery type (photo/illustration/abstract), brand consistency
- **Ad formats** -- Static image, carousel, video, text-only
- **CTAs** -- Primary action (demo, trial, signup, learn more), urgency language
- **Landing page structure** -- Hero, social proof, features, pricing, FAQ

### Copy Formulas
- **Headline structures** -- Question, statistic, benefit-first, problem-agitate-solve
- **Tone** -- Formal/casual, technical/accessible, aspirational/practical
- **Length** -- Short punchy vs detailed explanatory
- **Hooks** -- Opening patterns that drive engagement

### Audience Targeting
- **Segments** -- Job titles, industries, company sizes mentioned
- **Channels** -- Where they concentrate spend (search, social, display, email)
- **Funnel stage** -- Awareness, consideration, or decision-focused messaging
- **Geographic focus** -- Markets and languages targeted

### Pricing & Packaging
- **Model** -- Freemium, free trial, demo-gated, usage-based, seat-based
- **Tiers** -- Number of plans, feature differentiation, price points
- **Anchoring** -- How they frame value vs cost
- **Competitive positioning** -- Explicit comparisons or implicit positioning

## Output Location

Save all output files to:

```
~/Claude/skills/competitive-intel/{company-name}/
```

- Use a URL-safe, lowercase version of the company name for the directory (e.g., `acme-corp`, `stripe`, `notion`)
- Create the directory if it does not exist
- Place the main report and any supporting files (screenshots, spreadsheets) in this directory
- If analyzing multiple competitors in one session, use the primary subject or a descriptive group name (e.g., `crm-landscape-2026`)

## Output Formats

### Markdown Report (default)
Structure the report as:
1. Executive summary (3-5 bullet insights)
2. Per-competitor profiles
3. Cross-competitor comparison table
4. Key patterns and trends
5. Recommendations

### Spreadsheet (xlsx)
When user requests xlsx output, use the `/xlsx` skill to create a workbook with sheets:
- **Overview** -- Competitor name, URL, tagline, category
- **Messaging** -- Value props, pain points, positioning per competitor
- **Ads** -- Ad text, format, CTA, landing URL, date observed
- **Pricing** -- Plans, prices, features per tier
- **Comparison** -- Side-by-side feature/positioning matrix

### Presentation (pptx)
When user requests pptx output, use the `/pptx` skill to create a deck with slides:
- Title slide with scope and date
- Competitor landscape overview
- Per-competitor deep dive (1-2 slides each)
- Comparison matrix
- Key insights and recommendations

## Multi-Competitor Comparison Template

| Dimension | Competitor A | Competitor B | Competitor C | Our Position |
|---|---|---|---|---|
| Primary value prop | | | | |
| Target audience | | | | |
| Pricing model | | | | |
| Key differentiator | | | | |
| Strongest channel | | | | |
| Messaging tone | | | | |
| Top CTA | | | | |
| Notable gap | | | | |

## Recommendations Format

Structure recommendations as:

- **Test this** -- Tactics competitors use successfully that we should experiment with
- **Adapt this** -- Approaches to reinterpret for our brand and audience
- **Differentiate here** -- Gaps in competitor positioning we can own
- **Avoid this** -- Strategies that appear ineffective or saturated
- **Monitor this** -- Emerging trends or shifts to watch

Each recommendation should include: the observation, why it matters, and a concrete next step.

## Legal & Ethical Guidelines

- Use only **publicly accessible** information (ad libraries, public websites, public social profiles)
- **Do not** copy competitor ad creative, copy, or assets verbatim for use -- analysis only
- **Do not** attempt to access gated content, private accounts, or internal tools
- **Do not** scrape at volumes that could disrupt services; use reasonable request pacing
- Respect intellectual property -- reference and attribute, do not reproduce
- Clearly label all findings as competitive research, not endorsed or verified claims
- When in doubt about a data source's accessibility, ask the user before proceeding

## Continuous Improvement

This skill evolves with each use. After every invocation:

1. **Reflect** — Identify what worked, what caused friction, and any unexpected issues
2. **Record** — Append a concise lesson to `lessons.md` in this skill's directory
3. **Refine** — When a pattern recurs (2+ times), update SKILL.md directly

### lessons.md Entry Format

```
### YYYY-MM-DD — Brief title
- **Friction**: What went wrong or was suboptimal
- **Fix**: How it was resolved
- **Rule**: Generalizable takeaway for future invocations
```

Accumulated lessons signal when to run `/skill-optimizer` for a deeper structural review.
