# AWS-SAA-Projects
In this repository you will find a series of AWS demos as part of my preparation for the AWS Solutions Architect Associate exam. Hope it might be useful for those who are starting their AWS journey ;) <br/>

$$\color{red} IMPORTANT$$ <br/>
Before getting your hands on these projects, make sure you have set a Cost Management budget to raise an alarm everytime cost exceeds a fixed budget (some resources do not fall into the free tier (see, for instance, client managed KMS keys)). This may be useful in the scenario we left some resources undeleted after the demo lesson. Follow the steps below to set a budget for your AWS projects: <br/>

1 - Move to the Billing and Cost Management console and click on 'Budgets'. Once you've done that, click on 'Create budget'. There, you can pick between using a Simplified budget ('Use a template', where the recommended configurations are selected by default) or an Advanced budget ('Customize'). Most of the cases using the simplified version will be enough. If we pick the first, below the 'Templates' panel we can pick among a number of 4 different templates, deppending on our use case. All services used in this repo fall into the free tier (except AWS Route53 since falls apart of the free tier) so pick the 'Zero spend budget'. This template will automatically raise an alarm once our spending exceeds 0.01$ which is above the AWS Free Tier limits. <br/>

2 - Next, provide a name for the budget below, and specify the recipients of the budget alarms in the 'Email recipients' box. <br/>

3 - Finish the budget creation by clicking on 'Create budget' at the end of the pabe. <br/>

4 - In case some extra charges have been added to your monthly bill and do cannot find where do they come from, do not panic! Within the Billing and Cost Management service, click on 'Cost explorer' in the menu on the left under 'Cost and Usage Analysis'. Here you will find some reports and analytics of your AWS services usage and track those which are potentially increasing your bill. <br/>
