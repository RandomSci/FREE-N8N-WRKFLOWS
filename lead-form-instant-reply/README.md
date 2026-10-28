# Lead Form With Instant Reply (n8n)

A web form that replies to every lead in about one second, sorts hot leads, and saves every lead to a table. No custom code and no paid tools. Built in the Selwyn Builds tutorial **"How to Create a Workflow in n8n (Beginner Tutorial, Step by Step)"**, episode 1 of the free n8n Full Course for Beginners.

Watch it here https://youtu.be/IMYMcydk39k

![Workflow](screenshot.png)

## What it does

- A hosted n8n form asks for name, email, business type and leads per month.
- **Clean the data** trims the name, lowercases the email and turns leads per month into a number.
- **Is it a hot lead?** checks if leads per month is 50 or more.
- **Hot reply** and **Standard reply** write a personal message with the lead's own details.
- **Save lead** inserts the lead into a data table.
- **Show reply** shows the personal message on screen right after the form is sent.

## Nodes used

| Node | What it does |
|---|---|
| New lead form (n8n Form Trigger) | Hosted form with 4 fields |
| Clean the data (Edit Fields) | Trims and formats every answer |
| Is it a hot lead? (If) | Leads per month greater than or equal to 50 |
| Hot reply and Standard reply (Edit Fields) | Priority and a personal reply, other fields kept |
| Save lead (Data table) | Insert row, map automatically |
| Show reply (n8n Form, form ending) | Shows the reply to the visitor |

## Setup

1. In n8n, create a data table called **Leads** with the columns `name`, `email`, `business_type`, `leads_per_month` (number), `priority` and `reply`.
2. Create a new workflow, open the menu at the top right, choose **Import from File**, and pick `workflow.json`.
3. Open **Save lead** and select your **Leads** table.
4. Change the form title and the two reply messages to fit your business.
5. Click **Execute workflow**, fill in the test form, and check the new row in the table.
6. Click **Publish** and share the production form link.

## Credentials you need to add

None. Everything runs inside n8n.

Made by [Selwyn Builds](https://selwynbuilds.com). Free course and community https://whop.com/selwyn-builds/
