# AI Booking Agent With Tools (n8n)

An AI agent that books calls by itself. It chats with a visitor, checks which time slots are taken, offers the closest free time, collects the name and email, and saves the booking to a Data Table. Built in the Selwyn Builds tutorial **"n8n AI Agent Tools Tutorial (Your AI Books Calls by Itself)"**, episode 3 of the free n8n Full Course for Beginners.


## What it does

- A hosted chat page lets anyone ask for a call.
- The agent uses the **Check bookings** tool to read every booked slot before it offers a time.
- If the requested time is taken, it offers the closest free time on the same day.
- Once it has a name and email, it uses the **Book a call** tool to insert a new row. The AI fills every column with `$fromAI()`.
- Simple Memory keeps the conversation, so the agent remembers what the visitor already said.

## Nodes used

| Node | What it does |
|---|---|
| When chat message received (Chat Trigger) | Public hosted chat page with an initial message |
| AI Agent (Booking agent) | Decides what to say and which tool to use. The system message holds the booking rules |
| OpenAI Chat Model | The language model, low temperature |
| Simple Memory | Remembers the last 10 interactions |
| Data Table tool, Check bookings | Get rows, return all |
| Data Table tool, Book a call | Insert row, columns filled with `$fromAI()` |

## Setup

1. In n8n, create a Data Table called **Bookings** with 5 text columns: `name`, `email`, `date`, `time`, `topic`.
2. Create a new workflow, open the menu at the top right, choose **Import from File**, and pick `workflow.json`.
3. Open **OpenAI Chat Model**, select your OpenAI credential, and pick a small, fast model.
4. Open **Check bookings** and **Book a call**, and select your **Bookings** table in each.
5. Edit the system message in **Booking agent** with your own business name and hours.
6. Click **Publish**, then open the chat URL from the chat trigger and book a test call.

## Credentials you need to add

- **OpenAI** (or any OpenAI compatible provider, by changing the Base URL in the credential).

## Tips

- Write clear tool descriptions. The agent reads them to decide when to use each tool.
- Put hard rules in the system message, like checking bookings before offering a time.
- Test the tricky cases first: a taken slot, a missing email, a weekend.

Made by [Selwyn Builds](https://selwynbuilds.com).
