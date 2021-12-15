# Ventilator Pressure Prediction


## Team Members: 
Anamika Kumari, Ashay Aglawe, Kratika Shetty, Murlidhar Koripalli

## Background and Motivation
Ventilators have proven to be a lifesaver during the pandemic. However, mechanical ventilation is a clinician-intensive procedure, a limitation that was prominently on display during the early days of the COVID-19 pandemic.

A traditional pressure controlled ventilator lets air in and out of a patient’s lungs according to airway pressures specified by a clinician. These hand-tuned PID controllers and similar variants have dominated the industry for decades but these are very unreliable, overshooting or undershooting their values or fluctuating very fast. Hence a data driven machine learning approach can be considered as an alternative solution. The goal of the project, which is also a Kaggle competition, is to predict the airway pressure in the respiratory circuit during the breath given the time series of control inputs. We aim to develop a model that self-regulates as per the patient’s vital in real-time which can lead to future development of intelligent ventilators.



## Dataset
### Ventilator Data
The ventilator data used in this competition was provided by [Kaggle](https://www.kaggle.com/c/ventilator-pressure-prediction/data) and was produced using a modified [open-source ventilator](https://www.peoplesvent.org/en/latest/) connected to an [artificial bellows test lung](https://www.ingmarmed.com/product/quicklung/) via a respiratory circuit.

### Columns

| Column |Description|
|-------|--------|
| id | Globally unique time step identifier across an entire file |
| breath_id | Globally unique time step for breaths | 
| R | Lung attribute indicating how restricted the airway is (in cmH2O/L/S) |
| C | Lung attribute indicating how compliant the lung is (in mL/cmH2O). Physically, this is the change in volume per change in pressure |
| time_step | The actual time stamp | 
| u_in | The control input for the inspiratory solenoid valve. Ranges from 0 to 100 | 
| u_out | The control input for the exploratory solenoid valve. Either 0 or 1 |
| pressure | The airway pressure measured in the respiratory circuit, measured in cmH2O |
 

## Exploratory Data Analysis

### R & C Attributes

A ventilator must consider lung attributes Capacitance (C) and Resistance(R) to predict the optimal pressure.

R can be thought of as the resistance observed while blowing up a balloon through a straw. Higher R will have more resistance and it will be harder to let air inside.


C can be thought of as the thickness of the balloon’s latex, the one with higher C will have thinner latex and it would be easier to let air in.
| R |C|
|-------|--------|
| <img src="Images/r_explained.png" width="440" height="400"> | <img src="Images/c_explained.png" width="440" height="400"> |

### Analysis of pressure, u_in and u_out with respect to time_step

<p align="middle">
  <img src="Images/pressure_time.png" width="450" />
  <img src="Images/uin_time.png" width="450" /> 
  <img src="Images/uout_time.png" width="450" />
</p>

### Plot of pressure, u_in and u_out for breath_id = 1

It is evident from below that u_out is zero during the inhalation and 1 during the exhalation.
<p align="middle">
  <img src="Images/breath_id_1_plot.png" width="500" />
</p>

### Distribuition of Pressure

Variation of pressure is more during the inhalation phase of the breath.
<p align="middle">
  <img src="Images/pressure_one_cycle.png" width="450" />
  <img src="Images/pressure_inhalation.png" width="450" /> 
</p>

### R and C values present in the Dataset

There are three unique values for R and C each.
<p align="middle">
  <img src="Images/rc_unique.png" width="450" />
</p>

Plot of pressure across different combination of R & C for different Breath IDs is shown below. We can infer from below that pressure is dependent on the values of R and C.
<p align="middle">
  <img src="Images/pressure_rc.png" width="450" /> 
</p>

## Feature Engineering

Pressure is a function of past valve settings: p[i] = f(u_in[:i]). But u_in is not an independent variable, u_in is the output of a controller, and the inputs of the controller are the past measured pressures: u_in[i] = g(p[:i+1]). Hence in order to get data from previous time steps data was preprocessed as follows. Following features were added - 

 * New Lag features for u_in
 * Exponential Moving Mean, Standard Deviation and correlation of u_in for each breath ID 
 * Rolling Mean, Standard Deviation and Maximum of u_in for each breath. Here the size of the moving window is 10
 * Expanding Mean, Standard Deviation and Maximum of u_in for each breath ID where size of minimum period is 2
 * R and C after converting into indicator variables



## Implementation

Since we are using a Dataset from a kaggle competition, we were unable to to get the true Y values for the test data provided in kaggle. We split the trainng data as follows to get the training and test data - 

**Training Data** 70% of the Total Breath IDs = 4,225,200 records

**Test Data** 30% 70% of the Total Breath IDs =. 22,635 records


### XGBOOST Using XGBRegressor

The above features

### LSTM
![image5](ResNet18.png)




## Experiments

GPU: Google Colab

Epochs: 25

Unsegmented data set

Segmented data set

Batch size:4


## Conclusion
We plot all the accuracy in one linechart as below, and get three conclusion.
1. Segmented data performed  better on ResNet18.
2. Segmented data  and original data performed  almost same  on Densenet121.
3. Densenet121 performed better than ResNet18 because of the complicated input structure.
![image](pics/linechart.png)
