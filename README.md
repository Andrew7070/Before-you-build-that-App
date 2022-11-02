# Before-you-build-that-App
Guidelines to observe while designing a new App 

Before you build that App
EM
	

Evans Mbithi Ikindu

Posted in Google Cloud Platform - GADS 2022
Consider these best practices

Code repository - store your app's code in a repository (Git/Subversion) to track changes in source code and CI/CD.
Dependency management - DO NOT store external dependencies such as jar files or external packages in your code repo. Explicitly declare your dependencies with their versions and install them using a dependency manager. e.g Package.json for nodejs apps.
Configuration settings - Separate your application's configuration settings from your code. DO NOT store config settings as constants in your source code. Instead specify the settings as ENVIRONMENT VARIABLES. This enables you to easily modify settings between development, testing and production phases.
Microservices over monolithic - In a monolithic application, the codebase becomes bloated overtime and can be difficult to determine where code needs to be changed. With all components such as UI, Payment, Orders e.t.c tangled together, the entire application needs to be tested and deployed even if the change is made to a small part of the codebase. 
Microservices enable you to structure application components in relation to business boundaries. The UI, Payment, Orders are all broken up into individual services. The codebase is modular and easy to determine where code needs to be changed. Each service can be updated and deployed independently without requiring consumers to change simultaneously.
Perform asynchronous operations - remote operations can have unpredictable response times and can make your application seem slow. Keep the operations in the user thread at a minimum. Perform backend operations asynchronously. Use event-driven processing where possible, e.g if your app processes images that are uploaded by a user, you can use a google cloud storage bucket to store the uploaded images. Then call gcloud functions that are triggered whenever a new image is uploaded. Cloud functions can process the image then store the results to a different cloud storage location.
Design for loose coupling - Design application components so that they're loosely coupled at runtime. Tightly coupled components can make the application less resilient to failures, spikes in traffic and changes to services.
An intermediate component such as a message queue can be used to implement loose coupling, perform asynchronous processing and buffer requests in case of spikes in traffic. You can use a cloud Pub/Subtopic as a message queue.
Publishers can publish messages to the topic and subscribers can subscribe to messages from this topic.
Implement stateless components for scalability - implement app components so that they don't store state internally or access a shared state. Workers perform compute tasks without sharing state. Workers can scale up and down reliably.
If your app needs to process streaming data from IoT devices, you can use a cloud Pub/Sub topic to receive the data, then implement cloud functions that are triggered whenever a new piece of data comes in.
Cloud functions can process, transform and store the data. Alternatively, your app can subscribe to the Pub/Subtopic that receive the streaming data. Multiple instances of your app can spin up and process the messages in the topic and split the workload. These instances can automatically be shut down when there are very few messages to process.
 
 Use any compute environment to enable elastic scaling such as compute engine with cloud load balancing, GKE, or app engine so that your application can scale automatically without having to develop code to manage concurrency or scaling.
 
Cache app data (Memcached, Reddis)- caching content can improve application performance and lower network latency. Cache app data that is frequently accessed/computationally intensive to calculate each day. When a user requests data, the application component should check the cache first. If data exists in the cache (TTL hasn't expired) the app should return the previously cached data (Cache hit). If there's a cache miss (TTL has expired/data does not exist in the cache) the app should retrieve the data from backend sources and update cache with the new value.
Cache frontend content - you can also use a content delivery network to cache web pages.
Cloud CDN can cache load-balanced frontend content that comes from compute engine, VM instance groups or static content served from cloud storage.
API gateway to make backend functionality available to consumer applications - you can use cloud endpoints to develop, deploy, protect and monitor APIs based on the openAPI specification/GRPC. The API for your app can run on backend such as app engine, GKE or app engine.
If you have legacy applications that cannot be moved to the cloud, consider using API as a façade/adapter layer. Each consumer can then invoke these modern APIs to retrieve information from the backend instead of implementing functionality to communicate using outdated protocols and disperate interfaces.
Use federated identity management - Delegate user authentication to external identity providers such as google, facebook etc. this will minimize your effort for user administration.
Implement a health-check endpoint for each service. The endpoint handler should check the health of all dependencies and infrastructure components required for the service to function properly. It can check the performance and availability of storage, database and network connections required by the service. Thus returns a HTTP response 200 or 500.
 
Treat your logs as event streams. Don't manage logs in your app, instead, write to an event stream such standard out and let the underlying infrastructure collate all events for later analysis and storage. 
Stackdriver is Google's operational suite. You can debug your app, setup error monitoring, logs-based metrics etc.
