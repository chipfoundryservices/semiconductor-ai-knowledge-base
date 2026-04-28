# Stripe

**Keywords**: stripe,payment,api

---

**Stripe** is the **leading payment processing API enabling businesses to accept online payments, manage subscriptions, and handle complex financial operations programmatically**, trusted by hundreds of thousands of companies to process $1 trillion+ in transactions annually.

**What Is Stripe?**

- **Definition**: Payments infrastructure for the internet.
- **Core Function**: Accept payments, manage billing, handle payouts.
- **Foundation**: Full payment stack (processing, fraud, financial ops).
- **Global**: 135+ currencies, 45+ countries, 12M+ merchants.
- **Developer-Focused**: Excellent API, SDKs, documentation.

**Why Stripe Matters**

- **Completeness**: Single API for payments, subscriptions, invoicing
- **Developer Experience**: Well-designed API, excellent docs
- **Global Scale**: Works worldwide with local payment methods
- **Trust**: PCI Level 1, SOC 2, constantly audited
- **Fraud Prevention**: Machine learning-powered detection
- **Community**: Largest ecosystem of payment tools
- **Speed**: Setup account and start accepting payments in hours

**Key Products**

**Stripe Payments** (One-Time Payment):
```javascript
const paymentIntent = await stripe.paymentIntents.create({
  amount: 2000,  // $20.00
  currency: "usd",
  payment_method_types: ["card"]
});
```

Use cases: E-commerce purchases, SaaS subscriptions, donations

**Stripe Billing** (Recurring):
```javascript
const subscription = await stripe.subscriptions.create({
  customer: "cus_abc123",
  items: [{price: "price_xyz"}]
});
```

Use cases: SaaS, subscriptions, memberships

**Stripe Connect** (Marketplace):
```javascript
const account = await stripe.accounts.create({
  type: "express",
  country: "US",
  email: "vendor@example.com"
});
```

Use cases: Marketplaces, platforms, multi-party payments

**Stripe Checkout** (Pre-Built Page):
```javascript
const session = await stripe.checkout.sessions.create({
  line_items: [{price: "price_xyz", quantity: 1}],
  mode: "payment",
  success_url: "https://example.com/success",
  cancel_url: "https://example.com/cancel"
});
```

Use cases: Quick payment pages, no custom UI needed

**Stripe Invoicing**:
- Generate invoices automatically
- Recurring billing management
- Payment reminders
- Reconciliation reports

**Stripe Financial Tooling**:
- Payouts to bank accounts
- Card issuing
- Treasury products
- Loans for merchants

**Implementation Flow**

**Backend Setup**:
```javascript
const stripe = require("stripe")("sk_test_...");

// Create payment intent
const intent = await stripe.paymentIntents.create({
  amount: 1000,
  currency: "usd",
  payment_method_types: ["card", "apple_pay"]
});
```

**Frontend Handling**:
```javascript
const stripe = Stripe("pk_test_...");
const elements = stripe.elements();
const cardElement = elements.create("card");
cardElement.mount("#card-element");

// Confirm payment
const {error} = await stripe.confirmCardPayment(intent.client_secret, {
  payment_method: {card: cardElement}
});
```

**Webhook Processing**:
```javascript
app.post("/webhook", async (req, res) => {
  const sig = req.headers["stripe-signature"];
  const event = stripe.webhooks.constructEvent(
    req.body, sig, webhookSecret
  );
  
  if (event.type === "payment_intent.succeeded") {
    // Fulfill order
    await fulfillOrder(event.data.object);
  }
  
  res.json({received: true});
});
```

**Pricing Model**

**Standard Rates**:
- 2.9% + $0.30 per successful card charge (US)
- No setup fees, no monthly fees
- International cards: +1% additional
- Currency conversion: +1% additional

**Examples**:
- $10 transaction = $0.59 fee
- $100 transaction = $3.20 fee
- $1000 transaction = $29.30 fee

**Volume Discounts**:
- Large merchants negotiate custom rates
- Enterprise: Custom pricing with SLA

**Payment Methods Supported**

**Cards**:
- Visa, Mastercard, Amex, Discover
- Debit cards

**Digital Wallets**:
- Apple Pay, Google Pay
- Alipay, WeChat Pay

**Bank Transfers**:
- ACH (US), SEPA (EU), Bacs (UK)
- iDEAL, Bancontact

**Regional Methods**:
- Klarna (Sweden, Germany)
- EPS (Austria)
- Giropay (Germany)
- And 50+ more

**Use Cases**

**E-Commerce Stores**:
- Checkout integration
- Order management
- Refunds and disputes

**SaaS & Subscriptions**:
- Recurring billing
- Usage-based pricing
- Dunning (retry failed payments)

**Marketplaces**:
- Connect for seller payouts
- Escrow for transactions
- Separate account management

**Crowdfunding**:
- Campaign payments
- Refund management
- Goal tracking

**On-Demand Services**:
- Uber-style apps
- Real-time settlements
- Tip handling

**Nonprofits**:
- Donation processing
- Lower rates for nonprofits
- Recurring donor management

**Security & Compliance**

- **PCI DSS Level 1**: Highest security standard
- **Tokenization**: Never store raw card data
- **3D Secure**: Additional authentication when needed
- **Radar**: ML-powered fraud detection
- **Encryption**: SSL/TLS for all data transmission
- **SOC 2 Type II**: Third-party audited annually
- **GDPR Compliant**: Respect user privacy

**Stripe vs Alternatives**

| Feature | Stripe | PayPal | Square | Braintree |
|---------|--------|--------|--------|-----------|
| API Quality | ⭐⭐⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐⭐⭐ |
| Documentation | Best | Good | Good | Good |
| Payments | ✅ | ✅ | ✅ | ✅ |
| Subscriptions | ✅ | ✅ | Limited | ✅ |
| Payouts | ✅ | Limited | Limited | Limited |
| Price | 2.9%+ | 2.2%+ | 2.7%+ | 2.9%+ |
| Ease | Very Easy | Medium | Medium | Easy |

**Best Practices**

1. **Webhook Reliability**: Always handle webhook retries
2. **Idempotency**: Use idempotent keys for retry safety
3. **Error Handling**: Implement proper error recovery
4. **Testing**: Use test mode before production
5. **PCI Compliance**: Never handle raw card data
6. **Monitoring**: Monitor webhook delivery and payment status
7. **Documentation**: Document your payment flow
8. **Customer Communication**: Clear payment status messaging

**Integration Patterns**

**E-Commerce Workflow**:
1. Shopping cart built
2. Checkout page created
3. Create payment intent
4. Collect payment
5. Fulfill order via webhook
6. Send confirmation

**Subscription Setup**:
1. Create customer
2. Create subscription with price
3. Attach payment method
4. Handle status changes
5. Manage billing issues

**Marketplace Payout**:
1. Collect payment from buyer
2. Hold funds temporarily (escrow)
3. Order fulfilled
4. Transfer to seller's Stripe account
5. Seller receives payout to bank

**Common Integration Patterns**

- **Next.js + Stripe**: Frontend checkout
- **Node + Express + Stripe**: Backend billing
- **Vercel + Stripe Webhook**: Serverless workflow
- **Zapier + Stripe**: Automate Stripe workflows

Stripe is the **gold standard for online payments** — combining developer-friendly APIs, world-class security, global reach, and excellent documentation to make payments the easiest part of your product.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/stripe) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
