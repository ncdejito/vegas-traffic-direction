# vegas-traffic-direction

Get direction of traffic from live camera feeds

## Approach
Goal: get direction per road

Steps:

1. pair each point to its previous self - assume nearest point is its previous self, get nearest by creating a distance tree of particular point and the tree of points from the previous frame
1. get direction of each point - difference from previous frame
1. get directions for 5 frames - queue for updating points in the 5-frame time window, used to normalize against weird values e.g. car moving uphill on one side and camera is on the other side
    cluster direction vectors with

    * agglomerative clustering - no need to specify expected number of clusters, expected cosine similarity difference can be specified as <0.5), looks fast enough (<0.03s)?
    * cluster difference evaluator = cosine similarity (if 90 degrees from each other, cosine=0; if 180 degrees, cosine is -1)

1. get average of each cluster
1. direction for road is cluster average