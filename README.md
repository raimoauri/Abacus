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

## Inventory & sales (FIFO)
Each catalogue line is a received lot. Sales (entered manually or imported from a Vektori /
WooCommerce CSV) are matched to products by code (SKU/EAN/ISBN) then by name, and deplete the
**oldest lot first (FIFO)**. From that the app derives per-item stock on hand, per-invoice
sell-through, and ROI (realized profit ÷ amount invested in the batch). Unmatched sales are
flagged. Sales live in the same core file as a `sales` collection; they can split to a separate
file if volume grows.

**Inherited / pre-existing stock.** A product sold in Vektori that was never brought in through
an invoice here (legacy or inherited stock) can be marked **inherited** — at import time (tick it
in the "not in your catalogue" list) or later from the Sales list. Inherited sales are recognized
rather than flagged unmatched, relate to no invoice, and carry no cost basis, so they are left out
of ROI. The registry is a per-mode `inherited` list of `{code, name}` kept in the core file.

## Development
The whole app is one file, `index.html` (React + htm, inlined UMD builds). Changes are committed
here and go live on Pages after a refresh.
