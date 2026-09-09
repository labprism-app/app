# LabPrism — Lab result viewer & source detail inspector

Inspect lab observations with their original units, dates and source reference text. LabPrism keeps normalized record fields available beside readable results.

**Site:** https://labprismapp.com/
**Repository:** https://github.com/labprism-app/app

## Production integration

The previous synthetic demo integration has been removed. This client requests real patient-authorized records using [FinchNode production](https://finchnode.com/openapi.yaml) through the [shared connection service](https://github.com/visitquill/finchapps-connect). It never calls the public demo API or falls back to fixture data.

Production activation is pending operator legal/privacy details and a server-side live key plus webhook signing secret. Until that is complete, connecting fails closed with an explicit setup message. Deployment of this code alone is not evidence of a completed real EHR connection.


## Website and import flow

The public home route presents a complete, individually designed website with navigation, a product explanation, and a primary import button. Import and private records live at `/#/import`; opening the homepage never starts a record request. The import button opens category selection before any external connection. Back to home clears records from the rendered page. Returning from Hosted Connect opens the import route. Existing sessions can be resumed via the homepage button.

## Use

Choose record categories, click **Connect my EHR**, and complete FinchNode Hosted Connect and your own provider sign-in. Consent identifies **FinchApps Personal Health Tools**, the shared application behind these eleven sites. Return here to view the authorized record. Each visitor session is isolated to this site's origin and expires after 30 minutes; free service restarts can end it earlier. Reconnect if necessary.

Lab result inspector. Source names, dates, units, missing categories and partial sync warnings come from the production response. No patient identity, provider, measurement or connection is invented. FHIR Trail shows FinchNode's normalized records derived from FHIR, not an untouched FHIR bundle. ConsentLoom displays actual consent metadata. SourceWeave lists only the sources returned with the authorized record.

End this session removes local access. Revoke sharing or request deletion through [FinchNode data controls](https://finchnode.com/me). Sharing consent is for the common application, so revocation can affect all eleven tools. Exported or printed copies remain on the user's device.

## Local development

Node 22.13+:

```sh
npm ci
npm run dev
npm test
npm run build
```

The build outputs `dist/`. This is a React/Vite static frontend with responsive layouts, keyboard controls, visible focus styles and reduced-motion support. Development runs do not bypass production origin restrictions. To exercise authentication locally, run the backend's injected mock tests; do not relax its production allowlist or embed keys in the client.

## Render

Create a free Static Site from this repository, build with `npm ci && npm run build`, and publish `dist`. The supplied `render.yaml` documents the service and security headers. Public-repository deployments require a manual deploy after pushing a commit. The separate Node connection service runs on Render's free plan and may sleep.

CSP `connect-src` must allow only `https://finchapps-connect.onrender.com`. Deploy the backend and configure its secret environment before enabling live connections. **Never add API keys to Vite variables, source, browser storage, logs, README examples or Git.** No frontend environment secret is required.

## Privacy and verification

Read [the data-handling notice](https://labprismapp.com/privacy.html). No clinical record is saved in browser storage; visible data is held in memory, cleared on hiding the page, and periodically revalidated. No browser agent tools expose medical data. Unit tests validate production envelopes and preserve source values. Backend tests cover origin/session isolation, scope checks, invalid environments, expiration and signed revocation without using real medical data.

A real patient must perform their own EHR authentication and consent; these tests do not claim successful patient connectivity. Availability varies by healthcare organization.

## Custom domain

`labprismapp.com` is registered through Squarespace and assigned to this Render site. DNS uses an apex A record pointing to `216.24.57.1` and a `www` CNAME pointing to `labprism.onrender.com`. Render redirects `www` to the apex domain and manages HTTPS certificates.

<!-- public-discovery -->
## Public guide and project context

[A lab result is more than a number](https://labprismapp.com/guides/lab-results-with-source-context.html) — How LabPrism keeps units, dates, reference text and normalized fields beside a result without inventing an interpretation.

[Search LabPrism guides](https://labprismapp.com/guides/) · [About the site](https://labprismapp.com/about.html) · [Sitemap](https://labprismapp.com/sitemap.xml)

LabPrism is a standalone product with its own interface, documentation and repository, prepared for independent business operation and continued development. Its FinchNode integration is documented in the code. Live production activation remains pending.

## Public-page build and discoverability

Edit `content/seo.json` for reviewed article text and site metadata. `npm run build` generates public HTML pages, a sitemap, social metadata and structured data, then prerenders the actual React homepage. `npm run test:seo` checks the built crawl surface after a build. Public guide search filters only public text in the browser; no patient data or search analytics enter the index.

Keep canonical URLs on the custom domain configured in `content/seo.json`. Add only public, canonical pages to the sitemap. Validate links, mobile layout and the built HTML after editorial changes. Search engine indexing and rich results are not guaranteed.

## Independent business handoff

[Business handoff](HANDOFF.md) covers product identity, the receiving business’s production setup, domain migration, search verification and ongoing editorial maintenance.

## More product guides

Learn to inspect a lab entry as a complete record: result text, unit, source reference information, status and the normalized fields behind the display.

- [How to inspect a lab result’s fields in LabPrism](https://labprismapp.com/guides/inspect-lab-result-fields.html) — Find the value, unit, source reference text and status behind a lab entry, and inspect the normalized observation without inventing missing information.
- [Why LabPrism shows missing lab fields explicitly](https://labprismapp.com/guides/missing-lab-fields-in-normalized-records.html) — How LabPrism handles absent units, reference text and interpretation when displaying normalized lab entries from an authorized record connection.
