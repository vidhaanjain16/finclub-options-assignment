# finclub options assignment

This repository contains my solution for finding missing Volatility surface points in Nifty 50 options. The primary goal was to reconstruct the volatility surface across time and strike dimensions while minimizing the Mean Squared Error (MSE).

Logic & Approach
To solve it, I tested the following models:

### 1. Linear Interpolation
Logic: It simply connects the known IV data points across strikes with straight lines. 
Result: fails to capture the curve  of the volatility surface. The reultant surface has sharp spikes and edges.

### 2. Cubic Spline Interpolation
Logic: draw smooth polynomial curves between known data points.
Result: An MSE of about 0.000469

### 3. Stochastic Volatility Inspired (SVI) Model
logic: fit the 5 SVI parameters to each 5-minute timestamp using non-linear least squares optimization.
result: MSE of around 0.0003

### 4. Matrix Factorization (Sklearn Iterative Imputer)
logic ;Using sklearn's iterative imputer , treats the entire volatility dataset as a single matrix.
result: gave best MSE of about 0.0001033

### 5. The Final Submission: 50/50 Ensemble
Logic: JUst took the 50/50 average of the submission files from SVI and Matrix Factorization
Result: The 2nd best submission, with an MSE of 0.0001083

I tried some other startegies too, by mixing some of the best ones, or further refining interpolation results, but the results were not an improvement upon submission.
