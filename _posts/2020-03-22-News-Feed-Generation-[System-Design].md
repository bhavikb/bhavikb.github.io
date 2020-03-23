The home page of social media platforms generates personalized news feed for users. Two of the most important operations of social media platforms are *post update* ("post a tweet" in case of twitter) and *generate timeline*. I decided to know more about this topic because it is a popular system design interview question to design an social media platform like twitter or instagram. I will discuss the scalability of the system and not the algorithm for new feed generation (see [this post](https://techcrunch.com/2016/09/06/ultimate-guide-to-the-news-feed/) from tech crunch to know about the algorithm).

### **Post Update**

In this operation the user posts personal picture or tweet, which eventually is viewed by the user's followers. A simple google search says that there are about 6000 tweets per second on twitter and about 700 instagram posts. We can scale the database to handle 6000 writes but the issue arises after an celebrity user with millions of followers posts an update. This will result in millions of get requests to database.

### **News Feed Generation**

When the user requests the home page, there are 2 approaches in which we can deliver the timeline.

#### 1. Pull model

Posting an update simply puts it into the database. When the user requests for timeline the we check all the accounts the user follows, get the latest tweets and generate timeline. This approach struggles to keep up as lot of the work is done (getting tweets from database and sort it according to their rank) after the user requests the home page.

#### 2. Push model

In this approach we maintain a pre-generated cache of timeline for each user. When an user posts a tweet we update the cache of all the follwers of the user. The get home page requests are now cheap as the timeline is pre-generated in the cache. The problem with this approach is if the users have millions of followers (celebrity users) each post will generate millions of cache writes.

To get the best of both approaches, we can use an hybrid model where we use pull model for updates from celebrity accounts and push approach for other users.

### Reference
Designing Data-Intensive Applications by Martin Kleppmann
