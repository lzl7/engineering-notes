Generally, we all agree on that the dictionary's complexity is O(1). However, it is actually the best case even we think that in most case it should.

## What scenarios the complexity become worse?

Dictionary (like in C#) is implemented with the hash data structure which works well by mapping the key into different buckets and have a fast look up there. Different keys might map to the same bucket(s), and then we will need to resolve the key collision problem. There are two factors impact hash data structure's performance: hash function and the collision handling method. The hash function's distribution determines the key's conflict rate while the collision solution impact the conflict performance. So, actually the dictionary (i.e. hash data structure) is not really the O(1) complexity.

## How worse it is?
The collision handling solution will determine how worse it is. For example, the chaining solution uses separate chaining to store the conflict key's value. In the chaining solution, when it has conflict, and it will try to go through the whole chain to do the value comparison to check whether the value already exits and then take the corresponding actions. In this worse case, the performance will degrade to O(n). If you are doing n times look up, the complexity will be O(n^2).

## Attack
In particular case, if the key conflict a lot and it might regress to O(n^2), which also means that their might be a potential security attack point. 

For example, some online service might use the dictionary to store online data for serving, if the attacker make up the perfect dataset (which will have the same hash key) and send to the server, and it will lead to the dictionary regress into O(n^2) instead of O(n) (n is the data set size). If the dataset is large enough, it could slow down the server and make up a DOS attack.

There is an interesting question: how could the attacker create the perfect dataset? If the server use some standard libary or open source codes, the dictionary implementation is already known and it is very easy to create such kind of data.

> TODO: to discuss more about the possible attack cases
> How about adding runtime randomize for the hash function？
> What should we do to avoid the potential attack?