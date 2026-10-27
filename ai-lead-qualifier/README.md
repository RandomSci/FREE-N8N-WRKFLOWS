# AI Lead Qualifier (n8n)

The AI Agent node reads every lead from a form, scores it hot, warm or cold, writes a reply about the lead's exact problem, and saves everything to a data table. Built in the Selwyn Builds tutorial **"n8n AI Agent Tutorial for Beginners (Add AI to Any Workflow)"**, episode 2 of the free n8n Full Course for Beginners.

Watch it here https://youtu.be/TBenLsAKHy0

![Workflow](screenshot.png)

## What it does

- The form asks for name, email, business type, leads per month and **what they need help with**.
- **Clean the data** formats every answer.
- **AI Agent** gets the lead in its prompt and a system message with the scoring rules.
- **Structured Output Parser** forces the answer into three fields, `priority`, `reason` and `reply`.
- **Prepare the lead** joins the lead with the AI's answer.
- **Save lead** inserts it into a data table, and **Show reply** shows the AI's reply on screen.

## Nodes used

| Node | What it does |
|---|---|
| New lead form (n8n Form Trigger) | Hosted form with 5 fields |
| Clean the data (Edit Fields) | Trims and formats every answer |
| AI Agent | Scores the lead and writes the reply |
| OpenAI Chat Model | The language model, temperature 0.3 |
| Structured Output Parser | JSON example with priority, reason and reply |
| Prepare the lead (Edit Fields) | Lead fields plus the AI fields |
| Save lead (Data table) | Insert row, map automatically |
| Show reply (n8n Form, form ending) | Shows the reply to the visitor |

## Setup

1. Create a data table called **AI Leads** with the columns `name`, `email`, `business_type`, `leads_per_month` (number), `need`, `priority`, `reason` and `reply`.
2. Import `workflow.json` with **Import from File**.
3. Open **OpenAI Chat Model**, add your OpenAI credential (or any OpenAI compatible provider by changing the Base URL), and pick a small, fast model.
4. Open **Save lead** and select your **AI Leads** table.
5. Edit the system message in **AI Agent** with your own scoring rules.
6. Execute the workflow, fill in the form as a hot lead and as a cold lead, and compare the replies.

## Credentials you need to add

- **OpenAI** (or any OpenAI compatible provider).

## Tips

- Always use a structured output, so the next nodes get the same fields every time.
- Keep the temperature low and the rules clear.
- Check the table every week and adjust the system message if a lead is scored wrong.

Made by [Selwyn Builds](https://selwynbuilds.com). Free course and community https://whop.com/selwyn-builds/
