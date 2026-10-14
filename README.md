# No-Show Guard — standalone product site

Live at **https://noshowguard.northbeambuilds.com** — the standalone website for
No-Show Guard, a product by [Northbeam Builds](https://northbeambuilds.com).
northbeambuilds.com is the company site; each product gets its own site like
this one (free on a subdomain first; graduates to its own .com when it proves out).

Pattern source: TimothyjT/nevermiss-site (the reference implementation) — this
repo copies its layout, design system (assets/site.css + site.js, byte-identical),
and deployment path.

## Layout

```
index.html            Home: hero, 3 steps, sample, deposit in plain words, the SMS asterisk, fit, price summary, proof cards, limits, FAQ teaser, CTA
demos/index.html      "The Messages" — the kit's own confirmation / pay-reminder / 48h reminder emails rendered word-for-word on a fictional booking (Willow & Vine Salon), plus the two SMS templates labeled disabled
pricing/index.html    Full price box with totals, guarantee, what's included
faq/index.html        Full FAQ (10 Q&As) + FAQPage JSON-LD identical to the visible text
contact/index.html    northbeambuilds@gmail.com card + start-by-email CTA
llms.txt              LLM-facing summary; Q&As mirror the FAQ page
assets/site.css       Shared design system (copied from the sibling product sites)
assets/site.js        Header nav toggle + chat widget script (shared; this site does not mount the widget markup, same as faqagent-site)
CNAME                 noshowguard.northbeambuilds.com
robots.txt, sitemap.xml
```

Rules that hold on every page:

- **Copy source is the kit's canonical sales copy** — `deploy-kit-no-show-guard/`
  (README honesty rules, ONE-PAGER.md, LISTING-COPY-SECTION.md §H, QUALITY-RECORD.md
  NSG-7 decision). Prices and claims must match it exactly: **$99 setup + $29/mo**,
  first month total $128, month-to-month, billing starts at go-live.
- Stripe quick-buy link (created 2026-10-14, live account TubbsterBot):
  `https://buy.stripe.com/8x25kD3gE3hqbsC93ogYU07`
  (plink_1UQWHyDuI42vTlvNVXxFMati — product "No-Show Guard"
  prod_VRP5k6zoi3cE8R; two line items: $99 one-time "Setup (one-time)"
  price_1UQWHrDuI42vTlvNuc1rlLw7 + $29/mo recurring "Monthly service"
  price_1UQWHsDuI42vTlvNYk1TfncA; $128 due today, verified by re-reading
  the link's line items in Stripe. Post-payment redirect goes to this
  site's /contact/ page — the stand-in intake surface until a dedicated
  No-Show Guard intake page exists.) Every buy CTA (index ×3, pricing,
  faq, demos, contact) points at the link with the label "Get No-Show
  Guard — $99 setup + $29/mo"; llms.txt carries the "Buy online (Stripe
  checkout)" line. The start-by-email path (northbeambuilds@gmail.com)
  remains available and is unchanged.
- The product is the hero brand (header wordmark, titles). The footer carries
  "**No-Show Guard — by Northbeam Builds**" + a link to https://northbeambuilds.com.
- **SMS is never implied live.** The 24h/2h text reminders are templates, disabled
  until carrier A2P registration is approved AND a live delivery test is on record
  for the order. Every mention carries that label.
- **v1 is one workflow**: the booking page Northbeam provides. No CSV import,
  no forwarded-email mode, no sitting on top of an existing booking system,
  no calendar sync — the copy says so.
- **No effectiveness numbers.** No no-show reduction percentage exists or is claimed.
- Sample material is labeled fictional (Willow & Vine Salon, made-up names).
  Deposits in copy always run through the buyer's own Stripe link; card details
  never touch Northbeam's system.
- Guarantee, verbatim: "Live by the date we agree, or your setup fee comes back
  in full — guaranteed." The clock starts when payment, content, and access are
  all in — whichever comes last.

## Deployment

Same path as the siblings (see nevermiss-site README §§1–5 for the full runbook):

1. Porkbun DNS: CNAME `noshowguard` → `timothyjt.github.io` (TTL 600).
2. GitHub repo `TimothyjT/noshowguard-site` (public), `CNAME` file committed.
3. Pages enabled via API (source main /), custom domain set, build triggered.
4. HTTPS: GitHub provisions the Let's Encrypt cert once DNS resolves; when
   `https_certificate.state` = `approved`, PUT `https_enforced=true`.
   **Never trust a local curl from the build VM — its proxy intercepts TLS.**
   Verify state through the Pages API.

## Product registry

| Product | Subdomain | Stripe payment link |
| ------- | --------- | ------------------- |
| No-Show Guard | noshowguard.northbeambuilds.com | **NONE YET** — CTAs use the contact path (see rules above) |
