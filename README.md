# A/B Testing with Lingua

## Table of Contents
1. [Project Overview](#project-overview)
2. [Dataset Description](#dataset-description)
3. [Methods and Approach](#methods-and-approach)
4. [Key Findings](#key-findings)
5. [Limitations and Future Work](#limitations-and-future-work)
6. [Repository Structure](#repository-structure)
7. [Technical Skills Demonstrated](#technical-skills-demonstrated)

## Project Overview

This project involved simulating and analyzing A/B testing data for a fictional language learning app called *Lingua*. The primary objective was to evaluate whether gamification features (e.g., streaks, badges, leaderboards) improved user engagement and learning compared to a minimalist, text-based version of the app. Analyses were conducted using Bayesian statistical methods.

## Dataset Description

The dataset for this project was simulated using the `simstudy` package in *R*. The dataset consisted of 1,000 users who were randomly assigned to either the control (A) or gamified (B) version of the app. Users were tracked over a one-month period. The dataset includes:

- `user_id`: Unique identifier for each user.
- `testing_group`: Categorical variable indicated the group that the user was assigned to (A = control, B = gamified)
- `gender`: Self-reported gender at registration (Male, Female).
- `location`: Geographic location of user (North America, Europe).
- `user_age`: User's self-reported age in years.
- `proficiency_level`: Self-reported language proficiency at registration (Beginner, Intermediate, Advanced).
- `time_spent`: Total time spent on the app during the testing period, averaged to produce a daily figure (in minutes).
- `goal_completed`: Whether or not a given user completed the monthly lesson goal (i.e., 30 lessons; 0 = did not complete monthly goal, 1 = completed monthly goal)
- `satisfaction`: Ordinal variable representing self-reported user satisfaction (measured on a 1-10 Likert-type scale). 

## Methods and Approach

1. **Data Simulation**:
   - Used `simstudy` to define variable distributions and dependencies.
   - Created conditional variables (e.g., `time_spent` was influenced by `testing_group`, `proficiency_level`, `user_age`, and `gender`).
2. **Data Cleaning**
    - Applied transformations and factor conversions for usability in later analyses.
3. **Exploratory Data Analysis**:
   - Generated an interactive table for the raw data using `DT`.
   - Used `skimr`, `gtsummary`, and `correlation` for summary statistics and correlations.
   - Built visualizations (e.g., histograms, bar plots, box plots) using `ggplot2` to better understand the structure of the data.
3. **Bayesian Analysis**:
   - *Goal Completion*: Used Bayesian beta distributions to compare completion rates between groups.
   - *Time Spent*: Conducted a series of Bayesian *t*-tests (while varying prior distributions) to compare mean usage time between groups.
   - *Satisfaction*: Performed Bayesian linear regression (simple and multiple) to estimate the effect of gamification on user satisfaction.
   - Assessed results using posterior distributions, credible intervals, and Bayes Factors.
   - Used Region of Practical Equivalence (ROPE) testing to evaluate practical significance of effects.

## Key Findings

### Gamification Increases Goal Completion Rates

- The posterior probability distributions *strongly support* the inference that gamification will lead to an increase in goal completion rates if gamification is rolled out globally.
- The probability of direction measure was 100% - meaning that we can be *(practically) certain* that gamification will lead to an increase in goal completion rates, rather than a decrease or no change.
- ROPE analyses indicated that *we can be confident* that gamification of the app will lead to a practically significant increase in goal completion rates.

### Gamification Increases Time Spent on the App

- Bayesian *t*-tests provided *strong evidence* that the experimental group spent more time on the app, and that we can expect to see this increase if gamification was applied to the app for all users.
- Even under conservative priors and robust testing, the alternative hypothesis (i.e., that the time spent by the two groups differed) was *highly supported*.
- ROPE analysis indicated that the effect on time spent is practically significant, and that gamification is *extremely likely* to lead to a meaningful increase in user engagement.

### Gamification Increases User Satisfaction, But the Effect is Modest

- Bayesian simple regression indicated that users in group B reported *higher satisfaction* with the app than users in group A.
- Based on the Probability of Direction (PD) and 95% credible intervals, we can  be *highly confident* in a positive effect occurring in the user population at large.
- That being said, the *magnitude of this effect was modest*. The practical impact of gamification on user satisfaction will depend on implementation costs. 

### Multiple Predictors Influence Satisfaction

- A Bayesian multiple regression model found that the *strongest predictors* of user satisfaction were:
  - Time spent on the app;
  - Goal completion;
  - Gamification (i.e., whether a user was in the gamification condition or the control condition)
- Gamification *remained a significant predictor* even after controlling for other user engagement metrics and demographic factors.
- Model diagnostics indicated *strong predictive validity*, but there is room for further optimization. 

## Limitations and Future Directions

### Limitations

1. *Short-Term Engagement vs. Long-Term Effects*
    - The A/B test spanned only one month, limiting our ability to make any inferences about whether the gamification of the app would lead to sustained increases in user engagement.
  - Future work should assess the effect of gamification over a longer time period (e.g., 3-6 months).
2. *Potential Confounds and Additional Predictors*
    - While several important demographic and behavioral features were included, other unmeasured factors (e.g., intrinsic vs. extrinsic motivation, previous language app experience) may also influence outcomes or interact with existing features.
    - Future research could incorporate other psychological (e.g., personality) and behavioral (e.g., subscription status) variables.
3. *Generalizability*
    - The A/B test was based only on users from North America and Europe. Although these are the dominant markets for the app, this does limit our ability to generalize these results to other regional markets.
    - Future studies should include other regional markets and examine geographic location more granularly (e.g., by nation).

### Future Directions  

1. *Long-Term A/B Testing*
    - As mentioned previously, increasing the length of the follow-up periods may help determine if gamification effects persist or if gamification only provides an initial novelty effect.
2. *Optimizing Gamification Features*
    - A factorial experiment could assess the individual and combined impact of specific gamification elements (e.g., streaks by themselves, streaks in combination with badges) in order to identify the most impactful features/combinations.
3. *Personalization of Gamification*
    - Future research could also examine whether gamification effectivenesss varies according to individual difference variables (e.g., personality variables like conscientiousness, learning style).
    - If our models are sufficiently accurate, gamification could be tailored to the user based on model predictions. 
4. *Revenue and Monetization Impact*
    - Research is needed to formally investigate the relationship between user engagement and revenue metrics (e.g., subscriptions, merchandise, in-app purchases) An ROI analyses is needed to quantify the business value of gamification beyond increased user engagement.

## Repository Structure

- `data/`: Contains web links to the dataset and related documentation.
- `analysis/`: Includes analysis scripts (`.qmd`) and rendered outputs (`.html`).

## Technical Skills Demonstrated

- **Data Simulation**: Used `simstudy` to generate realistic A/B testing data.
- **Data Wrangling and Preprocessing**: Leveraged `dplyr` for data cleaning and transformations.
- **Exploratory Data Analysis**: Used `skimr`, `dplyr`, `gtsummary`, and `ggplot2` to explore the data through summary statistics and visualizations.
- **Bayesian Statistics**: Conducted Bayesian analyses using `rstanarm`, `BayesFactor`, and `bayestestR`. Used various packages from the `easystats` ecosystem (e.g., `see`, `performance`, `parameters`) to extract further information from models.
- **Data Visualization**: Visualized posterior distributions, ROPE analyses, and regression plots with `bayesplot`, `ggplot2`, and `see`.
- **Quarto**: Created clear documentation and an interactive report using the `.qmd` file format.

## Conclusion

This project demonstrates my ability to utilize Bayesian statistics in a UX context, and highlights the utility of Bayesian statistics in communicating important information to stakeholders in an intuitive way.
