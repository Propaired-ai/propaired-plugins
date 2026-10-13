# MLS member profile — field map

Mapped live on 2026-10-10. Fill from the Propaired member-profile package, show the realtor the
before-and-after, and let them click Save. Never save a profile change for them.

## Profile — `/preferences/profile/` (`form#formOffice`)

| Field | Name | Comes from |
|---|---|---|
| NRDS id | `AgtNRDS` | Not Propaired-managed; leave as found |
| Cell phone | `AgtPhone1-{area,local,num}` | Their cell phone |
| Office phone | `AgtPhone3-{area,local,num}` | Their office phone |
| Public email | `AgtEmail` | Their public email |
| Website | `AgtURL` | Their website |
| Facebook / X / Instagram | `url[facebook]`, `url[x]`, `url[instagram]` | Their social links |
| Designations | `designations[]` | Their certifications. 41 checkboxes (ABR, CRS, GRI, SRES, e-PRO, EcoBroker, Green, Veteran and others) — tick only the ones Propaired holds, and never tick one the realtor has not earned |
| Photo | upload | Their profile image |

## Public profile — `/preferences/profile/public.profile`

This is the "Find an Agent" listing consumers see.

| Field | Name | Comes from |
|---|---|---|
| Show the profile | `profileSwitch` | The realtor's choice — never flip it for them |
| Show phone | `mpnSwitch` | The realtor's choice |
| Show sales activity | `msaSwitch` | The realtor's choice |
| Neighbourhoods | `AgtNeigh` | Their service areas |
| Languages | `AgtLang1` and the following language fields | Languages they speak |
| Biography | `AgtBio` | Their long bio |

The three switches change what the public sees about them. Fill the content fields, point out the
switches, and leave the decision with the realtor.

## Other surfaces

| Surface | URL | Notes |
|---|---|---|
| Client communication | `/preferences/communication/` | Email signature and header on client reports. Fill from the realtor's brand kit |
| Work As permissions | `/preferences/permissions/` | Delegates inside the office. Not Propaired-managed |
| Name, office or account-type changes | the in-house forms area | Not self-service. These are submitted forms, so hand the realtor the form rather than trying to edit a field |

## Forms library

Around 110 forms by category. Two things to know before clicking one:

- Opening a form **saves a copy into the member's saved forms**. It is not a read-only preview, so
  do not browse the library to "check" something.
- Copyright marks matter: one asterisk is the Utah Association of REALTORS®, two is the Utah CCIM
  chapter, three is the **Utah Division of Real Estate** — the state forms, including the purchase
  contract, its addenda, lead-based paint, financing and earnest money.

Contract forms are the realtor's and their brokerage's responsibility. Point at the right form;
never fill or file one.
