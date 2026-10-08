# Import Desk

A self-contained purchasing & pricing workbench for **Katolinen Kirjakauppa**. It logs supplier
invoices (goods) and purchase batches / own-published titles (books), works out net landed cost,
a suggested price and the margin, flags data-quality issues, and tracks what has been keyed into
Vektori. It runs entirely in the browser — no server, no build step.

## Use it
Open `index.html` in any browser, or visit the GitHub Pages URL. It works offline: the libraries
(React, htm) are inlined; fonts fall back to system fonts with no network.

## Your data
- Lives in **this browser** (localStorage), tied to the address the app is opened from.
- Export / import a **core data file** (`.json`) from *Settings → Your data*. It can be
  **AES-256 encrypted** with a passphrase.
- **Vektori remains the master record** — this is the workbench upstream of it.
- Data is **not** stored in this repository. Only the app code is. (An encrypted `.enc.json`
  may optionally be kept as an off-site backup, since it is opaque without the passphrase.)

## Pricing (net-cost basis)
Recoverable VAT is stripped from cost to a **net** figure; margin compares net cost to the
VAT-exclusive part of the retail price. Transport is spread per unit across an invoice/batch (RTC).
Per-line VAT overrides are supported; books use the reduced rate, own titles strip production VAT.

## Development
The whole app is one file, `index.html` (React + htm, inlined UMD builds). Changes are committed
here and go live on Pages after a refresh. Roadmap: inventory/stock, then a separate FIFO
**sales** file to track per-invoice sell-through and ROI.
