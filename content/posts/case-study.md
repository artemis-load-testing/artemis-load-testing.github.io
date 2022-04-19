---
title: 'Artemis Case Study'
date: 2022-04-18T17:12:09-07:00
draft: false
---

## 0. Introduction

---

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

<table>
  <tr>
   <td>Options:
   </td>
   <td>
   </td>
  </tr>
  <tr>
   <td> <strong> </strong>-h, --help<strong> </strong>
   </td>
   <td> display help for command
   </td>
  </tr>
  <tr>
   <td>Commands:
   </td>
   <td>
   </td>
  </tr>
  <tr>
   <td>  run-test [options]
   </td>
   <td>Run the test script concurrently this number of times.
   </td>
  </tr>
  <tr>
   <td>  grafana-start
   </td>
   <td>Start the Artemis Grafana dashboard.
   </td>
  </tr>
  <tr>
   <td>  grafana-stop
   </td>
   <td>Stop the Artemis Grafana dashboard.
   </td>
  </tr>
  <tr>
   <td>  deploy
   </td>
   <td>Deploy Artemis infrastructure onto the user's AWS account.
   </td>
  </tr>
  <tr>
   <td>  sleep
   </td>
   <td>Stop all supporting container tasks for minimal AWS usage charges.
   </td>
  </tr>
  <tr>
   <td>  teardown
   </td>
   <td>Teardown Artemis infrastructure on user's AWS account, retain database.
   </td>
  </tr>
  <tr>
   <td><strong>  </strong>destroy-db
   </td>
   <td>Permanently delete the Artemis database.
   </td>
  </tr>
  <tr>
   <td><strong>  </strong>help [command]
   </td>
   <td>display help for command
   </td>
  </tr>
</table>

### **Login details for Artemis' Grafana dashboard:** \

username: artemis password: api_load_testing
