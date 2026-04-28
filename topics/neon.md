# Neon

**Keywords**: neon,serverless,postgres

---

**Neon** is a **serverless Postgres database platform that separates storage and compute**, offering instant auto-scaling, branching, and a generous free tier designed for modern cloud-native applications that need flexibility without operational overhead.

**What Is Neon?**

- **Definition**: Serverless PostgreSQL database with Git-like branching.
- **Architecture**: Separated storage (NeonVM) and compute (compute units).
- **Scaling**: Auto-scales from zero to full capacity based on demand.
- **Branching**: Create database branches like Git branches for development.
- **Cost Model**: Pay only for what you use, scale to zero when idle.

**Why Neon Matters**

- **Cost Efficiency**: Scale to zero when idle, only pay for actual usage.
- **Development Speed**: Instant database branches for every PR/feature.
- **No Downtime**: Compute scales instantly without restarting.
- **Developer Experience**: Modern workflow familiar to developers.
- **Scale Flexibility**: Handle traffic spikes without planning capacity.
- **Time-to-Market**: Deploy databases in seconds, not hours.

**Key Features**

**Instant Auto-Scaling**:
- Scale from 0.25 to 8 vCPUs automatically
- Respond to traffic spikes instantly
- Scale down to zero when idle
- No connection hopping or delays

**Database Branching**:
- Create unlimited development branches
- Test schema changes in isolation
- Branch from any point in history
- Fast branch creation (<1 second per GB)

**Connection Pooling**:
- Built-in pgBouncer (session and transaction pooling)
- Handle thousands of connections
- No connection limit issues
- Optimized for serverless runtime

**Point-in-Time Recovery**:
- Restore database to any moment
- 7-90 days retention (tier dependent)
- No data loss scenarios
- Fast recovery process

**Read Replicas**:
- Scale read-heavy workloads
- Independent compute for replicas
- Different regions (expanding)
- Cost-effective scaling

**Quick Start Workflow**
```bash
# Install CLI
npm install -g neonctl

# Create new project
neonctl projects create --name my-app

# Get connection string for main branch
neonctl connection-string main

# Connect with psql
psql postgresql://user:pw@ep-xxx.neon.tech/main

# Create a development branch
neonctl branches create --name dev-feature

# Test changes, then delete when done
neonctl branches delete dev-feature
```

**Development Branching Pattern**
```
main (production)
  ├── feature-auth (for auth changes)
  ├── feature-api (for API changes)
  └── staging (pre-production)
```

**Code Example**
```javascript
// Node.js with Drizzle ORM
import { drizzle } from "drizzle-orm/node-postgres";
import { Pool } from "pg";

const pool = new Pool({
  connectionString: process.env.DATABASE_URL
});

const db = drizzle(pool);

// Queries
const users = await db.select().from(usersTable).where(eq(usersTable.active, true));

// Transactions
await db.transaction(async (tx) => {
  await tx.insert(ordersTable).values(order);
  await tx.update(inventoryTable).set({qty: sql`qty - 1`});
});
```

**Use Cases**

**Web Applications**:
- Next.js apps with serverless functions
- Vercel deployments with instant scaling
- Rapid development with branching per feature

**Development Workflows**:
- Database branch per PR
- Automated testing on fresh branch
- Staging environment branches

**Cost-Sensitive Projects**:
- Scale to zero when idle
- Perfect for side projects
- Minimize unused capacity costs

**Multi-Environment**:
- Main: production
- Staging: pre-release testing
- Dev: feature branches
- Test: ephemeral testing databases

**Global Applications**:
- Regional read replicas
- Reduce cross-ocean latency
- Cost-effective scaling

**Pricing Structure**

**Free Tier** (Generous):
- 0.5 GB storage
- Unlimited branches (game-changer!)
- 191.9 compute hours/month
- Shared compute
- Perfect for learning and side projects

**Pro** ($19/month):
- 10 GB storage
- Unlimited branches
- 300 compute hours/month
- Auto-scaling included

**Business** ($69/month):
- 100 GB storage
- Priority support
- Advanced features
- SLA guarantees

**Scale** (Custom):
- Dedicated resources
- Enterprise SLA
- Custom support

**Integration Ecosystem**

**ORMs**:
- **Prisma**: First-class support with branching
- **Drizzle**: Native integration (lightweight)
- **TypeORM**: Full compatibility
- **SQLAlchemy**: Python ORM support

**Frameworks**:
- **Next.js**: Seamless integration
- **Remix**: Perfect for Remix deployments
- **SvelteKit**: Works great
- **Nuxt**: Vue framework support
- **Astro**: Static + dynamic hybrid

**Platforms**:
- **Vercel**: Built-in Neon marketplace
- **Netlify**: Deploy database seamlessly
- **Cloudflare Workers**: Scale databases
- **AWS Lambda**: Serverless backend
- **Railway**: Alternative PaaS

**Performance Metrics**

- **Latency**: <10ms for most queries (US, EU)
- **Throughput**: Thousands of queries per second
- **Scaling Speed**: <100ms to scale up
- **Branch Creation**: <1 second per GB
- **Availability**: 99.99% uptime SLA

**Neon vs Alternatives**

| Feature | Neon | RDS Aurora | Supabase | Railway |
|---------|------|-----------|----------|---------|
| Serverless | ✅ | ❌ | ✅ | ✅ |
| Branching | ✅ | ❌ | ❌ | ❌ |
| Free Tier | ✅ | ❌ | ✅ | ❌ |
| Self-hosted | ❌ | ❌ | ✅ | ❌ |
| Easy Setup | ✅ | ❌ | ✅ | ✅ |

**Best Practices**

1. **Use branching**: One branch per feature/PR for testing
2. **Leverage auto-scaling**: Let compute handle traffic spikes
3. **Connection pooling**: Always use built-in pooling
4. **Monitor usage**: Track compute hours in dashboard
5. **Set up backups**: Enable automated backups
6. **Use read replicas**: Scale reads independently
7. **Clean up branches**: Delete test branches when done
8. **Set scale limits**: Prevent runaway costs with compute limits

**Common Patterns**

**Development Workflow**:
1. Create branch for feature (`neonctl branches create feature-x`)
2. Deploy app against branch
3. Run tests on branch
4. Merge to main when approved
5. Delete branch automatically

**Multi-Tenant Apps**:
- Separate database per tenant
- Each gets own scale settings
- Zero-cost when tenant unused

**Webhooks & Events**:
- Notified on branch creation/deletion
- Automate environment setup
- Trigger CI/CD pipelines

Neon **reimagines database infrastructure for the serverless era** — eliminating capacity planning headaches while offering Git-like development workflows that make databases as developer-friendly as code repositories.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/neon) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
