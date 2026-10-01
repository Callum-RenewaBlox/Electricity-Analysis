# RenewaBlox — Electricity Analysis

Client electricity analyses by RenewaBlox, served as Streamlit apps.

## Citygate Church — Electricity Portrait

`streamlit_app.py` serves `citygate_electricity.html`: fourteen months of
half-hourly meter readings for Citygate Church (138a Holdenhurst Road,
Bournemouth), told in plain English for the church's leaders, who are not
energy people.

The page reads top to bottom as a briefing:

* **The short version** — the four ways the data points to lower bills, what
  each could be worth in a year and how ready it is, in the dark summary panel
  (pinned beside the page on a wide screen, under the introduction on a narrow
  one), with a "Talk to us" button.
* **Three words that make the rest easy** — kW, kWh and kVA, one card each,
  with an everyday comparison (a speedometer, miles travelled, seats booked in
  a hall).
* **01 The whole picture** — every half-hour of the fourteen months in one
  heat map, with numbered markers tied to the notes beneath it and Christmas
  and Easter flagged.
* **02 A week in the life** — an average winter week against an average summer
  week, hour by hour.
* **03 Where the electricity goes** — the always-on use against the extra used
  on weekdays, evenings and weekends.
* **04 Through the year** — each month's use, with August and September set
  against the same months of 2025.
* **05 The size of the connection** — the 250 kVA connection drawn as a
  250-seat hall, and every day's busiest half-hour against it.
* **06 What this could be worth** — the four savings in detail and a sensible
  order to take them in.

Every chart has a hover (and tap) read-out, a table or text alternative, and
is drawn from the embedded data at the container's real width, so labels stay
legible on a phone. The header's print button prints the page to A4, or saves
it as a PDF.

### How the page is built

It is one self-contained, hand-authored HTML file with no CDN and no
requests, so edit it directly:

* **Brand.** The RenewaBlox house style: the wordmark (inlined PNG), the petrol
  tokens (`--brand:#12475e`, `--accent:#1f5f7f`), numbered finding cards and
  the dark summary panel. Inter, the brand face, is inlined too: the Latin
  subset of the variable font (weights 100–900, SIL Open Font License) as a
  WOFF2 data URI, so clients see it on machines that don't have Inter
  installed.
* **Data.** `DATA` in the page holds 424 days × 48 half-hours of demand in
  tenths of a kW (`heat`), each day's busiest half-hour in kW (`dpk`), an
  average winter (Nov–Feb) and summer (May–Aug) week by hour (`week`), and the
  monthly totals (`monthly`). The "where the electricity goes" split is
  computed from `heat` over the 365 days to 28 September 2026: each day's
  always-on level is its average from 1am to 5am, and anything above it is
  counted against the time of week it was used.
* **Colour.** Winter `#21709b` and summer `#d4721a` were checked as a pair for
  colour-blind separation and contrast on white; the heat map uses a single
  petrol ramp from light to dark.
* **Streamlit.** The app strips Streamlit's chrome and gives the page the whole
  viewport; the page scrolls inside its frame. In-page links scroll with
  script rather than following `#anchors`, because inside Streamlit's `srcdoc`
  frame a bare anchor resolves against the app's own URL and would load the app
  inside itself.

### What the figures rest on

The figures were checked against the embedded meter data by two independent
recomputations, and the tax and network-charge statements against HMRC, SSEN,
DCUSA and NESO publications, as of 1 October 2026:

* "A year" is the 365 days to 28 September 2026 (161,500 kWh). The readings run
  from 1 August 2025 to 28 September 2026: 20,350 half-hours, leaving out the
  hour the clocks skipped in March.
* The meter records kW. The connection is sized in kVA, which for this building
  is a little higher (an 80 kW peak is roughly 80 to 90 kVA), so the page
  recommends confirming the busiest kVA figure on the invoices before the
  connection is reduced.
* VAT on qualifying electricity — domestic use and a charity's non-business
  use — is 0% from 1 October 2026 to 31 March 2027 (HMRC Revenue and Customs
  Brief 10 (2026)), then 5%. If at least 60% of the building's use qualifies,
  the whole supply does.
* Network residual bands were reset on 1 April 2026 and are fixed until
  31 March 2031, so a lower band from a smaller connection would start in April
  2031 at the earliest.
* The savings figures assume today's usage and prices, and overlap in places.

The meter is identified on the page by the last four digits of its MPAN only,
because this repository is public.

## Run locally

    pip install -r requirements.txt
    streamlit run streamlit_app.py

## Deploy

On Streamlit Community Cloud, point a new app at this repository, branch
`main`, main file `streamlit_app.py`. The repository is public, so the app
deploys as a public app. There are no secrets to set.
