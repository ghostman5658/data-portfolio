# Movie Recommendation System

Suggest the next movie from how people have rated titles, without relying only on genre labels.

## Business question

Given a title a person already knows, which other titles drew similar ratings from the same kind of viewer?

## Who this is for

Someone designing a catalog feature. The notebook is a working item-item recommender on a teaching dataset, not a production ranking service.

## Data

MovieLens Small, from GroupLens: 100,836 ratings of 9,742 movies by 610 users. The files are in `ml-latest-small/`, with the GroupLens readme. The notebook uses `ratings.csv` and `movies.csv`.

## Methods

Keep movies with at least 10 ratings. Build a user-item matrix. Rank candidates by cosine similarity and return the 10 nearest titles.

## Results

Franchise titles and films from a similar era cluster together. The matrix is about 98 percent empty. New titles and new users are a cold-start problem, and computing a full similarity matrix is not the design you would ship. A production version would store neighbors for the titles people actually open, and it would need a separate path for a title or a user with no ratings.

## Ethics and limitations

- Similarity repeats what this group of 610 users rated. It can crowd out titles those users never saw.
- The notebook does not filter for audience or content. A catalog product would.
- MovieLens Small is a research extract. Cite GroupLens if you reuse the files. Their terms are in `ml-latest-small/README.txt`.

## How to reproduce

Clone the public repository and run this project from its folder:

https://github.com/ghostman5658/data-portfolio/tree/main/projects/movie-recommendation

```bash
git clone https://github.com/ghostman5658/data-portfolio.git
cd data-portfolio
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
jupyter lab
```

Open `projects/movie-recommendation/analysis.ipynb` and run all cells from that folder. The write-up PDF is [`paper.pdf`](paper.pdf).
