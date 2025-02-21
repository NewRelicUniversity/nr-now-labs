# Disclaimer

This presentation and the information herein (including any information that may be incorporated by reference) is provided for informational purposes only and should not be construed as an offer, commitment, promise or obligation on behalf of New Relic, Inc. (“New Relic”) to sell securities or deliver any product, material, code, functionality, or other feature. Any information provided hereby is confidential and proprietary to New Relic and may not be replicated or disclosed without New Relic’s express written permission.

---

# _Lab:_ Using integrated APM and Infrastructure to troubleshoot a performance problem

**Objective:** The New Relic Pet Clinic application experienced a sudden increase in response time. Use integrated APM and Infrastructure to investigate the problem.

## Step 1
Select the **New Relic Pet Clinic** application in the **NRU Training** account. Set the time picker to 24 hours, and locate a response-time spike (there should be one daily between 06:10 and 06:50 UTC). 

![](/images/new-relic-pet-clinic-apm-overview.png)

## Step 2
Use integrated APM and Infrastructure to answer the following questions: 

- Were there any application deployments around the time of the incident?

- Was there an increase in application throughput that might explain the increased 
response time?

- Did response time increase across all hosts (containers), or only certain ones?

- Select an affected container and view the data for the associated host (_Hint:_ _Related entities_ > _See full map_). Do you notice anything unusual about that host’s data?

---

# _Lab:_ Creating alert conditions with guided mode

**Objective:** Use New Relic’s guided mode to create two alert conditions: 

- One that will open a critical incident if production APM services have a response time greater than 500 ms for at least 5 minutes; and

- Another that will open a critical incident if the page load time for a browser application is greater than 2 seconds for at least 5 minutes. 

## Step 1: Log into the training account

In a private browser window, use the following credentials to log into New Relic: 

- URL: [https://one.newrelic.com/](https://one.newrelic.com/)
- Email: learn@newrelicuniversity.com
- The instructor will provide the password

## Step 2
Select the **NRU Training** account. From New Relic’s main menu, select _Alerts_ > _Alert Conditions_, then click _+ New alert condition_ in the upper-right.

## Step 3
Select _Use guided mode_. Your first alert condition will monitor APM services, so select _Services - APM_ and click _Next_.

## Step 4
On the next page, use _Filter by name or tags_ to filter the list of services to those containing a value of “Production” in the “Environment” tag. Select the matching services.

## Step 5
Scroll down to **Select a metric to monitor**. From the list of golden metrics, select _Response time (ms)_. Note the automatically-generated NRQL query, then click _Next_.

## Step 6
On the next page, under **Set condition thresholds**, configure a static threshold to open an incident of critical severity when a query returns a value above 500 (ms) for at least 5 minutes. Note that you may optionally add a second threshold to create an incident of warning severity. Click _Next_.

## Step 7
On the final page, give your alert condition a descriptive name, such as “[Your initials] Production services response time > 500 ms”. 

## Step 8
Under **Connect this condition to a policy**, select _New policy_ and create a new alert policy to contain your conditions. Give it a name you will recognize, such as “[Your initials] Alert policy”. Accept the defaults for all other options.

## Step 9
Click _Save & set up notifications_. When prompted to create a workflow, select _Cancel_.

## Step 10
Repeat the above steps to create a second alert condition: Open a critical incident if the _New Relic Pet Clinic_ browser application has a page load time greater than 2 seconds for at least 5 minutes. Add the condition to the policy you just created.

## Additional resources
- [Alert conditions documentation](https://docs.newrelic.com/docs/alerts/create-alert/create-alert-condition/alert-conditions/)
- [Set thresholds for alert conditions](https://docs.newrelic.com/docs/alerts/create-alert/set-thresholds/set-thresholds-alert-condition/)
- [Decide when issues are created](https://docs.newrelic.com/docs/alerts/organize-alerts/specify-when-alerts-create-incidents/)

---

# _Lab:_ Creating alert conditions with custom NRQL queries

**Objective:** In the previous lab, you used New Relic’s guided mode to automatically generate NRQL queries for alert conditions. In this lab, you will create an alert condition by writing your own query.

## Step 1
Select the **NRU Training** account. From New Relic’s main menu, select _Alerts_ > _Alert Conditions_, then click _+ New alert condition_ in the upper-right.

## Step 2
Select _Write your own query_. **Challenge:** See if you can write a query to return the maximum percentage of CPU usage from each host running the _New Relic Pet Clinic_ APM service. (Try asking AI and see how it does.) When you have a working query, click _Next_.

## Step 3
On the next screen, set a static threshold to open a critical incident when the query returns a value above 50 (percent) for at least 5 minutes. You may optionally add a second threshold to open a warning incident when the query returns a value above 25 for at least 2 minutes.

## Step 4
On the next screen, give your alert condition a clear name to describe what’s wrong when an incident occurs, for example: “[Your initials] Pet Clinic host CPU usage > 50 percent”. Add the new condition to the alert policy you created in the previous lab.

## Additional resources
[NRQL conditions documentation](https://docs.newrelic.com/docs/alerts/create-alert/create-alert-condition/create-nrql-alert-conditions/)

---

# _Lab:_ Configuring alert notifications

**Objective:** Configure a workflow to connect alert issues to one or more notification channels, and configure an email destination to notify you when an alert issue is opened, closed, or acknowleged.

## Step 1
Select the **NRU Training** account. From New Relic’s main menu, select _Alerts_ > _Workflows_, then click _+ Add a workflow_ in the upper-right.

## Step 2
Under **Configure your workflow**, give your workflow a name you will recognize, such as “[Your initials] Workflow”.

## Step 3
Under **Filter data**, create a Basic or Advanced filter that matches the NRQL alert condition you created in the previous lab. You may use a Basic filter that matches your alert policy, or an Advanced filter that matches your alert condition name.  

## Step 4
Under **Notify**, select _Email_. Under **Email destination**, click in _Search by name or email_ and select “Create new destination”.

## Step 5
On the **Add a destination** page, select _+ Add email_ and enter your email address. Give the new destination a name, such as “[Your initials] Email”. Click _Save destination_.

## Step 6
Back on the **Email** page, confirm that your destination name appears under **Email destination**. If you wish, you may click _Send test notification_. Did you receive an email? 

When the destination is configured correctly, click _Save_.

## Step 7
Back on the **Configure your workflow** page, click _Activate workflow_.

The instructor will generate high CPU load on one of the hosts. Within 5 minutes, you should receive an email notification that your alert condition has triggered a critical incident.

## Additional resources
- [Workflows documentation](https://docs.newrelic.com/docs/alerts/get-notified/incident-workflows/)
- [Destinations documentation](https://docs.newrelic.com/docs/alerts/get-notified/destinations/)

---

# _Lab:_ Creating service levels

**Objective:** Configure a service level to monitor the availability of a service over time. In this lab, you will monitor the success rate of a Synthetic monitor.

## Step 1
Select the **NRU Training** account. From New Relic’s main menu, select _All Capabilities_, then select _Service Levels_. Click _+ Add a service level_ in the upper-right.

## Step 2
Under **Set SLI: Choose data**, select the _Pet Clinic AWS_ Synthetic monitor from the list of entities and click _Continue_.

## Step 3
Under **Set SLI: Configure queries**, New Relic automatically suggest queries to get the total number of checks for the Synethics monitor, and the number of successful checks. Select _Success. Proportion of successful synthetic checks_, then click _Continue_.

## Step 4
Under **Set SLO: Time window and target percentage**, select the desired time window and target percentage. For this lab, select a window of 1 hour and target of 90%, then click _Continue_.

## Step 5
Finally, give your service level a name, such as “[Your initials] Pet Clinic AWS - Success”. You may optionally add tags and a description. Click _Save_ in the lower right to save your service level.

## Additional resources
[Service levels documentation](https://docs.newrelic.com/docs/service-level-management/create-slm/#suggested-sli)
