theme: Sketchnote, 5

# Node.js in a Serverless World

^ I want to talk about how Node.js is an ally for an ops engineer and we can use it with Serverless frameworks to make our jobs better. 

<!-- Pitch: Node can be a great ally to any ops engineer. This talk will dive into AWS lambdas, a.k.a serverless functions and show how Node can be used to build a small application running on AWS Lambda with the help of the a new deployment model called SAM (AWS Serverless Application Model). We will also discuss testing (with Mocha and Chai) as well as moving towards purely functional javascript and some of its concepts (immutability and point-free programming). -->

---

# Hello! 👋🏻

### _@loganmeetsworld_
### _DevOps Engineer @ Kickstarter, PBC (NYC)_
### _Node.js Experience: Newbie!_

^ I'm a newbie to Node and Lambdas. 
My talk today will include: 
1. My experiences learning Node and using serverless.
2. How it made me a better developer.
3. Why I think the two are particularlygreat together. 

--- 

# Roadmap 

### _Background on Serverless_
### _Introduction to AWS Lambdas_
### _Why Node.js & I 💖 Serverless_
### _Examples_

^ We will be talk about what "serverless" is
* Introduce ourselves to the AWS lambda service we can use to leverage a serverless mindset
* Talk about why Node and Serverless should be friends
* Go over some examples generally and then with code

---

# "Serverless"

^ Surprise it's not actually serverless!
* I didn't come up with this word, nor do I particularly like it
* Several definitions - could be talking about many things
* Serverless is a bigger idea than just AWS Lambda, but important to intro this first so we don't get confused

--- 

# FaaS
## _App === Bunch of Functions_

^ When I say Serverless, I'm talking about a couple things
* The first is Functions as a Service (FaaS)
* You only pay for the time your function runs and each function has a single purpose
* Your app is basically a bunch of functions if break it down into component parts, so a serverless utopia would be just that, a collection of functions or services that when used together comprise an app

---

# Serverless
## _✨ an abstraction movement ✨_

^ The second thing I mean when I say serverless.
* This is an idea that has evolved over the history of compute. We want to make the unit of deployment faster, cheaper, smaller. We've gone from: Machines -> Virtual machines -> containers -> serverless.
* Furthermore, serverless is a movement to abstract developers away from servers, infrastructure, low-level configuration, etc. (which we can never truly move away from and should always try to understand anyway)
* At Kickstarter, we do not have everything running on serverless by any means, we use lots of EC2 instances, ECS infrastructure, etc. So this is not the talk for understand how to run your entire app "without" servers. 
* HOWEVER, using this model is a way to slowly chip away at the work that your servers are doing and a very valuable tool to have in your toolbelt
* You can take things that previous you had entire servers running forever and break them into functions that only run whenever they are needed, which is pretty cool. 

---

## AWS Lambda Function:
# _Events_
# _Code_
# _Results_

^ This is what I'm talking about today - AWS
* Amazon is the platform that I have experience in, but a lot of the major services are very similar (Google Cloud, IBM Whisk, Azure functions)
* AWS lambda is a single purpose service 
  * Examples of other single purpose services: Amazon S3 for file storage, IAM/authentication for credential management
* Lambda's single purpose is runtime for your functions: it provides parameters and builtin libraries (Node.js!), aws-sdk, some others, YOU provide the code
* To you: code magically runs when triggered
* Only three components you need to be aware of: Events, Your Code, Results
* Those events can be whatever you want: API gateway endpoint (route & method), aws-sdk trigger, simple notification service, CloudWatch event, etc. 
* Event streams are the power behind AWS lambda

---

### Manage functions: 
![inline](/Users/loganmcdonald/dev/nodejs-serverless-talk/assets/img/aws.png)
### Manage single functions:
![inline](/Users/loganmcdonald/dev/nodejs-serverless-talk/assets/img/aws-function.png)
 
^ This is the AWS console for interacting with lambdas
* Simple setup on AWS (as seen here): You define a function that performs an operation and AWS provisions and schedules the function
* Under the surface what it's doing: executing your code in a container and then killing the container

---

## Arguments for Serverless & Lambdas

^ Common arguments and mine

---

## autoscales automatically
## pay-per-execution
## inherently a microservice

^ Common arguements

---

## compatability with Node.js
## functional programming
## "devops" use cases 

^ My further arguements
* I put devops in quotes because what I mean by this is general things we could use to develop software
* But devops tends to take on a lot of the nitty gritty infrastructure type work I'm talking about 

---

# ⚠️ Caveats! ⚠️
#### _Lambda & deployment is new_
#### _5 minute timeout_
#### _AWS/Google/IBM lock in_
#### _Cold start functions_

^ Before we delve further into the arguments for serverless and lambdas, some caveats

---

# compatability with Node.js

^ Why is Node.js uniquely suited for handling these problems? 
* At Kickstarter, before lambdas we had no node in our codebase. Why write these lambdas in Node? Not Python? Or Java? 
* We believe that core node principles are helpful for AWS lambda and doing the type of functional programming we'd like to be doing. 
* Adopting lambda functions can expand ones Node.js toolkit.

---

## _Node.js Design Patterns_
![inline](/Users/loganmcdonald/dev/nodejs-serverless-talk/assets/img/design.png)

^ Note: I started reading this book at the same time I started using lambda functions, saw a lot of paralells

--- 

## Single responsibility
## Substack patterns
## Promises
## Simple model for i/o

^ Definitions: 
- Single responsibility principle:
* Def: Every module should have responsibility over a single functionality
* Lambda: functions should perform a single job, much like modules
- Substack patterns
* Def: Expose the main functionality of a module by exporing only one function and use the exported function as namespace to expose other functions
* Lambda: functions that should expose a single function for use, but may be comprised of many other functions
- Promises
* Def: abstraction that allows us to return a function 
* Lambda: Sequential iteration with Promises allows us to organize our code to make it readibe and easy to know how data is flowing through. 
- Simple model for i/o
* Def: non-blocking model for i/o
* Lambda: allows us to process lots of events asynchronously 

---

# functional programming

^ At Kickstarter, we <3 functional programming and Node fits right in
* Your apps are basically functions!
* I was introduced to the Node.js and functional programming around the same time, and I thought they complemented each other. 

--- 

## immutable & point-free
## pure functions with promises
## dependency injection for testing

^ Functional Programming with AWS Lambda, immutability and point-free programming
* We like our functions to be promise-based and reactive to event data that flows through them
* FP incorporates great with AWS lambda's "Push/Pull model" - an event producer directly calls the lambda function and pulls update from data stream to invoke the function
* Functions are stateless, so we try to use a type of programming that reduces states as much as possible. 
* Pure functions: functions that have no side effects. These functions allow for the lower complexity of programs.
* Immutability: reduces points, state.
* Dependency injection: instead of using hardcoded dependencies, we can pass the dependencies (mocked out for tests) into our module. This make testing much easier. For example, with our slack functions we don't have to deal with POSTs to the webhook repeatedly. There's no implicit execution of external code. 
* MAIN TAKEAWAY: We have this tech that is processing the same events over and over (either triggered or scheduled). A pure function is a function, that, given the same parameters, will always return the same result. So this builds perfectly into any functional ecosystem. The data comes in, data goes out.

---

# "devops" use cases

^ Serverless Redefining DevOps: https://redmonk.com/fryan/2017/03/02/serverless-redefining-devops/
* We believe that functions will be part of an overall continuum of development approaches, not just for devops but all software development
* Lambdas are a great way to bring value without the additional administrative overhead of another environment (server)

--- 

## use cases
#### _one off bash scripts_
#### _deployment notifier_
#### _proxy_
#### _chat notifications & Slack slash commands_

^ Use cases
* Most of these things are "stand alone" pieces that can be split across many services
* NOTE: I think the idea of having a ton of data and wanting to pipe it to actons in particular ways is _very_ familiar to the DevOps engineer. When we think about monitoring systems, we have a ton of metrics and information that all needs to be _filtered_ and _reduced_ along the way. Lambda makes a lot of sense to us. 

---

# deployment notifier
## _"lambda as a proxy"_

^ Deployment notifier: We have an application handling deployment notify Slack, Grafana, NewRelic, & Email when someone has deployed
* I want to talk a little bit about the deployment notifier. We had a deployment system running in a legacy VPC and we had a state of the art metrics system our monitoring team had made running in a new VPC thus we need something that can cross availability zones or VPCs
* Ryan Scott Brown serverlesscode.com blog interview on lambda as a proxy - totally what we do
* "lambda as a proxy" means using lambda as a way to access all of your services

---

# my first lambda function
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
![inline-left](/Users/loganmcdonald/dev/nodejs-serverless-talk/assets/img/index.png)

^ Starting with the index where our function is defined, we pass it some dependencies and then run those dependencies through a function "manager"
* Provide dependencies as input
* Thus allowing our slack manager to use any dependency and therefore allowing it to be used in different contexts (different kinds of SNS calls)
* These dependencies help us mock out our postToSlack for easy testing

---

# defaultDependencies.js
![inline-left](/Users/loganmcdonald/dev/nodejs-serverless-talk/assets/img/default-dependencies.png)

^ For references these are our dependencies: just the post to slack 

---

# slackManager.js
![inline-left](/Users/loganmcdonald/dev/nodejs-serverless-talk/assets/img/slack-manager.png)

^ Here we have the event manager, it collects our functions.
* We can clearly see what functions are data is passed through every time the function runs

---

# parseEvent.js
![inline-left](/Users/loganmcdonald/dev/nodejs-serverless-talk/assets/img/parse-event.png)

^ In our more complex event structure we have several events that are passing through (cloudformation events, rds events, ecs events) and we want to handle these differently
* Here we can use a subjectParser to take any of the events and return data that is particular to the event subject without having to write different functions for each type
* Thus every SNS event no matter what the structure can pass through the same function & logic

---

# postToSlack.js
![inline-left](/Users/loganmcdonald/dev/nodejs-serverless-talk/assets/img/post-to-slack.png)

^ Finally we have the postToSlack that takes all the formatted data and posts it back to slack 
* We can use a Promise.all

---

# postToSlack.js

![inline-left](/Users/loganmcdonald/dev/nodejs-serverless-talk/assets/img/promise.png)

^ The Promise.all allows us to take any urlOptions passed through to this function and post them to slack at once 
* Again this allows our functions to be agnostic to the events they get, processing each in pure data flow
* Allows for parallel execution 

---

# Make sure we have tests!

---

# parseEvent.test.js

![inline-left](/Users/loganmcdonald/dev/nodejs-serverless-talk/assets/img/test.png)

^ This allows us to test out these individual functions with Mocha and Chai, which is awesomme!

---

# Now deploy it!

--- 

# Painless Deployment Tools
## _Apex & CircleCI_
## _Serverless Framework_

^ serverless will get you up and started super quick
* we've used a combination of any of these, serverless has a lot of community support but is a little slow because it relies on cloudformation

---

# Summary
#### _Broaden Node.js toolbox_
#### _Sharpen Node.js skills_
#### _Save money_
#### _Have fun!_

----

# Thanks!
### My glorious Ops team: Natacha, Aaron, Kyle
### NodeConf Barcelona Organizers
