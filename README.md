# Product Scraping API Complete Guide: How to Choose, How to Sign Up, Which Plan Fits, and Is It Worth It? (With ScraperAPI Plan Breakdown and Real Cost Math)

If you've ever tried to pull product data off Amazon, Walmart, or eBay at scale, you already know the nightmare. One day your scraper works fine. The next day you're staring at a wall of CAPTCHAs, 403 errors, and IP bans that make you question every life choice that led you to this moment. That's the gap a **product scraping API** is supposed to fill — you hand it a URL, it hands you back clean data, and somebody else worries about the proxy rotation, the headless browsers, and the anti-bot chess match.

This guide walks through what a product scraping API actually is, what it's good for, where it falls short, and how to pick one without getting burned on pricing. Along the way I'll use ScraperAPI as the running example, because it's one of the most popular options in this space and its pricing model is a useful case study in the things you need to watch for — including the credit multiplier trap that catches almost every new user. By the end you'll know how to sign up, which plan to pick, and whether the math actually works for your use case.

## What Is a Product Scraping API, and Why Do People Use One?

A product scraping API is a hosted service that handles the infrastructure of web scraping for you. Instead of running your own proxy pool, managing headless Chrome instances, writing CAPTCHA solvers, and babysitting retry logic, you make a single HTTP request to a third-party endpoint. The service does the hard work in the background and returns either raw HTML or — better — already-parsed JSON with the product fields you actually want.

The reason this category exists at all is that scraping product data has gotten genuinely hard. Major marketplaces like Amazon and Walmart have invested heavily in anti-bot defenses: fingerprinting, behavioral analysis, rate limiting, CAPTCHA challenges, and IP reputation systems. A solo developer with a residential proxy subscription and a Playwright script can still get data, but maintaining that pipeline becomes a full-time job the moment you need to scale past a few thousand requests a day.

A product scraping API abstracts all of that away. The trade-off is cost — you pay per request, and the pricing is rarely as simple as the headline number suggests. That's the part most people underestimate, and it's the part this guide spends the most time on.

**Common use cases for a product scraping API:**

- **Price monitoring** — tracking competitor prices across marketplaces on a daily or hourly basis
- **Product catalog aggregation** — building a unified catalog from multiple retailers
- **Review and rating collection** — pulling customer reviews for sentiment analysis
- **Stock availability tracking** — watching for restocks on high-demand products
- **MAP violation detection** — finding sellers pricing below minimum advertised price
- **Market research** — understanding category trends, bestseller rankings, and new product launches

If any of those sound like your job, you're in the right place.

## How a Product Scraping API Actually Works (Under the Hood)

You don't need to know the internals to use one, but understanding the moving parts helps you make sense of the pricing. Here's what happens between your API call and the JSON landing in your response.

**The request flow:**

1. You send a request to the API endpoint with your API key, the target URL (or a structured query like an ASIN), and any feature flags you want — JavaScript rendering, premium proxies, geotargeting, etc.
2. The service assigns the request to a proxy from its pool. ScraperAPI, for example, runs a pool of over 40 million IPs across 50+ countries, with rotation handled by a machine-learning system that picks the best IP for each target.
3. If the target requires JavaScript rendering, the request is routed through a headless browser instance that loads the page, waits for dynamic content, and captures the rendered DOM.
4. If the target throws a CAPTCHA or anti-bot challenge, the service attempts to solve or bypass it. This is where most cheap scrapers fail and most paid APIs earn their keep.
5. The service returns the response to you — either raw HTML for you to parse, or, if you're using a structured data endpoint, already-parsed JSON with the fields extracted.

**Why this matters for pricing:** every one of those steps costs the provider money. Proxy bandwidth, headless browser compute, CAPTCHA-solving services, and the engineering to maintain parsers all add up. That's why a "1 credit = 1 request" promise almost never holds — the real cost depends on what features your target forced the service to use.

## ScraperAPI's Structured Data Endpoints: The Product Scraping Sweet Spot

For product scraping specifically, the most valuable feature ScraperAPI offers is its **Structured Data Endpoints (SDEs)**. Instead of returning raw HTML and leaving you to write a parser that breaks every time Amazon changes its layout, the SDEs return parsed JSON with the fields you actually want.

ScraperAPI currently offers 18 structured data endpoints across five platforms:

- **Amazon** (3 endpoints): Product details by ASIN, search results, and competitor offers. The Amazon Product API returns 18+ fields including name, price, brand, product information, images, feature bullets, bestsellers rank, reviews, and variant details. It supports 21 regional marketplaces (amazon.com, amazon.co.uk, amazon.de, amazon.co.jp, amazon.in, and so on).
- **Google** (5 endpoints): SERP, Shopping, Maps, News, and Jobs.
- **Walmart** (4 endpoints): Product, Search, Category, and Reviews.
- **eBay** (2 endpoints): Product and Search.
- **Redfin** (4 endpoints): Search, Agent Details, Rental Properties, and For Sale.

Here's what a real Amazon Product API response looks like — this is the actual JSON structure returned for a sample product:

json
{
  "name": "CUCKOO Twin Pressure Rice Cooker 6-Cup Uncooked...",
  "product_information": {
    "brand": "CUCKOO",
    "capacity": "3 Quarts",
    "product_dimensions": "13.5\"D x 9.5\"W x 9.25\"H",
    "color": "White",
    "material": "Stainless Steel",
    "item_weight": "12.76 pounds",
    "asin": "B0B44XTV71"
  },
  "images": ["https://m.media-amazon.com/images/I/..."],
  "product_category": "Home & Kitchen›Kitchen & Dining›Small Appliances›Rice Cookers",
  "feature_bullets": ["16 Versatile Modes...", "Mid to Large Capacity..."],
  "best_sellers_rank": ["#14,536 in Kitchen & Dining", "#49 in Rice Cookers"],
  "reviews": []
}


That's the value proposition in one screenshot: you ask for an ASIN, you get back a structured object with everything you'd want for price monitoring, catalog building, or market research. No HTML parsing, no selector maintenance, no breakage when Amazon redesigns their product pages.

The SDEs are available on every plan, including the free tier. That's worth knowing — you can test the Amazon endpoint with 1,000 free credits before you commit to anything.

## The Credit Multiplier Trap: What Every Product Scraping API Review Skips

Here's the part that catches almost everyone, and it's the single most important thing to understand before you sign up for ScraperAPI or any credit-based scraping API.

The headline credit numbers on the pricing page are not the number of requests you'll actually get. The real cost per request depends on two things: the domain you're scraping, and the feature flags you enable. And these costs stack in non-intuitive ways.

**Base credit costs by domain category:**

| Domain Category | Base Credits per Request | Examples |
| --- | --- | --- |
| Normal websites | 1 | Blogs, news sites, simple HTML |
| E-commerce | 5 | Amazon, eBay, Walmart |
| SERP (search engines) | 25 | Google, Bing |
| Social media | 30 | LinkedIn |

**Additional credit costs for feature flags:**

| Parameter | Extra Credits | Notes |
| --- | --- | --- |
| `render=true` (JS rendering) | +10 | All plans |
| `screenshot=true` | +10 | All plans |
| `premium=true` (premium proxy) | +10 | All plans |
| `ultra_premium=true` | +30 | Paid plans only |
| Anti-bot bypass (Cloudflare, DataDome, PerimeterX) | +10 each | Auto-detected |
| `premium=true` + `render=true` combined | +25 | Not +20 |
| `ultra_premium=true` + `render=true` combined | +75 | Not +40 |

That last row is the kicker. Combining features costs more than the sum of the individual costs. Premium proxy (+10) plus JavaScript rendering (+10) should logically cost +20, but ScraperAPI charges +25. Ultra-premium (+30) plus JavaScript rendering (+10) should cost +40, but it's actually +75 — nearly double. This non-linear stacking is documented but not prominently, and it's the primary reason users report credits vanishing faster than expected.

**What this means for product scraping specifically:** if you're scraping Amazon product pages, every request costs a minimum of 5 credits. If Amazon throws an anti-bot challenge that requires premium proxies, that's 15 credits per request. If you also need JavaScript rendering for variant data, you're at 25 credits per request. A Hobby plan with 100,000 credits gets you 4,000 Amazon product pages under that scenario — not 100,000.

This isn't a ScraperAPI-specific problem. Every credit-based scraping API has some version of this. But ScraperAPI's multipliers are on the higher end of the market, and the documentation buries the lede. Run the math for your specific use case before you commit to a paid plan.

## ScraperAPI's Full Plan Lineup: Every Tier, Side by Side

Here's every plan currently displayed on ScraperAPI's pricing page, with the configurations and prices verified against the official site and corroborated by multiple third-party sources.

| Plan | Monthly Price | Annual (per month) | API Credits | Concurrent Threads | Geotargeting | Best For |
| --- | --- | --- | --- | --- | --- | --- |
| **Free** | $0 | — | 1,000 | 5 | No | Testing target sites before committing |
| **Hobby** | $49 | $44.10 | 100,000 | 20 | US & EU only | Small projects, personal use |
| **Startup** | $149 | $134.10 | 1,000,000 | 50 | US & EU only | Low-volume scraping workflows |
| **Business** | $299 | $269.10 | 3,000,000 | 100 | Country-level (50+ countries) | Growing teams with geo needs |
| **Scaling** | $475 | $427.50 | 5,000,000 | 200 | Country-level | High-volume production scraping |
| **Enterprise** | Custom | Custom | 5,000,000+ | 200+ | Country-level | Large-scale data acquisition |

**Purchase links (each plan has a dedicated signup path):**

- 👉 [Start with the Free Plan — 1,000 credits, no credit card](https://www.scraperapi.com/?fp_ref=coupons)
- 👉 [Get the Hobby Plan — $49/mo, 100K credits](https://www.scraperapi.com/pricing/?fp_ref=coupons)
- 👉 [Get the Startup Plan — $149/mo, 1M credits](https://www.scraperapi.com/pricing/?fp_ref=coupons)
- 👉 [Get the Business Plan — $299/mo, 3M credits, country geotargeting](https://www.scraperapi.com/pricing/?fp_ref=coupons)
- 👉 [Get the Scaling Plan — $475/mo, 5M credits, pay-as-you-go](https://www.scraperapi.com/pricing/?fp_ref=coupons)
- 👉 [Talk to Sales about Enterprise — custom volume and pricing](https://www.scraperapi.com/pricing/?fp_ref=coupons)

A few things worth flagging about the plan structure:

- **Credits do not roll over.** Unused credits expire at the end of each billing cycle. There's no accumulation, so sizing your plan correctly matters more than it does with rollover-friendly services.
- **Pay-As-You-Go is only available on the Scaling plan ($475/mo) and above.** If you're on Hobby, Startup, or Business and you exhaust credits mid-cycle, you're cut off until the next billing period. Your only option is upgrading.
- **Geotargeting beyond US & EU requires the Business plan ($299/mo).** If you need to scrape Amazon Germany from German IPs, or Amazon Japan from Japanese IPs, you need at least the Business tier.
- **The 7-day free trial gives you 5,000 credits with no credit card required.** That's separate from the permanent free tier of 1,000 credits per month. Use the trial to test your actual target sites before committing.

## Real Cost per 1,000 Requests: The Math That Actually Matters

Headline pricing is meaningless without accounting for multipliers. Here's the effective cost per 1,000 requests at each paid tier, across the most common product scraping scenarios:

| Plan | Standard (1×) | E-commerce (5×) | JS Rendering (10×) | E-comm + JS (15×) | Ultra-Premium + JS (75×) |
| --- | --- | --- | --- | --- | --- |
| Hobby ($49) | $0.49 | $2.45 | $4.90 | $7.35 | $36.75 |
| Startup ($149) | $0.15 | $0.75 | $1.49 | $2.24 | $11.18 |
| Business ($299) | $0.10 | $0.50 | $1.00 | $1.50 | $7.48 |
| Scaling ($475) | $0.10 | $0.48 | $0.95 | $1.43 | $7.13 |

Read this table before you pick a plan, not after. A Hobby plan advertised as "100,000 credits" delivers only 1,333 actual requests when scraping protected sites with ultra-premium proxies plus JavaScript rendering. That works out to $36.75 per 1,000 pages — more expensive than many fully managed scraping services.

For pure Amazon product scraping with no extra features (5 credits per request), the math is more forgiving: Hobby gets you 20,000 Amazon products, Startup gets you 200,000, Business gets you 600,000, and Scaling gets you 1,000,000. That's the scenario where ScraperAPI's pricing is genuinely competitive.

## How to Sign Up and Make Your First Product Scraping Request

The signup flow is genuinely simple, which is one of the things ScraperAPI gets right. Here's the full path from zero to first JSON response.

**Step 1: Create your account**

Head to the ScraperAPI signup page (👉 [get started here with the 7-day trial and 5,000 free credits](https://www.scraperapi.com/?fp_ref=coupons)). No credit card is required for the trial. You'll get an API key immediately.

**Step 2: Make your first request with the standard API**

The simplest possible request — scraping a normal HTML page — looks like this:

python
import requests

url = "https://api.scraperapi.com/"
params = {
    "api_key": "YOUR_API_KEY",
    "url": "https://example.com/product-page"
}

response = requests.get(url, params=params)
print(response.text)


That returns raw HTML. Useful, but you still have to parse it yourself.

**Step 3: Use the Amazon Structured Data Endpoint for parsed product JSON**

This is where product scraping gets actually useful. Instead of scraping HTML, you ask the Amazon Product API for a specific ASIN and get back parsed JSON:

python
import requests

payload = {
    "api_key": "YOUR_API_KEY",
    "asin": "B0B44XTV71",
    "country_code": "us",
    "tld": "com"
}

r = requests.get(
    "https://api.scraperapi.com/structured/amazon/product",
    params=payload
)

print(r.json())


That single call returns the full product object — name, price, brand, images, feature bullets, bestsellers rank, reviews, variants, and 18+ other fields. No HTML parsing, no selector maintenance. This is the core value proposition of using a product scraping API with structured endpoints.

**Step 4: Handle pagination and rate limits**

For bulk product scraping, you'll want to use the `session_number` parameter to maintain IP consistency across related requests, and respect the concurrent thread limit on your plan (5 on Free, 20 on Hobby, 50 on Startup, etc.). Going over the concurrency limit returns 429 errors.

**Step 5: Monitor your credit consumption**

This is the step most people skip and regret. ScraperAPI's dashboard shows usage stats — average latency, domains scraped, concurrency — but there are no proactive alerts when credits are running low. You have to check manually. Set a calendar reminder for the first month until you build intuition for how fast credits burn on your specific targets.

## Where ScraperAPI Performs Well (and Where It Falls Short)

Independent benchmarks from Scrapeway paint a sharply bimodal picture. No scraping API works equally well on every website, and ScraperAPI is no exception.

**Performance by site category:**

| Target Site | Success Rate | Avg Speed | Cost per 1K (Business Plan) |
| --- | --- | --- | --- |
| Zillow | 100% | 10.5s | $0.49 |
| Etsy | 99% | 4.8s | $4.90 |
| Amazon | 98% | 6.5s | $2.45 |
| LinkedIn | 95% | 17.8s | $14.70 |
| Walmart | 93% | 11.4s | $2.45 |
| Indeed | 90% | 15.8s | $4.90 |
| StockX | 84% | 3.9s | $4.90 |
| Realtor.com | 12% | 11.8s | $0.49 |
| Instagram | 0% | — | — |
| Booking.com | 0% | — | — |
| Twitter/X | 0% | — | — |

The takeaway for product scraping specifically: ScraperAPI is genuinely strong on Amazon (98%), Walmart (93%), and Etsy (99%). Those are the three marketplaces that matter most for e-commerce price monitoring and catalog aggregation, and ScraperAPI's structured data endpoints for those sites return reliable parsed JSON. If your use case is centered on those three, the credit costs are justified by the success rates.

Where it falls short:

- **Social media is a dead zone.** Instagram, Twitter/X, and Booking.com all show 0% success rates. If you need social product tagging or influencer product data, look elsewhere.
- **Login-required sites are off-limits.** ScraperAPI supports session persistence via the `session_number` parameter, but it explicitly forbids scraping data behind login walls. It cannot handle form filling, two-factor auth, or complex auth flows.
- **Stale data on protected targets.** ScraperAPI applies a 10-minute forced result cache on difficult targets. If you're scraping time-sensitive data like real-time pricing or stock levels, you may receive results up to 10 minutes old.

## What Real Users Say: Sentiment from G2, Capterra, and Trustpilot

I aggregated feedback from three review platforms to get a sense of what actual users think. Here are the current ratings:

| Platform | Rating | Number of Reviews |
| --- | --- | --- |
| G2 | 4.4/5 | 16 |
| Capterra | 4.6/5 | 62 |
| Trustpilot | 4.5/5 | 43 |

Capterra sub-ratings: Ease of Use 4.9/5, Customer Service 4.6/5, Features 4.5/5, Value for Money 4.5/5.

**What users consistently praise:**

- **Ease of setup.** "Super easy to set up. You can start scraping in minutes." The documentation is above average for the category, and the API design is clean.
- **Reliability on supported targets.** Multiple reviews highlight that Amazon and Google scraping "just works" once you have the right feature flags set.
- **Only charging for successful requests.** ScraperAPI only bills for 200 and 404 responses, not for connection failures or timeouts.

**What users consistently complain about:**

- **Pricing transparency.** "Breakdown of credit costs can be confusing" — John S., Founder, Capterra. The credit multiplier system is the single most common source of negative feedback.
- **Reliability on harder targets.** "ScraperAPI becomes shaky for heavy duty jobs" — emcarter, Latenode community. Users scraping non-supported sites report high failure rates.
- **Unexpected billing.** One Reddit user reported being quoted one price, then billed at 5× the rate after a domain multiplier was applied without upfront disclosure. This is the credit multiplier trap in the wild.
- **Costs scaling faster than expected.** "If you're running large-scale operations, the expenses can add up quickly" — mikezhang, Latenode community. Several users note that building custom infrastructure becomes more cost-effective past a certain volume.

The pattern is consistent: ScraperAPI is well-regarded for ease of initial setup and performs reliably on popular, well-supported targets. The complaints cluster around pricing surprises and reliability on harder targets. Both of those are predictable if you understand the credit multiplier system before you sign up.

## How to Pick the Right Plan Without Wasting Credits

Here's a decision framework based on the actual math, not the marketing copy.

**Start with the Free tier to test your targets.** Use the 1,000 free credits (plus the 7-day trial with 5,000 credits) to test success rates on your specific target sites before committing to a paid plan. Document which sites need JavaScript rendering or premium proxies so you can estimate realistic monthly costs with multipliers applied. This is the single most important step, and almost nobody does it.

**Pick Hobby ($49/mo) if:**

- You're scraping fewer than 20,000 Amazon products per month (5 credits each = 100,000 credits)
- You only need US and EU geotargeting
- You're doing personal projects or proof-of-concept work
- You can tolerate being cut off mid-cycle if you exhaust credits

👉 [Get the Hobby Plan — $49/mo, 100K credits](https://www.scraperapi.com/pricing/?fp_ref=coupons)

**Pick Startup ($149/mo) if:**

- You're scraping 100,000 to 200,000 Amazon products per month
- You need 50 concurrent threads for faster throughput
- You're a small team running regular scraping jobs
- US/EU geotargeting is still sufficient

👉 [Get the Startup Plan — $149/mo, 1M credits](https://www.scraperapi.com/pricing/?fp_ref=coupons)

**Pick Business ($299/mo) if:**

- You need country-level geotargeting beyond US/EU (Amazon Germany from German IPs, Amazon Japan from Japanese IPs, etc.)
- You're scraping 500,000+ Amazon products per month
- You need 100 concurrent threads for production workloads
- You want access to analytics history beyond 2 weeks

👉 [Get the Business Plan — $299/mo, 3M credits, country geotargeting](https://www.scraperapi.com/pricing/?fp_ref=coupons)

**Pick Scaling ($475/mo) if:**

- You need Pay-As-You-Go billing (only available at this tier and above)
- You're scraping 1,000,000+ Amazon products per month
- You need 200 concurrent threads
- You've outgrown the Business plan's credit cap

👉 [Get the Scaling Plan — $475/mo, 5M credits, pay-as-you-go](https://www.scraperapi.com/pricing/?fp_ref=coupons)

**Talk to Sales about Enterprise if:**

- You need 5,000,000+ credits per month
- You need custom concurrency limits
- You want dedicated infrastructure or SLAs
- You're running mission-critical data pipelines

👉 [Talk to Sales about Enterprise — custom volume and pricing](https://www.scraperapi.com/pricing/?fp_ref=coupons)

## Practical Tips for Getting the Most Out of a Product Scraping API

Whether you end up with ScraperAPI or another provider, these tips apply broadly.

**Disable premium features unless the target requires them.** ScraperAPI does not auto-enable premium proxies or JavaScript rendering — you have to explicitly set `render=true`, `premium=true`, or `ultra_premium=true`. But domain-based pricing is automatic: Amazon always costs 5 credits, Google always costs 25, LinkedIn always costs 30. Anti-bot bypass credits are also added automatically when detected. Know this before you run a batch.

**Use structured data endpoints for supported sites.** If you're scraping Amazon, Walmart, eBay, or Google, the SDEs save development time even if they cost more credits. For unsupported sites, evaluate whether building a custom parser is worth the engineering investment.

**Have a backup plan for unreliable targets.** If a provider's success rate on a specific site is below 90%, consider routing those requests through a different provider or using a browser-based tool. Don't assume one API will handle every site you need.

**Know the gotchas before they bite you:**

- 404 responses consume credits — ScraperAPI charges for both 200 and 404 status codes
- Cancelled requests are charged if you cancel before the 70-second processing window completes
- 10-minute forced caching on difficult targets means you may get stale data
- Pay-As-You-Go is only on Scaling ($475/mo) and above — lower-tier users who exhaust credits are cut off
- Geotargeting beyond US & EU requires the Business plan ($299/mo)
- DataPipeline (the no-code scheduled scraping feature) uses a separate, higher credit schedule — basic requests cost 6 credits in DataPipeline versus 1 credit via the standard API

**Monitor your credit consumption daily for the first month.** There are no proactive usage alerts. You have to check the dashboard manually. Set a calendar reminder until you build intuition for how fast credits burn on your specific targets.

## Do You Even Need a Product Scraping API?

This is the question most reviews skip, and it's worth asking honestly. A surprising number of people searching for a "product scraping API" don't actually need one — they need product data, and an API is one of several ways to get it.

**A scraping API makes sense if:**

- You have a developer or engineering team
- You need to scrape 100,000+ pages per day programmatically
- You need deep customization of request headers, sessions, and retry logic
- Your targets are well-supported by structured data endpoints (Amazon, Google, Walmart, Zillow)
- You're building product data into a larger pipeline or application

**A no-code scraping tool makes more sense if:**

- You're in sales, e-commerce ops, marketing, or real estate — not engineering
- You need data from dozens of different sites without building custom parsers for each
- You want direct export to Excel, Google Sheets, Airtable, or Notion
- You need to scrape sites that require login (browser-based tools use your existing session)
- You want AI to read the page fresh each time — no code maintenance when sites change layouts
- Your volume is under 1,000 pages per day

The web scraping API market is a $2.03 billion industry growing at 14–18% CAGR, but that growth is driven largely by enterprise engineering teams — not by the sales ops manager who needs 500 product prices in a spreadsheet. Be honest about which one you are before you commit to a credit-based API contract.

## The Bottom Line on ScraperAPI for Product Scraping

After all the research, here's where I land:

**ScraperAPI is a solid choice for developer teams** scraping high-volume, well-supported targets like Amazon, Walmart, Etsy, and Google. The structured data endpoints are genuinely useful, the proxy infrastructure is large (40M+ IPs, 50+ countries), and the documentation is above average. For pure Amazon product scraping at scale, the 5-credit cost per request is competitive given the 98% success rate.

**The credit multiplier system is the biggest risk.** If you don't understand how multipliers stack, you will overspend. The gap between advertised credits and actual requests can be 5× to 75×. Run the math for your specific use case before committing to a paid plan, and use the free trial to test your actual targets.

**Reliability is site-dependent.** ScraperAPI is excellent on e-commerce and real estate, mediocre on job boards, and completely useless on Instagram, Twitter/X, and Booking.com. Don't assume uniform performance across the web.

**For non-technical teams**, a credit-based scraping API is the wrong tool. If you're in sales, marketing, or ops and need structured product data without writing code, a no-code browser-based tool gets you there faster and with more predictable costs.

**For developers on a budget**, test ScraperAPI's free tier on your specific targets, then compare effective per-request costs against ScrapingBee, Scrapfly, and Bright Data before choosing. The cheapest option depends entirely on your use case and feature requirements.

Want to see how the numbers work for your specific product scraping needs? 👉 [Start with ScraperAPI's free tier to test your target sites](https://www.scraperapi.com/?fp_ref=coupons) — 1,000 credits per month, no credit card required, and a 7-day trial with 5,000 credits to test at larger scale before you commit.

## Frequently Asked Questions

**Is ScraperAPI free?**

Yes, ScraperAPI offers a permanent free tier with 1,000 API credits per month and a 7-day trial with 5,000 credits. No credit card is required for either. However, credit multipliers for JavaScript rendering, premium proxies, or high-cost domains (Amazon = 5×, Google = 25×, LinkedIn = 30×) mean your real capacity may be far lower than 1,000 requests. On the free tier, ultra-premium proxies are not available.

**How much does ScraperAPI cost per request?**

It depends on the feature flags and target domain. A standard request to a simple HTML site costs 1 credit. An Amazon request costs 5 credits. A Google SERP request costs 25 credits. Adding JavaScript rendering adds 10 credits. Combining ultra-premium proxy with JavaScript rendering costs 75 credits per request. On the Hobby plan ($49/month, 100K credits), that's anywhere from $0.00049 per request (standard) to $0.0368 per request (ultra-premium + JS).

**Is ScraperAPI good for scraping Amazon?**

ScraperAPI's Amazon Structured Data endpoint is one of its strongest features, with a 98% success rate in independent benchmarks and comprehensive parsed JSON output (18+ fields including price, reviews, BSR, variants, images, and seller info). Each Amazon request costs 5 credits minimum, so costs add up at scale — but for developer teams running high-volume Amazon scraping, the per-product cost is competitive.

**Can ScraperAPI scrape sites that require login?**

No. ScraperAPI supports session persistence via the `session_number` parameter (same IP across multiple requests), but it explicitly forbids scraping data behind login walls. It cannot handle form filling, two-factor authentication, or complex auth flows. For login-required sites, browser-based tools that operate within your existing session are the more reliable option.

**What's the difference between ScraperAPI's standard API and structured data endpoints?**

The standard API returns raw HTML for any URL you pass in — you parse it yourself. The structured data endpoints (SDEs) return already-parsed JSON for specific supported sites (Amazon, Google, Walmart, eBay, Redfin). SDEs cost more credits per request (5 for Amazon vs. 1 for a standard request) but save you the engineering time of building and maintaining a parser.

**Do ScraperAPI credits roll over?**

No. Unused credits expire at the end of each billing cycle. There's no accumulation, so sizing your plan correctly matters. If you consistently underspend, downgrade to a lower tier; if you consistently hit the cap, upgrade before you run out, because lower-tier users are cut off mid-cycle with no pay-as-you-go option.
