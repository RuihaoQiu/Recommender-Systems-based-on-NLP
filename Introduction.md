## Introduction
Recommender systems is one of the most successful machine learning applications. We are exposed to the products/informations recommended by these systems everywhere in our daily life.

This project will introduce several recommender systems in NLP, specifically in the domain of online recruitment. I will explain the important ideas behind the recommender systems in our web application, it might be also similar with those in recruiting platforms like LinkedIn, Xing.

One of my main jobs is to design and develop recommender systems on the skill-based matching app. Before getting into this specific topic, I would like to go over the general concepts and methods of current recommender system in different domains.

### Recommender system in general
Recommender system is just a machine that introduce the users to products that they might be interested in. They can be:

- E-commerce platforms, such as Amazon, recommend their customers various of products based on huge amount of data from millions of customers and products.
- Entertainment providers, such as Youtube, Spotify and Netflix, recommend songs and movies to their users.
- Job platforms, such as LinkedIn, they are making two way recommendation, recommend jobs to job seeker, as well as, recommend profiles to job providers.
- Advertisement, real-estate market, dating market …

Overall, in terms of methodology, there are two different kinds of recommender systems  - collaborative filter and content-based filter.

### Collaborative filter
The Collaborative filter recommend items based on user's historical records, the user's explicit interest (like/dislike, rating or reviews) and implicit interest (clicks on an item, bookmarking an item’s URL, time spent on the page)

The past interactions recorded between users and items are stored in the so-called “user-item interactions matrix”. Each row represent a user and each column is a item, the interaction can be 0/1 (0-no record, 1-like) or rating level (e.g. 0-5). The main task is to decompose this matrix into two set of vectors - users and items. This is called embedding.

**Direct embedding**<br>
We can use the user(each row)/item(each column) vector from the matrix.
1. **based on user similarity**:
calculate the user similarity by the vectors, for a given user, find his nearest neighbors and recommend him the most popular items of his neighbors.
1. **based on item similarity**:
calculate the item similarity for each item, for a given user, based on his favorite items, recommend their most similar items to the user.

**Model-based embedding**<br>
Since the matrix can be huge and very sparse, and it can not scale easily. The alternative way is to decompose the huge and sparse user-item interaction matrix into a product of two smaller and dense matrices (the user vectors and item vectors). This method is called matrix factorization.

1. **Alternating least squares (ALS)**
Use all the records as the labels, alternative train the user vectors U and item V with gradient descent algorithm.

1. **Deep learning**
Simply concatenate the user and item vectors as the input feature and use corresponding records as the labels, train a deep leaning model on them.
This architect can be extended to including other content features from both the user and item and other implicit information(user behaviors, like/dislike and any other actions).

### Content-based filter
Content-based filter recommend item to user only based on based on the item/user's own content. It can be either a classification problem (predict like/dislike) or regression problem (predict rating).

1. Item-centered model
Use user's features (e.g. personal information, behaviors etc.) to predict their preference for each item. For example, a item is voted for 1000 users, train a model these users' profile, then we can predict how a new user will vote for this item.

1. User-centered model
Use the item's features(e.g. product information) to predict the user's preference for the item. For example, a user has listened 1000 songs, train a model on these song's characters, then we can predict how the user will vote for a new song.

### Mix approach
As I mentioned above, the deep learning architect can include different information. It can be viewed as a mix approach between collaborative filter and content-based filter.

I won't go into more details here, feel free to read this article see how youtube implement this architect in their system - [Deep Neural Networks for YouTube Recommendations.](https://static.googleusercontent.com/media/research.google.com/en//pubs/archive/45530.pdf)


**References**
1. [Matrix Factorization Techniques for Recommender Systems (Netflix prize paper)](https://datajobs.com/data-science-repo/Recommender-Systems-[Netflix].pdf)
1. [Introduction to recommender systems](https://towardsdatascience.com/introduction-to-recommender-systems-6c66cf15ada)
1. [Deep Neural Networks for YouTube Recommendations](https://static.googleusercontent.com/media/research.google.com/en//pubs/archive/45530.pdf)
