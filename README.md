# coupon-analysis
## Overview - Will a customer accept the coupon?

The goal is to understand what drives a driver to accept or reject a coupon, using bar coupons and coffee house coupons as the primary case studies, and to translate those patterns into interpretable hypotheses.

## Dataset
Target variable: Y — binary, 1 if the driver accepted the coupon, 0 if rejected.
Coupon types: coupon column — Bar, Coffee House, Restaurant(<20), Restaurant(20-50), Carry out & Take away.
Contextual features: time, weather, temperature, passanger, destination, direction_same, toCoupon_GEQ5min/15min/25min.
Behavioral features: Bar, CoffeeHouse, RestaurantLessThan20, Restaurant20To50 — self-reported frequency of visiting each venue type.
Demographic features: age, maritalStatus, income, occupation, education, gender, has_children.

├── data/
│   └── coupons.csv          # raw dataset
├── images/
│   └── 
├── coupon_analysis.ipynb                # main analysis notebook
└── README.md                     # this file
## Methodology
### Data quality check — inspected shape, dtypes, and missing values.
**car** was found to have a very high missing rate and was excluded from analysis rather than imputed.
Several trip-distance indicator columns (toCoupon_GEQ5min) were constant across all rows and carried no information.
### Univariate exploration 
Visualized the distribution of coupon type and temperature using bar plots (chosen over histograms since both are effectively discrete/categorical in this dataset).
### Bar coupon deep dive
isolated coupon == 'Bar' and computed acceptance rate under a series of increasingly specific conditions:
Bar-visit frequency (≤3/month vs. >3/month)
Frequency + age > 25
Frequency + non-kid passengers + non-farming/fishing/forestry occupation
Compound OR conditions combining bar frequency, passenger type, marital status, age, and restaurant frequency/income
### Coffee house coupon exploration
Applied the same framework to coupon == 'Coffee House', using CoffeeHouse visit frequency, age, and passanger as candidate drivers of acceptance.
### Comparing groups
for each condition, reported:
The raw acceptance rate (descriptive statistic)
The percentage-point difference vs. the relevant baseline
### Visualization 
Utilized seaborn, matplotlib and Pandas visualization libraries for plotting different scenarios of Bar and Coffee House coupons. Used bar plots, countplot, stacked/normalized bar plots, and small-multiples grids to compare accepted vs. rejected groups across multiple features at once.

## Key Findings
### Bar coupons are accepted more often by drivers who already visit bars frequently (>1x/month), especially when combined with:
No children as passengers
Younger age (<30)
Not widowed
### Existing habit is the strongest single predictor
the coupon appears to function more as a reward for behavior the driver was already inclined toward, rather than as an incentive that changes behavior.
### Situational fit compounds behavioral fit — conditions that make the coupon usable in the moment (no kids in the car, appropriate time of day) raise acceptance further on top of baseline habit frequency.
### Acceptance rates differ meaningfully by coupon type overall, with bar and coffee house coupons landing on the lower end compared to restaurant/carry-out coupons.
### Data
**toCoupon_GEQ5min** — usually all 1s (every trip in this dataset is ≥5 min to the coupon location). A constant column has zero variance, so it carries no information and can't help distinguish acceptance vs. rejection. Safe to drop.
**toCoupon_GEQ15min and toCoupon_GEQ25min** — these do vary, and represent driving-distance thresholds. Not useless, but they overlap in information with each other (a coupon ≥25 min away is also ≥15 min away by definition) — so including both can introduce redundancy/multicollinearity if you're building a model.
**direction_same** — indicates whether the coupon venue is in the same direction as the driver's current travel direction. There's typically also a **direction_opp** column that's just the logical inverse (direction_same = 1 - direction_opp), so keeping both is redundant — one fully determines the other.    
