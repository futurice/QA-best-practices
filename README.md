# QA-best-practises

1.   Purpose of this document
 
This is a summary of QA practices Futurice uses and recommends to be used. It is not supposed to be a detailed description and sometimes cannot fully be used of all tasks but as an overview of the most important QA processes and a list of good practices that should be used.
 
For the purpose of the clarity this document does not describe an error or incident process (i.e. a new bug is found from production).
2.   QA practices
 
Everybody in Futurice team has QA responsibilities, even if there is a named QA manager or QA specialists. To Futurice QA means three things.  
1)    The service runs as expected and is considered to be created with good practices. 
2)    The service can be easily and cost efficiently maintained and operated. 
3)    The service can be continuously improved and modified cost efficiently
 
On high level QA work and practices can be divided into two processes. 
1)    QA during agile sprints 
2)    QA during deployment process
 
Furthermore there are other QA actions. 
 
Test-Driven Development
Futurice uses TDD and ATDD (Acceptance Test Driven Development) methods when applicable.
 
Method forces the implementation to follow architecture and consider modules that are used. Implementation starts by writing the automated test first and then implementing the functionality to pass the test.
 
Pair programming
Futurice uses Pair programming method when applicable. This is very convenient way to share knowledge and experience about the project and software under development.
 
Code review

Reviewing the code helps other team members to get information of certain functionality and gives a possibility to give feedback to responsible person and also ensures knowledge sharing between team members.
 
Manual functional testing

Manual testing is mostly done using Exploratory testing methodology and found errors are either fixed right away or prioritised and recorded to task/story/error management tool. Exploring the app or service can be started right after something functional is “ready”.
 
Exploratory testing is a very powerful tool in end to end testing where the whole system is covered by testing. In the method tester goes beyond what can be defined in a test case, applies user-like thinking as well as tries to break the system by various error scenarios and is never “done” with testing. 
 
Non functional testing - localization, usability, performance/load testing

Around the functional requirements and testing there are usually non-functional requirements which can be tested applying  localisation, usability, performance and load testing. The needs and the tools are projects specific. 
For usability testing Futurice has a mobile usability lab for use. 
For Performance tests Futurice has used web based services like BrowserMob to name one. 
For Load testing LoadUI and J meter are actively in use to validate service capabilities in high traffic and to find possible service bottlenecks.
 
Error management

Issues found are recorded to a specific tool or board with a information like priority, environment (software and device information), steps to reproduce, expected result, time and date and a screenshot. 
Tools like Jira, Trello, PivotalTracker, Request Tracker are actively used also for error tracking.
 
 
2.1 QA in Sprints (Definition of Done)
 
One of the principles of agile is that the master code branch should always be potentially shippable. That means it should be always production quality. This is achieved by the following means:
 
1)    Each user story (or feature) is developed individually in their own feature branch. The purpose of this is to ensure that each update to master branch is at the same time 1) as small as possible and 2) potentially shippable, complete story.

2)    Before the feature branch can be merged to the master branch, it must pass a list of actions, requirements and practices called Definition of Done (DoD). This is defined together with the customer PO and the development team and can be modified during project should project needs change.
 
2.1.1   DoD example for a project
·       Manual regression test case (usually UI test cases) have been updated for the smoke / sanity checks (Test manager)
·       Automated unit test cases have been written (Developer)
·       Feature development is completed. Acceptance criteria of the story have been fulfilled (Developer)
·       Code review (both feature and test cases) is done by another developer (developer)
·       Functional test cases have been passed in Demo environment (Automated & manual)
·       The user story (feature) has been approved (Product Owner) *1
·       Integration tests done against stub/mockups, e2e when possible *2
·       Concept documentation updated (Product Owner) *3
·       All other documentation has been updated (Led by project manager)
·       All documentation changes have been reviewed and approved (customer / PO) *4
·       Feature branch merged successfully to Master branch (Developer)
·       Automated tests passed in Test environment (Updated and run automatically by CI)
·       Functional tests passed in test environment / Exploratory tests have been done (Led by test manager)
 
 
Following items in proposed DoD have been bolded due 
1)    Acceptance of the user story here means that the PO is happy with the design, UX and UI of a user story. In practice this means that once the user story is approved it can still have QA issues (performance, bugs etc). It should be noted that new POs, especially in an organization where agile methods have not been used before, have often found this responsibility quite heavy and challenging. Traditionally similar approvals are made during project steering groups. 
2)    Usually it’s very difficult to run these in Demo environment due to security policies / high overhead of maintaining this. TEST environment should be integrated with appropriate test environment 
3)    Similar to the first item. In traditional project methods this is usually done by Vendor’s PM after approvals have been made during project steering group. In agile concept documentation is normally not maintained at all. However as the concept documentation is most likely an acceptance criteria of this project it needs to be maintained to showcase the changes that we have made during the project. 
4)    Approval of the documentation should be part of DoD especially initially. Each organization has their own documentation practices and the purpose of this is to ensure that we start doing it “right” from the very beginning. 
 
 
2.2 Web / server specific QA during deployment process
 
The deployment process is a process how new release (or a version in master branch) is deployed to production. For this process there are three relevant environments.
 
1)   TEST. The test environment contains a single instance for the frontend. CI automatically updates the latest version of master branch here. The purpose of this environment is to test and validate that user stories are complete and systematically run regression testing (unit tests).
2)    QA. The QA environment should be as close as possible to the production environment. Therefore, it uses two instances for the frontend, connected to an Elastic Load Balancer (ELB). This is used prior to pushing updates to production environment, to run regression testing and acceptance testing by the customer.
3)    PROD. The production environment makes use of two or more frontend instances (behind an Elastic Load Balancer) to provide scalability and high availability of the public-facing website.
 
Continuous Integration (CI) server is used for automated tests and deploying code to environments. Automated tests are run always when any of these environments are updated.
 
2.2.1 Updating TEST environment
 
TEST environment is updated automatically by CI, whenever master branch has been updated. This means that automated test cases are also automatically run when master branch is updated.
 
TEST environment is the main testing server for Futurice. It is expected that any release that has been tested here is ready to be pushed to QA or Production (this depending of the terminology and integrations used in project). 
 
It is not necessary to run manual regression test cases, when updating TEST. Quite often time spent on exploratory testing is more beneficial. However, it is a good practice to run these tests at least once per Sprint.
 
2.2.2 Updating QA environment
 
QA environment should be used as an environment for acceptance tests, demos or any 3rd party testing or audits (security, localization, load, usability etc.). This is NOT Futurice’s primary testing server (TEST is). Update to QA are always agreed between the customer’s and Futurice’s PMs.
 
The main reason to have two testing environments (TEST and QA) is to comply to security and integration requirements. TEST allows Futurice to develop the service using agile principles. QA allows time and control for the  customer to run their current QA processes.
 
QA should not be updated unless the release has passed testing in TEST environment.
 
2.2.3 Updating PROD environment
 
PROD environment is the environment for the live site (production). 
 
Good practice is to deploy and run automated tests in QA at least, prior to pushing update to PROD. However at PROD major functionality needs to be checked after every deployment. 
 
It is recommended that the PO also have the right to push an update to PROD, without any testing in QA (if the version is passed in TEST). This is relevant where the update is very small or simple. 
 
Ultimately in agile development, when the service is in continuous development, the goal is to have a lot of small updates also to PROD. I.e. it is more important that the deployment process is lean and quick than bulletproof. It is considered more important to be able to make fixes quickly than to have a bug free service (obviously with DoD and automated testing the quality should be also high). Futurice does NOT recommend starting with this approach immediately. However, this should be the direction where both organizations want to go. 
 
 
2.3 Mobile application specific practices
 
Each mobile platform has its specific SDK, tool sets and Futurice’s best practices.
 
For Android 
https://github.com/futurice/android-best-practices
 
For iOS
https://github.com/futurice/ios-good-practices
 
For Windows Phone
https://github.com/futurice/windows-app-development-best-practices
 
 
2.3.1 Mobile native, web and hybrid app testing
 
Mobile platforms provide emulators within their SDK. 
 
During the implementation phase developer can use simulated / emulated devices to validate changes but nothing can beat the real device testing where the whole system from the touch screen and integrated memory to mobile processors is considered.
 
Mobile browsers can also be tested by cloud based services like Browserstack.
 
Test devices
Futurice has a good variety of different devices and operating system versions in the device library, ranging from basic phones to advanced tablets.
 
Data traffic heavy services

For heavy data traffic application testing and performance validation Futurice has been using Elisa test lab (Elisa is a Finnish mobile service provider). Within the laboratory environment different loads and speeds can be tested in controlled / isolated environment without a need to arrange expensive and not that effective field testing sessions.

Mobile backend

Usually mobile app connects to a backend service via mobile network. To get the most out of testing, the tester needs to have a full control of the backend where the mobile application is connected to in order to prepare for and execute different end to end  test scenarios.

2.3.2 Mobile app test releases

In mobile devices the test build installation can be done: 
Locally by cable using development SDK tools 
Controlled wirelessly via build distribution tools like TestFlight or HockeyApp. Releasing frameworks usually support crash log collection and version data. Dev, Alpha and Beta releases can also be provided in order to collect feedback before releasing the application to stores.
Google Play store has option to set up alpha and beta test groups where the specific app is available for the group members only
 
2.3.3 Analytics testing

In modern mobile applications analytics and data collection represent critical functionality which also needs testing.  Futurice considers this to be important part of functional testing. 

Due to fact that the mobile app update process differs from the web apps one, the analytics setup needs to get right from the first try or then valuable data is missed. Update process is done via application store procedures, but whether the update will be done or not is up to the user settings and activity. So if the first version misses some critical analytics functionality and some users never update the application, that data will then be missed.
The solution may be forced upgrade, where the app becomes unusable until the user updates to the newest version available but that might be too late.

