---
layout: post
title: "Naming Conventions for Logic Apps"
---

Naming things well is notoriously difficult, especially in software development. It's a problem we often underestimate, yet it has far-reaching effects on readability, maintainability, and even team communication. When it comes to Logic Apps, this challenge extends further: the right naming conventions help make your integrations manageable, understandable, and scalable.

I've worked with Logic Apps for some time now, and through experience, I've found certain naming conventions that make day-to-day work much easier. In this post, I'll share some patterns that have been helpful in organizing and maintaining Logic App solutions effectively.

## Resource Types

A good starting point is to include resource type in the name. This might seem unnecessary at first, given that Azure itself can display the resource type in the portal, but including this information in the name has advantages.

For example, when communicating within your team and writing documentations, it's clearer to use “the la-* needs some work" rather than saying "the Logic App bla ..." every time.

While Microsoft recommends using `logic-` as a prefix in their best practices, I opt for `la-` for brevity. Saving those extra characters may seem minor, but over time and across large environments, it adds up.

## Conveying Purpose

A good naming convention doesn’t stop at identifying the resource type. It’s just as important to capture the purpose of the resource. One effective pattern I’ve seen is using the `source-destination` format in Logic App names. This pattern highlights not only the purpose but also the flow of data or the integration’s intent.

For instance:

- `la-salesforce-procore` makes it clear that the Logic App integrates Salesforce with Procore.
This simple structure conveys both the type of resource `la-` and its function `salesforce-procore`.

## Handling Scalability with Environment Naming

Many of us deal with multiple environments—development, testing, production—and sometimes even multiple instances of those environments, like when handling different regions. Including environment details in the name helps scale your Logic App solution and maintain clarity across various deployments.

A few examples:

- `dev1-la-salesforce-procore` for a Logic App in the first development environment.
- `prod1-la-salesforce-procore` for a Logic App in the first production environment.

It might be tempting to skip instance numbering for production, opting for something shorter like prod-la-salesforce-procore. However, in cases where you have multiple production environments (such as one per region), it's better to include this instance identifier.

## Host ID Collision and Why Naming Matters

An overlooked issue is Host ID Collision, particularly if you're working with Logic Apps in the Standard tier. This is a known issue in Azure Function Apps, but it's equally relevant to Logic Apps since the underlying hosting model is similar.

The Logic Apps runtime generates a host ID by hashing the first 32 characters of the Logic App's name. If you have multiple Logic Apps sharing the same storage account and using similar names, you could end up with a Host ID collision, which causes all sorts of headaches, such as failed executions.

By sticking to clear, distinct naming conventions like those shared here, you reduce the likelihood of such collisions and ensure smoother deployment and execution of Logic Apps.

## Final Thoughts

Naming conventions are often seen as trivial details, but in complex environments like those involving Logic Apps, they play a significant role in ensuring scalability and team collaboration. By thoughtfully incorporating resource types, purpose, and environment details into your names, you create a more manageable and error-resistant system that your team will thank you for.

Consider adopting a convention before you start creating resources. It will save you much confusion down the line.

