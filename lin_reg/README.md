# lin_reg

## Interaction

- [x] YouTube [Interpreting Interaction in Linear Regression](https://www.youtube.com/watch?v=vZUtDJbzFRQ)

    * Concept of interaction
    * Beta interpretation for two categorical variables with two levels each
    * $Y$ length of stay in hospital
    * $X_1$ smoking - binary
    * $X_2$ asbestos exposure - binary
    * **Interaction** - the effect of smoking depends on whether or not there is exposure to asbestos and the effect of asbestos exposure depends on smoking

![ ](lin_reg.png)

    * $b_0 = 21.5$ intersect term tells us the mean length of stay for the reference group (non-smokers and no exposure to asbestos occurs)
    * $b_1 = 11.1 = 32.6 - 21.5$ the smoking effect for someone not exposed to asbestus
    * $b_2 = 14.6 = 36.1 - 21.5$ the asbestus effect for non-smokers
    * $b_3 = 23.1 = 70.3 - 14.6 - 32.6$ interaction effect of smoking when exposed to asbestus

![ ](interactions.png)

![ ](no_interactions.png)

    * there must be a good reason to include an interaction term into the model
        - it should make sense conceptually
        - interaction term should be statistically significant

- [x] My interpretation of $\beta$

    * A direct interpretation of $beta$ depends on how you worked your data. Were the columns standardized? Did data went through a pca transformation? So let's think of the simplest case where features are standardizd and features are not correlated. In this case, the $beta$ is simply the correlation between each feature and the dependent variable weighted down by the variance of each feature...if a variable is highly correlated with the dependent variable but has high variance then...




