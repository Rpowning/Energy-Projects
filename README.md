Methanol Price Estimator

A small end-to-end project I built to teach myself how commodity price prediction actually works, using methanol as the case study.

The .ipynb files are where the actual work happens. These are Jupyter notebooks — Python code that loads real data, engineers features, trains a LinearRegression model, and evaluates it. 
This is the only place any real computation, model fitting, or data pulling occurs. Methanol_Price_Prediction.ipynb builds the pipeline on synthetic data; Methanol_Price_Prediction_Real_Data.ipynb 
swaps in real pulled data from FRED and Methanex and adds lagged price features.

The .html file is a static readout of the trained model — it doesn't compute anything new. 
Once the notebook finishes training, I copied the model's learned coefficients, intercept, and scaler values out of the notebook and hardcoded them into this file's JavaScript. 
Moving a slider just reruns that same math (scale the input → multiply by its coefficient → sum everything → add the intercept) instantly in the browser. No Python, no server, 
no re-training — it's a frozen snapshot of whatever model the notebook produced, wrapped in a UI so the results are actually usable instead of sitting in a notebook cell.

In short: notebook trains it, dashboard displays it. If I retrain the model on new data, I'd manually update the numbers baked into the HTML to match.

Approach
Picked features by mechanism (cost-push: natural gas, crude oil, USD strength; demand-pull: housing starts, China industrial production, gasoline) before pulling any data
Built the full pipeline (one-hot encoding, scaling, chronological train/test split) on synthetic data first, then swapped in real FRED + Methanex data
Added lagged methanol price (1/3/12 months back) as features — the 1-month lag ended up the strongest predictor by far
Found that Region and FeedstockType are perfectly collinear in this data, which zeroed out the feedstock coefficients
Results

R² of 0.833, MAE of ~$30/MT on a chronological (not random) held-out test set.

Files
Methanol_Price_Prediction.ipynb — pipeline built on synthetic data
Methanol_Price_Prediction_Real_Data.ipynb — real-data version with lag features
methanol_price_estimator.html — interactive dashboard (open directly in a browser)
