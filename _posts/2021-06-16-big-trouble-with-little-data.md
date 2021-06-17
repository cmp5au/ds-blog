---
layout: post
title:  "Big Trouble with Little Data"
date:   2021-06-16 10:16:58 -0700
categories: jekyll update
---

This is the first blog post of (hopefully) many that detail my course through project-based learning of data science.
    I started in earnest on May 17 -- I'm exactly a month late in making this post -- but this project dates back
    to May 21. After learning the ropes of Git, Numpy, Pandas, and Matplotlib, I had the opportunity to test it and
    perform EDA on a real dataset.

Health, Nutrition, and Population Statistics compiled by the World Bank ([link to Kaggle dataset](https://www.kaggle.com/theworldbank/health-nutrition-and-population-statistics))
    includes 345 indicators across 258 countries (that seems like too many, we'll come back to this), collected on a yearly basis from 1960 - 2015.

A quick calculation gives $$345\cdot258\cdot60 = 5,340,600$$ entries in this table, about half of which are missing values as NaNs.
    The reporting rate for each year goes from about 40% in the 1960s to about 60% in the 2010s, with a reliably increasing trend between.
    This is by far my largest dataset to date, and I have trouble grappling with what to visualize and how. Some early thoughts:

1. What can be answered by the dataset?
    * What are the feature columns (indicators)?
    * How do these indicators relate to eachother? 
    * What countries have enough populated data for all of said indicators?
2. How do I visualize this data?
    * What type of data is it?
    * What kind of relationship am I trying to show?

<strong>...is what I should have asked myself, and answered sufficiently before continuing.</strong> Since this is an attempt
at project-based learning, I'm at this point all-too-keen to jump in and fail as fast as possible. Spoiler alert: mission accomplished.

<div align="center">
    <img width="500" src="/assets/images/2021-06-16-big-trouble-with-little-data/shitty_EDA.png" alt="chmod Options">
</div>

What I did here was to plot the very first row without NaNs in order to make an unspeakably bad graph. So many sins committed here; I
owe David McCandless a confessional and a hundred Hail Marys. I spent
the next 5 minutes figuring out what I was looking at, trying to figure out how to visualize the visualization and digging through
column labels and indicator names to learn that this is the number of births per 1000 women ages 15-19 in the Arab World. I'm still
unclear about what constitutes the 'Arab World', and at this point I'm realizing that there are about 200 countries total, not 258.
Those extra countries are actually groups of countries / populations / geographical areas, and I don't know what goes into those
determinations so I probably shouldn't consider these rows at all and bin this first graph.

Okay, that's enough self-flagellation. Let's revisit our questions and get some answers, shall we?
    
1. (returning to this below)
    * Get unique indicator names:

    ```python
    for name in df['Indicator Name'].unique(): print(name)
    ```
    
    * In broad strokes our categories are: healthcare, diet & nutrition, demographics, knowledge/literacy, & governmental issues (public health)
    * Break this into 2 parts: a function to find an indicator code from indicator name, and a function to output countries by list of NaN values descending:
    
    ```python
    def code_finder(ind_name):
        for i, name in enumerate(df['Indicator Name']):
            if name == ind_name:
                return df.iloc[i, 3]

    def non_null_country_list(ind):
        return sorted([(row['Country Code'], row.count())
                    for row in df[df['Indicator Code'] == ind]],
                    key=lambda x: x[1], reverse=True)
    ```

    <br>

2. This is time series data, so the go-to is a line graph with dots at each data point; however stacked-area charts and stacked-bar charts are also possibilities. The ambitious side of me wonders about using heatmaps to compare trends or a polar area graph to visualize some kind of seasonality.

Now that we've examined the indicator names for possible topics and are able to sort countries by which ones have populated rows, we can return to
our first question: what can be answered by the dataset? I found that the USA had relatively robust data, so for my first EDA I chose topics with a political spin.

<div align="center">
    <img width="500" src="/assets/images/2021-06-16-big-trouble-with-little-data/PresidentParty.png" alt="chmod Options">
</div>

To be continued!

<script src="https://polyfill.io/v3/polyfill.min.js?features=es6"></script>
<script id="MathJax-script" async src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js"></script>