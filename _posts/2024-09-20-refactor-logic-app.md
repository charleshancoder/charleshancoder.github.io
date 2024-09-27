---
layout: post
title: Refactoring Logic App
---

The other day, a colleague approached me with an interesting challenge. They were working on a Logic App that generates an email based on incoming JSON data. The logic app handles three different email types, each requiring a specific structure for the email body.

Here's a visual representation of the initial logic:

![before-la-refactor](/images/before-la.drawio.svg)

They wanted to add a new email type, driven by a JSON input like this one (demostration purpose only):

```json
{
  "persons": [
    {
      "firstName": "Joe",
      "lastName": "Doe",
      "isAbsent": false,
      "isSubmitter": true
    },
    {
      "firstName": "Jane",
      "lastName": "Smith",
      "isAbsent": true,
      "isSubmitter": false
    }
  ],
  "address": "123 Main St",
  "fromDate": "2021-07-01",
  "toDate": "2021-07-02"
}
```

## The Business Rules

The business rules for this new email type were simple:

### Scenario 1
If there's only one person, and they're both the submitter and absent, the email should indicate that they're absent:

```text

Hi Team,

Team member has indicated that they will be absent from the office, details below.

Submitted by: Joe Doe

Absentee Details: Joe Doe

From: <2021-07-01>

To: <2021-07-02>

Address: <123 Main St>

Comment: <GP appointment.>

```

### Scenario 2

 If there are two people, with one being the submitter and the other absent, the email should show them with a different message in Absentee Details:

```text

Hi Team,

Team member has indicated that they will be absent from the office, details below.

Submitted by: <Joe Doe>

Absentee Details: <Jane Smith>, but <Joe Doe> is not inlcuded in the absentee list.

From: <2021-07-01>

To: <2021-07-02>

Address: <123 Main St>

Comment: <GP appointment.>

```
## The Initial Approach

To solve this, my colleague used the `Compose` action in the Logic App, combining inline functions with HTML. The inline functions were fairly basic, such as this one to display the submitter's name:

```html

 <td>Submitted by</td><td>@{ if(equals(triggerBody()['persons'][0]?['isSubmitter'], true), triggerBody()['persons'][0]?['firstName'], triggerBody()['persons'][1]?['firstName'])}</td>

```

This works by checking if the first person in the array is the submitter. If true, it returns their first name; if not, it defaults to the second person. **While functional, this approach quickly grew complicated as the number of inline conditions increased—especially for handling both the submitter and absentee.**

## Initial Thoughts on Improvement

The complexity of the inline expressions made me think of two options:

1. **JavaScript in Logic Apps**: The `Execute JavaScript Code` action would allow us to process the JSON data more efficiently with custom logic. However, this feature is only available in the Standard tier or via an integration account, both of which would significantly increase costs (**estimated from $180 to $300 per month**).

2. **Azure Functions**: This would allow us to write a more structured solution using C#, separating logic and data processing. However, while keeping operational costs low, it introduces additional development and maintenance overhead.

Given that the app was only running about **20 times per month**, neither approach seemed cost-justified. So, I decided to refactor the existing Logic App rather than using the more expensive solutions.


## The Refactor

The goal was to reduce the complexity of the inline functions and make the email construction more maintainable. Here's how I tackled it:

1. **Extract Data with `JSON Filter array`**: I started by using the `JSON Filter array` action to filter out the submitters and absentees. This separated the logic for identifying the relevant people from the email body construction.

2. **Simplify Email Construction**: With filtered arrays, I could now cleanly reference the submitter and absentee in the email body. Here's a simplified version of the `Compose` action used to construct the email:

```html

 <td>Submitted by</td><td>@{ if(greater(length(body('Submitters')), 0), true), body('Submitters')[0]['firstName'], 'Unknown'}</td>

```

Even better, I refactored this further to use the `coalesce` function, which returns the first non-null value in a list of arguments:

```html

 <td>Submitted by</td><td>@{ coalesce(body('GetSubmitter')?[0]?['firstName'], 'Unknown')}</td>

```

This eliminates the need for a conditional check, making the code much more readable. I applied similar changes to the absentee section, resulting in a much cleaner email body construction.

This approach does require data validation to make sure that there is at least one submitter and one absentee. I will discuss the JSON schema validation in a separate post.

## Final Structure

Here's the updated structure after refactoring:

![after-la-refactor](/images/after-la.drawio.svg)

## Conclusion

By using Logic App's built-in actions like `JSON Filter array`, and `Compose`, I was able to simplify the process of generating emails without needing external services like Azure Functions or JavaScript. The result is a Logic App that's both more maintainable and cost-effective.

If you're working with Logic Apps—or any block-based programming language—there are always ways to refactor for clarity and maintainability. This refactor reduced complexity without introducing additional costs, showing that sometimes a small change in how you approach logic can make a big difference in the long run.

I hope this has given you some insights into simplifying your Logic Apps while keeping them effective and maintainable!
