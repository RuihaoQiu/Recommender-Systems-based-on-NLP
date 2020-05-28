## Introduction
Recommender systems is one of most successful machine learning applications. We are exposed to the items/informations recommended by these systems everywhere in our daily life.

This project will introduce several recommender systems in NLP, specifically in the domain of online recruitment. It includes the important ideas behind the recommender systems in our web application, it might be also similar with those in recruiting platform like LinkedIn, Xing.

### Recommender system in general
One of my main jobs is to design and develop recommender systems on the skill match web app. Before going into this specific recommender system, I would like to go over the general concepts and methods of current recommender system in different domains.

Recommender system is everywhere. It is just a machine that introduce the users to products that they might be interested in.

- E-commerce platforms, such as Amazon, recommend their customers various of products based on huge amount of data from millions of customers and products.
- Entertainment providers, such as Youtube, Spotify and Netflix, recommend songs and movies to their users.
- Job platforms, such as LinkedIn, they are making two way recommendation, recommend jobs to job seeker, as well as, recommend profiles to job providers.
- Advertisement, real-estate market, dating market …

Overall, there are two different kinds of recommender system:

- Collaborative filter: recommend items based on user's historical records, the user's explicit interest (like/dislike, rating or reviews) and implicit interest (clicks on an item, bookmarking an item’s URL, time spent on the page)
- Content-based filter: recommend item to user only based on based on the item/user's own content.

#### Collaborative filter
Collaborative are based on the past interactions recorded between users and items in order to make new recommendations. These interactions are stored in the so-called “user-item interactions matrix”.

Each row represent a user and each column is a item, the interaction can be 0/1 (0-no record, 1-like) or rating level (e.g. 0-5), as the following table

|       | item 1 | item 2 | item 3 | item 4 |
| :---: | :---: | :---:| :---: | :---: |
| **user 1**  |  0 |  1 |  1 |  0 |
| **user 2**  |  1 |  0 |  1 |  1 |
| **user 3**  |  1 |  1 |  0 |  1 |
| **user 4**  |  1 |  0 |  1 |  0 |

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

#### Content-based filter
Content based method is simply building model for each user(item) by using the item(user), they have interacted with. It can be either a classification problem (predict like/dislike) or regression problem (predict rating).

1. Item-centered model
Use user's features to predict their preference for each item. For example, a item is voted for 1000 users, train a model these users' profile, then we can predict how a new user will vote for this item.

1. User-centered model
Use the item's features to predict the user's preference for the item. For example, a user has listened 1000 songs, train a model on these song's characters, then we can predict how the user will vote for a new song.

#### Mix approach
As I mentioned above, the deep learning architect can include different information. It can be view as a mix approach between collaborative filter and content-based filter.

Won't go into more details here, feel free to read this article see how youtube implement this architect in their system - [Deep Neural Networks for YouTube Recommendations.](https://static.googleusercontent.com/media/research.google.com/en//pubs/archive/45530.pdf)


**References**
1. [Matrix Factorization Techniques for Recommender Systems](https://datajobs.com/data-science-repo/Recommender-Systems-[Netflix].pdf)
1. [Introduction to recommender systems](https://towardsdatascience.com/introduction-to-recommender-systems-6c66cf15ada)
1. [Deep Neural Networks for YouTube Recommendations](https://static.googleusercontent.com/media/research.google.com/en//pubs/archive/45530.pdf)
