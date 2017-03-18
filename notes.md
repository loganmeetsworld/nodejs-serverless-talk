# Node.js in a Serverless World

<!-- Pitch: Node can be a great ally to any ops engineer. This talk will dive into AWS lambdas, a.k.a serverless functions and show how Node can be used to build a small application running on AWS Lambda with the help of the a new deployment model called SAM (AWS Serverless Application Model). We will also discuss testing (with Mocha and Chai) as well as moving towards purely functional javascript and some of its concepts (immutability and point-free programming). -->

---

# Who am I? 

- Logan McDonald
- @loganmeetsworld on twitter / github
- Job: Kickstarter (in NYC) Ops & Security 
  - Ops, security, creative problem solving
- NodeJS Experience: Newbie!

<!-- 5 min
 * I want to talk about my experiences learning Node and using serverless, how it made me a better developer and why I think the two are great together. 
-->

--- 

# Roadmap

## Background on Serverless  
--
## Node 💖 Serverless
--
## Deployment & Performance
--
## Examples

---

# "Serverless"

<!-- 5 min
  * Surprise it's not actually serverless!
  * I didn't come up with this word, nor do I particularly like it.
--> 

--- 

# FaaS
## App === Bunch of Functions

<!-- 5 min
  * When I say Serverless, I'm talking about two things. 
  * The first is Functions as a Service. You only pay for the time your function runs and each function has a single purpose. 
  * An App === A bunch of functions
--> 

---

## _Serverless is an abstraction movement._

<!-- 5 min
  * The second thing I mean when I say serverless.
  * Serverless is a movement to abstract users away from servers, infrastructure, low-level configuration, etc.
  * Some of the examples I'm going to show today are ways to use a "serverless" model to find servers or initiate the creation of servers, so we are using serverless to create servers, oh the irony.
  * We do not have everything running on serverless by any means, we use lots of EC2 instances, ECS infrastructure, etc. 
  * It's a way to slowly chip away at the work that your servers are doing and a very valuable tool to have in your toolbelt, but I'm not making the argument for #NoOps or anything
  * For example, we were running this instance for our ChatOps infrastructure but slowly realized most of the things we were using that software for: deployments, notifications, AWS alerts (more about that later)
--> 

---

## Making the Argument for Serverless

---

<!-- 
  THEM
--> 

## autoscales automatically
--
## pay-per-execution
--
## inherintly a microservice
--

---

<!-- 
  ME
--> 

## fits with node
--
## functional programming
--
## devops compatible 

---

# Caveats! 
- Lambda is new
- Lambda deployment is new 
- 5 minute timeout
- AWS/Google/IBM lock in
- Cold start functions after ~4 hours

---

# AWS Lambda Functions

<!-- 
  * What I'm talking about today - what I have experience in, but a lot of the major services are very similar
  * Part of the "serverless" movement - a "single purpose service"
  * Examples of other single purpose services (a huge draw of serverless): 
    * Amazon S3 for file storage
    * IAM/authentication for credential management
--> 

<!-- 
  * Simple setup on AWS: You define a function that performs an operation and AWS provisions and schedules the function. 
  * All about data flow. Data flows in, data flows out. 
--> 

---

# fits with node

---

<!-- * Why is Node.js uniquely suited for handling these problems? 
* So we really have no connection to Node or allegiance to it. So why write these lambdas in Node? Not Python? Or Java? 
* We have competing coworkers at work who like Ruby (why does it take 50 lines to wirte a post?)
* This is a good question too. Because Kickstarter itself is written in Rails - we have two large Rails apps, and a series of Java microservices. Our frontend has been bounced around a bit but is written in jquery and more recently, React. 

ARGUEMENT #1: 

Core node principles are helpful for functional programming and AWS Lambda 
We could use Python or Go, but we chose Node. 

--> 

### _Node.js Design Patterns_
- Single functionality principle
- Substack patterns
- Small surface area
- Separation of concerns (functions should perform a single job, much like modules)
- Promises
- Simple model for i/o

---

# functional programming

<!-- 
- we really <3 functional programming and Node makes that easy (well, maybe not easy but doable and great for learning)
- Your apps are basically functions!

ARGUMENT #2: Writing lambdas can help you write Node.

I was introduced to the two of them at the same time. 

-->

--- 

## immutability & point-free
--
--
## pure functions with promises
--
--
## dependency injection for testing

<!--
Functional Programming with AWS Lambda, immutability and point-free programming
Dependency injection: instead of using hardcoded dependencies, we can pass the dependencies (mocked out for tests) into our module. This make testing much easier. For example, with our slack functions we don't have to deal with POSTs to the webhook repeatedly. There's no implicit execution of external code. 

Pure functions: no side effects, promise-based, promises help it be reactive to data. 

Incorporates great with AWS lambda's "Push/Pull model" - an event producer directly calls the lambda function and pulls update from data stream to invoke the function

Pure functions allow for the lower complexity of programs, immutability reduces points

Think: we have this tech that is processing the same events over and over (either triggered or scheduled)
A pure function is a function, that, given the same parameters, will always return the same result. 
So this build perfectly into any functional ecosystem. The data comes in, data goes out 
-->

---

# devops compatible 

<!--
 * Serverless Redefining DevOps: https://redmonk.com/fryan/2017/03/02/serverless-redefining-devops/
 * functions will be part of an overall continuum of development approaches
 * bring value without the additional administrative overhead of another environment
--> 

---

# Painless Deployment Tools
--
- SAM & Gateway API
- Apex & CircleCI
- Serverless Framework

<!--
- serverless will get you up and started super quick
- show a gif of using that and wuzz with deployment lines in influxdb 
-->

--- 

## DevOps Use Cases
- stack output outputs with cloudformation 
- vault secret store / consul for secret management app finder
- deployment notifier
- notifications to Slack for CloudFormation updates
- Slack slash commands

<!-- 
* stack output finder: gathers all the information needed to spin up a new instance like the latest AMI, VPCID, Subnets, and KeyID
* deployment notifier: have applications handling deployment notify Slack, Grafana, NewRelic, & Email when someone has deployed
--> 

---

# my first lambda function: 
# `sns-to-slack`

---

# Summary
- broaden your Node.js toolbox
- sharpen your node skills with functional programming techniques
- save money by only paying for what you use
- have fun!
