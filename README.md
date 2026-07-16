# Gett: Insights from Failed Orders

Diagnosing ride-matching failures for Gett's corporate ground transportation platform using cancellation pattern analysis and geospatial hex clustering.

## Background

When a client requests a ride on Gett's app, the matching system searches for and offers the ride to nearby drivers. Not every request ends in a successful ride — some are cancelled by the client before a driver is even found, some are cancelled after a driver is assigned, and some are rejected outright by the system. This project explores *why* and *when* these failures happen, using order-level data.

## Data

- **data_orders.csv** — one row per failed order, including order time, pickup coordinates, estimated arrival time (ETA), order status (cancelled by client vs. rejected by system), whether a driver was assigned, and time to cancellation.
- **data_offers.csv** — maps each order to an offer ID.

## Analysis

The notebook (`gett_failed_orders_analysis.ipynb`) walks through:

1. **Distribution of failure reasons** — cancellations before/after driver assignment, and system rejections.
2. **Failed orders by hour of day** — volume and proportional trends across the 24-hour cycle.
3. **Average time to cancellation by hour**, split by whether a driver was assigned, with outlier trimming.
4. **Average ETA by hour**, to explain cancellation behavior.
5. **Bonus: Geospatial hex clustering** — using H3 (resolution 8) to find the smallest set of hexagons covering 80% of all failed orders, visualized on an interactive Folium map colored by failure count.

## Key Findings

- **Cancelled before assignment** is the most common failure type (4,496 orders), ahead of system rejections (3,409) and post-assignment cancellations (2,811) — most failures happen during the search phase, before a driver is even found.
- **Hour 8 (morning rush)** has the highest raw failure volume and the longest average ETA of the day (~623s), tying elevated system rejections directly to driver-supply strain during commute hours.
- **Overnight hours** show a disproportionately high share of system rejections, consistent with fewer drivers being online.
- **Hours 10–11** show an unusual spike in post-assignment cancellations (up to 57% of failures), which isn't explained by cancellation wait time or ETA — an open question worth further investigation with data outside this dataset.
- **Cancelling after a driver is assigned consistently takes ~2x longer** than cancelling before assignment, across every hour of the day.
- **80% of all failed orders are concentrated in just 24 of 144 total hexagons** (resolution 8), showing failures cluster tightly around a compact geographic core rather than spreading evenly.

## Tools

- Python, Pandas, Matplotlib
- H3 (geospatial indexing)
- Folium (interactive mapping)

## How to Run

1. Clone this repo
2. Install dependencies: `pip install pandas matplotlib h3 folium branca`
3. Open `gett_failed_orders_analysis.ipynb` in Jupyter or VS Code
4. Run all cells top to bottom

The interactive hex map is also available standalone as `failed_orders_map.html` — open it directly in any browser.
