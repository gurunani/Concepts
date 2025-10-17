# AWS IAM — Simple Explanation (Markdown)

> IAM = Identity and Access Management.
> It controls **who** can do **what** in your AWS account.

---

## 🧑 1. User

* A **User** is someone who can log in to AWS.
* Each user has a **name** and **credentials** (password or access key).
* Used for people who work in the account.

---

## 👥 2. Group

* A **Group** is a collection of users.
* Permissions given to the group apply to all users inside it.
* Easy to manage many users at once.

---

## 📜 3. Policy

* A **Policy** is a set of rules written in JSON.
* It says what actions are **allowed** or **denied**.
* Main parts of a policy:

  * `Effect`: Allow or Deny
  * `Action`: What the user can do (example: start EC2)
  * `Resource`: Which resource the rule applies to
  * `Condition`: Extra check (optional)

---

## 🪪 4. Role

* A **Role** gives temporary permission.
* It does not belong to any one user.
* A user, application, or service can “assume” the role.
* When assumed, it gives temporary access.

---

## 🔐 5. Authentication vs Authorization

* **Authentication** = proving identity (login, key, etc.).
* **Authorization** = what you are allowed to do (based on policies).

---

## 🌐 6. Federation

* Users can log in to AWS using other systems (like company login).
* No need to create separate IAM users for them.

---

## ⏳ 7. Temporary Credentials (STS)

* IAM can give **short-lived access keys**.
* They expire after some time.
* Used with roles or federation.

---

## 🧭 8. Resource-based Policies

* Attached **directly** to an AWS resource.
* Example: S3 bucket policy.
* Defines who can access that resource.

---

## 🧱 9. Permission Boundaries & SCPs

* **Permission Boundary** = maximum limit of permissions for a user or role.
* **SCP (Service Control Policy)** = controls permissions across many AWS accounts (in AWS Organizations).
* **Deny** always wins over allow.

---

## 🧰 10. MFA (Multi-Factor Authentication)

* Extra security step when logging in.
* Requires a second device or code.
* Strongly recommended for important accounts.

---

## 🔑 11. Access Keys & Passwords

* **Password** = used for logging in to the web console.
* **Access key** = used for CLI or API access.
* Access keys should be rotated or replaced often.

---

## 🧠 12. Service Roles & Service-linked Roles

* **Service role**: A role that AWS service uses to do actions.
* **Service-linked role**: Created by AWS automatically for some services.

---

## 📊 13. Tools in IAM

* **Policy Simulator**: Test if a policy allows or denies an action.
* **Access Advisor**: Shows which services are used by a user or role.
* **Credential Report**: Shows password and access key status.
* **Access Analyzer**: Finds external access to resources.

---

## 🛡️ 14. Best Practices

* Use **roles** instead of long-term keys.
* Turn on **MFA**.
* Give **minimum permissions** needed.
* Do not use the root account for daily work.
* Check and rotate credentials often.
* Use SCPs and permission boundaries for control.

---

## ⚙️ 15. Simple Policy Example

```json
{
  "Version":"2012-10-17",
  "Statement":[
    {
      "Effect":"Allow",
      "Action":"s3:PutObject",
      "Resource":"arn:aws:s3:::my-bucket/*"
    }
  ]
}
```

This allows putting objects into the `my-bucket` S3 bucket.

---

## 📝 16. Important Terms

* **ARN**: Unique name for AWS resources.
* **Principal**: Who makes a request (user, role, service).
* **Policy**: Set of rules.
* **Role**: Temporary permissions.
* **SCP**: Organization-level control.

---

## 📌 17. Summary

* IAM controls access to AWS.
* Users and groups define *who*.
* Policies define *what they can do*.
* Roles give *temporary access*.
* Federation connects external logins.
* MFA and good security practices protect your account.

---
