# System Design: Social network for travelers

## Functional requirements

* create a post with photos, description and coordinates 
* comment on a post
* like a post
* subscribe on a user
* notify about a new post in subscriptions
* search posts by zones on map
* view user posts

## Non-functional requirements 

#### Given by business
* DAU = 10m
* geo distribution is not needed(only CIS)
* no seasonality

#### According to Instagram(average amount per day):
**Reactions** = 50 \
**RPS** = 10_000_000 * 50 / 86_400 = 5700

**Comments** = 5 \
Max text size = 1024 \
**RPS** = 10_000_000 * 5 / 86_400 = 570 \
**Traffic** = 1KB * 570rps = 570KB/s

**Posts** = 1 \
Photos per post = 3 \
Max text size = 2048 \
Max photo size = 1MB \
**RPS** = 10_000_000 * 1 / 86_400 = 115 \
**Traffic** = (2KB + 1024KB * 3) * 115 = 350MB/s 

**RPS on write** = 5700 + 570 + 115 = 6385 \
**Traffic on write** = 351MB/s

**Requests to read posts**(10 post by request) = 10 \
**RPS on read** = 10_000_000 * 10 / 86_400 = 1150 \
Average post = (1024KB * 3 + 2KB) = 3074KB
Average request = 3074KB * 10 = 30740KB
**Traffic on read** = 1150 * 30740KB = 35TB/s

