# System Design: Social network for travelers

## Functional requirements

* create a post with photos, description and coordinates 
* comment on a post
* like a post
* subscribe on a user
* search posts by zones on map
* view user posts

## Non-functional requirements 

### Given by business
* DAU = 10m
* geo distribution is not needed(only CIS)
* seasonality: increased load in summer and around 31st december

### Activities

**1. Comments** \
Write: 5 comments per day \
Read: 50 comments per day \
Data:
- text = 1024B
- post_id = 4B
- creator_id = 4B

**RPS(write)** = 10_000_000 * 5 / 86_400 = 570r/s \
**Traffic(write)** = 1032B * 570rps = 0.5MB/s \
**RPS(read)** = 10_000_000 * 50 / 86_400 = 5700r/s \
**Traffic(read)** = 1032B * 5700rps = 5.6MB/s

**2. Posts** \
Write: 1 post per day \
Read: 100 posts per day \
Data:
- photo(x3) = 3MB
- text = 2048B
- creator_id = 4B

**RPS(write)** = 10_000_000 * 1 / 86_400 = 115r/s \
**Traffic(write)** = (2KB * 3072KB) * 115 = 345MB/s \
**RPS(read)** = 10_000_000 * 100 / 86_400 = 11500r/s \
**Traffic(read)** = (2KB * 3072KB) * 11500rps = 33.7GB/s

**3. Reactions** \
Write: 50 reactions per day \
Read: 11500 (same as posts, get as counted via cache with posts) \
Data:
- post_id = 4B
- type(like/dislike) = 4B
- actor_id = 4B

**RPS(write)** = 10_000_000 * 50 / 86_400 = 5700r/s \
**Traffic(write)** = 12B * 5700rps = 67KB/s

## Total

**RPS(write)** = 570 + 115 + 5700 = 6300r/s \
**Traffic(write)** = 0.5MB/s + 345MB/s + 67KB/s = 350MB/s \
**RPS(read)** = 5700 + 11500 =17200r/s \
**Traffic(read)** = 5.6MB/s + 33.7GB/s = 34GB/s
