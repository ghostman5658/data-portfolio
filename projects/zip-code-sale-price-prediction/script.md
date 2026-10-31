# Speaker script

ZIP Code-Aware Sale Price Prediction. Week 4, Milestone 3.

Read this while the slides are up. Each block is one slide. At a natural pace this is about 7 to 8 minutes, inside the 5 to 10 minute window. Sentences marked optional can be skipped if you are running long.

## Slide 1

This is a screening estimate of what a house sold for, using beds, baths, living area, lot size, and ZIP code. The sales run from October 18, 2021, through May 6, 2022. It sits beside recent comps. It is not an appraisal, and it is not a number for a loan file.

## Slide 2

Buyers and agents need a price before an offer. A national median is the wrong tool, because an extra bathroom is not worth the same amount in every ZIP. Price the home too high and the deal stalls. Price it too low and the seller leaves money on the table.

I asked three questions. How close can those house features, plus the ZIP, get to the sale price? How much does the ZIP help, compared with a model that ignores location and compared with the ZIP's own median? And does the miss change in California, Texas, and Florida, and in ZIPs that have only a few sales?

## Slide 3

I started with about 812,000 sold listings. About one in four is missing beds, baths, living area, or lot size, so those rows cannot support this model. That cut is not neutral. The dropped homes have a median price of about 249,000 dollars. The homes I kept have a median of about 370,000 dollars. Florida is a much larger share of the dropped rows than of the kept rows. California is the reverse.

I also set aside ZIPs with fewer than 20 sales when I report the headline error. Those homes are cheaper too, about 250,000 dollars against about 380,000 dollars in the ZIPs I score, and they are much less likely to be in California. So the 15 percent I am about to quote describes documented houses in deeper, higher-priced ZIPs. It does not describe land, incomplete listings, or a ZIP with a handful of sales.

## Slide 4

I fit the models on the log of price, because sale prices have a long right tail, and I scored them in dollars. The headline number is the median absolute percent error. That is the typical miss.

I compared five predictors. A national median. The ZIP median, built only from training sales. A regression on beds, baths, size, and lot, with no location. That same regression plus the ZIP median. And a gradient boosting model on those same inputs. A five-fold check rebuilds the ZIP median inside each fold, so a home's own sale price is never part of the feature used to score it.

## Slide 5

On ZIPs with at least 20 training sales, the national median misses by about 42 percent. Beds, baths, and size, with no location, only get that to about 36 percent. The ZIP median alone gets to about 21 percent. Adding the house, and then letting boosting bend the fit, gets to 14.8 percent. The average dollar miss is about 99,000 dollars. Cross-validation lands within a tenth of a point of that 14.8. The ZIP does most of the work. The house adjusts it.

## Slide 6

This is the result I would not want an agent to miss. When the ZIP has fewer than 10 training sales, the typical miss is about 27 percent. At 100 or more sales, it is about 13 and a half percent. The model still beats the ZIP median in every bin. The 15 percent figure is for ZIPs that already have a real set of comps. Below 20 sales, I would not show a firm number.

## Slide 7

California is where a national model fails. Without location, the typical miss there is about 52 percent. The state's median sale in this file is 713,000 dollars, against 315,000 dollars in Texas, so a national anchor is far too low. Once the ZIP is included, California's typical percent error is 12.7 percent, the best of these groups. The dollar misses stay large, because the homes cost more. Optional: In Florida, the house features and the ZIP median help by about the same amount, and boosting still improves on both.

## Slide 8

Here is how I would use it. Start with the ZIP median. Adjust for living area and bathrooms. Show a range, not a point. A typical miss is about 15 percent, and about one in three estimates misses by more than 20 percent. If the list price is far outside that range, that is a flag to pull comps, not a price to write on an offer. Withhold a firm number under 20 sales. Do not reuse this fit outside late 2021 and early 2022 without refitting. The ZIP adjustment also restates neighborhood price levels, so it stays out of lending.

## Slide 9

Three answers. Where a ZIP has at least 20 sales, the boosted estimate is typically off by about 15 percent. ZIP information cuts that typical miss from about 36 percent to about 15 percent. Error is worse in thin ZIPs, and California is the market that falls apart without a ZIP and looks best once the ZIP is in the model.

That is the screen. Thank you.

## If someone asks

Do not read this during the timed talk. The full write-up is in the Week 4 notebook.

1. Sold price is the outcome an offer has to survive. Asking price is still a wish. Homes still on the market would answer a different question.
2. Use the estimate as a range next to comps. If the list price is about 30 percent off, pull comps. Do not set the offer from the point estimate.
3. The ZIP median is not leakage. It is built from training sales only, and cross-validation rebuilds it inside each fold. A test home's own price is not in its feature.
4. For a ZIP with eight sales, withhold a firm number. Under 10 training sales the typical miss was about 27 percent.
5. Once square footage is in the model, an extra bedroom mostly means smaller rooms. With ZIP held fixed, that bump is about 1 percent.
6. California's price level is far above the national median, so a national model misses by about half. After the ZIP is included, California has the best percent error of the large markets. Dollar error stays large because the homes cost more.
7. These error rates belong to late 2021 and early 2022. Refit on a current sold window before using the tool. The method can be reused. The 15 percent cannot.
8. Condition, year built, and schools are not in the file. That missing information is a large part of why a 15 percent miss remains.
9. The ZIP median restates neighborhood price levels, which track income and older patterns of segregation. Keep it as a comps screen. Keep it out of credit.
10. Most ZIPs do not have enough sales for their own model. One national model with a training-set ZIP median borrows strength, and thin ZIPs are still labeled low-confidence.
