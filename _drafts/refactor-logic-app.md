---
layout: post
title: Refactoring Logic App
---

My colleague came to me the other day with a problem. They have a logic app that creates an email based on the given JSON data. The logic app is working fine, but it's getting hard to maintain and extend. They asked me to help refactor the logic app to make it more maintainable and easier to work with.

The JSON data input look like this (not the exact data, but similar):

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

The business logic for creating the email is as follows:

Scenario 1:

Simple case where one person is the submitter as well as absentee. Then a message is displayed in the email body.

Scenario 2:

When there are two persons, one of them is the submitter and the other is absent. Then the name of the submitter is displayed in the submitter field and the name of the absentee is displayed in the absent field.

If both persons are absent, display both names in the absent field with a different message.

My colleague's solution was to utlisie Logic App inline functions in the email body. The email body looks like this:

```html

<!DOCTYPE html>
<html>
<head>
    <title></title>
</head>
```

The syntax for the inline function is getting very complex and hard to read. So my first thought was to use the built-in action in the Logic App `Execute JavaScript Code`. This way, we can write the JavaScript code to process the body and produce the email content.

However, when we added the `Execute JavaScript Code` action, we realised that the action is only supported in Standard tier or using an integration account. Both will have higher cost than our current consumption plan.

I looked at the Logic App run history and on everage it ran 20 times per month, which does not justify the cost of moving to the Standard tier.

My next thought was to use Azure Functions. We can write the C# code in the Azure Function and call the function from the Logic App. This way, we can keep the cost low and still have a maintainable solution but it will require more work, i.e. more cost for development and deployment time.

So I decided to refactor the inline functions in the email body.

The first step was to extract the submitters and absentees from the JSON data. I used the `Parse JSON` action to parse the JSON data and then used the `Filter array` action to filter the persons based on the `isSubmitter` and `isAbsent` fields. This make the logic app much easier to read and understand than the inline functions.



