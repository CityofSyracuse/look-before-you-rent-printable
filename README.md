# look-before-you-rent-printable

**Single-file page (`index.html`)** that shows a print‑friendly table of **code violations** and **auto‑filters by SBL** passed in the URL. The Static HTML is then converted to a printable PDF using the HTML2PDF.js cdn. 

## Contents
- `index.html` — HTML + CSS + JS (Axios + Bootstrap)
- `assets/` — images (logo, etc.)

## How It Works
1. Map pop‑up opens: `https://cityofsyracuse.github.io/look-before-you-rent-printable/?sbl={SBL}`
2. `index.html` reads `?sbl` and `?address`, pre‑fills the search, hides the input on load, queries the ArcGIS layer, and renders a summary + table.
3. Open without params to use manual search.

## Add the Map Pop‑up Link (ArcGIS)
Layer → Configure Pop‑up → Custom attribute display (Enable HTML) → add:
- Basic link:  
  `<a target="_blank" href="https://cityofsyracuse.github.io/look-before-you-rent-printable/?sbl={SBL}">View Printable Version of This Map</a>`

## URL Parameters
- `sbl` (required for auto‑filter), e.g., `?sbl=096.-05-01.0`
- `address` (optional label), e.g., `&address=500-50%20Salina%20St%20S%20%26%20Onondaga%20St`

## Configure Inside `index.html`
- **Feature service URL:**  
  `const url = "https://services6.arcgis.com/bdPqSfflsdgFRVVM/arcgis/rest/services/Code_Violations_V2/FeatureServer/0/query";`
- **Fields used (case‑sensitive):** `SBL`, `complaint_address`, `violation_date`, `violation`, `comply_by_date`, `status_type_name` (optional).
- **Default WHERE (prefix match on SBL or address):**  
  `term ? "(complaint_address LIKE '${term}%' OR SBL LIKE '${term}%')" : "1=1"`
- **Date formatting:** use `new Date(value).toLocaleDateString('en-US',{year:'numeric',month:'short',day:'numeric'})`.

## Deploy
- Hosted `index.html` + `assets/` on (GitHub Pages).

## Test
- Direct: `yourindex.html?sbl=096.-05-01.0` → search prefilled, table filtered.
- From map: click **View Printable Version of This Map**.
- No params: manual search works. ?sbl=
- Summary shows: `N open violations for ADDRESS (SBL)`; dates like `Mar 15, 2024`.

## Troubleshooting
- No rows → verify exact SBL string; prefix match is used.

## Branch / PR
- Branch name: Always created a new branch when creating a new feature `git checkout -b` and name it `feature/[name of feature you are working on]`
- Files to touch: `index.html`, `assets/*` (branding)
