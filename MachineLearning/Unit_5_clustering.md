---
layout: minimal
title: "Clustering"
---

<br>

# Wiki Activiy: Clustering

<br>

## Task

For this task we viewed and compared two differing animations illustrating the K-means clustering algorithm.  Having investigated both animations, observations and reflections were recorded in the module Wiki.  A key reflection gained from this activity was the importance of selecting an appropriate machine learning algorithm for the underlying dataset.  Use of clustering methods or other machine learning algorithms without appropriate understanding of the characteristics of the underlying data or the algorithm's assumptions and workflow, could produce misleading results. Depending on final application, this could have significant business ond ethical implications.  

<br>

### Wiki Entry 

Both animations of K-means clustering highlight the iterative process of centroid positioning, assigning each data point to the closest centroid and recalculating the new centroid location based on the mean of all the points in the current cluster

Animation 1 (https://shabal.in/visuals/kmeans/6.html) contained data points that contain visible patterns i.e. there was non-uniform spread between points.   Different initial centroid positions resulted in noticeable differences in the final cluster sizes and groupings, with one example resulting in three clusters and a lone centroid not associated with other data points.

Visualisation 2 https://www.naftaliharris.com/blog/visualizing-k-means-clustering/  included the Voroni diagram which partitions the space into areas to illustrate which centroids the points fall closest to.   The points in this animation were uniformly spread.  In this case I found that self-selection of 4 initial centroids resulted in final clusters that were similar, with one in each quarter of the overall space (although the observation is based on a limited selection of initial centroid options). 

Whilst the second visualisation appeared to be more consistent in terms of identifying similar clusters, this result may be less meaningful as the data is uniformly spread and clusters may not truly exist within the data.   As also highlighted by colleagues in the their posts, inappropriate utilisation of clustering to inform decision without sufficient the understanding of underlying methods and assumptions could introduce bias, risking inappropriate, misleading and ethically unsound outcomes. 

<br




<hr>

<a href="https://sjackson-ds25.github.io/MachineLearning/Landing%20page.html" style="display:inline-block; padding:8px 12px; background-color:#0366d6; color:white; text-decoration:none; border-radius:4px; margin-bottom:1em;">⬅️ Return to Machine Learning</a>

<hr>



