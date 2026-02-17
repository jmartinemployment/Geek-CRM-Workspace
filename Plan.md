# Geek-CRM — Product Plan

> **CRM built for local service businesses** — IT consultants, contractors, tradespeople.
> Manages contacts, quotes, invoices, follow-ups, and service history.
> Embedded in WordPress via Angular Elements. Express + Prisma + Supabase backend.

---

## Table of Contents

1. [Vision & Problem Statement](#1-vision--problem-statement)
2. [Target Audience](#2-target-audience)
3. [Competitive Landscape](#3-competitive-landscape)
4. [Gap Analysis — What's Missing for Local Service Businesses](#4-gap-analysis--whats-missing-for-local-service-businesses)
5. [Product Differentiators](#5-product-differentiators)
6. [Feature Roadmap](#6-feature-roadmap)
7. [Architecture](#7-architecture)
8. [Database Schema (Prisma)](#8-database-schema-prisma)
9. [API Design](#9-api-design)
10. [Frontend Components](#10-frontend-components)
11. [AI Integration](#11-ai-integration)
12. [Deployment & Infrastructure](#12-deployment--infrastructure)
13. [Phased Implementation](#13-phased-implementation)
14. [Success Metrics](#14-success-metrics)

---

## 1. Vision & Problem Statement

### The Problem

Only **1 in 5** small business owners are happy with how they manage customer relationships. Only **10%** believe existing business software is designed for businesses their size. Most CRMs are built for SaaS sales teams, not for a plumber tracking service calls or an IT consultant managing retainer clients.

Local service businesses deal with:
- **Scattered customer data** — contacts in phones, emails, spreadsheets, sticky notes
- **Manual quoting** — copy-pasting customer info, hunting down prices, formatting PDFs
- **Quote-to-invoice delays** — 5-10 days between quote approval and invoice, killing cash flow
- **Missed follow-ups** — 50% of leads go cold because response takes 4-24 hours
- **No service history** — technician arrives and doesn't know what was done last visit
- **Tool overload** — paying for 4-5 separate tools that don't talk to each other

### The Vision

**Geek-CRM** is a CRM purpose-built for Geek At Your Spot and similar local service businesses. It combines contacts, quoting, invoicing, scheduling, and follow-up automation in one interface that a non-technical business owner can use from day one — no onboarding fee, no 12-tab navigation, no sales jargon.

It lives inside the existing WordPress site, so clients interact with it where they already go. It's powered by Claude AI to draft quotes, suggest follow-ups, and surface insights — but stays out of the way when not needed.

---

## 2. Target Audience

### Primary: Geek At Your Spot (Internal Use)

- Jeff's own IT consultancy business
- 30+ years of client relationships to manage
- Service calls, project quotes, retainer tracking
- Broward & Palm Beach County, Florida

### Secondary: Other Local Service Businesses (Future SaaS)

| Vertical | Examples | Key Needs |
|----------|----------|-----------|
| IT Services | MSPs, break-fix, network setup | Retainer tracking, ticket-to-invoice, service history |
| Home Services | Plumbers, electricians, HVAC | Job scheduling, on-site quotes, mobile access |
| Landscaping | Lawn care, tree service | Recurring service schedules, seasonal pricing |
| Contractors | General, roofing, painting | Project-based quotes, milestone invoicing |
| Professional Services | Accountants, bookkeepers, consultants | Time tracking, retainer billing |

### User Persona

**"Mike the Owner-Operator"**
- Runs a 1-10 person service business
- Not technical — uses phone + email primarily
- Needs something simpler than HubSpot but more capable than a spreadsheet
- Budget: $0-50/user/month
- Decision criteria: "Can I figure this out in 10 minutes?"

---

## 3. Competitive Landscape

### Competitor Analysis

#### HubSpot CRM

| Aspect | Detail |
|--------|--------|
| **Free Tier** | Yes — up to 2 users, basic contacts, 2,000 emails/month |
| **Paid Starting** | $20/user/month (Starter); $100/month (Professional) |
| **Target** | Startups to Enterprise (sales & marketing teams) |
| **Strengths** | Massive ecosystem, excellent marketing tools, brand recognition, generous free tier |
| **Weaknesses** | Essential features locked behind $90+/user paywalls; overwhelming UI with 10+ tabs; $3,000+ onboarding fees; AI implementation described as "thrown in"; free tier locks out records after 30 days; steep jump from Starter to Professional |
| **Integrations** | 1,500+ via marketplace |
| **Fit for Service SMBs** | Poor — built for SaaS/B2B sales teams, not service businesses. No quoting, invoicing, or service history. |

#### Zoho CRM

| Aspect | Detail |
|--------|--------|
| **Free Tier** | Yes — up to 3 users |
| **Paid Starting** | $14/user/month (Standard) |
| **Target** | SMBs, growing teams |
| **Strengths** | Affordable (94% of users say so); massive Zoho ecosystem (40+ apps); good automation in higher tiers; quote & inventory management in Professional plan |
| **Weaknesses** | Overwhelming UI with too many buttons/tabs; steep learning curve; frequent bugs, crashes, syncing errors; two-way email sync not on lowest tier; no centralized contact history view; essential features spread across price points; hidden add-on costs |
| **Integrations** | 800+ native + Zoho ecosystem |
| **Fit for Service SMBs** | Moderate — has quoting in Pro plan but complexity is a barrier. Feature sprawl means paying for things you don't need. |

#### Freshsales (Freshworks)

| Aspect | Detail |
|--------|--------|
| **Free Tier** | Yes — up to 3 users |
| **Paid Starting** | $9/user/month (Growth) |
| **Target** | SMBs, mid-size sales teams |
| **Strengths** | Freddy AI for lead scoring; built-in phone/email/SMS/WhatsApp; clean interface; affordable starting price; multi-channel inbox |
| **Weaknesses** | Only 1 pipeline on cheaper plans; 4x price jump between Growth and Pro ($11 to $47); no email sequences on low tier; reporting difficult for non-technical users; recurring bugs; duplicate management only on expensive plans |
| **Integrations** | 100+ native, Freshworks ecosystem |
| **Fit for Service SMBs** | Moderate — good UX but still sales-team focused. No invoicing, no service history, no field service features. |

#### Pipedrive

| Aspect | Detail |
|--------|--------|
| **Free Tier** | No — 14-day trial only |
| **Paid Starting** | $14/seat/month (Essential) |
| **Target** | Small sales teams |
| **Strengths** | Excellent visual pipeline; unlimited contacts/deals on all plans; 500+ integrations; fast setup; strong sales focus |
| **Weaknesses** | No custom objects; no branching automation logic (if/else); email campaigns require add-on ($16+/month); add-ons can double total cost; weak marketing automation; limited reporting; poor fit for 20+ person teams |
| **Integrations** | 500+ apps |
| **Fit for Service SMBs** | Poor — purely sales pipeline focused. No quoting, invoicing, scheduling, or service tracking. |

#### Monday.com CRM

| Aspect | Detail |
|--------|--------|
| **Free Tier** | No — 3-seat minimum on all plans |
| **Paid Starting** | $12/user/month (Basic, 3-seat minimum = $36/month) |
| **Target** | Small teams, visual-first users |
| **Strengths** | Highly visual (Kanban, table, chart, timeline views); very customizable boards; unlimited pipelines from $10/user; intuitive drag-and-drop; good templates |
| **Weaknesses** | 3-seat minimum kills solo/micro use; "very weak" customer support (chat only, no phone); missing email sequences and lead scoring; overwhelming automation UI; tons of bugs including "large ones that make features unusable"; Google Calendar sync not on lower tiers |
| **Integrations** | 200+ native |
| **Fit for Service SMBs** | Poor — project management tool with CRM skin. No invoicing, quoting, or service-specific features. |

#### Insightly

| Aspect | Detail |
|--------|--------|
| **Free Tier** | Yes — up to 2 users (limited) |
| **Paid Starting** | $29/user/month (Plus, annual only) |
| **Target** | SMBs wanting CRM + project management |
| **Strengths** | Combined CRM + project management; good reporting/dashboards; drag-and-drop pipelines; quote generation in Enterprise plan |
| **Weaknesses** | 12 navigation tabs (complex UI); setup takes weeks; no email sequencing at all; $29 starting price is high for limited features; confusing pricing; weak integrations; limited mobile offline access; annual billing only |
| **Integrations** | 250+ via AppConnect |
| **Fit for Service SMBs** | Moderate — project management angle is useful for service businesses, but high price and complexity are barriers. |

### Pricing Comparison Matrix

| CRM | Free Users | Starter $/user/mo | Pro $/user/mo | Quoting | Invoicing | Service History |
|-----|-----------|-------------------|---------------|---------|-----------|----------------|
| HubSpot | 2 | $20 | $100 | No | No | No |
| Zoho | 3 | $14 | $23 | Pro plan | No | No |
| Freshsales | 3 | $9 | $47 | No | No | No |
| Pipedrive | 0 | $14 | $49 | No | No | No |
| Monday.com | 0 | $36 (3-seat min) | $84 (3-seat min) | No | No | No |
| Insightly | 2 | $29 | $49 | Enterprise | No | No |
| **Geek-CRM** | **1** | **$0 (internal)** | **—** | **Phase 1** | **Phase 2** | **Phase 1** |

---

## 4. Gap Analysis — What's Missing for Local Service Businesses

### Critical Gaps in Existing CRMs

| Gap | Impact | Which CRMs Have It |
|-----|--------|--------------------|
| **Quote-to-invoice pipeline** | 5-10 day delay per deal, lost cash flow | None natively (Zoho has quoting only in Pro) |
| **Service history per client** | Technician arrives blind, repeat diagnostics | None |
| **On-site mobile quoting** | Owner quotes from memory, inconsistent pricing | Freshsales has mobile but not quoting |
| **Automated follow-up sequences** | 50% of leads go cold waiting 4-24 hrs | HubSpot ($90+/user), Freshsales (Pro only) |
| **Recurring service scheduling** | Manual calendar management, missed appointments | None (these are CRMs, not FSM tools) |
| **Simple UI for non-technical users** | All competitors have 10+ navigation tabs | None score well here |
| **WordPress-native embedding** | Business owners already manage their site in WP | None |
| **AI-powered quote drafting** | Manual price lookups, inconsistent estimates | None |
| **Under $20/user/month with quoting** | Most small service businesses have $0-50 budget | Zoho ($23 Pro), but complex |

### The Core Insight

**No existing CRM combines contacts + quoting + invoicing + service history + follow-up automation in a simple interface for under $25/user/month.** They're all either:
- Too complex (HubSpot, Zoho, Insightly)
- Too sales-focused (Pipedrive, Monday.com)
- Missing critical service-business features (all of them)
- Too expensive for the features you actually need (HubSpot, Insightly)

---

## 5. Product Differentiators

### What Makes Geek-CRM Different

1. **Built for service businesses, not SaaS sales teams**
   - Service history timeline per client (what was done, when, by whom)
   - On-site quoting from mobile (generate a quote while at the client's location)
   - Recurring service schedules (monthly maintenance, quarterly checkups)

2. **Quote-to-invoice in one click**
   - Client approves quote → invoice auto-generated
   - Payment tracking built in (not a separate tool)
   - Template library for common service types

3. **5-minute setup, 3-tab navigation**
   - Contacts | Quotes & Invoices | Activity
   - No 12-tab enterprise UI
   - No $3,000 onboarding fee

4. **AI-assisted, not AI-dependent**
   - Claude drafts quotes based on service history and pricing rules
   - Suggests follow-up timing based on client patterns
   - Summarizes client history before a visit
   - Always optional — owner stays in control

5. **WordPress-native**
   - Runs inside the existing business website
   - No separate app to log into
   - Clients can request service through the same site

6. **Transparent, affordable pricing**
   - Free for Geek At Your Spot internal use
   - Future SaaS: flat rate under $20/user/month, no feature gating, no add-on upsells

---

## 6. Feature Roadmap

### Phase 1 — Core CRM (MVP)

| Feature | Priority | Description |
|---------|----------|-------------|
| Contact management | P0 | Clients, companies, with custom fields and tags |
| Service history | P0 | Timeline of all services performed per client |
| Quote builder | P0 | Create, send, track quotes with line items |
| Deal pipeline | P0 | Visual pipeline (Lead → Quoted → Approved → Scheduled → Completed) |
| Activity feed | P0 | Unified log of calls, emails, notes, quotes per contact |
| Dashboard | P0 | Overview cards (open quotes, upcoming jobs, revenue this month) |
| Search & filter | P1 | Find contacts, quotes, services by any field |
| Notes & attachments | P1 | Add notes and files to contacts and quotes |
| Tags & segments | P1 | Categorize contacts (residential, commercial, retainer) |

### Phase 2 — Quoting & Invoicing

| Feature | Priority | Description |
|---------|----------|-------------|
| Quote templates | P0 | Pre-built templates for common service types |
| Quote → Invoice conversion | P0 | One-click convert approved quote to invoice |
| Invoice tracking | P0 | Track sent, viewed, paid, overdue statuses |
| Payment reminders | P1 | Automated email reminders at 7, 14, 30 days |
| Line item pricing library | P1 | Reusable service/product catalog with rates |
| PDF generation | P1 | Professional PDF quotes and invoices |
| Client portal | P2 | Clients view/approve quotes and pay invoices online |

### Phase 3 — Automation & Follow-ups

| Feature | Priority | Description |
|---------|----------|-------------|
| Email sequences | P0 | Automated follow-up emails after quote sent |
| Task reminders | P0 | "Follow up with client X in 3 days" |
| Scheduled follow-ups | P1 | Auto-schedule check-ins after service completion |
| Lead capture form | P1 | WordPress form → CRM contact auto-creation |
| SMS notifications | P2 | Appointment reminders and quote approvals via SMS |
| Recurring services | P2 | Auto-generate quotes/invoices for recurring maintenance |

### Phase 4 — AI & Insights

| Feature | Priority | Description |
|---------|----------|-------------|
| AI quote drafting | P0 | Claude generates quote based on service request + history |
| Client summary | P0 | AI-generated briefing before a service visit |
| Revenue forecasting | P1 | Predict monthly revenue from pipeline + recurring services |
| Churn risk alerts | P1 | Flag clients who haven't engaged in X months |
| Smart pricing suggestions | P2 | Suggest pricing based on similar past jobs |
| Natural language search | P2 | "Show me all commercial clients in Boca Raton" |

### Phase 5 — Integrations & Scale

| Feature | Priority | Description |
|---------|----------|-------------|
| Google Calendar sync | P0 | Two-way sync for service appointments |
| Email integration | P0 | Log emails sent/received per contact |
| QuickBooks / Xero | P1 | Sync invoices to accounting software |
| Stripe / Square payments | P1 | Accept payments directly from invoices |
| Zapier / webhook triggers | P2 | Connect to other tools |
| Multi-user access | P2 | Role-based access for team members |

---

## 7. Architecture

### System Overview

```
WordPress (geekatyourspot.com)
  └── Angular Elements (geek-crm-elements)
        ├── <geek-crm-dashboard>     → FlowDashboardComponent (main view)
        ├── <geek-crm-contacts>      → ContactListComponent
        ├── <geek-crm-contact>       → ContactDetailComponent
        ├── <geek-crm-quotes>        → QuoteListComponent
        └── <geek-crm-quote-builder> → QuoteBuilderComponent
              │
              ▼
        GeekCrmApiService (HttpClient)
              │
              ▼
Express Backend (geek-crm-backend, port 3300)
  ├── /api/contacts     → Contact CRUD
  ├── /api/companies    → Company CRUD
  ├── /api/quotes       → Quote CRUD + PDF
  ├── /api/invoices     → Invoice CRUD + payments
  ├── /api/services     → Service history
  ├── /api/activities   → Activity log
  ├── /api/pipeline     → Deal stages
  ├── /api/ai           → Claude AI endpoints
  └── /health           → Health check
              │
              ▼
Supabase PostgreSQL (via Prisma ORM)
```

### Tech Stack

| Layer | Technology | Notes |
|-------|-----------|-------|
| Frontend | Angular 21 + Angular Elements | Standalone components, signals, OnPush, zoneless |
| CSS | Bootstrap 5.3.8 SCSS | Consistent with other Geek projects |
| Backend | Express.js + TypeScript | Port 3300 |
| Database | PostgreSQL via Supabase | Prisma ORM, connection pooling |
| AI | Anthropic Claude API (Sonnet 4+) | Quote drafting, client summaries |
| PDF | @react-pdf/renderer or pdfkit | Quote/invoice PDF generation |
| Email | SendGrid | Transactional emails, follow-up sequences |
| Hosting (frontend) | WordPress via FTP | wp_enqueue_script_module() |
| Hosting (backend) | Render.com | Same pattern as geek-flow-backend |
| Payments (Phase 5) | Stripe | Invoice payments |

### Workspace Structure

```
Geek-CRM-Workspace/
  projects/
    geek-crm-library/           # Angular library
      src/
        public-api.ts
        lib/
          models/
            contact.model.ts
            company.model.ts
            quote.model.ts
            invoice.model.ts
            service-record.model.ts
            activity.model.ts
            pipeline.model.ts
          services/
            geek-crm-api.service.ts
          components/
            crm-dashboard/
            contact-list/
            contact-detail/
            contact-form/
            quote-list/
            quote-builder/
            quote-detail/
            invoice-list/
            invoice-detail/
            service-history/
            activity-feed/
            pipeline-board/
            search-bar/
    geek-crm-elements/          # Angular Elements app
      src/main.ts
    geek-crm-backend/           # Express + TypeScript
      prisma/schema.prisma
      src/
        server.ts               # Port 3300
        config/
          environment.ts
          database.ts
          logger.ts
        routes/
          contacts.ts
          companies.ts
          quotes.ts
          invoices.ts
          services.ts
          activities.ts
          pipeline.ts
          ai.ts
        middleware/
          error-handler.ts
          auth.ts
        utils/
          errors.ts
          response.ts
          pdf.ts
        templates/
          quote.template.ts
          invoice.template.ts
  angular.json
  tsconfig.json
  CLAUDE.md
  Plan.md                       # This file
```

---

## 8. Database Schema (Prisma)

### Core Models

```prisma
model User {
  id            String    @id @default(uuid())
  email         String    @unique
  name          String
  role          UserRole  @default(OWNER)
  createdAt     DateTime  @default(now())
  updatedAt     DateTime  @updatedAt

  contacts      Contact[]
  quotes        Quote[]
  invoices      Invoice[]
  activities    Activity[]
}

model Contact {
  id            String    @id @default(uuid())
  firstName     String
  lastName      String
  email         String?
  phone         String?
  mobile        String?
  address       String?
  city          String?
  state         String?
  zip           String?
  type          ContactType  @default(RESIDENTIAL)
  tags          String[]
  notes         String?
  companyId     String?
  company       Company?  @relation(fields: [companyId], references: [id])
  userId        String
  user          User      @relation(fields: [userId], references: [id])
  createdAt     DateTime  @default(now())
  updatedAt     DateTime  @updatedAt

  quotes        Quote[]
  invoices      Invoice[]
  serviceRecords ServiceRecord[]
  activities    Activity[]
}

model Company {
  id            String    @id @default(uuid())
  name          String
  website       String?
  phone         String?
  address       String?
  city          String?
  state         String?
  zip           String?
  industry      String?
  notes         String?
  createdAt     DateTime  @default(now())
  updatedAt     DateTime  @updatedAt

  contacts      Contact[]
}

model Quote {
  id            String      @id @default(uuid())
  quoteNumber   String      @unique
  contactId     String
  contact       Contact     @relation(fields: [contactId], references: [id])
  userId        String
  user          User        @relation(fields: [userId], references: [id])
  title         String
  description   String?
  lineItems     Json        // Array of { description, quantity, unitPrice, total }
  subtotal      Decimal     @db.Decimal(10, 2)
  taxRate       Decimal?    @db.Decimal(5, 4)
  taxAmount     Decimal?    @db.Decimal(10, 2)
  total         Decimal     @db.Decimal(10, 2)
  status        QuoteStatus @default(DRAFT)
  validUntil    DateTime?
  sentAt        DateTime?
  approvedAt    DateTime?
  notes         String?
  createdAt     DateTime    @default(now())
  updatedAt     DateTime    @updatedAt

  invoice       Invoice?
  activities    Activity[]
}

model Invoice {
  id            String        @id @default(uuid())
  invoiceNumber String        @unique
  quoteId       String?       @unique
  quote         Quote?        @relation(fields: [quoteId], references: [id])
  contactId     String
  contact       Contact       @relation(fields: [contactId], references: [id])
  userId        String
  user          User          @relation(fields: [userId], references: [id])
  lineItems     Json
  subtotal      Decimal       @db.Decimal(10, 2)
  taxRate       Decimal?      @db.Decimal(5, 4)
  taxAmount     Decimal?      @db.Decimal(10, 2)
  total         Decimal       @db.Decimal(10, 2)
  amountPaid    Decimal       @default(0) @db.Decimal(10, 2)
  status        InvoiceStatus @default(DRAFT)
  dueDate       DateTime?
  sentAt        DateTime?
  paidAt        DateTime?
  notes         String?
  createdAt     DateTime      @default(now())
  updatedAt     DateTime      @updatedAt

  payments      Payment[]
  activities    Activity[]
}

model Payment {
  id            String    @id @default(uuid())
  invoiceId     String
  invoice       Invoice   @relation(fields: [invoiceId], references: [id])
  amount        Decimal   @db.Decimal(10, 2)
  method        PaymentMethod
  reference     String?
  paidAt        DateTime  @default(now())
  createdAt     DateTime  @default(now())
}

model ServiceRecord {
  id            String    @id @default(uuid())
  contactId     String
  contact       Contact   @relation(fields: [contactId], references: [id])
  title         String
  description   String?
  serviceDate   DateTime
  duration      Int?      // minutes
  technician    String?
  category      String?
  tags          String[]
  notes         String?
  attachments   Json?     // Array of { name, url, type }
  createdAt     DateTime  @default(now())
  updatedAt     DateTime  @updatedAt

  activities    Activity[]
}

model Activity {
  id            String       @id @default(uuid())
  type          ActivityType
  title         String
  description   String?
  contactId     String?
  contact       Contact?     @relation(fields: [contactId], references: [id])
  quoteId       String?
  quote         Quote?       @relation(fields: [quoteId], references: [id])
  invoiceId     String?
  invoice       Invoice?     @relation(fields: [invoiceId], references: [id])
  serviceId     String?
  service       ServiceRecord? @relation(fields: [serviceId], references: [id])
  userId        String
  user          User         @relation(fields: [userId], references: [id])
  metadata      Json?
  createdAt     DateTime     @default(now())
}

model PricingItem {
  id            String    @id @default(uuid())
  name          String
  description   String?
  category      String
  unitPrice     Decimal   @db.Decimal(10, 2)
  unit          String    @default("each") // each, hour, project, month
  isActive      Boolean   @default(true)
  createdAt     DateTime  @default(now())
  updatedAt     DateTime  @updatedAt
}

// Enums

enum UserRole {
  OWNER
  ADMIN
  TECHNICIAN
  VIEWER
}

enum ContactType {
  RESIDENTIAL
  COMMERCIAL
  REFERRAL
  LEAD
}

enum QuoteStatus {
  DRAFT
  SENT
  VIEWED
  APPROVED
  DECLINED
  EXPIRED
  CONVERTED
}

enum InvoiceStatus {
  DRAFT
  SENT
  VIEWED
  PARTIAL
  PAID
  OVERDUE
  CANCELLED
}

enum PaymentMethod {
  CASH
  CHECK
  CREDIT_CARD
  BANK_TRANSFER
  STRIPE
  OTHER
}

enum ActivityType {
  NOTE
  CALL
  EMAIL
  QUOTE_CREATED
  QUOTE_SENT
  QUOTE_APPROVED
  QUOTE_DECLINED
  INVOICE_CREATED
  INVOICE_SENT
  INVOICE_PAID
  SERVICE_COMPLETED
  FOLLOW_UP
  MEETING
  TASK
}
```

---

## 9. API Design

### Contact Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| `POST` | `/api/contacts` | Create contact |
| `GET` | `/api/contacts` | List contacts (paginated, filterable by type/tags/search) |
| `GET` | `/api/contacts/:id` | Get contact with company, recent activity, service history |
| `PUT` | `/api/contacts/:id` | Update contact |
| `DELETE` | `/api/contacts/:id` | Soft-delete contact |
| `GET` | `/api/contacts/:id/timeline` | Full activity timeline for a contact |

### Company Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| `POST` | `/api/companies` | Create company |
| `GET` | `/api/companies` | List companies |
| `GET` | `/api/companies/:id` | Get company with contacts |
| `PUT` | `/api/companies/:id` | Update company |
| `DELETE` | `/api/companies/:id` | Soft-delete company |

### Quote Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| `POST` | `/api/quotes` | Create quote |
| `GET` | `/api/quotes` | List quotes (filterable by status) |
| `GET` | `/api/quotes/:id` | Get quote with line items |
| `PUT` | `/api/quotes/:id` | Update quote |
| `DELETE` | `/api/quotes/:id` | Delete draft quote |
| `POST` | `/api/quotes/:id/send` | Mark as sent, trigger email |
| `POST` | `/api/quotes/:id/approve` | Client approval |
| `POST` | `/api/quotes/:id/decline` | Client decline |
| `POST` | `/api/quotes/:id/convert` | Convert to invoice |
| `POST` | `/api/quotes/:id/duplicate` | Clone quote |
| `GET` | `/api/quotes/:id/pdf` | Generate PDF |

### Invoice Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| `POST` | `/api/invoices` | Create invoice |
| `GET` | `/api/invoices` | List invoices (filterable by status) |
| `GET` | `/api/invoices/:id` | Get invoice with payments |
| `PUT` | `/api/invoices/:id` | Update invoice |
| `POST` | `/api/invoices/:id/send` | Send invoice email |
| `POST` | `/api/invoices/:id/payment` | Record payment |
| `GET` | `/api/invoices/:id/pdf` | Generate PDF |

### Service History Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| `POST` | `/api/services` | Log service record |
| `GET` | `/api/services` | List service records |
| `GET` | `/api/services/:id` | Get service detail |
| `PUT` | `/api/services/:id` | Update service record |

### Activity Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| `POST` | `/api/activities` | Log activity (note, call, email) |
| `GET` | `/api/activities` | List activities (filterable) |

### AI Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| `POST` | `/api/ai/draft-quote` | Generate quote from service request description |
| `POST` | `/api/ai/client-summary` | Generate pre-visit client briefing |
| `POST` | `/api/ai/suggest-followup` | Suggest follow-up action + timing |

### Pricing Library Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| `POST` | `/api/pricing` | Add pricing item |
| `GET` | `/api/pricing` | List pricing items by category |
| `PUT` | `/api/pricing/:id` | Update pricing item |
| `DELETE` | `/api/pricing/:id` | Deactivate pricing item |

### Dashboard Endpoint

| Method | Endpoint | Description |
|--------|----------|-------------|
| `GET` | `/api/dashboard` | Aggregated stats (open quotes, revenue, upcoming, overdue) |

---

## 10. Frontend Components

### Registered Custom Elements (WordPress-facing)

| Custom Element Tag | Component | Purpose |
|---|---|---|
| `<geek-crm-dashboard>` | `CrmDashboardComponent` | Main entry — overview cards + navigation |
| `<geek-crm-contacts>` | `ContactListComponent` | Searchable contact directory |
| `<geek-crm-contact>` | `ContactDetailComponent` | Single contact view with timeline |
| `<geek-crm-quotes>` | `QuoteListComponent` | Quote management grid |
| `<geek-crm-quote-builder>` | `QuoteBuilderComponent` | Create/edit quotes with line items |

### Internal Components (used within registered elements)

| Component | Purpose |
|-----------|---------|
| `ContactFormComponent` | Create/edit contact reactive form |
| `ServiceHistoryComponent` | Service record timeline for a contact |
| `ActivityFeedComponent` | Activity log with filters |
| `PipelineBoardComponent` | Visual Kanban pipeline (drag-and-drop) |
| `QuoteDetailComponent` | Read-only quote view with status actions |
| `InvoiceListComponent` | Invoice management grid |
| `InvoiceDetailComponent` | Invoice view with payment recording |
| `LineItemEditorComponent` | Reusable line item table (quotes + invoices) |
| `PricingPickerComponent` | Select from pricing library when building quotes |
| `SearchBarComponent` | Global search across contacts, quotes, services |
| `StatCardComponent` | Dashboard metric card (reusable) |
| `TagInputComponent` | Tag entry/display for contacts |
| `StatusBadgeComponent` | Color-coded status indicator |

---

## 11. AI Integration

### Claude API Usage

All AI features use the Anthropic Claude API (Sonnet 4+) through the Express backend. The frontend never calls Claude directly.

#### Quote Drafting

**Input:** Service request description + client history + pricing library
**Output:** Structured quote with line items, descriptions, and suggested pricing

```
User: "Client needs network setup for new office — 8 drops, switch, access point,
       cable runs through drop ceiling. Small office in Boca Raton."

Claude → {
  title: "Network Infrastructure Setup — New Office",
  lineItems: [
    { description: "Cat6 cable runs (drop ceiling)", qty: 8, unit: "each", price: 125 },
    { description: "48-port managed switch", qty: 1, unit: "each", price: 450 },
    { description: "Wireless access point (Wi-Fi 6E)", qty: 1, unit: "each", price: 275 },
    { description: "Network configuration & testing", qty: 4, unit: "hours", price: 150 },
    { description: "Cable termination & labeling", qty: 8, unit: "each", price: 35 }
  ],
  notes: "Includes cable testing and documentation. Access point placement
          optimized for office layout. 1-year warranty on installation labor."
}
```

#### Client Summary (Pre-Visit Briefing)

**Input:** Contact ID
**Output:** Natural language summary of client history, recent interactions, open items

```
"Mike Rodriguez (Rodriguez Plumbing) — Commercial client since 2019.
Last visit: Jan 15 — replaced backup drive on server, updated Windows on 3 workstations.
Open quote: $2,400 for security camera system (sent Dec 20, no response).
Note: Prefers morning appointments. Usually pays by company check within 15 days.
Suggestion: Follow up on security camera quote — it's been 8 weeks."
```

#### Follow-Up Suggestions

**Input:** Contact activity history
**Output:** Recommended action + timing + draft message

---

## 12. Deployment & Infrastructure

### Backend Deployment (Render.com)

| Setting | Value |
|---------|-------|
| Service name | `geek-crm-backend` |
| Build command | `npm install && npx prisma generate && npm run build` |
| Start command | `npm run start` |
| Health check | `/health` |
| Port | `3300` |
| Environment | `DATABASE_URL`, `DIRECT_URL`, `ANTHROPIC_API_KEY`, `CORS_ORIGIN`, `SENDGRID_API_KEY` |

### Frontend Deployment (WordPress FTP)

1. `ng build geek-crm-library`
2. `ng build geek-crm-elements` → produces `main.js` + `styles.css`
3. FTP upload to `themes/geek-at-your-spot/assets/geek-crm/`
4. WordPress page template loads via `wp_enqueue_script_module()`

### Database (Supabase)

- New Supabase project: `geek-crm`
- Region: `us-east-1` (same as other projects)
- Connection pooling via shared pooler for Render compatibility

---

## 13. Phased Implementation

### Phase 1 — Core CRM (4-6 sessions)

**Goal:** Contacts + service history + basic dashboard working end-to-end.

| Session | Deliverables |
|---------|-------------|
| 1 | Workspace setup, Claude init, Plan.md (THIS SESSION) |
| 2 | Angular library + Elements app scaffolding, backend scaffolding, Prisma schema, basic routes (contacts, companies, activities) |
| 3 | ContactListComponent, ContactDetailComponent, ContactFormComponent, ServiceHistoryComponent |
| 4 | CrmDashboardComponent with stat cards, ActivityFeedComponent, SearchBarComponent |
| 5 | Supabase setup, Render deployment, WordPress page template, FTP upload |
| 6 | Testing, bug fixes, polish |

### Phase 2 — Quoting & Invoicing (3-4 sessions)

**Goal:** Full quote lifecycle — create, send, approve, convert to invoice, record payment.

| Session | Deliverables |
|---------|-------------|
| 7 | Quote routes, QuoteBuilderComponent, LineItemEditorComponent, PricingItem model |
| 8 | QuoteDetailComponent, quote PDF generation, email sending |
| 9 | Invoice routes, quote-to-invoice conversion, InvoiceListComponent, InvoiceDetailComponent |
| 10 | Payment recording, status tracking, payment reminders, pipeline board |

### Phase 3 — Automation & Follow-ups (2-3 sessions)

**Goal:** Automated follow-ups, task reminders, lead capture.

| Session | Deliverables |
|---------|-------------|
| 11 | Email sequence engine, follow-up scheduling, task reminder system |
| 12 | WordPress lead capture form → CRM, recurring service setup |
| 13 | SMS integration (Twilio), notification preferences |

### Phase 4 — AI & Insights (2-3 sessions)

**Goal:** Claude-powered quote drafting, client summaries, revenue forecasting.

| Session | Deliverables |
|---------|-------------|
| 14 | AI quote drafting endpoint + UI, pricing library management |
| 15 | Client summary generation, pre-visit briefing, follow-up suggestions |
| 16 | Revenue dashboard, forecasting, churn risk alerts |

### Phase 5 — Integrations & Scale (2-3 sessions)

**Goal:** Connect to external tools, multi-user support.

| Session | Deliverables |
|---------|-------------|
| 17 | Google Calendar sync, email logging |
| 18 | Stripe payment integration, QuickBooks sync |
| 19 | Multi-user access, role-based permissions, Zapier webhooks |

---

## 14. Success Metrics

### Phase 1 Launch Criteria

- [ ] Can create, view, edit, delete contacts
- [ ] Can log service records against a contact
- [ ] Can view full activity timeline per contact
- [ ] Dashboard shows at-a-glance metrics
- [ ] Works inside WordPress page
- [ ] Mobile responsive
- [ ] Backend deployed on Render with health check passing

### Phase 2 Launch Criteria

- [ ] Can create quotes with line items from pricing library
- [ ] Can send quotes via email
- [ ] Can convert approved quote to invoice in one click
- [ ] Can record payments against invoices
- [ ] PDF generation for quotes and invoices
- [ ] Pipeline board shows deal progression

### Business Goals (6-month)

- Geek At Your Spot uses Geek-CRM for 100% of client management
- Average quote creation time under 5 minutes
- Quote-to-invoice conversion under 1 minute (from current 5-10 days)
- No leads lost to missed follow-ups
- All client service history accessible from mobile

---

*Last Updated: February 17, 2026*
