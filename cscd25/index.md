---
layout: page
title: CSCD25 Project
---

## Activity Diversity on Lemmy vs Reddit: Are Users More Specialized on the Open-source Platform?

In the past few years, the so-called 'Fediverse' has emerged as a decentralized alternative to traditional social media platforms. One of the most popular Fediverse platforms after Mastodon, especially since Reddit's API controversy this summer, is Lemmy, a Reddit-like community-based platform.

However, most people have probably not even heard of Lemmy, which begs the question: How diverse is Lemmy's user base compared to Reddit's? Are most of the users just open-source enthusiasts, interested in mainly the same topics, or is it actually realistic that Lemmy will grow into a platform as diverse as Reddit?

In this project, I have analyzed data that I scraped myself from Lemmy to answer the following two research questions:

1. How diverse is the average Lemmy user's activity compared to the average Reddit user's activity?
2. Are there certain patterns in the activity diversity of Lemmy users, not present in the Reddit user base, and vice versa?

In the following, I will present my findings and try to answer these questions by comparing them to the findings about Reddit in I. Waller and A. Anderson's paper [Generalists and Specialists: Using Community Embeddings to Quantify Activity Diversity in Online Platforms](http://csslab.cs.toronto.edu/gs/actdiv-www2019.pdf){: target="_blank"} (2019).

### Data Collection

I wrote a custom Lemmy API scraper, that I have open-sourced [here](https://github.com/Zollerboy1/scrape_lemmy){: target="_blank"}. Running it, I got a data set consisting of all posts on Lemmy since August 1, 2023, all comments on these posts, and all users who posted or commented on these posts.

The data set consists of almost 300,000 posts, of which I have the ID, community ID, creator ID, date, title, and score (upvotes - downvotes). I also have the ID, creator ID, date, parent post ID, parent comment ID, and the score of all of the almost 1.1 million comments on these posts. Finally, I have the ID, name, creation date, and total score (sum of all scores) of both the posts and comments of all the creators of those posts and comments, which only amounted to about 50,000 users.

Furthermore, I have some metadata about the communities and the Lemmy instances themselves.

### User GS-Score
{: #gs-score}

![Empirical distributions of the GS-score on Lemmy](/assets/img/gs_score_hist.png){: .figure}{: #fig1}

Figure 1: Empirical distributions of the GS-score on Lemmy
{: .figure-caption}

To measure a user's activity diversity, I used the GS-score as defined by Waller and Anderson. To calculate it, I first created community embeddings using a modified word2vec algorithm. With these embeddings, a user's GS-score is the weighted average cosine similarity between the embeddings of the communities they have posted in and commented in. Additionally, I calculated a GS-score for each community, which is the weighted GS-score of all users who have posted or commented in that community.

As you can see in [Figure 1](#fig1), users on Lemmy seem to be relatively generalist, with a mean GS-score of 0.58 for users who have posted or commented in at least 3 communities.

When we compare the GS-scores of Lemmy users to those of Reddit users as presented in Waller and Anderson's paper (see [Figure 2](#fig2)), we can see some interesting differences in the distributions. First of all, while there are quite a lot of Reddit users with a GS-score close to 1, i.e. they are very specialized, there are almost no Lemmy users with a score close to 1. I think this is because Lemmy is still a relatively small platform where only the biggest communities actually have much meaningful posting and commenting activity. This means that users who post in multiple communities will almost always post in the biggest communities, which are probably also the most generalist communities. We will take a closer look at this in the [section about community GS-scores](#community-gs-score). Also, the biggest chunk of users on Lemmy is active in 3-5 communities, while for Reddit many users are active in 12 and more subreddits. This probably stems from the fact that Reddit has a much bigger user base, and thus also a much bigger variety of communities, than Lemmy.

![Empirical distributions of the GS-score on Reddit](/assets/img/gs_score_hist_reddit.png){: .figure}{: #fig2}

Figure 2: Empirical distributions of the GS-score on Reddit (Waller & Anderson, 2019)
{: .figure-caption}

### User Success
{: #user-success}

![Comment success versus author GS-score on Lemmy](/assets/img/gs_score_vs_success.png){: .figure}{: #fig3}

Figure 3: Comment success versus author GS-score on Lemmy
{: .figure-caption}

Next we will look at comment success, another measure introduced by Waller and Anderson. The comment success is the percentage of comments that a user has made which have a higher score than their immediate parent comment.

Comparing the comment success to the GS-score, we can see that there is not much of a correlation between the two on Lemmy (see [Figure 3](#fig3)). This means that users who are more specialized are not necessarily more successful. This is in contrast to Reddit, where these two features are clearly positively correlated (see [Figure 4](#fig4)). However, we can also see, that the average comment success on Lemmy is much higher than on Reddit. If I had to guess, this is again because Lemmy is relatively small in comparison, which means that good comments have less competition and thus a higher chance of being upvoted.

![Comment success versus author GS-score on Reddit](/assets/img/gs_score_vs_success_reddit.png){: .figure}{: #fig4}

Figure 4: Comment success versus author GS-score on Reddit (Waller & Anderson, 2019)
{: .figure-caption}

### Diversity of exposure
{: #diversity-of-exposure}

![Distribution of the parent-universe GS-scores as a function of user GS-score on Lemmy](/assets/img/gs_score_vs_parent.png){: .figure}{: #fig5}

Figure 5: Distribution of the parent-universe GS-scores as a function of user GS-score on Lemmy
{: .figure-caption}

The parent-universe GS-score is a measure of the diversity of exposure of a user. It is the average GS-score of all the users, whose comments a user has replied to. This means that it measures how diverse the users are, who a user is exposed to. A high parent-universe GS-score means that a user is mainly exposed to very specialist users, while a low score means that a user is mainly exposed to very generalist users.

As we can see in [Figure 5](#fig5), the mean parent-universe GS-score doesn't really change much with the user GS-score on Lemmy. However, we can see that generalist users are mostly exposed to other generalist users, while specialist users respond to comments from users of all GS-scores.

In comparison, on Reddit, there is again a clear correlation between the two features (see [Figure 6](#fig6)). This means that specialist users on Reddit are also mainly exposed to other specialist users, while generalist users are more likely to be exposed to other generalist users.

![Distribution of the parent-universe GS-scores as a function of user GS-score on Reddit](/assets/img/gs_score_vs_parent_reddit.png){: .figure}{: #fig6}

Figure 6: Distribution of the parent-universe GS-scores as a function of user GS-score on Reddit (Waller & Anderson, 2019)
{: .figure-caption}

### Community GS-Score
{: #community-gs-score}

![Community size versus community GS-score on Lemmy](/assets/img/community_gs_score_vs_num_users.png){: .figure}{: #fig7}

Figure 7: Community size versus community GS-score on Lemmy
{: .figure-caption}

Finally, we take a look at the community GS-scores. These are the weighted average GS-scores of all the users who have posted or commented in a community. This means that they measure how diverse the users of a community are. A high community GS-score means that the community is mainly made up of specialist users, while a low score means that the community is mainly made up of generalist users.

We can see in [Figure 7](#fig7), that all the big communities on Lemmy are very generalist. While this trend is also there for Reddit (see [Figure 8](#fig8)), the big communities on Lemmy are much more skewed towards a lower community GS-score. Again, the most likely explanation for this is that Lemmy hasn't established itself as a platform with a lot of different communities yet, and thus the biggest communities are frequented by almost all users, making them very generalist. Reddit on the other hand is known and loved especially for the fact that there are so many different big communities and you can find many other specialists who are interested in the same niche topics as you.

![Community size versus community GS-score on Reddit](/assets/img/community_gs_score_vs_num_users_reddit.png){: .figure}{: #fig8}

Figure 8: Community size versus community GS-score on Reddit (Waller & Anderson, 2019)
{: .figure-caption}

### Conclusion
{: #conclusion}

From the data I have collected and analyzed, I can conclude that Lemmy users are generally more generalist than Reddit users. Lemmy just hasn't established itself as a platform for specialists yet, as the user base is so small that even though there are communities for many different topics, they are only frequented by a handful of users (which can then be hardcore specialists), and thus not very interesting for the average user. Therefore, I think that Lemmy will not grow into a platform as diverse as Reddit, but rather stay a platform for open-source enthusiasts and other generalists, unless it manages to attract a lot more users. In general, I think that the Fediverse is a great idea, but it will probably never be able to compete with the big social media platforms, as it is just too hard to get a critical mass of users to switch to a new platform.
