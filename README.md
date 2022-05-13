# systemRx


Author: Sheneka Allen


---


# Business problem:
>This model will use a maintenance dataset to estimate the percent likelihood of Machine Failure (target).

# Data:
>Since real predictive maintenance datasets are generally difficult to obtain and especially publish, 
>this synthetic dataset reflects real predictive maintenance encountered in industry to the best of my knowledge.

>Original Data Source: Source of data: https://archive.ics.uci.edu/ml/datasets/AI4I+2020+Predictive+Maintenance+Dataset#

>Credits: Dua, D. and Graff, C. (2019). UCI Machine Learning Repository [http://archive.ics.uci.edu/ml]. 
>Irvine, CA: University of California, School of Information and Computer Science.

# Attribute Info:

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

**Tool Wear Failure (TWF):** the tool will be replaced if fail at a randomly selected tool wear time between 200 and 240 mins (120 times in this dataset). At this point in time, the tool is replaced 69 times, and fails 51 times (randomly assigned).

**Heat Dissipation Failure (HDF):** heat dissipation causes a process failure, if the difference between air- and process temperature is below 8.6 K and the tool's rotational speed is below 1380 rpm. This is the case for 115 data points.

**Power Failure (PWF):** the product of torque and rotational speed (in rad/s) equals the power required for the process. If this power is below ~3612 W (Watts) or above ~7853 W, the process fails, which is the case 95 times in our dataset.

**Overstrain Failure (OSF):** if the product of tool wear and torque is between 11,550 - 12,000 min Nm, the process fails due to overstrain. This is true for 98 datapoints.

**Random Failures (RNF):** Each process has a chance of 0 or 1 % to fail regardless of its process parameters. This is the case for only 19 datapoints, a surprisingly small number for 10,000 datapoints in this dataset.

---


# Insights and Results: 



---
# Recommendations:

1. Which model performed the best? Random Forest Classifier

2. Which hyperparameters did you tune for the models?
>For Logistic Regression

>For RF, I tuned max_depth=2 which reduced variance and bias.

3. Is there a model that you liked the best and why? I liked RF better because it's slightly easier to tune, especially to reduce overfitting


# For further information:
For any additional questions, please contact: https://www.linkedin.com/in/shenekaallen/