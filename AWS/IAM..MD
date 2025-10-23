
---

# AWS IAM — My Notes

IAM stands for **Identity and Access Management**.
It’s the system that controls who can log in and what they can do inside an AWS account.
Everything in IAM is about *access control*.

---

## 1. User

A **user** is someone who can sign in to AWS.
Each user has a name and credentials — this can be a password or access keys.
Good for people who need their own login.

---

## 2. Group

A **group** is just a bunch of users.
If I attach permissions to a group, every user inside gets those same permissions.
Makes it easier to manage teams instead of each user one by one.

---

## 3. Policy

A **policy** is a JSON file with rules.
It decides what actions are allowed or denied.
Main parts inside a policy:

* `Effect` → allow or deny
* `Action` → what can be done
* `Resource` → what the rule applies to
* `Condition` → optional extra check

Example of a small policy:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "s3:PutObject",
      "Resource": "arn:aws:s3:::my-bucket/*"
    }
  ]
}
```

---

## 4. Role

A **role** gives permissions for a short time.
It’s not owned by a user.
Any user, application, or AWS service can “assume” it and get temporary access.

---

## 5. Authentication vs Authorization

* Authentication = proving who you are.
* Authorization = checking what you’re allowed to do.

---

## 6. Federation

This means people can log in to AWS using another login system (like their company account).
No need to create IAM users for everyone.

---

## 7. Temporary Credentials (STS)

IAM can hand out short-lived access keys.
They expire automatically after a set time.
Used with roles or federation to avoid long-term keys.

---

## 8. Resource-based Policies

These are policies attached directly to resources (like an S3 bucket).
They say who can access that specific resource.

---

## 9. Permission Boundaries & SCP

* **Permission boundary** → sets a limit on how much access a user or role can get.
* **SCP (Service Control Policy)** → used in AWS Organizations to control permissions across accounts.
* If there’s a deny anywhere, it wins over allow.

---

## 10. MFA

MFA = Multi-Factor Authentication.
An extra step when logging in (like a code from a phone).
Very useful for securing important accounts.

---

## 11. Access Keys & Passwords

* Password → for console login.
* Access key → for CLI or API.
  Keys should be rotated and not stored in code.

---

## 12. Service Roles & Service-linked Roles

* Service roles let AWS services act on your behalf.
* Service-linked roles are created automatically for certain services.

---

## 13. IAM Tools

* **Policy Simulator** → test if something is allowed or denied.
* **Access Advisor** → see what services a user or role has used.
* **Credential Report** → list of users and their credential status.
* **Access Analyzer** → checks if something is exposed outside your account.

---

## 14. Best Practices

* Use roles instead of long-term access keys.
* Turn on MFA.
* Give only the minimum permissions needed.
* Don’t use the root account daily.
* Rotate credentials regularly.
* Use permission boundaries and SCPs if you manage multiple accounts.

---

## 15. Important Terms

* **ARN** → unique name for an AWS resource.
* **Principal** → the person/service making the request.
* **Policy** → the rules.
* **Role** → temporary permissions.
* **SCP** → organization-wide control.

---

## 16. Quick Recap

IAM controls access in AWS.

* Users and groups define *who*.
* Policies define *what they can do*.
* Roles give *temporary access*.
* Federation lets outsiders log in.
* MFA and security practices keep everything safe.

---

