# Whale Channel Reply System

**Automated YouTube comment lead generation using n8n + GPT-4**

An n8n workflow that finds leads in YouTube comment sections automatically by:
- Searching YouTube for target topics (day-based rotation)
- Scraping comments from recent videos
- Filtering high-intent leads using GPT-4
- Generating peer-to-peer replies automatically
- Auto-posting via YouTube API
- Logging everything to Google Sheets for monitoring

---

## ⚠️ CRITICAL WARNINGS

**READ THIS BEFORE USING:**

- ✋ **This workflow AUTO-POSTS replies to YouTube.** You will get banned for spam if you abuse it.
- 📊 **Monitor your Google Sheet audit trail daily.** Check for bad replies, downvotes, or reports.
- 🐌 **Start small.** 10 replies/day max for the first week. Scale slowly.
- 📜 **Respect YouTube's TOS.** Use for legitimate lead generation only, not spam.
- 🔍 **Test deduplication thoroughly.** Replying to the same person twice = instant ban.

**I am not responsible for account bans, suspensions, or reputation damage. Use at your own risk.**

---

## What This Does

### The Flow:
1. **Schedule Trigger** → Runs every 8 hours (6 AM, 2 PM, 10 PM)
2. **Day-Based Search** → Monday = "Automation", Tuesday = "Affiliate Marketing", etc.
3. **YouTube Search** → Finds top 50 videos for that day's topic
4. **Channel Scraping** → Extracts unique channels, gets their 2 latest videos
5. **Comment Scraping** → Pulls top 11 relevant comments per video
6. **Deduplication** → Checks "Comment IDs Database" sheet to avoid duplicate replies
7. **AI Filtering** → GPT-4 analyzes comments, finds high-intent leads (time struggles, scaling pain, automation questions)
8. **Reply Generation** → GPT-4 generates 2-3 sentence peer-to-peer replies (casual, helpful, non-salesy)
9. **Prioritization** → Selects top 20 leads (15 high priority + 5 medium)
10. **Google Sheets Logging** → Records comment, reply, opportunity type, sales potential
11. **AUTO-POST** → Posts reply to YouTube automatically via API

---

## Prerequisites

- **n8n instance** (self-hosted or cloud)
- **YouTube Data API key** ([Get it here - FREE](https://console.cloud.google.com/apis/library/youtube.googleapis.com), 10,000 quota/day)
- **OpenAI API key** (GPT-4 access required)
- **Google Sheets account**
- **YouTube account** for posting (must authenticate OAuth)
- **Telegram Bot** (optional, for notifications)

---

## Cost Breakdown

| Service | Cost per Run | Monthly (3 runs/day) |
|---------|--------------|----------------------|
| OpenAI GPT-4 | ~$0.10 | ~$9.00 |
| YouTube API | Free | Free (quota-based) |
| Google Sheets | Free | Free |
| n8n Hosting | Varies | $0-20 (self-hosted free) |
| **TOTAL** | **~$0.15** | **~$13.50** |

---

## Setup Instructions

### 1. Import Workflow
- Download `Whale Channel Reply System(1).json`
- In n8n: **Import from File** → Select the JSON

### 2. Replace API Keys & Credentials

**YouTube Data API (3 nodes):**
- `HTTP Request` (YouTube Search)
- `HTTP Request1` (Get Channel Details)
- `HTTP Request2` (Get Latest Videos)

Replace `AIzaSyDV5etfLvMaBIVgmrRx8X2QZ-H4sMYZrvQ` with YOUR API key.

**OpenAI:**
- Node: `OpenAI Chat Model`
- Add your OpenAI API key

**Google Sheets (3 nodes):**
- `Append row in sheet` (main output)
- `Get row(s) in sheet1` (deduplication check)
- `Append row in sheet1` (log comment IDs)

Authenticate with OAuth.

**YouTube OAuth:**
- Node: `Reply To Comment`
- Authenticate with the YouTube account you want to post from

**Telegram (optional):**
- Node: `Send a text message2`
- Add your bot token and chat ID

### 3. Create Google Sheets

**Sheet 1: "Whale Channel Reply System"**
- Columns: `Date & Time`, `YouTube Channel URL`, `Video URL`, `CommentID`, `Comment`, `Suggested Reply`, `Opportunity Type`, `Sales Potential`

**Sheet 2: "YouTube Comment IDs Sub Database"**
- Columns: `Comment IDs`

Update Google Sheets node URLs to point to YOUR sheets.

### 4. Customize Search Queries

Edit node: **Code in Python (Beta)**

```javascript
const queries = {
  Monday: "Your Niche Here",
  Tuesday: "Another Topic",
  Wednesday: "YouTube Growth",
  // ... customize all 7 days
};
```

### 5. Test Manually FIRST

- **DO NOT schedule immediately**
- Click "Execute Workflow" manually
- Check Google Sheet output
- Verify replies sound natural
- Confirm deduplication works

### 6. Schedule (After Testing)

- Schedule Trigger cron: `0 */8 * * *` (every 8 hours)
- Start conservative: `0 */12 * * *` (every 12 hours) for first week

---

## Customization Guide

### Change Reply Tone

Edit node: **Prompt**

Key sections to modify:
- `TARGET COMMENTS` → What comments to reply to
- `REPLY RULES` → Tone, length, style
- `GOOD EXAMPLES` → Reference replies for AI

### Adjust Filtering Aggressiveness

Edit node: **Code in Python (Beta)1**

```javascript
const highPriority = replies
  .filter(r => r.Sales_Potential === 'high')
  .slice(0, 15); // Change to 10, 20, etc.
```

### Change Posting Frequency

- Every 6 hours: `0 */6 * * *`
- Every 12 hours: `0 */12 * * *`
- Daily at 9 AM: `0 9 * * *`

### Disable Auto-Posting (Manual Review Mode)

**Disable node:** `Reply To Comment`

Replies will log to Google Sheets but NOT post automatically.

---

## Monitoring & Safety

### Daily Checklist:
✅ Check Google Sheet for new replies  
✅ Look for downvoted or reported comments  
✅ Verify deduplication is working (no duplicates)  
✅ Review AI-generated reply quality  
✅ Check YouTube account health (no warnings)  

### Red Flags (STOP IMMEDIATELY):
🚨 Multiple downvotes on your replies  
🚨 People calling you out as spam  
🚨 Channel owners blocking you  
🚨 Duplicate replies to same person  
🚨 YouTube account warnings  

---

## Troubleshooting

**"No leads found"**
- Search queries too narrow → Broaden topics
- AI filtering too strict → Lower `Sales_Potential` threshold

**"Duplicate replies posted"**
- Deduplication failed → Check "Comment IDs Database" sheet
- `Get row(s) in sheet1` node error → Verify sheet permissions

**"API quota exceeded"**
- YouTube API has 10,000 units/day limit
- Reduce `maxResults` in HTTP Request nodes
- Space out runs (every 12 hours instead of 8)

**"Replies sound robotic"**
- Edit AI prompt examples in `Prompt` node
- Add more natural language variations
- Test with different GPT models (GPT-4o vs GPT-4)

---

## Ethical Use Guidelines

### ✅ DO:
- Reply to genuine questions about automation/workflows
- Offer helpful advice based on your experience
- Position yourself as a peer, not a salesperson
- Monitor and improve reply quality over time
- Respect rate limits and TOS

### ❌ DON'T:
- Spam unrelated channels
- Use for scams or misleading promotions
- Reply to every comment (be selective)
- Ignore downvotes or reports
- Post identical replies across channels

---

## Legal

**License:** MIT (see LICENSE file)

**Disclaimer:** Use at your own risk. The author is not responsible for account bans, suspensions, or reputation damage resulting from use or misuse of this workflow. Always respect platform Terms of Service.

---

## Related Workflows

Check out other workflows in this repo:
- [Automated Content Creation For Business](./Automated%20Content%20Creation%20For%20Business(1).json)
- [Reddit Lead Generation System](./For%20Reddit.json)

---

## Tutorial Video

📺 **Full breakdown:** [YouTube link - add after publishing]

---

## Support

Questions? Open an issue or watch the tutorial video.

**Built by:** [Selwyn Builds](https://youtube.com/@selwynbuilds)  
**Website:** [selwynbuilds.com](https://selwynbuilds.com)
