# The Bias-Variance Tradeoff: Choosing How Flexible a Model Should Be

## Objective
I wanted to see how the degree of a polynomial fit affects its error on new data, and to test whether cross-validation picks a good degree and gives an honest estimate of that error.

## Methodology
- I set up a simulated world with a known true curve, so I could measure error against the truth directly.
- At each of six polynomial degrees, I drew 200 fresh samples, fit a model to each one, and split the error into bias squared and variance.
- For degrees 1 to 15, I compared the training error with the error on 2,000 new points and found the degree where the held-out error was lowest.
- I chose a degree using 5-fold cross-validation and looked at how much the error changed from one fold to the next.
- I used nested cross-validation to check whether the smallest CV error is a fair estimate of the error on new data.
- I wrote a small function, `kfold_mse()`, that returns the k-fold CV error for any degree and number of folds, and I checked it using 10 folds.

## Key Findings
- Variance grew as the model became more flexible: it was 0.0180 at degree 5 and 0.0994 at degree 15.
- The held-out error on the 2,000 new points was lowest at degree 5.
- 5-fold cross-validation chose degree 4, with a CV MSE of 0.1401. This is close to, but not the same as, the degree that did best on the held-out points.
- Nested cross-validation gave an error estimate of 0.2070, which is higher than the smallest 5-fold CV error. This tells me that picking the lowest CV error and reporting it as the expected error on new data is too optimistic.
- With 10 folds, `kfold_mse()` gave a CV MSE of 0.1409 for degree 4 and 0.1457 for degree 5. Degrees 4 and 5 are very close, which fits with the 5-fold result and the held-out result pointing to neighboring degrees.
