Please find the Project Report in repo to get a detailed Document.  

Financial Transaction Monitoring and Fraud Anomaly Detection using Time-Series Forecasting

1. Introduction  
Modern banking systems process a very large number of digital transactions every day. Manual monitoring of these transactions is impossible, and traditional fraud detection methods that inspect individual transactions may fail to detect coordinated abnormal activity occurring across multiple accounts. Therefore, financial institutions require automated systems capable of monitoring overall transaction behavior and identifying suspicious patterns.
Instead of predicting whether each transaction is fraudulent, this project focuses on behavioral monitoring of the financial system. The objective is to model normal transaction activity over time and automatically detect abnormal deviations that may indicate fraud waves, coordinated withdrawals, or system misuse.
This approach is known as predictive monitoring, where expected system behavior is learned using time-series forecasting and anomalies are identified when real activity deviates from the learned baseline.

2. Objective  
The main objectives of the project are:
	Model normal financial transaction activity over time
	Forecast expected transaction behavior using time-series analysis
	Detect abnormal activity automatically using anomaly detection techniques

3. Dataset Description  
The project uses the PaySim mobile money transaction dataset, which simulates real mobile banking transactions.
Each row represents a single financial transaction recorded by the banking system.
Important attributes:
	step – time progression (each step represents 1 hour)
	type – transaction type (CASH_OUT, TRANSFER, PAYMENT, etc.)
	amount – transaction value
	oldbalanceOrg, newbalanceOrig – sender account balances
	oldbalanceDest, newbalanceDest – receiver balances
	isFraud – fraudulent transaction indicator
Since individual transactions are highly irregular and noisy, the data was aggregated to obtain a system-level behavioral time series.

4. Data Preprocessing
The following preprocessing steps were performed:
	Selected relevant transaction types (CASH_OUT and TRANSFER) because these are commonly associated with fraudulent withdrawals.
	Converted the step variable into a daily time index:
day=step/24

	Aggregated all transactions per day by summing transaction amounts.
This process is called temporal aggregation, where individual events are transformed into behavioral trends over time.
The resulting dataset represents daily total outgoing transaction volume of the financial system.

5. Exploratory Data Analysis (EDA)
The daily transaction series was visualized.
 
Observations:
	A stable baseline transaction behavior was observed initially.
	A sudden large spike in activity occurred.
	A sharp drop followed the spike.
Interpretation:
The spike indicates abnormal liquidity movement, which may correspond to coordinated fraudulent activity or mass account withdrawals. The subsequent drop may indicate system response actions such as account freezing or transaction blocking.

6. Time-Series Modeling
6.1 Stationarity Check
An Augmented Dickey-Fuller (ADF) test was performed.
Result:
	p-value > 0.05 → non-stationary
Therefore, first-order differencing was applied to stabilize the series.

6.2 Model Selection
An ARIMA(1,1,1) model was used.
Reason for choosing ARIMA:
	Data showed short-term dependency
	No stable repeating seasonal cycle
	Objective was behavior monitoring, not long-term forecasting
The model learned the expected daily transaction behavior of the financial system.

6.3 Expected Behavior Forecast
The model generated predicted transaction values.
 
The predicted values represent the normal expected activity baseline of the financial system.

7. Anomaly Detection
Anomalies were detected using two independent approaches.

7.1 Residual-Based Detection (Z-Score)
Residuals were computed:
Residual=Actual-Predicted

Large residuals indicate abnormal behavior.
Residuals were standardized using Z-score:
Z=(x-μ)/σ

Days with:
∣Z∣>2.5

were flagged as suspicious.
 
This method detects contextual anomalies, meaning behavior inconsistent with expected activity patterns.

7.2 Isolation Forest (Machine Learning)
Isolation Forest was applied to daily transaction values.
The algorithm isolates rare points using random partitioning. Points isolated quickly are considered anomalies.
 
This method detects global anomalies, meaning unusually large or small transaction values.

7.3 Hybrid Detection
Both statistical and machine learning approaches identified the same abnormal activity period, strengthening confidence in the detection.

8. Results
The system successfully identified abnormal transaction activity corresponding to a major spike in withdrawals.
The following conclusions were observed:
	Forecast model established normal behavior baseline
	Residual deviations identified abnormal days
	Isolation Forest confirmed extreme transaction events
	Both methods agreed on suspicious activity period

9. Conclusion
This project demonstrates that financial systems can be effectively monitored using time-series forecasting combined with anomaly detection. Instead of analyzing individual transactions, monitoring system behavior allows detection of coordinated fraud patterns.
The ARIMA model provided an expected activity baseline, while residual analysis and Isolation Forest detected abnormal activity. The hybrid detection approach improved reliability and reduced false alerts.
The developed system represents a simplified version of real banking monitoring systems used to identify suspicious financial activity.

