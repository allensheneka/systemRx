# systemRx


Author: Sheneka Allen


---


## Business problem:
>This model will use a maintenance dataset to estimate the percent likelihood of Machine Failure (target).

## Data:
>Since real predictive maintenance datasets are generally difficult to obtain and especially publish, 
>this synthetic dataset reflects real predictive maintenance encountered in industry to the best of my knowledge.

>Original Data Source: https://archive.ics.uci.edu/ml/datasets/AI4I+2020+Predictive+Maintenance+Dataset#

>Credits: Dua, D. and Graff, C. (2019). UCI Machine Learning Repository [http://archive.ics.uci.edu/ml]. 
>Irvine, CA: University of California, School of Information and Computer Science.

## Attribute Info:

The dataset consists of 10,000 data points stored as rows with **_14 features_** in columns--

**UID:** unique identifier ranging from 1 to 10000

**Product ID:** consisting of a letter L, M, or H for low (50% of all products), medium (30%) and high (20%) as product quality variants and a variant-specific serial number

**Air temperature [K]:** generated using a random walk process later normalized to a standard deviation of 2 K around 300 K (degrees kelvin, a thermodynamic temperature measurement)

**Process temperature [K]:** generated using a random walk process normalized to a standard deviation of 1 K, added to the air temperature plus 10 K.

**Rotational speed [rpm]:** calculated from a power of 2860 W, overlaid with a normally distributed noise

**Torque [Nm]:** torque values are normally distributed around 40 Nm (Newton-meter) with a standard deviation of 10 Nm and no negative values.

**Tool wear [min]:** The quality variants H/M/L add 5/3/2 minutes of tool wear to the used tool in the process.

**Machine failure (target):** Indicates whether the machine has failed in this particular datapoint for any of the following failure modes are true. (If at least one of the below failure modes is true, the process fails and the 'machine failure' label is set to 1. It is therefore not transparent to the machine learning method, which of the failure modes has caused the process to fail)

The machine failure consists of **_five_** independent failure modes:

**Tool Wear Failure (TWF):** the tool will be replaced if fail at a randomly selected tool wear time between 165 and 250 mins (46 times in this dataset). At this point in time, it is not particularly clear from this dataset how often the tool is replaced prior to its failure.

**Heat Dissipation Failure (HDF):** heat dissipation causes a process failure, if the difference between air- and process temperature is between 309.5 - 312 K and the tool's rotational speed is between 1250 - 1395 rpm. This is the case for 115 data points.

**Power Failure (PWF):** the product of torque and rotational speed (in rad/s) equals the power required for the process. If this power is below ~3612 W (Watts) or above ~7853 W, the process fails, which is the case 95 times in our dataset.

**Overstrain Failure (OSF):** if the product of tool wear and torque is between 11,550 - 12,000 min Nm, the process fails due to overstrain. This is true for 98 datapoints.

**Random Failures (RNF):** Each process has a chance of 0 or 1 % to fail regardless of its process parameters. This is the case for only 19 datapoints, a surprisingly small number for 10,000 datapoints in this dataset.

---


## Insights and Results: 


This heatmap highlights the following:
>1.  HDF, OSF and PWF independent failures (scores .58, .53, .52, respectively) are strongly correlated with Machine Failure.
>2.  Rotational Temperature and Air Temperature have a strong postive correlation with .88 scores
>3.  Rotational Speed and Torque have a strong negative correlation with a -.88 score

![systemRx_heatmap](https://user-images.githubusercontent.com/100389581/168302971-ec15d067-80a6-4e21-83e6-64c0a7a4145c.png)


This barplot shows Type independent failure totals (by percent) and clearly graphs the TOP 3 categories:
>1. HDF which precede ~33% of machine failures (1)
>2. OSF which precede ~28% of machine failures
>3. PWF which precede ~26% of machine failures

![systemRx_TypeMF](https://user-images.githubusercontent.com/100389581/168302674-5a3aa8a7-b4cd-463b-9840-2967474d483a.png)


This scatterplot shows the highest operational occurrence for heat dissipation failure (HDF) that lead to machine failures:

![systemRx_HDF](https://user-images.githubusercontent.com/100389581/168303099-b6e8c580-30e3-4572-8d20-7ef5fc76bcf0.png)





---
## Summary:

Overall, the XGBClassifier model performed decently as evidenced by its classification report heatmap below which indicates 80% precision and an average of 65% recall.  Translation: basically 0 (.049%) false positives and 35% false negatives for this model. Accuracy is 99%, very high and expected, since the data is unbalanced and in favor of 'NO Machine Failures'.


![systemRx_cm_r1](https://user-images.githubusercontent.com/100389581/169563095-9e29cab3-ae94-45e1-b1d6-af48d193a9a3.png)



I also explored using the RandomForestClassifier model to see if the classification report metrics would be different, but they were surprisingly similar, if not the same with minor tuning of the hyperparameters.

**It is important to emphasize that this dataset does NOT distinguish between a machine failure resulting from 1) component replacement performed during preventive maintenance activities and 2) component run-time failure/fatigue.  This sort of machine failure analysis must be evaluated outside of this statistical model.**

## For further information:
For any additional questions, please contact: https://www.linkedin.com/in/shenekaallen/
