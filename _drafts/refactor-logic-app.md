---
layout: post
title: Refactoring Logic App
---

My colleague came to me the other day with a problem. They have a logic app that creates an email based on a given JSON data. There are foure different type of email types for creating the email body.

The logic app has the following structure:

<!--insert-digram -->

They wanted to add a new email type which has the following JSON data input (demostration purpose only):

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

The business requirements for creating the email are as follows:

Scenario 1:

The `persons` array contains one person who is the submitter as well as absentee. Then a message is displayed in the email body as follows:

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

Scenario 2:

When there are two persons in the collection, one of them is the submitter and the other is absent. Then the name of the submitter is displayed in the submitter field and the name of the absentee is displayed in the absent field with a different message in the email body as follows (demostration purpose only not the exact message):

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

My colleague's solution was to utlisie `Compose` action which has inline functions mixed with HTML code.

The inline functions are single statement expressions, for example:

```html

 <td>Submitted by</td><td>@{ if(equals(triggerBody()['persons'][0]?['isSubmitter'], true), triggerBody()['persons'][0]?['firstName'], triggerBody()['persons'][1]?['firstName'])}</td>

```

The assumption here is that one of the persons must be a submiiter.

The above inline function is used to extract the submitter from the JSON data. The function checks if the `isSubmitter` field is true for the first person in the array, if it is true then it returns the first person's first name, otherwise it returns the second person's first name.

You can imagine how complex the inline functions can get when we have to extract the submitter and absentee from the JSON data and then display them in the email body with nested `if` conditions.

So my first thought was to use the `Execute JavaScript Code` built-in action so we can write the JavaScript code to process the body and produce the email content.

However, we realised that the action is only supported in Standard tier or using an integration account. Both will have higher cost than our current consumption plan.

I looked at the Logic App run history and on everage it ran **20 times per month**, which does not justify the cost of upgrading to the Standard tier.

My next thought was to use Azure Functions. We can write the C# code in the Azure Function and call the function from the Logic App. This way, we can keep the cost low and still have a maintainable solution but it will require more work, i.e. more cost for development and deployment time.

So I decided to refactor the inline functions in the email body.

The first step was to extract the submitters and absentees from the JSON data. I used the `Parse JSON` action to parse the JSON data and then used the `Filter array` action to filter the persons based on the `isSubmitter` and `isAbsent` fields.

The next step was to use the above filtered arrays to construct the email body. I used the `Compose` action to construct the email body. The email body is constructed based on the number of submitters and absentees.

The new Logic App structure is as follows:

<!---insert diagram-->

Now the email body `Compose` action is much cleaner and easier to read:


```html

 <td>Submitted by</td><td>@{ if(greater(length(body('Submitters')), 0), true), body('Submitters')[0]['firstName'], null}</td>

```

We could further refactor the inline function to use `coalesce` function to make it more readable:

```html

 <td>Submitted by</td><td>@{ coalesce(body('Submitters').first?['firstName'], null)}</td>

```

The `coalesce` function returns the first non-null value from the list of arguments. In this case, it returns the first submitter's first name if there is any submitter, otherwise it returns null.
