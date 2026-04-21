# DNS Server

cs4700 project 6

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
