# Node in a Serverless World

---

Who am I? 

- Logan McDonald
- @loganmeetsworld on twitter / github
- Node since last year
- Enjoys: Ops, security, creative problem solving
- Job: Kickstarter (in NYC) Ops & Security 
- NodeJS Experience: Newbie!

--- 

# Roadmap
- What is serverless anyway? Why use it? 
- Where does Node come in? 
- What about deployment, security, performance? 
- Examples

---

## What is serverless?
- Surprise it's not actually serverless!
- Some of the examples I'm going to show today are ways to use a "serverless" model to find servers or initiate the creation of servers, so we are using serverless to create servers, oh the irony.
- We do not have everything running on serverless by any means, we use lots of EC2 instances, ECS infrastructure, etc. 
- Serverless is a movement to abstract users away from servers, infrastructure, low-level configuration, etc.
- It's a way to slowly chip away at the work that your servers are doing
- For example, we were running this instance for our ChatOps infrastructure but slowly realized most of the things we were using that software for: deployments, notifications, AWS alerts

---

## AWS Lambda
- Part of the "serverless" movement -- a "single purpose service"
- Examples of other single purpose services (a huge draw of serverless): 
Amazon S3 for file storage
IAM/authentication for credential management

<!-- 5 min
* actually misnomer, very serverfull
-->

---

## Why serverless? 
- Autoscales automatically
- pay-per-execution
- inherintly a microservice, which we love

---

## AWS Lambda
Simple: You define a function that performs an operation and AWS provisions and schedules the function. 

All about data flow. Data flows in, data flows out. 

For example 
SNS Notifications / S3 uploads -> LAMBDA -> visualization, slack message (through webhook), data sent to an endpoint.
[diagram]


- caveats! Lambda is pretty new, ways of deploying lambda are pretty new, you can't do super long running processes because there is a 5 minute timeout, it's through AWS (lock in), cold start functions after ~4 hours (which is less promblematic for JS than Java)


---

## Why is Node.js uniquely suited for handling these problems? 
( I am well prepared to talk about this as I have been battling a coworker of mine for the past couple months who really wants to write all of our lambdas in Go and really hates Javascript. Why does it take 50 lines of code to write a POST??? )
This is a good question too. Because Kickstarter itself is written in Rails - we have two large Rails apps, and a series of Java microservices. Our frontend has been bounced around a bit but is written in jquery and more recently, React. 

So we really have no connection to Node or allegiance to it. So why write these lambdas in Node? Not Python? Or Java? 

ARGUEMENT #1: 

Core node principles are helpful for functional programming and AWS Lambda 

We could use Python or Go, but we chose node. 
- Single Functionality principle
- Substack patterns - define more here
- Small surface area
- separation of concerns (functions should perform a single job)
- Promises
- simple model for i/o
- we really <3 functional programming and Node makes that easy (well, maybe not easy but doable and great for learning)
- your apps are basically functions!

---

ARGUMENT #2: Writing lambdas can help you write Node.

I was introduced to the two of them at the same time. 

Functional Programming with AWS Lambda, immutability and point-free programming
<!-- 5 min
-->
Dependency injection: instead of using hardcoded dependencies, we can pass the dependencies (mocked out for tests) into our module. This make testing much easier. For example, with our slack functions we don't have to deal with POSTs to the webhook repeatedly. There's no implicit execution of external code. 

Pure functions: no side effects, promise-based, promises help it be reactive to data. 

Incorporates great with AWS lambda's "Push/Pull model" - an event producer directly calls the lambda function and pulls update from data stream to invoke the function

Pure functions allow for the lower complexity of programs, immutability reduces points

Think: we have this tech that is processing the same events over and over (either triggered or scheduled)
A pure function is a function, that, given the same parameters, will always return the same result. 
So this build perfectly into any functional ecosystem. The data comes in, data goes out 

---

Deployment Options & Tools

<!-- 2 min
-->
- single function deployments

* SAM & Gateway API - command line tool written in Nodejs
* Apex & CircleCI
* Serverless (love CF)
- serverless will get you up and started super quick
- show a gif of using that and wuzz with deployment lines in influxdb 

--- 

Testing
<!-- 3 min
-->

--- 

Examples
<!-- 15 min
-->

---

sns-to-slack

---

finder for stack outputs with cloudformation (gathers all the information needed to spin up a new instance)

For example when a resource is created via CloudFormation, it triggers a lambda, which returns it the latest AMI, VPCID, Subnets, and KeyID


```
LambdaVPCSubnetsGetter:
  Type: Custom::Getter
  Properties:
    ServiceToken: '<lamba-function-vpc-finder>'
    Environment: !Ref Environment
    Service: influxdb
```

---

Vault secret store / Consul for secret managment 
Resource creation via CloudFormation -> hits the lambda, through access control lists

---

twitter bot?

---

Summary: It will sharpen your node skills
save you money


---

<!-- Prompt: Node can be a great ally to any ops engineer. This talk will dive into AWS lambdas, a.k.a serverless functions and show how Node can be used to build a small application running on AWS Lambda with the help of the a new deployment model called SAM (AWS Serverless Application Model). We will also discuss testing (with Mocha and Chai) as well as moving towards purely functional javascript and some of its concepts (immutability and point-free programming). -->

---

