# DNS Server

cs4700 project 6

zone file

- after researxhing about zoning files we decided to keep it in a dict and have name and type be the key so that we could check against that everytime

main logic

- for the main logic we wanted to rebuild the recv to have selective logic for recieving question queries.
- specifically if the record was in our zone or not or if we had to send a query up stream to find its authoritative server.
- this was done by our is authoritative method our authoritative method could also handle name server and CNAMe records which is something we had to implement later on to get test cases to work

recursive lookup logic

- our recursive lookup logic changed with bailwick checking and when we added cacheing to pass level 18 test but overall it just follows a domain down until it finds its autoritative server with a record and gets those answers.
- the issue with this which is where we saw in problem 14 was sending queries in parallel that may go deep recursivley and may come at the same time

Challenges

- we had challenges with implementing our level 14 tests which we initially thought up of an idea to have some store of queries whose resposes were pending (an idea we found on piazza) , however we didn't decide to pursue this because me and my partner came up with something we thought could work easier being implementing threads where all we did waa just copy and paste our recv logic into another method and build a thread and start the thread for every call of that method.

- the challenges we had here had alot to do with the recursive lookup method where tests that took alot of parrallel requests that utilized the resursive lookup more and more to find deeper queries would sometimes crash with network delay.

- we did everything we could by trying to log and use try catches to assist with logging but it turns out that many of the issues we had happened to do with the network we were running.

- for example when I initially was running parallel requests on a coffee shops wifi my test 14 would pass every single time without fail, however when my partner pulled the code and ran on school wifi the tests would fail
- i confirmed this again by trying between my school wifi and my hotspot in which we found differences in the network delay and maybe even some throttle effected the outcome

Bailiwick Checking approach:

- After reviewing the slides on bailiwick checking and doing some research:
  https://www.domaintools.com/blog/what-is-a-bailiwick
- decided that I should be looking checking name / zone within the recursion look up
- added helper function that would first check if both the name and zone ended in a . and add one if
  they didn't already have one (per the instructions)
- then to check needed to check if all the answer name ended with the zone, so used endswith function
  and it if it didn't then it should be throw out
- also updated the recursion arguments to include a zone so when we go down the ns chain
  the first part of the function can easily read the zone and filter out answers

Challenges:

- figuring out how to extract the zone and name from the answers

Caching Approach:

- Decided to go with a dict approach so it would be easy to lookup if a record was already in dict
- followed the slides on when the cache is called: before lookup and when answer is found

- Before lookup: check if the record is in cache already and if it is need to check all the ttls
- after all ttl are updated (if they are expired remove them) then need to send back the new record from
  cache with updated value

- If the record wasn't found in the cache then proceed with the normal recursive lookup then when an answer
  is found add to cache if not already in there

Challenges:

- not realizing that I had to seperate the NS answers from the glue record, becuase originally was trying to
  put all in one dict entry and that caused tests not to pass
