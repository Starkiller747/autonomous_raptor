<a name="readme-top"></a>

[![LinkedIn][linkedin-shield]][linkedin-url]

<h1 align="center"> Erlang C vs Machine learning algorithms in Call Centers </h1>

## Index

<a href="#introduction">Introduction</a>

<a href="#data-manipulation">Data manipulation</a>

<a href="#models">Models</a>
 
<a href="#xgboost-model">XGBoost model</a>

<a href="#rf-model">Random Forest model</a>

<a href="#rnn-model">Recurrent Neural Network model</a>
 
<a href="#model-comparison">Model comparison</a>
 
<a href="#conclusion-and-future-thoughts">Conclusion and future thoughts</a>

<a href="#references">References</a>

## Introduction <a name="introduction"></a>

This project aims to compare the actual usage of the Erlang C algorithm for rep administration vs 3 models of machine learning, namely XGBoost, Random Forest and a Recurrent Neural Network.

In call centers, there are several important variables to control, being the following 4 the most important of them (for the majority of the call centers):

*  Handled calls (amount of calls taken)
*  Abandoned calls (calls that were put on hold and, after waiting for at least 15 seconds (in this case), were not answered by an employee)
*  Service level (rate obtained by dividing the amount of abandoned calls over the total amount of calls)
*  Employee count (the amount of people actively capable and available to answer calls)

### Top 10 lines in the data:

![Head of dataset][data-head]

In this project, we aim to calculate the amount of employees needed on site every day to be able to handle the workload, while keeping the service level below 5%.

Currently, most call centers use a simplified version of the Erlang A formula, called Erlang C. [[1]](#1)

<p align="right">(<a href="#readme-top">back to top</a>)</p>

## Data Manipulation <a name="data-manipulation"></a>

The data was obtained from a real-world call center database, divided by several sites (identified by number). The data goes from 01/01/2021 to 29/12/2022, with about 14,000 lines for all sites together.

The Jupyter Notebook goes through the process of explaining the data manipulation in order to convert it to a time-series and to normalize it. We graph several site's data in order to compare and select the least anomalous one.

The selected site, 237322 is shown below.

### Count of employees in site 237322:

![Count of Employees in site 237322][237322-cor]

### Handled calls on site 237322:

![Handled calls on site 237322][237322-hc]

<p align="right">(<a href="#readme-top">back to top</a>)</p>

## Models <a name="models"></a>

For the model selection, I based it on a paper by Hou, Cao & Fan [[2]](#2), in whcih they use a bagging and a boosting method. In our case, the XGBoost and Random Forest models were selected (for their simplicity), as well as a Recurrent Neural Network for its ability to keep and use information on the next cell.

The parameters of each model are shown in the Jupyter Notebook as well. After training the models, the prediction of each one is as follows.

<p align="right">(<a href="#readme-top">back to top</a>)</p>

## XGBoost model <a name="xgboost-model"></a>

First, we obtain the feature importance of the dataset, letting us know which variables are the most important when calculating the amount of employees needed.

![XGBoost features][xgboost-features]

And when the model is trained and tested against the real data, it stacks up like this:

![XGBoost prediction][xgboost-pred]

<p align="right">(<a href="#readme-top">back to top</a>)</p>

## Random Forest model <a name="rf-model"></a>

The skforecast library was used for the random forest model (in order to "lag" and create the "memory" needed for this)[[3]](#3).

We first use the library as normal, and plot the resulting prediction:

![Random Forest prediction][rf-pred1]

Which looks like a good estimate but way off compared to the XGBoost prediction. In the skforecast library, there is a module (?) called grid_search_forecaster, which automatically looks for the best hyperparameters for our model. This gives us the following table, from best to worst optimization:

![Random Forest results grid][rf-search]

We use the results of the above table (max depth=10, n_estimators=100 with 20 lags) and plot the resulting prediction:

![Random Forest prediction][rf-pred2]

Which, does not seem to improve the prediction and, in this case, even worsens it.

<p align="right">(<a href="#readme-top">back to top</a>)</p>

## Recurrent Neural Network model <a name="rnn-model"></a>

### The model used the following structure:

![RNN Model Summary][model-summary]

Which, after several tries, was the best model I came up with, that had the least amount of error:

![RNN Model Validation and Loss Error][rnn-val]

Finally, here is how it stacks up to the test data:

![Recurrent Neural Network prediction][rnn-pred]

As you can see in the above graph and in the following table, an increase in the amount of epochs (the easier way to improve a model) will not have an effect on this one.

![Recurrent Neural Network epochs][rnn-epoch]

<p align="right">(<a href="#readme-top">back to top</a>)</p>

## Model comparison <a name="model-comparison"></a>

We plot all of the models in one single graph for easier comparison:

![All models plotted][all-models]

Comparing the models, we obtain the mean squared error and mean absolute error for each one.

![Boxplot of errors of the models][all-boxplot]

<p align="right">(<a href="#readme-top">back to top</a>)</p>

## Conclusion and future thoughts <a name="conclusion-and-future-thoughts"></a>

Now, with all the models plotted, we can analyze the best one for our case. As the graphs above show, the best performing model is the neural network, followed by XGBoost and then the Random Forest model, according to both the prediction and error graphs.

One may wonder, why did the models not converge or match the test data better? When analyzing this, I came to the realization of something:

![237322 employee increase][237322-jump-cor]

If we look closely, we can see that in October and forward, the amount of employees increased well over 20% (at least, in some cases more than that) but if we try to look for the required demand for this:

![237322 handled calls increase][237322-hc-jump]

We see that there is no discernible increase for it.

This happens not only with this site, but with at least 2 other sites (and more, that I did not graph):

### Site 249337:

![249337 employee increase][249337-jump]

### Site 249760:

![249760 employee increase][249760-jump]

Which lets us know that there was a site-wide increase of 20% (at least, as stated before) of the required employees, in order to handle the predicted calls. 
With either the neural network or the XGBoost models, we are able to predict the amount of required employees to a better degree, thus **saving well over 20% of the required personnel per site**.

This, along with some studies such as the one by Robbins, Medeiros & Harrison [[4]](#4), denote that in certain conditions, the Erlang C model fails to accurately predict the requirements for a call center (which is improved in the Erlang A formula, but this formula is far less used than the C formula).

For future thoughts, my suggestions would be to look into other models, perhaps changing the hyperparameters of the recurrent neural network can improve its accuracy.
Another suggestion would be to obtain a greater dataset, as this one only includes 2 years (I excluded 2020 due to the pandemic conditions that affected every industry).

<p align="right">(<a href="#readme-top">back to top</a>)</p>

## References <a name="references"></a>

<a id="1">[1]</a> 
CHROMY, E., MISUTH, T., & KAVACKY, M. (March 2011). Erlang C Formula and its Use in the Call Centers. Information and Communication Technologies and Services.

<a id="2">[2]</a> 
Hou, C., Cao, B., & Fan, J. (November 2020). A data-driven method to predict service level for call centers. IET Communications.

<a id="3">[3]</a> 
Amat, J. R. (Obtained on December 2022). skforecast. Skforecast's API: https://joaquinamatrodrigo.github.io/skforecast/api/ForecasterAutoreg.html

<a id="4">[4]</a> 
Robbins, T. R., Medeiros, D. J., & Harrison, T. P. (2010). Does the Erlang C model fit in real call centers? Winter Simulation Conference.

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!--Images and links-->

[linkedin-shield]: https://img.shields.io/badge/-LinkedIn-black.svg?style=for-the-badge&logo=linkedin&colorB=555
[linkedin-url]: https://www.linkedin.com/in/jrus93
[data-head]: images/data-head.png
[237322-cor]: images/237322-cor.png
[237322-hc]: images/237322-hc.png
[xgboost-features]: images/xgboost-features.png
[xgboost-pred]: images/xgboost-pred.png
[rf-pred1]: images/rf-pred1.png
[rf-search]: images/rf-search.png
[rf-pred2]: images/rf-pred2.png
[model-summary]: images/model-summary.png
[rnn-val]: images/rnn-val.png
[rnn-pred]: images/rnn-pred.png
[rnn-epoch]: images/rnn-epoch.png
[all-models]: images/all-models.png
[all-boxplot]: images/all-boxplot.png
[237322-jump-cor]: images/237322-jump-cor.png
[237322-hc-jump]: images/237322-hc-jump.png
[249337-jump]: images/249337-jump.png
[249760-jump]: images/249760-jump.png

