# Listing Input wizard — field names and step codes

Mapped live on 2026-10-10 against a real member account (Residential class).

Selectors are the form `name` attributes. Labels and layout change; the names are the contract.
Fill only from the Propaired listing package, which is keyed by these same names.

## Create Listing modal (`form#new_listing_form`)

| Field | Name | Values |
|---|---|---|
| Property Class | `class` | `1` Residential · `3` Land · `2` Multi Unit · `5` Commercial · `6` Farm · `7` Residential Lease |
| Base on MLS# (optional) | `listno`, plus a keep-address checkbox | Clones an existing listing — the fast path for a relist or a prior sale |
| Agent | `agent` (with `office`) | Defaults to the logged-in member |
| Co Agent | `coagent_input` → hidden `coagent` | Type-ahead |
| Listing Name | `title` | Internal draft label only |

"Start Listing" creates a resumable draft. Its id is the `hbid` used by `jumpStep`.

## Wizard steps — `jumpStep(code, hbid)`

| Step | Code | Holds |
|---|---|---|
| Property Overview | `1` | Every field below. The left panel counts Required / Errors / Warnings with jump links |
| Floor Plan | `128` | Drag-and-drop uploader; rights warranty |
| Photos | `2` | Uploader; the MLS auto-tags room type and search tags; photo tags and descriptions |
| Open House | `4` | Schedule list |
| Listing Documents | `8` | PDF upload with a title |
| Showings | `64` | Aligned Showings · a different product · none |
| Data Distribution | `32` | Public display permissions · IDX and VOW · syndication · limited marketing |
| Verify & Publish | `16` | Summary, Prev, Cancel Listing, Save as Draft — and Publish only when valid and broker-permitted |

Property Overview toolbar: Print PDF · Save · Save as Draft · Delete Draft · Preview Listing.

## Property Overview fields

**Status and price:** `LIP_status` (Active | Coming Soon), `LIP_dt_coming_soon`, `LIP_listprice`

**Address:** `LIP_nonstandaddress`, `LIP_housenum`, `LIP_dir_pre` (radio N/E/S/W), `LIP_street`,
`LIP_dir_post`, `LIP_streettype`, `LIP_unitnbr`, `LIP_quadrant` (NW/NE/SW/SE), `LIP_coord_ns`,
`LIP_coord_ew` (Utah grid coordinates), `LIP_zip`, `LIP_city_disp`, `LIP_county_disp`, `LIP_state`,
`LIP_subdivision`, `LIP_zoningchar`.
The map point needs the address first, then *Verify Map*.

**Schools:** `LIP_schooldistrict` (41 districts) cascades into `LIP_schoolelem`,
`LIP_schooljunior`, `LIP_schoolsenior`; plus `LIP_schoolother`. Set the district first or the rest
stay empty.

**Community and HOA:** `LIP_has_hoa`, `LIP_hoa_fee`, `LIP_hoa_terms`, `hoa_transfer_type`
(percent of price or dollars), `LIP_hoa_transfer_fee`, `LIP_hoa_contact`, `LIP_hoa_phone`,
`LIP_hoa_voicetdd`, `LIP_hoa_amenities`, `LIP_hoa_remarks`, `LIP_hoa_rental_cap`,
`LIP_short_term_rental`, `LIP_projectrestrict`, `LIP_seniorcommunity`, `LIP_maint_free`

**Accessory dwelling unit:** `LIP_adu`, `LIP_adu_attach`, `LIP_adu_sqft`, `LIP_adu_beds`,
`LIP_adu_baths`, `LIP_adu_kitchen`, `LIP_adu_enter`, `LIP_adu_gas`, `LIP_adu_water`, `LIP_adu_elec`,
`LIP_adu_rented`, `LIP_adu_rent`, `LIP_adu_remarks`

**Pets and solar:** `LIP_pets`, `LIP_animals`, `LIP_has_solar`, `LIP_dt_solar_install`,
`LIP_solar_lease_own`, `LIP_solar_leasing_co`, `LIP_solar_finance_co`

**Site and tax:** `LIP_premarket`, `LIP_taxid`, `LIP_taxes`, `LIP_proptype`, `LIP_pud`, `LIP_pid`,
`LIP_style`, `LIP_yearblt`, `LIP_yearremodeled` (effective year built), `LIP_watershares`,
`LIP_conststatus`

**Lot and parking:** `LIP_dim_acres`, `LIP_dim_frontage`, `LIP_dim_side`, `LIP_dim_back`,
`LIP_dim_irregular`, `LIP_frontface`, `LIP_cap_garage`, `LIP_cap_carport`, `LIP_cap_parking`,
`LIP_rv_pkg_height`, `LIP_rv_pkg_length`

**Rooms by level.** Prefixes `LIP_lev4_`, `LIP_lev3_`, `LIP_lev2_`, `LIP_lev1_`, `LIP_levb_`
(basement 1), `LIP_levb2_`, `LIP_levb3_`, and totals `LIP_tot_`. Each takes the suffixes
`sqf`, `bed`, `bathfull`, `bathtq`, `bathhalf`, `famroom`, `den`, `formalliving`, `kitchen_k`,
`kitchen_b`, `dining_f`, `dining_s`, `laundry`, `fireplace`.

The kitchen and dining columns are easy to mix up: `kitchen_k` is an eat-in kitchen, `kitchen_b` is
a dining bar, `dining_f` is formal dining, `dining_s` is semi-formal.

Also `LIP_master_bedroom` (the primary bedroom's level), and `LIP_deck` / `LIP_patio` as counts.

**Basement:** `LIP_basement` (multi-select), `LIP_basmnt_fin` (percent finished)

**Listing info:** `LIP_dt_list`, `LIP_dt_expire`, `LIP_owner`, `LIP_owner_type`,
`LIP_contact_type`, `LIP_contact`, `LIP_contact_ph1`, `LIP_contact_ph2`, `LIP_listtype`,
`LIP_shortsale` (special listing conditions), `LIP_possession`, `LIP_office`

**Feature multi-selects** (checkbox groups): `LIP_accessibility`, `LIP_aircon`, `LIP_amenities`,
`LIP_driveway`, `LIP_env_certification`, `LIP_exterior`, `LIP_features_ext`, `LIP_floor`,
`LIP_garage`, `LIP_heating`, `LIP_inclusions`, `LIP_exclusions`, `LIP_features_int`,
`LIP_landscape`, `LIP_lotfacts`, `LIP_has_pool`, `LIP_has_spa`, `LIP_has_community_pool`,
`LIP_pool`, `LIP_roof`, `LIP_show`, `LIP_storage`, `LIP_telcom`, `LIP_terms`, `LIP_utilities`,
`LIP_water`, `LIP_window`, `LIP_zoning`

**Remarks:** `LIP_publicremarks`, `LIP_realtorremarks` (agent-only), `LIP_exclusions_remarks`,
`LIP_directions`. Each has its own character counter.

## Validation

A new draft starts with 23 required fields and 3 errors. The messages seen on an empty draft:
"Need Sq. Ft. 1+ or enter a Totals number below", "Enter Total Bedrooms", "Enter Total Bathrooms".

Resolve every error the package can answer, then list the rest for the realtor.

## Status names

The MLS interface and the data feed use different words for the same state:

| In the wizard | In the feed |
|---|---|
| Active | Active |
| Backup | Active Under Contract |
| Under Contract | Pending |
| Active-No Show | Hold |
| Coming Soon | Coming Soon |
| Withdrawn | Withdrawn |
| Sold | Closed |

The Residential class covers single-family, townhouse, condominium, mobile home, twin and
recreational. Duplex through five-plus units are Multi Unit.
