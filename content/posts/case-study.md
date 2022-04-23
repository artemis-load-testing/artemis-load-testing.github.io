---
title: 'Artemis Case Study'
date: 2022-04-18T17:12:09-07:00
draft: false
---

# Artemis Case Study

## 0. Introduction {#introduction}

APIs are widely used and play an integral role in providing essential functionality to third-party applications. Given their vital nature, they should be tested continuously to ensure high availability. API load testing, or the process of simulating realistic traffic to an API to test its limits, can be performed using any number of available load testing tools in a local environment or in the cloud. Tests run locally often face a physical constraint, limiting the amount of load that can be generated. Alternatively, using a third-party cloud provider lacks flexibility in choosing test duration and volume on top of cost concerns.

Artemis is an open source, serverless framework for scalable API load testing. It fills the gap by enabling the user to execute tests of varying volume and duration. The user can run tests that meet their needs, without the constraints of limited local resources or the limitations imposed by a paid cloud solution. Artemis is an easily deployable, cloud-based testing framework that provides near real-time results and robust data retention. Artemis utilizes an open source load testing tool with a performant runtime (written in Go) that leverages a ubiquitous scripting language (JavaScript) and provides a clean, customizable dashboard to visualize test results in a meaningful way.

### 0.1 Artemis Quickstart Guide

**Artemis requires:**

- an[ AWS account](https://portal.aws.amazon.com/gp/aws/developer/registration/index.html?nc2=h_ct&src=default)
- npm[ installed](https://www.npmjs.com/get-npm)
- AWS CLI[ installed](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html) and configured
- [AWS named profile](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-profiles.html)
- AWS CDK command-line tool[ installed](https://docs.aws.amazon.com/cdk/latest/guide/cli.html)

**Installation**

- run `npm install -g artemis-load-testing`
- run `artemis help` to see a list of available commands

**Artemis Command List**

Usage: `artemis [options] [command]`

| Command            | Description                                                             |
| ------------------ | ----------------------------------------------------------------------- |
| run-test [options] | Run the test script concurrently the specified number of times.         |
| grafana-start      | Start the Artemis Grafana dashboard.                                    |
| grafana-stop       | Stop the Artemis Grafana dashboard.                                     |
| deploy             | Deploy Artemis infrastructure onto the user's AWS account.              |
| sleep              | Stop all supporting container tasks for minimal AWS usage charges.      |
| teardown           | Teardown Artemis infrastructure on user's AWS account, retain database. |
| destroy-db         | Permanently delete the Artemis database.                                |
| admin-dashboard    | Start admin dashboard GUI.                                              |
| help [command]     | Display help for a specific command.                                    |

**Login details for Artemis' Grafana dashboard:**

| username | password         |
| -------- | ---------------- |
| artemis  | api_load_testing |

---

## 1. Overview of APIs and Load Testing {#overview_api_load_testing}

### 1.1 What is load testing?

{{< figure src="../assets/6_what-is-load-testing.gif" caption="Figure 1.1 What is load testing?" >}}

**Load testing** is the process of simulating usage of an application, website or collection of interconnected resources or services, to test their behavior under predefined conditions.

For example, a computer running a load test would simulate a number of users performing requests against a server under test. Results of this load test are then generated based on the responses received and specific metrics can be visualized based on those results.

### 1.2 Why perform a load test?

{{< figure src="../assets/7_why-load-test.gif" caption="Figure 1.2 Why load test?" >}}

Load testing allows for errors, bugs and bottlenecks to be identified and fixed before deployment into a production environment. Additionally, load testing can confirm assumptions made about the performance of a company’s existing infrastructure and its capacity.

### 1.3 What forms of load testing exist?

There are two main branches of load testing: browser-based and protocol-based. **Browser-based load testing** is concerned with end-to-end testing of an application via its user interface (UI). User actions are the driving force and as such, tests are written from the perspective of a user interacting with the application through a browser.

**Protocol-based load testing**, unlike browser-based testing, focuses on the performance of the underlying HTTP, or other network protocol, requests and doesn’t simulate the browser UI. For applications without a web UI, there is an argument for targeted, resource-efficient testing provided by protocol-based solutions.

API load testing follows a protocol-based load testing approach. Before delving further into API load testing, it’s important to establish a definition for API.

### 1.4 What is API load testing?

{{< figure src="../assets/8_what-is-api-load-testing.gif" caption="Figure 1.3 What is API load testing?" >}}

**API** stands for **Application Programming Interface**. APIs are vital tools for web applications since they allow the capabilities of one system to be used by another. If an application is treated as a black box, the API is like a set of rules that indicates how to interact with the application without having to know how the application works.

**API load testing** is the process of simulating raw network requests, without regard to a graphical user interface, to measure the performance of a system. An effective API load test will generate the type of load a user would expect to see in production. This gives the user a realistic assessment of how their application architecture will behave and highlights any performance bottlenecks that need to be fixed prior to release. ​​

### 1.5 Why would APIs need load testing?

Calls to API endpoints make up an increasing amount of public and private network traffic. Networking giant Cloudflare has reported that more than 50% of the requests they process are API calls and that API call traffic is growing twice as fast as browser-based web traffic.

For some companies, APIs are the very focus of their business. Companies like this may expose several endpoints that perform tasks or retrieve data upon receiving requests. Since their APIs are their primary product, it is important to know at what point they would break.

Global payment provider Stripe is an excellent example of this type of company.

{{< figure src="../assets/12_stripe_api.png" caption="Figure 1.4. What is Stripe?" >}}

Stripe provides an API that allows businesses to access its services without needing to know anything about how Stripe implements its core functionality of processing payments. Users just need to provide the relevant information to Stripe using the rules defined by Stripe’s API. It is up to Stripe to ensure that the user receives timely and meaningful responses to their requests.

High availability is essential for companies like Stripe and the customers that rely on them. They face severe consequences for any degradation of service when it comes to the performance of their APIs. As an example, a single minute of downtime on Black Friday could cost a business thousands, tens of thousands or even hundreds of thousands of dollars. Conversely, a company that has done adequate load testing, and is able to perform as expected during peak traffic, puts themselves in a position to maintain existing customers, attract new customers, and increase sales.

To get an understanding of what load testing entails, it is useful to start by looking at how a load test is written.

### 1.6 What does a load test typically look like?

{{< figure src="../assets/pokeapi.png" caption="Figure 1.5 An example of an API load testing tool test script." >}}

Test scripts are executed with the help of a load testing tool. Load testing tools are used for load generation, simulating how real users would interact with an application, website, or other network resource that accepts protocol-based requests. Figure 1.5 shows an example of a simple load test script. When a user writes a test script, the goal is to simulate a realistic number of users that will interact with the API and the manner in which they will interact with it. These simulated users are often referred to as virtual users.

**Virtual Users** (**VUs**) mimic the behavior of a real user by performing separate, concurrent executions of a test script. For example, they can make HTTP requests against a webpage or API. A virtual user is not limited to making simple, single requests, but rather can execute an entire _scenario_ of actions against a network resource.

A **scenario** describes a sequence of endpoints traversed by a virtual user. For example, for a standard e-commerce application, a scenario may involve making a call to a third-party API for checking out, which then makes subsequent calls to other endpoints for collecting payment details, verifying an account balance, and submitting a payment. Lines 12-17 in Figure 1.5 outline a scenario describing the endpoints that every virtual user will repeatedly execute for the duration of the test.

On line 6 in Figure 1.5, the target attribute with a value of 200 refers to the number of VUs to be simulated in the first two minutes of the test.

{{< figure src="../assets/200_vus.png" caption="Figure 1.6 A graph of virtual users simulated over the duration of a test." >}}

This particular test is designed to ramp from 0 to 200 virtual users (VUs) over the course of the first two minutes, then gradually decrease the VU number to 100 over the next minute and a half, and finally decrease the virtual user count to zero over the last 30 seconds of the load test. The desired change in VUs over the duration of the test is shown by Figure 1.6.

{{< figure src="../assets/k6_output.png" caption="Figure 1.7. An example of a load test output." >}}

After the load test has completed execution, the user is presented with the results across a predefined collection of metrics. As shown in Figure 1.7, some of these key metrics include total HTTP requests made, average requests per second, request duration, and request failures.

{{< figure src="../assets/fig1_8.png" caption="Figure 1.8 Explanation of some key metrics from the test results output in Figure 1.7." >}}

## 2. Existing Solutions {#existing_solutions}

Companies that need a flexible, performant load testing solution to ensure high availability of their APIs generally pursue one of two avenues: implement an open source tool in a local environment or pay for a cloud-based solution.

### 2.1 Local open source load testing solutions

JMeter, Gatling and k6 are some of the more commonly used API load testing tools that are available and can be run locally.

Each of these tools provides the functionality to perform protocol-based load testing. These tools are written in a variety of different languages and vary in the way a user would create and run tests. JMeter has limited scripting capabilities; tests are composed primarily through a GUI whereas Gatling and k6 tests must be written as scripts in the prescribed language. For example, k6 tests are written entirely in JavaScript and Gatling tests are written in a domain-specific language (DSL).

{{< figure src="../assets/fig2_1.png" caption="Figure 2.1 Comparison chart of three prominent load testing tools: JMeter, Gatling, and k6 [cite 1]" >}}

A consideration when comparing local open source solutions is memory requirements. In Figure 2.1, the max traffic generation capability results, measured in requests per second (RPS), are based on each tool running on the same hardware. Max traffic generation capability is closely tied to memory usage. Generally, a higher number of requests per second correlates to lower memory usage. With such varying memory usage amounts it is important to understand the capabilities of an individual machine before performing tests.

### 2.2 Limitations of using open source solutions locally

The following section discusses a number of limitations shared by API load testing tools, using k6 as an example.

#### 2.2.1 CPU and Memory restrictions

The largest limitation when using an API load testing solution locally is that the amount of load generated is limited by the machine’s memory and CPU. Even though k6 is typically considered a more performant load testing tool, CPU and memory limitations remain a concern.

Research was conducted to test the hardware limits of two standard computers that could be used in a development setting. Figure 2.2 summarizes the observations of this research. It shows the output for two tests of 600 and 1000 virtual users running the same two minute test script on different machines. Computer 1 has 8GB RAM while computer 2 has 32 GB of RAM, both using an 8 core CPU.

{{< figure src="../assets/fig2_2.png" caption="Figure 2.2 A comparison of two computers with different compute resources simulating the same number of VUs but producing a different number of requests." >}}

Due to lack of resources, computer 1 simulated the same number of requests for both 600 and 1000 virtual users. For this computer, the maximum number of VUs that can be simulated is 600. When this test was performed on computer 2, a machine with quadruple the memory, the total requests increased in proportion to the VUs simulated. In this case, the only way for computer 1 to simulate more VUs is to upgrade the machine’s memory.

The amount of CPU needed depends on the test script. The larger the test, the more CPU power is needed. If the chosen load testing tool uses too many CPU resources to generate load, there will not be enough resources left to adequately measure the responses received and result metrics will likely be reported incorrectly.

The amount of memory needed depends on the defined scenarios for a load test. The form in which result data is collected and output also impacts memory requirements. A simple k6 test will use around 1 to 5MB of memory per virtual user. This means for a simulation of 1000 virtual users, the memory consumed would range from 1 to 5GB. Local load testing tools, such as k6, can be used effectively for tests of a short to moderate duration and with a limited number of VUs. Anything beyond this limit requires additional non-trivial configuration and significantly increased CPU and memory.

#### 2.2.2 Lack of access to results as test is running

Another limitation in using local load testing tools is the lack of available results while the test is running. While this may not pose much of a problem in a short test of just a few minutes, when running longer tests there is little to no information about how the API is performing. Until the test completes and a summary view is output to the console, the developer can neither derive actionable insights nor cancel and restart a test based on real-time results.

{{< figure src="../assets/k6_run.gif" caption="Figure 2.3 Local k6 test running and completing from the CLI." >}}

#### 2.2.3 No graphical representation of results after test completes

The open source version of k6, like many other local load testing tools, provides no default graphical representation of test results. The user may specify an output file, in JSON or CSV format, to capture the more granular results of their tests as seen in Figure 2.4.

{{< figure src="../assets/k6_csv_results.png" caption="Figure 2.4 CSV k6 test result output, rows 12-48361 are hidden." >}}

Figure 2.4 CSV k6 test result output, rows 12-48361 are hidden

However, the user will still need to come up with a way to process and parse that data and visualize it in a meaningful way.

### 2.3 Building a custom solution around an open source tool

Given the limitations described in the preceding sections, a company may consider building a custom solution around an open source tool. As the application and testing framework grows, so should the load testing solution. Building a scalable load testing application is not an easy undertaking, however; along with committing resources for long-term maintenance, there are known risks with developing in-house tools that are not mission-critical.

{{< figure src="../assets/22_inhouse-table.png" caption="Figure 2.5 In-house solution features" >}}

Additionally, technical debt and project delays can be expected due to the lack of domain expertise and effort spent on building a reliable tool. Assuming these challenges can be surmounted, the user is still at risk of reinventing the wheel unless a highly customized solution is a necessity. The biggest benefit of building a load testing solution from scratch is flexibility of implementation, which the user pays for by managing several unknowns.

These challenges are usually enough to convince developer teams to search for pre-packaged solutions, which are available in the form of cloud-based tools.

### 2.4 Cloud-based load testing solutions

Cloud-based SaaS load testing solutions are well-established and provide the sought-after traits of reliability and scalability. They abstract away much of the complexity required to automate tests, scale clusters, and produce visual results of the test data. Many open source load testing solutions also offer enterprise-level software for performing large-scale tests from the cloud. BlazeMeter, Gatling Enterprise and k6 Cloud are the cloud-based equivalents of JMeter, Gatling, and k6, respectively. These solutions address a number of the previously mentioned shortcomings of running tests locally. One such shortcoming is a lack of real-time results visualization. An example of how cloud based solutions deal with this issue can be seen in Figure 2.6 which shows a k6 cloud visualization dashboard.

{{< figure src="../assets/k6_cloud.png" caption="Figure 2.6 k6 Cloud test visualization" >}}

Cloud-based solutions provide real-time visualized results of the test. For some solutions this is a built-in feature, whereas other solutions require setup to output results to a third-party tool. This provides indication of any performance concerns as they occur during the test. Imagine running a 6 hour test without any insight into the results, only to find out that there was an unexpectedly high failure rate within the first thirty minutes. Without near real-time result visualization, a user could be wasting potential development time waiting for the tests to complete.

A major benefit of using a cloud-based solution is the ability to simulate a much larger number of virtual users than could be achieved with a local load testing solution. The cloud provider is responsible for provisioning resources based on testing needs.

{{< figure src="../assets/fig2_7.png" caption="Figure 2.7 Comparison of highest tier cloud-based solutions" >}}

Figure 2.7 is an example of the highest tier monthly subscription offered by the aforementioned cloud providers. Many of these solutions also provide a separate breakdown for individual endpoints within a given scenario. Other features such as scheduling tests, generating reports, and providing performance insights are common amongst vendors in this space. Although the number of virtual users, test duration and data storage differs between solutions, they share a common subscription model that locks users into the tier’s offerings.

### 2.5 Limitations with existing cloud solutions

The following section discusses a number of limitations shared by cloud-based load testing tools, using k6 cloud as an example.

#### 2.5.1 Subscription model restrictions

Cloud-based load testing tools typically require a subscription that imposes limitations as a function of that chosen tier.

{{< figure src="../assets/21_k6cloud-tiers-table.png" caption="Figure 2.8 k6 Cloud subscription tiers" >}}

As seen in Figure 2.8, if the user were subscribed to Tier 1 and wanted to run a continuous 30 minute test, they would need to upgrade to the next tier or settle for the 15 minute limit. If they wanted to simulate 2000 virtual users, Tier 3 would be the only option even if the test lasted 15 minutes or less. Although the user’s testing needs span multiple tiers, they would be forced to choose the least restrictive option and could possibly be overpaying for functionality they do not need.

The subscription-based model these companies use locks users into only simulating a certain amount of virtual users, restricts them into tests of a certain length, and limits the amount of total tests performed over a given time period. Large companies do not have to choose; they can simply select the most expensive option as long as it gets the job done. Small and medium-sized companies do not have this luxury; they must make a choice between testing needs and budget.

{{< figure src="../assets/23_saas-table.png" caption="Figure 2.9 Software-as-a-service (SaaS) vendor features" >}}

Cloud-based SaaS tools can be both costly and restrictive, providing unnecessary functionality of features while limiting testing usage.

## 3. Use case {#use_case}

Using a load testing tool locally is adequate for simulating a small amount of load, and when simple, summary reporting at the end of the test is sufficient. When larger scale tests need to be performed and a more thorough result visualization is needed, cloud-based SaaS solutions can meet that need. These solutions also offer additional features, but introduce testing limitations as well. If a user is only interested in overcoming the load and visualization limitations of a local tool, and does not have a need for additional features, then a cloud-based SaaS solution may not be the right fit.

Artemis allows users to overcome the load generation limitations of a single machine while providing access to near real-time results visualization. It does so while retaining the testing flexibility that a SaaS subscription tier may restrict.

### 3.1 Where Artemis fits in

<p id="gdcalert14" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image14.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert15">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>

![alt_text](images/image14.png 'image_tooltip')

Figure 3.1. Artemis’ solution features

Artemis deploys the necessary infrastructure for performing API load tests to a user’s AWS account with a single CLI command. Users can run load tests without restrictions on the number of virtual users and the test duration. Aggregated test results can be visualized in near real-time with the provided dashboard, and test results are retained in long-term storage, allowing the user to further parse, transform or query the data as desired.

Artemis is not as feature-rich as some of the existing cloud solutions. It does not perform parallel tests or generate load across multiple regions. There is a limit of 20,000 virtual users that can be simulated. The results dashboard displays a limited set of metrics and scheduled tests are not a built-in feature.

However, Artemis is a flexible, scalable solution for users who are unable to build out a dedicated testing environment and for whom tiered cloud-based SaaS solutions are too restrictive or costly.

## 4. Introducing Artemis {#introducing_artemis}

### 4.1 Four components

<p id="gdcalert15" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image15.gif). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert16">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>

![alt_text](images/image15.gif 'image_tooltip')

Figure 4.1. Artemis’ Architecture

Artemis’ architecture can be divided up into four different components: load generation, aggregation, data storage, and result visualization.

- The first component is concerned with load generation. The load testing containers in this component simulate virtual users that perform requests against the API the user wishes to test.
- The second component is concerned with the aggregation of the test results. A single container collects and processes all the test results generated by the load testing containers.
- The third component is concerned with data storage. The aggregated test results are stored in a time-series database.
- The fourth and final component is concerned with result visualization. The provided dashboard allows the user to visualize meaningful metrics generated by their load test.

### 4.2 Deploying Artemis

Prior to load generation, the supporting infrastructure needs to be in place.

By using the command **artemis deploy**, the necessary components can be deployed to AWS. This command makes use of the AWS Cloud Development Kit (CDK), which is a framework for modeling cloud infrastructure, via code, that can then be synthesized and deployed to a user’s AWS environment. Artemis deploys serverless components, launched on demand, meaning the user can run load tests without having to manage their own servers.

Deploying the infrastructure involves compiling the CDK code into a template which can then be used by the CloudFormation service to create the specified resources.

AWS CDK is not the only way to build out infrastructure on AWS. As mentioned previously, the CDK code must be translated into a CloudFormation template, which is a JSON or YAML file that indicates the resources the user wants to deploy and how they want them to be configured. The advantages of using the CDK for Artemis’ use case, instead of constructing a CloudFormation template directly, are many:

- It’s easier to get started with the CDK since it’s available in many commonly used programming languages.
- The CDK documentation is easier to parse and the API reference is much more detailed compared to that of CloudFormation.
- The CDK is an abstraction over CloudFormation, hence the code tends to be more concise. Artemis’ CDK code was 429 lines long, but the CloudFormation template spanned 1455 lines.

### 4.3 Starting a test with Artemis

<p id="gdcalert16" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image16.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert17">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>

![alt_text](images/image16.png 'image_tooltip')

Figure 4.2. Each task running the test gets a copy of the test script from S3

Having deployed Artemis’ infrastructure, the user can utilize the Artemis CLI to initiate a load test. The user provides two inputs: the local file path to the test script and the number of load testing containers to spin up. The start command, **artemis run-test**, triggers an action causing the script to be uploaded to an S3 bucket and invokes a lambda function to spin up the total number of load testing containers specified by the user.

S3 is AWS’ object storage service. Object storage provides a way to store unstructured data in a structurally flat data environment. There are no folders, directories, or complex hierarchies as in a file-based system. While cloud-based object storage is ideal for long-term data retention, here it was used as a means to an end; namely, as a simple way to copy a test script on a user’s machine to a resource that could be accessed by Artemis’ load testing containers.

Since local files can’t be read by a Lambda function, S3 serves as a stepping stone for getting the test script onto the AWS infrastructure. AWS Lambdas fall in the realm of Functions-as-a-Service. Functions-as-a-Service implementations make functions, or chunks of code, available on demand to respond to events as they occur. The Functions-as-a-Service provider manages the underlying infrastructure and the user is charged by execution time. In Artemis’ case, the Start Test lambda pictured in Figure 4.2 spins up the specified number of containers. Each application in the container then creates a local copy of the test script and starts the test.

## 5. Using Artemis {#using_artemis}

Artemis’ infrastructure is accessible to the user through the built-in admin dashboard and CLI. This enables the user to run tests, visualize results, and manage, deploy and teardown their load testing infrastructure in AWS.

### 5.1 artemis deploy

Deploying the infrastructure can be performed using **artemis deploy**. The user will receive confirmation in the terminal that the infrastructure was successfully deployed. A user would typically run this command once upon first using Artemis and before running any load tests.

<p id="gdcalert17" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image17.gif). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert18">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>

![alt_text](images/image17.gif 'image_tooltip')

Figure 5.1 Demo of the artemis deploy command running in the CLI

### 5.2 artemis teardown

When load testing has concluded, the infrastructure can be removed using **artemis teardown**. All components, with the exception of the database, will be removed from the user’s AWS infrastructure. A separate command, **artemis destroy-db**, is available if a user wishes to delete the database storing Artemis’ results.

###

<p id="gdcalert18" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image18.gif). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert19">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>

![alt_text](images/image18.gif 'image_tooltip')

Figure 5.2 Demo of artemis teardown command running in the CLI

### 5.3 artemis run-test

To initiate a test through the Artemis CLI, the user executes **artemis run-test** and specifies the path to the test script and the number of containers to spin up to generate the desired load. The user is presented with a three minute spinner that represents Artemis’ timestamp coordination function. This function ensures that the user’s test, across all containers, starts at the same time. This functionality will be explained in greater depth in the next section.

After three minutes have elapsed, the user is presented with confirmation that the test has started along with a unique ID assigned to the test. This ID will be present in every row of data in the database such that the data can be grouped by test when queried.

<p id="gdcalert19" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image19.gif). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert20">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>

![alt_text](images/image19.gif 'image_tooltip')

Figure 5.3 Demo of the artemis run-test command running in the CLI

Now that the test is running, the user can expect their test results to be stored in the database and may choose to launch a container running the Artemis Grafana dashboard.

### 5.4 artemis grafana-start

To use Artemis’ Grafana dashboard, the user runs **artemis grafana-start** from the CLI. Once the container is up and running, the user will receive an IP address which they can access to view the dashboard.

<p id="gdcalert20" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image20.gif). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert21">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>

![alt_text](images/image20.gif 'image_tooltip')

Figure 5.4 Demo of the artemis grafana-start command running in the CLI

Artemis’ default dashboard view displays the results of the most recent test, and the user can zoom into specific portions of the test by highlighting the areas of interest. A user is not confined to the provided dashboard and queries. They may add their own panels to the dashboard and define their own custom queries.

<p id="gdcalert21" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image21.gif). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert22">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>

![alt_text](images/image21.gif 'image_tooltip')

Figure 5.5 Demo of user interacting with the Artemis Grafana dashboard

When a user no longer needs to use the dashboard, they can run **artemis grafana-stop** to terminate the container.

### 5.5 artemis sleep

Artemis also provides the command **artemis sleep** to stop both the Telegraf and Grafana containers. The user can think of this as ensuring their infrastructure is in a “low power mode”. This command is useful when a user doesn’t want to teardown their infrastructure but is finished testing for the moment and wants to make sure that no containers are running in the background to prevent incurring unnecessary AWS charges.

<p id="gdcalert22" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image22.gif). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert23">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>

![alt_text](images/image22.gif 'image_tooltip')

    Figure 5.6 Demo of the artemis sleep command running in the CLI

### 5.6 artemis admin-dashboard

To launch the admin dashboard the user runs **artemis admin-dashboard**. This provides the Artemis user a visual interface to run all the previously mentioned CLI commands.

<p id="gdcalert23" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image23.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert24">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>

![alt_text](images/image23.png 'image_tooltip')

Figure 5.7 Artemis admin dashboard

This dashboard uses a React frontend to make API calls to endpoints defined in an Express.js backend to execute a CLI command on a button click.

## 6. Design Decisions {#design_decisions}

When building Artemis’ architecture, a number of options were considered in order to achieve the goal of a scalable and flexible solution. This section explores the design decisions and tradeoffs that were made while building Artemis.

### 6.1 Load testing tool

Artemis required the use of a load testing tool to create scenarios and generate the number of virtual users specified by those scenarios.

There were two characteristics of interest while evaluating different load testing tools for Artemis:

- User is able to write a test script in a ubiquitous language
- Tool generates adequate load using minimal resources

JMeter, Gatling, and k6 were the three options considered. All of these tools allow for API load testing, but differ when it comes to scripting vs non-scripting, language used to write tests, and resources used to generate load.

While JMeter performs protocol-based load testing, test scripts are created in a GUI, making JMeter an unsuitable choice. Artemis needed a script-based tool to control functionality from the CLI. Additionally, the use case is aimed towards developers that would already be familiar with writing tests in code and using the CLI.

Gatling was closer to the required load testing tool as it is a script-based, non-GUI solution. However, to use Gatling, engineers would have to learn a domain specific language (DSL). Engineers at a small or medium-sized company may not have the time to learn another language. It was prefered to use a widely used language, rather than a DSL, to fit the use case of being easy to deploy and use for web developers.

k6 aligned with this goal as test scripts are written in JavaScript. k6 itself is written in Go, which is built with performance in mind; its low memory footprint means k6 provides the capability of producing more load while using the same resources, compared to JMeter and Gatling. This made k6 a suitable choice to integrate into Artemis’ architecture..

### 6.2 Load generation

Generating load at scale requires compute resources beyond what a local environment or single virtual machine can provide. Artemis’ solution coordinates the running of multiple containers within a Virtual Private Cloud. These containers are shown as tasks in Figure 6.1.

<p id="gdcalert24" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image24.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert25">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>

![alt_text](images/image24.png 'image_tooltip')

Figure 6.1 Load generation component of Artemis’ architecture

A Virtual Private Cloud, or VPC, is a secure, logically isolated portion of the AWS infrastructure where the user can deploy their own resources. These tasks generate virtual users that perform requests against the API the user wishes to test. In the next section, the elements that make up Artemis’ load testing containers will be examined.

#### 6.2.1 Running load tests in the cloud

Each of the load testing instances pulls a custom image from AWS’s public container repository, known as Elastic Container Registry or ECR. The custom image was configured and published to ECR. It includes k6 and the artemis.js Node application.

<p id="gdcalert25" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image25.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert26">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>

![alt_text](images/image25.png 'image_tooltip')

Figure 6.2 The internals of a single load testing container

The Node script, **artemis.js**, fetches the previously uploaded test script from the S3 bucket, waits for a prescribed period for test start coordination, then uses k6 to run the test based on the provided script. The test results are then sent to a separate container for aggregation.

During a regular load test, a user may be spinning up one or more of these containers. Where are these containers running and how is the scaling of these containers handled? The answer is AWS Fargate and AWS Elastic Container Service on a VPC.

AWS Fargate is a serverless compute engine for containers. Fargate’s serverless nature aligned with the goal of making Artemis serverless for its users. Artemis users can simply write load tests without the burden of determining the optimal compute power needed to run the tests.

Lambdas, and more generally Functions-as-a-Service, are also considered serverless. However, Lambda functions proved to be too restrictive since they can only be used for tasks that run for 15 minutes or less. This function timeout imposes a test duration limit on Artemis users, which was not acceptable for Artemis’ use case.

All of Artemis’ containerized applications run in Fargate instances. ECS allows Artemis to manage, deploy, and scale the containerized applications provisioned by Fargate. The number of instances specified by the user when running a test is used by ECS to determine the number of Fargate instances to spin up.

#### 6.2.2 Challenge: Determining container size and number of VUs per container

One challenge when utilizing AWS Fargate instances is determining the best combination of CPU and memory for a specific use case. After testing various sizes of Fargate instances and analyzing Cloudwatch logs and graphical results for CPU and memory utilization, it was determined that the optimal number of Virtual Users per load testing container was 200. This gives the user flexibility in terms of overall total VUs and length of test duration. Artemis’ users write their test scripts for up to 200 VUs and then specify the number of tasks desired for a particular test run. With this information, Artemis can generate load across multiple containers to achieve a large total number of Virtual Users. For example, if a user wants to test for 5,000 VUs, they simply write their script based on 200 VUs and specify a task count of 25. Likewise, if a user wants to perform a load test that simulates 200 or fewer users, they can use the same test script and specify just one task.

<p id="gdcalert26" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image26.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert27">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>

![alt_text](images/image26.png 'image_tooltip')

Figure 6.3 Scaling of Fargate instances for an increasing number of virtual users

Although ECS and Fargate made it easy to spin up serverless instances, a way to coordinate the starting of load tests across these multiple instances remained a problem. Ideally, all virtual users should be executing requests at the same time to accurately simulate the desired load. Otherwise, test results would not accurately reflect the test input and therefore invalidate the test itself.

#### 6.2.3 Challenge: Starting the same test across multiple containers at the same time

####

<p id="gdcalert27" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image27.gif). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert28">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>

![alt_text](images/image27.gif 'image_tooltip')

Figure 6.4 Tests start as soon as the container is spun up, leading to inaccurate results

The question to answer then became: How can the spinning up of instances be decoupled from the starting of a test?

AWS Step Functions was first considered to coordinate a series of lambdas for running tasks concurrently. AWS has a Distributed Load Testing Implementation Guide that makes use of Step Functions using worker tasks and a leader task. This required the leader task to communicate with each of the worker tasks using a Python script to create a socket on the workers to listen for a start message from the leader. This workflow went beyond what is described here to start multiple tests and would introduce unnecessary complexity to Artemis’ use case. Another option, and how the problem was ultimately addressed, was by introducing a delay to the running of the test script.

<p id="gdcalert28" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image28.gif). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert29">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>

![alt_text](images/image28.gif 'image_tooltip')

Figure 6.5 Test start at the same time, irrespective of when the container has spun up

This was achieved by generating a timestamp three minutes from when the Start Test Lambda is invoked. Each test container then calculates a specific wait time based on when it is created, using the initial timestamp as a reference. This wait time can be thought of as a container-specific timer, enabling test start synchronization.

### 6.3 Data aggregation

<p id="gdcalert29" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image29.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert30">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>

![alt_text](images/image29.png 'image_tooltip')

Figure 6.6 A high level overview of Artemis’ data aggregation component.

A single container collects and processes all the test results generated by the load testing containers. In order to provide meaningful insights into the performance of the API, the metrics displayed to the user should reflect the load testing results across all containers.

#### 6.3.1 Why aggregation is necessary

Generating load from multiple containers creates a problem in that the results generated by each container do not represent the load test as a whole. Instead, it represents the performance of the API from the viewpoint of a single container.

<p id="gdcalert30" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image30.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert31">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>

![alt_text](images/image30.png 'image_tooltip')

Figure 6.7 A generic data aggregation workflow

Aggregation of data was also necessary in providing meaningful results. Plotting too many data points would produce too much noise, inhibiting the user from understanding and interpreting the data correctly.

Data aggregation reduces the number of writes since the amount of test results are reduced before they reach the database. Reads also go down, since Artemis’ Grafana dashboard does not need to query as many data points to display results.

A solution was needed that would allow Artemis to funnel test results from all containers into a central location, where the aggregation of said results could proceed. Two approaches were considered to solve this problem.

#### 6.3.2 Challenge: Aggregating data from test results

First, a polling-based approach was examined. In this approach, k6 would perform the load test from the test containers and the results would be written to a file in either JSON or CSV format. Artemis would then tail the results file, sending the gathered results to be aggregated.

<p id="gdcalert31" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image31.gif). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert32">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>

![alt_text](images/image31.gif 'image_tooltip')

Figure 6.8 - Polling test containers for results

An issue with this approach is that k6 consumes a large amount of memory when test results are written to a file. Every virtual user simulated by k6 stores a copy of its individual test result file in memory. This presented a problem because larger tests depleted the container memory prematurely and terminated the container. Even if k6 did not have this memory issue, the challenge of aggregation remained unsolved.

<p id="gdcalert32" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image32.gif). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert33">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>

![alt_text](images/image32.gif 'image_tooltip')

Figure 6.9 - Streaming-based approach using Telegraf

A streaming-based approach was then considered. Instead of bi-directional communication, a streaming approach entails a one-way data flow in near real-time. Each individual load test container sends data, as it is generated, to a single server. The server is configured to combine the results received at a regular time interval. After evaluating a number of data aggregation solutions, a tool called Telegraf was determined to be the right fit for Artemis.

<p id="gdcalert33" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image33.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert34">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>

![alt_text](images/image33.png 'image_tooltip')

Figure 6.10 - Example of Telegraf aggregating data points

Telegraf is an open source data collection agent that is written in Go. It has no external dependencies and has minimal memory requirements. Telegraf includes dozens of built-in plugins that the user can activate as needed for their specific use case. These plugins provide a wide range of input, processing, aggregation and output functionality.

Telegraf has the ability to receive data in **line protocol** **format** making it compatible with k6, which is able to output data in the same format through a built-in plugin. Telegraf’s BasicStats and Quantile plugins calculate the mean, min, max, count, and sum, as well as percentile values, of all the incoming data. These metrics are important for accurately representing the test results as a whole.

Telegraf was chosen since it permitted Artemis to stream the data from every single container running a k6 load test to a central location. The Telegraf container was configured to aggregate the incoming data every ten seconds, and then insert it into a database for long-term storage. A ten second window was determined to substantially reduce the amount of raw data that needed to be stored while retaining meaningful insights in the test results.

#### 6.3.3 Challenge: Container communication

Having determined that Telegraf was Artemis’ best option for addressing the problem of data aggregation, the next problem to tackle was how to make the Telegraf container reachable by the numerous test containers executing **artemis.js** and running the user’s tests with k6.

While all of Artemis’ containers run on a single VPC, the serverless nature of the implementation means that container instances get spun up and torn down on demand and the private and public IP addresses of the Telegraf container change whenever a new instance of Telegraf is created. A straightforward way for the current instance of the Telegraf container to be reachable by all of the test containers was needed.

<p id="gdcalert34" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image34.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert35">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>

![alt_text](images/image34.png 'image_tooltip')

Figure 6.11 - Setting and receiving an IP address from Route 53

This problem was solved by running the Telegraf container as a service within the ECS cluster and enabling **service discovery**. This allows for the ephemeral nature of the container to be maintained while providing a consistent service discovery endpoint for the test containers to connect to when sending results. If a user runs a test and a Telegraf instance is not running within the service, Artemis starts a new Telegraf container along with the test containers. Whenever a new Telegraf instance within that service is launched, ECS uses AWS CloudMap to update Route 53, which is AWS’ DNS service, with the IP address of the Telegraf container within the VPC. Route 53 then maps that address to the DNS of the service Artemis defined, so when the test containers send data to the endpoint [http://artemis-telegraf.artemis:8186](http://artemis-telegraf.artemis:8186), it is correctly routed to the currently running instance of Telegraf.

### 6.4 Data storage

<p id="gdcalert35" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image35.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert36">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>

![alt_text](images/image35.png 'image_tooltip')

Figure 6.12 - High level overview highlighting the data storage component

The next problem to consider was how to best store data long-term for historical monitoring purposes while retaining the scalable and serverless quality of Artemis’ architecture.

#### 6.4.1 Why store all this data?

Cloud-based SaaS load testing solutions typically offer one to six months of data retention. Early on, it was decided that Artemis would not impose any data retention restrictions to allow for better historical data analysis.

Storing data long-term allows the user to answer several important questions when an anomaly arises: has this issue happened before? And if it has, when did it first start and why? Being able to search over months or years of historical data can be a huge advantage and may allow the user to more easily answer these questions. Additionally, the knowledge gained from historical data can provide the user with greater confidence in their system.

#### 6.4.2 Time-series databases

k6, the load testing tool at the center of Artemis, outputs test results as time-series data. **Time-series data** is a sequence of data points collected over time intervals, giving the user the ability to track changes over time. Time-series data can track changes over milliseconds, days, or even years. This type of data accumulates very quickly and normal databases are not designed to handle the large amounts of necessary writes.

k6 also recognizes how large amounts of data introduce an additional challenge: "...a large load test will likely produce a massive amount of data points. On the storage side, the database will likely become another bottleneck…In this case, [a] team must choose a high-performing database, optimizing for this use case, and typically aggregating the data before storing it." [cite 2]

The two main benefits that time-series databases provide over traditional databases are usability and scale. Usability refers to any features, like continuous queries, that enable the user to more easily perform data analysis. Time-series databases are able to offer greater scalability since they treat time as a first-class citizen. Treating time as a first-class citizen introduces efficiencies such as higher data ingest rates and faster queries at scale.

#### 6.4.3 Prometheus as a data storage solution

First, Prometheus was considered as a potential data storage solution. Prometheus is an open source, metrics-based monitoring system and time-series database. The user is able to query data from Prometheus using its own custom, independent query language, PromQL. PromQL has no overlap with other query languages used in time-series databases, hence the user would need to learn a new SQL-like syntax to utilize this solution.

k6 supports sending test result metrics to a Prometheus remote write endpoint through the use of an extension. To maintain Artemis’ serverless nature, the AWS Managed Prometheus service (AMP) would need to be used. AMP is a serverless solution where cost is based on the metrics ingested, stored or queried. Otherwise, using Prometheus on AWS would require the user to manage an EC2 instance and thus not qualify as serverless.

The envisioned infrastructure would have a load testing container, running k6, streaming the test result data directly to the AMP remote write endpoint. Data aggregation was not considered at this point. This remote write endpoint would be provided by AMP immediately after creating a Prometheus instance. An issue with this approach is that AMP, as well as other AWS services, requires requests sent over HTTP to provide AWS authentication information through the SigV4, or Signature Version 4, signing process. Since k6 lacks the ability to do this, ways in which requests could be signed before they were sent to AMP were evaluated.

A potential solution involved running a sidecar container which would run AWS SigV4 Proxy. AWS SigV4 Proxy is an open source project that looks for AWS SDK credentials in three different areas: environment variables, shared credentials file, or the IAM role for the ECS task role. Requests would be routed from k6 to the proxy, the proxy would sign these requests using the credentials found, and then the requests would be forwarded to AWS AMP. While this solution would allow AMP to accept the test results produced by k6, it would effectively double the amount of containers deployed per test. Doubling the containers would duplicate the cost per test as well as introduce more points of failure.

#### 6.4.4 Timestream

The next data storage solution that was examined was AWS Timestream. **AWS Timestream** is a scalable, serverless time-series database. Like AWS Managed Prometheus, Timestream's pricing is based on writes, queries, and storage. For Artemis’ use case, there were two qualities of Timestream that made it a better solution as the data storage component when compared to Prometheus: the query language used and the way data is stored.

Timestream uses SQL as its query language. Given the overall goal of making Artemis easy to use, enabling the user to query data in a widely used language like SQL was determined to be a better option than using a PromQL-based solution. Timestream stores data in both memory and magnetic stores. The in-memory store is used for recent data while the magnetic store is used for historical data. Timestream has the ability to automatically move data from the memory store to the magnetic store as it reaches a certain age. The magnetic store is a more cost-effective way of storing data; this is ideal for Artemis’ users since they might choose to retain their load test results over long periods of time.

This, combined with the fact that it integrates well with k6, Telegraf and Artemis’ overall architecture implementation, made Timestream the right choice for Artemis’ data storage component.

<p id="gdcalert36" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image36.gif). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert37">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>

![alt_text](images/image36.gif 'image_tooltip')

Figure 6.13 - Aggregated time-series data is inserted to Timestream database

### 6.5 Result Visualization

Artemis needed to provide the user with a means to make sense of the stored data in a way that was easy to understand. This leads to Artemis’ fourth and final component: result visualization.

<p id="gdcalert37" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image37.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert38">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>

![alt_text](images/image37.png 'image_tooltip')

Figure 6.14 - High level overview of the data visualization component

#### 6.5.1 Grafana dashboard

**Grafana** provides an open source solution for visualizing data over time via a customizable dashboard. It connects to several databases such as PostgreSQL, InfluxDB, MySQL, and AWS Timestream and allows the user to decide on what information to display by associating a data source and one or more database queries with a panel. A dashboard can be composed of several user-customized panels. At any point during the creation process, a dashboard’s configuration can be preserved as a JSON file and re-used as a template for other dashboards.

<p id="gdcalert38" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image38.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert39">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>

![alt_text](images/image38.png 'image_tooltip')

Figure 6.15 - A panel (top) and the associated data source and database query (bottom) that informs the graph displayed by the panel.

Grafana was chosen for its modular, easy-to-use dashboard that allows for data to be visualized in different ways. Grafana was connected to Amazon Timestream via a plugin available from Grafana Labs. Although Artemis provides a pre-configured dashboard view, the user has the flexibility to display additional metrics that they deem important.

As demonstrated in section 5.4, the Artemis CLI can be used to spin up a Grafana container that will automatically query the Timestream database. This will allow the user to visualize the results of their tests. Results are updated every ten seconds as the tests are running.

The Artemis Grafana dashboard includes custom database queries to display the following metrics in the dashboard: virtual users simulated, HTTP request duration, requests per second, and request errors. These metrics are often displayed in other load testing result visualizations and provide a baseline understanding of how the API is performing. For example, if the request rate (requests/second) does not correlate with the change in VUs, it could indicate a bottleneck.

The four summary panels shown at the top of the dashboard provide statistics to enable a user to see how their test is running at a glance.

Total requests per second shows the number of requests made every second to all endpoints in the test script. On its own, this metric does not tell the user much about the performance of the API. But when request duration increases, or errors start to appear, it lets the user know at what point their system starts to break.

<p id="gdcalert39" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image39.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert40">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>

![alt_text](images/image39.png 'image_tooltip')

Figure 6.16 - Top half of the Grafana dashboard showing the four summary panels and total requests per second.

Figure 6.16 is the associated query that obtains the data from Timestream to illustrate the total requests/second. It locates the rows that have a value of “value_sum” within the measure_name column of the http_reqs table. A single measure_value within a “value_sum” row indicates the sum of HTTP requests made across all containers over the Telegraf aggregation period. On line 1, the sum of the number of HTTP requests is divided by the 10 second period to determine an approximate number of requests per second.

<p id="gdcalert40" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image40.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert41">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>

![alt_text](images/image40.png 'image_tooltip')

Figure 6.17 - Timestream query for total requests/second panel.

HTTP Request Duration displays how long it takes to perform the entire request-response cycle. This metric is represented in four different ways: min, p90, p95, and max. The percentile values allow the user to determine the request duration that most users are experiencing. If these percentiles are complemented with minimum and maximum measurements, then it is possible to have a much more complete view of the data and better understand how the system behaved.

<p id="gdcalert41" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image41.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert42">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>

![alt_text](images/image41.png 'image_tooltip')

Figure 6.18 - Bottom half of the Grafana dashboard showing three panels: HTTP Request Duration, HTTP Failures, and Virtual Users

The HTTP Failures panel on the bottom left of Figure 6.18 displays dots that represent any response that returned with a status code in the 400 or 500 range. This allows the user to quickly determine how many errors occurred within a certain time period.

The bottom right panel, Virtual Users, displays the total number of users simulated across all containers.

## 7. Conclusion & Future Work {#future_work}

In summary, Artemis addresses many of the inherent limitations of existing load testing solutions. Artemis deploys a flexible, scalable load testing infrastructure that enables users to visualize and store near real-time results. This infrastructure is accessible by either Artemis’ CLI or graphical user interface.

Artemis is not feature-complete; there are some additional capabilities that could be incorporated in future versions:

- Artemis’ current infrastructure allows for testing up to 20,000 VUs. This limitation is a function of Artemis’ aggregation implementation. A way to scale this aggregation step to accommodate an even greater number of VUs should be explored.
- The implementation of an automatically generated “executive summary” of the results of each test that could be easily shared across the user’s team or organization would be a valuable addition.
- Currently, tests can be started on demand only. Functionality that allows users to schedule tests should be incorporated.

## 8. References {#references}

[1] [https://k6.io/blog/comparing-best-open-source-load-testing-tools/](https://k6.io/blog/comparing-best-open-source-load-testing-tools/) (local testing solution table)

[2] [https://k6.io/what-to-consider-when-building-or-buying-a-load-testing-solution/](https://k6.io/what-to-consider-when-building-or-buying-a-load-testing-solution/) (quote saying databases can be a bottleneck)

(more general references)

## Presentation {#presentation}

{{< youtube id="l-NEJnETKxs" title="Artemis Presentation" >}}

## Our Team {#our_team}
