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



---

## Why is Node.js uniquely suited for handling these problems? 
( I am well prepared to talk about this as I have been battling a coworker of mine for the past couple months who really wants to write all of our lambdas in Go and really hates Javascript. Why does it take 50 lines of code to write a POST??? )
This is a good question too. Because Kickstarter itself is written in Rails - we have two Rails apps, and a series of Java microservices. Our frontend has been bounced around a bit but is written in jquery and more recently, React. 

So we really have no connection to Node or allegiance to it. So why write these lambdas in Node? 

Core node principles are helpful for functional programming and AWS Lambda 
We could use Python or Go, but we chose node. 
- Single Functionality principle
- Substack patterns - define more here
- Small surface area
- separation of concerns (functions should perform a single job)
- Promises
- simple model for i/o
- we really <3 functional programming and Node makes that easy (well, maybe not easy but doable and great for learning)

- caveats! Lambda is pretty new, ways of deploying lambda are pretty new, you can't do super long running processes because there is a 5 minute timeout, it's through AWS 

---

Deployment Options

<!-- 2 min
-->
* SAM - command line tool written in Nodejs
* Apex & CircleCI
* Gateway API + Serverless
- serverless will get you up and started super quick
- show a gif of using that and wuzz with deployment lines in influxdb 

---

Functional Programming with AWS Lambda, immutability and point-free programming
<!-- 5 min
-->

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

<!-- Prompt: Node can be a great ally to any ops engineer. This talk will dive into AWS lambdas, a.k.a serverless functions and show how Node can be used to build a small application running on AWS Lambda with the help of the a new deployment model called SAM (AWS Serverless Application Model). We will also discuss testing (with Mocha and Chai) as well as moving towards purely functional javascript and some of its concepts (immutability and point-free programming). -->

---

