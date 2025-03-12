---
title: "Project #3: Spatiotemporal study of vehicular emission pattern in Manhattan"
excerpt: "This project was completed as part of my Data Modeling course's final project in 2021."
collection: portfolio
---

# emission_pattern_modeling_with_R
Spatial and temporal pattern of light-duty vehicles' emission in NYC

## Motivation
The contribution of the transportation system to air pollution is not negligible. A large part of this emission contribution comes from gasoline fuel vehicles. Understanding vehicular emission behavior can help policymakers to leverage the the existing strategies, mitigate the fuel consumption and emission rate and thus have a clean air. with analysing GPS trajectory data, one can extract useful information regarding spatial and temporal vehicular emission in a city. In this project, I aim to analyze the light-duty vehicle trajectory data set to present new insights regarding the spatial and temporal pattern of vehicular emission in new york city. 

## Prediction of emissions greater than 50 gr
In this step, the variogram and semivariances are calculated for emissions greater than 50 gr.
Then best spatial linear model is estimated using the ordinary krigging approach.
![Algorithm](./figures/emission_prediction.png)

## Data
The GPS trajectoriy data set used for this project  was collected by 27,000 city owned fleet of vehicles for one year and four months in 2015 and 2016. Data sampling rate is 30 seconds and consists of a timestamp, location (latitude and longitude), and speed.The data size is above 200 million rows of GPS records.

The raw data is stored in **Postgres** server, thereby, all emission calculations has been done in **SQL** and the aggregated emissions per segment are exported and saved as csv file which are loaded for furthur spatial analysis. 
### Data transformation
The density plot shows that the emissions have positive skew, i.e., they are more toward the lower values. To have a normal distribution, data is transformed by taking the square root of emissions,however, the transformed data is not still quite normal distribution. For this project, it is assumed that transformed data has normal distribution.
![image](https://github.com/user-attachments/assets/8fffdfc3-845a-4086-8bbd-e5f8c664abba)


## Method
### Data Processing
The GPS records usually are noisy especially in metropolitan areas where the high rise buildings blocks the sattelite signals and cause error in GPS location records. To this end, using **map matching algorithms**, the GPS points are projected to the best nearest segment candidate. After map matching,  roadway segment id has been added to the GPS trajectory data set, i.e., a segment id has been assigned to each GPS record and these ids are identical to the roadway segment ids presented at New York city shape file. Therefore, trajectory of each vehcile consists of a sequence of segments in each of which the average speed of vehicle speed can be calculated. 
#### Emission
After calculating vehicles average speed at each roadway segment, the emission is calulated using COPERT model. For this project, only hot emission is calculated. The COPERT model for calculating hot emission is as follows [1]:

$$ Q_{ij} = q_{i,j,k} l_j $$

where $Q_{ij} (g)$ and $q_{i,j,k} (g/km)$ are the emission and hot emission factor of pollutant type $k$ generetaed by vehicle $i$ at segment $j$, respectively. $l_j$ is the segment length which are available in New York City shapefile.
The hot emission factor of each pollutant can be calculated as below:

$$q_{i,j,k} = \frac{\alpha_kv_{i,j}^2 + \beta_kv_{i,j} + \lambda_k}{\epsilon_kv_{i,j}^2 + \zeta_kv_{i,j} + \eta_k}$$

where the $v_{i,j} (km/h)$ is the average speed of vehicle i at segment j. The parameters values are presented at the table below [1].


|$k$|$\alpha_k$|$\beta_k$|$\lambda_k$|$\epsilon_k$|$\zeta_k$|$\eta_k$ 
|:-|:- | :- | :- | :- | :-|:-
|CO|0.00|-0.033|5.11|0.002|-0.529|37.51 
| NOx|0.00 | -0.009 | 0.577 | 0 | 0 | 5.43 



## Spatial Analysis
### Semivariances and Variogram
In this project, **semivariograms** are calculated to assess the **spatial correlation** between the spatially distributed point emissions. Semivariograms **depict the degree of similarity between neighbors as distance between them increases**. Semivariances can be calculated using the equation as follow:

$$ \lambda(h) = \frac{1}{2}E[(z(s_i) - z(s_i+h))^2]$$

where $z(s_i)$ is the value of a spatial random variable $s_i$ and $z(s_i + h)$ is the value of the neighbor random variable $s_i + h$ with $h$ as the distance between two points.
Herein, the sample semivariances are calculated and shown by the variogram plot below. Then, the **exponential Variogram** has been fitted to the the semivariances.   

## Kriging
After calculating the variograms, the best **linear model** can be estimated to predict the vehicular emission at the intermediate locations.  The **semivariances** are used to calculate the weights of the linear estimator. Suppose, $z(x_i)s, i=1,..N$ are the observed values at locations $x_i$s, thus for any new location $x_0$ the value of $z(x_0)$ can be calculated as follows [2]:

$$Z(x_0) = \sum_{i=1}^{N} = \lambda_iz(x_i)$$

where $\lambda_i$s are the weights that minimizes the the error variance of the predicted model. $\lambda_i$s can be computed through the equations below:

$$\sum_{i=1}^{N} = \lambda_i\gamma(x_i-x_j) + \psi(x_o) = \gamma(x_j-x_0) $$

$$\sum_{i=1}^{N}\lambda_i = 1$$

where $\gamma(x_i-x_j)$ is the semivariance between $x_i$ and $x_j$, $\gamma(x_j-x_0)$ is the semivariance between $x_j$ and the target location $x_0$, and $\psi(x_o)$ is the lagrange multiplier.

To implement the **kriging**, first, the area needs to be gridded into cells. To this end, the polygon of the Manhattan borough and the Central park needs to be loaded.
