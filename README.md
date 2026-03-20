# SentinelWatch Workspace

This repository serves as the **community annotation layer** for [SentinelWatch](https://github.com/stablewatch-io/sentinelwatch).

## Purpose

Each **allocation** tracked by SentinelWatch has **3 separate GitHub Discussions**:
1. **Risk Analysis** - Discuss risk factors, exposures, and mitigation
2. **Settle** - Discuss settlement processes and execution plans
3. **Upcoming** - Discuss upcoming changes and future considerations

Each discussion is independent with its own labels, comments, and context.

## How to Use

### Discussion Categories

Navigate to the **Discussions** tab and select a category:

#### 🔴 Risk Analysis
Discuss:
- Risk scores and assessments
- Exposure concerns
- Smart contract risks
- Counterparty risks
- Mitigation strategies

**Labels:** `risk:low`, `risk:medium`, `risk:high`, `risk:reviewed`, `watch`

#### 💰 Settle
Discuss:
- Settlement processes
- Execution plans
- Operational timelines
- Withdrawal/deposit procedures
- Rebalancing schedules

**Labels:** `settle:pending`, `settle:in-progress`, `settle:completed`, `settle:blocked`, `needs-action`

#### 📅 Upcoming
Discuss:
- Upcoming changes
- Planned updates
- Future considerations
- Protocol upgrades
- Strategy adjustments

**Labels:** `upcoming:planned`, `upcoming:proposed`, `upcoming:confirmed`, `monitor`, `breaking-change`

### Labeling Discussions

Each category has its own label namespace. Apply labels relevant to the discussion context.

### Commenting

Each discussion thread is **independent** - comments stay focused on that specific aspect (risk, performance, or compliance).

### Frontend Integration

The SentinelWatch frontend displays different discussions on different pages:
- **Risk Dashboard** → Shows Risk Analysis discussions
- **Settle Page** → Shows Settle discussions
- **Upcoming Page** → Shows Upcoming discussions

## Discussion Format

Each discussion follows this format:

```markdown
## [Allocation Name]

[Category-specific intro text]

### Allocation Details

- **Star:** Spark
- **Protocol:** morpho
- **Blockchain:** ethereum
- **Type:** allocation
- **Underlying:** `ethereum:0x...`
- **Holding Wallet:** `ethereum:0x...`

### Flags

- Lending position
- Has RRC oversight

---

<!-- allocation_id:spark-morpho-usdc-ethereum -->

Add labels and comments specific to [category name].
```

The `<!-- allocation_id:... -->` comment is used by the sync process to match discussions to allocations.

## Automation

Discussions are automatically:
- **Created** when new allocations are added to SentinelWatch (one per category)
- **Kept in sync** on every deployment
- **Never deleted** to preserve discussion history

If an allocation is removed from the config, its discussions remain open for historical reference.

## Contributing

All contributions are welcome! Simply:
1. Navigate to **Discussions** tab
2. Select the relevant category (Risk, Performance, or Compliance)
3. Find the discussion for the allocation
4. Add labels or comments
5. Changes appear in the frontend within 60 seconds (cache refresh)

No special permissions required - standard GitHub access is sufficient.

## API Access

The frontend accesses this data via:
- **GitHub GraphQL API** for reading discussions, labels, and comments
- **Category-filtered queries** to fetch discussions for specific pages
- User authentication via GitHub OAuth (optional - read-only access works without auth)
- Writes require GitHub login (users post under their own account)

## Example: Finding a Discussion

```graphql
query {
  repository(owner: "stablewatch-io", name: "sentinelwatch-workspace") {
    discussions(first: 100, categoryId: "DIC_kwDON...", filterBy: {searchTerm: "spark-morpho-usdc"}) {
      nodes {
        title
        body
        labels(first: 10) {
          nodes {
            name
          }
        }
        comments(first: 10) {
          totalCount
          nodes {
            body
            author {
              login
            }
          }
        }
      }
    }
  }
}
```
