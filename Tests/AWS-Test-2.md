# AWS Test 2 — Questions, Answers and Explanations

**Name:** Sanket Kolhe · **Date:** 5 September 2026 · **45 MCQs (1 mark each)**

Topics covered: Shared Responsibility Model · Root account · IAM (users, groups, roles, policies) · STS & AssumeRole · Support plans · Global infrastructure (Regions, AZs, Edge Locations, Local Zones)

---

## Quick answer key

| Q | Ans | Q | Ans | Q | Ans | Q | Ans | Q | Ans |
|---|---|---|---|---|---|---|---|---|---|
| 1 | C | 10 | D | 19 | D | 28 | A | 37 | C |
| 2 | B | 11 | D | 20 | B | 29 | C | 38 | B |
| 3 | C | 12 | D | 21 | A | 30 | A | 39 | A |
| 4 | C | 13 | B | 22 | A | 31 | A | 40 | D |
| 5 | A | 14 | D | 23 | C | 32 | D | 41 | C |
| 6 | B | 15 | B | 24 | A | 33 | A | 42 | C |
| 7 | C | 16 | D | 25 | C | 34 | B | 43 | C |
| 8 | A | 17 | C | 26 | B | 35 | C | 44 | B |
| 9 | C | 18 | B | 27 | B | 36 | C | 45 | C |

---

## Detailed answers

**Q1. Which statement best describes the Shared Responsibility Model?**
**Ans: C — AWS and customer have different security responsibilities**
AWS secures **of** the cloud (hardware, data centers, network, hypervisor); the customer secures **in** the cloud (data, IAM, encryption, OS, firewall rules). Neither side is responsible for everything.

**Q2. An administrator wants to permanently delete an AWS account. Which identity has the authority?**
**Ans: B — Root user**
Closing the account is a **root-only task**. No IAM user or role can do it, however much permission it has.

**Q3. A user logs in using the root email and root password. What identity is this?**
**Ans: C — Root user**
Signing in with the **email address** = root user. An IAM user signs in with account ID/alias + user name.

**Q4. IAM user should only start and stop specific EC2 instances. Which principle?**
**Ans: C — Follow least privilege**
Give only the exact actions (`ec2:StartInstances`, `ec2:StopInstances`) on only those instance ARNs. AdministratorAccess, root access and `Action:*` all violate least privilege.

**Q5. Restrict an IAM user's maximum possible permissions even if other policies grant more.**
**Ans: A — Permissions boundary**
A boundary **grants nothing**; it only caps the maximum. Effective permission = boundary ∩ attached policies.

**Q6. One policy allows s3:GetObject, another explicitly denies it. What happens?**
**Ans: B — Access is denied**
**An explicit Deny always wins** over any number of Allows. This is the core rule of IAM policy evaluation.

**Q7. IAM user given Action "*" and Resource "*". What does this represent?**
**Ans: C — Broad / full permissions**
Every action on every resource — effectively administrator access. A serious least-privilege violation.

**Q8. Users should authenticate with the existing corporate identity provider instead of separate IAM users.**
**Ans: A — Federation**
Federation (SAML 2.0 / IAM Identity Center / web identity) lets corporate users get temporary AWS credentials without creating an IAM user per employee.

**Q9. User in Mumbai, website hosted in Europe, high latency. Which service helps?**
**Ans: C — CloudFront**
CloudFront is the CDN — it caches content at Edge Locations near the user, cutting latency.

**Q10. Enterprise with business-critical workloads needs the highest support and proactive guidance.**
**Ans: D — Enterprise**
Enterprise gives the fastest response (business-critical ~15 min) and a designated Technical Account Manager (TAM).

**Q11. Main security problem with sharing root credentials among admins?**
**Ans: D — It makes individual accountability difficult and increases risk**
If everyone uses the same login, CloudTrail cannot tell who did what, and one leak compromises the entire account.

**Q12. Role max session duration 4 hours, assumed at 2 PM. When does it expire?**
**Ans: D — 6 PM**
2 PM + 4 hours = 6 PM.

**Q13. What does a role's trust policy primarily define?**
**Ans: B — Who or what can assume the role**
Trust policy = **WHO** may assume. Permissions policy = **WHAT** the role can do. Every role needs both.

**Q14. Developer experimenting, needs support mainly during business hours.**
**Ans: D — Developer**
Developer support = email access to a Cloud Support Associate during business hours; the cheapest paid plan.

**Q15. EC2 instance needs temporary access to DynamoDB. Which service issues temporary credentials?**
**Ans: B — AWS STS**
AWS Security Token Service issues the temporary Access Key + Secret Key + Session Token when a role is assumed.

**Q16. Why deploy an application across two Availability Zones?**
**Ans: D — To improve resilience and availability**
If one AZ fails, the other keeps serving. It does not reduce cost — cross-AZ traffic actually costs a little more.

**Q17. Production application, company needs 24/7 technical support.**
**Ans: C — Business**
Business support gives 24×7 phone, email and chat, with production-down response under 1 hour.

**Q18. Company needs to change account-level settings that require root credentials.**
**Ans: B — Use the root user only for that required task**
Sign in as root, do only that one task, sign out. Never share root or hand out AdministratorAccess instead.

**Q19. CloudFront delivers content through locations around the world. What are they called?**
**Ans: D — Edge Locations**
Edge Locations are the CDN cache points — far more numerous than Regions.

**Q20. Employee needs EC2 access only during a specific task; no long-term access keys.**
**Ans: B — IAM role**
A role gives temporary credentials that expire automatically — exactly the case for short-term access.

**Q21. Trust policy allows the user, but the user still gets Access Denied when assuming the role.**
**Ans: A — sts:AssumeRole permission for the user**
**Both sides** are needed: the role's trust policy must allow the user **and** the user's own identity policy must allow `sts:AssumeRole` on that role ARN.

**Q22. Launch an EC2 instance in a specific geographic AWS location. What must be selected?**
**Ans: A — Region**
The Region is the geographic area. An AZ is chosen inside the Region (often automatically).

**Q23. Customer forgets to configure appropriate IAM permissions. Who is responsible?**
**Ans: C — Customer**
IAM configuration is security **in** the cloud — always the customer's responsibility.

**Q24. EC2 instance must access an S3 bucket without storing access keys in the application.**
**Ans: A — IAM role**
Attach an instance-profile role. AWS supplies and rotates temporary credentials, so no keys sit on the disk.

**Q25. An employee asks for the root password to manage EC2. What should the admin do?**
**Ans: C — Create an appropriate IAM identity instead**
Root is never shared. Create an IAM user/role with only the EC2 permissions needed.

**Q26. A policy that can be reused by multiple IAM users and groups.**
**Ans: B — Customer managed policy**
Managed policies are standalone and reusable. An inline policy belongs to a single identity; a trust policy is only for roles.

**Q27. 50 developers all need read-only S3 access, without configuring each one.**
**Ans: B — Create an IAM group with S3 read-only permissions and add the users**
Groups are the standard way to manage permissions for many users at once.

**Q28. With a fully managed service AWS manages more infrastructure than with EC2. This shows…**
**Ans: A — Responsibilities can vary depending on the service**
The split shifts across IaaS → PaaS → SaaS. The customer always keeps some responsibility (data and access).

**Q29. A Region contains multiple isolated locations for high availability. They are called…**
**Ans: C — Availability Zones**
Each AZ is one or more discrete data centers with independent power, cooling and networking.

**Q30. Choosing a support plan based on response times and technical assistance — compare what?**
**Ans: A — Support features and response targets**
Support plans differ in response SLAs, contact channels, Trusted Advisor checks and TAM access.

**Q31. AWS manages the physical servers and data centers. Which responsibility is this?**
**Ans: A — Security of the cloud**
"Of the cloud" = AWS side. "In the cloud" = customer side.

**Q32. Does a higher support plan increase EC2 CPU performance?**
**Ans: D — No**
Support plans only change the help you receive. Performance depends on the instance type and size.

**Q33. Who is responsible for securing the physical AWS data center?**
**Ans: A — AWS**
Physical security is entirely AWS's responsibility; customers can never enter a data center.

**Q34. Policy allows s3:GetObject only. The user tries to delete an object.**
**Ans: B — Delete is implicitly denied**
Anything not explicitly allowed is denied by default. (Implicit deny — no explicit Deny statement is needed.)

**Q35. Customer chooses an insecure OS configuration on EC2. Who secures the guest OS?**
**Ans: C — Customer**
The guest OS inside the instance is the customer's job; AWS manages only the host OS and hypervisor.

**Q36. A policy created directly inside one IAM user, not meant for reuse.**
**Ans: C — Inline**
An inline policy has a one-to-one relationship with the identity and is deleted along with it.

**Q37. A developer needs permission to assume ProductionRole.**
**Ans: C — sts:AssumeRole**
The action belongs to the STS service, not S3, IAM or EC2.

**Q38. A role should be assumable by only one specific IAM user. Which Principal?**
**Ans: B — The specific IAM user**
Put that user's ARN in the trust policy: `"Principal": { "AWS": "arn:aws:iam::123456789012:user/name" }`. Anything broader over-grants access.

**Q39. Why does an IAM policy contain multiple statements?**
**Ans: A — Each statement can define a separate permission rule**
One statement can allow S3 reads while another denies deletes — different Effect/Action/Resource/Condition sets.

**Q40. AWS provides secure data centers, customer configures application security. Which model?**
**Ans: D — Shared Responsibility Model**
This is the definition of the model.

**Q41. "AWS Support Plan and AWS Free Tier are the same thing." Correct response?**
**Ans: C — Support plans provide support services; Free Tier relates to eligible AWS usage**
Support plans are about technical help and response times. Free Tier is about free usage limits (12-months free, always free, trials).

**Q42. Which statement best describes the root user?**
**Ans: C — It has broad access to the AWS account**
It has unrestricted access, is never deleted, and cannot be limited by an IAM policy (only an Organizations SCP can restrict it).

**Q43. Extremely low latency for users near one specific metropolitan area.**
**Ans: C — Local Zone**
Local Zones place compute and storage close to a large city. Edge Locations only cache content; they do not run your application.

**Q44. After assuming a role, which permissions determine what the user can do?**
**Ans: B — Role's permissions policies**
While the role is active the user's own permissions are dropped — only the role's permissions apply.

**Q45. Which identity is created during AWS account signup?**
**Ans: C — Root user**
The sign-up email becomes the root user. IAM users, groups and roles are created manually afterwards.

---

## Points to remember from this test

- **Explicit Deny > Allow > implicit deny.** Anything not allowed is denied by default.
- **Roles for temporary access**, IAM users for permanent identities, groups for teams.
- **AssumeRole needs both sides** — the role's trust policy *and* the user's `sts:AssumeRole` permission.
- **Trust policy = WHO**, permissions policy = **WHAT**.
- **Root-only tasks:** close the account, change support plan, change root email/password, move to Organizations, fix a broken S3/SQS policy.
- **Support plans:** Basic (free) → Developer (business hours, email) → Business (24×7, prod down < 1 h) → Enterprise (business-critical < 15 min, TAM). Support plans never change performance.
- **Infrastructure words:** Region = geographic area · AZ = isolated data centers for HA · Edge Location = CloudFront cache · Local Zone = low latency near a city.
- **Managed policy = reusable**, inline policy = one identity only, permissions boundary = maximum cap.
