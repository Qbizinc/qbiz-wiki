---
title: Lyft Interview 2021-06-18 (Henry)
description: 
published: true
date: 2021-07-30T00:50:45.834Z
tags: interview, business-requirements, lyft
editor: markdown
dateCreated: 2021-07-30T00:50:45.834Z
---

Interviewer: Andrew Lewis

Duration: 50 minutes

Background: Andrew relayed that the position he was looking to fill would be working on consolidating Lyft’s analytics tables into their ‘core concepts’ framework, as well as working with key stakeholders to build out new metrics and processes to serve them up. Requirements gathering was a large portion of the interview. Most questions revolved around me asking HIM questions to elucidate the parameters of the problem.


### Questions

**Given what you know about Lyft: what metrics are important to you as the rider?**

1. Time to pick up
1. Time to destination
1. Price
1. Safety
1. Cleanliness/professionalism of the driver

**Let’s say we’re interested in time to pick up: what data would you collect to effectively monitor that metric? And, how would you logically store it? Can you create an ERD for our model?**

1. He specifically asked if I knew what ERD stood for 
-- Entity relationship diagram
1. I created quick and dirty table models in google sheets:
-- I clarified that rider_id and driver_id were foreign keys to the users table
-- I asked what kind of events would be in the event_type enum: (ride_requested, ride_accepted, passenger_picked_up, passenger_dropped_off)

| users |
| --- |
| id | int |
| address | varchar |
| is_driver |bool |


| rides |
| --- |
| id | int |
| rider_id | int |
| driver_id | int |
| location_lat | float |
| location_long | float |
| event_type | enum |  |
| event_ts | timestamp |

**What SQL would you write to answer the question: how long does it take on average for our passengers in seattle to be picked up on any given day?**
* I asked if there was a more human-readable format to our geo-location data
* There was. We amended the rides table to be:

| rides |
| --- |
| id | int |
| rider_id | int |
| driver_id | int |
| region | varchar |
| event_type | enum |  |
| event_ts | timestamp |


```
WITH step1 AS (
 SELECT id
      , MAX(CASE WHEN event_type = 'ride_requested' THEN event_ts) AS ride_requested
      , MAX(CASE WHEN event_type = 'passenger_picked_up' THEN event_ts) AS pick_up
FROM rides
WHERE LOWER(region) = 'seattle'
GROUP BY 1
)

SELECT ride_requested::DATE AS day
     , AVG(DATEDIFF(seconds, pick_up, ride_requested)) AS avg_time_to_pickup
FROM step1
GROUP BY 1
```

**What under-the-hood performance improvements would you make to improve the performance of this query, assuming that we’ll be running it frequently.**

1. I wasn’t sure what he was trying to get at and talked about asking stakeholders if we could apply some filters to a narrower time-frame
1. It ended up being a discussion about how to partition the table on a cluster. I said we should partition it by day so our aggregate AVG function would have all the necessary data in each partition to return the results as quickly as possible
1. We then amended the rides table again to denote the partition strategy (with ds being the partition stored as a VARCHAR date string)

| rides |
| --- |
| id | int |
| rider_id | int |
| driver_id | int |
| region | varchar |
| event_type | enum |  |
| event_ts | timestamp |
| ds | varchar |

**How would you handle partitioning in cases where the beginning of the user’s session was just before midnight and the driver didn’t perform the pick up until after midnight?**
1. I said that I would talk with the stakeholders on how they would want to handle this, while stating my opinion that the first event in the session should be the demarcation point for the entire session.
