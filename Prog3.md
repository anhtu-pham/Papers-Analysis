# Programming Assignment 3
**Instructions:**  Only one notebook/program is needed from a group unless otherwise specified. Please make sure to write well-documented code using good variable names. Each person in each group must commit and push their own work. You will not get credit for work committed/pushed by someone else even if done by you. Each person is expected to do an approximately equal share of the work, as shown by the git logs. **If we do not see evidence of equal contribution from the logs for someone, their individual grade will be reduced substantially.** Please comment the last commit with "FINAL COMMIT" and **enter the final commit ID in canvas by the due date.**

Some data are called *time series*. Here we have features and outcomes indexed by time. Download the "us_consumption.csv" file from Canvas. This dataset has a time series that describes the quarterly percentage changes of real personal consumption expenditure (RPCE), percentage changes of real personal disposable income (RPDI), percentage changes in industrial production (IP) and personal savings (PS), and  changes in the unemployment rate (UR) for the US from 1960 Q1 to 2016 Q3.

Write a Julia program "linreg.jl" containing the following functions.
1. Write a function "linreg" that, given a matrix of features, an associated response vector and a regularization coefficient, estimates the linear regression parameters. You may use matrix operations from the Julia libraries, but do not use any builtin functions that solve linear regression or computes the $R^2$ metric. (30 points) 
2. Write a function "predict" that, given a vector of parameters for linear regression and a matrix of features, outputs predicted responses for the given data. (10 points)
3. Write a function "rsq" that, given a predicted response vector, a true response vector and a mean prediction, computes and returns the $R^2$ coefficient of determination. (5 points)

Create a jupyter notebook called "timeseries.ipynb". In it create the following cells. Treat RPCE as the response and the other features as the inputs (don't forget to add an intercept).
1. Load the csv file into a dataframe. Add a column of 1's as an intercept "feature".
2. Plot the response variable RPCE against time (quarters). (5 points)
3. On the same plot, add RPDI against time (quarters). Observe the similarity in their trends. (5 points)
4. Use your linreg function to estimate a regression model for RPCE using RPDI and an intercept as the features. Use a regularization coefficient of zero. Print the weights estimated as "Intercept" and "Weight for RPDI". (5 points)
5. Use your predict and rsq functions to find the $R^2$ metric for your model. Print it out. (5 points)
6. Plot the RPCE and the estimated RPCE (output by your predict function) against time (quarters) on the same plot. (5 points)
7. Now use your linreg function to estimate a regression model for RPCE using all of the other features (RPDI, IP, PS, UR) and an intercept. Use a regularization coefficient of zero. Print the weights estimated as "Intercept", "Weight for RPDI:", "Weight for IP:", etc. (5 points)
8. Use your predict and rsq functions to find the $R^2$ metric for this model. Print it out. Does the $R^2$ metric improve? (5 points)
9. Plot the RPCE and the estimated RPCE (output by your predict function) against time (quarters) on the same plot. (5 points)
10. Using the absolute values of the weights found, sort and output the four features by order of importance in decreasing order (most important feature first). (5 points)
11. Output the $R^2$ values for five models using different regularization coefficients of 0, 0.001, 0.01, 0.1 and 1. Is there a trend in the $R^2$ values? (5 points)
12. Now divide the data into two. One set (the "training set") will contain the data from 1960Q1 to 2010Q4. The other (the "test set") will contain the rest of the data from 2011Q1 to 2016Q3. Use linreg to estimate a regression model for RPCE using all of the other features and an intercept and a zero regularization coefficient, using the training set only. Then use predict and rsq to output the $R^2$ value for the *test set* (note that the mean prediction will be different from before as well). How does this $R^2$ compare to the previous $R^2$ metrics you found? (5 points)
13. Static predictive time-series models like the above lose predictive power over time. To counter this we can move to a sequential procedure, where we repeatedly re-estimate model parameters for each time step using *all the previous data*. So we would build a model for 2011Q1 using all the features and responses up to 2010Q4, then another model for 2011Q2 using all the features and responses up to 2011Q1 and so on. In this case each quarter will have its own model. Create a sequence of linear models like this for the data points in the test set. Use each model to make a prediction for the associated quarter. Then compute and print an $R^2$ value for these predictions. Does it improve over Q12? (10 points)
14. (Bonus) An *autoregressive model of order 1* (AR(1))also uses the *response at step $t-1$* as a feature when predicting the response at step $t$. Build AR(1) models for each quarter in the test set (note that the first quarter, 1960Q1, won't have a response feature for $t-1$; you can use zero). Compute and print the $R^2$ value for the test set predictions. Does it improve over Q13? (10 points)
15. (Bonus) The sequential model building process in Q13/14 is computationally expensive especially as the data grows over time. Can you find a way to *update* the models instead of re-estimating their weights from scratch? Write an "update" function in linreg.jl to do this. Your update can use the previously computed and stored weights, matrix inverses, or partial products, without recomputing them. (Hint: the following can be helpful: http://www.cs.nthu.edu.tw/~jang/book/addenda/matinv/matinv/ , look at "Special case 1.") Then in timeseries.ipynb, use your update function to solve Q14 again, and print out the times taken with and without the update. (10 points)