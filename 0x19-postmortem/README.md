# Postmortem

![Flogging a dead horse](images/post-mortem-meetings.jpg)

## Issue Summary

We had just released a new feature to our recently launched Ruby on Rails site that we had our first intake of users complaining about the site. 5 minutes after we performed a feature update, we started receiving emails from our users talking about "they can't sign in or sign up to our platform". It was quite surprising to us because we knew it worked on our machines and it worked before. About 127 of such emails came to our inbox. It was an avalanche of emails. Knowing how hard it can be to attract and keep users, we couldn't afford to lose 127 of our users in that way and decided to take a closer look at the problem. We cloned our site's repository from GitBug, followed the installation instructions on the README and to our surprise the site couldn't startup. It wasn't long before we realized that the cause of the problem was failing to update the requirements for our project. The site was malfunctioning from 9:55 AM GMT+1 to 11:20 AM GMT+1.

## Timeline

+ 14:00 UTC: Issue detected via automated monitoring alert indicating high database response times.
+ 14:05 UTC: Initial investigation began, focusing on recent code changes and server performance metrics.
+ 14:15 UTC: Engineers hypothesized a network issue, leading to a check on the network infrastructure.
+ 14:30 UTC: Network issues ruled out; attention shifted back to the application layer.
+ 14:40 UTC: Code review initiated; the recent deployment identified as a potential cause.
+ 14:50 UTC: Misleading path investigated involving load balancer configuration.
+ 15:00 UTC: Escalation to the database team after discovering excessive database CPU usage.
+ 15:10 UTC: Unoptimized query identified in the latest code deployment.
+ 15:20 UTC: Rollback of the recent code deployment initiated.
+ 15:30 UTC: Service restored to normal functionality after rollback completed.

## Root Cause And Resolution

**Root Cause:**
The outage was caused by an unoptimized SQL query introduced in the latest code deployment. The query inadvertently triggered a full table scan on a high-traffic database table, causing a substantial increase in CPU usage and slowing down database response times. This cascading effect resulted in the overall slowdown of the website.

**Resolution:**
The immediate fix involved rolling back the recent deployment to restore the previous, stable version of the code. Subsequently, the problematic query was optimized by adding appropriate indexes and refining the query logic to ensure efficient execution. The optimized query was thoroughly tested in a staging environment before redeployment.

## Corrective And Preventative Measures

**Improvements:**

+ Enhanced code review processes to include performance testing for database queries.
+ Improved monitoring to detect and alert on inefficient database queries before they affect users.
+ Increased capacity planning for the database server to handle unexpected query loads without significant performance degradation.

**Tasks:**

 + Patch and redeploy the optimized query with proper indexing.
 + Implement query performance monitoring tools on the database server.
 + Conduct training for developers on writing efficient SQL queries.
 + Update the CI/CD pipeline to include automated performance testing for database interactions.
 + Review and optimize existing database queries across the application to prevent similar issues.

## Fun Fact

Remember, a little bit of humor and some pretty diagrams can go a long way in keeping your audience engaged. After all, who said postmortems have to be boring?
