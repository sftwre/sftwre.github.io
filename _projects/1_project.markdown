---
layout: page
title: Logistic Regression API
description: Deploying an ML model for consumption using Flask
tech: Python | Flask | Docker
img: /assets/img/12.jpg
importance: 1
---

I worked on this project as a part of the employment assessment for a Machine Learning Engineer role
with an insurance company. The task was simple: deploy a logistic regression model as an API. The model segments customers
based on past purchasing behavior to predict whether a customer will purchase a product (y=1) or not (y=0). 
Customers that are more likely to purchase a product will be more receptive towards an advertisement for that
product, so this model could help the marketing department efficiently allocate ad spending. The model was provided to
me in a Jupyter-notebook. I studied this notebook to understand the pre-processing being applied,
to determine features being learned, and to understand how the model is trained.
I then saved the trained model as a pickle file for use in the API.

 
I decided to deploy the model with Flask for the following reasons<br/>
<ol>
    <li> Lightweight framework with minimal library dependencies </li>
    <li> REST API development is supported out the box </li>
    <li> Straight forward programming interface </li>
</ol>

The API had to be able to accept 1 to N rows of input data and return the prediction,
model parameters, and the expected probability of the purchase $$\hat{p}$$ in JSON format. The image below shows
my test data and the features I would be passing to the model, which was provided as a csv file.

<div class="row">
    <div class="col-sm-12">
        <img class="img-fluid rounded z-depth-1" src="{{ '/assets/img/glm_api_test_data_head.png' | relative_url }}" alt="" title="example image"/>
    </div>
</div>
<div class="caption">
    Output from head function for a panda's Dataframe
</div>

Since the model was trained on normalized data with mean filling applied to the columns, I had to
serialize the imputer statistics and mean values for feature columns. This would allow me to apply the same scaling
and fill values on input data to the API. Through feature engineering, it was evident that not all the features 
were correlated with the dependent variable. So I also serialized the top 25 features with the largest squared
coefficients, which can change when more data is available. The imputer statistics, standard scaler parameters, and
features of interest were saved to ``features.pkl``.

From the image above you can see the input features are mainly floats. However some of the features contain categorical values, 
NaN values, percentages, or monetary values. Therefore, when pre-processing the input data I had to remove: percentage signs, 
dollar signs, commas, and parenthesis from the input data. Categorical features were then discretized and all values 
were converted to floats. Next, I utilized the saved imputer and standard scalar to normalize input data and mean fill any NaN values.
Finally I passed the input to the model and saved the result (probability of a purchase) in a separate pandas DataFrame.
If $$ \hat{p} > .75 $$ for the input then the business_outcome was set to 1, indicating that the customer should
receive an advertisement.

Since the API should be able to accept more than one row of input, I stored all input into a DataFrame to handle batch requests.

I loaded the model, features of interest, standard scalar parameters, and the imputer at startup so that they are accessible
while the API server was active.

I created a test suite using ``pytest`` to evaluate new functionality as I developed it. The tests covered the following cases:

<ul>
    <li> Empty request </li>
    <li> One row of input </li>
    <li> Five rows of input </li>
    <li> 10k rows of input </li>
</ul> 

<h2> Deployment </h2>
Although the API is not intended for public consumption, I tested it locally using 
<a href="https://www.postman.com/" target="_blank"> Postman </a>. 
I also created a Docker image for the API and published the image on Docker-hub, <a href="https://hub.docker.com/repository/docker/texashookem/glm_api" target="_blank"> https://hub.
docker.com/repository/docker/texashookem/glm_api</a>. If this were deployed for public consumption, e.g. deploying the API on EC2, 
I could use a load balancer to efficiently route requests to my API servers. I could also implement rate limiting on a 
user basis, so no single user consumes too much processing time.

<h2> Take aways</h2>
This was a unique project as it required Software Engineering and Machine Learning acumen. I gained hands on experience
taking some one else's model and making it accessible over the web. 



```html
<div class="row justify-content-sm-center">
    <div class="col-sm-8 mt-3 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="{{ '/assets/img/6.jpg' | relative_url }}" alt="" title="example image"/>
    </div>
    <div class="col-sm-4 mt-3 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="{{ '/assets/img/11.jpg' | relative_url }}" alt="" title="example image"/>
    </div>
</div>
```
