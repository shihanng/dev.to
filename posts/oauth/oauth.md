When building a consumer web service, it is very likely that we need to have some kind of way to let our user sign-up, sign-in, and manage access control to our application. A modern approach of implementing authentication and managing user identities is using the **OAuth** standard where we verify the user's identity using third-party services such as Google, Facebook, etc. While with many open source libraries available now days, it is not difficult to roll out one by ourselves, but it is also trivial to do so especially when you have a microservice architeture where you need to protect most of your services endpoint from unknown users.

In this post, we will explore a way to do **OAuth** for microservice without having to write the code for **OAuth**. The ??? is that we should only need to where the important business logic for our service without the need to worry about authentication.


The service that we want to protect is called `protected`. It serves under port `8080` inside our mini Docker cluster.

```
$ curl 'localhost:8080/protected/test'
Hi there, I love test!% 
```
