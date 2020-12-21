# AB Test
## By Murong (Sophie) Cui

## Introduction
A/B tests are very commonly performed by data analysts and data scientists.  It is important that you get some practice working with the difficulties of these

For this project, I will be working to understand the results of an A/B test run by an e-commerce website.  My goal is to work through this notebook to help the company understand if they should implement the new page, keep the old page, or perhaps run the experiment longer to make their decision.

## Part I - Probability
There are 294,478 users in the campaign, 145,310 of them received the new page and 145,274 received the old page. Generally speaking, we create equal conditions to split our user sublist, approxiately, 50/50 splits is for our test.

Between two groups, old page group (control group) has 12.04% conversion rate and new page group (treatment group) has 11.89% conversion rate. it seems like that the conversion rate in control group is slightly larger than the rate in treatment group. However, it is not significant to say that the control group performs better than the treatment group.

## Part II - A/B Test
<code>
org_old_mean = df.query('group == "control"').converted.mean()<br>
org_new_mean = df.query('group == "treatment"').converted.mean()<br>
org_diff = org_new_mean - org_old_mean<br>

(np.array(p_diffs) > org_diff).mean()
</code>

The p-value is 0.8906 , which means there are 89% probability of observing a sample statistic that at least as extreme as the sample statistic when we assume that null hypothesis is true.  Our sample results are consistent with a true null hypothesis.
Therefore, we can not reject the hypothesis null.
So we can not assume that the performance of old page is better.

## Part III - A regression approach
![Alt text](pic/lr1.png?raw=true)
The function is: $l=\ln\frac{p}{1-p} = \beta_0 + \beta_1x_1$<br>
The p-value is 0.19 which is greater than 0.05, which indicated that we could not reject null hypothesis ($H_0: p_{new} = p_{old}$). In other words, there is not evident showing that control group is performing better than the treatment group.

![Alt text](pic/lr2.png?raw=true)
The final function is $$l = \ln\frac{p}{1-p} = \text{intercept} + \beta_1\text{US} + \beta_2\text{CA} + \beta_3\text{ab_page} + \beta_4\text_{US}\text{ab_page} + \beta_5\text{CA}\text{ab_page}
$$
When we add countries, and interaction between pages and countries into the model only interaction term is significant, which shows that countries and interaction between page and countries have not affect to conversion.
