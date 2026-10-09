# BazaarPilot AI: Complete Feature Documentation

**Product vision:** An AI-powered sales and business management assistant for small online sellers in Pakistan.

BazaarPilot AI helps sellers turn customer messages into organized orders. It also helps them manage products and customers, track deliveries and payments, calculate profit, and see how the business is doing, all from one mobile app.

The app is built for sellers who run their business today through WhatsApp, Instagram, Facebook, spreadsheets or notebooks.

| Pillar | Description |
| --- | --- |
| AI-powered | Converts unstructured customer messages into structured sales data. |
| Order management | Tracks every order from inquiry through delivery. |
| Business analytics | Shows sales, expenses, estimated profit and returns. |
| Local-first experience | Supports English, Urdu and Roman Urdu workflows. |

This document covers the full proposed feature set: the first release, future integrations, subscription plans, and the technical requirements for building it safely.

## Table of contents

1. [User types and access levels](#1-user-types-and-access-levels)
2. [Authentication and onboarding](#2-authentication-and-onboarding)
3. [Home dashboard](#3-home-dashboard)
4. [AI Assistant: the core feature](#4-ai-assistant-the-core-feature)
5. [Order management](#5-order-management)
6. [Product and inventory management](#6-product-and-inventory-management)
7. [Customer management and CRM](#7-customer-management-and-crm)
8. [Payment tracking and customer balances](#8-payment-tracking-and-customer-balances)
9. [Delivery and dispatch management](#9-delivery-and-dispatch-management)
10. [Expenses and profit calculation](#10-expenses-and-profit-calculation)
11. [Reports and business analytics](#11-reports-and-business-analytics)
12. [Notifications and reminders](#12-notifications-and-reminders)
13. [Customer messaging and channel integrations](#13-customer-messaging-and-channel-integrations)
14. [Invoices and order documents](#14-invoices-and-order-documents)
15. [Subscription and premium features](#15-subscription-and-premium-features)
16. [Team management and multiple stores](#16-team-management-and-multiple-stores)
17. [Settings, personalization and accessibility](#17-settings-personalization-and-accessibility)
18. [Admin panel for the SaaS owner](#18-admin-panel-for-the-saas-owner)
19. [Security, privacy and data reliability](#19-security-privacy-and-data-reliability)
20. [Recommended database structure](#20-recommended-database-structure)
21. [End-to-end example](#21-end-to-end-example-how-all-features-work-together)
22. [What to build first versus later](#22-what-to-build-first-versus-later)
23. [How to measure success](#23-how-to-measure-whether-bazaarpilot-ai-is-succeeding)
24. [Final recommendation](#final-recommendation)

---

## 1. User types and access levels

BazaarPilot AI should support these user types.

1. **Business owner**: Owns the business workspace and manages products, orders, customers, expenses, reports, subscriptions and staff permissions.
2. **Staff member**: Handles the operational tasks the owner assigns, such as entering orders, updating dispatch statuses and managing customers, within the permissions the owner grants.
3. **Platform administrator**: Manages subscriptions, account support, plan limits, abuse reports and service configuration. Access to customer business data should be restricted and audited.

Each business should have its own workspace. If an owner runs several stores, the app can later support multiple business profiles, each with separate orders, inventory and reports.

## 2. Authentication and onboarding

The goal is to get a seller using the app quickly without needing accounting or technical knowledge.

### 2.1 Phone or email registration

Let users register with an email and password or with phone verification, depending on which authentication options the product configures. Include login, logout, password recovery and session management.

### 2.2 Business setup wizard

Collect the business name, business category, currency (PKR), contact information and preferred language. Let sellers pick a business type such as clothing, cosmetics, accessories, home baking or general retail.

### 2.3 Business profile and workspace

Store business-specific settings and link every record to the correct workspace. This is the foundation for multi-store management and team access later.

### 2.4 Guided onboarding

Walk users through adding a product, importing or entering an order, using AI extraction and viewing the sales dashboard. Show sample data only in a clearly labelled demo workspace.

> **Important:** Authentication identifies the user. Authorization decides which business data that user may access, and it must be enforced in the backend, not just in the Flutter UI.

## 3. Home dashboard

The dashboard is the first screen users see after signing in. It should answer four questions immediately:

- How much have I sold?
- Which orders need attention?
- How much money have I received?
- How is my business performing?

### Dashboard components

| Component | What it does |
| --- | --- |
| Total sales | Shows the value of sales for the selected date range, with clearly defined handling of cancellations and returns. |
| Orders count | Shows total orders, optionally broken down by status. |
| Pending orders | Highlights orders that need confirmation, packing, dispatch or another action. |
| Payments received | Tracks recorded payments separately from the total order value. |
| Estimated profit | Shows an estimate based on known sales, product costs and recorded expenses. |
| Sales chart | Compares sales across days, weeks or months. |
| Recent orders | Shows recently created or updated orders with their current status. |
| Low-stock alerts | Warns the seller when tracked inventory falls below a set threshold. |
| AI business insights | Highlights patterns, such as a product getting many orders or a rise in returns. |
| Quick actions | Shortcuts to create an order, add a product, record an expense or open the AI assistant. |

### Date filters

Today, 7 days, 30 days, This month, and Custom range.

> The dashboard must keep **revenue**, **cash received**, **outstanding payments** and **profit** distinct. They measure different things and must never be presented as the same.

## 4. AI Assistant: the core feature

The AI Assistant is what sets BazaarPilot AI apart. It should act like a business assistant that understands customer messages and helps the seller get tasks done.

### 4.1 Message-to-order conversion

**Purpose:** Turn customer messages into structured order records.

Example incoming message:

> Assalam o Alaikum, mujhe 2 blue kurtis chahiye, medium size, price 1800 each. Address Karachi, payment COD.

The AI should extract:

| Field | Extracted value |
| --- | --- |
| Product | Blue kurti |
| Quantity | 2 |
| Size | Medium |
| Unit price | PKR 1,800 |
| Subtotal | PKR 3,600 |
| Delivery city | Karachi |
| Payment method | Cash on delivery |

The seller can edit any field before saving.

The AI must not invent a customer's name, phone number, full address, product availability or delivery fee when the message doesn't include them. It should flag the missing information instead.

**Expected workflow:**

1. The seller pastes or shares a message.
2. The AI picks out possible order information.
3. The app matches the product against the seller's catalogue.
4. The app highlights missing or uncertain fields.
5. The seller reviews and confirms the order.
6. The order is saved and the dashboard updates.

### 4.2 Multi-language understanding

The AI should understand common business instructions written in:

- English
- Urdu
- Roman Urdu
- Mixed English and Roman Urdu

Examples:

- "Ahmed ka order save kar do."
- "Aaj ki sale 15 hazar hai."
- "Is customer ka balance batao."
- "Mujhe pending orders dikhao."

Test language understanding against real seller messages, including spelling variations, abbreviations and incomplete sentences.

### 4.3 AI business commands

Users should be able to ask questions in natural language.

| User asks | App action |
| --- | --- |
| "Show today's orders." | Filters the order list to today's records. |
| "Which orders are pending?" | Shows orders that need action. |
| "How much money do customers owe me?" | Calculates recorded outstanding balances. |
| "Which product sold the most this month?" | Ranks products by units sold or sales value and states which metric it used. |
| "Create an expense of 500 for packaging." | Prepares an expense record for confirmation. |
| "Show my profit for September." | Shows the estimated profit calculation for September. |

The assistant must answer from real business records and calculations, never from language-model guesses.

### 4.4 AI-generated customer replies

The assistant can draft replies for sellers to review, then copy or share:

- Order confirmation
- Request for missing delivery details
- Product availability response
- Dispatch notification
- Delayed delivery apology
- Payment reminder
- Customer feedback request

The seller can pick a tone: friendly, professional, concise or apologetic.

### 4.5 AI image and screenshot extraction

A later version could let sellers upload screenshots of customer conversations or order slips. The AI extracts the visible information and asks the seller to confirm it.

For example, a screenshot showing a product, quantity and delivery address can become a draft order. This should stay an optional later feature, because screenshots can contain private customer information and OCR can make mistakes.

### 4.6 Voice-to-order

Let sellers record or dictate instructions such as:

> "Create an order for Sara, three handbags, total 4,500 rupees, payment received."

Speech recognition turns the recording into text, and the AI then prepares a draft transaction. If the customer or product is ambiguous, the app should ask a follow-up question rather than guess.

### 4.7 AI safety and confirmation

This feature needs strict rules:

- AI suggestions are not saved as final transactions until the seller confirms them.
- Money amounts, quantities and product variants are shown for review.
- Destructive operations require explicit confirmation.
- The assistant only accesses the current user's authorized business data.
- All completed financial changes are auditable.
- AI mistakes can be corrected by editing the original record.

> **MVP focus:** message-to-order conversion, business questions and reply drafting. Voice and screenshot extraction can follow once the core workflow works reliably.

## 5. Order management

The Orders module keeps every sale in an organized, searchable list.

### 5.1 Create orders manually or with AI

Sellers can create orders through a manual form or the AI Assistant. Each order should support:

- A unique order number
- Customer details
- One or more products
- Product variants, quantities and prices
- Discount and delivery charges
- Order total
- Payment method and payment status
- Delivery address
- Order source, such as WhatsApp, Instagram, Facebook or manual entry
- Internal notes
- Creation and update timestamps

### 5.2 Order status workflow

Use a fixed set of statuses rather than letting sellers type arbitrary status names.

| Status | Meaning |
| --- | --- |
| New | Order received |
| Confirmed | Seller has verified the order |
| Processing | Being prepared and packed |
| Dispatched | Handed to the courier |
| Delivered | Delivery confirmed |

**Exception statuses:** Cancelled, Returned, Delivery failed.

```
New → Confirmed → Processing → Dispatched → Delivered
                     ↘ Cancelled    ↘ Returned / Delivery failed
```

The system should block invalid status changes and ask for confirmation when an order is cancelled or returned. Returning an order must not silently erase the original sale or its payment history.

### 5.3 Search, filters and sorting

Search by:

- Order number
- Customer name or phone number
- Product name
- Tracking number

Filter by status, order date, source channel, payment status and delivery status.

Sort by newest, oldest, highest value or orders needing attention.

### 5.4 Order detail screen

Opening an order shows its full timeline, product lines, customer details, payment history, delivery information, notes and status changes.

From there the user can edit permitted fields, duplicate the order, cancel it or create a message for the customer.

### 5.5 Duplicate-order detection

If a customer submits an order that looks identical to one they sent shortly before, the app can warn the seller.

The warning should be based on matching details such as phone number, product, amount and timing. It must never delete an order automatically, because repeat purchases are legitimate.

### 5.6 Bulk actions

A future version can let sellers select multiple orders to update statuses, export records or prepare dispatch lists. For the first release, actions on individual orders are enough.

## 6. Product and inventory management

This module helps sellers keep a product catalogue and, when they turn it on, track stock.

### 6.1 Product catalogue

Each product should include:

- Product name and SKU
- Product images
- Category
- Selling price
- Purchase or production cost
- Description
- Active or archived status

A product can have variants such as size, colour, flavour or packaging. For example, a clothing product might come in small, medium and large, in black, blue and white.

### 6.2 Stock management

Let the seller turn on inventory tracking per product. Features include:

- Current stock quantity
- Stock adjustments
- Stock deduction when an order is confirmed, following the chosen policy
- Stock restoration when a cancellation or return qualifies
- Low-stock thresholds
- Stock movement history

Stock changes must be recorded consistently so stock never goes negative or gets deducted twice. For the MVP, a simple, auditable stock ledger is better than a complex warehouse system.

### 6.3 Product profitability

Store the cost of each product or variant so the app can estimate gross margin by comparing selling price with cost.

For example, if an item costs PKR 900 and sells for PKR 1,500:

- Gross profit per item: PKR 600
- Gross margin: 40%

These figures come before other expenses, discounts, delivery subsidies and any applicable taxes.

### 6.4 Product performance

Show which products bring in the most sales, which sell slowly and which are low on stock. State clearly whether a ranking is based on units sold, revenue or profit contribution.

## 7. Customer management and CRM

The Customer module works as a simple customer relationship management (CRM) system.

### 7.1 Customer profiles

Store the customer's name, phone number, preferred contact method, delivery addresses and notes that help fulfil orders. Each customer profile should show:

- Order history
- Total order value
- Completed and cancelled orders
- Outstanding balances, if any
- Last order date
- Communication history, where recorded

### 7.2 Customer segmentation

Let sellers create useful groups such as:

- First-time customers
- Repeat customers
- High-value customers
- Customers with pending deliveries
- Customers with outstanding payments

These groups help sellers decide who to follow up with first. Don't automatically label customers as unreliable based only on cancellations or late payments.

### 7.3 Repeat-customer insights

The app can flag customers who reorder often and the products they've bought before, so sellers can suggest relevant products or give better service.

Send marketing messages only in line with applicable consent and platform rules.

### 7.4 Privacy and data management

Customer profiles hold personal information, so provide access controls, secure storage, and tools to correct or delete information where appropriate. Don't collect sensitive personal details that order fulfilment doesn't need.

## 8. Payment tracking and customer balances

This module records each order's payment state. It does not move money between customers, sellers or banks.

### 8.1 Payment methods

Support recording payments made through:

- Cash
- Cash on delivery
- Bank transfer
- Easypaisa
- JazzCash
- Other configurable methods

Record these as payment methods or payment references. Don't treat them as verified transactions unless an authorized integration confirms them.

### 8.2 Payment statuses

- Unpaid
- Partially paid
- Paid
- Refunded

For cash-on-delivery orders, track the courier's collection and settlement separately when the seller has that information.

### 8.3 Partial payments

Say an order totals PKR 5,000 and the customer pays PKR 2,000 in advance:

| | Amount |
| --- | --- |
| Order total | PKR 5,000 |
| Amount recorded as paid | PKR 2,000 |
| Outstanding balance | PKR 3,000 |

The outstanding balance should update automatically as payments or refunds are recorded.

### 8.4 Customer outstanding balances

Show the total recorded amount owed across eligible orders, with a drill-down into the underlying transactions so the seller can check how the balance was calculated.

### 8.5 Payment reminders

Generate a polite message reminding the customer about an outstanding amount. The seller reviews it and sends it. Add automated messaging only when an authorized channel and its rules allow it.

## 9. Delivery and dispatch management

Delivery management shows sellers which orders are awaiting dispatch, in transit, delivered or returned.

### 9.1 Delivery details

Record:

- Recipient name and phone
- Delivery address and city
- Courier company
- Tracking number
- Delivery fee
- Dispatch date
- Estimated delivery date, if provided
- Delivery status

### 9.2 Shipment tracking

At first, let the seller enter tracking numbers and update shipment statuses by hand. Later, supported courier integrations could sync tracking events. Don't show live tracking or delivery estimates until a real data source provides them.

### 9.3 Cash-on-delivery reconciliation

For COD orders, track the amount expected from the courier and the amount the courier has settled. This helps sellers spot pending remittances and discrepancies, and keeps "delivered" separate from "money actually received".

### 9.4 Returns and failed deliveries

Record the reason for a return or failed delivery, any return charges, what happens to the stock, and the refund status. Returned products must not be marked sellable again until the seller confirms their condition.

## 10. Expenses and profit calculation

This is one of the most valuable modules, because many small sellers track sales but don't know whether they're actually making a profit.

### 10.1 Expense tracking

Let sellers record:

- Product purchases or production costs
- Packaging
- Delivery and courier charges
- Advertising
- Marketplace fees
- Staff costs
- Utilities
- Other operating expenses

Each expense has an amount, category, date, description and an optional receipt.

### 10.2 Profit calculation

The app should define its metrics clearly. A simplified estimate:

```
Gross Profit               = Net Sales − Cost of Goods Sold
Estimated Operating Profit = Gross Profit − Recorded Operating Expenses
```

Proper accounting may need extra handling for inventory, refunds, taxes, timing and unpaid courier settlements. Label the result as an estimate until the accounting model is complete.

### 10.3 Daily and monthly reports

Provide summaries of:

- Gross order value
- Net sales after eligible discounts and returns
- Cost of goods sold
- Recorded expenses
- Estimated profit
- Amounts collected
- Outstanding customer balances
- Pending courier settlements

Reports should use consistent date ranges and state what each figure includes.

### 10.4 Expense reminders and unusual changes

The app could flag a sudden jump in advertising costs or a rise in return-related expenses. These flags are prompts to look into something, not proof of fraud or business failure.

## 11. Reports and business analytics

The Reports module turns saved business data into information sellers can act on.

### 11.1 Sales report
Show sales by day, week, month or custom date range, including order counts, discounts, cancellations and returns, with documented calculation rules.

### 11.2 Product report
Rank products by units sold, revenue and estimated profit contribution, and let sellers compare products across time periods.

### 11.3 Customer report
Show new versus repeat customers, average order value, repeat purchases and customer order trends.

### 11.4 Order fulfilment report
Show how many orders are awaiting confirmation, processing, dispatched, delivered, cancelled or returned. Where timestamps are available, estimate how long orders spend at each stage.

### 11.5 Payment report
Summarize recorded payments by method, outstanding customer balances, refunds and unsettled COD amounts.

### 11.6 Export reports
Let sellers export reports to CSV or PDF for bookkeeping or for their accountant. Exports must respect the user's permissions and include the selected date range and metric definitions.

## 12. Notifications and reminders

Notifications keep important tasks from getting lost among customer conversations.

- **12.1 Order reminders**: Notify the seller when new orders need confirmation, orders sit unprocessed or dispatch tasks are overdue.
- **12.2 Inventory alerts**: Notify sellers when tracked products drop below their stock thresholds.
- **12.3 Payment reminders**: Remind sellers about recorded outstanding balances and pending courier settlements.
- **12.4 Daily business summary**: An optional daily summary of sales, pending orders, payments and important tasks.

Let users choose which notifications they get and when. Don't send repeated notifications about the same issue.

## 13. Customer messaging and channel integrations

BazaarPilot AI is built for sellers who talk to customers on several channels, and each integration has its own technical requirements.

### 13.1 Share-to-app functionality
For the MVP, users can share or copy text from supported apps into BazaarPilot AI. The app opens a draft-order screen with the shared text ready for extraction. This is cheaper and simpler than building full messaging integrations straight away.

### 13.2 WhatsApp Business integration
A later version can integrate with the official WhatsApp Business Platform where the seller is eligible. Depending on permissions and approved API capabilities, this may cover incoming messages, approved outbound messaging workflows and message status events.

It must not rely on unofficial WhatsApp Web automation or silently read private conversations.

### 13.3 Instagram and Facebook integration
Possible future features include receiving supported business messages and linking customer inquiries to order records. Access depends on Meta's APIs, permissions, account setup and review requirements. Don't promise automatic inbox syncing until the needed integration is confirmed available.

### 13.4 Message templates
Create reusable templates for order confirmations, delivery information, payment reminders and post-purchase feedback. Let sellers insert approved order fields, such as the customer's first name or the order number, and review the final message before sending.

### 13.5 Channel attribution
Every order can store its source channel, so sellers can see whether Instagram, Facebook, WhatsApp or another channel brings in the most orders and revenue.

## 14. Invoices and order documents

The app should generate simple documents for customers and for internal use. Features:

- Order invoice with a unique number
- Business name and contact information
- Product descriptions and quantities
- Subtotals, discounts and delivery charges
- Payment method and outstanding amount
- PDF generation
- Share or save invoice
- Invoice history

Sellers can configure a logo, brand colours and footer text.

Invoices must not be presented as official tax invoices unless they meet the necessary tax requirements.

## 15. Subscription and premium features

BazaarPilot AI is a SaaS product, so subscription management is a core business feature.

### 15.1 Free plan
Let new users try the core workflow with limits on order volume and AI usage.

### 15.2 Paid plans
Paid subscriptions could unlock:

- More monthly AI usage
- More orders
- Advanced reports
- Product and stock analytics
- Data exports
- Additional staff accounts
- Multi-store support

### 15.3 Usage tracking
Track AI requests, orders, storage and other billable resources on the backend. Don't rely on the Flutter client to enforce subscription limits.

### 15.4 Upgrade and renewal screens
Show the current plan, its limits, usage, price, billing period and renewal date. Explain clearly how cancellation works and what happens to existing data if a subscription expires.

### 15.5 Payments in Pakistan
Start with a simple way to activate pilot subscriptions and record payments by hand. Before automating recurring billing, check the chosen provider's merchant facilities, recurring-payment support, fees and compliance requirements.

## 16. Team management and multiple stores

These features matter once sellers have staff or run more than one business.

### 16.1 Invite team members
The owner can invite staff to a workspace and assign suitable permissions.

### 16.2 Role-based access

| Role | Example permissions |
| --- | --- |
| Owner | Full access to the business |
| Manager | Orders, products, customers and reports |
| Order operator | Create and update orders |
| Inventory operator | Products and stock adjustments |
| Accountant | Expenses, payments and financial reports |

Sensitive actions, such as deleting records or exporting customer data, need extra permission checks.

### 16.3 Activity log
Record who created or changed an order, updated a payment, adjusted stock or changed permissions. Keep an audit trail of important business changes.

### 16.4 Multiple business profiles
Let an owner switch between stores without mixing their customer lists, inventory, sales or expenses. This can be a paid feature once the single-store experience is proven.

## 17. Settings, personalization and accessibility

The Settings module lets sellers fit the app to how they work.

- **Language**: English, Urdu and Roman Urdu for AI interactions.
- **Appearance**: Light and dark themes.
- **Currency**: PKR by default, with more currencies optional later.
- **Business details**: Logo, name, contact information and invoice details.
- **Order settings**: Default payment methods, statuses and delivery charges.
- **Inventory settings**: Stock tracking and low-stock thresholds.
- **Notifications**: Choose categories and frequency.
- **AI preferences**: Default reply tone and visibility of AI usage.
- **Privacy**: Account management, data export and deletion requests.
- **Security**: Password or authentication management, session controls and available account-protection features.
- **Help and support**: FAQs, tutorials and a way to contact support.

Urdu is written right to left, so implement correct text direction and layout behaviour instead of just translating the English labels.

## 18. Admin panel for the SaaS owner

This is the internal platform management dashboard, separate from each seller's workspace.

### Admin features

1. **User management**: View account status, registration date and subscription tier.
2. **Business management**: Manage workspace status and handle account issues.
3. **Subscription management**: Configure plans, limits and trials, and approve manual payments.
4. **AI usage analytics**: Monitor request volume, failure rates and estimated model costs.
5. **Platform analytics**: Review active users, order volumes, upgrades and retention.
6. **Support tools**: Log and resolve customer support requests.
7. **Feature configuration**: Turn features on or off and set usage limits.
8. **Abuse prevention**: Detect suspicious usage, spam and attempts to get around limits.
9. **Audit logs**: Record administrative actions.
10. **Service health**: Monitor backend errors and integration failures.

> **Privacy requirement:** By default, administrators must not have unrestricted access to private customer conversations or business records. Any exceptional support access must be limited, justified, permission-controlled and audited.

## 19. Security, privacy and data reliability

These requirements are essential because BazaarPilot AI stores customer contact details and business financial records.

### 19.1 Data isolation
Every business record must belong to the correct business workspace. Firestore Security Rules and backend authorization must stop users from reading or changing another business's records.

### 19.2 Secure AI processing
AI API credentials must stay on the server. Send the AI provider only the information a task needs, and check the provider's data-retention policies before choosing one.

### 19.3 Audit trails
Keep a history of important changes to orders, payments, refunds, stock and permissions. Where possible, record financial corrections as new transactions instead of silently overwriting historical records.

### 19.4 Backup and recovery
Set a database backup policy, test restores, and handle failed writes and network interruptions gracefully.

### 19.5 Account deletion and data export
Give users a clear way to request account deletion and to export their business data. Explain how linked customer records, backups and legally required financial records are handled.

### 19.6 Offline and synchronization behaviour
A later version could let users draft orders offline and sync them when the connection returns. It must prevent duplicate submissions, and conflicts when the same record is changed on more than one device.

For the first release, reliable online operation is better than a complicated offline sync system.

## 20. Recommended database structure

A well-designed database prevents reporting errors and makes future features easier to add.

| Collection or entity | Purpose |
| --- | --- |
| `users` | User identity and account preferences |
| `businesses` | Business profile and settings |
| `business_members` | Workspace membership and permissions |
| `products` | Product catalogue and cost information |
| `product_variants` | Size, colour, SKU and variant-level stock |
| `customers` | Customer contact information |
| `orders` | Order header, status, source and totals |
| `order_items` | Products, quantities and price snapshots for each order |
| `payments` | Recorded payments, refunds and settlement references |
| `expenses` | Business expense records |
| `stock_movements` | Inventory additions, deductions and adjustments |
| `shipments` | Courier and delivery details |
| `ai_requests` | AI usage, status and cost tracking |
| `subscriptions` | Plan, billing status and usage limits |
| `audit_logs` | Important business and administrative changes |
| `notification_preferences` | User-selected notification settings |

### Important data rules

- Store money as integers in minor units where applicable, with an explicit currency. Don't use floating-point arithmetic for financial totals.
- Store timestamps consistently and display them in the user's configured time zone.
- Save product prices and costs as snapshots on order items, so historical orders don't change when catalogue prices are edited.
- Calculate order totals and financial summaries in trusted backend logic.
- Use idempotency protection for transactions that might be submitted more than once.
- Keep payment records, order status and shipment status separate.

This schema is a starting point. Finalize the exact structure alongside the transaction and reporting logic.

## 21. End-to-end example: how all features work together

Take a home-based clothing business using BazaarPilot AI.

1. **A customer sends a message.** The seller gets an order request on Instagram or WhatsApp.
2. **The AI prepares the order.** The seller shares the message with the app, and the AI extracts the products, quantities, prices and delivery details.
3. **The seller confirms the order.** They fill in missing details, select or create the customer, and save the order.
4. **Inventory updates.** If stock tracking is on, the system records the stock movement the configured order policy calls for.
5. **The order is packed and dispatched.** The seller updates the status and records the courier and tracking number.
6. **The payment is reconciled.** Once delivery and COD settlement are confirmed, the seller records the payment received.
7. **Profit is calculated.** The system combines the sale, the product cost and recorded expenses into a clearly defined estimate.
8. **The dashboard and reports update.** The new records appear in the order, sales, payment, inventory and reporting views.

The payoff: one confirmed order feeds several parts of the business without the seller re-entering the same information.

## 22. What to build first versus later

There are many possible features, and you shouldn't try to build them all before launching.

### Phase 1: Launchable MVP

A complete, usable workflow for a single business.

- [ ] Authentication and business setup
- [ ] Home dashboard
- [ ] Manual order creation and order list
- [ ] AI message-to-order extraction with confirmation
- [ ] Product catalogue and basic variants
- [ ] Customer profiles and order history
- [ ] Payment and delivery status tracking
- [ ] Basic expenses and profit estimate
- [ ] Simple notifications
- [ ] Secure workspace access and basic audit logging

*These are planning checkboxes, not implementation status.*

### Phase 2: Paid SaaS growth

- Subscription management and usage limits
- CSV/PDF exports and advanced reports
- Team accounts and role-based permissions
- Stock movement reports
- AI-generated replies and daily summaries
- Better onboarding and retention analytics

### Phase 3: Automation and expansion

- Official messaging-platform integrations
- Courier API integrations
- Voice and screenshot extraction
- Multi-store management
- Advanced AI insights
- More accounting and business integrations

## 23. How to measure whether BazaarPilot AI is succeeding

Track these metrics from the first pilot:

| Metric | What it tells you |
| --- | --- |
| Time to first order | How quickly a new user gets value |
| AI extraction accuracy | How often extracted fields match the original message |
| Order correction rate | How much manual fixing the AI output needs |
| Weekly active sellers | Whether users keep coming back |
| Orders processed per seller | Whether the app has become part of daily operations |
| Trial-to-paid conversion | Whether users see enough value to pay |
| Monthly churn | Whether customers keep subscribing |
| AI cost per active seller | Whether subscription pricing can cover the service |
| Support requests per seller | Whether the experience is easy to understand |

For the first pilot, prioritize repeat usage, time saved and willingness to pay over downloads or total sign-ups.

## Final recommendation

Build BazaarPilot AI around one promise:

> **"Turn customer messages into organized orders and understand your business without complicated software."**

The first release should do three things exceptionally well:

1. Extract order information accurately from English, Urdu and Roman Urdu messages.
2. Manage orders from confirmation to delivery, with reliable payment tracking.
3. Give sellers a trustworthy estimate of sales and profit.

Everything else should either support those workflows or wait until users show demand for it.

The most important architectural decision is to **keep AI extraction separate from the transaction system**: the AI proposes the order, the seller confirms it, and trusted backend logic saves the order and calculates the financial records. That keeps the app more reliable, easier to test, and safer to scale into a paid SaaS business.
