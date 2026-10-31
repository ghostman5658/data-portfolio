# Listing data

The sale-price notebook reads `realtor-data.zip.csv`. The file is about 171 MB, which is over GitHub's 100 MB file limit, so it is not part of this repository.

## Download

1. Open the USA Real Estate Dataset on Kaggle:

   https://www.kaggle.com/datasets/ahmedshahriarsakib/usa-real-estate-dataset

   Or, with the Kaggle CLI already authenticated:

   ```bash
   kaggle datasets download -d ahmedshahriarsakib/usa-real-estate-dataset
   ```

2. Download the listings file and save it in this folder, next to `analysis.ipynb`, as `realtor-data.zip.csv`. If the CLI downloads a zip archive, unzip it and keep that CSV name.

   The notebook checks `../realtor-data.zip.csv` first, then `realtor-data.zip.csv` in the working directory. Run the notebook from this folder and use the second location.

3. Do not commit the CSV.

The file is a public compilation of Realtor.com-style listings (Sakib, n.d.). The model uses sold rows with a sale date from October 18, 2021, through May 6, 2022. Cleaning rules are in `analysis.ipynb`.
