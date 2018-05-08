In web development, we need to buy/hire server(s) to setup and host the application as well as maintain, monitor them. This is a fact, which doesn't need to / cannot be argued!

However, in the cloud computing age, this may not be exactly correct anymore with `serverless` term. So, what is `serverless`?


#### What is Serverless?

`Serverless` doesn't mean we don't need any server to let the application runs. It just means we don't need to setup or maintain any server. All things we use in a serverless application are `services`. An example of serverless application which uses AWS services:

<center>
  <img src="assets/images/serverless/serverless_aws.png">
</center>

1. [AWS S3](https://aws.amazon.com/s3/) (too familiar, right?) to host the frontend assets. E.g: React application
2. [AWS DynamoDB](https://aws.amazon.com/dynamodb/) (a managed NoSQL service from AWS) for storing data
3. [AWS Lambda](https://aws.amazon.com/lambda/) to process requests from users. It is an AWS service, which run predefined functions when a condition was triggered
4. [AWS API Gateway](https://aws.amazon.com/api-gateway/) to receive requests from users, trigger AWS Lambda functions and return the result

As you can see, they provided all we need to build an application without any server setup/maintain - `serverless`

#### Why?

Nowsaday, many giants in cloud computing are focusing into their serverless services. To build a serverless application, we need several services (see above simple example) but the most important service is the one that allows us to execute our dynamic server side code (Ruby, NodeJS, Python, etc.) because without them, we won't be able to make our application dynamic. Here are some cloud computing provider's serverless core services:

- AWS with [AWS Lambda](https://aws.amazon.com/lambda/)
- Google Cloud Platform with [Cloud Functions](https://cloud.google.com/functions/)
- Azure (Microsoft's product, if you don't hear about before) with [Azure Functions](https://azure.microsoft.com/en-us/services/functions/)

So, what are the benefits of `serverless` that make it is interesting enough for us to care about? Well, many benefits, here are some of them:

1. Most important benefit: no server setup, no maintenance effort, mostly unlimited scalibility. The cloud services are designed to be scalable themselves. We never need to think about how many servers in load balancer, how strong they are, etc. Just focus to development.
2. Low infrastructure cost. Actually, this depends on each application but in most cases, serverless cost less than traditional model. Why? Because you only pay when the services are used, not pay for idle time
3. It forces us to build the app by microservices architecture. On AWS, each lambda function is indepentdent and acts as a service. IMHO, it is a good thing.

#### Why not?

Nothing is perfect, serverless also has drawbacks, some most important drawbacks:

1. We will be locked to a cloud services provider (e.g: AWS, Google). There are no standard (yet) in serverless services, so if we use AWS and then want to move them to Azure, it is hard and may take much time. We also don't have much choices like the traditional model with hundreds of providers, with serverless, we have only some options
2. We need to manage so much services. Microservices is good, but if we have thousands of them, it may be a nightmare
3. Too complex for normal developers

#### Conclusion

So, should we use it or not? The answer is "depend on...", what things we need to consider?

1. How is the scalibility is needed for the application
2. Cost for server maintainance
3. Level of developers

Beside going completely serverless (which is a bit complex), we can do it partially: move some most-used parts of the application to be serverless and take its advantage. Example: with a blog with millions views per day, we can split it to:

1. List posts and post detail goes to serverless because they are features that will be used millions times per day
2. Write post, manage users, etc. (which is not to much used as #1) still follow traditional model



