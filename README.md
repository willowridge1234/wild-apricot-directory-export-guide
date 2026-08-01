# How to export a public Wild Apricot member directory

[Wild Apricot](https://www.wildapricot.com/) is association management software used by tens of thousands of clubs, chambers, and nonprofits to run membership, events, and a public-facing website — including, on most sites, a searchable **member directory**. Wild Apricot is now part of [Personify](https://www.wildapricot.com/legal-center/privacy-policy), the same corporate family that owns MemberClicks; the two products share a similar shape but are separate platforms with their own admin tools, terminology, and default behavior.

The directory is a common target for anyone trying to build a list of an association's members, sponsors, or contacts: sales teams, researchers, journalists, other associations doing market comparisons, and members themselves. The usual first question is simple — *"how do I export this?"* The honest answer depends on who you are. Wild Apricot ships real export and API tools, but they are **administrator tools** reached only after logging into that association's own Wild Apricot account. If you are not that administrator and the association hasn't given you a file, your options are narrower, and this guide is about using them correctly.

This guide is scoped to one situation: **a public Wild Apricot directory that has no export offered to you**, and how to collect the visible, unauthenticated part of it responsibly. It assumes you already understand the general legal and quality issues around directory-sourced leads; for that broader treatment across multiple association-management platforms, see [chamber-association-lead-lists](https://github.com/willowridge1234/chamber-association-lead-lists), a companion guide from the same authors. A sibling guide, [memberclicks-directory-export-guide](https://github.com/willowridge1234/memberclicks-directory-export-guide), covers the same situation on Wild Apricot's sister platform.

**Commercial disclosure:** Rook Data Tools publishes a purpose-built [Wild Apricot Directory Scraper](https://apify.com/rook-data-tools/wild-apricot-directory-scraper) on the Apify platform, linked in the relevant section below. That link is to our own product, not an independent recommendation. Every other section of this guide stands on its own whether you end up collecting a handful of records by hand, get an export from the association directly, or use a tool — ours or anyone else's.

## Who this is for

- A salesperson or founder who wants a lead list of a Wild Apricot-hosted association's public members.
- A researcher, journalist, or comparison shopper who needs a defensible snapshot of a public membership roster.
- An association staffer evaluating a *different* association's public directory, who doesn't have (and shouldn't ask for) that association's own admin login.
- Anyone who looked for the obvious "export" button and found it either doesn't exist for them or sits behind a login they don't have.

This guide does not cover accessing your own association's data as its administrator — Wild Apricot's own help center already covers that in detail — and it does not cover accessing any member-only or login-gated area under any circumstance.

## First, recognize a Wild Apricot site

Confirm you're actually looking at a Wild Apricot directory before assuming anything about export options or rules, since those differ by platform.

Look for explicit vendor branding: "Wild Apricot" or "WildApricot" in the site footer, sign-in screen, or help links, or a hosted domain ending in `wildapricot.org`. But don't stop there — Wild Apricot explicitly supports embedding a member directory into a completely different, custom domain using its [member directory widget](https://gethelp.wildapricot.com/en/articles/476-member-directory-widget), so a directory can look entirely native to an association's own site with no `wildapricot.org` URL anywhere. If branding is inconclusive, record the platform as unknown rather than guessing from layout.

## Why the built-in export and API usually aren't available to you

Wild Apricot's own help documentation describes two genuine ways to pull directory data, and both require an administrator login to that specific association's account:

1. **Contacts/Members export.** From [Exporting members and contacts](https://gethelp.wildapricot.com/en/articles/152-exporting-members-and-contacts): an admin opens the Contacts or Members module, filters or selects records, clicks Export, chooses a format (XLS, CSV, or XML) and which fields to include, and receives a download link — but only "if you are logged into your Wild Apricot account as an administrator, and in admin view."
2. **The Wild Apricot API.** Wild Apricot runs [two REST APIs](https://gethelp.wildapricot.com/en/articles/182-using-wildapricot-s-api) — an admin API and a member API — described as "intended for use by developers with technical expertise." Every call requires an authentication token obtained through OAuth 2.0 from `oauth.wildapricot.org`, using either an API key issued from that account's own Authorized Applications screen or a Wild Apricot user's login credentials (see [API authentication](https://gethelp.wildapricot.com/en/articles/484-api-authentication)). None of the supported authentication paths work without a relationship to that specific account.

If you are not that association's administrator (or a developer they've authorized), neither tool exists for you, no matter how the directory looks from the outside. Three honest paths follow:

1. **Ask.** If your use case is legitimate — a partnership, a sponsorship, a research project — ask the association directly for an export or API access. This is the only path that can also get you fields the public page doesn't show, and many associations will say yes to a clearly stated, reasonable request.
2. **Use whatever the association already published for visitors.** Some associations post a public roster, PDF list, or downloadable file outside the login. Check the directory page and its FAQ/help links before assuming none exists.
3. **Collect only what the public directory page already displays to any visitor**, without logging in, if neither of the above applies and the site's own rules allow it. The rest of this guide is about doing that third path correctly and knowing when not to.

## What a public Wild Apricot directory may show — and what it won't

There is no single Wild Apricot record format. Wild Apricot's own [Member directory gadget](https://gethelp.wildapricot.com/en/articles/390-member-directory-gadget) documentation makes this explicit: the administrator "can control which members appear in the directory, restricting the list by membership level, member groups, or saved searches," and separately "control which fields are displayed for each member and the order in which member records are sorted" — up to four columns, each combining up to three database fields. An association can also run multiple directory pages with different settings, for example one per chapter or region.

On top of the admin's settings, **individual visibility is layered per member.** Wild Apricot's [member privacy settings](https://gethelp.wildapricot.com/en/articles/147-member-privacy-settings) documentation defines "public" precisely: *"anyone who is not a member of your organization and anyone who may view your website without logging in."* Each member profile has a "Show profile to others" setting — if unchecked, "the member will not be listed in the member directory" at all — plus separate field-level "Show details" controls admins can lock so individual members can't override them. A directory you can see, in other words, is already the association's and its members' filtered view, not the full membership database.

### Fields commonly visible on a public profile, when the association and member have both chosen to show them

- organization or individual member name;
- membership level, category, or member type;
- city, region, or full mailing address;
- a phone number;
- a website;
- a public email address, when published;
- a link to that member's own profile page;
- any other custom field the association added and chose to display.

### What a public directory almost never gives you

- anything behind the member login — private profile fields, member-to-member messaging, event rosters, invoices, or renewal status;
- fields an admin marked hidden, or a member individually chose to hide;
- reliable revenue, headcount, budget, or purchasing authority;
- confirmation that a named person still holds that role;
- confirmation that a displayed email or phone is actively monitored;
- current buying intent, timing, or need — membership is not a purchase signal;
- the association's underlying export files, reports, or admin/API-only data.

Treat an empty field as "not shown to the public," not as evidence the association has no such information. Don't fill gaps with guesses.

## The line that matters: public directory vs. member-only area

Wild Apricot's own [page access and visibility](https://gethelp.wildapricot.com/en/articles/189-page-access-and-visibility) documentation describes restricting any site page — including the one holding the directory gadget — "to selected membership levels" or "to site administrators only." Many associations run their whole directory this way; others leave it open to any visitor. Both are legitimate configuration choices the association makes, not a mistake to be worked around.

- **In scope:** whatever a directory page shows to an ordinary visitor with no account, no payment, no invitation link, and no bypass of any access control.
- **Out of scope, always:** anything that requires signing in as a member, requesting member access, using someone else's credentials, or working around a login wall, a paywall, or a bot challenge. If the only way to see a field is to be a logged-in member, that field is not part of a public collection — full stop, regardless of how the field is displayed once you're inside.

This guide, and the actor linked below, only address the case where the directory page itself is genuinely reachable without logging in.

## Whose policies actually govern the data

This is the same trap that applies on Wild Apricot's sister platform, MemberClicks: **the software vendor's privacy policy is not the association's privacy policy.** Wild Apricot's own [privacy policy](https://www.wildapricot.com/legal-center/privacy-policy) states that when personal data "was collected by or on behalf of a WildApricot customer, then such personal data is controlled by our customer, with WildApricot acting as a data processor" — meaning the operative rules for a specific directory live on that association's own site, under its own privacy policy and terms of use, not on wildapricot.com. Read the association's own pages before deciding what's appropriate.

## Respect the site's own rules, robots.txt, and rate limits

This section is operational guidance, not legal advice, and applies to any public directory, not just Wild Apricot's.

- Read the association's terms of use and privacy notice for that specific site before collecting anything, and don't proceed if they prohibit your intended use.
- Check the site's `robots.txt`. The [Robots Exclusion Protocol](https://www.rfc-editor.org/rfc/rfc9309.html) is the standard way a site communicates crawler access preferences. Wild Apricot's platform-default `robots.txt` already disallows automated access to a number of internal system paths — login, search, admin screens, and the internal service the directory page itself uses to load member data — and states an explicit crawl-delay and request-rate limit that applies site-wide. Treat all of this as a floor, not a grant of permission: a path that isn't disallowed can still be off-limits under the site's own terms.
- Keep any automated request volume conservative regardless of what a platform default allows, and stop or slow down at the first sign of rate-limit responses, errors, or strain on the site. Never rotate identities, defeat bot challenges, or retry aggressively to push past a block.
- Never attempt to reach anything behind the member login, under any framing — that includes borrowing a member's credentials, joining specifically to reach non-public data, or treating "visible after I log in" as if it were public.

## Preserve provenance

A directory-sourced record is only as trustworthy as its documented source. For every record, keep:

- the exact profile URL, when the directory links to individual profile pages (Wild Apricot directories commonly do — see "Where the actor fits" below for what our own tool preserves);
- the association's name and site domain (note that a Wild Apricot-powered directory may be embedded on a domain that doesn't mention Wild Apricot at all — see "First, recognize a Wild Apricot site");
- the date and time you collected it;
- the raw values as displayed, before any cleanup or normalization;
- which membership level or category the record appeared under, since the same association may run several differently-scoped directory pages.

If the association later migrates platforms or reconfigures its directory, that provenance note is also what tells you why a later collection looks structurally different.

## Deduplicating Wild Apricot-sourced records

Full deduplication method is covered in the [companion cross-platform guide](https://github.com/willowridge1234/chamber-association-lead-lists#how-to-clean-and-deduplicate-a-directory-sourced-list); Wild Apricot has one structural wrinkle worth knowing before you start.

Wild Apricot supports **bundle memberships** — one paid membership shared across several individual people, common for households, family memberships, or small firms with multiple staff. The gadget settings include a "Show bundle coordinator only" option specifically because, as Wild Apricot's own documentation notes, when it's checked "individual members of bundles" are excluded from the directory listing while remaining "accessible from the bundle coordinator's profile." Depending on how a given association has configured that setting, a directory may show one row per bundle or one row per person within it. Decide up front whether your list unit is the bundle/organization or the individual, and don't silently treat a bundle coordinator and their bundle members as unrelated duplicates — or as the same record.

As with any directory source: keep the raw values, use more than one identity signal (domain, name, phone, address) before merging two records, and don't merge people just because they share an employer or a bundle.

## When automation is the wrong call

Don't automate collection of a Wild Apricot directory when:

- the directory, or the fields you actually need, sits behind a member login;
- the site's terms, robots.txt, or an explicit request from the association say no;
- the directory is small enough to review and copy by hand in a few minutes — a script adds risk for no real benefit;
- what you actually need is verified, current, or private information no public profile will ever contain — the honest fix is asking the association, not scraping harder;
- the association has already told you no, or you have a live relationship where simply asking is faster and cleaner.

Automation is a convenience for the *public, at-scale, repetitive* case. It is never a workaround for a login wall or a "no."

## Where the actor fits

If you've confirmed the site's directory runs on Wild Apricot (or a Wild Apricot directory embedded elsewhere), confirmed it's genuinely public with no login required to view it, and checked the association's own terms and `robots.txt`, we publish the [Wild Apricot Directory Scraper](https://apify.com/rook-data-tools/wild-apricot-directory-scraper) on Apify for exactly that job.

What it does, plainly, per its current public Apify listing: it crawls public Wild Apricot member directories — on `wildapricot.org` domains or a custom domain the directory is embedded into — and turns visible member listings into structured records, with fields such as available names, organizations, categories, phone numbers, emails, websites, addresses, and each member's own profile URL when the directory exposes one, output as JSON, CSV, or Excel-ready data. It only works against directories that are reachable without logging in; it has no path into a member login, and it isn't built or intended to reach anything behind one. It runs pay-per-event on Apify — billed per member record actually extracted, so cost scales with what a given directory actually returns rather than a flat run fee. Check the listing itself for current per-record pricing, since pricing can change.

In the interest of not overselling a new listing: it is new, has no reviews yet, and we don't have independent evidence of how many people have used it beyond ourselves. Judge it on the Apify listing's own current stats and a small test run against a directory you already understand, not on anything claimed here.

We don't publish how it identifies or reaches directory data — consistent with the rest of this guide, the goal is a described outcome, not a technique write-up.

## Final checklist

Before collecting anything from a Wild Apricot directory:

- [ ] Confirmed the site is actually Wild Apricot, from visible evidence, or marked it unknown — including the possibility it's embedded via widget on a non-`wildapricot.org` domain.
- [ ] Checked whether the association would simply provide an export or API access if asked.
- [ ] Confirmed the specific page and fields you want are visible to an ordinary visitor with no login.
- [ ] Read that association's own terms of use and privacy notice — not Wild Apricot's vendor-level policy — and confirmed nothing there prohibits your use.
- [ ] Checked `robots.txt` and treated it as a floor, not a green light.
- [ ] Planned conservative request volume and a stop condition if the site shows any strain.
- [ ] Decided your list unit (organization, bundle, or individual) before collecting, given how Wild Apricot's bundle memberships can be displayed.
- [ ] Have a plan to preserve profile URL, association name, membership level/category, and collection date per record.
- [ ] Ruled out that what you actually need is private, member-only, or unverifiable from a public profile.

## Frequently asked questions

### Can I export a Wild Apricot member directory as an outside visitor?

Not through Wild Apricot's built-in export or API tools — both require an association administrator login or an admin-issued API key. If the association hasn't offered you a file and the directory is genuinely public, you're limited to collecting what an ordinary visitor can already see on the page, subject to the site's own terms and robots.txt.

### Does Wild Apricot have a public API for member directories?

Wild Apricot's admin and member APIs are built for that specific account's authenticated use — every call needs an OAuth token tied to that account, obtained with an API key or a Wild Apricot user's login. There's no separate public API for an outside visitor to query another association's directory data.

### What information is public on a Wild Apricot directory?

It depends entirely on how that association configured its directory gadget and how each individual member set their own privacy options — there's no single Wild Apricot record shape. Commonly public fields include member or organization name, category, location, phone, website, and a profile link; anything a member marked hidden, or the admin excluded from the directory's displayed fields, won't appear.

### Is scraping a public Wild Apricot directory legal?

There's no universal answer, and this isn't legal advice. Staying outside any login wall, respecting the association's own terms and `robots.txt`, keeping request volume conservative, and using only what's already visible to an ordinary visitor are the baseline conditions. Separately, collecting a public business contact doesn't by itself authorize marketing to it — see the FTC's [CAN-SPAM compliance guide](https://www.ftc.gov/business-guidance/resources/can-spam-act-compliance-guide-business) for US email rules and the UK ICO's [business-to-business marketing guidance](https://ico.org.uk/for-organisations/direct-marketing-and-privacy-and-electronic-communications/business-to-business-marketing/) for a jurisdiction where B2B rules differ from consumer rules. Get legal advice for a high-risk or large-scale use.

### Why isn't Wild Apricot's privacy policy the one that governs this data?

Because Wild Apricot is the software vendor, not the data owner. Its own privacy policy states that when personal data is collected by or on behalf of a customer association, that data "is controlled by our customer, with WildApricot acting as a data processor" — so the operative rules for a specific directory live on the association's own site.

### What's a "bundle" membership, and why does it matter for a directory export?

A bundle is one Wild Apricot membership shared by several individual people — common for households or small firms. Depending on the association's directory settings, a bundle can appear as a single coordinator row or as separate rows per member. Knowing which pattern applies before you collect changes what counts as a duplicate.

## The useful standard

A responsibly collected Wild Apricot directory export is not the biggest file you can pull. It's a traceable set of the records that association actually chose to make public, collected without touching anything behind its member login, respecting its own rules, and honest about what it can and can't tell you about intent, timing, or authority. If you need more than that, the association — not a workaround — is the right next step.

## Related

Other free workflows and guides we publish:

- [n8n-ai-lead-scoring](https://github.com/willowridge1234/n8n-ai-lead-scoring) — Free workflow — score scraped leads against your ICP, log to Google Sheets
- [n8n-review-intent-lead-scoring](https://github.com/willowridge1234/n8n-review-intent-lead-scoring) — Free workflow — score G2/Capterra reviewers by switching intent
- [n8n-tradeshow-exhibitor-lead-scoring](https://github.com/willowridge1234/n8n-tradeshow-exhibitor-lead-scoring) — Free workflow — score trade-show exhibitors against your ICP
- [n8n-lead-scoring-guide](https://github.com/willowridge1234/n8n-lead-scoring-guide) — Guide — which signals predict a good lead, and how to tell if scoring works
- [chamber-association-lead-lists](https://github.com/willowridge1234/chamber-association-lead-lists) — Guide — building B2B lead lists from chamber & association directories
- [memberclicks-directory-export-guide](https://github.com/willowridge1234/memberclicks-directory-export-guide) — Guide — exporting a public MemberClicks member directory
- [new-liquor-license-data-guide](https://github.com/willowridge1234/new-liquor-license-data-guide) — Guide + tool — building a lead list from public liquor-licence records
- [chicago-food-service-license-data-guide](https://github.com/willowridge1234/chicago-food-service-license-data-guide) — Guide + tool — building a lead list from Chicago food-service licence records
- [membershipworks-member-directory-export-guide](https://github.com/willowridge1234/membershipworks-member-directory-export-guide) — Guide + tool — exporting a public MembershipWorks member directory
- [chambermaster-directory-export-guide](https://github.com/willowridge1234/chambermaster-directory-export-guide) — Guide — exporting a public ChamberMaster or GrowthZone member directory
