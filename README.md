# A/B Recommenders
An end-to-end implementation of online recommender systems A/B testing

## Table of contents
* [Introduction](#introduction)
* [Background](#background)
* [Objective](#objectives)
* [Business Case](#business-case)
* [System design](#system-design)
* [Tools and packages](#tools-and-packages)
* [Data](#data)
* [Data preprocessing](#data-preprocessing)
* [Recommender system](#recommender-system)
* [Experiment design](#experiment-design)
* [Integration and deployment](#integration-and-deployment)
* [Results](#results)
* [Conclusion](#conclusion)
* [References](#references)
* [Challenges and future work](#challenges-and-future-work)

## Introduction

This is the first repository/tutorial from a series of others where I challenge myself to build, deploy and 
explain an end to end data science solution to a concrete business use case.

For this first project, the main goal is to demonstrate the design and deployment of an A/B test experiment comparing
the end business value of two different recommender systems, by creating an easy to deploy and scale containerized 
microservices architecture.

While the compared machine learning models are recommendation systems, the approach detailed here works for comparing
two models of any type.
## Background

This project is inspired from a question I was asked during an interview : 
> How would you compare the performance of two recommender systems in production ?

I knew the theoretical answer to this question but found that I lacked some of the 
practical skills that would allow me to solve this problem and implement its solution in production.
So here we go !

## Objectives

#### Main objectives :
* Design an A/B test experiment to compare the two recommender systems
* Create, configure and deploy the architecture to run the experiment
#### Secondary objectives : 
* Train two recommender systems : the baseline and improved models
* Develop and deploy a proof of concept product website to run the experiment
* Automate the A/B testing experiment for testing purposes

## Example Business Case

Funflix, a movie streaming platform, wants to improve its recommendation engine. The model currently used in production 
has a click-through rate of 7% (called the baseline CTR). The Funflix data science team has developed
a newer recommender system, that supposedly recommends more relevant movies, and wants to test if it improves
user recommendation click-through rate.

## System design

The MovieLens 25M dataset will first be used to train two recommender systems : the baseline model, yielding a 7% CTR,
and the challenger model. Recommendations prediction request from user traffic will be routed randomly to one of the 
two recommender systems and recommendations will be shown on the website. If the user clicks on one of the 
recommendations, the click will be collected and stored to evaluate possible statistically significant change
in click-through rate.

![System diagram](./ressources/system_diagram.png)

Docker, Kubernetes and Seldon Core will be used to deploy the two recommender systems. It will make this A/B testing experiment easier as
it will provide a unified endpoint to query for recommendations, taking care of the random routing and the feedback
and evaluation loop by collecting custom metrics such as in this case the click-through rate through another
special feedback endpoint. It also integrates well with Prometheus and Grafana which will allow 
monitoring of the experiment's results.

Flask will be used to build a mock website in order to test the implementation and run the mock A/B test experiment 
and selenium will be used to simulate user traffic and interaction on this website.

One idea to improve this overall system would be to also implement periodical online re-training so that the model 
would take into consideration new user interactions with movies and to palliate any possible data drift.
This improvement brings alone its fair share of new problems (eg. long and costly retraining, adapting the A/B testing
statistics) so we will focus here on the deployment and serving of the models and the A/B test experiment.

## Project steps

1. Train the two recommender systems to be compared
2. Design the A/B experiment
3. Create, configure and deploy the containerized architecture
4. Deploy and serve the two models
5. Test deployment and serving
6. Develop the mock website
7. Automate user traffic and interaction
8. Explore and interpret the A/B experiment results

## Tools and packages

| Task        | Tools/Package                   |
|-------------|---------------------------------|
|Data & Statistics| numpy, scipy|
|Data visualisation| matplotlib|
| Model deployment and A/B testing | seldon core |
| Mock experiment website | Flask |
| Experiment simulation | selenium |
| Metrics and dashboard | Prometheus, Grafana |
| Deployment | Kubernetes, helm |

## Data

The data used is the MovieLens 25M Dataset [[1]](#1). It is an industry standard recommender system benchmark. 
It contains 25M ratings of 62k different movies by 162k users. It can be downloaded 
[here](https://grouplens.org/datasets/movielens/). The following files are needed:

| File Name   | Description        | Columns                         |
|-------------|--------------------|---------------------------------|
| ratings.csv | Movies ratings     | userId,movieId,rating,timestamp |
| movies.csv  | Movies information | movieId,title,genres            |

The code in this repository assumes that the data is present in the ```/data``` folder, this can be easily modified.

## Data preprocessing

## Baseline model

The baseline model 

## Challenger model

It is worth noting that the two recommender systems obtained are still not perfect, especially in the production
environment and use case presented. It suffers from several problems, one of them being cold start (what do we 
recommend to new users ? how do we recommend new movies ?) or retraining (how do we take into account new
interactions ?) The important objective here being the productionization and A/B testing experiment design and 
deployment, the model developed here is probably enough for the scope of this project.

## Experiment design

Details of the experiment design can be found in [this notebook](02%20-%20AB%20experiment.ipynb).

Taking into consideration the current supposed baseline click-through rate of 7%, and deciding that the desired 
minimum detectable effect is 4% (absolute), the needed sample size for each group of the experiment (each recommender)
is estimated to be **1124 users** for a 5% significance level and a 90% power. 

## Integration and deployment

## Experiment simulation 

## Results

## Conclusion

## References

<a id="1">[1]</a> 
F. Maxwell Harper and Joseph A. Konstan. 2015. The MovieLens Datasets: History and Context. ACM Transactions on 
Interactive Intelligent Systems (TiiS) 5, 4: 19:1–19:19. https://doi.org/10.1145/2827872

## Challenges and future work

🚧 In progress 🚧