# ✅ **Adaptive Card Notification (Rich Teams Message)**

This version shows:

✔ Branch (`develop`)
✔ Trigger time
✔ Commit SHA
✔ Commit message
✔ Author
✔ Button: “View Commit”
✔ Button: “View Build Run”
✔ Color + proper layout

---

# 🔥 **Add this step to your workflow (after detecting changes)**

```yaml
      - name: Notify Teams (Adaptive Card)
        if: steps.changes.outputs.changed == 'true'
        env:
          WEBHOOK_URL: ${{ secrets.TEAMS_WEBHOOK_URL }}
          COMMIT: ${{ steps.head.outputs.current_commit }}
        run: |
          commit_msg=$(git log -1 --pretty=format:"%s")
          author=$(git log -1 --pretty=format:"%an")
          timestamp=$(date -u +"%Y-%m-%d %H:%M:%S UTC")

          curl -H "Content-Type: application/json" -d "{
            \"type\": \"message\",
            \"attachments\": [
              {
                \"contentType\": \"application/vnd.microsoft.card.adaptive\",
                \"content\": {
                  \"type\": \"AdaptiveCard\",
                  \"version\": \"1.4\",
                  \"body\": [
                    {
                      \"type\": \"TextBlock\",
                      \"size\": \"Large\",
                      \"weight\": \"Bolder\",
                      \"text\": \"🚀 New Commit Detected on *develop*\"
                    },
                    {
                      \"type\": \"FactSet\",
                      \"facts\": [
                        { \"title\": \"Commit SHA:\", \"value\": \"${COMMIT}\" },
                        { \"title\": \"Message:\", \"value\": \"${commit_msg}\" },
                        { \"title\": \"Author:\", \"value\": \"${author}\" },
                        { \"title\": \"Triggered:\", \"value\": \"${timestamp}\" }
                      ]
                    }
                  ],
                  \"actions\": [
                    {
                      \"type\": \"Action.OpenUrl\",
                      \"title\": \"View Commit\",
                      \"url\": \"https://github.com/${{ github.repository }}/commit/${COMMIT}\"
                    },
                    {
                      \"type\": \"Action.OpenUrl\",
                      \"title\": \"View Build Run\",
                      \"url\": \"${{ github.server_url }}/${{ github.repository }}/actions/runs/${{ github.run_id }}\"
                    }
                  ]
                }
              }
            ]
          }" $WEBHOOK_URL
```

---

# 📝 **What this Adaptive Card looks like**

A clean card in Teams:

```
🚀 New Commit Detected on develop

Commit SHA: abc1234567
Message: Fix user authentication bug
Author: Rohith
Triggered: 2025-01-12 14:00 UTC

[View Commit]   [View Build Run]
```

Looks highly professional and DevOps-ready.

---

