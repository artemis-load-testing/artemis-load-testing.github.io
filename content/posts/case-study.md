---
title: 'Artemis Case Study'
date: 2022-04-18T17:12:09-07:00
draft: false
---

## 0. Introduction {#introduction}

Artemis is an open source, serverless framework for scalable load testing of your APIs.

Artemis is an easily deployable, scalable, cloud-based testing infrastructure that provides near real-time results and robust data retention. Artemis utilizes an open source load testing tool with a performant runtime (written in Go) that leverages a ubiquitous scripting language (JavaScript) and provides an elegant dashboard to visualize test results in a clear and meaningful way.

### 0.1 Artemis Quickstart Guide

### **Artemis requires:**

- an[ AWS account](https://portal.aws.amazon.com/gp/aws/developer/registration/index.html?nc2=h_ct&src=default)
- npm[ installed](https://www.npmjs.com/get-npm)
- AWS CLI[ installed](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html) and configured
- [AWS named profile](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-profiles.html)
- AWS CDK command-line tool[ installed](https://docs.aws.amazon.com/cdk/latest/guide/cli.html)

### **Installation**

- run npm install -g artemis-load-testing
- run artemis help to see a list of available commands

### **Artemis Command List**

Usage: `artemis [options] [command]`

### **Login details for Artemis' Grafana dashboard:**

username: artemis password: api_load_testing

## 1. Overview of APIs and Load Testing {#overview_api_load_testing}

### 1.1 What is load testing?

Figure 1.1 What is load testing?

Load testing is the process of simulating usage of an application, website or collection of interconnected resources or services, to test their behavior under conditions that a user defines.

A computer simulates a number of users that perform requests against a server that they wish to test. Results of this load test are then generated based on the responses received and specific metrics can be visualized based on those results.


### 1.2 Why would you want to do a load test?

Figure 1.2 Why load test?

Load testing allows for errors, bugs and bottlenecks to be identified and fixed before they are deployed into a production environment. Additionally, load testing can confirm assumptions made about the performance of a company’s existing infrastructure and its capacity. 


### 1.3 What is API load testing?

Figure 1.3 What is API load testing?

API stands for Application Programming Interface. APIs are vital tools for web applications since they allow the capabilities of one system to be used by another. Essentially, they are a means of enabling communication between two programs. If we think of an application as a black box, the API is like a set of rules that indicates how to interact with the application without having to know how the application works.

API load testing is the process of simulating raw network requests, without regard to a graphical user interface, to measure the performance of a system. A good API load test will generate the type of load you would expect to see in production. This gives you a realistic assessment of how your application architecture will behave and highlights any performance bottlenecks that need to be fixed prior to release. ​​


### 1.4 Load Testing Walkthrough

Figure 1.4 An example of an API load testing tool test script.

Test scripts are executed with the help of a load testing tool. Load testing tools are used for load generation, simulating how real users would interact with an application, website, or other network resource that accepts protocol-based requests. In Figure 1.4, we see an example of a simple load test script. When you write a test script, the goal is to simulate a realistic number of users that will interact with the API and the manner in which they will interact with it.

**Virtual Users** (**VUs**), can be used to mimic the behavior of a real user. Virtual Users are used to perform separate, concurrent executions of your test script. For example, they can make HTTP requests against a webpage or API. On line 6 in Figure 1.4, the target attribute with a value of 200 refers to the number of VUs to be simulated in the first two minutes of the test.

Figure 1.5 A graph of virtual users simulated over the duration of a test.

This particular test is designed to ramp from 0 to 200 virtual users (VUs) over the course of the first two minutes, then gradually decrease the VU number to 100 over the next minute and a half, and finally decrease the virtual user count to zero over the last 30 seconds of the load test. The desired change in VUs over the duration of the test is shown by Figure 1.5.

A virtual user is not limited to making simple, single requests, but rather can execute an entire scenario of actions against a network resource. A **scenario** describes the journey a single user performs on the application, navigating from page to page, endpoint to endpoint, and so on, depending on the type of application. For example, for a standard e-commerce application, a scenario may involve accessing the home page, opening a product description, buying a product, logging in, checking out, and paying. Lines 12-17 in Figure 1.4 outline a scenario describing the endpoints that every virtual user will repeatedly execute for the duration of the test.

Figure 1.6. An example of a load test output.

After the load test has completed execution, the user is presented with the results across a predefined collection of metrics. As shown in Figure 1.6, some of these key metrics include total HTTP requests made, average requests per second, request duration, and request failures. 


```
 http_reqs..........: 26212 108.802851/s  	
 http_reqs_failed...: 0.00%  √ 0  x 26212	   
 http_req_duration..: avg=38.25 ms		   
 http_req_duration..: p(90)=48.05 ms		     
```


Figure 1. 7 Some key metrics from the test results output in Figure 1.6.

Requests made (http_reqs) indicates how many total HTTP requests were generated over the course of the test and reports the average requests per second. 

Request failures (http_reqs_failed) identifies the total amount of requests that failed and the failure rate. 

Request duration (http_req_duration) is a measure of how long the remote server took to process the request and respond. 

Along with totals and averages, it is common to report the min, median, max, and percentile values for a more complete view of the results. For example, the p90 value for request duration in Figure 1.7 indicates that 90% of the requests made lasted less than 48.05 ms.


### 1.5 Why would APIs need load testing?

Calls to API endpoints make up an increasing amount of public and private network traffic. Networking giant Cloudflare has reported that more than 50% of the requests they process are API calls and that API call traffic is growing twice as fast as browser-based web traffic. 

For some companies, APIs are the very focus of their business. Companies like this may expose several endpoints that perform tasks or retrieve data upon receiving requests. Since their APIs are their primary product, it is important to know at what point they would break. 

Global payment provider Stripe is an excellent example of this type of company. 

Figure 1.8. What is Stripe?

Stripe provides an API that allows businesses to access its services without needing to know anything about how Stripe is implementing its core functionality of processing payments. Users of the Stripe API are provided with an interface that accepts credit card, debit card or mobile wallet information across multiple currencies. To use Stripe’s API, business owners don’t need to know the internal processes of how Stripe works; they just need to provide the relevant information to Stripe using the rules defined by Stripe’s API. It is then up to Stripe to ensure that the user receives timely and meaningful responses to their requests.

High availability is essential for companies like Stripe and the customers that rely on them. They face severe consequences for any degradation of service when it comes to the performance of their APIs. As an example, depending on its size, a single minute of downtime on Black Friday could cost a business thousands, tens of thousands or even hundreds of thousands of dollars. Conversely, a company that has done adequate load testing, and is able to perform as expected during peak times such as this, puts themselves in a position to attract new customers and increase sales.


## 2. Use case {#use_case}

Given that companies need a flexible, performant load testing solution to ensure high availability of their APIs, what options do they have?


### 2.1 Existing API load testing solutions

There are several existing load testing solutions that can be run locally and in the cloud. 


#### 2.1.1 JMeter

Figure 2.1. JMeter logo

JMeter is an open source Java application with a graphical interface. Of particular interest to us is JMeter’s ability to perform load and performance testing on servers communicating over common web protocols such as HTTP/HTTPS.

A typical workflow consists of test plan building with the GUI, running the load test, and receiving load test analysis. 

When we start a load or performance test of an application, JMeter simulates the number of users desired by sending requests to the target server. As soon as the server starts responding to the requests, JMeter begins to save all the responses. Based on the gathered data,  JMeter calculates statistical information and prepares a report describing the performance of the application under test. This is not an automatic process, but rather requires additional steps and configuration on the part of the user. 

While JMeter performs protocol-based load testing, test scripts are created in a GUI, making JMeter unsuitable for our use case. We needed a script-based tool as we wanted functionality to be controlled from the CLI, as our use case is aimed towards developers that would already be familiar with writing tests in code and using the CLI.


#### 2.1.2 Gatling

Figure 2.2. Gatling logo

Gatling is an open source load and performance testing tool. It provides multiple Domain-Specific Languages (DSLs) for writing tests. Gatling also uses the concept of scenarios to describe user interactions. Multiple scenarios can be aggregated to form a **simulation** to describe different types of users that may run simultaneously.

Gatling can be deployed on-premises or to the cloud. The free, open source version of Gatling does not support scheduled deployments. In addition, results cannot be visualized as the load test is being run.

Gatling was closer to the load testing tool that we were looking for as it is a script-based, non-GUI solution. However, to use Gatling, engineers would have to learn the DSL. Engineers at a small or medium-sized company may not have the time to learn another language. To fit the use case of being easy to deploy and use for web developers we required a script-based load testing tool that did not require a lot of time to learn.


#### 2.1.3 k6

Figure 2.3. Running k6 open source locally

k6 is an open source load testing tool originally developed by Load Impact, now part of Grafana Labs. Figure 2.3 shows a k6 load test running locally using the open source version of k6.

k6 is written in Go, which is built with performance in mind; its low memory footprint means it can run more virtual users and generate more load on average when compared to other load testing tools. In our case, this meant our virtual user generation per configured load testing instance was only constrained by the memory and CPU that we specified. 

An additional benefit of k6 is that it allows users to write their test scripts in JavaScript making it easily accessible for many developers.


### 2.2 Issues with using an open source solution locally


#### 2.2.1 CPU and Memory restrictions

The largest issue when using an API load testing solution locally, is that the amount of load you generate is limited by your machine’s memory and CPU. Even though k6 is performant compared to other load testing tools, CPU and memory limitations still remain a concern.

The amount of CPU needed depends on the test script. It can be assumed that the larger the test (large number of VUs, long test duration, complex scenarios, etc.), the more CPU power is needed. If your chosen load testing tool uses too many CPU resources to  generate load), there will not be enough resources left to adequately measure the responses received and result metrics will likely be reported incorrectly.

The amount of memory needed depends on the defined scenarios for a load test. The form in which result data is collected and output also impacts memory requirements. A simple k6 test will use around 1 to 5MB of memory per virtual user. This means that if we simulate 1000 virtual users, the memory consumed ranges from 1 to 5GB.

Figure 2.4 Comparison of running 600 and 1000 virtual users with k6

Figure 2.4 shows the output for a test of 600 virtual users and the same test with 1000 virtual users (VUs) run on the same machine locally. The test using 600 VUs simulates around 63,000 requests total and it seems that the API was able to hold up under this load. The same test performed with 1000 VUs also reports around 63,000 requests.

Why is it that both 600 virtual users and 1000 virtual users generate the same amount of requests? Lack of resources. For this particular user, the maximum number of VUs they can simulate on their local machine is 600. The only way to simulate more VUs is to upgrade the machine’s memory and CPU.


#### 2.2.2 Lack of access to results as test is running

Another limitation in using local load testing tools is the lack of available results while the test is running. While this may not pose much of a problem in a short test of just a few minutes, when running longer tests the user has little to no information about how the test is going until it completes and a summary view is output to the console. Instead of providing useful insights throughout the duration of the test, the load test is effectively a black box until the test is complete meaning that the developer can neither derive actionable insights nor cancel and restart a test based on real-time results.

Figure 2.5. Local k6 test running and completing from the CLI


#### 2.2.3 No graphical representation of results after test completes

Figure 2.6. CSV k6 test result output

As we saw in Figure 2.5, the open source version of k6, like many other local load testing tools, provides no default graphical representation of test results. The user may specify an output file, in JSON or CSV format, to capture the more granular results of their tests. A short example of some results in CSV format is shown in Figure 2.6. However, the user will still need to come up with a way to process and parse that data and visualize it in a meaningful way.


### 2.3 Issues with existing cloud solutions

Many open source load testing solutions also offer enterprise-level software for performing large-scale tests from the cloud. These solutions address a number of the previously mentioned shortcomings of running tests locally.

As an example, k6 Cloud is k6’s commercial Software-as-a-Service (Saas) offering.

Figure 2.7 k6 Cloud test visualization 

A major benefit of using a cloud-based solution, like k6 Cloud, is the ability to simulate a much larger number of virtual users than could be achieved with a local load testing solution.

Figure 2.7 displays the results of a sample test run on k6 Cloud. Unlike using k6 locally, the results are visible as the test is running. This provides indication of any performance concerns as they occur during the test. Imagine running a 6 hour test without any insight into the results, only to find out that there was an unexpectedly high failure rate within the first thirty minutes. Without near real-time result visualization, you could be wasting potential development time waiting for the tests to complete.

In addition to providing live, graphical representations of the overall test results, many cloud-based load testing solutions also provide a separate breakdown for individual endpoints within a given scenario. Other features such as scheduling tests, generating reports, and providing performance insights are common amongst cloud SaaS vendors in this space.

Figure 2.8 k6 Cloud subscription tiers

Figure 2.8 summarizes the standard subscription tiers offered by k6 Cloud. Cloud-based load testing tools typically require a subscription that imposes limitations as a function of that chosen tier.

For example, if you were subscribed to Tier 1 and wanted to run a continuous 30 minute. test, you would need to upgrade to the next tier or settle for the 15 minute limit. If you wanted to simulate 2000 virtual users, Tier 3 would be the only option even if the test lasted 15 minutes or less. Although your testing needs span multiple tiers, you’d be forced to choose the least restrictive option and could possibly be overpaying for functionality you don’t need.

## 3. Existing solutions and where Artemis fits in {#existing_solutions}

### 3.1 Option #1: In-house solution

Figure 3.1. In-house solution features

Constructing an in-house solution around an open source tool such as k6 can be a good starting point for developing a minimum viable product or if load testing is not a regular part of development. As the application and testing framework grows, however, so should the load testing solution. Building a scalable load testing application is not an easy undertaking; along with committing resources for long-term maintenance, there are known risks with developing in-house tools that are not mission-critical.

Additionally, technical debt and project delays can be expected due to the lack of domain expertise and effort spent on building a reliable tool. Assuming these challenges can be surmounted, you are still at risk of reinventing the wheel unless a highly customized solution is a necessity. The biggest benefit of building a load testing solution from scratch is flexibility of implementation, which you pay for by managing several unknowns.


### 3.2 Option #2: Cloud solution (SaaS Vendor)

Figure 3.2. Software-as-a-service (SaaS) vendor features

Cloud-based SaaS load testing solutions are well-established and provide the sought-after traits of reliability and scalability. They abstract away much of the complexity required to automate tests, scale clusters, and produce visual results of the test data. Due to their feature-rich nature, cloud-based SaaS tools can be costly and may provide unnecessary functionality.

The subscription-based model these companies use locks users into only simulating a certain amount of virtual users, restricts them into tests of a certain length, and limits the amount of total tests performed over a given time period. Large companies do not have to choose; they can simply select the most expensive option as long as it gets the job done. Small and medium-sized companies do not have this luxury; they must make a choice between testing needs and budget.


### 3.3 Where Artemis fits in

Figure 3.3. Artemis’ solution features

Artemis deploys the necessary infrastructure for performing API load tests to a user’s AWS account with a single CLI command. Artemis leverages k6, an open source load testing tool, that allows developers to write load tests in JavaScript. Users can run load tests without restrictions on the number of virtual users and the test duration. Aggregated test results can be visualized in near real-time with the provided dashboard, and test results are retained in long-term storage, allowing the user to further parse, transform or query the data as desired.

Artemis is not as feature-rich as some of the existing cloud solutions. It does not perform parallel tests or generate load across multiple regions. The results dashboard displays a limited set of metrics and scheduled tests are not a built-in feature.

However, Artemis is a flexible, scalable solution for users who are unable to build out a dedicated testing environment and for whom tiered cloud-based SaaS solutions are too restrictive or costly.


## 4. Introducing Artemis {#introducing_artemis}

### 4.1 Four components

Figure 4.1. Artemis’ components

Artemis’ architecture can be divided up into four different components: load generation, aggregation, data storage, and result visualization.

The first component is concerned with load generation. The load testing containers in this component simulate virtual users that perform requests against the API you wish to test.

The second component performs aggregation of the test results . A single container collects and processes all the test results generated by the load testing containers.

The third component is focused on data storage. The aggregated test results are stored in a time-series database.

The fourth and final component is for result visualization. The provided dashboard allows us to visualize meaningful metrics generated by our load test.


### 4.2 Full architecture

Figure 4.2. Artemis’ Architecture 

Artemis’ infrastructure is accessible to the user through the Artemis CLI. This enables the user to run tests, visualize results, and manage, deploy and teardown their load testing infrastructure in AWS.


### 4.3 Deploying Artemis

Prior to load generation, the supporting infrastructure needs to be in place.
 

With a single command, **artemis deploy**, the necessary components can be deployed to AWS. This command makes use of the AWS Cloud Development Kit (CDK), which is a framework for modeling cloud infrastructure, via code, that can then be synthesized and deployed to a user’s AWS environment. Artemis deploys serverless components, launched on demand, meaning the user can run load tests without having to manage their own servers.

Deploying the infrastructure involves compiling the CDK code into a template which can then be used by the CloudFormation service to create the specified resources.

AWS CDK is not the only way to build out infrastructure on AWS. As mentioned previously, the CDK code must be translated into a CloudFormation template, which is a JSON or YAML file that indicates the resources you want to deploy and how you want them to be configured. The advantages of using the CDK for our use case, instead of constructing a CloudFormation template directly, are many: 

- It’s easier to get started with the CDK since it’s available in many commonly used programming languages. We chose to interact with the CDK using Typescript for its inherent benefit of static typing. 
- The CDK documentation is easier to parse and the API reference is much more detailed compared to that of CloudFormation.
- The CDK is an abstraction over CloudFormation, hence the code tends to be more concise. Our CDK code was 429 lines long, but the CloudFormation template spanned 1455 lines.


### 4.4 Starting a test with Artemis

Figure 4.3. Each task running the test gets a copy of the test script from S3

To use the Artemis CLI to initiate a load test,  the user provides two inputs: the local file path to the test script and the number of load testing containers to spin up. The start command, **artemis run-test**, triggers an action causing the script to be uploaded to an S3 bucket and invokes a lambda function to spin up the total number of load testing containers specified by the user.

S3 is AWS’ object storage service.  Object storage provides a way to store unstructured data in a structurally flat data environment. There are no folders, directories, or complex hierarchies as in a file-based system. While cloud-based object storage is ideal for long-term data retention, we used it as a means to an end. We needed a way for the user’s test script to run inside the load testing containers. 

Since a user’s local files can’t be read by a Lambda function, S3 serves as a stepping stone for getting the test script onto the AWS infrastructure. AWS Lambdas fall in the realm of Functions-as-a-Service. Functions-as-a-Service implementations allow you to make functions, or chunks of code, available on demand to respond to events as they occur. The Functions-as-a-Service provider manages the underlying infrastructure and the user is charged by execution time. In our case, the Start Test lambda pictured in Figure 4.3 spins up the specified number of containers. Each application in the container then creates a local copy of the test script and starts the test.


## 5. Using Artemis {#using_artemis}

### 5.1 artemis deploy

Deploying the infrastructure can be performed with a single command, **artemis deploy**. The user will receive confirmation in the terminal that the infrastructure was successfully deployed. A user would typically run this command once upon first using Artemis and before running any load tests. 

Figure 5.1 Demo of the artemis deploy command running in the CLI


### 5.2 artemis teardown

When load testing has concluded,  the infrastructure can be removed using **artemis teardown**. All components, with the exception of the database, will be removed from the user’s AWS infrastructure. A separate command, **artemis destroy-db**, is available if a user wishes to delete the database storing Artemis’ results.

Figure 5.2 Demo of artemis teardown command running in the CLI


### 5.3 artemis run-test

To initiate a test through the Artemis CLI, the user executes **artemis run-test** and specifies the path to the test script and the number of containers to spin up to generate the desired load. The user is presented with a three minute spinner that represents our timestamp coordination function. This function ensures that the user’s test, across all containers, starts at the same time. We explain this functionality in greater depth in the next section. 

After three minutes have elapsed, the user is presented with confirmation that the test has started along with a unique ID assigned to the test. This ID will be present in every row of data in the database such that the data can be grouped by test when queried. 

Figure 5.3 Demo of the artemis run-test command running in the CLI

Now that the test is running, the user can expect their test results to be stored in the database and may choose to launch a container running our provided Grafana dashboard.


### 5.4 artemis grafana-start

To use Artemis’ Grafana dashboard, the user runs **artemis grafana-start** from the CLI. Once the container is up and running, the user will receive an IP address which they can access to view the dashboard.

Figure 5.4 Demo of the artemis grafana-start command running in the CLI

Artemis’ default dashboard view displays the results of the most recent test, and the user can zoom into specific portions of the test by highlighting the areas of interest. A user is not confined to the provided dashboard and queries. They may add their own panels to the dashboard and define their own custom queries.

Figure 5.5 Demo of the artemis grafana-stop command running in the CLI

When a user no longer needs to use the dashboard, they can run **artemis grafana-stop** to terminate the container. 


### 5.5 artemis sleep

Artemis also provides the command **artemis sleep** to stop both the Telegraf and Grafana containers. The user can think of this as ensuring their infrastructure is in a “low power mode”. This command is useful when a user doesn’t want to teardown their infrastructure but is finished testing for the moment and wants to make sure that no containers are running in the background to prevent incurring unnecessary AWS charges.

Figure 5.6 Demo of the artemis sleep command running in the CLI


## 6. How we built it - Design Decisions {#design_decisions}

### 6.1 Scaling load generation

Figure 6.1 Load generation component of Artemis’ architecture

The load testing containers, identified as tasks in Figure 6.1, are running in a Virtual Private Cloud. A Virtual Private Cloud, or VPC, is a secure, logically isolated portion of the AWS infrastructure where you can deploy your own resources. These tasks generate virtual users that perform requests against the API you wish to test.  Next, let’s take a closer look at the elements that make up our load testing containers.

Figure 6.2 The internals of a single load testing container


#### 6.1.1 Running load tests in the cloud 

Each of the load testing instances pulls a custom image from AWS’s public container repository, known as Elastic Container Registry or ECR. The custom image is one that we configured and published to ECR. It includes k6 and our Node application.  

The Node application, **artemis.js**, fetches the previously uploaded test script from the S3 bucket, waits for a prescribed period for test start coordination,  then uses k6 to run the test based on the provided script. The test results are then sent to a separate container for aggregation. 

During a regular load test, a user may be spinning up one or more of these containers. Where are these containers running and how is the scaling of these containers handled?  The answer is AWS Fargate and AWS Elastic Container Service on a VPC.

AWS Fargate is a serverless compute engine for containers. It is commonly compared to Elastic Cloud Compute (EC2) as a way to run containers on AWS. Unlike EC2, Fargate eliminates the need to provision and manage servers, and lets you specify and pay for resources per application. Fargate’s serverless nature aligns with our goal of making Artemis serverless for its users. Artemis users can simply write load tests without the burden of determining the optimal compute power needed to run the tests. Lambdas, and more generally Functions-as-a-Service, are also considered serverless. However, Lambda functions proved to be too restrictive since they can only be used for tasks that run for 15 minutes or less. This function timeout imposes a test duration limit on Artemis users, which was not acceptable for our use case.

All of our containerized applications are running in Fargate instances. ECS allows us to manage, deploy, and scale the containerized applications provisioned by Fargate. The number of instances specified by the user when running a test is used by ECS to determine the number of Fargate instances to spin up. 


#### 6.1.2 Challenge: Determining container size and number of VUs per container

Through testing and analysis, we determined the optimal number of Virtual Users per container to be 200. This gives the user the greatest flexibility in terms of overall total VUs and length of test duration. Our users write their test scripts for up to 200 VUs and then specify the number of tasks desired for a particular test run. With this information, we can generate load across multiple containers to achieve a large total number of Virtual Users. For example, if a user wants to test for 5,000 VUs, they simply write their script based on 200 VUs and specify a task count of 25. Likewise, if a user wants to perform a load test that simulates 200 or fewer users, they can use the same test script and specify just one task. 

Figure 6.3 Scaling of Fargate instances for an increasing number of virtual users

Although ECS and Fargate make it easy to spin up serverless instances, we still needed a way to coordinate the starting of load tests across these multiple instances.


#### 6.1.3 Challenge: Starting the same test across multiple containers at the same time

Figure 6.4 Tests start as soon as the container is spun up, leading to inaccurate results

Once we solved the problem of spinning up more than one instance, we noticed that containers in the ready state would automatically run the test script. 

The question to answer then became: How can we decouple the spinning up of instances from the starting of a test?

Ideally, you would want to have all of the virtual users running at the same time to accurately simulate the desired load. Otherwise, you could end up with skewed test results that wouldn’t accurately reflect the test input and therefore invalidate the test itself. 

[talk about Express app here]

We first considered using AWS Step Functions to coordinate a series of lambdas for running tasks concurrently. AWS has a Distributed Load Testing Implementation Guide that makes use of Step Functions using worker tasks and a leader task. This requires the leader task to communicate with each of the worker tasks using a Python script to create a socket on the workers to listen for a start message from the leader. This workflow goes beyond what is described here to start multiple tests and would introduce unnecessary complexity to our use case.

Figure 6.5 Test start at the same time, irrespective of when the container has spun up

Another option we explored, and how we ended up addressing the problem, was introducing a delay to the running of the test script. This was achieved by generating a timestamp 3 minutes from when the Start Test Lambda is invoked. Each test container then calculates a specific wait time based on when it’s created, using the initial timestamp as a reference. This wait time can be thought of as a container-specific timer, enabling test start synchronization.


### 6.2 Data aggregation

Figure 6.6 A high level overview of our data aggregation component. 

A single container collects and processes all the test results generated by the load testing containers. Before we go deeper into this part of our infrastructure, let’s examine three reasons why Artemis needs to aggregate result data.

Figure 6.7 A generic data aggregation workflow


#### 6.2.1 Why we need to aggregate

In order to provide meaningful insights into the performance of the API, the metrics displayed to the user should reflect the load testing results across all containers. As we mentioned before, to generate the load desired by the user, we are spinning up multiple containers with each container running their own load test. The downside of spinning up multiple containers to generate load is that the results generated by each test do not take into account the results generated by tests in other containers. This means that every data point presented to the user when visualizing does not represent the load test as a whole, instead, it represents the performance of the API from the view of a single container. 

We also want to aggregate data so the user can make sense of it when visualized. Plotting too many data points produces too much noise, and by noise we mean any data that a user cannot understand and interpret correctly. If every single data point generated by each container is shown to the user, it is harder to immediately determine how the API is doing. 

Aggregating data also prevents us from having unnecessary writes to and reads from the database. When we aggregate, writes to the database go down since we are reducing the amount of test results before they hit the database. Reads also go down, since our dashboard does not need to query as many data points to display meaningful results. 

We needed a solution that would allow us to funnel test results from all containers into a central location, where we would then proceed to aggregate said results. We considered two approaches to solve this problem.

Figure 6.8 - Polling test containers for results 


#### 6.2.2 Challenge: aggregating data from test results

First, we looked into a polling-based approach. In this approach, k6 would perform the load test and have the test results written into a file in either JSON or CSV format. We would tail the results file, take those results, and send them to be aggregated.

An issue with this approach is that k6 consumes a large amount of memory when test results are written to a file. Every virtual user simulated by k6 stores a copy of this file in memory. This presented a problem because simulating a large number of virtual users or performing long tests would deplete the container resources prematurely and terminate the container. 

Even if k6 did not have this memory issue, there was still the challenge of aggregation for us to solve.

Figure 6.9 - Streaming-based approach using Telegraf

Then we looked into a streaming-based approach. Instead of bi-directional communication, a streaming approach entails a one-way data flow in near real-time. Each individual load test container sends data, as it is generated, to a single server. The server is configured to combine the results received at a regular time interval. k6 provides the ability to output data in line protocol format through a built-in plugin. After evaluating a number of data aggregation solutions, we chose a tool called Telegraf.

Figure 6.10 - Example of Telegraf aggregating data points

Telegraf is an open source data collection agent that is written in Go. It has no external dependencies and has minimal memory requirements. Telegraf includes dozens of built-in plugins that the user can activate as needed for their specific use case. These plugins provide a wide range of input, processing, aggregation and output functionality. 

For our use case, Telegraf has the ability to receive data in **line protocol** format making it compatible with k6. We use Telegraf’s BasicStats and Quantile plugins to calculate the mean, min, max, count, and sum, as well as the p90 and p95 value, of all the incoming data. These metrics are important for accurately representing the test results as a whole.

We chose to use k6 and Telegraf since they allowed us to do exactly what we needed: stream the data from every single container into a central location and aggregate that data to generate more meaningful results for the user. The Telegraf container aggregates the data every ten seconds, based on the metrics we specify, and then inserts it into a database for long-term storage. We found the ten second window substantially reduced the amount of raw data that we needed to store while retaining meaningful insights into the test results.  


#### 6.2.3 Challenge: Container communication

Having determined that Telegraf was Artemis’ best option for addressing the problem of data aggregation, we were faced with the problem of how to make our Telegraf container reachable by the numerous test containers executing **artemis.js** and running the user’s tests with k6.

While all of our containers are running in a single VPC, the serverless nature of our implementation means that container instances get spun up and torn down on demand and the private and public IP address of our Telegraf container will change whenever a new instance of it is created. We needed a straightforward way to always make the current instance of our Telegraf container reachable by all of our test containers.

We solved this problem by running our Telegraf container as a service within our ECS cluster and enabling service discovery on that service. This allowed us to retain the ephemeral nature of the container while providing a static address for our test containers to connect to when sending results. If a user runs a test and a Telegraf instance is not running within this service, our implementation starts a new Telegraf container along with the test containers. Whenever a new Telegraf instance within that service is launched, ECS uses AWS CloudMap to update Route 53, which is AWS’ DNS service, to be aware of the IP address of the Telegraf container within the VPC. Route 53 then maps that address to the DNS of the service we defined, so when our test containers send data to [http://artemis-telegraf.artemis:8186](http://artemis-telegraf.artemis:8186), it is correctly routed to the currently running instance of Telegraf. 


### 6.3 Data storage

Figure 6.11 - High level overview highlighting the data storage component

Next, we were faced with the problem of how to best store data long-term for historical monitoring purposes while retaining the scalable and serverless quality of the rest of Artemis’ architecture. 


#### 6.3.1 Why store all this data?

One of the main restrictions imposed by existing cloud solutions that we wanted to address with Artemis was that of data retention. Existing API load testing SaaS solutions, depending on the subscription tier, offer from one to six months of data retention. Early on, we decided that Artemis would not impose any restrictions in terms of data retention for better historical data analysis. 

Historical data, regardless of the industry, allows us to look at the past and helps us detect and analyze changes as we approach the present. When dealing with an issue, keeping data long-term allows us to answer several important questions: has this issue happened before? And if it has, when did it first start and why? Being able to search over months or years of historical data can be a huge advantage and may allow you to more easily answer these questions. 

The knowledge gained from past data can provide you with greater confidence in your system. For load testing specifically, historical data enables the tracking of performance over time, which is essential when dealing with APIs.


#### 6.3.2 Time-series databases

k6, the load testing tool at the center of Artemis, outputs test results as time-series data. **Time-series data** is a sequence of data points collected over time intervals, giving us the ability to track changes over time. Time-series data can track changes over milliseconds, days, or even years. This type of data accumulates very quickly, and normal databases are not designed to handle the large amounts of necessary writes.

k6 also recognizes how large amounts of data introduce an additional challenge, "...a large load test will likely produce a massive amount of data points. On the storage side, the database will likely become another bottleneck…In this case, your team must choose a high-performing database, optimizing for this use case, and typically aggregating the data before storing it."

The two main benefits that time-series databases provide over traditional databases are usability and scale. Usability refers to any features, like continuous queries, that enable the user to more easily perform data analysis. Time-series databases are able to offer greater scalability since they treat time as a first-class citizen. Treating time as a first-class citizen introduces efficiencies such as higher data ingest rates and faster queries at scale. 


#### 6.3.3 Prometheus as a data storage solution

First, we looked into Prometheus as a potential data storage solution. Prometheus is an open source, metrics based monitoring system and time-series database. The user is able to query data from Prometheus using its own custom, independent query language, PromQL. PromQL has no overlap with other query languages used in time-series databases. The user would need to learn a new language to utilize this solution.

k6 supports sending test result metrics to a Prometheus remote write endpoint through the use of an extension. To maintain Artemis’ serverless nature, we would need to use the AWS Managed Prometheus service (AMP), a serverless solution where cost is based on the metrics ingested, stored or queried. Otherwise, using Prometheus on AWS would require the user to manage an EC2 instance and thus not be serverless.

Our envisioned infrastructure had our load testing container, running k6, stream the test result data directly to the AMP remote write endpoint. Data aggregation was not being considered at this point. This remote write endpoint was provided by AMP immediately after creating a Prometheus instance. An issue with this approach is that AMP, as well as other AWS services, requires requests sent over HTTP to provide AWS authentication information through the SigV4, or Signature Version 4, signing process. Since k6 lacks the ability to do this, we looked into ways in which we would sign the requests before they were sent to AMP. 

A potential solution involved running a sidecar container which would run AWS SigV4 Proxy. AWS SigV4 Proxy is an open source project that looks for AWS SDK credentials in three different areas: environment variables, shared credentials file, or the IAM role for the ECS task role. Requests would be routed from k6 to the proxy, the proxy would sign these requests using the credentials found, and then the requests would be forwarded to AWS AMP. While this solution would allow AMP to accept the test results produced by k6, it would effectively double the amount of containers deployed per test. Doubling the containers would duplicate the cost per test as well as introduce more points of failure.


#### 6.3.4 Timestream

The next data storage solution we looked into was AWS Timestream. AWS Timestream is a scalable, serverless time-series database. Like AWS Managed Prometheus, Timestream's pricing is based on writes, queries, and storage. For our use case, there are two qualities of Timestream that make it a better solution for our data storage component when compared to Prometheus: the query language used and the way data is stored. 

Timestream uses SQL as its query language. Since one of our main design decisions is to make Artemis easy to use, giving the user the option to query data in a widely used language like SQL is a better option compared to PromQL. Timestream stores data in both memory and magnetic stores. The in-memory store is used for recent data while the magnetic store is used for historical data. Timestream has the ability to automatically move data from the memory store to the magnetic store as it reaches a certain age. The magnetic store is a more cost-effective way of storing data; this is ideal for our users, since they might choose to retain their load test results over long periods of time.

This, combined with the fact that it integrates well with k6, Telegraf and our overall architecture implementation, made Timestream the right choice for Artemis’ data storage component. 

Figure 6.12 - Aggregated time-series data is inserted to Timestream database


### 6.4 Result Visualization

At this point we haven’t seen any of the results of the load tests. We needed a means to make sense of the data, in a way that was easy to understand. This leads us to our fourth  and final component: result visualization. Let’s take a look at how we decided to implement this part of Artemis’ infrastructure.

Figure 6.13 - High level overview of the data visualization component


#### 6.4.1 Grafana dashboard

Grafana provides an open source solution for visualizing data over time via a customizable dashboard. It connects to several databases such as PostgreSQL, InfluxDB, MySQL, and AWS Timestream and allows the user to decide on what information to display by associating a data source and one or more database queries with a panel. A dashboard can be composed of several user-customized panels. At any point during the creation process, a dashboard’s configuration can be preserved as a JSON file and re-used as a template for other dashboards.

Figure 6.14 - A panel (top) and the associated data source and database query (bottom) that informs the graph displayed by the panel. 

We chose Grafana for its modular, easy-to-use dashboard that allows for data to be visualized in different ways. We connected Grafana to Amazon Timestream via a plugin available from Grafana Labs. Although we configured a default dashboard view, the Artemis user has the flexibility to display additional metrics that they deem important. 

As demonstrated in section 5.4, the Artemis CLI can be used to spin up a Grafana container that will automatically query the Timestream database. This will allow us to visualize the results of our tests. Results are updated every ten seconds as the tests are running.

We wrote custom database queries to display the following metrics in the dashboard: virtual users simulated, HTTP request duration, requests per second, and request errors. These metrics are often displayed in other load testing result visualizations and provide a baseline understanding of how the API is performing. For example, if the request rate (requests/second) does not correlate with the change in VUs, it could indicate a bottleneck.

The four summary panels shown at the top of the dashboard provide statistics to enable a user to see how their test is running at a glance. 

Total requests per second shows the number of requests made every second to all endpoints in the test script. On its own, this metric does not tell us much about the performance of the API, but when request duration increases or errors start to appear, it lets us know at what point our system starts to break.

Figure 6.15 - Top half of the Grafana dashboard showing the four summary panels and total requests per second.

Figure 6.16 is the associated query that obtains the data from Timestream to illustrate the total requests/second. It locates the rows that have a value of “value_sum” within the measure_name column of the http_reqs table. A single measure_value within a “value_sum” row indicates the sum of HTTP requests made across all containers over the Telegraf aggregation period. On line 1, the sum of the number of HTTP requests is divided by the 10 second period to determine an approximate number of requests per second.

Figure 6.16 - Timestream query for total requests/second panel.

HTTP Request Duration displays how long it takes to perform the entire request-response cycle. This metric is represented in four different ways: min, p90, p95, and max. The percentile values allow the user to determine the request duration that most users are experiencing. If these percentiles are complemented with minimum and maximum measurements, then it is possible to have a much more complete view of the data and better understand how the system behaved.

Figure 6.17 - Bottom half of the Grafana dashboard showing three panels: HTTP Request Duration, HTTP Failures, and Virtual Users

The HTTP Failures panel displays dots that represent any response that returned with a status code in the 400 or 500 range. This allows the user to quickly determine how many errors occurred within a certain time period.

The bottom right panel, Virtual Users, displays the total number of users simulated across all containers.  


## 7. This is Artemis - Final Architecture {#this_is_artemis}

[bullet summary]

Figure 7.1 - Full Artemis architecture highlighting its four components 


## 8. Future Work {#future_work}

Let’s briefly talk about some things we’d like to add to Artemis in the future.

Our current infrastructure allows for testing up to 20,000 VUs. This limitation is a function of our aggregation implementation. We would like to explore ways to scale this aggregation step to accommodate an even greater number of VUs.

We would also like to implement an automatically generated “executive summary” of the results of each test that could be easily shared across the user’s team or organization.

Currently, tests can be started on demand only. We would like to add functionality that allows users to schedule tests as well.


## 9. References {#references}


## Presentation {#presentation}


## Our Team {#our_team}