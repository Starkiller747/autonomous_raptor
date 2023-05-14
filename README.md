<h1 align="center"> Erlang C vs Machine learning algorithms in Call Centers </h1>

<a name="readme-top"></a>

## Index

[Introduction](#Introduction)

[Data Manipulation](#data-manipulation)
 
[Model development](#Model-development)
 
[Comparison between models](#Comparison-between-models)
 
[Conclusion and future thoughts](#Conclusion-and-future-thoughts)

[References](#References)

## Introduction <a name="introduction"></a>

This project aims to compare the actual usage of the Erlang C algorithm for rep administration vs 3 models of machine learning, namely XGBoost, Random Forest and a Recurrent Neural Network.

In call centers, there are several important variables to control, being the following 4 the most important of them (for the majority of the call centers):

*  Handled calls (amount of calls taken)
*  Abandoned calls (calls that were put on hold and, after waiting for at least 15 seconds (in this case), were not answered by an employee)
*  Service level (rate obtained by dividing the amount of abandoned calls over the total amount of calls)
*  Employee count (the amount of people actively capable and available to answer calls)

In this project, we aim to calculate the amount of employees needed on site every day to be able to handle the workload, while keeping the service level below 5%.

Currently, most call centers use a simplified version of the Erlang A formula, called Erlang C. [[1]](#1)

<p align="right">(<a href="#readme-top">back to top</a>)</p>

## Data Manipulation <a name="data-manipulation"></a>

The data was obtained from a real-world call center database, divided by several sites (identified by number).
The variables in this file are:
  Campaign (site number)
  Start date (date of call)
  Total calls (all calls done on that day)
  Abandoned calls (calls that were on hold for at least 15 seconds and were not answered)
  Handled calls (calls that were handled by employees)
  Count of reps (amount of employees)

We graph several site's data in order to compare and select the least anomalous one.

We proceed with the data normalization and cleaning, to start the models.

After the models are completed, we notice that the sites all have something in common, they 


## Model Development <a name="model-development"></a>

## Comparison between models <a name="comparison-between-models"></a>

## Conclusion and future thoughts <a name="conclusion-and-future-thoughts"></a>




## References <a name="references"></a>

<a id="1">[1]</a> 
CHROMY, E., MISUTH, T., & KAVACKY, M. (March 2011). Erlang C Formula and its Use in the Call Centers. Information and Communication Technologies and Services.
