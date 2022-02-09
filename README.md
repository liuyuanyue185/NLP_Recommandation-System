# NLP_Recommandation-System

Data Source:
We obtained the data on Amazon product reviews from https://jmcauley.ucsd.edu /data/amazon/. Considering that the dataset is pretty large for all the departments, we only focused on the product reviews in the luxury beauty category, covering years 1996-2018. Basically, we have the information on product ID, reviewer ID, text of the review, and rating of the product.

Collaborative filtering recommender system:
The major logic is: input customers’ rating and then let the algorithm find out the best laten feature features to match those rating, finally compute and create the new rating for non-rating products for each customers.

Firstly, I used userid, productid and rating to build a simple recommendation system

Improved Recommendation System:
Considering the laten feature is generated from the inputted users. There is my first hypothesis, the similar users, the more precise laten space, the more correct recommendation. So I decided to clustering these users based on their comments patterns.
First, I used Bert to tokenize the reviews. Then I feed them into three clustering algorithms (GaussianMixture, hierarchy, kmeans) respectively and find out the best number of clustering for each algorithm. Finally, silhouette was decided by each algorithm under its best number of clustering. And the result shows the performance of em(GaussianMixture) is best, when the number of clustering is 3. The silhouette value is a measure of how similar an object is to its own cluster (cohesion) compared to other clusters, which is between -1 and 1 and the higher the better.
 
So I feed the clustering data and non_clustering respectively to four recommendation algorithms(SVD, NMF, NormalPredictor, KNNBasic), calculate their lowest rmse respectively, and let the system record the best algorithm for each clustering. And take the average rmse for clustering dataset. 
 
Bad Performance Analysis:
The above result shows the performance from clustering dataset is worse than non_clustering dataset, which seems unreasonable.  Especially for clustering0:
 
So I make some hypothesis to find the reasons like below:
 
The first hypothesis is sentiment distribution in clustering0 is unique from clustering 1 and clustering2. However, the figure shows the distribution of clustering is similar to clustering1 and clustering2, so there is no correlation; The second hypothesis is the liar distribution is unique in clustering0, however the figure shows the percentage of liars are between clustering1’s and clustering2’s. That is to say, there is no correlation between liars’ distribution and clustering0’s bad performance. The third hypothesis is the low silhouette, which may cause wrong clustering. So that I increased it from 0.175 to 0.227 by cleaning the review – dropping punctuations, stopwords and doing Lemmatisation. The figure shows the performance of clustering is a bit better than nonclustering dataset, which shows I am on the right direction.
Improved Recommendation System Again:
I do the word frequency analysis for nonclustering dataset, clustering0 dataset (worst performance) and clustering2 dataset(better performance), in order to dig more what is inside.
   
The above result shocked me. It is really hard for the system to do clustering when 70% words from clustering0 in top10 high frequency list are same as the nonclustering dataset. And the below figure shows the up to 70% comments are positvie. It is hard to distinct users by this bias dataset.
 
So I excluded all the reviews with highest rate(rate=5), and the sentiment distribution become more balance:
 
And feed the system with the new dataset. The highest silhouette is still from em(GuassionMixture), but now, the best number of clustering is 2.
 
Conclusion(Collaborative filtering system):
Performance for most algorithms is increased as the below figure, the lower difference, the lower rmse_clustering_avg, the best performance.
 
And comparing to the performance(nonclusering_SVD) from original dataset and basic recommendation system, the rmse of the second improved system is decreased from 0.805 to 0.787(clustering_NMF), which also prove my first hypothesis, the similar users, the precise laten features and better performance.  So my system will cluster users and then use NMF to do recommendation for users.
