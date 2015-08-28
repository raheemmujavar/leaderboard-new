Leaderboard
-----------

Leaderboard backed by [Redis](http://redis.io) in Node.js. 

[![Build Status](https://www.npmjs.com/package/leaderboard-new)](https://www.npmjs.com/package/leaderboard-new)

Installation
------------

    $ npm install leaderboard-new

API
---

#Constructor

  var lb = new Leaderboard('name', [options], [redisOptions|redisClient])

Creates a new leaderboard or attaches to an existing leaderboard.

###Options

  - `pageSize` - default: `0`

    Page size to be used when paging through the leaderboard.

  - `reverse` - default: `false`

    If `true` various methods will return results in lowest-to-highest order.

##Methods

  - `add(member, score, [λ])`

    Ranks a member in the leaderboard.

        lb.add('raheem', 100, function(err) {
          // no arguments except err
        });

  - `incr(member, score, [λ])`

    Increments the score of a member by provided value and ranks it in the leaderboard. Decrements if negative.

        lb.incr('raheem', 2, function(err) {
          // no arguments except err
        });
    now the score to the member raheem would be 102.

  - `rank(member, λ)`

    Retrieves the rank for a member in the leaderboard.

        lb.rank('raheem', function(err, rank) {
          // rank - current position, -1 if a member doesn't
          // fall within the leaderboard
        });

  - `score(member, λ)`

    Retrieves the score for a member in the leaderboard.

        lb.score('raheem', function(err, score) {
          // score - current score, -1 if a member doesn't
          // fall within the leaderboard
        });

  - `list([page], λ)`

    Retrieves a page of leaders from the leaderboard.

        lb.list(function(err, list) {
          // list - list of leaders are ordered from
          // the highest to the lowest score
          // [
          //   {member: 'member1', score: 30},
          //   {member: 'member2', score: 20},
          //   {member: 'member3', score: 10}
          // ]
        });

  - `at(rank, λ)`

    Retrieves a member on the spicified ranks.

        lb.at(2, function(err, member) {
          // member - member at the specified rank i.e who has 2nd rank,
          // null if a member is not found
          // {
          //   member: 'member1',
          //   score: 30
          // }
        });

  - `rm(member, [λ])`

    Removes a member from the leaderboard.

        lb.rm('raheem', function(err, removed) {
          // removed - false in case the removing member 
          // doesn't exist in the leaderboard.
          // true - successful remove
        });

  - `total([λ])`

    Retrieves the total number of members in the leaderboard.

        lb.total(function(err, number) {
          // captain obvious
        });

  - `rangevalue(min, max, options, [λ])`

    Returns the number of elements in the sorted set at key with a score between min and max.

        var options = {
          a:"exclusive",
          b:"inclusive"
        }

        lb.rangevalue(90,110,options,function(err,number){
            // returns a number -- how many records in between score 91 (here 90 exclusive) to 110
            // returns 0 if no records found
        })

  - `rangeByScoreInfo(min, max, options, [λ])`

    Returns all the elements in the sorted set at key with a score between min and max.

        var options = {
          a:"exclusive",
          b:"inclusive"
        }

        lb.rangeByScoreInfo(90,110,options,function(err,number){
            // returns all records in between score 91 (here 90 exclusive) to 110
            // returns null if no records found
        })

   - `removeRangeByRank(min, max, [λ])`

          Removes all elements in the sorted set stored at key with rank between min rank and max rank. min rank starts from 0.

          lb.removeRangeByRank(0,2,function(err,number){
              // returns number of records removed if true
              // returns 0 if no records removed
          })

   - `removeRangeByScore(min, max, options, [λ])`

        Removes all elements in the sorted set stored at key with a score between min and max.

          var options = {
            a:"exclusive",
            b:"inclusive"
          }

          lb.removeRangeByScore(90,110,options,function(err,number){
              // remove all records in between score 91 (here 90 exclusive) to 110
              // returns null if no records found
          })

   - `getScoreByPattern(pattern, [λ])`

          scan in the sorted set for matching pattern and retrieve all matching records.

          var pattern = "rah"

          lb.getScoreByPattern(pattern,function(err,number){
               //return matching elements in array
               //[{member : "raheem",score:100},{member:"raheja",score:110}]
               //return empty array if no records match

          })



## License 

[MIT](http://en.wikipedia.org/wiki/MIT_License#License_terms). Copyright (c) 2015 raheemmujavar &lt;raheemmujavar@gmail.com&gt;

#### Author: [raheemmujavar](https://github.com/raheemmujavar/leaderboard-new)
