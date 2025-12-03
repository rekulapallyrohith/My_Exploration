# ✅ How to Generate Microsoft Teams Incoming Webhook URL

### **1. Open Microsoft Teams**

Go to the **Team → Channel** where you want to receive notifications.

Example:

```
Team: DevOps
Channel: CI-CD Alerts
```

---

# **2. Add Incoming Webhook Connector**

### In the channel:

1. Click the **⋯ (More options)** next to the channel name
2. Click **Connectors**

If you don’t see it, click:
**Manage channel → Connectors**

---

# **3. Search for “Incoming Webhook”**

Click **Configure → Add**

---

# **4. Set up the webhook**

* Give it a name (example: `GitHub Actions`)
* Optionally upload an icon
* Click **Create**

---

# **5. Copy the generated Webhook URL**

Teams will generate a URL like:

```
https://outlook.office.com/webhook/xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx/IncomingWebhook/xxxxxxxxxxxxxxxxxxxxxxxx/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
```

This long URL is the **actual value** for:

```
TEAMS_WEBHOOK_URL
```

---

# **6. Save it in GitHub Secrets**

Go to:

**GitHub Repo → Settings → Secrets → Actions → New repository secret**

Name:

```
TEAMS_WEBHOOK_URL
```

Value:

```
(paste the webhook URL copied from Teams)
```

---

# 🎉 Done!

Now GitHub Actions can send formatted messages to your Teams channel.
