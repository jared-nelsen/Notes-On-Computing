# System Design Primer

11/22/19

https://github.com/donnemartin/system-design-primer

## Scalability

Watch this lecture: https://www.youtube.com/watch?v=-W9F__D3oY4

### Scalability Article

#### Clones

Public servers of a scalable web service are hidden behind a load balancer. A load balancer ***evenly distributes requests from users onto your cluster of application servers***. A user should always get the same results back regardless of what server he landed on. ***Every server should contain the exact same code base*** and does not store any user related data.

***Sessions*** need to be ***stored in a centralized data store*** which is accessible to all application servers. Good choices are external persistent caches like Redis.

#### Databases

Once you have scaled horizontally to accommodate a large number of requests you need to address your database. There are two possible paths:

1. Path 1: Stick with MySQL and keep feeding it resources.
2. Path 2: Go with a scalable NoSQL database and do your joins in application code.

### Caches

Now that you have scaled the databases you have no problem with large data storage. However, now the users might suffer slow requests when a lot of data is fetched. The solution is to implement a cache using a tool like Mamcached or Redis.

The idea is to use the cache as a ***buffering layer***. Whenever your app has to read data it should ***first try to read it from the cache***. The reason it is fast is that it ***holds the dataset in RAM*** instead of on disk.

One technique is to ***cache objects***. Say you have a class with a complex data array that has pictures and text and such. This is traditionally assembled by using functions on the object to get the data from the database. However, once it is assembled you can just cache it for future use.

Some ideas of objects to cache:

1. User sessions
2. Fully rendered pages
3. Activity Streams

### Asynchronism

1. Turn Dynamic Content into Static content - You can run a regular job to go through and generate all possible pages of a web app using the database and cache or store them as static pages. These can then be delivered through a Content Delivery Network or CDN.
2. Queue and Poll - Sometimes there are special requests that come in that can take a long time to compute. A good tool is RabbitMQ Here is how to do this:
   1. Front end sends a job to the job queue
   2. Font end signals user that process is in wait
   3. Job Queue is constantly checked by multiple workers
   4. First available worker takes job, performs it, and send signal that job is done
   5. Front end is constantly checking if the job is done
   6. When job is done front end gets the signal and loads it back up

## Performance vs Scalability

A service is ***scalable*** if it results in ***increased performance proportional to the amount of resources added.*** An ***always on*** service is said to be scalable if adding resources to ***facilitate redundancy*** does ***not*** result in a ***loss of performance or service.***

1. If you have a ***performance problem*** then your system is slow for a single user.
2. If you have a ***scalability*** problem then your system is fast for a single user but slow under heavy load.

## Latency vs Throughput

***Latency*** is the time it takes to perform some action.

***Throughput*** is the number of such actions or results per unit of time.

***Aim for maximal throughput with acceptable latency***

## CAP Theorum

In a distributed system you can only support two of the following guarantees:

1. Consistency - Every read receives the most recent write or an error
2. Availability - Every request receives a response without guarantee that it contains the most recent information.
3. Partition Tolerance - The system continues to operate despite arbitrary partitioning due to network failures.

***Networks are not reliable so you need to support partition tolerance!*** Therefore you will need to make a tradeoff between consistency and availability.

