

# How to name things in programming

Naming things is one of the most important things in programming and it's also one of the hardest things to do. It's hard because it's a creative process and it's subjective. There are no hard rules to follow, but there are some guidelines that can help you to name things in a better way.

## Why is naming important?

Naming things is capturing the essence of what something does in a single word or phrase. It's about finding the right balance between being descriptive and concise. A good name should be self-explanatory and should convey the purpose and intent of the thing it represents. However, in reality, finding the right name is often a challenging task because it requires creativity, domain knowledge, and context.

I was working on an integration project where we sync data from one system to another. In the source system, we had a concept called **Resident** and in the destination system, we had a concept called **Client**. The question was, what should we name the entity in the integration layer? Should it be **Resident**, **Client**, or something else? After some discussion, we decided to name it **Client** because it was more aligned with the destination system and it made more sense in the context of the integration.

## Name Logic App

There are several limitations for Logic App names:

- For Consumption (multi-tenant) Logic Apps: Maximum 80 characters
- For Standard (single-tenant) Logic Apps: Maximum 43 characters
- [Host ID Collisions][hostidcollision]


When naming a Logic App for intgertaion project, it's important to follow a consistent naming convention that reflects the purpose and intent of the Logic App.

### Source-Destination Pattern

We decided to use the **Source-Destination pattern** for naming Logic Apps. For example, if we are syncing data from Salesforce to Dynamics 365, we would name the Logic App `salesforce-dynamics365`. This naming convention helps to quickly identify the source and destination systems and provides a clear indication of what the direction of the data flow.

### Specify Resource Type

When referring to the Logic App in code or documentation, it does not tell us what resource type it is. For example, if we are using an Azure Function to trigger the Logic App, we would name the Logic App `la-salesforce-dynamics365`. This naming convention helps to quickly indicate that it is a Logic App and provides a clear indication of the source and destination systems.

### Scalability

What if we have multiple Logic Apps for the same integration but for different environments scenario? In this case, we would append environment-specific prefix to the Logic App name. For example, if we have a Logic App for syncing data from Salesforce to Dynamics 365 in the development environment, we would name the Logic App `dev1-la-salesforce-dynamics365`.

The reason to have it in this way is to avoid Host ID Collisions.

> Host ID Collisions
> 
> If you have multiple Function Apps sharing a single storage account and they're each using the same Host ID (generated or explicit), that can result in a Host ID collision. For example, if you have multiple apps with names longer than 32 characters their computed IDs may be the same. Consider two Function Apps with names myfunctionappwithareallylongname-eastus-production and myfunctionappwithareallylongname-westus-production that are sharing the same storage account. The first 32 characters of these names are the same, resulting in the same Host ID myfunctionappwithareallylongname being generated for each.


## Name Workflows

When creating workflows, we have split the workflows into two categories: **Preparation** and **Update**. Preparation workflows are responsible for gathering data from the source system and preparing it for the destination system. Update workflows are responsible for updating the destination system with the data from the source system.



[hostidcollision]: https://github.com/Azure/azure-functions-host/wiki/Host-IDs#host-id-collisions

