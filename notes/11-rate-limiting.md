# Rate-Limiting

It a system for maintaining users request to apis.

## requirements

1. configuration Rules: Rules like 100req/minute/user
2. HTTP 429 +Helful Headers lke ratelimit-remaining,Reset
3. minimal latency p35 <3ms
4. highly available

### implement

a window of 100 request per minute per user.it maintain a counter for every user.

but if we have many server then it will increase req for user that many times. we can maintain redis for counter for many servers.

#### Algorithams

1. Token bucket algoritham
    it mantain a bucket which can have mimites tokens and refiled that may limits per minute.
    like for 100 req/min/user it have bucket of 100 tokens and refile at rate of 100 req per min.

    user takes token from this bucket by user for every request if bucket is empty then user reqest rejected with 429 status code.

2. siding window logs
3. sliding window counters
4. leay bucket algoritham
    there is a bucket with one hole. it puts all requests in bucket then response only some amount requests.

#### location of rate limiter

1. frontend
2. backend
3. middleware separated from both
