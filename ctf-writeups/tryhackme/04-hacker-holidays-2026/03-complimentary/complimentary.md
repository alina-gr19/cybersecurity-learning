# Complimentary Write-Up

## Objective

This is the third challenge from [TryHackMe](https://tryhackme.com)'s **Hacker Holidays 2026**, focusing on web security.

The objective was to determine how the Byte Lotus Wellness application issued guest AWS credentials, identify the underlying misconfiguration, and retrieve the hidden flag.

## Investigation Steps

I began by inspecting the application's JavaScript source. During the review, I discovered that it used an **Amazon Cognito Identity Pool** to automatically obtain guest AWS credentials.

```javascript
const IDENTITY_POOL_ID = "us-east-1:836c0949-292d-485b-b532-52d5ca7bb688";
const AWS_REGION = "us-east-1";
const TABLE_NAME = "complimentary-GuestWellnessProfiles";
```

The application initialized the AWS SDK using these guest credentials and performed a `getItem()` request to retrieve only the current guest's record based on the `guest_id` stored in the browser's local storage.

Because the challenge description suggested an **IAM misconfiguration**, I investigated whether the guest credentials had permissions beyond those intended by the application. Instead of relying on the frontend's `getItem()` request, I opened the browser's Developer Tools console and issued a DynamoDB `scan()` request against the same table.

```javascript
db.scan({ TableName: TABLE_NAME }, (err, data) => {
  console.log("Error:", err);
  console.log("Data:", data);
});
```

The request succeeded and returned every item stored in the `complimentary-GuestWellnessProfiles` table. This confirmed that the IAM policy attached to the Cognito guest role allowed unrestricted `Scan` access instead of limiting users to their own records.

After reviewing the returned records, I located another guest's entry containing the flag, completing the challenge.

## Final Thoughts

This challenge demonstrated how cloud misconfigurations can expose sensitive information even when an application's frontend appears to enforce proper access controls. Although the interface only retrieved a single guest's record, the underlying IAM permissions granted far broader access than intended.

The exercise highlighted the importance of applying the **principle of least privilege** when configuring IAM policies. Guest identities should only be granted the minimum permissions necessary for their intended functionality, preventing unauthorized access to other users' data even if the application's client-side logic is bypassed.
