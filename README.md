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
>1.  HDF, OSF and PWF independent failures (scores .46, .41, .41, respectively) are strongly correlated with Machine Failure.
>2.  Rotational Temperature and Air Temperature have a strong postive correlation with .86 scores
>3.  Rotational Speed and Torque have a strong negative correlation with a -.84 score

![sysRx_ups_hmap](https://user-images.githubusercontent.com/100389581/170729749-3fc535d2-bef2-490c-b251-52354754e76b.png)


This barplot shows Type independent failure totals (by percent) and clearly graphs the TOP 3 categories:
>1. HDF which precede ~34% of machine failures (1)
>2. OSF which precede ~28.49% of machine failures
>3. PWF which precede ~28.38% of machine failures

![typemf_ups](https://user-images.githubusercontent.com/100389581/170729932-ae80fe83-bfe3-4d34-b24b-a8ab852e45bf.png)


This scatterplot shows the highest operational occurrence for heat dissipation failure (HDF) that lead to machine failures:

![hdf_ups](https://user-images.githubusercontent.com/100389581/170729973-58697a4f-a781-4b70-861d-cd6fbc76d92e.png)






---
## Summary:

To maintain the integrity of the training and test model predictions, the five independent failure modes were dropped from the dataset during the train-test split.  This ensured one target, machine failure, was the focus of this analysis.  Overall, the XGBClassifier model performed quit well as evidenced by its classification report heatmap below which indicates 94% precision and an average of 96% recall.  Translation: basically (6.2%) false positives and 3.5% false negatives for this model. Accuracy is 95%, and I have a higher confidence in the model's performance after correcting the dataset imbalance by upsampling.


![XGBC_ups_cm](https://user-images.githubusercontent.com/100389581/170731090-6407e622-7f64-48dc-af37-add7e8d3c19d.png)




I also explored using the RandomForestClassifier model to see if the classification report metrics would be different, but they were surprisingly similar, if not the same with minor tuning of the hyperparameters.

**It is important to emphasize that this dataset does NOT distinguish between a machine failure resulting from 1) component replacement performed during preventive maintenance activities and 2) component run-time failure/fatigue.  This sort of machine failure analysis must be evaluated outside of this statistical model.**

## For further information:
For any additional questions, please contact: https://www.linkedin.com/in/shenekaallen/
