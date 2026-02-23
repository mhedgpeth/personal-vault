# Money

We use YNAB (You Need A Budget) for budgeting.

## YNAB CLI

The CLI is installed via bun (`bunx ynab`). The default budget is set to Family Budget 2.0 via `bunx ynab budgets set-default`.

### Common commands

```bash
# List all accounts
bunx ynab accounts list 
# List all categories (shows budget groups and assigned/available amounts)
bunx ynab categories list 
# View a specific month's budget summary
bunx ynab months view --budget 141ac16f-e161-48a9-af77-ec5c6a43b58e 2026-02-01

# Recent transactions (last 30 days)
bunx ynab transactions list --budget 141ac16f-e161-48a9-af77-ec5c6a43b58e --since 2026-01-23

# Search transactions by payee
bunx ynab transactions search --budget 141ac16f-e161-48a9-af77-ec5c6a43b58e --payee-name "Amazon"

# Search transactions by memo
bunx ynab transactions search --budget 141ac16f-e161-48a9-af77-ec5c6a43b58e --memo "groceries"

# Unapproved transactions (need review)
bunx ynab transactions list --budget 141ac16f-e161-48a9-af77-ec5c6a43b58e --approved false

# Scheduled/recurring transactions
bunx ynab scheduled list ```

### Tips

- Amounts are in dollars (not YNAB milliunits), e.g. `--min-amount 100` means $100
- Use `--fields` to narrow output, e.g. `--fields date,payee_name,amount,category_name`
- Use `--compact` flag on `bunx ynab -c` for minified JSON
- Dates use YYYY-MM-DD format

## Workflow: Review Transactions

Use this workflow to review and approve outstanding transactions.

### Step 1: Fetch unapproved transactions

```bash
bunx ynab transactions list --budget 141ac16f-e161-48a9-af77-ec5c6a43b58e --approved false --fields id,date,payee_name,amount,category_name
```

### Step 2: Separate into two groups

- **Uncategorized**: transactions where `category_name` is "Uncategorized" — these need a category assigned before approval
- **Categorized**: transactions that already have a category — these just need approval

### Step 3: Categorize uncategorized transactions

For each uncategorized transaction, present it to the user and ask which category to assign. Fetch the category list for reference:

```bash
bunx ynab categories list --budget 141ac16f-e161-48a9-af77-ec5c6a43b58e --fields id,name,category_group_name
```

Once the user picks a category, update and approve in one step:

```bash
bunx ynab transactions update <transaction-id> --budget 141ac16f-e161-48a9-af77-ec5c6a43b58e --category-id <category-id> --approved
```

### Step 4: Approve already-categorized transactions

Present the remaining categorized transactions to the user for review. For each one the user confirms, approve it:

```bash
bunx ynab transactions update <transaction-id> --budget 141ac16f-e161-48a9-af77-ec5c6a43b58e --approved
```

### Step 5: Confirm completion

Re-fetch unapproved transactions to verify the list is empty:

```bash
bunx ynab transactions list --budget 141ac16f-e161-48a9-af77-ec5c6a43b58e --approved false
```

If the list is empty, all transactions are reviewed. Report the count of transactions categorized and approved.
