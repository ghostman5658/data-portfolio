# Combined Cycle Power Plant Dataset

**Source:** [UCI Machine Learning Repository](https://archive.ics.uci.edu/dataset/294/combined+cycle+power+plant)

**Citation:** Pınar Tüfekci, Prediction of full load electrical power output of a base load operated combined cycle power plant using machine learning methods, International Journal of Electrical Power & Energy Systems, Volume 60, September 2014, Pages 126-140.

## Description

9,568 data points collected from a Combined Cycle Power Plant over 6 years (2006-2011) when the plant was set to work with full load.

## Variables

| Column | Description | Unit |
|--------|-------------|------|
| AT | Ambient Temperature | °C |
| V | Exhaust Vacuum | cm Hg |
| AP | Ambient Pressure | milibar |
| RH | Relative Humidity | % |
| PE | Net hourly electrical energy output | MW |

## Files

- `CCPP/Folds5x2_pp.xlsx` - Excel format (original)
- `CCPP/Folds5x2_pp.ods` - OpenDocument format
- `ccpp.csv` - CSV (create via `pd.read_excel('CCPP/Folds5x2_pp.xlsx').to_csv('ccpp.csv')` if needed)

## Usage

```python
# Load from Excel (requires openpyxl: pip install openpyxl)
df = pd.read_excel('data/CCPP/Folds5x2_pp.xlsx')

# Or from CSV if converted
df = pd.read_csv('data/ccpp.csv')
```
