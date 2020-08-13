# A/B Testing Case Study
 
 In the previous repostories, I've talked about 
 *  1. [The Experimental Design](https://github.com/A2Amir/Experimental-Design) 
 *  2. [The Statistical Considerations in Testing](https://github.com/A2Amir/Statistical-Considerations-in-Testing) 
 
 components that go in designing and analyzing an experiment. In this Repo, you'll see how these components you've learned can be used in a case study.  You'll go through the design process for a web-based experiment including:
 
   * Building a user funnel.
   * Deciding on metrics.
   * Performing experiment sizing.
   * Checking Validity, Bias, and Ethics
   * Analyzing data.



# Scenario Description
 
 
Let's say that you're working for a fictional productivity software company that is looking for ways to increase the number of people who pay for their software. The way that the software is currently set up, users can download and use the software free of charge, for a 7-day trial. After the end of the trial, users are required to pay for a license to continue using the software.

**One idea that the company wants to try is to change the layout of the homepage to emphasize more prominently and higher up on the page that there is a 7-day trial available for the company's software**. The current fear is that some potential users are missing out on using the software because of a lack of awareness of the trial period. If more people download the software and use it in the trial period, the hope is that this entices more people to make a purchase after seeing what the software can do.

In this case study, you'll go through steps for planning out an experiment to test the new homepage. You will start by **constructing a user funnel** and **deciding on metrics to track**. You'll also **perform experiment sizing to see how long it should be run**. Afterwards, you'll be given some data collected for the experiment, **perform statistical tests to analyze the results**, and come to conclusions regarding how effective the new homepage changes were for bringing in more users.

# 1. Building a Funnel

Before we do anything else, the first thing we should do is specify **the objective or goal of our study**:

> Revising the structure of the homepage will increase the number of people that download the software and, ultimately, the number of people that purchase a license.



Now, we should think about the activities that a user will take on the website that are relevant to measuring our objective. This path or funnel will help us figure out how we will create experimental condition groups and which metrics we'll need to track to measure the experiment's effect. To help you construct the funnel, here's some information about the way the company's website is structured, and how the software induces users to purchase a license.

The company's website has five main sections:

    1. the homepage;
    2. a section with additional information, gallery, and examples;
    3. a page for users to download the software;
    4. a page for users to purchase a license; 
    5. a support sub-site with documentation and FAQs for the software.

For the software itself, the website requires that users create an account in order to download the software program. The program is usable freely for seven days after download. When the trial period is hit, the program will bring up a dialog box that takes the user to the license page. After purchasing a license, the user will receive a unique code associated with their site account. This code can then be used with the program to register it with that user, and the program can be used thereafter without issue.


#### A straightforward funnel might include the following steps:

        1.  Visit homepage
        2.  Visit download page
        3.  Sign up for an account
        4.  Download software
        5.  After 7-day trial, software takes user to license-purchase page
        6.  Purchase license
   
   Note that it is possible for the visitor to drop from the funnel after each step. There might be additional steps that a user might take between visiting the homepage and visiting the download page that aren't accounted for in the above funnel. For example, someone might want to check out the additional informational pages before visiting the download page, or even visit the license purchase page to check the license price before even deciding to download. Considering the amount of browsing that a visitor could perform on the page, it might be simplest just to track whether or not a user gets to the download page at some point, without worrying about the many paths that they could have taken to get there.


#### Atypical events

There are a few events in the expected funnel that might not correspond with the visitors we want to target. For example, there might be users on the homepage who aren't new users. Users who already have a license might just be visiting the homepage as a way to access the support sub-site. A user who wants to buy a license might also come in to the license page through the homepage, rather than directly from the software.

When it comes to license purchasing, it's possible that users don't come back after exactly seven days. Some users might come back early and make their purchase during their trial period. Alternatively, a user might end up taking more than seven days to decide to make their purchase, coming back days after the end of the trial. Anticipating scenarios like this can be useful for planning the design, and coming up with metrics that come as close as possible to measuring desired effects.


# 2. Deciding on Metrics

From our user funnel, we should consider two things: 

* **where and how we should split users into experiment groups (The choice of unit of diversion)**.
* **what metrics we will use to track the success or failure of the experimental manipulation**. 

The choice of unit of diversion (**the point at which we divide observations into groups**) may affect what metrics we can use, and whether the metrics we record should be considered invariant or evaluation metrics. Three main categories of diversion were presented in [this repo](https://github.com/A2Amir/Experimental-Design) course: 



* An **event-based diversion** (like a pageview) can provide many observations to draw conclusions from, but doesn't quite hit the mark for this case. If the condition changes on each pageview, then a visitor might get a different experience on each homepage visit. **Event-based diversion is much better when the changes aren't as easily visible to users, to avoid disruption of experience**. In addition, pageview-based diversion would let us know how many times the download page was accessed from each condition, but can't go any further in tracking how many actual downloads were generated from each condition.

* Diverting based on **account or user ID** can be stable, but it's not the right choice in this case. Since visitors only register after getting to the download page, this is too late to introduce the new homepage to people who should be assigned to the experimental condition.

* This leaves the consideration of **cookie-based diversion**, which feels like the right choice. We can assign a cookie to each visitor upon their first page hit, which allows them to be separated into the control and experimental groups. Cookies also allow tracking of each visitor hitting each page, recording whether or not they eventually hit the download page and then whether or not they actually register an account and perform the download.**A cookie-based diversion** seems best in this case for dividing visitors into experimental groups since we can split visitors on their initial visit and it's fairly reliable for tracking.



That's not to say that the cookie-based diversion is perfect. The usual cookie-based diversion issues apply: we can get some inconsistency in counts if users enter the site via incognito window, different browsers, or cookies that expire or get deleted before they make a download. This kind of assignment 'dilution' could dampen the true effect of our experimental manipulation. As a simplification, however, we'll assume that this kind of assignment dilution will be small, and ignore its potential effects.


In terms of **metrics**, we might want to keep track of the number of cookies that are recorded in different parts of the website. In particular, the number of cookies on the homepage, download page, and account registration page (in order to actually make the download) could prove useful. The described metrics are all based on **absolute counts**. We could instead perform our analysis on **ratios of those counts**. For example, we could be interested in the proportion of downloads out of all homepage visits. License purchases could be stated as a ratio against the number of registered users (downloads) or the original number of cookies.

> **There's one invariant metric that really stands out here, and that's the number of cookies that hit the homepage. If we've done things correctly, each visitor should have an equal chance of seeing each homepage, and that means that the number of cookies assigned to each group should be about the same.** Since visitors come in without any additional information (e.g. account info) and the change effected by the experimental manipulation comes in right at the start, there aren't other invariant metrics we should worry about.

> **Selecting evaluation metrics is a trickier proposition. Count-based metrics at other parts of the process seem like natural choices: the number of times the software was downloaded and the number of licenses purchased are exactly what we want to change with the new homepage. The issue is that even though we expect the number of cookies assigned to each group to be about the same, it's much more likely than not they they won't be exactly the same. Instead, we should prefer using the download rate (# downloads / # cookies) and purchase rate (# licenses / # cookies) relative to the number of cookies as evaluation metrics. Using these ratios allows us to account for slight imbalances between groups.**

The other metrics like the ratio between the number of licenses and number of downloads are potentially interesting, but not as direct as the other two ratios discussed above. It's possible that the manipulation increases both the number of downloads and number of licenses, but increases the former to a much higher rate. In this case, the licenses-to-downloads ratio might be worse off for the new homepage compared to the old, even though the new homepage has our desired effects. There's no such inconsistency issue with the ratios that use the number of cookies in the denominator.

Product usage statistics like the average time the software was used in the trial period are potentially interesting features, but aren't directly related to our experiment. We might not have a strong feeling about what kind of effect the homepage will have on people that actually download the software. Stated differently, product usage isn't a direct target of the homepage manipulation. Certainly, these statistics might help us dig deeper into the reasons for observed effects after an experiment is complete. They might even point toward future changes and experiments to conduct. But in terms of experiment success, product usage shouldn't be considered an invariant or evaluation metric.



 
# 3. Experiment Sizing

Now that we have our main metrics selected: **number of cookies as an invariant metric**, and **the download rate and license purchase rate (relative to number of cookies) as evaluation metrics**, we should take a look at the feasibility of the experiment in terms of the amount of time it will take to run. 

Because we have two evaluation metrics of interest, we should make sure that we are **choosing an appropriate significance level to conduct each test** in order to preserve a maximum overall Type I error rate of .05. Since we would be happy to deploy the new homepage if either download rate or license purchase rate showed a statistically significant increase, performing both individual tests at a .05 error rate carries the risk of making too many Type I errors. As such, we'll apply **the Bonferroni correction** to run each test at a .025 error rate so as to protect against making too many errors. If it were the case that we needed to see both metrics with a statistically significant increase, then we wouldn't need to include the correction on the individual tests.

Recent history shows that there are about **3250 unique visitors per day**, with slightly more visitors on Friday through Monday, than the rest of the week. There are about **520 software downloads per day (a .16 rate)** and about **65 licenses purchased each day (a .02 rate)**. In an ideal case, both the download rate and license purchase rate should increase with the new homepage; a statistically significant negative change should be a sign to not deploy the homepage change. However, if only one of our metrics shows a statistically significant positive change we should be happy enough to deploy the new homepage.

* Let's say that we want to detect an increase of 50 downloads per day.To calculate how many days of data we need to collect in order to get enough visitors to detect this new rate at an overall 5% Type I error rate and at 80% power I used the code presented below:
~~~python
from statsmodels.stats.power import NormalIndPower
from statsmodels.stats.proportion import proportion_effectsize
s = NormalIndPower().solve_power(effect_size=proportion_effectsize(520/3250,570/3250), power=0.8, alpha=0.05/2)

print(s)
print(s / 3250 )


11204.701585895478
3.4476004879678395

~~~

**We'd need at least four days to get the 11204 visitors in each condition to detect a 0.015 (570/3250 -520/3250) increase in the download rate.**

* What if we wanted to detect an increase of 10 license purchases per day (.023 rate). In order to caculate how many days of data  we need to collect in order to get enough visitors to detect this new rate at an overall 5% Type I error rate and at 80% power I used the below code:

~~~python
from statsmodels.stats.power import NormalIndPower
from statsmodels.stats.proportion import proportion_effectsize
s = NormalIndPower().solve_power(effect_size=proportion_effectsize(65/3250,75/3250), power=0.8, alpha=0.05/2)

print(s)
print(s / 3250 )

42263.140542608366
21.004043243879497

~~~
**We'd need at least 21 days to get the 42263 visitors in each condition to detect a 0.0031 (75/3250 -65/3250) increase in the download rate.**

One thing that isn't accounted for in the base experiment length calculations is that there is going to be a delay between when users download the software and when they actually purchase a license. That is, when we start the experiment, there could be about seven days before a user account associated with a cookie actually comes back to make their purchase. Any purchases observed within the first week might not be attributable to either experimental condition. As a way of accounting for this, we'll run the experiment for about one week longer to allow those users who come in during the third week a chance to come back and be counted in the license purchases tally.



# 4. Validity, Bias, and Ethics 

We probably don't have too much to worry about in terms of validity. For conceptual validity, the evaluation metrics are directly aligned with the experimental goals, no abstraction needed. Internal validity is maintained by performing an experiment with properly-handled randomization and controls. We don't really need to answer to external validity since we're drawing from the full site population and there's no other population we're looking to generalize to.

As for biases, we might think of novelty bias as being a potential issue. However, we don't expect users to come back to the homepage regularly. Downloading and license purchasing are actions we expect to only occur once per user, so there's no real 'return rate' to worry about. One possibility, however, is that if more people download the software under the new homepage, the expanded user base is qualitatively different from the people who came to the page under the original homepage. This might cause more homepage hits from people looking for the support pages on the site, causing the number of unique cookies under each condition to differ. If we do see something wrong or out of place in the invariant metric (number of cookies), then this might be an area to explore in further investigations.


Finally, for ethical issues, the changes to the homepage should be benign and present no risk to users. Our experiment objectives are also clearly stated. Considering the low risks of the experiment, informed consent is at worst a minor concern; a standard popup to let visitors know that cookies are used to track user experience on the site will likely suffice. The largest ethics principle we should be concerned about is data sensitivity. We shouldn't get any sensitive data out of the cookie assignment and collection, though some information will be collected from the user when they go to download the software. No sensitive data is required for the metrics we've laid out, so what we should do is just aggregate daily visits, downloads, and purchase counts without looking at any individual outcomes.

# 5. Analyze Data

By looking at [the collected data](https://github.com/A2Amir/A-B-Testing-Case-Study/blob/master/data/homepage-experiment-data.csv) can be found that  data was collected for 29 days. As a reminder of the discussion on experiment sizing, it was found that a three-week period was needed to collect enough visitors to achieve our desired power level. Eight additional days of collection were added to allow visitors in the last week to complete their trials and come back to make a purchase – if you look at the data linked, you will see that it takes about eight days before the license purchases reaches its steady level.


#### Invariant Metric

First, we should check our invariant metric (the number of cookies assigned to each group). If there is a statistically significant difference detected, then we shouldn't move on to the evaluation metrics right away. We'd need to first dig deeper to see if there was an issue with the group-assignment procedure, or if there is something about the manipulation that affected the number of cookies observed, before we feel secure about analyzing and interpreting the evaluation metrics.

~~~python
import seaborn
import pandas as pd 
import numpy as np

data = pd .read_csv('./data/homepage-experiment-data.csv',header=0)

plt.figure(figsize=(15,5))
plt.subplot(1,2,1)
# left plot: showing kde lumps with the default settings
sb.distplot(data['Control Cookies'].values,hist=False,rug=True ,rug_kws={'color':'r'})
plt.title('the mean of %.3f and the standard deviation of %.3f'%(np.mean(data['Control Cookies'].values),np.std(data['Control Cookies'].values)))

plt.subplot(1,2,2)

# central plot: kde with narrow bandwidth to show individual probability lumps
sb.distplot(data['Experiment Cookies'].values,hist=False,rug=True ,rug_kws={'color':'r'})
plt.title('the mean of %.3f and the standard deviation of %.3f'%(np.mean(data['Experiment Cookies'].values),np.std(data['Experiment Cookies'].values)))

~~~

<p align="center">
  <img src="/data/1.PNG" alt="" width="700" height="400" >
 </p>

#### Evaluation Metrics

Assuming that the invariant metric passed inspection, we can move on to the evaluation metrics: **download rate and license purchasing rate**. For a refresher, **the download rate is the total number of downloads divided by the number of cookies, and the license purchasing rate the number of licenses divided by the number of cookies**.

One tricky point to consider is that there is a seven or eight day delay between when most people download the software and when they make a purchase. There's no direct way of attributing cookies all the way through license purchases due to the daily aggregation of results, so the best we can do is to make a justified argument for handling the data. To answer the question about the license purchasing rate, you should only take the cookies observed through day 21 as the denominator of the ratio as being responsible for all of the license purchases observed.

~~~python

data.iloc[7:,:].sum()



Control Cookies         35681
Control Downloads        5808
Control Licenses          695
Experiment Cookies      35872
Experiment Downloads     6503
Experiment Licenses       710
dtype: int64

control_download_rate = round(5808/35681,4)
print(control_download_rate)  -> 0.1628


control_license_purchase = round(695/35681,4)
print(control_license_purchase) -> 0.0195


experiment_download_rate  = round(6503/35872,4)
print(experiment_download_rate) -> 0.1813

experiment_license_purchase = round(710/35872,4)
print(experiment_license_purchase) -> 0.0198
~~~

