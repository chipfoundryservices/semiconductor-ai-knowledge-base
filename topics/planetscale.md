# PlanetScale

**Keywords**: planetscale,mysql,serverless

---

**PlanetScale** is a **serverless MySQL database platform built on Vitess with Git-like branching** and non-blocking schema migrations, enabling zero-downtime deployments and horizontal scaling without traditional database operational complexity.

**What Is PlanetScale?**

- **Definition**: MySQL-compatible serverless database with branching.
- **Foundation**: Built on Vitess (YouTube's battle-tested sharding engine).
- **Schema Changes**: Non-blocking migrations (no locks, zero downtime).
- **Scaling**: Automatic horizontal sharding with transparent growth.
- **Workflow**: Git-like deploy requests for schema changes.

**Why PlanetScale Matters**

- **Zero-Downtime Migrations**: Deploy schema changes without downtime.
- **Git Workflow**: Familiar branching model for databases.
- **Horizontal Scaling**: Auto-sharding handles unlimited growth.
- **Cost Efficient**: Serverless pricing, pay per query.
- **MySQL Compatibility**: Use existing MySQL tools and libraries.
- **Production Ready**: Battle-tested Vitess at YouTube scale.

**Key Features**

**Non-Blocking Schema Changes**:
- Alter tables without locking
- Deploy during business hours
- Automatic rollback if issues
- Instant deployments at any scale

**Database Branching**:
- Create branches like Git
- One branch per feature
- Merge or discard safely
- Test before production

**Horizontal Sharding**:
- Automatic sharding based on shard key
- Scale reads and writes independently
- Handle billions of rows
- Transparent to application

**Connection Pooling**:
- PlanetScale proxy (built-in)
- No connection limit issues
- Session and transaction pools
- Optimized for serverless

**Insights Dashboard**:
- Query performance analytics
- Slow query detection
- Index recommendations
- Real-time metrics and alerts

**Quick Start**
```bash
# Install CLI
brew install planetscale/tap/pscale

# Authenticate
pscale auth login

# Create database
pscale database create mydb

# Create development branch
pscale branch create mydb dev-auth

# Connect to branch
pscale connect mydb dev-auth

# Make schema changes
# In another terminal: pscale shell mydb dev-auth
# ALTER TABLE users ADD COLUMN email VARCHAR(255);

# Create deploy request (like PR)
pscale deploy-request create mydb dev-auth

# Deploy to production (zero downtime!)
pscale deploy-request deploy mydb 1
```

**Non-Blocking Migration Example**
```sql
-- On development branch
ALTER TABLE users ADD COLUMN email VARCHAR(255) NOT NULL DEFAULT '';

-- This triggers:
-- 1. Create shadow table
-- 2. Copy data in background
-- 3. Rename process
-- 4. Drop old table
-- -- All without locking!

-- Deploy request shows this operation is safe
-- Deploy to production -> zero downtime
```

**Branching Workflow for Schema Changes**

**Scenario: Adding Email Column to Users Table**
```bash
# 1. Create branch
pscale branch create mydb add-email

# 2. Make changes
pscale shell mydb add-email
> ALTER TABLE users ADD COLUMN email VARCHAR(255);

# 3. Test changes (connect app to branch)
pscale connect mydb add-email

# 4. Create deploy request
pscale deploy-request create mydb add-email

# 5. Review schema diff
# (PlanetScale shows exact changes)

# 6. Deploy to production (zero downtime!)
pscale deploy-request deploy mydb 1

# 7. Cleanup
pscale branch delete mydb add-email
```

**Code Example**
```javascript
// Node.js with Prisma
import { PrismaClient } from "@prisma/client";

const prisma = new PrismaClient();

// Regular queries work same as MySQL
const users = await prisma.user.findMany({
  where: { active: true }
});

// Create with transaction
await prisma.$transaction([
  prisma.order.create({ data: order }),
  prisma.inventory.update({
    where: { id: item.id },
    data: { quantity: { decrement: 1 } }
  })
]);
```

**Use Cases**

**High-Growth Startups**:
- Start small, scale automatically
- No sharding complexity
- Grow from zero to billions of rows

**E-commerce Platforms**:
- Handle traffic spikes (flash sales, holidays)
- Zero-downtime schema deployments
- Inventory across shards

**SaaS Applications**:
- Add features with safe migrations
- Multi-tenant with sharding per customer
- Continuous deployment pipelines

**Team Collaboration**:
- Database branch per developer
- Feature branches like Git
- Safe experimentation

**Data-Heavy Applications**:
- Analytics and reporting
- Millions of events
- Horizontal scaling

**Pricing Model**

**Hobby Plan** (Free):
- 5 GB storage
- Very limited usage (good for side projects)
- 1 production branch

**Scaler Plan** ($29/month):
- 10 GB storage
- 100 million row reads/month
- 50 million row writes/month
- Unlimited branches
- Horizontal sharding

**Team Plan** ($299/month):
- Unlimited storage
- Unlimited usage
- Team collaboration
- Advanced features

**Enterprise** (Custom):
- Dedicated infrastructure
- SLA guarantees
- Advanced support
- Custom retention

**Integration Ecosystem**

**ORMs & Tools**:
- **Prisma**: Excellent PlanetScale integration
- **Drizzle**: Native support
- **Sequelize**: Works well
- **Knex**: Query builder support
- **SQLAlchemy**: Python ORM

**Platforms**:
- **Vercel**: Official integration for Next.js
- **Netlify**: Deploy functions + database
- **Cloudflare Workers**: Edge compute + DB

**Tools**:
- **Migrate**: DBeaver, Adminer
- **Monitoring**: Datadog, New Relic
- **Backup**: Automated backups

**Performance Benchmarks**

- **Latency**: <5ms within region
- **Throughput**: Millions of queries/second
- **Scaling**: Linear horizontal scaling
- **Availability**: 99.99% uptime SLA

**PlanetScale vs Alternatives**

| Feature | PlanetScale | Neon | RDS Aurora | Traditional MySQL |
|---------|------------|------|-----------|------------------|
| MySQL | ✅ | ❌ | ✅ | ✅ |
| Branching | ✅ | ✅ | ❌ | ❌ |
| Zero-Downtime Deploy | ✅ | ❌ | ❌ | ❌ |
| Auto-Sharding | ✅ | ❌ | ❌ | ❌ |
| Serverless | ✅ | ✅ | ❌ | ❌ |

**Best Practices**

1. **Use branches**: One per feature, test before production
2. **Monitor queries**: Use Insights to find slow queries
3. **Add indexes**: Follow recommendations in dashboard
4. **Safe migrations**: Test on branch before production
5. **Connection pooling**: Use built-in PlanetScale proxy
6. **Choose shard key**: Critical for performance
7. **Backup strategy**: Enable automated backups
8. **Team permissions**: Control who can deploy

**Common Patterns**

**Add New Column**:
- Branch → Add column → Deploy request → Approve → Deploy (0 downtime)

**Index Addition**:
- Branch → Add index → Test that query improves → Deploy (0 downtime)

**Data Migration**:
- Add column → Branch to populate → Deploy → Switch code over

PlanetScale **brings GitHub-like workflows to databases**, eliminating schema change anxiety and enabling continuous deployment of database changes alongside application code.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/planetscale) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
