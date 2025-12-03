# ✅ 1. Slack Notification (only when changes found)

Slack uses an **incoming webhook URL**.
Store it safely in GitHub Secrets:

```
SLACK_WEBHOOK_URL
```

### **Slack Step**

```yaml
      - name: Notify Slack (changes detected)
        if: steps.changes.outputs.changed == 'true'
        run: |
          curl -X POST -H 'Content-type: application/json' \
            --data "{
              \"text\": \":rocket: *New commits detected on develop!*  
              Scheduled build has started.  
              Commit: ${{ steps.head.outputs.current_commit }}\"
            }" \
            ${{ secrets.SLACK_WEBHOOK_URL }}
```

---

# ✅ 2. Microsoft Teams Notification (only when changes found)

Teams also uses an **incoming webhook**.
Store it in GitHub Secrets:

```
TEAMS_WEBHOOK_URL
```

### **Teams Step**

```yaml
      - name: Notify Teams (changes detected)
        if: steps.changes.outputs.changed == 'true'
        run: |
          curl -H 'Content-Type: application/json' -d "{
              \"title\": \"New Develop Branch Changes Detected\",
              \"text\": \"A new merge/commit was detected on 'develop'.  
              Scheduled build has started.  
              Commit: ${{ steps.head.outputs.current_commit }}\"
          }" \
          ${{ secrets.TEAMS_WEBHOOK_URL }}
```

---

# ✅ Full Integrated Snippet (Add AFTER "Detect changes" step)

Just paste it after this block in your workflow:

```yaml
- name: Detect if new commits exist
  id: changes
  ...
```

### **Complete Notification Section**

```yaml
      # Notify Slack
      - name: Notify Slack
        if: steps.changes.outputs.changed == 'true'
        run: |
          curl -X POST -H 'Content-type: application/json' \
            --data "{
              \"text\": \":bell: New commits detected on *develop*!  
              Build triggered automatically.  
              Commit: ${{ steps.head.outputs.current_commit }}\"
            }" \
            ${{ secrets.SLACK_WEBHOOK_URL }}

      # Notify Teams
      - name: Notify Teams
        if: steps.changes.outputs.changed == 'true'
        run: |
          curl -H 'Content-Type: application/json' -d "{
            \"title\": \"Develop Build Triggered\",
            \"text\": \"New commit detected on 'develop'.  
            Commit: ${{ steps.head.outputs.current_commit }}\"
          }" \
          ${{ secrets.TEAMS_WEBHOOK_URL }}
```

---

# 🚀 What You Will See

### Slack

A message like:

> 🔔 New commits detected on *develop*!
> Build triggered automatically.
> Commit: `abc1234`

### Teams

A card with:

**Develop Build Triggered**
New commit detected on develop
Commit: `abc1234`

