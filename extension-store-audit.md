# Browser Extension Store Readiness Audit

When this skill is invoked, perform a comprehensive store readiness audit for a browser extension. Work through each phase below in order. Be proactive — read files, check dimensions, generate draft content, and flag issues without waiting to be asked.

---

## PHASE 0 — Orientation

1. Find and read `manifest.json` (or `manifest.v3.json`). Extract:
   - `name`, `short_name`, `description`, `version`
   - All `permissions` and `host_permissions`
   - `icons` map (paths and sizes declared)
   - `action.default_popup`, `background`, `content_scripts`
   - `homepage_url`, `author`
   - Manifest version (must be 3 for Chrome/Edge)

2. Ask the user which stores they are targeting if not already stated. Default to all three: **Chrome Web Store**, **Microsoft Edge Add-ons**, **Firefox AMO**.

3. State a one-paragraph summary of what the extension does (inferred from manifest + any source files you can read). Ask the user to confirm or correct it — this becomes the basis for all generated copy.

---

## PHASE 1 — Icon Audit

Check for icon files at every required dimension for each target store. Read the `icons` paths declared in manifest.json. For each file found, check it exists on disk.

### Required icons per store

| Store | Required sizes | Format | Notes |
|-------|---------------|--------|-------|
| Chrome Web Store | 128×128 | PNG | Store listing icon; also 16, 32, 48 for browser UI |
| Edge Add-ons | 128×128 (min), 300×300 (recommended) | PNG | Square 1:1 ratio strictly enforced |
| Firefox AMO | 128×128 | PNG or JPEG | Avoid text/letters — icon is shown across all locales |

**Also needed in manifest for browser UI (all stores):**
- 16×16, 32×32, 48×48, 128×128 PNG

### Icon checks to perform
- [ ] All sizes declared in manifest actually exist as files
- [ ] Each file is square (width === height)
- [ ] Files are PNG format
- [ ] 128×128 version exists
- [ ] Icons look clean at 32×32 (small sizes matter for toolbar display)

**If icons are missing or wrong size:** Tell the user which sizes are missing. If the project has an SVG source or a large PNG, suggest using ImageMagick or Sharp to generate the missing sizes:
```bash
# ImageMagick — generate all required sizes from a source file
magick icon-source.png -resize 16x16 icons/icon16.png
magick icon-source.png -resize 32x32 icons/icon32.png
magick icon-source.png -resize 48x48 icons/icon48.png
magick icon-source.png -resize 128x128 icons/icon128.png
magick icon-source.png -resize 300x300 icons/icon300.png  # Edge recommended
```

---

## PHASE 2 — Screenshot Audit

Look for a `screenshots/`, `store-assets/`, `assets/`, or similar directory. List all image files found.

### Required screenshots per store

| Store | Accepted dimensions | Min count | Max count | Format |
|-------|-------------------|-----------|-----------|--------|
| Chrome Web Store | 1280×800 OR 640×400 | 1 | 5 | PNG or JPEG |
| Edge Add-ons | 1280×800 OR 640×480 | 0 (optional) | 6 per language | PNG or JPEG |
| Firefox AMO | 1280×800 (or 1.6:1 ratio) | 0 (optional) | No hard limit | PNG or JPEG |

### Screenshot checks to perform
- [ ] At least 1 screenshot exists for Chrome (required)
- [ ] Screenshots are at a valid dimension for each target store
- [ ] Screenshots are not blurry or stretched
- [ ] Screenshots show actual extension UI / real usage (not mockups with fake browser chrome)
- [ ] No screenshots contain sensitive/personal data
- [ ] Chrome: square corners, no padding/border (full bleed)

### Promotional images (optional but recommended)
| Asset | Dimensions | Stores |
|-------|-----------|--------|
| Small promo tile | 440×280 | Chrome, Edge |
| Large/marquee tile | 1400×560 | Chrome (homepage carousel), Edge |

**If screenshots are missing:** Suggest the user take screenshots of the extension in use at 1280×800 browser window size, or offer to write a Playwright/Puppeteer script to automate screenshots if the project has a test setup.

---

## PHASE 3 — Privacy Policy Check

- [ ] Does the extension collect, store, or transmit any user data? (Check permissions: `storage`, `cookies`, `history`, `tabs`, `identity`, `geolocation`, any `host_permissions` that could imply data access)
- [ ] Is there a `privacy_policy_url` or `homepage_url` in the manifest?
- [ ] Is the privacy policy hosted at an HTTPS URL?
- [ ] Is the policy accessible (not behind a login)?

**If a privacy policy is needed but missing:** Recommend these generators (all support browser extension / GDPR / CCPA requirements):
- [TermsFeed](https://www.termsfeed.com/privacy-policy-generator/) — good browser extension template
- [FreePrivacyPolicy.com](https://www.freeprivacypolicy.com/)
- [Termly](https://termly.io/products/privacy-policy-generator/)

**Firefox-specific (mandatory from Nov 2025):** New extensions must declare data collection in `manifest.json` using `browser_specific_settings.gecko.data_collection_permissions`. Flag if missing.

**Rule of thumb:** If the extension uses ANY of these permissions, a privacy policy is required by all stores: `cookies`, `history`, `tabs`, `identity`, `geolocation`, `storage` (if storing user-identifiable data), any broad `host_permissions`.

---

## PHASE 4 — Generate Store Listing Copy

Using the confirmed extension description from Phase 0, generate ready-to-paste draft copy for each target store. Ask for any missing details before generating.

### 4A — Chrome Web Store

**Name** (max 75 chars):
> [Generate from manifest name — flag if over limit]

**Summary / Short description** (max 132 chars for manifest `description`):
> [Generate a punchy, benefit-led one-liner]

**Detailed description** (no hard limit, but 300–1000 words recommended):
```
[Generate a full description using this structure:]
- Opening: what the extension does in 1–2 sentences
- Key features: bullet list of 4–6 main features
- How it works: brief walkthrough
- Privacy: what data is/isn't collected
- Permissions: plain-English explanation of why each permission is needed
- Support/feedback: where to get help
```

**Category:** [Suggest based on extension functionality — e.g., Productivity, Developer Tools, Accessibility, Shopping, Entertainment]

**Additional fields to fill in on the dashboard:**
- Homepage URL: [from manifest or ask user]
- Support URL: [ask user — e.g., GitHub issues page]
- YouTube video: [ask if they have one]
- Privacy Policy URL: [from Phase 3]
- Mature content: No (unless applicable)

---

### 4B — Microsoft Edge Add-ons

Most fields overlap with Chrome. Key differences:

**Description** (min 250 chars, max 10,000 chars):
> [Reuse Chrome description — verify it meets 250 char minimum]

**Search Terms** (max 7 terms, max 30 chars each, max 21 words total):
> [Generate 7 relevant search terms based on extension functionality]

**Additional Edge-specific fields:**
- Category: [same as Chrome suggestion]
- Privacy Policy URL: [same]
- Website URL: [must be your own site, not the store listing]
- Support Contact: [URL or email]
- Small Promotional Tile: 440×280 (same as Chrome)
- Large Promotional Tile: 1400×560 (same as Chrome)

**Notes for Certification field** — generate a draft:
```
Extension Name: [name]
Version: [version]

Purpose: [one sentence]

How to test:
1. Install the extension
2. [Step-by-step walkthrough of main features]
3. [Any features that require specific setup]

Permissions used and why:
- [permission]: [plain-English reason]

Test account: [N/A or provide credentials if needed]

No external server dependencies / Server URL: [if applicable]
```

---

### 4C — Firefox AMO

**Name:** [same as Chrome]

**Summary** (max 250 chars — shorter than Chrome):
> [Generate a tighter version of the Chrome summary, strictly under 250 chars]

**Description** (HTML supported, no strict limit — keep scannable):
```html
[Generate description with:]
- <p> opening statement
- <ul> feature list
- <p> privacy statement
- <p> support info
```

**Firefox-specific fields:**
- Tags/Keywords: [generate 5–8 relevant tags]
- Support email or URL: [ask user]
- Homepage URL: [from manifest]
- License: [ask user — e.g., MIT, All Rights Reserved]
- Is source code available?: [ask — if yes, provide URL]

**Source code submission (required if using a bundler/minifier):**
- [ ] Does the extension use webpack, rollup, parcel, esbuild, or similar?
- [ ] If yes: source code ZIP + build instructions must be submitted
- [ ] Build instructions should include: Node version, `npm install`, build command, output directory
- [ ] No obfuscation allowed — even with source submitted

**Firefox Reviewer Notes** — generate a draft:
```
This add-on [one sentence description].

To test:
1. [Install steps]
2. [Feature walkthrough]

Permissions explanation:
- [permission]: [reason]

Build instructions (if applicable):
- Node version: [x]
- Run: npm install && npm run build
- Output: dist/

[Any other notes for reviewer]
```

---

## PHASE 5 — Per-Store Gotcha Checklist

Work through this checklist for each target store and flag any items that are at risk.

### Chrome Web Store
- [ ] Manifest V3 (not V2 — MV2 no longer accepted)
- [ ] No remote code execution (no eval, no external scripts loaded at runtime)
- [ ] No crypto mining code
- [ ] Single clear purpose (no unrelated feature bundles)
- [ ] Permissions list is minimal — remove any unused permissions
- [ ] Description does NOT keyword-stuff (no repeated keywords)
- [ ] Extension name does NOT include "Chrome" or trademarked browser names
- [ ] All screenshots are 1280×800 or 640×400 exactly
- [ ] At least 1 screenshot provided
- [ ] $5 one-time developer registration fee paid
- [ ] Service worker used (not persistent background page)
- [ ] All external resources referenced comply with CWS policy

### Microsoft Edge Add-ons
- [ ] Same MV3 requirements as Chrome
- [ ] Icon is exactly square (1:1 ratio) — common rejection reason
- [ ] Description is at least 250 characters
- [ ] Search terms: max 7 terms, max 30 chars each
- [ ] Screenshots are exactly 640×480 OR 1280×800 (no other sizes accepted)
- [ ] YouTube video (if provided) has ads disabled
- [ ] Website URL is your own domain, not a store URL
- [ ] Notes for Certification field completed

### Firefox AMO
- [ ] Summary is max 250 characters
- [ ] Icon is square
- [ ] Source code ZIP submitted if extension uses a bundler
- [ ] Build is reproducible from submitted source
- [ ] `browser_specific_settings.gecko.id` set in manifest (strongly recommended)
- [ ] `browser_specific_settings.gecko.strict_min_version` set
- [ ] `browser_specific_settings.gecko.data_collection_permissions` declared (required for new extensions from Nov 2025)
- [ ] No obfuscated code anywhere in the package
- [ ] WebExtensions API used (no Chrome-only APIs without polyfill)
- [ ] `browser_action` vs `action` — Firefox 109+ supports `action`; older versions need `browser_action`

### Universal (all stores)
- [ ] Privacy policy is live at an HTTPS URL
- [ ] Privacy policy accurately reflects all data collected
- [ ] Extension works correctly in its target browser(s) before submission
- [ ] Version number incremented from any previous submission
- [ ] No personally identifiable data visible in screenshots
- [ ] Support contact method is valid and monitored
- [ ] Description matches actual functionality (no feature promises not yet implemented)

---

## PHASE 6 — Website / Landing Page Readiness

If the extension has or needs a homepage:
- [ ] Page is live and loads over HTTPS
- [ ] Page clearly describes what the extension does
- [ ] Has a visible link to the store listing (or "Coming soon" before launch)
- [ ] Privacy policy linked from the footer
- [ ] Contact/support link present
- [ ] Page is mobile-responsive
- [ ] No broken links or console errors
- [ ] If collecting emails: compliant with CAN-SPAM/GDPR

---

## PHASE 7 — Final Readiness Summary

Produce a structured summary:

```
STORE READINESS REPORT
======================
Extension: [name] v[version]
Target stores: [Chrome / Edge / Firefox]
Date: [today]

ICONS          ✓/✗  [details]
SCREENSHOTS    ✓/✗  [details]
PRIVACY POLICY ✓/✗  [details]
MANIFEST V3    ✓/✗  [details]
STORE COPY     ✓/✗  [draft generated / needs input]
GOTCHAS        [list any flagged items]

BLOCKERS (must fix before submission):
- [list]

WARNINGS (should fix):
- [list]

READY TO SUBMIT: Yes / No / Partially ([which stores])
```

Then offer to:
1. Write any missing/generated content to files
2. Help fix any blockers identified
3. Generate a build script for missing icon sizes
4. Draft the privacy policy page HTML if needed
