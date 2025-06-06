---
title: "Variational Sparse Bayesian learning with Relevance Vector Machine"
excerpt: "Probabilistic prediction with VRVM from Boston housing price to route travel time. "
collection: portfolio
---

# Variational sparse Bayesian learning
Variational sparse Bayesian learning with Relevance Vector Machine (VRVM)
# Abstract
Accurate predictions of the route travel times and quantifying the reliability of the predictions are crucial in optimizing the service delivery transport in a city. This paper aims to predict the travel time distributions between any arbitrary locations in an urban network by training a probabilistic machine learning algorithm using historical trip data. In this project, **variational relevance vector machines (VRVM)** method and **ensemble learning** to **probabilistically** predict the trip travel time for any origin-destination pair at different times of day through learning the similarities between the previously observed travel times across the city road network. The similarities between the observed route travel times are quantified with multi-kernel function. Moreover, the VRVM method allows us to efficiently use historical data through sparse Bayesian learning that identifies the **``relevance'' basis functions from the entire data.  

In the following, some results relating to model verification and trip travel time are presented. Then, modeling details, including the algorithm and the mathematics behind the model, can be found in the following sections.
## Model Verification
Before training the VRVM model using travel time data sets, the implemented VRVM algorithm is verified with two widely used data sets: 
1. **$Sinc$ function**, and
2. **Boston housing** data sets,
and compare the results with repoted results in literature review.

## Sinc function
Using the suggested synthetic data generation in [1], we evaluate the performance of the VRVM model through three different trials. The **Gaussian kernel** is used for training the VRVM models. For each trial, we generate 100 noisy target values, $y$, at 100 linearly selected $x$ from $x \in [-10,10]$ using the function $Sinc(x)=sin(x)/x$ with **Gaussian additive noise** with standard deviations of 0.1, 0.2, and 0.3, respectively. The noise estimate and the root-mean-square (RMS) deviation from the true function evaluated at 1000 linearly spaced test samples in $[-10,10]$ averaged over 100 random instantiations of the noise are presented at table 1. The reported RMS deviation from the true function for Gaussian additive noise 0.1 is 0.038 [2].

![image](https://github.com/user-attachments/assets/416136e3-f0cb-4cfa-8945-ff24cc66606d)

<p align="center">
<img src="/images/sinc.png" width="400" height="400" />
</p>

## Boston housing data
The housing data set contains 506 examples for median housing prices with 13 feature variables. We train the VRVM model using Gaussian kernel and based on housing data to predict housing prices. The averaged results for 10 randomly partitioned Boston Housing data into 481 training, and 25 test data sets are presented in table 1. The estimated noise and the mean squared error are 2.42 and 9.84, respectively, whereas the corresponding values reported at [2] are 2.49 and 10.36 ([2] utilized third order polynomial kernel), and the slight difference might be due to random data partition and different kernel type used.
 
<p align="center">
<img src="/images/BHP.png" width="400" height="400" />
</p>


## Travel time predictions and predictive uncertainty
Then, trained $\mathcal{M}=20$ VRVM models are utilized to construct the predictive ensemble model. The **predictive ensemble distribution** is the mixture of predictive distributions of the trained models. To evalute the performance of the trained models and the final ensemble model, two different metrics are deployed, the **root-mean-square error (RMSE)** and **mean absolute percentage error (MAPE)**.
Results present the predictions on unseen test data set (size of ~1.2k) for trip travel times +/- one predicted standard deviation, which represents the epistemic uncertainty automatically predicted in the adopted Bayesian learning framework. Below also is the errors histograms where error is defined as $(error = y_{predict}-y_{true})$. To evaluate the overall generalization performance of the model, we tested the trained ensemble model on five different unseen data sets. The average RMSE score was 147.24s with a standard deviation of 10.31s, while the average MAPE score was 0.45 with a standard deviation of 0.016. Overall, the scores imply a satisfactory performance specifically for most trips with medium travel time length.
In contrast, for very short trips and very long trips, the model's performance degrades. It can be observed that for the relatively short trips, the model overestimates the travel times, and this is perhaps due to averaging effect that arises from utilizing constant width parameter in Gaussian kernel functions. While, for the long trips, the discrepancy might be due to either something unusual along the journey or the lack of enough long trip data.  

### Historical Trip Travel Time Spatial Pattern
![image](https://github.com/user-attachments/assets/077c9718-93a5-47bf-b1f1-b7361ba02b73)


### Relevance Vectors
![image](https://github.com/user-attachments/assets/047d0f64-b4bb-4cec-af70-6f7e815f23bf)


### Trip Travel Time Predictions vs Ground Truth
![image](https://github.com/user-attachments/assets/0c36315a-b742-40aa-9b20-7a8e3cd50aaf)


### Temporal pattern of aggregated predictions for trips from an origin zone to different destination zones across 24hr a day
![image](https://github.com/user-attachments/assets/653db434-ec28-47da-99ef-9add1cf8c8f6)


# Methodology
The variational relevance vector machine is a kernel-based probabilistic machine learning algorithm that is formulated based on the sparse Bayesian learning [1-2]. This algorithm has a functional form equivalent to the support vector machine while using a sparse set of basis functions gives higher generalization performance. In sparse Bayesian learning, most parameters are automatically estimated as zero, and only a few ``relevance'' parameters are non-zero. Therefore, this approach finds a set of relevant basis functions that can be used to make efficient predictions. Sparse Bayesian learning poses prior distributions over model parameters and hyperparameters and estimates their posteriors using optimization. While the posterior distributions of most irrelevant parameters become zero, the remaining parameters are relevant parameters for the predictions. In the variational relevance vector machine, variational inference approximates the optimal posterior distributions. The following section will summarize the Bayesian model structure and the variational posteriors.

## Model specification
Suppose our data is $\mathcal{D} = \{(\textbf{x}_i, y_i) | i=1,\dots, n\}$ where $\textbf{x}_i \in \mathbb{R}^d$ is the input feature vector, and $y_i \in \mathbb{R}$ is the target scalar. From historical data, our goal is to learn a model that given input vector $x_i$ outputs the value of the target variable $y_i$. In our travel time modeling context, using historical trip data, we train a probabilistic model that predicts the route travel time given origin-destination coordinations and the trip's start time. The probabilistic framework enables us to model the uncertainties in the travel times predictions by estimating both aleatoric and epistemic uncertainties that are attributed, to name a few, to the inherent stochasticity of the phenomenon, lack of enough knowledge, and imperfection model structure assumption. Based on the standard probabilistic formulation, the target value can be calculated by a function $f$ parametrized by $w$ as equation:

$$ y_i = f(x_i;w) + \epsilon_i $$

where to deal with travel time uncertainties, $\epsilon_i$ s are assumed to be independent and identically distributed zero-mean Gaussian random noises, $\mathcal{N}(0,\lambda^{-1})$, with unkown variance $\lambda^{-1}$. This noise variance in the generative model captures the aleotric uncertainty in the travel time prediction. In probabailistic viewpoint, this means that the target variable, $y_i$, is generated by a Gaussian distribution that can be estimated by function $f(x_i;w)$ that in RVM it takes the form of the sum of linearly-weighted kernel functions $k(x,x_i)$ calculated over all pairs of $x_i$ and the relevance vectors $x$. Let $\Phi$ be the kernel matrix, $\Phi=[\phi(x_1),\dots,\phi(x_m)]$ and $\phi(x_i)=[1, d_i, k(x_i,x_1), \dots, k(x_i,x_m)]^T$, where $d_i,$ is the trip geodesic distance, and $m$ is the number of relevance vectors. Then, $y$ follows a Gaussian likelihood model written as

$$
p(y|x)=\mathcal{N}(\Phi^Tw,\lambda^{-1})
$$

Here, we use Gaussian kernel to calculate multi-kernel functions since it is proven that Gaussian kernels yield higher prediction accuracy in machine learning application[3]. Kernel functions enable us to capture the non-linearity in the raw representation of the input space by mapping the input space onto the higher dimensional feature space that also gives rise to the higher computational power\cite{muller2001introduction}. Simply put, with kernel functions evaluated at each data point, we establish a linear relationship between the implicit feature space and the target values where the implicit higher dimensional feature space contains the quantified similarities between pairs of raw input features via Gaussian kernel function. The Gaussian multi-kernel function between two vectors $x_1$ and $x_2$ is expressed by

$$
k(x_1,x_2)=\sum_{l\in L}\exp(-\frac{||x_1-x_2||^2}{l^2}),
$$

where $L$ contains the width parameters that need to be specified a priori. The predictive accuracy of the Kernel-based models is highly influenced by the choice of width parameter, while finding its optimum value is a challenge. To identify the optimum width parameter, a general approach is to utilize the cross-validation technique. Besides, adopting the combination of multiple kernels with different width parameters instead of using one single kernel function can also increase the generalization of the kernel-based models. In our modeling setting, we adopt the combination of the multiple kernels, each constructed by a different width parameter. We find the optimum values for the width parameters using cross-validation.
Imposing a sparsity promoting distributions, zero-mean Gaussian prior over model parameters can control the complexity of the model and prevent overfitting. Thus, the unknown coefficient vector $w \in \mathbb{R}^m $ is assumed to have a multivariate Gaussian prior with the unknown variance $\sigma^2_w$ expressed by 

$$
p(w) \sim \mathcal{N}(\textbf{0}, \sigma^2_w),\\
$$ 

We define $\sigma^2_w = (\lambda\Lambda)^{-1}$ where $\Lambda=\text{diag}(\alpha_1,...,\alpha_m)$. It implies that the parameter $w_i$ depends on $\alpha_i$ which corresponds to inverse of variance of $w_i$ [4].

In fully Bayesian paradigm, following the hierarchical prior setting, the variational RVM method considers hyperpriors over hyperparameters. To benefit from analytical propertries of the Gaussian distributions' conjugate priors, we pose a Gamma prior distribution over inverse noise variance, $\lambda$, as $p(\lambda)=\text{Gamma}(e_0,f_0)$ and a Gamma hyperprior distribution over $\alpha_k,k \in [1,\dots,m]$ as $p(\alpha_k)=\text{Gamma}(a_0,b_0)$. Initially, we set small values for $a_0$, $b_0$, $e_0$, and $f_0$ to have non-informative priors. This method called ``Automatic Relevance Determination (ARD)"  that automatically selects the relevant parameters $w_i$ based on the variance of the parameter $\sigma^2_{w_i}$ [2].

## Predictive Distribution
Bayesian learning aims at estimating the predictive posterior distribution of the future observation $y'$ for corresponding new input vector $x'$ given all previously observed data $(y,X)$ and the same modeling assumptions. The posterior predictive distribution is the marginalization of the likelihood over the posterior distributions of the model parameters, shown by

$$
p(y'|x';y,X) = \int{p(y'|x',w,\lambda)p(w|y,X,\lambda)p(\lambda|y,X)}dwd\lambda
$$

If we show the model parameters with $\theta=\{ \lambda, w \}$, using the Bayes rule, the model posteriors $p(\theta\|y)$, can be calculated as

$$
p(\theta|y) = \frac{p(y|\theta)p(\theta)}{p(y)}\\
p(y) = \int{p(y|\theta)p(\theta)d\theta}
$$

With our model specification, calculating the marginal probability of observed data, a.k.a evidence (the integral in the denominator), is intractable. Variational inference (VI) techniques can be adopted to approximate the inference in such a case. Basically, the VI technique finds the optimum member from the apriori specified family for posteriors,$\mathcal{Q}$, by minimizing the distance of the approximated distribution from the exact posterior distributions

$$
p(\theta|y) = q(\theta)\\
q^*(\theta) = \underset{q(\theta)\in \mathcal{Q}}{\arg\min} \text{ KL}q(\theta) || p(\theta|y)
$$

The distance between approximated and exact posteriors of parameters is quantified using Kullback-Leibler (KL) divergence measure defined by

$$
\text{ KL}q(\theta) || p(\theta|y) = \mathbb{E}[\ln q(\theta)] - \mathbb{E}[\ln p(\theta,y)] + \ln p(y)
$$ 

The following section shows how to solve this optimization objective function.
## Variational Inference
It is not easy in many complex statistical models with a Bayesian setting, or sometimes it is impossible to derive an exact analytical expression for the inference. Monte Carlo sampling techniques are the alternatives for estimating distributions for such intractable analytical solutions. While in such models with many parameters or in a large data set, using Monte Carlo sampling techniques are computationally expensive. Variational inference is another alternative that poses an assumption on the family of posteriors and then uses optimization to find the optimal member of the family. Therefore, variation inference gives exact closed-form expression for the approximated inference. Even though VI is often faster when dealing with large data sets or complex models, in contrast to the sampling techniques that are often unbiased and produced samples from the target distribution, it is not guaranteed that the target posterior distribution is in the same family as assumed posterior family $\mathcal{Q}$.

### Variational Inference Objective Function
The KL divergence calculation requires the calculation of evidence part $p(y)$, which is hard to obtain in our case. In such cases, instead of minimizing the whole equation, the optimization objective can be held by maximizing the negative of the two first terms, also called evidence lower bound (ELBO). Thereby, maximizing ELBO equivalently minimizes the KL divergence measure (for more detail, see [5]).

In our modeling context, approximating the joint posteriorof parameters as $p(w,\lambda, \alpha_1,...,\alpha_m\|y,X)$ with $q(w,\lambda, \alpha_1,\dots,\alpha_m)$, the ELBO objective function which we represent by $\mathcal{L}$  can be presented by 

$$
\mathcal{L} = \mathbb{E}_q[\ln p(y,w,\lambda, \alpha_1,...,\alpha_m|X)] -\mathbb{E}_q[\ln q(w,\lambda, \alpha_1,...,\alpha_m)]
$$

With mean-field property that assumes all parameters are independent, approximate joint posterior can be factorized as

$$
q(w,\lambda, \alpha_1,...,\alpha_m) = q(w)q(\lambda)q(\alpha_1)...q(\alpha_m)
$$

Next, we utilize the variational inference technique to derive a closed-form expression for each approximate posterior of model parameters.

## Approximate Inferences
To find the optimal approximate posterior distribution say $q^*(\theta_j)$, the procedure is the exponentiation of the expected log of the joint probability of observed data and parameters taken over all parameters except the parameter $j$, i.e., $q(\theta_{-j})$ (the details of derivation can be found in [2]:

$$
q^*(\theta_j) \propto \exp\{\mathbb{E}_{-j}[\ln p(y, w, \lambda, \alpha_1,...,\alpha_m|x)]\} 
$$

The optimal variational posterior of model parameters are derived analytically since the conjugate priors (Gamma distributions) to the Gaussian model are adopted. The approximated inferences are presented below.
 
$$
q(\lambda) \propto \text{Gamma}(e',f'),\\
q(w) \propto \mathcal{N}(\mu',\Sigma'),\\
q(\alpha_k) \propto \text{Gamma}(a'_k,b'_k), k\in{1,...m}\\
$$


where the distributions parameters are described as:


$$
e' = e_0 + 0.5m+ 0.5N,\\
$$

$$
f' = 0.5\sum_{i=1}^{N}\mu'(y_i-\phi(x_i)^Tw)^2 + f_0 + 0.5\mu'^TM_{\alpha}\mu',\\
$$

$$
M_{\alpha}=\text{diag}(\frac{a'_1}{b'_1},\dots,\frac{a'_k}{b'_k}),\\
$$

$$
\Sigma' = \frac{f'}{e'}(M_{\alpha} + \Sigma_{i=1}^{N}\phi(x_i)\phi(x_i)^T)^{-1},\\
$$

$$
\mu' = \Sigma' \frac{e'}{f'}\sum_{i=1}^{N}y_i\phi(x_i)),\\
$$

$$
a'_k = a_0 + 1/2,\\
b'_k = 0.5\frac{e'}{f'}\mathbb{E}[w_k^2]+ b_0,\\
$$

$$
\mathbb{E}_{q(w)}[w_k^2] = {\mu'_k}^2 + \Sigma'_{kk}.
$$

## Approximated Posterior Predictive Distribution
And, the posterior predictive distribution can be approximated as follows:

$$
p(\hat{y}|y) = \int p(\hat{y}|\phi(\hat{x}_i),w,\lambda)q(w,\lambda)d\lambda dw.\\
$$

However, as stated in [2], it can be shown that for increasing number of data points, $\lambda$ tends to its mean value ($\frac{e'}{f'}$), thus the predictive distribution is approximated as

$$ 
p(\hat{y}|y) = \int p(\hat{y}|\phi(\hat{x}_i),w,\mathbb{E}_{q(\lambda)}[\lambda])q(w)dw,\\
$$

or simply we can present it as Gaussian distribution with mean and variance as [2],

$$
p(\hat{y}|y) = \mathcal{N}( \mu'^T\phi(\hat{x}_i),\frac{f'}{e'}+\phi(\hat{x}_i)^T\Sigma'\phi(\hat{x}_i )),\\
$$

where the variance parameter of the distribution quantifies the aleotric and epistemic uncertainties in the predicted travel times.

## Ensemble Prediction
The CAVI optimization algorithm becomes expensive for large data sets in the variational relevance vector machine. The computation scale of the VRVM algorithm is $\mathcal{O}(N^3)$, and this is due to the existence of the covariance matrix inverse operation in computing weights posterior distributions. Hence, this method is not scalable for larger data sets unless an appropriate training process is adopted. One way to make use of this approach for large data sets is to take advantage of the ARD property of the method, such that iteratively adding new basis functions and pruning non-relevance ones. To prune insignificant basis functions, either a weight threshold or a threshold for hyperparameter of $\alpha$ can be used. Another way to make this approach practical is to train different ensembles of the VRVM model in parallel with different samples of data and then average predictions. To do so, the ensemble prediction can be treated as uniformly weighted mixture model of the predictive distributions defined as [6],

$$
p(\hat{y}|\hat{x}, \mathcal{D})  \propto \frac{1}{\mathcal{M}} \sum_{\mathfrak{m}=1}^\mathcal{M} p_{\mu_\mathfrak{m},\sigma^2_\mathfrak{m}}(\hat{y}|\hat{x}, \mathcal{D}_\mathfrak{m})
$$

$$
p(\hat{y}|\hat{x}, \mathcal{D})  \propto \frac{1}{\mathcal{M}} \sum_{\mathfrak{m}=1}^\mathcal{M} \mathcal{N}(\hat{\mu}_\mathfrak{m}, \hat{\sigma}^2_\mathfrak{m}),
$$

where $\mathcal{M}$ is the number of ensembles.


Hence mixture model above can be approximated with Gaussian distribution with mean and variance shown expressed as,

$$
p(\hat{y}|\hat{x}, \mathcal{D})  \propto\mathcal{N}(\hat{\mu}, \hat{\sigma}^2),
$$

$$
\hat{\mu} = \frac{1}{\mathcal{M}}\sum_{\mathfrak{m}=1}^\mathcal{M}\hat{\mu}_\mathfrak{m}\\
$$

$$
\hat{\sigma}^2= \frac{1}{\mathcal{M}}\sum_{\mathfrak{m}=1}^\mathcal{M}(\hat{\sigma}^2_\mathfrak{m}+\hat{\mu}^2_\mathfrak{m})-\hat{\mu}^2
$$

![image](https://github.com/user-attachments/assets/cbf8a748-6d47-4fe4-86e0-8b93740669e3)



References

1. Tipping, Michael E. "Sparse Bayesian learning and the relevance vector machine." Journal of machine learning research 1.Jun (2001): 211-244.

2. Bishop, Christopher M., and Michael Tipping. "Variational relevance vector machines." arXiv preprint arXiv:1301.3838 (2013).

3. Smola, Alex J., and Bernhard Schölkopf. "A tutorial on support vector regression." Statistics and computing 14 (2004): 199-222.

4. Ueda, Naonori, and Zoubin Ghahramani. "Bayesian model search for mixture models based on optimizing variational bounds." Neural Networks 15.10 (2002): 1223-1241.

5. Blei, David M., Alp Kucukelbir, and Jon D. McAuliffe. "Variational inference: A review for statisticians." Journal of the American statistical Association 112.518 (2017): 859-877.

6. Lakshminarayanan, Balaji, Alexander Pritzel, and Charles Blundell. "Simple and scalable predictive uncertainty estimation using deep ensembles." Advances in neural information processing systems 30 (2017).


