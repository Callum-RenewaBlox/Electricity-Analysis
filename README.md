# RenewaBlox — Electricity Analysis

Client electricity analyses by RenewaBlox, served as Streamlit apps.

## Citygate Church — Electricity Portrait

`streamlit_app.py` serves `citygate_electricity.html`: fourteen months of
half-hourly meter readings for Citygate Church (138a Holdenhurst Road,
Bournemouth), told in plain English for the church's leaders, who are not
energy people.

The page opens on a chart, not a wall of text, and splits into three tabs:

1. **Charity consumption** (opens first) — a large donut of the last 12
   months' electricity in five sections, business use in amber (the café,
   room hire) and charity use in petrol (always on, weekday church
   activities, and everything else: Sundays, weekday evenings, early mornings
   and Saturday church events), each labelled with its central estimate,
   marked `~` and rounded so the sections add up to their group's share. A prompt above the
   chart invites a hover (a tap on a phone): the section lifts out, lights up
   and splits into its parts, and the card beside the chart lists each part's
   kWh. A bar underneath sets the ~79% charity share against the 60% line at
   which the whole supply gets the charity rate of VAT and no Climate Change
   Levy.
2. **Electricity portrait** — the findings in full: the year at a glance, then
   the whole year in one half-hourly heat map (numbered markers, Christmas and Easter flagged), a winter and a summer
   week, where the electricity goes, the months against last year, and the
   250 kVA connection drawn as a 250-seat hall.
3. **June 2027 simulation** — renewing with British Gas against moving to tem
   with RenewaBlox for the year from 9 June 2027, when today's British Gas
   contract ends, on the last 12 months of meter readings, like for like: both
   at 110 kVA with the charity rate of VAT (5%) and no Climate Change Levy.
   Each unit rate carries its broker's commission, shown in the open: 1.00p a
   kWh for British Gas's broker, Annex Solutions (in British Gas's renewal
   prices), and 0.5p a kWh for RenewaBlox (added to tem's quoted rates).
   British Gas's renewal standing charge includes the national network charge
   (TNUoS), and the Nuclear RAB levy is a separate line, as on today's bills;
   tem passes the network charge through at cost (its forecast for the site)
   and includes the RAB levy in its rates. The tab
   leads with the headline figure, the saving over tem's 2-year contract,
   then shows the two yearly totals, the two commissions side by side, every
   charge line by line, where the difference comes from, and its assumptions.

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
* **Charity consumption.** Business use is the café and room hire by
  Citygate Events (central estimates, kWh a year):
  * the café (19,300), from its opening hours (Monday to Friday 8.30am to 3pm,
    Sundays 10am to 2pm) at the middle of each range: fridges and chillers at
    about 1.1 kW around the clock (9,900), about 4 kW while open (6,600 on
    weekdays, 800 on Sundays) and heating the café on winter weekdays (2,000);
  * room hire (15,300), from the meter: Monday and Wednesday evenings, which
    have no church activity, plus a fifth of the church evenings (4,900);
    three quarters of Saturday use above the always-on level (5,100); two
    weeks of large events, 17–22 November 2025 and 5–10 January 2026 (2,500);
    and regular weekday bookings, the middle of 0–5,500 (2,800).
  Each part is taken out of the matching part of the split, and what is left is
  charity use — ~79% in all, against a range of about 73–85%. The charity
  sections' breakdowns come straight from the meter (overnight against
  daytime, winter against the rest of the year, the evening of the week).
  The Citygate Events booking diary and a month on a café sub-meter would firm
  the estimate up; the church's VAT adviser confirms the share.
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
