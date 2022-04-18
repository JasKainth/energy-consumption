# Energy Consumption
Creating predictive models for residential energy consumption.  

## Feature Engineering  

The data we have has a lot of variables (over 900 columns) and most of the data is categorical. They have integers as labels but refer to categorical data. To make use of this data, we will need to perform one hot encoding but first, we try to remove columns that might not help predict the energy use (KWH column).  

Based on the data codebook, when a column name starts with a 'Z', that means it tells us if another column had its value imputed or not. We will remove all of these columns. KWH is broken up into parts that show energy usage in different areas - we will remove these columns as KWH is just a sum of these. Also, we have different units of kWh (BTU & $) which will lead to look-ahead bias so these are also removed.  

After this, all the continuous columns are kept and for categorical data, if more than 75% of the values are the same then they are removed, otherwise, we keep them.  

### PCA  

To further perform dimensionality reduction (and remove correlation among variables) we use Principal Component Analysis. We retain 80% of the variance which leads to 270 columns.


## Machine Learning  

When creating models, we use two design matrices - the first is the PCA predictors and the other is the filtered columns. After conducting some literature review it seems the following models might be useful:  

* Linear Regression 
* Support Vector Machine 
* Random Forest 
* KNN  

Also, since we do have a lot of variables, we will also try LASSO and Ridge regression as tuning the penalty term may lead to reduced overfitting.  

The metric used to test the effectiveness of the model was Root Mean Squared Error (RMSE). Since the RMSE of the Null Model is the SD of the Y, we will test how much of the RMSE is reduced with the algorithms above relative to the SD of Y. The table below shows the results. 

### Summary

![Summary Table](https://github.com/JasKainth/energy-consumption/blob/main/summary-table.png)  

Using the table above, we see that the best model we had was the LASSO when not using the PCA predictors. However, the Linear Regression and Ridge Regression models perform almost as well (along with the SVM with PCA model).  
* A pattern that we see for all the models is, that when the actual KWH is on the higher end, our models generally underestimate the electricity. This might be something we want to take a look at when considering how to improve our model performance.  
* It seems like the best model we have is the LASSO which sets some of the coefficients of the $\beta$ matrix to 0. For the best model we have, we see that out of the possible 951 predictors, it is only using 518. So, it might be interesting to re-run the SVM and KNN with those 518 predictors and re-run the SVM, KNN and RF with the 221 PCA predictors. We aren't assured we will get a better model but it may be interesting to examine. 

## Appendix  

List of articles used (some of them are pdfs and will be downloaded once you click on them).  

https://www.nber.org/system/files/working_papers/w26531/w26531.pdf

https://www.mdpi.com/1996-1073/14/22/7810/pdf

https://www.sciencedirect.com/science/article/pii/S235271022101264X