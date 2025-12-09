---
title: "Event 4"
date: "2025-09-09"
weight: 4
chapter: false
pre: " <b> 4.4. </b> "
---


# Summary Report: "AWS Cloud Mastery Series #3: AWS Well-Architected Security Pillar"

### Event Objectives

- Understanding the basics of Security (The Foundation).
- Controlling who gets in and what they can do (Identity & Access Management).
- Spotting the bad guys (Detection).
- Protecting the servers and the data (Infrastructure & Data Protection).
- What to do when things go wrong (Incident Response).

### Speakers

- **Le Vu Xuan An**
- **Tran Duc Anh**
- **Tran Doan Cong Ly**
- **Danh Hoang Hieu Nghi**
- **Thinh Lam**
- **Viet Nguyen**
- **Mendel Branski (Long)**

### Key Highlights

#### Introduction to Cloud Clubs

- Got to know about Cloud Clubs at universities like UTE and SGU. It's cool to see student communities active in this field.

#### Security Foundation

- **SCPs**: The big rules for the whole organization.
- **Permission Boundaries**: Setting limits so users can't give themselves too much power.
- **MFA**: The absolute must-have for security.

#### Detection and Monitoring

- **Multi-Layer Security**: Checking security at every level, not just the front door.
- **Alerting**: Using EventBridge to wake us up when something suspicious happens.
- **Detection-as-Code**: Writing code to automatically find threats (sounds fancy and super useful).

#### Incident Response

- **Prevention**: The best incident is the one that never happens.
- **"Sleeping better"**: Setting things up so you don't get paged at 3 AM.
- Learning the step-by-step process of fixing things when they break.

### Key Takeaways

#### Service Control Policies (SCPs)

- Think of this as the "Constitution" for your AWS accounts.
- It controls the *maximum* permission available. Even if you are an Admin, if the SCP says "No", it means **No**.
- It never grants permission; it only filters/blocks things.

#### Permission Boundaries

- A way to stop a user (or a role) from becoming too powerful. It sets a "ceiling" on what they can ever do, no matter what other policies say.

#### Multi-Factor Authentication (MFA) - Just do it!

- **TOTP**: The standard Google Authenticator codes we use. It's free and works well.
- **FIDO2**: The physical USB keys (like YubiKey). More secure, requires a touch, harder to hack.

#### Detection-as-Code

- Using tools like CloudFormation/Terraform to automatically set up **GuardDuty** (the threat detector) everywhere.
- **Version Control**: Storing your security rules in Git so you can track changes just like software code.

#### Incident Response

- **Prepare**: Have automation ready *before* the hack happens.
- **Lesson Learned**: After fixing the issue, always look back to see why it happened so it doesn't happen again.

### Applying to Work

- **Least Privilege**: Go back and check permissions. Don't give "AdminAccess" to everyone anymore.
- **Enforce MFA**: Make sure every account uses MFA.
- **Plan ahead**: Think about "What if this breaks?" and have a plan ready.

### Event Experience

Attending the **“AWS Well-Architected Security Pillar”** workshop helped me realize that security isn't just about firewalls; it's a whole mindset.

#### Learning from the Seniors
- Listening to senior engineers talk about how they handle real incidents was eye-opening. They focused a lot on "lessons learned" rather than just fixing the bug.

#### Networking with Cloud Clubs
- It was nice to see the energy from the university Cloud Clubs. It connects learners like me from everywhere.

#### Real-time Defense
- I learned that we can't watch screens 24/7. We need tools like CloudTrail and EventBridge to watch the system for us and alert us immediately.


> Overall, this event expanded my view on Automation and Security. It's not just about locking doors; it's about having a smart alarm system and knowing what to do when it goes off.