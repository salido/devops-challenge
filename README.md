# SALIDO DevOps Challenge

This challenge is provided to help assess a candidate's development skills. There is no time limit, but we ask that you be prompt in submitting your solution.

## Submission Instructions

1. Complete the challenge according to the Challenge Instructions below.
1. Deploy your code on the internets so we can see it in action.
1. Create a GitHub repository and push your code to it.
1. Include a README in your repository to explain and document your solution.
1. Send an email to challenge-accepted@salido.com that includes:
  * A link to your live solution.
  * A link to your GitHub repository.
  * Database credentials.

## Resources

* AWS provides free and low-cost resources for anything you may need to complete this challenge. (https://aws.amazon.com)
* mLab provides free hosted instances of MongoDB. (https://mlab.com)
* Heroku provides free containers for hosting apps and services. (https://www.heroku.com)
* You are encouraged to use DevOps tools, such as [Puppet](https://puppet.com), [Chef](https://www.chef.io) or [Salt](https://saltstack.com).

## Challenge Instructions

### Objective
Create a "one click" process for cloning production data into other environments.

### Intro
SALIDO has four environments to which it deploys code: **Development**, **Staging**, **Demo** and **Production**.
We often need to clone data from our production MongoDB into other environments so we can test and demo against data that is consistent with production.

Each customer is stored in our database as an "organization".
Most documents in the database are associated with an organization via direct or indirect relationships.
For example, an organization has many brands; a brand has many locations; a location has many checks; a check has many payments.
HINT: Use these relationships to find all of the documents that ultimately belong to an organization.

When cloning data from production, the current environment's data is purged (except for certain things as described in the business requirements), and the production data is pulled in to replace it.
We need the ability to specifically select which organizations should be cloned from production, and which organizations (if any) should be retained in the current environment.

**IMPORTANT: You are encouraged to ask questions when you need clarification on any part of this challenge.**

### Business Requirements
* Data that cannot be associated (directly or indirectly) with an organization should never be purged or cloned.
* The user must be able to select which organizations to retain in the current environment. Data associated with all other organizations will be purged.
* The user must be able to select which organizations to clone from production data. Only the data associated with the selected organizations will be cloned.
* Cloned production data must be sanitized as follows:
  * All email addresses must be replaced with mock email addresses: `fake.email+{UUID}@salido.com` (where `{UUID}` is a UUID with no dashes).
  * Credit card gateway configurations must all be sandboxed. This simply means setting `sandbox = true` for all documents in the `cc_gateways` collection.
* The following collections must be explicitly excluded from the cloned production data:
  * `banks`
  * `cc_gateway_transactions`
  * `checks`
  * `shifts`

### Requirements for Mock Email Addresses
Email addresses are found in two places:

* In the `employees` collection in the `personal_email` property.
* In the `emails` collection in the `address` property.

**If a value at `employees.personal_email` matches a value at `emails.address` in the production data, then the mock email addresses must also match.**

### To Do
1. Use the MongoDB dumps provided to populate databases for each environment: Development, Staging, Demo and Production.

2. Review the database collections to familiarize yourself with how things are generally structured, and how documents are associated with each other.

3. Build a tool that performs the cloning process according to the business requirements.

4. Create a single-page website that serves as a UI for this tool, and allows a user to:
  * Select the environment into which Production data will be cloned (either Development, Staging or Demo).
  * Select organizations to clone from production.
  * Select organizations to retain in the destination environment.
  * Invoke the cloning process.

5. Display some sort of progress indicator on the website while the cloning process is running. Some ideas:
  * Progress bar.
  * Percentage complete.
  * A "live log" that displays console output as it's happening.

6. When cloning is complete, produce a log that includes pertinent information about the cloning process, and what data was cloned, retained, purged, etc.

7. Display appropriate and informative success and failure messages as needed.
