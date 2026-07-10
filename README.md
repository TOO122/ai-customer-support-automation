# AI-Powered Customer Support Automation

**Built by:** Nnoli Godslove Chito

**Platform:** Make.com

**Tools:** Groq AI · Gmail · Airtable · Slack

**Project 2 of 5 | AI Automation Portfolio**

---

## What This Project Does

Customer support teams spend hours every day manually reading emails, deciding what type of issue each one is, writing responses, and logging tickets manually. This project automates that entire process.

The moment a support email arrives in Gmail, Groq AI reads it and classifies it as **Billing**, **Technical**, **General**, or **Urgent**. The correct auto-reply is sent instantly, the ticket is logged in Airtable, and if the issue is urgent, the support team receives an immediate Slack notification.

The whole workflow runs automatically without human intervention.

---

## The Problem It Solves

A small support team receiving dozens of emails every day cannot efficiently sort, respond to, and track every customer request manually.

Urgent issues become buried, customers wait too long for replies, and support tickets are difficult to manage.

This automation solves all of these problems by automatically classifying, replying to, logging, and escalating support requests.

---

## System Architecture

```text
Customer sends email
        |
        v
Gmail (Watch Emails trigger)
        |
        v
HTTP Module calls Groq AI API
        |
        v
Groq AI classifies email
(Returns: Billing / Technical / General / Urgent)
        |
        v
Router splits flow by category
        |
   _____|_____________________________
   |          |          |           |
   v          v          v           v
Billing   Technical   General    Urgent
   |          |          |           |
   v          v          v           v
Gmail      Gmail      Gmail       Gmail
Reply      Reply      Reply       Reply
   |          |          |           |
   v          v          v           v
Airtable  Airtable  Airtable    Airtable
  Log       Log       Log         Log
                                    |
                                    v
                               Slack Alert
```
---

## Tools and Their Roles

### Make.com
Make.com is the core automation platform that connects all the tools together and orchestrates the entire workflow from start to finish.

### Gmail
Gmail receives incoming customer support emails and sends automated replies back to customers based on the AI classification.

### Groq AI (via HTTP Module)
Groq AI reads the content of each incoming email and classifies it into one of four categories:

- Billing
- Technical
- General
- Urgent

The HTTP module is used because Make.com does not currently provide a native Groq integration.

### Router
The Router receives the classification returned by Groq AI and directs each email down the appropriate workflow path.

### Airtable
Airtable automatically logs every support ticket, including:

- Customer Name
- Email
- Subject
- Message
- Category
- Status
- Date Received

### Slack
Slack sends an instant notification to the support team whenever an **Urgent** support ticket is received.

---

## Auto-Reply Messages

### Billing

> Thank you for contacting us regarding your billing enquiry. Our billing team has received your message and will respond within **24 hours**.

### Technical

> Thank you for contacting us. Our technical team has received your issue and will respond within **4 hours**.

### General

> Thank you for contacting us. We have received your enquiry and will respond within **48 hours**.

### Urgent

> **URGENT:** We have received your message and a senior agent will contact you within **1 hour**.

---

## Airtable Fields

The **Support Tickets** table in Airtable captures the following information for every incoming support request:

| Field | Description |
|--------|-------------|
| Customer Name | Mapped from the Gmail sender's name |
| Email | Mapped from the Gmail sender's email address |
| Subject | Mapped from the Gmail subject line |
| Message | Mapped from the full email body |
| Category | Mapped from the Groq AI classification |
| Status | Automatically set to **Open** |
| Date Received | Mapped from the Gmail received date |

---

## Groq AI Prompt

### System Prompt

```text
You are a customer support classifier.

Classify the email into exactly one of these categories:

Billing
Technical
General
Urgent

Reply with only the category word and nothing else.
```

### User Message

```text
The full text body of the incoming email from Gmail.
```

---

## Slack Alert Message

Whenever an **Urgent** ticket is detected, the following Slack notification is sent to the support team:

```text
🚨 URGENT Support Ticket Received!

From: customer name

Email: customer email address

Subject: email subject line
```

---

## Test Results

Four test emails were sent to verify that the automation worked correctly across all support categories.

### Billing Test

A billing dispute email about being charged twice was correctly classified as **Billing**.

**Result:**

- ✅ Correct auto-reply sent
- ✅ Ticket logged in Airtable
- ✅ No Slack notification triggered

---

### Technical Test

An email describing an application crash was correctly classified as **Technical**.

**Result:**

- ✅ Correct auto-reply sent
- ✅ Ticket logged in Airtable
- ✅ No Slack notification triggered

---

### General Test

A customer enquiry about business hours was correctly classified as **General**.

**Result:**

- ✅ Correct auto-reply sent
- ✅ Ticket logged in Airtable
- ✅ No Slack notification triggered

---

### Urgent Test

An email reporting that a customer's account had been hacked was correctly classified as **Urgent**.

**Result:**

- ✅ Priority auto-reply sent
- ✅ Ticket logged in Airtable
- ✅ Slack notification sent immediately

---

**All four test scenarios passed successfully.**

---

## Key Technical Decisions

### Why use the HTTP Module instead of a native Groq integration?

Make.com does not currently provide a native Groq module. Calling the Groq API through the HTTP module is the standard approach for integrating Groq AI into Make.com workflows.

---

### Why use AI classification instead of keyword filters?

Keyword filters are limited because customers rarely use the exact words you expect.

For example, a customer might write:

> "I cannot get into my account."

instead of

> "My account has been hacked."

A keyword filter could easily miss this, while Groq AI understands the meaning and context of the message, resulting in more accurate classification.

---

### Why do only Urgent tickets trigger a Slack notification?

Sending Slack notifications for every incoming email would create unnecessary noise and reduce their effectiveness.

Only **Urgent** tickets require immediate attention, so Slack notifications are reserved for those cases.

---

### Why is every ticket created with the status **Open**?

This automation handles ticket intake only.

Support agents update the ticket status to:

- **In Progress** when work begins.
- **Resolved** once the issue has been completed.

This follows the same workflow used by enterprise helpdesk platforms such as Zendesk.

---

## Screenshots

Create a folder named:

```text
screenshots/
```

Add the following screenshots to the folder:

1. Full Make.com scenario canvas
2. Airtable Support Tickets table
3. Gmail auto-reply received by the customer
4. Slack urgent notification
5. Groq AI HTTP module configuration
6. Router filter configuration

---

## Business Impact

Before implementing this automation, the support team spent several hours every morning manually reading emails, categorising support requests, and replying to customers.

Urgent issues could easily become buried beneath routine enquiries, resulting in slower response times and inconsistent ticket tracking.

After implementing this workflow:

- Every email receives an automated response within seconds.
- Every support ticket is logged automatically in Airtable.
- Urgent issues trigger an immediate Slack notification.
- Support agents spend more time solving customer problems instead of managing the inbox.

  ---

## What This Project Demonstrates

This project demonstrates:

- AI integration into a real-world business workflow.
- Multi-branch conditional routing using Make.com Router.
- CRM data management with Airtable.
- Prompt engineering for consistent AI classification.
- Automated email responses using Gmail.
- Team collaboration through Slack notifications.
- End-to-end workflow automation that reduces manual work and improves customer response times.

---

## Project Structure

```text
ai-customer-support-automation/
├── screenshots/
├── ai-customer-support-workflow.json
└── README.md
```

---

## Author

**Nnoli Godslove Chito**

AI Automation Engineer | No-Code Developer | Workflow Automation Specialist

---

## How to Recreate This Project

### Step 1

Create an Airtable base called **Customer Support** with a table named **Support Tickets**.

Add the following fields:

- Customer Name *(Single line text)*
- Email *(Email)*
- Subject *(Single line text)*
- Message *(Long text)*
- Category *(Single select: Billing, Technical, General, Urgent)*
- Status *(Single select: Open, In Progress, Resolved)*
- Date Received *(Date)*

---

### Step 2

Create a new Make.com scenario.

Add a **Gmail – Watch Emails** module.

Configure it as follows:

- Folder: Inbox
- Content Format: Full content
- Limit: 1

---

### Step 3

Add an **HTTP – Make a Request** module.

Configure:

- URL:
  ```
  https://api.groq.com/openai/v1/chat/completions
  ```

- Method:
  ```
  POST
  ```

Headers:

- Content-Type: `application/json`
- Authorization: `Bearer YOUR_GROQ_API_KEY`

Body type:

```
application/json
```

Add the system prompt together with the mapped Gmail email body.

---

### Step 4

Add a **Router** module after the HTTP module.

Create four routes with filters:

- Billing
- Technical
- General
- Urgent

Each filter checks whether the Groq AI response equals the corresponding category.

---

### Step 5

On every route, add a **Gmail – Reply to an Email** module.

Configure:

- Thread ID → map from Gmail trigger
- Reply Mode → Reply to sender only
- Body Type → Raw HTML

Write the appropriate auto-reply for each category.

---

### Step 6

After every Gmail reply module, add an **Airtable – Create a Record** module.

Select:

- Base: Customer Support
- Table: Support Tickets

Map:

- Customer Name
- Email
- Subject
- Message
- Category
- Status
- Date Received

---

### Step 7

Only on the **Urgent** route, add a **Slack – Send a Message** module.

Configure:

- Support Channel
- Customer Name
- Customer Email
- Email Subject

---

### Step 8

Test the scenario by sending one email for each category:

- Billing
- Technical
- General
- Urgent

Verify:

- ✅ Correct AI classification
- ✅ Correct Gmail auto-reply
- ✅ Airtable ticket created
- ✅ Slack notification for Urgent only

