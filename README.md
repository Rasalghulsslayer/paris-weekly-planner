# Paris Weekly Events Newsletter 🎭🎬🎵

An automated weekly newsletter that curates the best events happening in Paris, delivered every Monday at 7 AM with an interactive, filterable dashboard.

---

## 📋 Features

- **Automated Weekly Aggregation** — Fetches events from 9+ trusted Paris event sources
- **Smart Filtering** — Categorized by: Pop-ups, Sports, Live Music, Food/Dining, Cinema, Theater, Comedy, Dance, Exhibitions, Galleries
- **Interactive Dashboard** — Color-coded cards, filterable by category, chronologically sorted, clickable links to event pages
- **Multi-Channel Delivery** — Slack notification + Email summary every Monday morning
- **Cross-Referenced Data** — Deduplicates events across multiple sources for accuracy

---

## 🚀 Quick Start

### Prerequisites
- GitHub account (repo created: https://github.com/Rasalghulsslayer/paris-weekly-planner)
- Vercel or Netlify account (free tier works)
- Slack workspace (for notifications)
- Gmail account (for email setup)

### Step 1: Set Up Vercel/Netlify

#### Option A: Vercel (Recommended)

1. Go to [https://vercel.com](https://vercel.com) and sign in with GitHub
2. Click "Add New..." → "Project"
3. Select `paris-weekly-planner` repo from your GitHub account
4. Click "Import"
5. **Project Settings:**
   - Framework: None (static HTML)
   - Build Command: (leave empty)
   - Output Directory: `public` or `.` (wherever the generated HTML lives)
6. Click "Deploy"
7. You'll get a live URL like: `https://paris-weekly-planner.vercel.app`

#### Option B: Netlify

1. Go to [https://netlify.com](https://netlify.com) and sign in with GitHub
2. Click "Add new site" → "Import an existing project"
3. Select GitHub, authorize, then choose `paris-weekly-planner`
4. **Build Settings:**
   - Build command: (leave empty)
   - Publish directory: `.` (root, or adjust to where HTML is generated)
5. Click "Deploy site"
6. You'll get a live URL like: `https://paris-weekly-planner-abc123.netlify.app`

Once deployed, note your **live URL** — you'll need it in the next step.

---

### Step 2: Create a GitHub Personal Access Token

The automated routine needs permission to commit event data to this repo. Here's how to create a token:

1. Go to [https://github.com/settings/tokens](https://github.com/settings/tokens)
2. Click "Generate new token" → "Generate new token (classic)"
3. **Token settings:**
   - Token name: `paris-weekly-planner-routine`
   - Expiration: 90 days (or "No expiration" if you prefer)
   - Scopes: Check only `repo` (full control of private repositories)
4. Click "Generate token"
5. **Copy the token immediately** (you won't see it again)
6. Store it securely — you'll provide it to the routine setup

---

### Step 3: Push Initial Files to GitHub

The routine will auto-generate the dashboard each week, but you need a base repository structure. Here's how to initialize it:

```bash
# Clone the repo locally
git clone https://github.com/Rasalghulsslayer/paris-weekly-planner.git
cd paris-weekly-planner

# Create initial folder structure
mkdir -p public
touch public/index.html

# Create a placeholder index.html (the routine will replace this each week)
cat > public/index.html << 'EOF'
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Paris Weekly Events</title>
    <style>
        * { margin: 0; padding: 0; box-sizing: border-box; }
        body { font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif; background: #f5f5f5; padding: 20px; }
        .header { text-align: center; margin-bottom: 30px; }
        .header h1 { color: #333; margin-bottom: 10px; }
        .loading { text-align: center; color: #999; padding: 40px; }
    </style>
</head>
<body>
    <div class="header">
        <h1>🗼 Paris Weekly Events</h1>
        <p>Loading this week's events...</p>
    </div>
    <div class="loading">
        <p>Dashboard will be generated every Monday at 7 AM</p>
    </div>
</body>
</html>
EOF

# Commit and push
git add .
git commit -m "Initial commit: placeholder dashboard"
git push origin main
```

---

### Step 4: Connect Slack Notification

The routine will post to your **#Paris-Weekly-Planner** Slack channel each Monday.

To verify your Slack workspace is ready:
1. Go to your Slack workspace
2. Create a channel called `#paris-weekly-planner` (if it doesn't exist)
3. The routine will have access to post there automatically

---

## 📊 How the Routine Works

Every Monday at 7 AM Paris time, the automated routine:

1. **Fetches events** from these sources:
   - paris.fr/quefaire
   - parissecret.com
   - highsnobiety.com
   - views.fr
   - sortiraparis.com
   - timeout.fr/paris
   - parisjetaime.com
   - feverup.com
   - lefooding.com

2. **Filters & categorizes** by your ranked interests:
   - 🎪 Pop-ups (#FF6B6B)
   - ⚽ Sports (#4ECDC4)
   - 🎵 Live Music (#FFE66D)
   - 🍽️ Food/Dining (#FF9FF3)
   - 🎬 Cinema (#54A0FF)
   - 🎭 Theater (#A29BFE)
   - 😂 Comedy (#6C5CE7)
   - 💃 Dance (#00B894)
   - 🖼️ Exhibitions (#FDCB6E)
   - 🎨 Galleries (#E17055)

3. **Generates an interactive HTML dashboard** with:
   - Color-coded cards (one per event)
   - Filter buttons by category
   - Chronological sorting
   - Animated entrance effects
   - Direct links to event pages
   - Mobile-friendly design

4. **Commits the dashboard** to this repo
   - Vercel/Netlify auto-deploys instantly
   - Updated dashboard is live at your configured URL

5. **Sends notifications:**
   - **Slack:** Summary + link to live dashboard in #paris-weekly-planner
   - **Email:** akadosthomas@gmail.com with top 3 "don't miss" events + dashboard link

---

## 🎨 Dashboard Features

### Interactive Cards
- **Category Badge** — Color-coded label (e.g., 🎵 Live Music)
- **Event Details** — Name, date/time, venue, brief description
- **Click-Through** — Click any card to jump to the event's official page

### Filter Bar
- **Toggle Categories** — Click category buttons to filter dashboard
- **Smart Filtering** — Show/hide events by type without page reload
- **Visual Feedback** — Active filters are highlighted

### Sorting & Layout
- **Chronological** — Events sorted earliest → latest in the week
- **Responsive Grid** — Adapts to desktop, tablet, mobile
- **Animations** — Smooth fade-in effects, hover transitions

---

## 🛠️ Customization

### Change Event Categories or Colors

Edit the routine's category list (provided in routine setup). Each category has an associated hex color code.

### Add or Remove Event Sources

The routine fetches from a hardcoded list of 9 sources. To modify:
1. Update the routine prompt with new source URLs
2. Re-run the routine (`/schedule run <routine-id>`)

### Adjust Notification Time

The routine runs **Monday 7 AM Paris time** (6 AM UTC, accounting for winter time). To change:
1. Go to https://claude.ai/code/routines
2. Edit the routine's cron expression
3. Save changes

---

## 🐛 Troubleshooting

### Dashboard isn't updating
- Check the routine logs at https://claude.ai/code/routines
- Verify Vercel/Netlify deployment succeeded (check their dashboards)
- Confirm GitHub PAT token hasn't expired

### Slack notification didn't arrive
- Verify #paris-weekly-planner channel exists in Slack
- Check routine logs for errors during Slack API call
- Confirm the Slack workspace is connected to Claude Code

### Events are duplicated
- The routine has deduplication logic; if it fails, check source data quality
- Some sources may list the same event differently — this is expected occasionally

### Missing events from a source
- If a source is temporarily down, the routine continues with remaining sources
- Check routine logs to see which sources failed
- Re-run the routine manually to retry failed sources

---

## 📅 Calendar

This routine runs automatically every Monday morning. You can also:
- **Manually trigger** a routine run anytime from https://claude.ai/code/routines
- **Pause the routine** during vacations (disable it temporarily)
- **Update preferences** anytime by editing the routine prompt

---

## 🔐 Security & Privacy

- **GitHub Token**: Stored securely in the routine environment. Only used to commit dashboard updates.
- **Email**: Only akadosthomas@gmail.com receives the weekly summary.
- **Slack**: Messages posted only to #paris-weekly-planner.
- **Event Data**: Sourced from public websites; no personal data is collected or stored.

---

## 📞 Support

If something breaks:
1. Check the routine logs at https://claude.ai/code/routines
2. Review this README's troubleshooting section
3. Manually re-run the routine to test
4. If issues persist, DM Thomas on Slack or email thomas.dos@nomadic-labs.com

---

**Enjoy exploring Paris! 🗼✨**
