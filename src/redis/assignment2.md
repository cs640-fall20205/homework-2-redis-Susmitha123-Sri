# Assignment 2


## Getting Started:
  * Read the README.md file in the redis directory
  * You do not need the redis-server or redis-cli commands as described in your text.
  * Simply start with ./up.bash and ./shell.bash, this will put you into the Redis prompt.

## Day 1
1. (10 points) Read Day 1. Skip section on Unions on page 270. Work through the examples in the text. Save your database to dump.rdb (see README.md).

2. (1 point) What is the URL to Redis' documentation on its commands:
```
http://redis.io/
```

3. (1 point) What is the runtime complexity (in Big-O notation) for LREM?
```
O(N+M)
N = the length of the list
M = the number of elements removed
```

4. (2 points) Create a structure to hold the value of your address with the key of your first name. For instance, mine would be “heidi”  “CS & IT Department, Herman Hall 207C, Western New England University, Springfield, MA, 01119”. Use the proper Redis command to show that the data has correctly been entered and followed by the results. Show both commands below including the prompts:
```
127.0.0.1:6379> SET heidi "CS & IT Department, Herman Hall 207C, Western New England University, Springfield, MA, 01119"
OK
127.0.0.1:6379> GET heidi
"CS & IT Department, Herman Hall 207C, Western New England University, Springfield, MA, 01119"

```

5. (2 points) Create a hash structure to hold your WNE user name, first name, last name, and password. Of course provide a fake password. For instance, my data would be: “he302979”, “heidi”, “ellis”, “xxxx”.  Use the proper Redis command to show that the data has correctly been entered followed by the results. Show both commands below including the command prompts:

127.0.0.1:6379> HSET he302979 username "he302979" firstname "heidi" lastname "ellis" password "xxxx"
(integer) 4

127.0.0.1:6379> HGETALL he302979
1) "username"
2) "he302979"
3) "firstname"
4) "heidi"
5) "lastname"
6) "ellis"
7) "password"
8) "xxxx"

```

6. (2 points) Create a hash that holds your father’s and mother’s names as separate key-value pairs. Use the proper Redis commands to show that the data has correctly been entered followed by the results. Show both commands below including the command prompt:
127.0.0.1:6379> HSET parents father "Sridhar" mother "Padma"
(integer) 2

127.0.0.1:6379> HGETALL parents
1) "father"
2) "Sridhar"
3) "mother"
4) "Padma"
```

7. (3 points) Use a list to:
    * Create a stack called ‘my_stack’
    * Add elements for pears, apples, peaches
    * Remove the top element
    * Display the list to check that you have removed the proper elements
    * Add oranges to the stack
    * Display the list to check that you have removed the proper elements

Show the commands below including the command prompts:
127.0.0.1:6379> LPUSH my_stack "pears" "apples" "peaches"
(integer) 3

127.0.0.1:6379> LPOP my_stack
"peaches"

127.0.0.1:6379> LRANGE my_stack 0 -1
1) "apples"
2) "pears"

127.0.0.1:6379> LPUSH my_stack "oranges"
(integer) 3

127.0.0.1:6379> LRANGE my_stack 0 -1
1) "oranges"
2) "apples"
3) "pears"

LPUSH adds elements onto the stack (left side of the list).

LPOP removes the top element ("peaches").

LRANGE my_stack 0 -1 shows the full stack after each operation.

Finally, LPUSH adds "oranges" on top, and the next LRANGE confirms the correct order.

```

8.  (3 points)  In class we talked about the problem with name changes. For instance. Jane Ellen Doe might change her name to Jane Ellen Doe Fitzgerald where Ellen Doe becomes Jane’s middle name. Do the following:
    * Create hashes to hold first name of "Jane", middle name of "Ellen" and last names of "Doe".
    * Use Redis’ **transactions** to modify Jane to have a middle name of “Ellen Doe” and a last name of “Fitzgerald”. Remember, we need to ensure atomicity for this action.

Write the commands below including the command prompts:
127.0.0.1:6379> HSET first_name jane "Jane"
(integer) 1
127.0.0.1:6379> HSET middle_name jane "Ellen"
(integer) 1
127.0.0.1:6379> HSET last_name jane "Doe"
(integer) 1

127.0.0.1:6379> MULTI
OK
127.0.0.1:6379> HSET middle_name jane "Ellen Doe"
QUEUED
127.0.0.1:6379> HSET last_name jane "Fitzgerald"
QUEUED
127.0.0.1:6379> EXEC
1) (integer) 1
2) (integer) 1

127.0.0.1:6379> HGET middle_name jane
"Ellen Doe"
127.0.0.1:6379> HGET last_name jane
"Fitzgerald"


```

9. (2 points) Create a sorted set that holds the costs of the following items:  Cheetos: $2.99, apple: $1.00, Hershey’s: $3.45, and Coke: $1.79.  Print the set in ascending order.  Reduce the price of Hersheys by $1.00 using the ZINCRBY command. Print the set again in ascending order. Show the Redis commands below including the command prompts:
127.0.0.1:6379> ZADD store 2.99 "Cheetos" 1.00 "apple" 3.45 "Hersheys" 1.79 "Coke"
(integer) 4

127.0.0.1:6379> ZRANGE store 0 -1 WITHSCORES
1) "apple"
2) "1"
3) "Coke"
4) "1.79"
5) "Cheetos"
6) "2.99"
7) "Hersheys"
8) "3.45"

127.0.0.1:6379> ZINCRBY store -1.00 "Hersheys"
"2.45"

127.0.0.1:6379> ZRANGE store 0 -1 WITHSCORES
1) "apple"
2) "1"
3) "Coke"
4) "1.79"
5) "Hersheys"
6) "2.45"
7) "Cheetos"
8) "2.99"


```

10. (2 points) When using 2-factor authentication, many web sites send the user a link to confirm their identity. These links typically expire within 5 minutes. Reproduce this concept in Redis by storing a key named 'link' whose value is a URL (your choice), that expires in 5 minutes. Do it in a single command. Check to ensure that the link exists. Write the commands below including the command prompts:
127.0.0.1:6379> SET link "https://example.com/verify/12345" EX 300
OK

127.0.0.1:6379> EXISTS link
(integer) 1

SET link "…" EX 300 stores the URL with an expiration of 300 seconds (5 minutes) in a single command.

EXISTS link checks if the key still exists (returns 1 if yes, 0 if expired or missing).

```

## Day 2

1. Read the following sections in Day 2. You do not have to "do" them. Just read and study them and know them for the exam. There is nothing to turn in for Day 2.

* Publish-subscribe
* Durability
* Snapshotting
* Append-Only File
* Security
* Master-Slave Replication

## Day 3 - Skip

Submission
1. The following files should contain all your work for this assignment:
    * assignment2.md
    * dump.rdb

 * All work must be committed to your individual GitHub repo to be considered submitted.
 * Confirm your submission by opening your repository in GitHub.
