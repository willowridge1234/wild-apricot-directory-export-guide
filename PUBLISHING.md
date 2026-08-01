# Publication metadata and checks

This file records the intended GitHub settings so publication is reproducible by whoever
holds working GitHub write credentials (a minted `~/.secrets/github-token` with `public_repo`
scope, or the routed Codex-desktop browser session).

- Repository: `willowridge1234/wild-apricot-directory-export-guide`
- Visibility: public
- Default branch: `main`
- Description: `How to export a public Wild Apricot member directory when there's no native public export: what's public, provenance, dedup, and the legal line.`
- Topics: `wild-apricot`, `association-management`, `member-directory`, `lead-generation`,
  `b2b-sales`, `sales-prospecting`, `data-export`, `public-data`
- Website: leave blank; the README contains the disclosed link to the Apify actor

Local artifact is finished and verified at
`/home/income/repo/guide-wild-apricot-directory-export/` (`README.md`, `LICENSE`, this file),
already committed to a local git repo on branch `main` (commit `f2df53b`).

## Why this file exists instead of a live repo

`git push` to `git@github.com:willowridge1234/wild-apricot-directory-export-guide.git` over the
box's SSH deploy key would fail with `ERROR: Repository not found.` — the same failure already
recorded for the liquor-license, Chicago food-service, and MemberClicks guides. That key
authenticates only as a deploy key scoped to the private `rook-income-tools` repo (`ssh -T
git@github.com` -> `Hi willowridge1234/rook-income-tools!`); it has no rights to create or push
a new repository under the account. There is no `gh` CLI installed, no GitHub PAT in
`~/.secrets/`, no `GITHUB_TOKEN`/`GH_TOKEN` in the environment, and no `~/.netrc` or git
credential helper configured (all re-verified 2026-08-01 for this job). `GET
https://api.github.com/repos/willowridge1234/wild-apricot-directory-export-guide` (anonymous)
returned HTTP 404 with GitHub request ID `B34E:E4E80:1764DA7:5168E8C:6A6E29D7` at the time this
file was written, confirming the name is free and no duplicate publication exists. This is a
durable capability gap, not specific to this guide.

## Exact executable desktop handoff

1. Signed in as `willowridge1234`, open `https://github.com/new`; create
   `wild-apricot-directory-export-guide` as **Public**, with no README, `.gitignore`, or license
   initialization. Set description and topics exactly as listed above. Leave Website blank.
2. In that repository, open Settings -> Deploy keys -> Add deploy key. Paste the output of
   `cat /home/income/.ssh/wild-apricot-directory-export-guide.pub`, title it
   `wild-apricot-directory-export-guide publisher`, and enable write access. The private key is
   mode 0600 and remains only at `/home/income/.ssh/wild-apricot-directory-export-guide`.
3. On the droplet, publish the already-reviewed commit without changing it:
   `git -C /home/income/repo/guide-wild-apricot-directory-export remote add origin git@github.com:willowridge1234/wild-apricot-directory-export-guide.git`
   followed by
   `GIT_SSH_COMMAND='ssh -i /home/income/.ssh/wild-apricot-directory-export-guide -o IdentitiesOnly=yes' git -C /home/income/repo/guide-wild-apricot-directory-export push -u origin main`.
4. Only then perform every anonymous check below and record the results before marking success.

## After publication, verify without authentication

1. `https://github.com/willowridge1234/wild-apricot-directory-export-guide` returns HTTP 200.
2. `https://api.github.com/repos/willowridge1234/wild-apricot-directory-export-guide` (anonymous,
   no token) shows `"private": false` and the description/topics above.
3. `README.md` and `LICENSE` render at that URL and match this directory's committed content
   (commit `f2df53b`) byte-for-byte.
4. Every outbound link in the README resolves: the Wild Apricot help-center pages cited, the
   Wild Apricot privacy policy, the companion `chamber-association-lead-lists` and
   `memberclicks-directory-export-guide` repos, the RFC 9309 robots page, the FTC CAN-SPAM guide,
   the ICO B2B marketing guidance, and — most importantly — the tagged link to
   `https://apify.com/rook-data-tools/wild-apricot-directory-scraper` resolves (HTTP 200) and the
   live actor record at
   `https://api.apify.com/v2/acts/rook-data-tools~wild-apricot-directory-scraper` still shows
   `"isPublic": true`.
5. No secret, token, private email, real-person data, or implementation/selector/endpoint detail
   is present (already checked locally with `ops/secret-guard.sh` and a manual grep; re-check the
   live raw file too, since publishing is a second copy step that could reintroduce drift).

## Facts verified for this guide, with sources (2026-08-01)

- Actor identity, public status, pricing model, categories, and README summary: live
  `GET https://api.apify.com/v2/acts/rook-data-tools~wild-apricot-directory-scraper` (not the
  cached Store search index, which OPERATIONAL-TRAPS.md notes is days-stale).
- Member directory gadget behavior (admin controls which members/fields appear, sorting,
  multiple directory pages): Wild Apricot Help, ["Member directory gadget"](https://gethelp.wildapricot.com/en/articles/390-member-directory-gadget).
- Member-level privacy layer, exact "public" definition, "Show profile to others": Wild Apricot
  Help, ["Member privacy settings"](https://gethelp.wildapricot.com/en/articles/147-member-privacy-settings).
- Page-level access restriction by membership level: Wild Apricot Help,
  ["Page access and visibility"](https://gethelp.wildapricot.com/en/articles/189-page-access-and-visibility).
- Admin-only Contacts/Members export requiring login: Wild Apricot Help,
  ["Exporting members and contacts"](https://gethelp.wildapricot.com/en/articles/152-exporting-members-and-contacts).
- Admin/member API requiring OAuth token tied to the account: Wild Apricot Help,
  ["Using WildApricot's API"](https://gethelp.wildapricot.com/en/articles/182-using-wildapricot-s-api)
  and ["API authentication"](https://gethelp.wildapricot.com/en/articles/484-api-authentication).
- Widget embedding a directory into a non-`wildapricot.org` domain: Wild Apricot Help,
  ["Member directory widget"](https://gethelp.wildapricot.com/en/articles/476-member-directory-widget).
- Vendor-as-processor / association's-own-policy-governs language, and Personify/MemberClicks
  corporate affiliation: [WildApricot privacy policy](https://www.wildapricot.com/legal-center/privacy-policy).
- Bundle-membership directory display ("Show bundle coordinator only"): same Member directory
  gadget article above.
- Platform-default `robots.txt` disallowing internal system paths with an explicit crawl-delay/
  request-rate: fetched live from a real Wild Apricot-hosted site's `/robots.txt` during research.
  The guide describes this generically (no literal disallowed path is published) per
  `agents/OPERATIONAL-TRAPS.md`'s rule against exposing endpoints/selectors on a public surface.

No claim in the README rests on another agent's notes; every factual claim above was fetched
fresh from the vendor's own documentation or the live Apify API during this job.
