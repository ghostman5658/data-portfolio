# TSA Complaint Trends

A traveler's view of where and when airport-security complaints pile up.

## Business question

Which airports and which kinds of problems generate the complaints, and what should a traveler do differently because of that?

## Who this is for

Everyday travelers, not an aviation analyst. The piece is a visual brief: a map, a heat map, and a box plot, with a recommendation at the end.

## Data

TSA complaint releases joined to a public airport-code table. This folder contains:

- `complaints-by-airport.csv`
- `complaints-by-category.csv`
- `complaints-by-subcategory.csv`
- `iata-icao.csv`

Duplicate monthly reports were removed, dates were parsed, and the map is limited to U.S. airports. The subcategory file is large, about 77 MB.

## Methods

Counts, a map, a heat map, and a box plot. There is no predictive model. The map reads a public U.S. states GeoJSON at run time and uses GeoPandas to draw the state outlines.

## Results

LAX, JFK, ORD, and SFO lead complaint volume. Damaged or missing checked baggage is the largest subcategory, and summer months spike at major hubs. The recommendation is to allow extra time at those airports, keep valuables in a carry-on, and file a complaint when screening or baggage handling goes wrong.

## Ethics and limitations

- Complaint volume follows passenger volume. A busy airport can lead the count without being worse per traveler. The brief is about where complaints pile up, which is the traveler's planning question.
- A filed complaint is not a finding that the agency was at fault.
- The files are a public release, not a live feed.

## How to reproduce

Clone the public repository and run this project from its folder:

https://github.com/ghostman5658/data-portfolio/tree/main/projects/tsa-complaint-trends

```bash
git clone https://github.com/ghostman5658/data-portfolio.git
cd data-portfolio
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
jupyter lab
```

Open `projects/tsa-complaint-trends/analysis.ipynb` and run all cells from that folder. The notebook looks for the four CSV files beside it, and it needs a network connection for the state map. The saved charts are already in the notebook if GeoPandas is slow to install. The write-up PDF is [`paper.pdf`](paper.pdf).
