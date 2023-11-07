# Project Documentation/Report Outline

   - ### Title: Container Vulnerability Remediation
   - ### Author(s): Collin Edwards
   - ### Date: Sept-Nov 2023

### Introduction
According to weekly reports, a number of our applications--particularly those that run on older versions of Node and Java--possess a number of container vulnerabilities which must be patched out as per established security regulations. With the amount of pipeline changes and deployment challenges that accompany this type of work, I felt this was a good fit for the CICD module project.

### Requirements
- The solution will remove as many security vulnerabilities as feasibly possible
- The solution will include any necessary CICD changes
- The solution will maintain application functionality

### Planning
When the request was brought up to the team, we decided to divide the work by application, with 10 stories in total. I opted to take on the services I was most familiar with, which were initially 2 Java applications (later picked up another after team members were having difficulty with a particular issue).

### Process
Given the nature of this task, I felt it best to start with an application I developed myself. This would be act as an easy lead into the process of vulnerability patching without having to navigate through any unfamiliar architecture. This application--the caching client from my very first project--had among the least amount of vulnerabilities and, since it was relatively new, was exceptionally easy to complete. The Java container image was more or less up to date, and the only issues lay with a couple transitive dependencies. The next application, however, was a different story. This one happened to be a couple years old, and was running on numerous now-deprecated images and internal frameworks. The first step was to update the container image to use a much newer version. This necessary change in and of itself wasn't complicated (used the same Java version), but it resulted in basically every aspect of deployment and monitoring completely breaking. To resolve the monitoring capabilities, I had to make changes to the Dockerfile to expose the logs volume so an external logging service could have access. I was able to figure this out quickly, since I had run into a similar issue in the past. At the same time, due to some deadlines, we were required to onboard this application to a separate monitoring service. This ran into some issues as well, which were resolved by again modifying the Dockerfile to install some basic commands which were not present in the Java image. With those taken care of, the remaining work was to update the dependencies and transitive dependencies which were considered vulnerable. This was mostly a matter of trial and error to determine which changes would break the functionality so as to easily troubleshoot afterwards. Onto the third service, which had been mostly patched out by a colleague. Somewhere along the line, though, the service started throwing an error for a core function. We rolled back to a partially patched-out version, and two teammates spent some time troubleshooting to determine the cause and fix the issue. They unfortunately had little luck, and with feature deadlines approaching shifted to other tasks, so the work was assigned to me. Through some research and extensive debugging, I was able to determine that the object mapper wasn't being initialized properly, which was leading to some JSON-encoding errors. With that sorted out, the service stopped throwing errors, and the line of features/PRs that were building up were now able to get merged. That marked the end of my work on this story.

### Testing
There were certain cases where tests had to be rewritten (or new ones added), when core functionality of a given dependency would change after an update. Other than that, I would often build the docker images locally and run containers to ensure everything was working properly before deploying.

### Maintenance
To ensure the overall security of each service, it is important to perform frequent updates to each application's container image to ensure that common, OS-related vulnerabilites are not an issue. Aside from that, it will be crucial to keep an eye on the weekly vulnerability reports to see whether or not any dependencies have become an issue.

### Outcomes
With this project, I feel as though I have solidified my position as a dependable teammate, and competent engineer. The key takeaway for me was that it is best to stay on top of security vulnerabilities, so that it does not become a major timesink. Also, this type of work is terrible and no one enjoys it (but it still needs to get done).
