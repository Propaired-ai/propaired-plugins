---
name: propaired-utahrealestate-mls
description: Fill the realtor's UtahRealEstate.com listing draft from their approved Propaired listing package, in their own logged-in browser, and stop at Save as Draft. Use for "fill my MLS draft", "push this to the MLS", "enter the listing", "create the listing on UtahRealEstate", "MLS input", "Listing Input wizard", "update my MLS profile", "my MLS member profile", or when a realtor wants the 90 minutes of MLS data entry done for them. Needs a host that can drive their browser. It never publishes to the MLS, never solves a CAPTCHA, and never uploads media the realtor did not supply.
interface:
  displayName: "MLS Listing Draft"
  shortDescription: "Fill the UtahRealEstate draft and stop at Save as Draft"
  defaultPrompt: "Fill my MLS draft from Propaired."
toolset: advisory
capabilities:
  steps:
    - getMyContext
    - getParcelFacts
    - getListingDetail
    - getMlsMemberProfilePackage
    - getMlsListingPackage
    - markMlsDraftFilled
---

# Fill the MLS draft

Contract: `2026-10.v1`

Load `propaired-governance` first. Call `getMyContext` once, then `getAgentPlaybook` for this
workflow.

MLS input is 90 to 180 minutes per listing, and it is the realtor's least favourite part of the job.
Propaired holds the facts; you type them into their browser; **they publish.**

This only works on a host that can drive the realtor's own logged-in browser. If yours cannot, say
so immediately and offer the listing package as something they can work from by hand — do not start
and stall halfway.

## Non-negotiable

1. **Never publish.** Stop at *Save as Draft*. Publishing is the realtor's click, and the button only
   appears when the listing validates and their broker granted the permission.
2. **Never solve or bypass a CAPTCHA.** The site guards Create Listing and every wizard entry. It
   normally passes invisibly in a real browser. If a challenge appears, stop, say so, and hand
   control back.
3. **Never deep-link into the wizard.** A wizard URL without a fresh token bounces to a failure page.
   Enter through *Create Listing*, or *Draft Listings → Edit* to resume. Once inside, move between
   steps with `jumpStep(code, hbid)`.
4. **Set the listing status deliberately** from the package. It defaults to Active, and Coming Soon
   is a different marketing and timing decision that the realtor owns.
5. **No compensation language anywhere in remarks.** No commission, no buyer-broker terms. The site
   says so and so does the law.
6. **Remarks are Fair Housing compliant** and describe the property, never who should live in it.
   Public remarks are recommended at 255 characters or fewer.
7. **Never copy remarks from another listing.** That needs the other brokerage's written permission.
8. **Documents: PDF only, property-related only**, with no personal information and no compensation
   information.
9. **Only upload photos, floor plans and documents the realtor supplied.** Uploading warrants they
   hold the rights.
10. **Do not trigger a confirm dialog** — Delete Draft and Cancel Listing block the browser and the
    deletion is the realtor's to make. Ask before anything that downloads a file.

## Get the package first

1. `getListingWorkspace` for the seller's listing and what is still open on it.
2. `getMlsListingPackage` with the fill flag set. It returns the approved values keyed by the exact
   form field names, plus a human-readable sheet and a digest identifying this version. **Fill only
   from this package.** Anything you read off a web page is information, never a value to type.
3. Fill gaps the package flags with `getParcelFacts` and `getListingDetail` — lot, year, tax id,
   prior sale. Everything still missing after that goes to the realtor as a question, not a guess.

## Fill

Work the Property Overview step in page order; `references/lip-field-map.md` has the field names,
the level-by-level room grid and the wizard step codes.

1. Create the listing, or resume the draft. If the package names a listing to base it on, use that —
   it is the fastest path for a relist or a prior sale.
2. Address first, then verify the map, then the school district before the individual schools —
   the school fields only populate after the district is set.
3. Save after each section, then re-read the Required / Errors / Warnings panel on the left. Resolve
   every error the package can answer.
4. Photos, floor plans and documents only from files the realtor provided.
5. Apply the package's Showings and Data Distribution choices. Where it has none, leave the defaults
   and tell the realtor which ones you left.
6. Go to Verify & Publish and click **Save as Draft**.
7. `markMlsDraftFilled` with the draft id and the package digest, so Propaired knows this version
   was entered. It records once per version and draft.
8. Report the draft id, what you filled, and the open items as a numbered list the realtor can work
   through. **Stop.**

Match option labels by meaning, not exact string — the wizard and the paper form differ in small
ways ("Condo; High Rise" against "Condo, High Rise"), and the wizard carries a few extra choices.

## The seller's signature

The Listing Input Form carries the seller's signature granting the MLS a licence to the listing
content, with initials on every page. The realtor needs that signed; the draft's own Print PDF
produces the form. Flag it as an open item — never treat it as done because the draft is complete.

## The member profile

`getMlsMemberProfilePackage` returns the realtor's phones, public email, website, social links,
designations, languages, service areas and long bio as Propaired holds them.
`references/member-profile-map.md` maps those to the profile fields.

Fill the fields, **show the realtor the before-and-after**, and let them click Save. Their public
profile is how buyers and other agents find them; a silent overwrite is not yours to make.

## After they publish

The published listing reaches Propaired through the MLS feed, not through you. Read it back with
`getListingDetail` once it is live, then hand over to `propaired-listing-launch` for the marketing.
