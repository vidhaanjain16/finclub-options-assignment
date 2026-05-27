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
logic: fit the 5 SVI parameters to each 5-minute timestamp. It treats a specific time to expiration(T) as a snapshot, fitting a curve directly to market data.  
      It follows this equation :  w(k) = a + b(rho(k-m) + sqrt{(k-m)^2 + s^2})  ; where--  
      1. w(k) is implied variance, and IV = sqrt(w/T), T is tte.  
      2. k is the log moneyness, calculated as k = ln(strike price/ underlying price)  
      3. a is the vertical shift  
      4. b is the steepness  
      5. rho is the tilt of the smile(-1 to 1)  
      6. m controls the horizontal shift( ie from at the money , ie from k=0)  
      7. s controls the curvature at the bottom of the smile  
      If the svi fails to find a mathematical solution, it falls back to cubic interpolation.   
result: MSE of around 0.0003, an improvement over interpolatrion  

### 4. Matrix Factorization (Sklearn Iterative Imputer)  
logic ;Using sklearn's iterative imputer , treats the entire volatility dataset as a single matrix. It runs regression using neighbour strike's values to fill a  missing value at a particular strike.   
First fill all the empty cells with some random guess,then run regression using neighbor values.   
it stops when the number of iterations reach 50, or if any iteration gives results very close to the previous one.  
result: gave best MSE of about 0.0001033

### 5. The Final Submission: 50/50 Ensemble
Logic: JUst took the 50/50 average of the submission files from SVI and Matrix Factorization  
Result: The 2nd best submission, with an MSE of 0.0001083

I tried some other startegies too, by mixing some of the best ones, or further refining interpolation results, but the results were not an improvement upon submission.
