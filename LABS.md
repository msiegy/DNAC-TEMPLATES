# [DNAC Template LABS](https://github.com/kebaldwi/DNAC-TEMPLATES/tree/master/LABS#dnac-template-labs-)

This section built out in a lab format to guide you through the typical steps to complete various automation tasks delivered by Cisco Catalyst Center. It allows for customers to practice Cisco Catalyst Center workflows with Onboarding, DayN Templates, and Application Policy automation on both Wired and Wireless Platforms, while reducing the time and effort needed to instantiate the network The lab will also introduce advanced velocity templating topics and troubleshooting tools, which may help determine common failure scenarios in a deployment.

> **Note:** Thank you for continuing your **support** of the **DNAC-Template Repository** by your continued visits to the site. Over the past few years we have developed a lot of content as a result, it is **time to reorganize** it to help be more consise, and to aide users in finding the relevant content easily. Over the next few months you will **notice changes and updates** to the repository, as we **add** additional content, **revise** existing dated content and nomenclature. The **folder structure** and **resources** will be **reorganized** for ease of use, and so that CODE examples and TUTORIALS will be grouped together for ease of use. We hope this will help users to find the content they need in an easier manner.

## New Catalyst Center Lab Content

This newer and more modular lab approach is designed to deal with and includes concepts from the legacy labs in a newer more modular format.

1. [Lab 1 Wired Automation](https://github.com/kebaldwi/DNAC-TEMPLATES/tree/master/LABS/LAB-1-Wired-Automation) - Covers green and brown field use cases **(allow 4.0 hrs)**
2. [Lab 2 Wireless Automation](https://github.com/kebaldwi/DNAC-TEMPLATES/tree/master/LABS/Lab-2-Wireless-Automation) - Covers traditional wireless automation  **(allow 4.0 hrs)**

## Legacy Lab Content

In this section you will continue to find all the existing labs which deal with specifics in separate easy to do labs.

* [PnP Preparation](https://github.com/kebaldwi/DNAC-TEMPLATES/blob/master/LABS/LAB-A-PNP-PREP/) - The lab covers setup for Plug and Play **(allow 1.5 hrs)**
* [Onboarding Templates](https://github.com/kebaldwi/DNAC-TEMPLATES/blob/master/LABS/LAB-B-Onboarding-Template/) - The lab covers in depth topics in deploying Day 0 templates **(allow 1.5 hrs)**
* [Day N Templates](https://github.com/kebaldwi/DNAC-TEMPLATES/blob/master/LABS/LAB-C-DayN-Template/) - The lab covers Day N template constructs and use cases **(allow 0.5 hrs)**
* [Composite Templates](https://github.com/kebaldwi/DNAC-TEMPLATES/blob/master/LABS/LAB-D-Composite-Template/) - This lab covers building a composite template on Cisco Catalyst Center **(allow 0.5 hrs)**
* [Application Policys](https://github.com/kebaldwi/DNAC-TEMPLATES/tree/master/LABS/LAB-E-Application-Policy/) - This lab covers Application Policy & SDAVC in Cisco Catalyst Center **(allow 1.0 hrs)**
* [Telemetry](https://github.com/kebaldwi/DNAC-TEMPLATES/tree/master/LABS/LAB-F-Telemetry-Enablement/) - This lab explains how to deploy Streaming Telemetry for Cisco Catalyst Center Assurance **(allow 0.5 hrs)**
* [Advanced Automation](https://github.com/kebaldwi/DNAC-TEMPLATES/tree/master/LABS/LAB-G-Advanced-Automation/) - This lab will explore Advanced Automation examples **(allow 1.5 hrs)**
* [Dynamic Automation](https://github.com/kebaldwi/DNAC-TEMPLATES/tree/master/LABS/LAB-H-Dynamic-Automation/) - This lab will explore additional Advanced Automation examples **(allow 2.0 hrs)**
* [Rest-API Orchestration](https://github.com/kebaldwi/DNAC-TEMPLATES/tree/master/LABS/LAB-I-Rest-API-Orchestration/) - This lab uses [Postman](https://www.postman.com) Collections to automate Cisco Catalyst Center **(allow 2.0 hrs)**
* [Wireless Automation](https://github.com/kebaldwi/DNAC-TEMPLATES/tree/master/LABS/LAB-J-Wireless-Automation/) - This lab covers Traditional Wireless Automation  **(allow 6.0 hrs)**
* [Orchestration](https://github.com/kebaldwi/DNAC-TEMPLATES/tree/master/LABS/LAB-K-Orchestration/) - This lab covers [Postman](https://www.postman.com) and [Ansible](https://www.ansible.com) orchestration **(allow 4.0 hrs)**
* [CICD Orchestration](https://github.com/kebaldwi/DNAC-TEMPLATES/tree/master/LABS/LAB-L-CICD-Orchestration/) This lab covers [Python](https://www.python.org), [Ansible](https://www.ansible.com) and [JENKINS](https://www.jenkins.io) to orchestrate via REST-API **(allow 4.0 hrs)**

## [Practical Template Examples](https://github.com/kebaldwi/DNAC-TEMPLATES/blob/master/TUTORIALS/ExamplesAndConcepts.md)

In this section I have compiled a number of template examples built with Jinja2 Language. These samples include a wide ranging set of configurations which may be used in your labs to solve specific configuration requirements. This collection summarizes aspects covered previously in this templating repository, and are aimed at providing quick practical references to help engineers solve automation tasks with the help of Cisco Catalyst Center. Additionally these examples may be used in testing in [dCloud](https://dcloud.cisco.com) with any of the labs on this repository.

* [Practical Template Examples](https://github.com/kebaldwi/DNAC-TEMPLATES/blob/master/TUTORIALS/ExamplesAndConcepts.md)

## DCLOUD as a LAB

### Overview

As a quick start with Cisco Catalyst Center Automation, you may utilize the above labs in conjuction with DCLOUD's sandbox:

1. [Cisco Enterprise Networks Hardware Sandbox West DC](https://dcloud2-sjc.cisco.com/content/catalogue?search=Enterprise%20Networks%20Hardware%20Sandbox&screenCommand=openFilterScreen)
2. [Cisco Enterprise Networks Hardware Sandbox East DC](https://dcloud2-rtp.cisco.com/content/catalogue?search=Enterprise%20Networks%20Hardware%20Sandbox&screenCommand=openFilterScreen)

This allows you to run these labs and gives not only an environment to try the various code samples, but also to develop and export your own code for use in your production environment. DCLOUD  provides for rapid and safe POC/POV on-demand environment without impacting production environments. DCLOUD also negates the need for shipping equipment, associated lead times, and licensing issues associated with setting up your own private testing environment. Please do adhere to the best practices for the DCLOUD environment when using it.

DCLOUD allows for use with a web-based browser client for VPN-less connectivity, as well as AnyConnect VPN client connectivity for those who prefer it. The labs are hosted in Cisco San Jose Facility (Select US East or US West Region when scheduling in DCLOUD). Choose the Cisco Enterprise Network Sandbox version you prefer. 

>**Note:** To access this or any other content, including demonstrations, labs, and training in DCLOUD please work with your Cisco Account team or Cisco Partner Account Team directly. Your Account teams will make sure the session is scheduled and shared for you to use. Once booked follow the guide within Github to complete the tasks adhering to the best practices of the DCLOUD environment.

> **Feedback:** If you found this repository please fill in comments and [**give feedback**](https://app.smartsheet.com/b/form/f75ce15c2053435283a025b1872257fe) on how it could be improved.
