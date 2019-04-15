# Predicting Solar Panel Adoption
#### UC Berkeley Master of Information and Data Science Capstone Project, Spring 2019
`Team: Gabriel Hudson, Noah Levy, Laura Williams`

## Mitigating Climate Change
We were motivated toward this project by our interest in reducing carbon emissions to mitigate the impact of climate change. Distributed renewable electricity generation additionally creates more diversity and resilience in electricity production and reduces demand on electrical grids. However, distributed electricity generation is more complex to integrate into the grid than traditional centralized energy production and requires understanding of complex customer adoption trends. Our work takes a step toward this latter goal of understanding what factors drive customer adoption of distributed residential solar panel systems.

## Open Tool with Geographic and Feature Analysis
We've created an interactive visualization of our results, focussing in particular on identifying geographic areas (as census tracts) that have high values of predictive factors driving residential solar panel adoption, but instead are under-saturated relative to our predictions. We've visualized general predictions for the continental United States and several state maps allow exploration of over 100 potentially predictive factors for every census tract in that state.

**_[Add link(s) to viz here]_**

## Data Source
We started with a recently released dataset from Stanford's DeepSolar team, who identified 1.47 million solar panel systems in the US using satellite imagery and their convolutional neural network.  Their dataset additionally included over 150 other variables to explore relative to solar panel systems.  Their work is described in more detail here: [DeepSolar](http://web.stanford.edu/group/deepsolar/home "DeepSolar")

## Process
We trained a two-stage Random Forest model based on a model structure that worked well for the DeepSolar team.  We carried out a number of feature engineering steps on the DeepSolar dataset, and performed careful hyperparameter tuning with that model and engineered dataset.  Our final dataset, model and results are in the [_Final_](https://github.com/nwlevy/capstone_solarpanels/tree/master/Final/) directory in this repo. Notebooks with more detailed explanations, dataset explorations and model experiments are primarily in the [_EDA_](https://github.com/nwlevy/capstone_solarpanels/tree/master/EDA) and [_Modeling_](https://github.com/nwlevy/capstone_solarpanels/tree/master/Modeling) directories.

## Results and Insights
We were able to improve upon DeepSolar's predictive model results with a 10-fold cross-validated R squared score of 0.744. We explored the nationally most important features in more detail, noting the particular importance of the number of incentives available in any given geographic area.  Industry experts repeatedly pointed to the value of exploring the role of incentives and solar installation company business models. Adding more detail about incentives and business models would be the highest value next step to continuing this work in predicting residential solar panel adoption.

## Final Presentation
Our final presentation contains more detail about our feature engineering, model training, results and insights. That presentation is available  [here]() **_add link_** or as a PDF document in this repo [here]()  **_add PDF and link_**
