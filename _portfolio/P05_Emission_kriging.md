---
title: "Spatial and temporal pattern of light-duty vehicles' emission using Kriging and autoregressive models"
excerpt: "This project was completed as part of my Data Modeling course's final project in 2021."
collection: portfolio
---


Spatial and temporal pattern of light-duty vehicles' emission in Manhattan, NY, using Kriging and autoregressive models

## Motivation
The contribution of the transportation system to air pollution is not negligible. A large part of this emission contribution comes from gasoline fuel vehicles. Understanding vehicular emission behavior can help policymakers leverage existing strategies, mitigate fuel consumption and emission rates, and thus have clean air. Analyzing GPS trajectory data can extract useful information regarding spatial and temporal vehicular emissions in a city. In this project, I aim to analyze the light-duty vehicle trajectory data set to present new insights regarding the spatial and temporal pattern of vehicular emission in New York City. 


## Data
 City-owned fleets of vehicles collected the GPS trajectory data set used for this project for one year and four months in 2015 and 2016. The data sampling rate is 30 seconds and consists of a timestamp, location (latitude and longitude), and speed. The data size is above 200 million rows of GPS records.

The raw data is stored in a **Postgres** server; thereby, all emission calculations have been done in **SQL**, and the aggregated emissions per segment are exported and saved as a CSV file, which is loaded for further spatial analysis. 


## Method
### Data Processing
The GPS records are usually noisy, especially in metropolitan areas where high-rise buildings block the satellite signals and cause errors in GPS location records. To this end, using **map matching algorithms**, the GPS points are projected to the best nearest segment candidate. After map matching, the roadway segment ID has been added to the GPS trajectory data set, i.e., a segment ID has been assigned to each GPS record, and these IDs are identical to the roadway segment IDs presented in the New York City shape file. Therefore, the trajectory of each vehicle consists of a sequence of segments, each of which can be calculated using the average speed of the vehicle. 
#### Emission
After calculating the vehicle's average speed at each roadway segment, the emission is calculated using the COPERT model. For this project, only hot emission is calculated. The COPERT model for calculating hot emission is as follows [1]:

$$ Q_{ij} = q_{i,j,k} l_j $$

where $Q_{ij}$ (g) and $q_{i,j,k}$ (g/km) are the emission and hot emission factor of pollutant type $k$ generetaed by vehicle $i$ at segment $j$, respectively. $l_j$ is the segment length which is available in New York City shapefile.
The hot emission factor of each pollutant can be calculated as below:

$$q_{i,j,k} = \frac{\alpha_kv_{i,j}^2 + \beta_kv_{i,j} + \lambda_k}{\epsilon_kv_{i,j}^2 + \zeta_kv_{i,j} + \eta_k}$$

where the $v_{i,j}$ (km/h) is the average speed of vehicle $i$ at segment $j$. The parameters values are presented at the table below [1].


|$k$|$\alpha_k$|$\beta_k$|$\lambda_k$|$\epsilon_k$|$\zeta_k$|$\eta_k$ 
|:-|:- | :- | :- | :- | :-|:-
|CO|0.00|-0.033|5.11|0.002|-0.529|37.51 
| NOx|0.00 | -0.009 | 0.577 | 0 | 0 | 5.43 

### Data transformation
The density plot shows that the emissions have a positive skew, i.e., they are more toward the lower values. To have a normal distribution, data is transformed by taking the square root of emissions. However, the transformed data still does not have a normal distribution. For this project, it is assumed that transformed data has a normal distribution.
![image](https://github.com/user-attachments/assets/8fffdfc3-845a-4086-8bbd-e5f8c664abba)

## Spatial Analysis
### Semivariances and Variogram
In this project, **semivariograms** are calculated to assess the **spatial correlation** between the spatially distributed point emissions. Semivariograms **depict the degree of similarity between neighbors as distance between them increases**. Semivariances can be calculated using the equation as follows:

$$ \lambda(h) = \frac{1}{2}E[(z(s_i) - z(s_i+h))^2]$$

where $z(s_i)$ is the value of a spatial random variable $s_i$ and $z(s_i + h)$ is the value of the neighbor random variable $s_i + h$ with $h$ as the distance between two points.
Herein, the sample semivariances are calculated and shown by the variogram plot below. Then, the **exponential Variogram** has been fitted to the the semivariances.  
![image](https://github.com/user-attachments/assets/079df675-0f39-4661-bbd9-f6d52c7e454e)




## Kriging
After calculating the variograms, the best **linear model** can be estimated to predict vehicular emissions at intermediate locations. The **semivariances** are used to calculate the weights of the linear estimator. Suppose, $z(x_i), i=1,..N$ are the observed values at locations $x_i$, thus for any new location $x_0$ the value of $z(x_0)$ can be calculated as follows [2]:

$$Z(x_0) = \sum_{i=1}^{N} = \lambda_iz(x_i)$$

where $\lambda_i$s are the weights that minimizes the the error variance of the predicted model. $\lambda_i$s can be computed through the equations below:

$$\sum_{i=1}^{N} = \lambda_i\gamma(x_i-x_j) + \psi(x_o) = \gamma(x_j-x_0) $$

$$\sum_{i=1}^{N}\lambda_i = 1$$

where $\gamma(x_i-x_j)$ is the semivariance between $x_i$ and $x_j$, $\gamma(x_j-x_0)$ is the semivariance between $x_j$ and the target location $x_0$, and $\psi(x_o)$ is the lagrange multiplier.

To implement the **kriging**, first, the area needs to be gridded into cells. To this end, the polygon of the Manhattan borough and the Central park needs to be loaded.
![image](https://github.com/user-attachments/assets/06c4efb5-5d06-43de-a9ca-fb022837a4c8)

### Ordinary Kriging with cellsize = 1000
![image](https://github.com/user-attachments/assets/47b2f7da-a830-4c5a-9edf-b95e91005609)

### Plot spatial variances 
![image](https://github.com/user-attachments/assets/f0e1b146-0c34-44c2-8ef3-5e52961d8e96)

## Discussion
The ordinary kriging results show that there exist higher vehicular emission at the center of the Manhattan midtown and relatively lower emission at lower Manhattan. However, there exist sporadically locations with relatively higher emissions in this borough. This is consistent with the intuition since, relatively speaking, other modes of transportation, such as Subway or walking, are popular in lower Manhattan. While at locations with higher traffic volume and higher speed, the emission is observed to be higher. The prediction plot also highlights the fact that the probability of vehicular emission to be higher than 150 grams is higher at the center of Manhattan Midtown.


## Analyze the temporal pattern of emission in New York City
In this step, the temporal pattern of emission in New York City is analyzed. To do so, emissions of all roadway segments are summed and then aggregated at the level of daily and weekly. Then, different autoregression models are estimated.

## Daily emissions in New York City
The histogram of the data shows that the total emission in the city has a mixed Gaussian Distribution. The autocorrelation plot below shows that the daily total emission somehow has a sinusoidal pattern. Therefore, the linear regression model has been estimated using the sine of the emission, the square root of the emission, and the emission itself as the explanatory variables. The estimated model could explain the 20% of the variations in the data.
![image](https://github.com/user-attachments/assets/7e31c1de-0752-448b-a63d-59687b35e83f)

![image](https://github.com/user-attachments/assets/165abcb3-f082-4e06-944c-9a69a91473d4)
![image](https://github.com/user-attachments/assets/4063a7f2-ccaa-481f-9db5-7418b780e013)
![image](https://github.com/user-attachments/assets/4441fd8b-737c-4d56-b01b-59ff11d52765)


## Weekly emissions in New York City
In this step, an autoregression linear model is first estimated using the weekly emission data.
### Exploratory Data Analysis
![image](https://github.com/user-attachments/assets/bdd185be-6d7a-4808-a5ae-f4a225727885)

![image](https://github.com/user-attachments/assets/238c267b-ebf8-4719-af1f-168e82fd4e74)

### Fitting linear model
![image](https://github.com/user-attachments/assets/28b0a250-ef28-4770-b343-5c2cd8ddce08)


![image](https://github.com/user-attachments/assets/c5260d98-fea5-4264-b22d-e99674e31d4e)
![image](https://github.com/user-attachments/assets/7ce27395-57cc-43c2-a329-4a44bbf0e542)
![image](https://github.com/user-attachments/assets/ac3f7e99-ce85-442f-a6a5-f35bc990a428)

The estimated autoregression has a 17% R-squared. Although the plots of residuals do not follow the requirements of the linear regression estimation, i.e., the residuals are  **heteroscedastic**.

In the next section, the **polynomial linear regression** is estimated using the emission and sine of the emission as the other explanatory variables. The estimated model explains 42% of the variation in the data, which is relatively better than the other estimated models.
![image](https://github.com/user-attachments/assets/823d12b9-1625-4809-9783-03d7d03a5fff)
![image](https://github.com/user-attachments/assets/35b4200f-b8bc-4d23-a8a3-e74c617c9f64)

In the following section, using data on weekly emissions in New York City,  a linear regression model is estimated with the emission, root square of emission, and the sine of emission as the explanatory variables. The R-squared of the model is 48%. 
![image](https://github.com/user-attachments/assets/ba9ffe25-4072-4817-a868-2335f48d17bd)

![image](https://github.com/user-attachments/assets/def4c55d-483f-4215-af37-e5cf33571705)

![image](https://github.com/user-attachments/assets/cc668269-9d09-4a16-8fdd-5e283cdb7431)

# Conclusion
The aim of this project was to assess the **spatial and temporal pattern** of emission in New York City. The **GPS trajectory data** and COPERT emission model are used for the analysis. The spatial analysis has been done in two steps, 

1. Calculating the semivariances and variograms
2. Estimating the kriging model.
The results showed that higher emissions are centered in Midtown Manhattan, and lower emissions are estimated to be around Times Square.

To do temporal analysis, different linear regression models, as well as the **autoregression model**, are estimated using both daily and weekly emission data in the city. The **autocorrelation functions showed that data has a sinusoidal pattern**. Therefore, in most of the models, the sine of the emission is considered as the explanatory variable. The R-square of the different models ranged from 20 to 40 percent.
## Challenges
Working with the large GPS dataset was a big challenge in this project. Preparing the grid was also another challenge since the coordinates in data and polygons needed the appropriate projection. In addition to that, in temporal analysis, the **temporal extent** of the data was not enough to capture the existing trend in the time series. Besides, the data was noisy, which required smoothing techniques before employing temporal statistical models.

# References
[1] Sui, Y., Zhang, H., Song, X., Shao, F., Yu, X., Shibasaki, R., ... & Li, Y. (2019). GPS data in urban online ride-hailing: A comparative analysis on fuel consumption and emissions. Journal of Cleaner Production, 227, 495-505.

[2] Oliver, M. A., & Webster, R. (2014). A tutorial guide to geostatistics: Computing and modelling variograms and kriging. Catena, 113, 56-69.
