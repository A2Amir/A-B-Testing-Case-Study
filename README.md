# A/B Testing Case Study
 
 In the previous repostories, you've learned about conceptual and statistical components that go in designing and analyzing an experiment. In this Repo, you'll see how the things you've learned can be used in a case study.  You'll go through the design process for a web-based experiment including:
 
   * Build a user funnel.
   * Decide on metrics.
   * Perform experiment sizing.
   * Analyze result.



# 1. Scenario Description

Let's say that you're working for a fictional productivity software company that is looking for ways to increase the number of people who pay for their software. The way that the software is currently set up, users can download and use the software free of charge, for a 7-day trial. After the end of the trial, users are required to pay for a license to continue using the software.

One idea that the company wants to try is to change the layout of the homepage to emphasize more prominently and higher up on the page that there is a 7-day trial available for the company's software. The current fear is that some potential users are missing out on using the software because of a lack of awareness of the trial period. If more people download the software and use it in the trial period, the hope is that this entices more people to make a purchase after seeing what the software can do.

In this case study, you'll go through steps for planning out an experiment to test the new homepage. You will start by **constructing a user funnel** and **deciding on metrics to track**. You'll also **perform experiment sizing to see how long it should be run**. Afterwards, you'll be given some data collected for the experiment, **perform statistical tests to analyze the results**, and come to conclusions regarding how effective the new homepage changes were for bringing in more users.

# 2. Building a Funnel

Before we do anything else, the first thing we should do is specify the objective or goal of our study:

> Revising the structure of the homepage will increase the number of people that download the software and, ultimately, the number of people that purchase a license.


Now, we should think about the activities that a user will take on the site that are relevant to measuring our objective. This path or funnel will help us figure out how we will create experimental condition groups and which metrics we'll need to track to measure the experiment's effect. To help you construct the funnel, here's some information about the way the company's website is structured, and how the software induces users to purchase a license.

The company's website has five main sections:

    1. the homepage;
    2. a section with additional information, gallery, and examples;
    3. a page for users to download the software;
    4. a page for users to purchase a license; and
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

* where and how we should split users into experiment groups (The choice of unit of diversion).
* what metrics we will use to track the success or failure of the experimental manipulation. 

The choice of unit of diversion (**the point at which we divide observations into groups**) may affect what metrics we can use, and whether the metrics we record should be considered invariant or evaluation metrics. Three main categories of diversion were presented in [this repo]() course: 



* An **event-based diversion** (like a pageview) can provide many observations to draw conclusions from, but doesn't quite hit the mark for this case. If the condition changes on each pageview, then a visitor might get a different experience on each homepage visit. **Event-based diversion is much better when the changes aren't as easily visible to users, to avoid disruption of experience**. In addition, pageview-based diversion would let us know how many times the download page was accessed from each condition, but can't go any further in tracking how many actual downloads were generated from each condition.

* Diverting based on **account or user ID** can be stable, but it's not the right choice in this case. Since visitors only register after getting to the download page, this is too late to introduce the new homepage to people who should be assigned to the experimental condition.

* This leaves the consideration of **cookie-based diversion**, which feels like the right choice. We can assign a cookie to each visitor upon their first page hit, which allows them to be separated into the control and experimental groups. Cookies also allow tracking of each visitor hitting each page, recording whether or not they eventually hit the download page and then whether or not they actually register an account and perform the download.**A cookie-based diversion** seems best in this case for dividing visitors into experimental groups since we can split visitors on their initial visit and it's fairly reliable for tracking.



That's not to say that the cookie-based diversion is perfect. The usual cookie-based diversion issues apply: we can get some inconsistency in counts if users enter the site via incognito window, different browsers, or cookies that expire or get deleted before they make a download. This kind of assignment 'dilution' could dampen the true effect of our experimental manipulation. As a simplification, however, we'll assume that this kind of assignment dilution will be small, and ignore its potential effects.


In terms of **metrics**, we might want to keep track of the number of cookies that are recorded in different parts of the website. In particular, the number of cookies on the homepage, download page, and account registration page (in order to actually make the download) could prove useful. We can track the number of licenses purchased through the user accounts, each of which can be linked back to a particular condition. Though it hasn't been specified, it's also possible that the software includes usage statistics that we could track.

The above metrics are all based on absolute counts. We could instead perform our analysis on ratios of those counts. For example, we could be interested in the proportion of downloads out of all homepage visits. License purchases could be stated as a ratio against the number of registered users (downloads) or the original number of cookies.


**There's one invariant metric that really stands out here, and that's the number of cookies that hit the homepage. If we've done things correctly, each visitor should have an equal chance of seeing each homepage, and that means that the number of cookies assigned to each group should be about the same.** Since visitors come in without any additional information (e.g. account info) and the change effected by the experimental manipulation comes in right at the start, there aren't other invariant metrics we should worry about.

**Selecting evaluation metrics is a trickier proposition. Count-based metrics at other parts of the process seem like natural choices: the number of times the software was downloaded and the number of licenses purchased are exactly what we want to change with the new homepage. The issue is that even though we expect the number of cookies assigned to each group to be about the same, it's much more likely than not they they won't be exactly the same. Instead, we should prefer using the download rate (# downloads / # cookies) and purchase rate (# licenses / # cookies) relative to the number of cookies as evaluation metrics. Using these ratios allows us to account for slight imbalances between groups.

As for the other proposed metrics, the ratio between the number of licenses and number of downloads is potentially interesting, but not as direct as the other two ratios discussed above. It's possible that the manipulation increases both the number of downloads and number of licenses, but increases the former to a much higher rate. In this case, the licenses-to-downloads ratio might be worse off for the new homepage compared to the old, even though the new homepage has our desired effects. There's no such inconsistency issue with the ratios that use the number of cookies in the denominator.

Product usage statistics like the average time the software was used in the trial period are potentially interesting features, but aren't directly related to our experiment. We might not have a strong feeling about what kind of effect the homepage will have on people that actually download the software. Stated differently, product usage isn't a direct target of the homepage manipulation. Certainly, these statistics might help us dig deeper into the reasons for observed effects after an experiment is complete. They might even point toward future changes and experiments to conduct. But in terms of experiment success, product usage shouldn't be considered an invariant or evaluation metric.


