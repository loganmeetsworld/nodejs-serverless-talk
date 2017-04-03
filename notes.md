theme: Next, 1

# Node.js in a Serverless World

^ Want to talk about how Node is an ally for an ops engineer

<!-- Pitch: Node can be a great ally to any ops engineer. This talk will dive into AWS lambdas, a.k.a serverless functions and show how Node can be used to build a small application running on AWS Lambda with the help of the a new deployment model called SAM (AWS Serverless Application Model). We will also discuss testing (with Mocha and Chai) as well as moving towards purely functional javascript and some of its concepts (immutability and point-free programming). -->

---

# Who am I? 

- Logan McDonald
- @loganmeetsworld on twitter / github
- Job: Kickstarter (in NYC) Ops & Security 
  - Ops, security, creative problem solving
- Node.js Experience: Newbie!

^ I'm a newbie to Node and Lambdas. 
I want to talk about:
1. My experiences learning Node and using serverless
2. How it made me a better developer 
3. Why I think the two are great together. 

--- 

# Roadmap

## Background on Serverless  
--
## Introduction to AWS Lambdas
--
## Why Node.js & I 💖 Serverless
--
## Examples

---

# "Serverless"

^ Surprise it's not actually serverless!
* I didn't come up with this word, nor do I particularly like it.

--- 

# FaaS
## App === Bunch of Functions


^ When I say Serverless, I'm talking about two things. 
* The first is Functions as a Service. 
* You only pay for the time your function runs and each function has a single purpose. 
* Your app is basically a bunch of functions, break it down into component parts

---

## _Serverless is an abstraction movement._


^ The second thing I mean when I say serverless.
* Serverless is a movement to abstract users away from servers, infrastructure, low-level configuration, etc.
* Some of the examples I'm going to show today are ways to use a "serverless" model to find servers or initiate the creation of servers, so we are using serverless to create servers, oh the irony.
* We do not have everything running on serverless by any means, we use lots of EC2 instances, ECS infrastructure, etc. 
* It's a way to slowly chip away at the work that your servers are doing and a very valuable tool to have in your toolbelt, but I'm not making the argument for #NoOps or anything
* For example, we were running this instance for our ChatOps infrastructure but slowly realized most of the things we were using that software for: deployments, notifications, AWS alerts (more about that later)

---

# AWS Lambda Functions

![inline](/Users/loganmcdonald/dev/nodejs-serverless-talk/assets/img/aws.png)
![inline](/Users/loganmcdonald/dev/nodejs-serverless-talk/assets/img/aws-function.png)
 
^ What I'm talking about today - AWS
* What I have experience in, but a lot of the major services are very similar
* Part of the "serverless" movement - a "single purpose service"
* Examples of other single purpose services (a huge draw of serverless): 
  * Amazon S3 for file storage, IAM/authentication for credential management
* Simple setup on AWS (as seen here): You define a function that performs an operation and AWS provisions and schedules the function. Nice console for dealing with it. 
* under the surface what it's doing: executing your code in a container and then killing the container (where the cold start issue comes in because of warm containers)

---

## Making the Argument for Serverless & Lambdas

^ Common arguments and mine

---

## autoscales automatically
--
## pay-per-execution
--
## inherently a microservice
--

^ Common arguements

---

## 1. compatability with Node.js
--
## 2. functional programming
--
## 3. "devops" use cases 

^ My further arguements
I put devops in quotes because what I mean by this is general things we could use to develop software, but devops tends to take on a lot of the nitty gritty infrastructure type work I'm talking baout 

---

# Caveats! 
- Lambda is new
- Lambda deployment is new 
- 5 minute timeout
- AWS/Google/IBM lock in
- Cold start functions after ~4 hours

^ Before we delve further into the arguments for serverless and lambdas

---

# 1. compatability with Node.js

^ Why is Node.js uniquely suited for handling these problems? 
* So we really have no connection to Node or allegiance to it. So why write these lambdas in Node? Not Python? Or Java? 
* We have competing coworkers at work who like Ruby (why does it take 50 lines to wirte a post?)
* This is a good question too. Because Kickstarter itself is written in Rails - we have two large Rails apps, and a series of Java microservices. 
* Core node principles are helpful for functional programming and AWS Lambda 
* We could use Python or Go, but we chose Node. 
* This can broaden your Node toolkit.

---

# _Node.js Design Patterns_
## Single responsibility principle
## Substack patterns
## Promises
## Simple model for i/o

^ Definitions: 
* Note: I started reading this book at the same time I started using lambda functions, saw a lot of paralells
- Single responsibility principle:
* Every module should have responsibility over a single functionality
* functions should perform a single job, much like modules
- Substack patterns
* Expose the main functionality of a module by exporing only one function and use the exported function as namespace to expose other functions
- Promises
* abstraction that allows us to return a function 
* Sequential iteration with Promises allows us to organize our code to make it readibe and easy to know how data is flowing through. 
- Simple model for i/o
* non-blocking model for i/o
* allows us to process lots of events asynchronously 

---

# 2. functional programming

^ we really <3 functional programming and Node makes that easy (well, maybe not easy but doable and great for learning)
* Your apps are basically functions!
* ARGUMENT #2: Writing lambdas can help you write Node.
* I was introduced to the two of them at the same time. 
* We promisify everything in our lambdas

--- 

## immutable & point-free
--
--
## pure functions with promises
--
--
## dependency injection for testing

^ Functional Programming with AWS Lambda, immutability and point-free programming
* Pure functions: no side effects, promise-based, promises help it be reactive to data. 
* Incorporates great with AWS lambda's "Push/Pull model" - an event producer directly calls the lambda function and pulls update from data stream to invoke the function
* Pure functions allow for the lower complexity of programs, immutability reduces points
* Think: we have this tech that is processing the same events over and over (either triggered or scheduled)
* A pure function is a function, that, given the same parameters, will always return the same result. 
So this build perfectly into any functional ecosystem. The data comes in, data goes out 
* Dependency injection: instead of using hardcoded dependencies, we can pass the dependencies (mocked out for tests) into our module. This make testing much easier. For example, with our slack functions we don't have to deal with POSTs to the webhook repeatedly. There's no implicit execution of external code. 

---

# 3. devops use cases

^ Serverless Redefining DevOps: https://redmonk.com/fryan/2017/03/02/serverless-redefining-devops/
* functions will be part of an overall continuum of development approaches
* bring value without the additional administrative overhead of another environment

---

# Painless Deployment Tools
--
- SAM & API Gateway
- Apex & CircleCI
- Serverless Framework

^ serverless will get you up and started super quick
* we've used a combination of any of these, serverless has a lot of community support but is a little slow because it relies on cloudformation

--- 

## DevOps Use Cases
- one off bash scripts -> lambda functions
- deployment notifier
- chat notifications
- Slack slash commands

^ deployment notifier: have applications handling deployment notify Slack, Grafana, NewRelic, & Email when someone has deployed
* I want to talk a little bit about the deployment notifier
* NOTE: I think the idea of having a ton of data and wanting to pipe it to actons in particular ways is _very_ familiar to the DevOps engineer. When we think about monitoring systems, we have a ton of metrics and information that all needs to be _filtered_ and _reduced_ along the way. Lambda makes a lot of sense to us. 
* Lots of one off scripts that have tricky problems like crossing availability zones or VPC's too.

---

# my first lambda function: 
# `sns-to-slack`

---

![inline](/Users/loganmcdonald/dev/nodejs-serverless-talk/assets/img/cloudformation.png)

^ The idea is that we have all this data about our stacks being generated by AWS in the console, but we want it to go directly to a special slack channel where we can watch and comment on it

---

![inline](/Users/loganmcdonald/dev/nodejs-serverless-talk/assets/img/simple-event.png)

^ Here's a simple diagram of what it will take for that event to come through 
* CF creates, sends data to an event subscriber like SNS, that SNS is hooked up to report to a function when it's subscription endpoint is hit, and that function should tell slack what's happening as events come through

---

![inline](/Users/loganmcdonald/dev/nodejs-serverless-talk/assets/img/complex-events.png)

^ Let's make this a little more complicated
* Not only do we have lots of events for stacks being created, updated, deleted... 
* We have lots of different types of events all subscribed to an SNS topic that reports to a Lambda
* All these events look different, and we want them all to send data to slack slightly different too

---

# index.js
![inline](/Users/loganmcdonald/dev/nodejs-serverless-talk/assets/img/index.png)

^ Starting with the index where our function is defined, we pass it some dependencies and then run those dependencies through a function "manager"
* Provide dependencies as input
* Thus allowing our slack manager to use any dependency and therefore allowing it to be used in different contexts (different kinds of SNS calls)
* These dependencies help us mock out our postToSlack for easy testing

---

# defaultDependencies.js
![inline](/Users/loganmcdonald/dev/nodejs-serverless-talk/assets/img/dependencies.png)

^ For references these are our dependencies: just the post to slack 

---

# slackManager.js
![inline](/Users/loganmcdonald/dev/nodejs-serverless-talk/assets/img/slack-manager.png)

^ Here we have the event manager, it collects our functions.
* We can clearly see what functions are data is passed through every time the function runs

---

# parseEvent.js
![inline](/Users/loganmcdonald/dev/nodejs-serverless-talk/assets/img/parse-event.png)

^ In our more complex event structure we have several events that are passing through (cloudformation events, rds events, ecs events) and we want to handle these differently
* Here we can use a subjectParser to take any of the events and return data that is particular to the event subject without having to write different functions for each type
* Thus every SNS event no matter what the structure can pass through the same function & logic

---

# postToSlack.js
![inline](/Users/loganmcdonald/dev/nodejs-serverless-talk/assets/img/post-to-slack.png)

^ Finally we have the postToSlack that takes all the formatted data and posts it back to slack 
* We can use a Promise.all

---

# postToSlack.js

![inline](/Users/loganmcdonald/dev/nodejs-serverless-talk/assets/img/post-to-slack-edit.png)

^ The Promise.all allows us to take any urlOptions passed through to this function and post them to slack at once 
* Again this allows our functions to be agnostic to the events they get, processing each in pure data flow
* Allows for parallel execution 

---

# Make sure we have tests!

---

# Now deploy it!
![inline](/Users/loganmcdonald/dev/nodejs-serverless-talk/assets/img/deploy.png)

^ Deploy using whichever framework you want
* Both very simple to deploy with

---

# Summary
- broaden your Node.js toolbox
- sharpen your Node.js skills with functional programming techniques
- save money by only paying for what you use
- have fun!

----

# Thanks!
### My glorious team: 

# Natacha
# Aaron
# Kyle
