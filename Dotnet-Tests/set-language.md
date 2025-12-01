# ✅ **You CANNOT get the language automatically from GitHub context**

GitHub does **not** expose a context such as:

```
${{ github.repository.language }}
```

GitHub only shows languages in the UI (via Linguist), but **that information is NOT available inside workflows**.

So the workflow does **not** have an automatic variable like `${{ github.language }}`.

---

# ✅ **Correct way: Use a workflow input OR a matrix**

If you want to avoid hardcoding `"csharp"` and make it flexible, you must use:

---

# OPTION 1️⃣: **Matrix for languages** (most common)

```yaml
jobs:
  analyze:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        language: [csharp]

    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Initialize CodeQL
        uses: github/codeql-action/init@v3
        with:
          languages: ${{ matrix.language }}

      - name: Perform CodeQL Analysis
        uses: github/codeql-action/analyze@v3
```

Now your workflow easily supports:

```yaml
matrix:
  language: [csharp, javascript, python]
```

---

# OPTION 2️⃣: **Use workflow_dispatch input**

If you want developers to choose the language when manually triggering:

```yaml
on:
  workflow_dispatch:
    inputs:
      lang:
        description: "Language for CodeQL"
        required: true
        default: csharp
```

Then:

```yaml
with:
  languages: ${{ inputs.lang }}
```

---

# OPTION 3️⃣: Put language in a repo variable

1. Go to: **Settings → Variables → Repository variables**
2. Create variable: `CODEQL_LANG` = `csharp`

Then use:

```yaml
with:
  languages: ${{ vars.CODEQL_LANG }}
```

This is the easiest if you want to avoid hardcoding.

---

# 🔥 Best Recommendation for Your Workflow

If your repo will *always* be C#, don’t overcomplicate it.
Just use:

```yaml
with:
  languages: csharp
```

But if you want *dynamic* control:

### ⭐ Use repository variable:

```
CODEQL_LANG=csharp
```

Then:

```yaml
languages: ${{ vars.CODEQL_LANG }}
```

---
