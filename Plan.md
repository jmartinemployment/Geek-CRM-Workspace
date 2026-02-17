# Geek-CRM: Full HubSpot Clone — Implementation Plan

## Context

Geek-CRM is a full HubSpot clone — not a simplified or niche-adapted CRM, but a complete mirror of HubSpot's architecture, modules, navigation, data model, and feature set. Built as Angular Elements on WordPress + Express backend with Prisma ORM on Supabase PostgreSQL. The workspace exists but contains no projects or code yet — only Angular 21 workspace infrastructure from `ng new --no-create-application`.

---

## Competitive Analysis — CRM Market Differentiators

### What Makes Each CRM Different (Head-to-Head)

| CRM | Core Identity | What They Do That Nobody Else Does | Who It's Actually For | Fatal Flaw |
|---|---|---|---|---|
| **HubSpot** | All-in-one platform (Marketing + Sales + Service + Commerce unified) | Free tier that actually works (1K contacts, live chat, bots, email tracking). Sequences = multi-step email+task cadences. Prospecting Workspace = daily SDR queue | Mid-market companies ($10M-$500M revenue) willing to pay $90-$150/seat for full platform | 6x price cliff from Starter ($15) to Professional ($90). Features you need are always in the next tier |
| **Salesforce** | Metadata-driven enterprise platform + Agentforce AI | Joined Reports (cross-object). AppExchange (9K+ apps). Agentforce autonomous AI agents (SDR, Service, Sales Coach) with Atlas Reasoning Engine. Einstein AI: lead scoring, call transcription, email drafting, record summarization, case classification, forecasting | Enterprise (500+ employees) with dedicated CRM admin, $165/seat + $125/user/mo for AI, and $50K+ implementation budget | AI requires admin to configure (Agent Builder, Topics, Actions, Flows). Einstein needs training data. $290+/seat/mo for CRM+AI. Every feature needs admin setup |
| **Pipedrive** | Activity-based sales pipeline | Deal rotting indicator (configurable stale detection with color coding). Activity completion reports (coaches behaviors, not just outcomes). Pioneered drag-and-drop Kanban for CRM | Individual sales reps and small sales teams (1-50 users) focused purely on pipeline management | No marketing hub, no service hub, no custom objects. Reporting is weak. Doesn't scale past 50 users |
| **Zoho** | 50+ app ecosystem with native data sharing | Blueprint (stage-gating: required fields/actions before deal progression). Canvas Design Studio (drag-and-drop custom record layouts). Zia AI (sentiment + emotion + anomaly detection) | SMBs wanting everything (CRM + Books + Desk + Campaigns + 44 more) at $37/user for entire suite | Overwhelming complexity. UX quality varies wildly between apps. Setup takes weeks |
| **Freshsales** | Unified communication (phone + email + chat + WhatsApp + SMS all native) | Built-in VoIP telephony with call recording, voicemail drops, IVR routing — no Aircall/RingCentral needed. Freddy AI deal tags: Likely to Close / At Risk / Gone Cold | Small sales teams (3-50 users) who make lots of calls and want phone built into CRM | Contact limits (1K free, 10K on Growth). Only 20 workflow automations on Growth. CPQ is a paid add-on |
| **Monday.com** | Work OS (sales + project delivery + onboarding on same platform) | Multi-view on same data (Kanban, Timeline, Gantt, Calendar, Chart, Map, Workload). Closed deal auto-creates delivery project with zero handoff | Teams that need project management and CRM in one tool. Agencies, consultancies, creative shops | Not CRM-first — requires setup. Minimum 3 seats. No native sequences, no knowledge base |
| **Jobber** | Field service lifecycle (Lead → Quote → Job → Invoice → Paid) | Client Hub (self-serve portal for approvals + payments). Route Optimization. Chemical Tracking (regulatory compliance). GPS clock-in tracking | Trades and field service: plumbers, electricians, HVAC, pest control, lawn care (1-15 crew) | Only for field service. Basic reporting. Scales poorly past 15 users. Per-team pricing not per-seat |
| **Close** | Outbound calling as core product | Power Dialer (auto-advance through call list). Predictive Dialer (multi-line). Voicemail drops. Mixed-channel sequences (email + SMS + call in one cadence) | SDR/BDR teams doing high-volume outbound calling (50+ calls/day) | Expensive ($99/mo for 3 seats). No marketing hub. No service hub. Calling-centric only |
| **Copper** | Google Workspace native CRM | Lives inside Gmail — auto-captures contacts from email, full CRM sidebar in Chrome, Google Drive file storage. Only Google-endorsed CRM | Teams already all-in on Google Workspace who want CRM without leaving Gmail | Completely useless outside Google Workspace. Limited customization. Basic reporting |
| **Insightly** | Deal-to-Project bridge | One-click conversion from closed deal to delivery project with Gantt charts, milestones, and dependencies. Bridges sales and delivery natively | Professional services firms where every deal becomes a project (consulting, agencies, IT services) | Jack of all trades. Neither CRM nor PM is best-in-class. Limited automation |
| **Keap** | Automation-first for solopreneurs | Visual campaign builder with advanced branching. Built-in e-commerce (order forms, cart, payment triggers). Predictive email send times | Coaches, consultants, course creators running automated sales funnels ($249/mo for 1,500 contacts) | Expensive per contact. Steep learning curve for campaign builder. Not a real CRM for teams |
| **OnePageCRM** | GTD (Getting Things Done) philosophy | Every contact MUST have a "Next Action" with due date. Action Stream sorts by urgency. Cannot dismiss overdue actions. Reported 50%+ follow-up improvement | Solo salespeople and tiny teams who need forced follow-up discipline | Extremely simple. No marketing, no service, no automation. For follow-up discipline only |
| **Nutshell** | Simplicity + unlimited contacts | Free live support + free data migration on every plan. Unlimited contacts including cheapest tier ($13/seat). Rare in CRM market | Small teams (5-25 users) wanting simple CRM with no contact-based pricing surprises | Limited customization. Basic automation. No service hub. Small ecosystem |

### Where Each CRM Wins and Loses (Feature Gaps)

| Capability | Best-in-Class | Good | Weak/Missing |
|---|---|---|---|
| **Pipeline/Kanban** | Pipedrive (pioneered it) | HubSpot, Zoho, Freshsales, Monday | Close, Copper, OnePageCRM |
| **Reporting** | Salesforce (joined reports, dynamic dashboards) | HubSpot (Pro+), Zoho (Ultimate) | Pipedrive (consistently rated weak), Jobber |
| **Marketing Email** | HubSpot (native, full hub) | Zoho (via Campaigns), Keap | Pipedrive (add-on), Monday (none), Freshsales (via Freshmarketer) |
| **Phone/Calling** | Freshsales (native VoIP all tiers), Close (power+predictive dialer) | HubSpot (Pro+, minutes-based) | Pipedrive (add-on), Monday (none), Copper (none) |
| **Workflow Automation** | Salesforce (all tiers, unlimited), HubSpot (Pro+, 300 max) | Zoho (Pro+), Keap (visual builder) | Freshsales (20 max on Growth), Pipedrive (Growth+) |
| **Service/Tickets** | HubSpot (full Service Hub), Salesforce (Service Cloud) | Zoho (via Desk), Freshsales (via Freshdesk) | Pipedrive (none), Monday (none), Close (none) |
| **Quote-to-Invoice** | HubSpot (native free), Jobber (full lifecycle) | Zoho (Professional+), Pipedrive (Smart Docs add-on) | Salesforce (CPQ add-on), Freshsales (CPQ $19/user add-on) |
| **Knowledge Base** | HubSpot (Pro+) | Salesforce (Service Cloud) | Everyone else (none or via separate product) |
| **Custom Objects/Fields** | Salesforce (25+ field types, all tiers) | HubSpot (Enterprise only), Zoho (Enterprise) | Pipedrive (none), Freshsales (Enterprise), Monday (via boards) |
| **AI Features** | Salesforce (Einstein + Agentforce — requires Enterprise $165/seat + $125/user/mo + admin setup) | HubSpot (Breeze AI — Pro+ $90/seat), Zoho (Zia — Enterprise+), Freshsales (Freddy — all paid) | Pipedrive (Premium+), Monday (Pro+). All require admin configuration |
| **AI Without Admin** | None — all CRMs require admin/developer to configure AI features | — | Every CRM in the market |
| **Autonomous AI Agents** | Salesforce Agentforce (SDR, Service, Sales Coach — $125/user/mo + Enterprise) | HubSpot (Breeze Agents — limited, Pro+) | Everyone else (no autonomous agents) |
| **AI Lead/Deal Scoring** | Salesforce Einstein (needs training data + admin), Freshsales Freddy (all paid tiers) | HubSpot (Pro+ manual, Enterprise AI), Zoho Zia (Enterprise+) | Pipedrive (Premium+), Monday (none) |
| **AI Email Drafting** | Salesforce (Enterprise + Agentforce), HubSpot (Breeze, Pro+) | Zoho (Enterprise+), Freshsales (Pro+) | Pipedrive (none native), Monday (none) |
| **AI Call Summaries** | Salesforce Conversation Insights (Enterprise + $150/seat) | HubSpot (Enterprise $150/seat only) | Everyone else (none or third-party) |
| **Field Service** | Jobber (purpose-built) | — | Everyone else (not designed for it) |
| **Google Integration** | Copper (native, Google-endorsed) | HubSpot, Salesforce | — |
| **Follow-up Discipline** | OnePageCRM (forced next actions) | Pipedrive (activity-based) | Most CRMs (optional, not enforced) |
| **Multi-view (Kanban+Table+Calendar)** | Monday.com (7+ views on same data) | HubSpot, Pipedrive | Most CRMs (1-2 views only) |
| **Deal-to-Project Handoff** | Monday.com (auto-creates project), Insightly (one-click convert) | — | Everyone else (manual process) |

### HubSpot

| Category | Details |
|---|---|
| **Core Strength** | All-in-one platform: Marketing + Sales + Service + Commerce + Content unified on one data layer |
| **Free Tier** | Genuinely functional: 1,000 contacts, 1 pipeline, 1 meeting link, 2,000 email sends/mo, live chat, basic bots, 2 edit-access users |
| **Free Tier Limits** | 10 custom properties, 3 email templates, 5 snippets, 200 email tracking notifications/mo, zero workflows, zero sequences |
| **Key Feature Gates** | Sequences + Workflows + Forecasting require Professional ($90/seat/mo). Custom Objects require Enterprise ($150/seat/mo) |
| **Unique Features** | Sequences (multi-step email+task cadences), Playbooks (interactive call guides), Conversation Intelligence (call transcription+coaching, Enterprise only), Prospecting Workspace (daily SDR queue), Data Hub programmable automation (custom JS in workflows) |
| **Commerce** | Native quote-to-invoice with e-signatures, payment links, subscriptions — included free (payment processing fees only) |
| **Biggest Weakness** | Massive price cliff: Starter ($15/seat) to Professional ($90/seat) is a 6x jump. Custom objects gated to Enterprise ($150/seat). Forced onboarding fees ($1,500-$7,000/hub) |
| **Pricing** | Free / Starter $15/seat / Professional $90/seat / Enterprise $150/seat (annual, per hub) |

### Salesforce / Agentforce

| Category | Details |
|---|---|
| **Core Strength** | Metadata-driven platform — custom objects, page layouts, record types, validation rules, territory management, role hierarchies, all configurable without code |
| **Unique Features** | Joined Reports (multi-object in single report), Flow Builder (5 flow types including screen flows), AppExchange (9,000+ apps that install natively), Einstein Trust Layer (AI data isolation for regulated industries), Dynamic Dashboards (each viewer sees their own data scope) |
| **Agentforce AI** | Atlas Reasoning Engine (multi-step autonomous ReAct loop). Pre-built agents: Service Agent (24/7 customer service), SDR Agent (outbound prospecting + meeting booking), Sales Coach (deal-specific coaching), Marketing Agent (campaign building). Agent Builder for custom agents with Topics + Instructions + Actions (Flows, Apex, APIs, Knowledge) |
| **Einstein AI Features** | Lead Scoring (ML-trained on historical conversions), Opportunity Scoring (deal health), Activity Capture (auto-log emails/calendar), Forecasting (predictive revenue), Conversation Insights (call transcription + summaries), Case Classification (auto-triage), Reply Recommendations, Record Summarization, Email Drafting, Send Time Optimization, Engagement Scoring, Sentiment Analysis |
| **Agentforce Pricing** | Enterprise Edition minimum ($165/seat). Then: Flex Credits ($0.10/action), OR $2/conversation (customer-facing), OR $125/user/mo (unlimited internal). Data Cloud is a separate license. Implementation partners: $2K-$6K per agent quickstart |
| **AI Admin Requirement** | Agent Builder requires admin/developer. Needs understanding of Salesforce data model, Flows, Apex, permissions. Einstein Lead Scoring needs sufficient historical data. Case Classification needs 1,000+ labeled examples. Not plug-and-play |
| **Custom Objects** | Available on all paid tiers (vs. HubSpot Enterprise-only). 25+ field types including formula, roll-up summary, geolocation, encrypted |
| **Reporting** | Most powerful in market: tabular, summary, matrix, joined reports. Subscription reports, dynamic dashboards, CRM Analytics (full BI add-on) |
| **Biggest Weakness** | Total cost of ownership is extreme ($165-$500/seat + Agentforce $125/user/mo + Data Cloud license + $50K-$500K implementation). Every AI feature requires admin configuration. No self-serve AI for end users without admin setup |
| **Pricing** | Starter $25/seat / Professional $75/seat / Enterprise $165/seat / Unlimited $330/seat / Einstein 1 $500+/seat. Agentforce adds $125/user/mo or $2/conversation on top |

### Pipedrive

| Category | Details |
|---|---|
| **Core Strength** | Activity-based selling — every deal must have a scheduled next action. System warns when deals have no activity. Daily workflow optimized for the individual rep |
| **Unique Features** | Deal rotting indicator (configurable stale detection with color coding), Activity completion reports (shifts coaching from outcomes to behaviors), Smart Contact Data (auto-enrichment from email), Leadbooster ($32.50/mo per company — chatbot + live chat + prospector with 400M+ profiles), Smart Docs (proposals + eSignature built-in) |
| **Pipeline UX** | Pioneered drag-and-drop Kanban for CRM. Real-time filtering by owner/product/amount/date. Multiple pipelines with instant switching |
| **Biggest Weakness** | No native marketing hub, no service hub, no custom objects. Reporting consistently rated insufficient. Scales poorly beyond 50 users. No free tier |
| **Pricing** | Lite $14/seat / Growth $39/seat / Premium $49/seat / Ultimate $79/seat. Add-ons priced per company (not per user) |

### Zoho CRM

| Category | Details |
|---|---|
| **Core Strength** | 50+ app ecosystem (Zoho One at $37/user) with native data sharing — CRM + Books + Desk + Campaigns + Analytics + Projects + 44 more, no API connectors needed |
| **Unique Features** | Blueprint (stage-gating that enforces required fields/actions before deal progression), CommandCenter 2.0 (cross-department journey orchestration), Canvas Design Studio (drag-and-drop custom record layouts like a design tool), Zia AI (sentiment analysis, emotion detection, anomaly detection, intent analysis, generative module creation), SalesInbox (email client that sorts by relationship type) |
| **Multi-Channel** | Native email, VoIP (50+ telephony providers), social media, live chat (SalesIQ), SMS, WhatsApp Business — more channels than any competitor at this price |
| **Biggest Weakness** | Overwhelming setup complexity. UX quality varies across apps. Fewer third-party integrations vs. HubSpot/Salesforce. AI requires data volume to be effective |
| **Pricing** | Free (3 users) / Standard $14/seat / Professional $23/seat / Enterprise $40/seat / Ultimate $52/seat. Zoho One $37/user (all 50+ apps) |

### Freshsales (Freshworks)

| Category | Details |
|---|---|
| **Core Strength** | Truly unified communication — phone (built-in VoIP via Freshcaller), email, live chat, WhatsApp, Facebook Messenger, SMS all native with no third-party integrations |
| **Unique Features** | Built-in telephony with call recording, voicemail drops, IVR routing (no Aircall/RingCentral needed), Auto-profile enrichment (populates contact from email address), Freddy AI (deal tags: Likely to Close / At Risk / Gone Cold, next best action, sales forecasting), Territory management (Enterprise $59/seat) |
| **Biggest Weakness** | Contact limits (Free: 1,000, Growth: 10,000). Only 20 workflow automations on Growth tier. CPQ is a paid add-on ($19/user/mo). 5GB file storage on Pro |
| **Pricing** | Free (3 users) / Growth $9/seat / Pro $39/seat / Enterprise $59/seat |

### Monday.com CRM

| Category | Details |
|---|---|
| **Core Strength** | Work OS flexibility — sales pipeline, project delivery, onboarding, and support all on the same platform with shared data. A closed deal auto-creates a delivery project with zero manual handoff |
| **Unique Features** | Multi-view on same data (Kanban, Timeline, Gantt, Calendar, Chart, Map, Workload, List), Workdocs (collaborative docs tied to records with live data), Cross-board dashboards (widgets pull from multiple boards), Formula and Mirror columns, Autopilot hub (centralized automation management) |
| **Biggest Weakness** | Not CRM-first — requires setup time to configure. Minimum 3 seats. Mirror column 2-hop limit. 5-10 hrs/month maintenance overhead. AI features gated to Pro ($28/seat) |
| **Pricing** | Basic $12/seat / Standard $17/seat / Pro $28/seat / Enterprise custom. Minimum 3 seats |

### Jobber (Field Service)

| Category | Details |
|---|---|
| **Core Strength** | Purpose-built for trades/field service: Lead → Quote → Approved → Job → Scheduled → Completed → Invoice → Paid — entire lifecycle in one tool |
| **Unique Features** | Client Hub (self-serve portal for approvals, payments, service history), Online Booking widget, Route Optimization (native, not third-party), Chemical Tracking (regulatory compliance for pest/lawn), GPS tracking (clock-in activated), Job scheduling with drag-and-drop dispatch, Two-way SMS with business number |
| **Biggest Weakness** | Not suitable for non-field-service. Basic reporting. QuickBooks sync unreliable. Scales poorly beyond 15 users. Pricing per-team not per-seat |
| **Pricing** | Core $39/mo (1 user) / Connect $169/mo (5 users) / Grow $349/mo (10 users) / Plus $599/mo (15 users) |

### Other Notable CRMs

| CRM | Core Differentiator |
|---|---|
| **Close** | Outbound calling as core product: Power Dialer (auto-advance), Predictive Dialer (multi-line), voicemail drops, AI call summaries, mixed-channel sequences (email+SMS+call). Starts $99/mo for 3 seats |
| **Copper** | Only Google-endorsed CRM. Lives inside Gmail — auto-captures contacts from email, full CRM sidebar in Chrome, Google Drive file storage. Useless outside Google Workspace |
| **Insightly** | Deal-to-Project conversion in one click. Built-in project management (Gantt, milestones, dependencies). Bridges sales and delivery natively |
| **Keap** | Automation-first for coaches/consultants: visual campaign builder with advanced branching, built-in e-commerce (order forms, cart, payment triggers), predictive email send times. $249/mo for 1,500 contacts |
| **OnePageCRM** | GTD philosophy: every contact must have a "Next Action" with due date. Action Stream sorts by urgency. Cannot dismiss overdue actions. Reported 50%+ follow-up improvement |
| **Nutshell** | Free live support + free data migration + unlimited contacts on every plan (including cheapest at $13/seat). Rare in CRM market |

### Cross-Competitor Feature Matrix

| Feature | HubSpot | Salesforce | Pipedrive | Zoho | Freshsales | Monday |
|---|---|---|---|---|---|---|
| Free tier | Yes (1K contacts) | No | No | Yes (3 users) | Yes (3 users) | No |
| Custom objects | Enterprise only | All tiers | No | Enterprise | Enterprise | Via boards |
| Built-in phone | Pro+ (minutes) | Add-on | Add-on | Via PhoneBridge | Native (all tiers) | No |
| Email sequences | Professional+ | Add-on | Add-on | Professional+ | Pro+ | Via automations |
| Kanban pipeline | Yes | Yes | Best-in-class | Yes | Yes | Yes |
| Marketing email | Native | Via Pardot | Add-on ($13/mo) | Via Campaigns | Via Freshmarketer | No |
| Knowledge base | Pro+ | Via Service Cloud | No | No (via Desk) | No (via Freshdesk) | No |
| Workflow automation | Pro+ (300) | All tiers | Growth+ | Pro+ | Growth+ (20 max) | Standard+ |
| Quote/Invoice | Native (free) | CPQ add-on | Smart Docs add-on | Professional+ | CPQ add-on ($19) | No |
| AI lead scoring | Pro+ (manual) / Ent (AI) | Einstein (add-on) | Premium+ | Enterprise+ (Zia) | All paid (Freddy) | Pro+ |
| Custom record layouts | Enterprise | All tiers | No | Enterprise (Canvas) | No | Via board config |
| Process enforcement | No | Validation rules | No | Blueprint | No | Via automations |
| Multi-channel inbox | Pro+ (omnichannel) | Service Cloud | No | Native (6+ channels) | Native (6+ channels) | No |
| Meeting scheduler | Free (1 link) | Add-on | Add-on | Via Bookings | Via Freshchat | No |
| Reporting depth | Pro+ (custom) | Best-in-class | Weak | Ultimate (Analytics) | Pro+ | Cross-board |

### Geek-CRM Differentiators — How We Beat Every Competitor

#### vs. HubSpot (Primary Target)

HubSpot is the architecture blueprint — Geek-CRM mirrors its entire structure. The differentiator is **removing the price gates**:

| HubSpot Gates Behind $90-$150/seat | Geek-CRM Includes From Day One |
|---|---|
| Sequences (email+task cadences) — Professional $90/seat | Unlimited sequences, all step types |
| Workflows (automation) — Professional $90/seat, max 300 | Unlimited workflows, no cap |
| Custom objects — Enterprise $150/seat | Custom Properties on all objects, unlimited |
| Forecasting — Professional $90/seat | Revenue forecasting built into reporting |
| Conversation Intelligence (call transcription) — Enterprise $150/seat | AI call summaries via Claude API |
| Playbooks (interactive call guides) — Professional $90/seat | Playbook builder included |
| Custom record layouts — Enterprise $150/seat | Configurable record layouts |
| ABM tools — Professional $90/seat | Account-based views on companies |
| Calculated properties — Professional $90/seat | Formula fields on custom properties |
| Teams + hierarchies — Professional $90/seat | Role-based access with team structure |
| Onboarding fee — $1,500-$7,000/hub | Zero onboarding fee (self-hosted) |

**Additional HubSpot weaknesses exploited:**
- 30-day activity timeline cutoff on free → Geek-CRM: unlimited history
- 10 custom properties on free → Geek-CRM: unlimited custom properties
- 3 email templates on free → Geek-CRM: unlimited templates
- 5 snippets on free → Geek-CRM: unlimited snippets
- 200 email tracking notifications/mo → Geek-CRM: unlimited tracking
- No workflows on free/Starter → Geek-CRM: full workflow engine

#### vs. Salesforce / Agentforce

| Salesforce Weakness | Geek-CRM Advantage |
|---|---|
| $25-$500/seat + $50K-$500K implementation | Self-hosted, zero licensing |
| Requires dedicated admin for everything | Zero admin — all features work out-of-the-box |
| Steep learning curve, inconsistent UI | HubSpot-style clean UI, consistent across all modules |
| AppExchange dependency for basic features | All features native — no marketplace add-ons |
| Agentforce requires Enterprise ($165/seat) + $125/user/mo | Claude API: ~$20-50/mo total for full AI suite |
| Agent Builder needs admin to configure Topics, Instructions, Actions, Flows | AI features activate automatically — set API key, done |
| Einstein Lead Scoring needs historical training data | Claude-powered scoring works from day one via prompt reasoning |
| Data Cloud is a separate license for AI grounding | All CRM data used as AI context automatically via Prisma queries |
| Implementation partners charge $2K-$6K per agent | Zero implementation cost — every AI feature is built-in |
| Atlas Reasoning Engine logs require admin review | AI interaction audit trail viewable by any user |

**Borrowed from Salesforce/Agentforce:** Full AI feature map (lead scoring, deal health, email drafting, record summarization, call summaries, ticket classification, meeting prep, sales coaching, revenue forecasting, conversational assistant), cross-object reporting (joined reports), validation rules per stage, custom field types (formula, roll-up)

#### vs. Pipedrive

| Pipedrive Weakness | Geek-CRM Advantage |
|---|---|
| No native marketing hub | Full Marketing Hub (lists, email, forms) |
| No service/ticket hub | Full Service Hub (tickets, KB, chatflows, feedback) |
| No custom objects | Custom Properties on all objects |
| Weak reporting | Cross-object report builder with dashboards |
| No free tier | Self-hosted, unlimited users |

**Borrowed from Pipedrive:** Deal rotting indicator, activity-based selling (next action required on every deal), best-in-class Kanban UX

#### vs. Zoho CRM

| Zoho Weakness | Geek-CRM Advantage |
|---|---|
| Overwhelming setup complexity (50+ apps) | Single unified app, all modules built-in |
| UX quality varies across apps | Consistent HubSpot-style UI |
| Fewer third-party integrations | Focused feature set, no integration dependency |
| AI requires data volume | Claude API for AI from day one |

**Borrowed from Zoho:** Blueprint-style process enforcement (required fields/actions per pipeline stage), Canvas-style custom layouts, multi-channel communication architecture

#### vs. Freshsales

| Freshsales Weakness | Geek-CRM Advantage |
|---|---|
| Contact limits (1K free, 10K Growth) | Unlimited contacts |
| Only 20 workflow automations on Growth | Unlimited workflows |
| CPQ is a paid add-on ($19/user/mo) | Native quote-to-invoice lifecycle |
| 5GB file storage on Pro | Self-hosted storage, no limits |

**Borrowed from Freshsales:** Built-in telephony architecture (VoIP-ready), auto-profile enrichment pattern, Freddy-style deal health tags (Likely to Close / At Risk / Gone Cold)

#### vs. Monday.com CRM

| Monday.com Weakness | Geek-CRM Advantage |
|---|---|
| Not CRM-first — requires setup | Purpose-built CRM, zero configuration |
| Minimum 3 seats | Single-user capable |
| No native email sequences | Full sequence builder |
| No knowledge base | Full Knowledge Base with categories |

**Borrowed from Monday.com:** Multi-view on same data (Kanban, table, calendar), cross-board dashboards, formula columns

#### vs. Jobber (Field Service)

| Jobber Weakness | Geek-CRM Advantage |
|---|---|
| Only for field service businesses | Full CRM for any business type |
| Basic reporting | Cross-object report builder |
| Scales poorly beyond 15 users | No user limits |
| QuickBooks sync unreliable | Self-contained invoicing + payments |

**Borrowed from Jobber:** Quote → Job → Invoice → Paid lifecycle, Client Hub (self-serve portal), service history tracking, equipment/asset tracking per client

### Geek-CRM Feature Advantage Summary

| Feature | HubSpot Price Gate | Salesforce Price Gate | Geek-CRM |
|---|---|---|---|
| Sequences | $90/seat/mo | Add-on | Included |
| Workflows | $90/seat/mo (300 max) | All tiers | Included (unlimited) |
| Custom Properties | 10 on free, unlimited at $90/seat | All tiers | Included (unlimited) |
| Quote-to-Invoice | Free (but limited templates) | CPQ add-on | Included |
| Knowledge Base | $90/seat/mo | Service Cloud add-on | Included |
| Meeting Scheduler | 1 link free, unlimited at $15/seat | Add-on | Included (unlimited) |
| Pipeline Stage Enforcement | Not available | Validation rules (all tiers) | Included (Zoho Blueprint-style) |
| Deal Rotting Detection | Not available | Not native | Included (Pipedrive-style) |
| Cross-Object Reports | $90/seat/mo | All tiers (best-in-class) | Included |
| AI Lead/Deal Scoring | $90/seat + needs training data | Enterprise + Einstein (needs admin + data) | Included — works day one via Claude prompt reasoning |
| AI Email Drafting | Breeze AI $90/seat | Enterprise + Agentforce $125/user/mo | Included — context-grounded, zero config |
| AI Call Summaries | Enterprise $150/seat only | Enterprise + Conversation Insights $150/seat | Included — one-click on any call activity |
| AI Record Summarization | Breeze AI $90/seat | Enterprise + Agentforce $125/user/mo | Included — on every record page |
| AI Deal Coaching | Not available | Agentforce Sales Coach $125/user/mo + admin | Included — one-click on any deal |
| AI Ticket Classification | Not available | Einstein Case Classification (needs 1K+ examples + admin) | Included — auto-classifies on creation |
| AI Meeting Prep Briefings | Not available | Agentforce Assistant (Enterprise + admin) | Included — one-click before any meeting |
| AI Revenue Forecasting | $90/seat | Enterprise + Einstein (needs admin) | Included — pipeline + historical analysis |
| AI Conversational Assistant | Not available | Agentforce Assistant $125/user/mo (admin configures) | Included — Cmd+J on any page, zero config |
| AI Sentiment Analysis | Not available | Marketing Cloud Einstein (separate license) | Included — auto-tags on all text activities |
| AI Admin Required | N/A | Yes — Agent Builder, Topics, Instructions, Flows, Apex | **No — set API key, every feature works** |
| Custom Record Layouts | $150/seat/mo | All tiers | Included |
| E-Signatures on Quotes | $90/seat/mo | DocuSign add-on | Included |
| Feedback Surveys | $90/seat/mo | Service Cloud | Included |
| Chatflows/Bots | Basic free, advanced $90/seat | Service Cloud | Included |

### Key Takeaways

1. **HubSpot's Professional cliff is the #1 vulnerability** — Sequences, workflows, and forecasting all require $90/seat/mo. Geek-CRM includes everything with zero per-seat licensing.
2. **Custom Properties available from day one** — HubSpot gates to 10 on free, Salesforce charges enterprise pricing. Geek-CRM: unlimited.
3. **Unified communication architecture** — Freshsales proves built-in phone/email/chat wins users. Geek-CRM plans for native communication channels.
4. **Blueprint-style process enforcement** — Only Zoho offers stage-gating at SMB pricing. Geek-CRM implements required fields per pipeline stage.
5. **Activity-based selling (Pipedrive model)** — Next actions required on deals, overdue activities surfaced prominently.
6. **3-column record layout (HubSpot standard)** — Properties left, timeline center, associations right.
7. **Quote-to-Invoice lifecycle is native** — HubSpot, Jobber, and Zoho all prove this is table-stakes.
8. **Cross-object reporting (Salesforce capability)** — Joined reports across objects. Geek-CRM matches Salesforce's reporting depth.
9. **Client self-serve portal (Jobber model)** — Reduces support calls, increases payment speed.
10. **AI features via Claude API** — Full Agentforce-class AI with zero admin setup. Not gated behind enterprise pricing.

---

## AI Integration — Agentforce-Class Features Without an Administrator

### The Salesforce AI Problem

Salesforce Agentforce is powerful but inaccessible:
- **Requires Enterprise Edition minimum** ($165/seat/mo) before any AI features
- **Agentforce license adds $125/user/mo** (or $2/conversation for customer-facing)
- **Data Cloud is a separate license** (priced per data volume)
- **Requires admin/developer to configure** — Agent Builder, Topics, Instructions, Actions, Flows, Apex classes, Trust Layer rules, channel deployment
- **Implementation partners charge $2K-$6K** for a single agent quickstart
- **Training data minimums** — Einstein Lead Scoring needs sufficient historical conversions; Case Classification needs 1,000+ labeled examples

Geek-CRM provides the same capabilities powered by **Claude API (Sonnet 4+)**, working out-of-the-box with **zero configuration and zero admin**. Every AI feature activates automatically when the backend has a Claude API key.

### AI Feature Map — Salesforce vs. Geek-CRM

| Salesforce/Agentforce Feature | SF Price Gate | Geek-CRM Equivalent | How It Works |
|---|---|---|---|
| **Agentforce Assistant** (sidebar copilot) | Enterprise + $125/user/mo | **AI Assistant sidebar** | Conversational panel on every record page. Ask questions about the record, request summaries, draft emails — context-aware based on current page |
| **Einstein Lead Scoring** | Enterprise + Einstein add-on | **AI Lead Scoring** | Claude analyzes contact properties, activity history, engagement patterns, and deal associations to score leads 1-99. No training data minimum — works from day one using prompt-based reasoning |
| **Einstein Opportunity Scoring** | Enterprise + Einstein add-on | **AI Deal Health Tags** | Every open deal gets a tag: **Likely to Close** / **On Track** / **At Risk** / **Gone Cold**. Based on: days in stage, activity recency, email sentiment, stale detection, comparison to won/lost patterns |
| **Einstein Activity Capture** | Enterprise | **Activity auto-logging** | Phase 1 manual logging; future: email integration auto-captures sent/received emails to contact timeline |
| **Einstein Forecasting** | Enterprise + Einstein add-on | **AI Revenue Forecasting** | Claude analyzes pipeline by stage, historical close rates, deal velocity, and seasonal patterns to project monthly/quarterly revenue |
| **Conversation Insights** (call transcription + summaries) | Enterprise + $150/user/mo | **AI Call Summaries** | After logging a call activity, click "Summarize" — Claude generates: key topics discussed, commitments made, next steps, sentiment, and auto-creates follow-up tasks |
| **Einstein Email Drafting** | Enterprise + Agentforce | **AI Email Composer** | On any contact/deal record, click "Draft Email" — Claude generates personalized email grounded in: contact properties, deal stage, recent activities, communication history. User reviews and sends |
| **Einstein Record Summarization** | Enterprise + Agentforce | **AI Record Summary** | One-click summary on any Contact, Company, Deal, or Ticket. Returns: relationship overview, recent activity highlights, open deals/tickets, key dates, recommended next actions |
| **Einstein Meeting Prep** | Enterprise + Agentforce | **AI Meeting Briefing** | Before a meeting, generates: contact background, company context, open deals and their stages, recent communications, talking points, potential objections |
| **Einstein Case Classification** | Service Cloud + Einstein | **AI Ticket Classification** | When a ticket is created, Claude auto-suggests: priority, category, and pipeline routing based on subject + description. No training data needed — uses prompt-based reasoning |
| **Einstein Reply Recommendations** | Service Cloud + Einstein | **AI Ticket Reply Drafting** | On ticket detail, click "Draft Reply" — generates response grounded in: ticket description, activity history, knowledge base articles, and resolution patterns |
| **Einstein Case Summarization** | Service Cloud + Einstein | **AI Ticket Summary** | Summarizes entire ticket thread (all activities, notes, status changes) into a concise brief for agent handoff |
| **Agentforce SDR Agent** | Enterprise + $2/conversation | **AI Sequence Personalization** | When enrolling contacts in a sequence, Claude personalizes each automated email step using contact/company data. Not autonomous — user reviews before sending |
| **Agentforce Sales Coach** | Enterprise + $125/user/mo | **AI Deal Coaching** | On deal detail, click "Coach Me" — Claude analyzes the deal and provides: messaging recommendations, objection handling tips, suggested next steps, competitive positioning based on deal notes |
| **Einstein Send Time Optimization** | Marketing Cloud + Einstein | **AI Send Time Optimization** | Analyzes past email open/click data per contact to recommend optimal send times for campaigns and sequences |
| **Einstein Engagement Scoring** | Marketing Cloud + Einstein | **AI Engagement Scoring** | Scores contacts on engagement level (email opens, form submissions, page views, activity frequency) to identify hot leads and at-risk customers |
| **Einstein Copy Insights** | Marketing Cloud + Einstein | **AI Subject Line Suggestions** | When composing email campaigns, Claude suggests subject lines optimized for open rates based on audience and content |
| **Einstein Sentiment Analysis** | Marketing Cloud + Einstein | **AI Sentiment Analysis** | Analyzes email replies, notes, and ticket descriptions to tag sentiment (Positive / Neutral / Negative). Surfaces on contact and deal records |
| **Agentforce Service Agent** (autonomous) | Enterprise + $2/conversation | **AI Chatflow Responses** (future) | Phase 8: chatflow bot can use Claude to generate contextual responses grounded in knowledge base articles |
| **Einstein Wrap-Up Notes** | Service Cloud + Einstein | **AI Activity Wrap-Up** | After completing a call or meeting, Claude generates structured notes: summary, action items, follow-up date, and auto-populates the activity record |
| **Atlas Reasoning Engine** (multi-step) | Enterprise + Agentforce | **AI Workflow Suggestions** | Claude analyzes your CRM patterns and suggests workflows: "You often create a follow-up task 3 days after sending a quote — want to automate this?" |

### AI Integration Points Per Phase

| Phase | AI Features Added |
|---|---|
| **Phase 1** | AI Record Summary (contacts, companies), AI Email Composer, AI Sentiment Analysis on activities |
| **Phase 2** | AI Deal Health Tags, AI Deal Coaching, AI Call Summaries, AI Activity Wrap-Up |
| **Phase 3** | AI Ticket Classification, AI Ticket Reply Drafting, AI Ticket Summary |
| **Phase 4** | AI Quote personalization (cover letter generation), AI Invoice reminders (tone-aware follow-up) |
| **Phase 5** | AI Subject Line Suggestions, AI Engagement Scoring, AI Send Time Optimization |
| **Phase 6** | AI Sequence Personalization (per-contact email customization), AI Meeting Briefing |
| **Phase 7** | AI Workflow Suggestions (pattern-based automation recommendations) |
| **Phase 8** | AI Chatflow Responses (KB-grounded bot replies), AI Article Drafting |
| **Phase 9** | AI Revenue Forecasting, AI Dashboard Insights ("deals are closing 20% slower this month") |
| **Phase 10** | AI Assistant Sidebar (conversational copilot on every page), AI Global Search (natural language queries) |

### AI Architecture

```
┌─────────────────────────────────────────────────┐
│  Angular Frontend                                │
│  ┌───────────────┐  ┌────────────────────────┐  │
│  │ AI Assistant   │  │ Inline AI Buttons      │  │
│  │ Sidebar Panel  │  │ (Summarize, Draft,     │  │
│  │ (Cmd+J toggle) │  │  Coach, Classify...)   │  │
│  └───────┬───────┘  └───────────┬────────────┘  │
│          │                      │                │
│          └──────────┬───────────┘                │
│                     │                            │
└─────────────────────┼────────────────────────────┘
                      │ POST /ai/{action}
┌─────────────────────┼────────────────────────────┐
│  Express Backend     │                            │
│  ┌──────────────────┴──────────────────────────┐ │
│  │  AI Router (src/routes/ai.ts)               │ │
│  │                                              │ │
│  │  Endpoints:                                  │ │
│  │  POST /ai/summarize     {objectType, id}     │ │
│  │  POST /ai/draft-email   {contactId, context} │ │
│  │  POST /ai/score-lead    {contactId}          │ │
│  │  POST /ai/deal-health   {dealId}             │ │
│  │  POST /ai/coach-deal    {dealId}             │ │
│  │  POST /ai/classify-ticket {subject, desc}    │ │
│  │  POST /ai/draft-reply   {ticketId}           │ │
│  │  POST /ai/call-summary  {activityId}         │ │
│  │  POST /ai/meeting-prep  {contactId, dealId}  │ │
│  │  POST /ai/suggest-subject {campaignId}       │ │
│  │  POST /ai/forecast      {pipelineId, period} │ │
│  │  POST /ai/chat          {message, pageCtx}   │ │
│  └──────────────────┬──────────────────────────┘ │
│                     │                            │
│  ┌──────────────────┴──────────────────────────┐ │
│  │  AI Service (src/services/ai.service.ts)    │ │
│  │                                              │ │
│  │  1. Gather context from Prisma (record data, │ │
│  │     related records, activity history,       │ │
│  │     custom properties, KB articles)          │ │
│  │  2. Build structured prompt with system      │ │
│  │     instructions + record context            │ │
│  │  3. Call Claude API (Sonnet 4+)              │ │
│  │  4. Parse structured response                │ │
│  │  5. Return to frontend                       │ │
│  └──────────────────┬──────────────────────────┘ │
│                     │                            │
└─────────────────────┼────────────────────────────┘
                      │ Anthropic Messages API
                      ▼
            ┌─────────────────┐
            │  Claude Sonnet   │
            │  (Anthropic API) │
            └─────────────────┘
```

### AI Service Design Principles

1. **Zero configuration** — Every AI feature works the moment a `ANTHROPIC_API_KEY` env var is set. No training, no data minimums, no admin setup.
2. **Context grounding** — Every prompt includes relevant CRM data (contact, company, deal, activities, custom properties). Claude never hallucinates about the customer because it has the real data.
3. **User-in-the-loop** — AI drafts, suggests, and scores. Humans review and approve. No autonomous actions without explicit user consent.
4. **Graceful degradation** — If no API key is set, AI buttons simply don't render. The CRM works fully without AI.
5. **Cost-aware** — Each AI call uses Sonnet (fast, cheap). Only complex reasoning tasks (forecasting, coaching) use extended thinking. Responses are cached where appropriate (e.g., record summaries cached until record changes).
6. **Privacy-first** — No customer data is stored by Anthropic. Claude API has zero data retention by default. Equivalent to Salesforce's Einstein Trust Layer but with no configuration needed.

### AI Cost Projections

**Claude API Pricing (Sonnet 4+):** Input: $3/million tokens | Output: $15/million tokens
**Claude API Pricing (Haiku 4.5):** Input: $0.80/million tokens | Output: $4/million tokens

#### Per-Feature Cost Breakdown

| AI Feature | Avg Input Tokens | Avg Output Tokens | Cost Per Call | Model | Typical Monthly Usage (5 users) | Monthly Cost |
|---|---|---|---|---|---|---|
| Record Summary | ~2,000 | ~500 | $0.0135 | Sonnet | 200 summaries | $2.70 |
| Email Drafting | ~1,500 | ~400 | $0.0105 | Sonnet | 150 drafts | $1.58 |
| Sentiment Analysis | ~300 | ~50 | $0.0004 | Haiku | 500 activities | $0.22 |
| Deal Health Tags | ~2,500 | ~200 | $0.0105 | Sonnet | 100 deals scored | $1.05 |
| Deal Coaching | ~3,000 | ~800 | $0.0210 | Sonnet | 50 requests | $1.05 |
| Call Summaries | ~1,000 | ~600 | $0.0120 | Sonnet | 100 calls | $1.20 |
| Activity Wrap-Up | ~800 | ~400 | $0.0084 | Sonnet | 200 activities | $1.68 |
| Ticket Classification | ~500 | ~100 | $0.0008 | Haiku | 100 tickets | $0.08 |
| Ticket Reply Drafting | ~2,000 | ~500 | $0.0135 | Sonnet | 80 replies | $1.08 |
| Ticket Summary | ~2,000 | ~400 | $0.0120 | Sonnet | 50 summaries | $0.60 |
| Quote Cover Letter | ~1,500 | ~400 | $0.0105 | Sonnet | 30 quotes | $0.32 |
| Invoice Reminder | ~800 | ~300 | $0.0069 | Sonnet | 40 reminders | $0.28 |
| Subject Line Suggestions | ~500 | ~200 | $0.0045 | Sonnet | 20 campaigns | $0.09 |
| Engagement Scoring | ~1,500 (batch of 50) | ~300 | $0.0017 | Haiku | 10 batches (500 contacts) | $0.02 |
| Sequence Personalization | ~1,200 | ~400 | $0.0096 | Sonnet | 100 enrollments | $0.96 |
| Meeting Prep Briefing | ~3,000 | ~800 | $0.0210 | Sonnet | 40 meetings | $0.84 |
| Workflow Suggestions | ~4,000 | ~600 | $0.0210 | Sonnet | 10 requests | $0.21 |
| KB Article Drafting | ~2,000 | ~1,000 | $0.0210 | Sonnet | 10 articles | $0.21 |
| Chatflow Reply | ~1,500 | ~400 | $0.0105 | Sonnet | 200 bot replies | $2.10 |
| Revenue Forecasting | ~5,000 | ~800 | $0.0270 | Sonnet | 10 forecasts | $0.27 |
| Dashboard Insights | ~3,000 | ~500 | $0.0165 | Sonnet | 20 insights | $0.33 |
| AI Assistant Chat | ~2,000 | ~500 | $0.0135 | Sonnet | 300 messages | $4.05 |
| AI Search | ~500 | ~300 | $0.0060 | Sonnet | 100 searches | $0.60 |

#### Monthly Cost by Team Size

| Team Size | Usage Level | Est. AI Calls/mo | Raw Monthly Cost | With Caching + Haiku | Salesforce Equivalent |
|---|---|---|---|---|---|
| **1 user** (solo) | Light | ~500 | $5-10/mo | **$3-6/mo** | $290/mo |
| **5 users** (small team) | Moderate | ~2,500 | $20-35/mo | **$12-20/mo** | $1,450/mo |
| **10 users** (mid team) | Heavy | ~6,000 | $45-75/mo | **$25-45/mo** | $2,900/mo |
| **25 users** (larger team) | Heavy | ~15,000 | $100-180/mo | **$60-110/mo** | $7,250/mo |

#### Cost Reduction Strategies (Built Into Design Principles)

| Strategy | Savings | How |
|---|---|---|
| **AI Caching** (`AICache` table) | 30-50% reduction | Record summaries and deal health scores cached until record changes. Same summary served to multiple users requesting it |
| **Haiku for simple tasks** | 60-70% cheaper per call | Sentiment analysis, ticket classification, engagement scoring use Haiku ($0.80/$4 per million) instead of Sonnet ($3/$15 per million) |
| **Batch operations** | Reduces per-call overhead | Engagement scoring and lead scoring run in batches of 50-100 contacts, not one-at-a-time |
| **Graceful degradation** | $0 when unused | AI buttons don't render without API key. No background AI costs — only on-demand user-triggered calls |

#### Cost Visibility

The `AIInteraction` audit table tracks every call's `tokensUsed` and `durationMs`, enabling:
- Real-time cost dashboard (tokens * price per token)
- Per-user usage breakdown
- Per-feature cost analysis
- Monthly spend alerts
- Usage trending (identify cost spikes)

#### Bottom Line

For a 5-user team with all 24 AI features active: **~$12-20/month** in Claude API costs versus **$1,450/month** for Salesforce Enterprise + Agentforce. That is **~99% cheaper** with zero admin setup required.

### Database Additions for AI

```prisma
// Add to Phase 1 migration
model AIInteraction {
  id          String   @id @default(cuid())
  action      String   // summarize, draft-email, score-lead, etc.
  objectType  String?  // CONTACT, DEAL, TICKET, etc.
  objectId    String?
  userId      String
  prompt      String   @db.Text  // For audit trail
  response    String   @db.Text  // AI response
  tokensUsed  Int      @default(0)
  durationMs  Int      @default(0)
  cached      Boolean  @default(false)
  createdAt   DateTime @default(now())
}

model AICache {
  id          String   @id @default(cuid())
  cacheKey    String   @unique  // hash of objectType+objectId+action
  response    String   @db.Text
  expiresAt   DateTime
  createdAt   DateTime @default(now())
}
```

### API Endpoints for AI (added to each phase)

| Method | Path | Description | Phase |
|---|---|---|---|
| POST | `/ai/summarize` | Summarize any record | 1 |
| POST | `/ai/draft-email` | Draft email for contact/deal | 1 |
| POST | `/ai/sentiment` | Analyze sentiment of text | 1 |
| POST | `/ai/deal-health` | Score deal health (Likely/At Risk/Cold) | 2 |
| POST | `/ai/coach-deal` | Deal coaching recommendations | 2 |
| POST | `/ai/call-summary` | Summarize call activity + create tasks | 2 |
| POST | `/ai/wrap-up` | Generate activity wrap-up notes | 2 |
| POST | `/ai/classify-ticket` | Auto-classify ticket priority/category | 3 |
| POST | `/ai/draft-reply` | Draft ticket reply grounded in KB | 3 |
| POST | `/ai/ticket-summary` | Summarize ticket thread | 3 |
| POST | `/ai/quote-cover` | Generate quote cover letter | 4 |
| POST | `/ai/invoice-reminder` | Draft tone-aware payment reminder | 4 |
| POST | `/ai/subject-lines` | Suggest email subject lines | 5 |
| POST | `/ai/engagement-score` | Score contact engagement level | 5 |
| POST | `/ai/personalize-step` | Personalize sequence email for contact | 6 |
| POST | `/ai/meeting-prep` | Generate meeting briefing | 6 |
| POST | `/ai/suggest-workflow` | Suggest automations from patterns | 7 |
| POST | `/ai/draft-article` | Draft KB article from ticket resolutions | 8 |
| POST | `/ai/chatflow-reply` | Generate bot reply from KB | 8 |
| POST | `/ai/forecast` | Revenue forecast for pipeline/period | 9 |
| POST | `/ai/dashboard-insight` | Natural language insight on dashboard data | 9 |
| POST | `/ai/chat` | Conversational assistant (page-context-aware) | 10 |
| POST | `/ai/search` | Natural language search across all objects | 10 |

---

## Recommended Geek-CRM Tier Pricing

### Pricing Philosophy

HubSpot's #1 vulnerability is the **6x price cliff** from Starter ($15/seat) to Professional ($90/seat). Every feature a growing business actually needs — sequences, workflows, custom reporting, forecasting — sits behind that $90/seat wall. Geek-CRM's pricing strategy is to **collapse that cliff**: deliver Professional-tier functionality at Starter-tier pricing, and use AI as a value multiplier rather than a cost gate.

**Key principles:**
- No per-seat pricing on Free or Starter — removes growth anxiety
- AI features included at every paid tier — not gated behind enterprise
- Unlimited contacts at every tier — no contact-based pricing surprises (borrowed from Nutshell)
- Self-hosted option always available for $0 (open-core model)

### Tier Structure

#### Free — $0/mo (up to 2 users)

*Competes with: HubSpot Free, Zoho Free (3 users), Freshsales Free (3 users)*

| Category | What's Included | HubSpot Free Comparison |
|---|---|---|
| **Users** | 2 edit-access users | 2 edit-access users (same) |
| **Contacts** | Unlimited | 1,000 contact limit |
| **Pipelines** | 1 deal pipeline, 1 ticket pipeline | 1 deal pipeline only |
| **Custom Properties** | 25 per object | 10 total |
| **Email Templates** | 10 | 3 |
| **Activities** | Unlimited history | 30-day cutoff |
| **CRM Core** | Contacts, Companies, Deals, Tickets, Activities | Same |
| **Quotes/Invoices** | 5 quotes/mo, 5 invoices/mo | Unlimited quotes (no invoices) |
| **Forms** | 1 published form | Unlimited (HubSpot-branded) |
| **Lists** | 5 static lists | 10 static lists |
| **Meeting Links** | 1 per user | 1 per user (same) |
| **Reporting** | 3 default dashboards (not customizable) | 3 default dashboards |
| **AI Features** | None | None |
| **Support** | Community forum + docs | Community forum + docs |

**Why it wins:** Unlimited contacts + unlimited activity history + 25 custom properties vs. HubSpot's 1K contacts + 30-day cutoff + 10 properties.

---

#### Starter — $19/user/mo (billed annually) · $25/user/mo (monthly)

*Competes with: HubSpot Starter ($15/seat), Pipedrive Lite ($14/seat), Zoho Standard ($14/seat), Freshsales Growth ($9/seat)*

| Category | What's Included | HubSpot Starter ($15/seat) |
|---|---|---|
| **Users** | Unlimited | Unlimited |
| **Contacts** | Unlimited | 1,000 (marketing contacts extra) |
| **Pipelines** | 3 deal + 2 ticket pipelines | 2 deal pipelines |
| **Custom Properties** | Unlimited | 1,000 |
| **Email Templates** | Unlimited | 5,000 |
| **Sequences** | 3 active sequences, 50 enrollments/mo | **Not available** ($90/seat required) |
| **Workflows** | 5 active workflows | **Not available** ($90/seat required) |
| **Email Campaigns** | 5,000 sends/mo | 5,000 sends/mo (same) |
| **Forms** | Unlimited (no branding) | Unlimited (HubSpot branding removed) |
| **Lists** | 25 active + unlimited static | 25 active + 25 static |
| **Meeting Links** | Unlimited | Unlimited (same) |
| **Products** | Full catalog | Full catalog (same) |
| **Quotes/Invoices** | Unlimited | Unlimited quotes (no invoices) |
| **Knowledge Base** | 25 articles | **Not available** ($90/seat required) |
| **Reporting** | 10 custom dashboards, 25 reports | 10 dashboards (same) |
| **AI Features** | AI Record Summaries, AI Email Drafting, AI Sentiment Analysis | **None** (AI requires $90/seat) |
| **Support** | Email support (24hr response) | Email support |

**Why it wins:** Sequences + workflows + AI + knowledge base + invoicing — all features HubSpot gates behind $90/seat — for $19/seat. That's a **79% savings** on the features growing businesses need most.

**Geek-CRM hosting cost at this tier:** ~$25/mo (Render + Supabase) + ~$5-10/mo Claude API = **~$30-35/mo base cost** regardless of seats.

---

#### Professional — $39/user/mo (billed annually) · $49/user/mo (monthly)

*Competes with: HubSpot Professional ($90/seat), Pipedrive Growth ($39/seat), Zoho Professional ($23/seat), Freshsales Pro ($39/seat)*

| Category | What's Included | HubSpot Professional ($90/seat) |
|---|---|---|
| **Users** | Unlimited | 5 included (additional $90 each) |
| **Pipelines** | Unlimited | 15 deal pipelines |
| **Sequences** | Unlimited active, unlimited enrollments | 5,000 sequences, 500 enrollments/user/day |
| **Workflows** | Unlimited active | 300 workflows |
| **Email Campaigns** | 25,000 sends/mo | 20,000 sends/mo |
| **Custom Properties** | Unlimited + formula fields | 1,000 + calculated properties |
| **Pipeline Stage Enforcement** | Required fields/actions per stage (Zoho Blueprint-style) | **Not available** |
| **Deal Rotting Detection** | Configurable per stage (Pipedrive-style) | **Not available** |
| **Lists** | Unlimited active + static | 1,000 active + static |
| **Meeting Links** | Unlimited + team scheduling | Unlimited + round-robin |
| **Knowledge Base** | Unlimited articles + categories | 5,000 articles |
| **Chatflows** | Live chat + bot builder | Live chat + bot builder (same) |
| **Feedback Surveys** | NPS, CSAT, CES | NPS, CSAT, CES (same) |
| **Reporting** | Unlimited dashboards, cross-object reports | 25 dashboards, custom reports |
| **Goals** | Sales + service goals | Revenue goals + custom goals |
| **AI Features** | **Full AI suite** — all 24 AI features including Deal Health Tags, Deal Coaching, Call Summaries, Ticket Classification, Meeting Prep, Sequence Personalization, Revenue Forecasting, AI Assistant (Cmd+J) | AI email writer (basic), ChatSpot assistant |
| **E-Signatures** | Built-in on quotes | Included |
| **Support** | Priority email (8hr response) + live chat | Phone + email |

**Why it wins:** Full AI suite (Agentforce-class) + unlimited workflows + unlimited sequences + stage enforcement + deal rotting + cross-object reports — at **57% less** than HubSpot Professional. No forced onboarding fee (HubSpot charges $3,000).

**Geek-CRM hosting cost at this tier:** ~$50/mo (Render Pro + Supabase Pro) + ~$15-30/mo Claude API = **~$65-80/mo base cost**.

---

#### Enterprise — $69/user/mo (billed annually) · $89/user/mo (monthly)

*Competes with: HubSpot Enterprise ($150/seat), Salesforce Enterprise ($165/seat), Salesforce + Agentforce ($290/seat)*

| Category | What's Included | HubSpot Enterprise ($150/seat) | Salesforce + Agentforce ($290/seat) |
|---|---|---|---|
| **Everything in Professional** | Yes | Yes | Yes |
| **Custom Objects** | Custom record types with full CRUD | Custom objects (limited) | Full custom objects |
| **Role-Based Access** | Teams, hierarchies, field-level permissions | Teams + permissions | Profiles + permission sets |
| **Audit Logs** | Full activity + AI interaction audit trail | Audit log | Shield Event Monitoring (add-on) |
| **Multi-Currency** | Unlimited currencies | 30 currencies | Unlimited |
| **Advanced Reporting** | Joined reports (Salesforce-style), scheduled reports, export | Custom attribution, revenue analytics | Joined reports, dynamic dashboards |
| **AI Features** | Everything in Professional + AI Workflow Suggestions + AI Dashboard Insights + priority AI processing (larger context windows) | Conversation Intelligence, Predictive Lead Scoring | Full Agentforce (requires admin setup) |
| **Sandbox** | Staging environment for testing | Sandbox included | Full + partial sandboxes |
| **API Access** | Full REST API with webhooks | 500K API calls/day | Unlimited API |
| **SSO/SAML** | SAML 2.0 SSO | SAML SSO | SAML SSO |
| **Dedicated Support** | Dedicated account manager + Slack channel | Phone + email priority | Premier Success (add-on) |
| **SLA** | 99.9% uptime guarantee | 99.9% | 99.9% |
| **Onboarding Fee** | **$0** | **$7,000** | **$50K-$500K implementation** |

**Why it wins:** Full Agentforce-class AI without an administrator, no onboarding fee, at **54% less** than HubSpot Enterprise and **76% less** than Salesforce + Agentforce.

**Geek-CRM hosting cost at this tier:** ~$100/mo (Render + Supabase + Redis) + ~$30-60/mo Claude API = **~$130-160/mo base cost**.

---

### Pricing Comparison Summary

| | Geek-CRM | HubSpot | Salesforce | Pipedrive | Zoho |
|---|---|---|---|---|---|
| **Free** | 2 users, unlimited contacts | 2 users, 1K contacts | No free tier | No free tier | 3 users |
| **Starter** | **$19/seat** | $15/seat | $25/seat | $14/seat | $14/seat |
| **Professional** | **$39/seat** | $90/seat | $75/seat | $49/seat | $23/seat |
| **Enterprise** | **$69/seat** | $150/seat | $165/seat | $79/seat | $40/seat |
| **With AI** | **Included at every paid tier** | $90-$150/seat | +$125/user/mo (Agentforce) | Premium+ only | Enterprise+ (Zia) |
| **Sequences** | Starter ($19) | Professional ($90) | Add-on | Add-on | Professional ($23) |
| **Workflows** | Starter ($19) | Professional ($90) | All tiers | Growth ($39) | Professional ($23) |
| **Onboarding Fee** | **$0** | $1,500-$7,000 | $50K-$500K | $0 | $0 |

### Revenue Model

| Tier | Price/Seat/Mo | 5-User Team Revenue | 10-User Team Revenue | Hosting Cost | AI Cost | Gross Margin |
|---|---|---|---|---|---|---|
| Free | $0 | $0 | $0 | ~$25/mo | $0 | -$25/mo (lead gen) |
| Starter | $19 | $95/mo | $190/mo | ~$30/mo | ~$10/mo | **~58-79%** |
| Professional | $39 | $195/mo | $390/mo | ~$65/mo | ~$25/mo | **~54-77%** |
| Enterprise | $69 | $345/mo | $690/mo | ~$130/mo | ~$50/mo | **~48-74%** |

### Feature Gate Strategy — What Drives Upgrades

| Free → Starter | Starter → Professional | Professional → Enterprise |
|---|---|---|
| More than 2 users | Unlimited sequences + workflows | Custom objects |
| AI features (any) | Full AI suite (24 features) | Role-based access + teams |
| Unlimited quotes/invoices | Pipeline stage enforcement | Joined reports |
| More than 1 pipeline | Deal rotting detection | Sandbox environment |
| Email campaigns | Cross-object reports | SSO/SAML |
| Remove branding on forms | Bot builder (chatflows) | API access + webhooks |
| Email support | AI Assistant sidebar (Cmd+J) | Dedicated support |

### Self-Hosted Option — $0 Forever

For technical users who want to run their own instance:
- Full source code (open-core license)
- Deploy on own infrastructure (Docker + PostgreSQL)
- All features of Enterprise tier
- Bring your own Claude API key
- No per-seat fees, no monthly fees
- Community support only
- No SLA guarantee

This is the Geek At Your Spot internal deployment model — zero recurring cost beyond hosting (~$25-100/mo) and Claude API usage (~$12-60/mo).

---

## HubSpot Architecture Overview

### Navigation Structure (Left Icon Rail — HubSpot Style)

HubSpot uses a vertical left icon rail that expands on hover to show section labels. Each section contains sub-pages accessed via top tabs or sub-navigation.

| Icon | Section | Sub-Pages |
|---|---|---|
| Home | **Home** | Activity feed, tasks due today |
| Users | **CRM** | Contacts, Companies, Deals, Tickets, Activities |
| Megaphone | **Marketing** | Lists, Email, Forms, Landing Pages |
| Dollar | **Sales** | Deals (board), Tasks, Sequences, Meetings, Quotes |
| Cart | **Commerce** | Products, Quotes, Invoices, Payments, Payment Links |
| Headset | **Service** | Tickets, Knowledge Base, Feedback Surveys, Chatflows |
| Lightning | **Automations** | Workflows, Sequences |
| Chart | **Reporting** | Dashboards, Reports, Goals |
| Gear | **Settings** | Account, Users, Properties, Pipelines, Integrations |

### Record Layout (3-Column — HubSpot Pattern)

Every record detail page (Contact, Company, Deal, Ticket) follows the same 3-column layout:

| Column | Width | Content |
|---|---|---|
| **Left Sidebar** | ~280px | Avatar/icon, name, lifecycle stage, owner, all properties (collapsible groups), custom properties |
| **Center** | Flex | Tab bar (Activity, Emails, Calls, Tasks, Notes, Meetings) + reverse-chronological timeline + activity composer at top |
| **Right Sidebar** | ~300px | Association cards: Companies, Deals, Tickets, Attachments, Playbooks. Each card shows summary + "View all" link |

### Custom Properties System

HubSpot allows users to define custom properties on any object (Contact, Company, Deal, Ticket). Each property has:
- `name`, `label`, `groupName`
- `fieldType`: TEXT, NUMBER, DATE, DATETIME, SELECT, MULTI_SELECT, CHECKBOX, RADIO, TEXTAREA, PHONE, EMAIL, URL
- `options[]` for SELECT/MULTI_SELECT/RADIO
- `isRequired`, `isVisible`, `sortOrder`
- Property values stored in a separate `PropertyValue` table (EAV pattern)

---

## Complete Database Schema (53 Models)

Full Prisma schema at `projects/geek-crm-backend/prisma/schema.prisma`.

### Enums

```prisma
// ─── CORE ENUMS ──────────────────────────────────────────────────────────────
enum UserRole { SUPER_ADMIN  ADMIN  SALES  SERVICE  MARKETING  READONLY }
enum LifecycleStage { SUBSCRIBER  LEAD  MQL  SQL  OPPORTUNITY  CUSTOMER  EVANGELIST  OTHER }
enum LeadStatus { NEW  OPEN  IN_PROGRESS  OPEN_DEAL  UNQUALIFIED  ATTEMPTED  CONNECTED  BAD_TIMING }

// ─── ACTIVITY ENUMS ──────────────────────────────────────────────────────────
enum ActivityType { CALL  EMAIL  MEETING  NOTE  TASK  LINKEDIN_MESSAGE  WHATSAPP  POSTAL_MAIL  SMS  OTHER }
enum CallOutcome { CONNECTED  BUSY  NO_ANSWER  LEFT_VOICEMAIL  WRONG_NUMBER }
enum CallDirection { INBOUND  OUTBOUND }
enum MeetingOutcome { SCHEDULED  COMPLETED  RESCHEDULED  NO_SHOW  CANCELLED }
enum EmailDirection { SENT  RECEIVED }
enum TaskType { TODO  CALL  EMAIL  FOLLOW_UP }
enum TaskPriority { LOW  MEDIUM  HIGH }
enum TaskStatus { NOT_STARTED  IN_PROGRESS  WAITING  COMPLETED  DEFERRED }

// ─── PIPELINE ENUMS ──────────────────────────────────────────────────────────
enum DealStatus { OPEN  WON  LOST }
enum TicketPriority { LOW  MEDIUM  HIGH  URGENT }
enum TicketStatus { NEW  WAITING  IN_PROGRESS  CLOSED }
enum TicketSource { EMAIL  CHAT  PHONE  FORM  OTHER }
enum TicketPipelineType { SUPPORT  BUGS  FEATURE_REQUESTS }

// ─── COMMERCE ENUMS ──────────────────────────────────────────────────────────
enum QuoteStatus { DRAFT  PENDING_APPROVAL  APPROVED  SENT  SIGNED  PAID  LOST  EXPIRED }
enum InvoiceStatus { DRAFT  SENT  VIEWED  PARTIAL  PAID  OVERDUE  VOID }
enum PaymentMethod { CASH  CHECK  CREDIT_CARD  ACH  WIRE  ZELLE  VENMO  PAYPAL  OTHER }
enum BillingFrequency { ONE_TIME  MONTHLY  QUARTERLY  ANNUALLY }

// ─── MARKETING ENUMS ─────────────────────────────────────────────────────────
enum ListType { ACTIVE  STATIC }
enum EmailCampaignStatus { DRAFT  SCHEDULED  SENDING  SENT  CANCELLED }
enum FormFieldType { TEXT  EMAIL  PHONE  NUMBER  DATE  SELECT  MULTI_SELECT  CHECKBOX  TEXTAREA  HIDDEN }
enum FormStatus { DRAFT  PUBLISHED  ARCHIVED }

// ─── AUTOMATION ENUMS ────────────────────────────────────────────────────────
enum WorkflowStatus { ACTIVE  INACTIVE  DRAFT }
enum WorkflowTriggerType { FORM_SUBMISSION  LIFECYCLE_CHANGE  DEAL_STAGE_CHANGE  PROPERTY_CHANGE  DATE_BASED  MANUAL }
enum WorkflowActionType { SEND_EMAIL  CREATE_TASK  UPDATE_PROPERTY  ADD_TO_LIST  REMOVE_FROM_LIST  SEND_NOTIFICATION  DELAY  IF_BRANCH  ENROLL_IN_SEQUENCE }
enum SequenceStatus { ACTIVE  PAUSED  ARCHIVED }
enum SequenceStepType { AUTOMATED_EMAIL  MANUAL_EMAIL  CALL_TASK  GENERAL_TASK  LINKEDIN_TASK  DELAY }
enum EnrollmentStatus { ACTIVE  PAUSED  COMPLETED  UNENROLLED  BOUNCED  REPLIED }

// ─── SERVICE ENUMS ───────────────────────────────────────────────────────────
enum KBArticleStatus { DRAFT  PUBLISHED  ARCHIVED }
enum ChatflowType { LIVE_CHAT  BOT }
enum FeedbackType { CES  CSAT  NPS  CUSTOM }
enum FeedbackStatus { DRAFT  PUBLISHED  CLOSED }

// ─── PROPERTY ENUMS ──────────────────────────────────────────────────────────
enum ObjectType { CONTACT  COMPANY  DEAL  TICKET }
enum PropertyFieldType { TEXT  NUMBER  DATE  DATETIME  SELECT  MULTI_SELECT  CHECKBOX  RADIO  TEXTAREA  PHONE_NUMBER  EMAIL  URL  CURRENCY }

// ─── REPORTING ENUMS ─────────────────────────────────────────────────────────
enum ReportType { CONTACTS  DEALS  TICKETS  ACTIVITIES  REVENUE  CUSTOM }
enum ChartType { BAR  LINE  PIE  DONUT  TABLE  NUMBER  FUNNEL  AREA }
enum GoalMetric { REVENUE  DEALS_CLOSED  CALLS_MADE  MEETINGS_BOOKED  EMAILS_SENT  TICKETS_CLOSED }
enum GoalPeriod { WEEKLY  MONTHLY  QUARTERLY  YEARLY }

// ─── TAG / UI ENUMS ──────────────────────────────────────────────────────────
enum TagColor { RED  ORANGE  YELLOW  GREEN  TEAL  BLUE  INDIGO  PURPLE  PINK  GRAY }
```

### Core Models

```prisma
// ═══════════════════════════════════════════════════════════════════════════════
// AUTHENTICATION & USERS
// ═══════════════════════════════════════════════════════════════════════════════

model User {
  id                   String    @id @default(cuid())
  email                String    @unique
  passwordHash         String
  firstName            String
  lastName             String
  displayName          String?
  avatar               String?
  role                 UserRole  @default(SALES)
  isActive             Boolean   @default(true)
  lastLoginAt          DateTime?
  failedLoginAttempts  Int       @default(0)
  lockedUntil          DateTime?
  passwordResetToken   String?
  passwordResetExpires DateTime?
  calendarLink         String?   // Personal meeting link
  emailSignature       String?   @db.Text
  refreshTokens        RefreshToken[]
  activities           Activity[]      @relation("CreatedActivities")
  tasksCreated         Task[]          @relation("CreatedTasks")
  tasksAssigned        Task[]          @relation("AssignedTasks")
  dealsOwned           Deal[]          @relation("DealOwner")
  contactsOwned        Contact[]       @relation("ContactOwner")
  companiesOwned       Company[]       @relation("CompanyOwner")
  ticketsOwned         Ticket[]        @relation("TicketOwner")
  sequencesCreated     Sequence[]
  meetingLinks         MeetingLink[]
  enrollmentsSender    SequenceEnrollment[] @relation("EnrollmentSender")
  goals                Goal[]
  createdAt            DateTime  @default(now())
  updatedAt            DateTime  @updatedAt
}

model RefreshToken {
  id        String   @id @default(cuid())
  userId    String
  token     String   @unique
  expiresAt DateTime
  userAgent String?
  ipAddress String?
  isRevoked Boolean  @default(false)
  revokedAt DateTime?
  createdAt DateTime @default(now())
  user      User     @relation(fields: [userId], references: [id], onDelete: Cascade)
}

// ═══════════════════════════════════════════════════════════════════════════════
// CRM CORE — CONTACTS
// ═══════════════════════════════════════════════════════════════════════════════

model Contact {
  id             String         @id @default(cuid())
  firstName      String?
  lastName       String?
  displayName    String         // Computed: "First Last"
  email          String?
  phone          String?
  jobTitle       String?
  lifecycleStage LifecycleStage @default(LEAD)
  leadStatus     LeadStatus?
  ownerId        String?
  source         String?
  avatarUrl      String?
  dateOfBirth    DateTime?      @db.Date
  addresses      ContactAddress[]
  phones         ContactPhone[]
  emails         ContactEmail[]
  tags           ContactTag[]
  owner          User?          @relation("ContactOwner", fields: [ownerId], references: [id])
  companies      ContactCompany[]
  deals          ContactDeal[]
  tickets        ContactTicket[]
  activities     Activity[]
  tasks          Task[]         @relation("ContactTasks")
  quotes         Quote[]
  invoices       Invoice[]
  propertyValues PropertyValue[] @relation("ContactProperties")
  listMemberships ListMembership[]
  sequenceEnrollments SequenceEnrollment[]
  formSubmissions FormSubmission[]
  feedbackResponses FeedbackResponse[]
  notes          String?        @db.Text
  isActive       Boolean        @default(true)
  createdAt      DateTime       @default(now())
  updatedAt      DateTime       @updatedAt
}

model ContactPhone {
  id        String  @id @default(cuid())
  contactId String
  label     String  @default("Mobile") // Mobile, Work, Home, Fax, Other
  number    String
  isPrimary Boolean @default(false)
  contact   Contact @relation(fields: [contactId], references: [id], onDelete: Cascade)
}

model ContactEmail {
  id        String  @id @default(cuid())
  contactId String
  label     String  @default("Work") // Work, Personal, Other
  address   String
  isPrimary Boolean @default(false)
  contact   Contact @relation(fields: [contactId], references: [id], onDelete: Cascade)
}

model ContactAddress {
  id        String  @id @default(cuid())
  contactId String
  label     String  @default("Work") // Work, Home, Billing, Shipping
  street    String?
  street2   String?
  city      String?
  state     String?
  zip       String?
  country   String? @default("US")
  isPrimary Boolean @default(false)
  contact   Contact @relation(fields: [contactId], references: [id], onDelete: Cascade)
}

model Tag {
  id       String     @id @default(cuid())
  name     String     @unique
  color    TagColor   @default(BLUE)
  contacts ContactTag[]
}

model ContactTag {
  contactId String
  tagId     String
  contact   Contact @relation(fields: [contactId], references: [id], onDelete: Cascade)
  tag       Tag     @relation(fields: [tagId], references: [id], onDelete: Cascade)
  @@id([contactId, tagId])
}

// ═══════════════════════════════════════════════════════════════════════════════
// CRM CORE — COMPANIES
// ═══════════════════════════════════════════════════════════════════════════════

model Company {
  id             String    @id @default(cuid())
  name           String
  domain         String?
  industry       String?
  description    String?   @db.Text
  phone          String?
  website        String?
  annualRevenue  Decimal?  @db.Decimal(14, 2)
  employeeCount  Int?
  lifecycleStage LifecycleStage @default(LEAD)
  ownerId        String?
  logoUrl        String?
  street         String?
  city           String?
  state          String?
  zip            String?
  country        String?   @default("US")
  owner          User?     @relation("CompanyOwner", fields: [ownerId], references: [id])
  contacts       ContactCompany[]
  deals          CompanyDeal[]
  tickets        CompanyTicket[]
  activities     Activity[]  @relation("CompanyActivities")
  propertyValues PropertyValue[] @relation("CompanyProperties")
  isActive       Boolean   @default(true)
  createdAt      DateTime  @default(now())
  updatedAt      DateTime  @updatedAt
}

// ═══════════════════════════════════════════════════════════════════════════════
// ASSOCIATIONS (Many-to-Many Junction Tables — HubSpot Pattern)
// ═══════════════════════════════════════════════════════════════════════════════

model ContactCompany {
  contactId String
  companyId String
  isPrimary Boolean @default(false)
  contact   Contact @relation(fields: [contactId], references: [id], onDelete: Cascade)
  company   Company @relation(fields: [companyId], references: [id], onDelete: Cascade)
  @@id([contactId, companyId])
}

model ContactDeal {
  contactId String
  dealId    String
  contact   Contact @relation(fields: [contactId], references: [id], onDelete: Cascade)
  deal      Deal    @relation(fields: [dealId], references: [id], onDelete: Cascade)
  @@id([contactId, dealId])
}

model ContactTicket {
  contactId String
  ticketId  String
  contact   Contact @relation(fields: [contactId], references: [id], onDelete: Cascade)
  ticket    Ticket  @relation(fields: [ticketId], references: [id], onDelete: Cascade)
  @@id([contactId, ticketId])
}

model CompanyDeal {
  companyId String
  dealId    String
  isPrimary Boolean @default(false)
  company   Company @relation(fields: [companyId], references: [id], onDelete: Cascade)
  deal      Deal    @relation(fields: [dealId], references: [id], onDelete: Cascade)
  @@id([companyId, dealId])
}

model CompanyTicket {
  companyId String
  ticketId  String
  company   Company @relation(fields: [companyId], references: [id], onDelete: Cascade)
  ticket    Ticket  @relation(fields: [ticketId], references: [id], onDelete: Cascade)
  @@id([companyId, ticketId])
}

model DealTicket {
  dealId   String
  ticketId String
  deal     Deal   @relation(fields: [dealId], references: [id], onDelete: Cascade)
  ticket   Ticket @relation(fields: [ticketId], references: [id], onDelete: Cascade)
  @@id([dealId, ticketId])
}

// ═══════════════════════════════════════════════════════════════════════════════
// CRM CORE — ACTIVITIES
// ═══════════════════════════════════════════════════════════════════════════════

model Activity {
  id              String        @id @default(cuid())
  type            ActivityType
  subject         String
  body            String?       @db.Text
  bodyHtml        String?       @db.Text
  contactId       String?
  companyId       String?
  dealId          String?
  ticketId        String?
  createdById     String
  // Call-specific
  callDirection   CallDirection?
  callDuration    Int?          // seconds
  callOutcome     CallOutcome?
  callRecordingUrl String?
  // Email-specific
  emailDirection  EmailDirection?
  emailTo         String?
  emailFrom       String?
  emailCc         String?
  emailBcc        String?
  emailThreadId   String?
  // Meeting-specific
  meetingOutcome  MeetingOutcome?
  meetingStartTime DateTime?
  meetingEndTime   DateTime?
  meetingLocation  String?
  // General
  outcome         String?
  scheduledAt     DateTime?
  completedAt     DateTime?
  contact         Contact?    @relation(fields: [contactId], references: [id])
  company         Company?    @relation("CompanyActivities", fields: [companyId], references: [id])
  deal            Deal?       @relation(fields: [dealId], references: [id])
  ticket          Ticket?     @relation(fields: [ticketId], references: [id])
  createdBy       User        @relation("CreatedActivities", fields: [createdById], references: [id])
  attachments     Attachment[]
  createdAt       DateTime    @default(now())
}

model Attachment {
  id         String   @id @default(cuid())
  activityId String?
  fileName   String
  fileUrl    String
  fileSize   Int
  mimeType   String
  activity   Activity? @relation(fields: [activityId], references: [id])
  createdAt  DateTime @default(now())
}

// ═══════════════════════════════════════════════════════════════════════════════
// CRM CORE — TASKS
// ═══════════════════════════════════════════════════════════════════════════════

model Task {
  id            String       @id @default(cuid())
  title         String
  description   String?      @db.Text
  type          TaskType     @default(TODO)
  status        TaskStatus   @default(NOT_STARTED)
  priority      TaskPriority @default(MEDIUM)
  dueDate       DateTime?
  reminderAt    DateTime?
  contactId     String?
  dealId        String?
  ticketId      String?
  createdById   String
  assignedToId  String?
  queueId       String?
  completedAt   DateTime?
  contact       Contact?    @relation("ContactTasks", fields: [contactId], references: [id])
  deal          Deal?       @relation("DealTasks", fields: [dealId], references: [id])
  ticket        Ticket?     @relation("TicketTasks", fields: [ticketId], references: [id])
  createdBy     User        @relation("CreatedTasks", fields: [createdById], references: [id])
  assignedTo    User?       @relation("AssignedTasks", fields: [assignedToId], references: [id])
  taskQueue     TaskQueue?  @relation(fields: [queueId], references: [id])
  createdAt     DateTime    @default(now())
  updatedAt     DateTime    @updatedAt
}

model TaskQueue {
  id        String   @id @default(cuid())
  name      String
  sortOrder Int      @default(0)
  tasks     Task[]
  createdAt DateTime @default(now())
}

// ═══════════════════════════════════════════════════════════════════════════════
// SALES — DEALS & PIPELINES
// ═══════════════════════════════════════════════════════════════════════════════

model Pipeline {
  id          String          @id @default(cuid())
  name        String
  objectType  String          @default("DEAL") // DEAL or TICKET
  description String?
  sortOrder   Int             @default(0)
  isDefault   Boolean         @default(false)
  isActive    Boolean         @default(true)
  stages      PipelineStage[]
  deals       Deal[]
  tickets     Ticket[]
  createdAt   DateTime        @default(now())
  updatedAt   DateTime        @updatedAt
}

model PipelineStage {
  id          String   @id @default(cuid())
  pipelineId  String
  name        String
  sortOrder   Int      @default(0)
  probability Int      @default(0) // 0-100%
  color       String?
  rottenDays  Int?     // Days before "stale" warning
  isClosed    Boolean  @default(false)  // Won/Lost/Closed stage
  isWon       Boolean  @default(false)
  pipeline    Pipeline @relation(fields: [pipelineId], references: [id], onDelete: Cascade)
  deals       Deal[]
  tickets     Ticket[]
}

model Deal {
  id             String         @id @default(cuid())
  title          String
  value          Decimal?       @db.Decimal(14, 2)
  status         DealStatus     @default(OPEN)
  pipelineId     String
  stageId        String
  ownerId        String?
  expectedClose  DateTime?      @db.Date
  actualClose    DateTime?      @db.Date
  lostReason     String?
  wonReason      String?
  notes          String?        @db.Text
  priority       String?        // High, Medium, Low
  dealType       String?        // New Business, Existing Business
  pipeline       Pipeline       @relation(fields: [pipelineId], references: [id])
  stage          PipelineStage  @relation(fields: [stageId], references: [id])
  owner          User?          @relation("DealOwner", fields: [ownerId], references: [id])
  contacts       ContactDeal[]
  companies      CompanyDeal[]
  tickets        DealTicket[]
  activities     Activity[]
  tasks          Task[]         @relation("DealTasks")
  quotes         Quote[]
  lineItems      DealLineItem[]
  propertyValues PropertyValue[] @relation("DealProperties")
  createdAt      DateTime       @default(now())
  updatedAt      DateTime       @updatedAt
}

model DealLineItem {
  id          String  @id @default(cuid())
  dealId      String
  productId   String?
  name        String
  quantity    Decimal @db.Decimal(10, 2) @default(1)
  unitPrice   Decimal @db.Decimal(12, 2) @default(0)
  discount    Decimal @db.Decimal(12, 2) @default(0)
  total       Decimal @db.Decimal(12, 2) @default(0)
  billingFrequency BillingFrequency @default(ONE_TIME)
  sortOrder   Int     @default(0)
  deal        Deal    @relation(fields: [dealId], references: [id], onDelete: Cascade)
  product     Product? @relation(fields: [productId], references: [id])
}

// ═══════════════════════════════════════════════════════════════════════════════
// SERVICE — TICKETS
// ═══════════════════════════════════════════════════════════════════════════════

model Ticket {
  id           String         @id @default(cuid())
  subject      String
  description  String?        @db.Text
  status       TicketStatus   @default(NEW)
  priority     TicketPriority @default(MEDIUM)
  source       TicketSource   @default(EMAIL)
  pipelineId   String
  stageId      String
  ownerId      String?
  category     String?
  resolvedAt   DateTime?
  closedAt     DateTime?
  slaDeadline  DateTime?
  pipeline     Pipeline       @relation(fields: [pipelineId], references: [id])
  stage        PipelineStage  @relation(fields: [stageId], references: [id])
  owner        User?          @relation("TicketOwner", fields: [ownerId], references: [id])
  contacts     ContactTicket[]
  companies    CompanyTicket[]
  deals        DealTicket[]
  activities   Activity[]
  tasks        Task[]         @relation("TicketTasks")
  propertyValues PropertyValue[] @relation("TicketProperties")
  createdAt    DateTime       @default(now())
  updatedAt    DateTime       @updatedAt
}

// ═══════════════════════════════════════════════════════════════════════════════
// COMMERCE — PRODUCTS, QUOTES, INVOICES, PAYMENTS
// ═══════════════════════════════════════════════════════════════════════════════

model Product {
  id               String   @id @default(cuid())
  name             String
  description      String?  @db.Text
  sku              String?
  unitPrice        Decimal  @db.Decimal(12, 2) @default(0)
  recurringPrice   Decimal? @db.Decimal(12, 2)
  billingFrequency BillingFrequency @default(ONE_TIME)
  unit             String   @default("each")
  taxable          Boolean  @default(true)
  isActive         Boolean  @default(true)
  sortOrder        Int      @default(0)
  dealLineItems    DealLineItem[]
  quoteLineItems   QuoteLineItem[]
  invoiceLineItems InvoiceLineItem[]
  createdAt        DateTime @default(now())
  updatedAt        DateTime @updatedAt
}

model Quote {
  id               String      @id @default(cuid())
  number           String      @unique // GQ-2026-001
  title            String
  status           QuoteStatus @default(DRAFT)
  contactId        String
  dealId           String?
  expiresAt        DateTime?
  subtotal         Decimal     @db.Decimal(12, 2) @default(0)
  taxRate          Decimal     @db.Decimal(5, 4) @default(0)
  taxAmount        Decimal     @db.Decimal(12, 2) @default(0)
  discountPercent  Decimal     @db.Decimal(5, 2) @default(0)
  discountAmount   Decimal     @db.Decimal(12, 2) @default(0)
  total            Decimal     @db.Decimal(12, 2) @default(0)
  notes            String?     @db.Text
  terms            String?     @db.Text
  senderCompany    String?
  senderName       String?
  senderEmail      String?
  senderPhone      String?
  recipientCompany String?
  recipientName    String?
  recipientEmail   String?
  sentAt           DateTime?
  viewedAt         DateTime?
  signedAt         DateTime?
  signatureData    String?     @db.Text
  counterSignedAt  DateTime?
  convertedToInvoiceId String?
  lineItems        QuoteLineItem[]
  contact          Contact     @relation(fields: [contactId], references: [id])
  deal             Deal?       @relation(fields: [dealId], references: [id])
  createdAt        DateTime    @default(now())
  updatedAt        DateTime    @updatedAt
}

model QuoteLineItem {
  id               String  @id @default(cuid())
  quoteId          String
  productId        String?
  name             String
  description      String?
  quantity         Decimal @db.Decimal(10, 2) @default(1)
  unitPrice        Decimal @db.Decimal(12, 2) @default(0)
  discount         Decimal @db.Decimal(12, 2) @default(0)
  total            Decimal @db.Decimal(12, 2) @default(0)
  billingFrequency BillingFrequency @default(ONE_TIME)
  sortOrder        Int     @default(0)
  quote            Quote   @relation(fields: [quoteId], references: [id], onDelete: Cascade)
  product          Product? @relation(fields: [productId], references: [id])
}

model Invoice {
  id             String        @id @default(cuid())
  number         String        @unique // GI-2026-001
  title          String
  status         InvoiceStatus @default(DRAFT)
  contactId      String
  quoteId        String?
  dueDate        DateTime?     @db.Date
  subtotal       Decimal       @db.Decimal(12, 2) @default(0)
  taxRate        Decimal       @db.Decimal(5, 4) @default(0)
  taxAmount      Decimal       @db.Decimal(12, 2) @default(0)
  discountAmount Decimal       @db.Decimal(12, 2) @default(0)
  total          Decimal       @db.Decimal(12, 2) @default(0)
  amountPaid     Decimal       @db.Decimal(12, 2) @default(0)
  notes          String?       @db.Text
  terms          String?       @db.Text
  sentAt         DateTime?
  viewedAt       DateTime?
  paidAt         DateTime?
  voidedAt       DateTime?
  lineItems      InvoiceLineItem[]
  payments       Payment[]
  contact        Contact       @relation(fields: [contactId], references: [id])
  createdAt      DateTime      @default(now())
  updatedAt      DateTime      @updatedAt
}

model InvoiceLineItem {
  id          String  @id @default(cuid())
  invoiceId   String
  productId   String?
  name        String
  description String?
  quantity    Decimal @db.Decimal(10, 2) @default(1)
  unitPrice   Decimal @db.Decimal(12, 2) @default(0)
  discount    Decimal @db.Decimal(12, 2) @default(0)
  total       Decimal @db.Decimal(12, 2) @default(0)
  sortOrder   Int     @default(0)
  invoice     Invoice @relation(fields: [invoiceId], references: [id], onDelete: Cascade)
  product     Product? @relation(fields: [productId], references: [id])
}

model Payment {
  id        String        @id @default(cuid())
  invoiceId String
  amount    Decimal       @db.Decimal(12, 2)
  method    PaymentMethod @default(OTHER)
  reference String?
  paidAt    DateTime
  notes     String?
  invoice   Invoice       @relation(fields: [invoiceId], references: [id])
  createdAt DateTime      @default(now())
}

// ═══════════════════════════════════════════════════════════════════════════════
// MARKETING — LISTS, EMAIL CAMPAIGNS, FORMS
// ═══════════════════════════════════════════════════════════════════════════════

model List {
  id          String   @id @default(cuid())
  name        String
  listType    ListType @default(STATIC)
  description String?
  filters     Json?    // Active list: JSON filter criteria
  memberCount Int      @default(0)
  members     ListMembership[]
  createdAt   DateTime @default(now())
  updatedAt   DateTime @updatedAt
}

model ListMembership {
  listId    String
  contactId String
  addedAt   DateTime @default(now())
  list      List     @relation(fields: [listId], references: [id], onDelete: Cascade)
  contact   Contact  @relation(fields: [contactId], references: [id], onDelete: Cascade)
  @@id([listId, contactId])
}

model EmailTemplate {
  id        String   @id @default(cuid())
  name      String
  subject   String
  bodyHtml  String   @db.Text
  bodyText  String?  @db.Text
  category  String?
  isShared  Boolean  @default(true)
  campaigns EmailCampaign[]
  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt
}

model EmailCampaign {
  id          String              @id @default(cuid())
  name        String
  subject     String
  status      EmailCampaignStatus @default(DRAFT)
  templateId  String?
  listId      String?
  scheduledAt DateTime?
  sentAt      DateTime?
  totalSent   Int       @default(0)
  totalOpened Int       @default(0)
  totalClicked Int      @default(0)
  totalBounced Int      @default(0)
  template    EmailTemplate? @relation(fields: [templateId], references: [id])
  createdAt   DateTime @default(now())
  updatedAt   DateTime @updatedAt
}

model Form {
  id          String     @id @default(cuid())
  name        String
  status      FormStatus @default(DRAFT)
  description String?
  submitText  String     @default("Submit")
  thankYouMessage String?
  redirectUrl String?
  notifyEmails String?
  fields      FormField[]
  submissions FormSubmission[]
  createdAt   DateTime   @default(now())
  updatedAt   DateTime   @updatedAt
}

model FormField {
  id           String        @id @default(cuid())
  formId       String
  label        String
  name         String
  fieldType    FormFieldType @default(TEXT)
  placeholder  String?
  helpText     String?
  isRequired   Boolean       @default(false)
  options      Json?         // For SELECT, MULTI_SELECT, CHECKBOX
  defaultValue String?
  sortOrder    Int           @default(0)
  form         Form          @relation(fields: [formId], references: [id], onDelete: Cascade)
}

model FormSubmission {
  id        String   @id @default(cuid())
  formId    String
  contactId String?
  data      Json     // { fieldName: value, ... }
  pageUrl   String?
  ipAddress String?
  form      Form     @relation(fields: [formId], references: [id])
  contact   Contact? @relation(fields: [contactId], references: [id])
  createdAt DateTime @default(now())
}

// ═══════════════════════════════════════════════════════════════════════════════
// AUTOMATIONS — WORKFLOWS & SEQUENCES
// ═══════════════════════════════════════════════════════════════════════════════

model Workflow {
  id           String         @id @default(cuid())
  name         String
  description  String?
  status       WorkflowStatus @default(DRAFT)
  triggerType  WorkflowTriggerType
  triggerConfig Json?         // Trigger criteria
  objectType   ObjectType     @default(CONTACT)
  steps        WorkflowStep[]
  enrolledCount Int           @default(0)
  createdAt    DateTime       @default(now())
  updatedAt    DateTime       @updatedAt
}

model WorkflowStep {
  id         String             @id @default(cuid())
  workflowId String
  actionType WorkflowActionType
  config     Json               // Action parameters
  sortOrder  Int                @default(0)
  delayMs    Int?               // Delay before execution
  parentId   String?            // For branching
  branchName String?            // "yes" / "no" for IF_BRANCH
  workflow   Workflow           @relation(fields: [workflowId], references: [id], onDelete: Cascade)
}

model Sequence {
  id          String         @id @default(cuid())
  name        String
  description String?
  status      SequenceStatus @default(ACTIVE)
  createdById String
  steps       SequenceStep[]
  enrollments SequenceEnrollment[]
  createdBy   User           @relation(fields: [createdById], references: [id])
  createdAt   DateTime       @default(now())
  updatedAt   DateTime       @updatedAt
}

model SequenceStep {
  id         String           @id @default(cuid())
  sequenceId String
  stepType   SequenceStepType
  subject    String?
  bodyHtml   String?          @db.Text
  delayDays  Int              @default(1)
  sortOrder  Int              @default(0)
  sequence   Sequence         @relation(fields: [sequenceId], references: [id], onDelete: Cascade)
}

model SequenceEnrollment {
  id          String           @id @default(cuid())
  sequenceId  String
  contactId   String
  senderId    String
  status      EnrollmentStatus @default(ACTIVE)
  currentStep Int              @default(0)
  enrolledAt  DateTime         @default(now())
  pausedAt    DateTime?
  completedAt DateTime?
  unenrolledAt DateTime?
  nextStepAt  DateTime?
  sequence    Sequence         @relation(fields: [sequenceId], references: [id])
  contact     Contact          @relation(fields: [contactId], references: [id])
  sender      User             @relation("EnrollmentSender", fields: [senderId], references: [id])
}

// ═══════════════════════════════════════════════════════════════════════════════
// SALES — MEETINGS
// ═══════════════════════════════════════════════════════════════════════════════

model MeetingLink {
  id           String   @id @default(cuid())
  userId       String
  title        String
  slug         String   @unique  // geek-crm.com/meetings/jeff-30min
  duration     Int      @default(30) // minutes
  description  String?
  availability Json?    // { dayOfWeek: [startTime, endTime] }
  bufferBefore Int      @default(0)  // minutes
  bufferAfter  Int      @default(0)
  maxPerDay    Int?
  isActive     Boolean  @default(true)
  bookings     MeetingBooking[]
  user         User     @relation(fields: [userId], references: [id])
  createdAt    DateTime @default(now())
  updatedAt    DateTime @updatedAt
}

model MeetingBooking {
  id            String   @id @default(cuid())
  meetingLinkId String
  contactName   String
  contactEmail  String
  startTime     DateTime
  endTime       DateTime
  notes         String?
  isCancelled   Boolean  @default(false)
  meetingLink   MeetingLink @relation(fields: [meetingLinkId], references: [id])
  createdAt     DateTime @default(now())
}

// ═══════════════════════════════════════════════════════════════════════════════
// SERVICE — KNOWLEDGE BASE, CHATFLOWS, FEEDBACK
// ═══════════════════════════════════════════════════════════════════════════════

model KBCategory {
  id          String      @id @default(cuid())
  name        String
  slug        String      @unique
  description String?
  sortOrder   Int         @default(0)
  parentId    String?
  parent      KBCategory? @relation("CategoryParent", fields: [parentId], references: [id])
  children    KBCategory[] @relation("CategoryParent")
  articles    KBArticle[]
  createdAt   DateTime    @default(now())
}

model KBArticle {
  id          String          @id @default(cuid())
  title       String
  slug        String          @unique
  bodyHtml    String          @db.Text
  status      KBArticleStatus @default(DRAFT)
  categoryId  String?
  viewCount   Int             @default(0)
  helpfulYes  Int             @default(0)
  helpfulNo   Int             @default(0)
  category    KBCategory?     @relation(fields: [categoryId], references: [id])
  createdAt   DateTime        @default(now())
  updatedAt   DateTime        @updatedAt
}

model Chatflow {
  id          String       @id @default(cuid())
  name        String
  type        ChatflowType @default(LIVE_CHAT)
  welcomeMsg  String?
  offlineMsg  String?
  config      Json?        // Bot steps, routing rules
  isActive    Boolean      @default(true)
  createdAt   DateTime     @default(now())
  updatedAt   DateTime     @updatedAt
}

model FeedbackSurvey {
  id          String         @id @default(cuid())
  name        String
  type        FeedbackType   @default(CSAT)
  status      FeedbackStatus @default(DRAFT)
  question    String
  thankYouMsg String?
  responses   FeedbackResponse[]
  createdAt   DateTime       @default(now())
  updatedAt   DateTime       @updatedAt
}

model FeedbackResponse {
  id        String   @id @default(cuid())
  surveyId  String
  contactId String?
  score     Int      // NPS: 0-10, CSAT: 1-5, CES: 1-7
  comment   String?  @db.Text
  survey    FeedbackSurvey @relation(fields: [surveyId], references: [id])
  contact   Contact?       @relation(fields: [contactId], references: [id])
  createdAt DateTime @default(now())
}

// ═══════════════════════════════════════════════════════════════════════════════
// CUSTOM PROPERTIES (EAV Pattern)
// ═══════════════════════════════════════════════════════════════════════════════

model PropertyDefinition {
  id          String            @id @default(cuid())
  objectType  ObjectType
  groupName   String            @default("Custom Properties")
  name        String            // Internal name (snake_case)
  label       String            // Display label
  fieldType   PropertyFieldType @default(TEXT)
  description String?
  options     Json?             // For SELECT, MULTI_SELECT, RADIO: [{label, value}]
  isRequired  Boolean           @default(false)
  isVisible   Boolean           @default(true)
  sortOrder   Int               @default(0)
  values      PropertyValue[]
  createdAt   DateTime          @default(now())
  updatedAt   DateTime          @updatedAt

  @@unique([objectType, name])
}

model PropertyValue {
  id           String             @id @default(cuid())
  definitionId String
  contactId    String?
  companyId    String?
  dealId       String?
  ticketId     String?
  value        String             @db.Text
  definition   PropertyDefinition @relation(fields: [definitionId], references: [id], onDelete: Cascade)
  contact      Contact?           @relation("ContactProperties", fields: [contactId], references: [id], onDelete: Cascade)
  company      Company?           @relation("CompanyProperties", fields: [companyId], references: [id], onDelete: Cascade)
  deal         Deal?              @relation("DealProperties", fields: [dealId], references: [id], onDelete: Cascade)
  ticket       Ticket?            @relation("TicketProperties", fields: [ticketId], references: [id], onDelete: Cascade)
}

// ═══════════════════════════════════════════════════════════════════════════════
// REPORTING — DASHBOARDS, REPORTS, GOALS
// ═══════════════════════════════════════════════════════════════════════════════

model Dashboard {
  id          String           @id @default(cuid())
  name        String
  description String?
  isDefault   Boolean          @default(false)
  isShared    Boolean          @default(true)
  widgets     DashboardWidget[]
  createdAt   DateTime         @default(now())
  updatedAt   DateTime         @updatedAt
}

model DashboardWidget {
  id          String    @id @default(cuid())
  dashboardId String
  reportId    String?
  title       String
  chartType   ChartType @default(NUMBER)
  config      Json?     // Chart config, dimensions
  width       Int       @default(6) // Grid columns (1-12)
  height      Int       @default(4) // Grid rows
  positionX   Int       @default(0)
  positionY   Int       @default(0)
  dashboard   Dashboard @relation(fields: [dashboardId], references: [id], onDelete: Cascade)
  report      Report?   @relation(fields: [reportId], references: [id])
}

model Report {
  id          String     @id @default(cuid())
  name        String
  description String?
  type        ReportType @default(CUSTOM)
  chartType   ChartType  @default(TABLE)
  config      Json       // Dimensions, measures, filters, groupBy
  isShared    Boolean    @default(true)
  widgets     DashboardWidget[]
  createdAt   DateTime   @default(now())
  updatedAt   DateTime   @updatedAt
}

model Goal {
  id          String     @id @default(cuid())
  name        String
  metric      GoalMetric
  targetValue Decimal    @db.Decimal(14, 2)
  currentValue Decimal   @db.Decimal(14, 2) @default(0)
  period      GoalPeriod @default(MONTHLY)
  startDate   DateTime   @db.Date
  endDate     DateTime   @db.Date
  userId      String?
  pipelineId  String?
  user        User?      @relation(fields: [userId], references: [id])
  createdAt   DateTime   @default(now())
  updatedAt   DateTime   @updatedAt
}

// ═══════════════════════════════════════════════════════════════════════════════
// SETTINGS
// ═══════════════════════════════════════════════════════════════════════════════

model AccountSettings {
  id           String   @id @default(cuid())
  companyName  String
  companyLogo  String?
  timezone     String   @default("America/New_York")
  dateFormat   String   @default("MM/DD/YYYY")
  currency     String   @default("USD")
  fiscalYearStart Int   @default(1) // Month 1-12
  defaultPipelineId String?
  defaultTicketPipelineId String?
  createdAt    DateTime @default(now())
  updatedAt    DateTime @updatedAt
}
```

---

## Target Directory Structure

```
projects/
  geek-crm-library/
    src/
      public-api.ts
      lib/
        environments/
          environment.ts / environment.prod.ts
        core/
          models/index.ts                    # All TS interfaces
          services/
            api.service.ts                   # Base HttpClient service
            auth.service.ts
            contact.service.ts
            company.service.ts
            deal.service.ts
            pipeline.service.ts
            activity.service.ts
            task.service.ts
            ticket.service.ts
            quote.service.ts
            invoice.service.ts
            product.service.ts
            list.service.ts
            email-campaign.service.ts
            form.service.ts
            workflow.service.ts
            sequence.service.ts
            meeting.service.ts
            kb.service.ts
            chatflow.service.ts
            feedback.service.ts
            property.service.ts
            dashboard.service.ts
            report.service.ts
            goal.service.ts
            search.service.ts
            tag.service.ts
            settings.service.ts
          guards/auth.guard.ts
          interceptors/auth.interceptor.ts
        shared/
          components/
            sidebar/  navbar/  data-table/  loading/
            stats-card/  timeline/  badge/  confirm-dialog/
            kanban-board/  kanban-card/  line-item-editor/
            global-search/  property-editor/  property-display/
            association-card/  record-sidebar/  activity-composer/
            pagination/  empty-state/  avatar/  tag-selector/
            rich-text-editor/  date-picker/  file-upload/
          pipes/
            relative-time.pipe.ts
            currency.pipe.ts
        layout/layout.component.ts/.html/.scss
        components/
          auth/login/
          home/
          crm/
            contacts/list/ detail/ form/
            companies/list/ detail/ form/
            activities/
          sales/
            deals/board/ list/ detail/ form/
            tasks/list/ form/
            sequences/list/ detail/ form/
            meetings/list/ settings/
            quotes/list/ detail/ form/
          commerce/
            products/list/ form/
            invoices/list/ detail/ form/
            payments/
          service/
            tickets/board/ list/ detail/ form/
            knowledge-base/categories/ articles/list/ detail/ form/
            chatflows/list/ form/
            feedback/list/ detail/ form/
          marketing/
            lists/list/ detail/ form/
            email/campaigns/ templates/ editor/
            forms/list/ detail/ builder/
          automations/
            workflows/list/ detail/ builder/
            sequences/ (reuses sales/sequences)
          reporting/
            dashboards/list/ detail/ widget-editor/
            reports/list/ detail/ builder/
            goals/list/ form/
          settings/
            account/ users/ properties/ pipelines/ products/ tags/ integrations/
        geek-crm-app.component.ts/.html/.scss
        geek-crm-app.routes.ts

  geek-crm-elements/
    src/main.ts                              # createApplication + customElements.define

  geek-crm-backend/
    prisma/schema.prisma  seed.ts  migrations/
    src/
      server.ts
      config/database.ts  logger.ts  environment.ts
      middleware/error-handler.ts  auth.middleware.ts  validate.middleware.ts
      utils/errors.ts  response.ts  pagination.ts  document-number.ts
      routes/
        health.ts  auth.ts
        contacts.ts  companies.ts  activities.ts  tags.ts
        tasks.ts  pipelines.ts  deals.ts
        tickets.ts
        quotes.ts  invoices.ts  payments.ts  products.ts
        lists.ts  email-campaigns.ts  email-templates.ts  forms.ts
        workflows.ts  sequences.ts
        meetings.ts
        knowledge-base.ts  chatflows.ts  feedback.ts
        properties.ts
        dashboards.ts  reports.ts  goals.ts
        search.ts  settings.ts
```

---

## Phase 1: Foundation + CRM Core (Contacts, Companies, Activities)

**Goal:** Working CRM shell with auth, HubSpot-style navigation, 3-column record layout, contacts, companies, associations, activity timeline, and custom properties.

### Database (Phase 1 migration)
Tables: `User`, `RefreshToken`, `Contact`, `ContactPhone`, `ContactEmail`, `ContactAddress`, `Tag`, `ContactTag`, `Company`, `ContactCompany`, `Activity`, `Attachment`, `PropertyDefinition`, `PropertyValue`, `AccountSettings`

### API Endpoints (28 routes)

| Method | Path | Description |
|---|---|---|
| POST | `/auth/login` | JWT login |
| POST | `/auth/refresh` | Refresh access token |
| POST | `/auth/logout` | Revoke refresh token |
| GET | `/auth/me` | Current user |
| GET | `/health` | Health check |
| GET | `/contacts` | Paginated list with search/filter/sort |
| POST | `/contacts` | Create contact |
| GET | `/contacts/search` | Quick search (autocomplete) |
| GET | `/contacts/:id` | Full detail with associations |
| PATCH | `/contacts/:id` | Update |
| DELETE | `/contacts/:id` | Soft delete (isActive=false) |
| GET | `/contacts/:id/activities` | Activity timeline |
| POST | `/contacts/:id/activities` | Log activity |
| GET | `/companies` | Paginated list |
| POST | `/companies` | Create company |
| GET | `/companies/search` | Quick search |
| GET | `/companies/:id` | Full detail with associations |
| PATCH | `/companies/:id` | Update |
| DELETE | `/companies/:id` | Soft delete |
| POST | `/associations/contact-company` | Create association |
| DELETE | `/associations/contact-company` | Remove association |
| GET | `/tags` | All tags |
| POST | `/tags` | Create tag |
| PATCH | `/tags/:id` | Update tag |
| DELETE | `/tags/:id` | Delete tag |
| GET | `/properties/:objectType` | List property definitions |
| POST | `/properties` | Create property definition |
| PATCH | `/properties/:id` | Update property definition |

### Components (16)

| Component | Role |
|---|---|
| `GeekCrmAppComponent` | Root app shell, `<geek-crm-app>` custom element |
| `LoginComponent` | Auth page |
| `LayoutComponent` | Shell: sidebar + router-outlet |
| `SidebarComponent` | Left icon rail (HubSpot-style, expands on hover) |
| `NavbarComponent` | Top bar: search trigger, notifications, user menu |
| `ContactListComponent` | Table with search/filter/sort, bulk actions |
| `ContactDetailComponent` | 3-column HubSpot record layout |
| `ContactFormComponent` | Create/edit modal |
| `CompanyListComponent` | Table with search/filter |
| `CompanyDetailComponent` | 3-column layout |
| `CompanyFormComponent` | Create/edit modal |
| `TimelineComponent` | Reusable activity timeline |
| `ActivityComposerComponent` | Log call/email/meeting/note inline composer |
| `RecordSidebarComponent` | Reusable left properties panel |
| `AssociationCardComponent` | Reusable right sidebar association card |
| `PropertyEditorComponent` | Reusable custom property field renderer |
| `LoadingComponent` | Loading spinner |
| `BadgeComponent` | Colored status badge |
| `AvatarComponent` | User/contact avatar with initials fallback |
| `EmptyStateComponent` | Empty list placeholder |

### UX Details
- **Sidebar:** Vertical left icon rail (56px collapsed, 240px expanded on hover). Icons for each section. Active section highlighted. Collapse animation.
- **Contact List:** HubSpot-style table with column selector, search bar, filter dropdowns (lifecycle stage, owner, lead status, tags), sort, pagination. Checkbox selection for bulk actions.
- **Contact Detail:** 3-column layout. Left: all properties in collapsible groups + custom properties. Center: activity composer at top + tab bar (Activity | Emails | Calls | Tasks | Notes) + timeline. Right: association cards (Companies, Deals placeholder, Tickets placeholder).
- **Activity Composer:** Inline at top of center column. Tab buttons: Note, Email, Call, Meeting, Task. Each tab shows relevant fields. On submit, creates activity + adds to timeline.

### Backend Setup
- Express server with Winston logging, Zod validation, JWT auth (15min access / 7-day refresh)
- Prisma client singleton, consistent `{ success, data, meta }` / `{ success, error }` response format
- `toErrorMessage()` utility, pagination helper with `{ page, perPage, total, totalPages }`
- CORS configured for WordPress domain
- Seed script: creates admin user + default account settings + default property definitions

---

## Phase 2: Deals + Tasks + Pipelines

**Goal:** Kanban deal board, deal detail with 3-column layout, multiple pipelines, task management, deal line items.

### Database (Phase 2 migration)
Tables: `Pipeline`, `PipelineStage`, `Deal`, `DealLineItem`, `ContactDeal`, `CompanyDeal`, `Task`, `TaskQueue`

### API Endpoints (24 routes)

| Method | Path | Description |
|---|---|---|
| GET | `/pipelines` | All pipelines with stages |
| POST | `/pipelines` | Create pipeline |
| PATCH | `/pipelines/:id` | Update pipeline |
| DELETE | `/pipelines/:id` | Delete pipeline |
| PATCH | `/pipelines/:id/stages` | Reorder/update stages |
| GET | `/deals` | Paginated deal list with filters |
| POST | `/deals` | Create deal |
| GET | `/deals/:id` | Deal detail with associations |
| PATCH | `/deals/:id` | Update deal |
| POST | `/deals/:id/move` | Move to stage (drag-drop) |
| POST | `/deals/:id/won` | Mark won |
| POST | `/deals/:id/lost` | Mark lost with reason |
| GET | `/deals/:id/activities` | Deal activity timeline |
| POST | `/deals/:id/activities` | Log activity on deal |
| POST | `/associations/contact-deal` | Associate contact to deal |
| DELETE | `/associations/contact-deal` | Remove association |
| POST | `/associations/company-deal` | Associate company to deal |
| DELETE | `/associations/company-deal` | Remove association |
| GET | `/tasks` | Paginated with status/priority/type filter |
| POST | `/tasks` | Create task |
| GET | `/tasks/:id` | Task detail |
| PATCH | `/tasks/:id` | Update task |
| POST | `/tasks/:id/complete` | Mark complete |
| DELETE | `/tasks/:id` | Delete task |

### Components (10)

`PipelineBoardComponent`, `DealCardComponent`, `DealFormComponent`, `DealDetailComponent`, `KanbanBoardComponent` (reusable), `KanbanCardComponent` (reusable), `TaskListComponent`, `TaskFormComponent`, `ConfirmDialogComponent`, `LineItemEditorComponent` (reusable)

### UX Details
- **Kanban Board:** Horizontal scroll columns per stage. Column headers: stage name + deal count + total value. Cards: contact avatar, deal name, value, owner, days in stage, stale icon. HTML5 drag-and-drop. Pipeline selector dropdown.
- **Deal Detail:** 3-column layout. Left: deal properties, amount, close date, pipeline/stage. Center: activity timeline + composer. Right: associated contacts, companies, line items, quotes (placeholder).
- **Task List:** Filterable table. Due today/overdue/upcoming views. Inline complete via checkbox. Task type icons (call, email, todo).
- **Stale Detection:** `deal.updatedAt` or last activity older than `stage.rottenDays` -> orange flame icon.

### Seed Data
Default "Sales Pipeline": Appointment Scheduled -> Qualified to Buy -> Presentation Scheduled -> Decision Maker Brought-In -> Contract Sent -> Closed Won / Closed Lost

---

## Phase 3: Tickets + Help Desk

**Goal:** Ticket pipelines, ticket detail with 3-column layout, ticket creation from multiple sources, SLA tracking.

### Database (Phase 3 migration)
Tables: `Ticket`, `ContactTicket`, `CompanyTicket`, `DealTicket`
Uses existing `Pipeline` and `PipelineStage` tables with `objectType: "TICKET"`.

### API Endpoints (16 routes)

| Method | Path | Description |
|---|---|---|
| GET | `/tickets` | Paginated with status/priority/source filter |
| POST | `/tickets` | Create ticket |
| GET | `/tickets/:id` | Ticket detail with associations |
| PATCH | `/tickets/:id` | Update ticket |
| POST | `/tickets/:id/move` | Move to pipeline stage |
| POST | `/tickets/:id/close` | Close ticket |
| GET | `/tickets/:id/activities` | Ticket activity timeline |
| POST | `/tickets/:id/activities` | Log activity on ticket |
| POST | `/associations/contact-ticket` | Associate contact |
| DELETE | `/associations/contact-ticket` | Remove association |
| POST | `/associations/company-ticket` | Associate company |
| DELETE | `/associations/company-ticket` | Remove association |
| POST | `/associations/deal-ticket` | Associate deal |
| DELETE | `/associations/deal-ticket` | Remove association |
| GET | `/tickets/board` | Kanban board data |
| GET | `/tickets/stats` | Ticket counts by status |

### Components (6)
`TicketListComponent`, `TicketBoardComponent` (reuses KanbanBoard), `TicketDetailComponent`, `TicketFormComponent`, `TicketCardComponent`

### Seed Data
Default "Support Pipeline": New -> Waiting on Contact -> Waiting on Us -> Closed

---

## Phase 4: Commerce (Products, Quotes, Invoices, Payments)

**Goal:** Full product catalog, quote builder with e-signature placeholder, quote-to-invoice conversion, payment recording.

### Database (Phase 4 migration)
Tables: `Product`, `Quote`, `QuoteLineItem`, `Invoice`, `InvoiceLineItem`, `Payment`

### API Endpoints (22 routes)

| Method | Path | Description |
|---|---|---|
| GET/POST | `/products` | List / Create |
| GET/PATCH/DELETE | `/products/:id` | Detail / Update / Deactivate |
| GET/POST | `/quotes` | List / Create |
| GET/PATCH/DELETE | `/quotes/:id` | Detail / Update / Delete draft |
| POST | `/quotes/:id/send` | Mark sent |
| POST | `/quotes/:id/approve` | Approve (if approval required) |
| POST | `/quotes/:id/sign` | Record signature |
| POST | `/quotes/:id/convert` | Convert to invoice |
| POST | `/quotes/:id/duplicate` | Duplicate quote |
| GET/POST | `/invoices` | List / Create |
| GET/PATCH | `/invoices/:id` | Detail / Update |
| POST | `/invoices/:id/send` | Mark sent |
| POST | `/invoices/:id/payment` | Record payment |
| POST | `/invoices/:id/void` | Void invoice |
| GET | `/payments` | Payment history |
| GET | `/commerce/stats` | Revenue KPIs |

### Components (10)
`ProductListComponent`, `ProductFormComponent`, `QuoteListComponent`, `QuoteFormComponent`, `QuoteDetailComponent`, `InvoiceListComponent`, `InvoiceFormComponent`, `InvoiceDetailComponent`, `PaymentFormComponent`, `CommerceStatsComponent`

### UX Details
- **Quote Builder:** Multi-section: sender info, recipient info, line items (product search, qty, price, discount), terms, signature block. Real-time total calculation via signals.
- **Quote Numbers:** `GQ-YYYY-NNN`, Invoice: `GI-YYYY-NNN`
- **PDF:** `@media print` CSS for print-ready layout. No external library.

---

## Phase 5: Marketing (Lists, Email, Forms)

**Goal:** Active/static contact lists with filter criteria, email campaigns with templates, form builder with submission tracking.

### Database (Phase 5 migration)
Tables: `List`, `ListMembership`, `EmailTemplate`, `EmailCampaign`, `Form`, `FormField`, `FormSubmission`

### API Endpoints (22 routes)

| Method | Path | Description |
|---|---|---|
| GET/POST | `/lists` | List / Create |
| GET/PATCH/DELETE | `/lists/:id` | Detail / Update / Delete |
| POST | `/lists/:id/members` | Add contacts to static list |
| DELETE | `/lists/:id/members` | Remove contacts |
| POST | `/lists/:id/refresh` | Re-evaluate active list filters |
| GET/POST | `/email-templates` | List / Create |
| GET/PATCH/DELETE | `/email-templates/:id` | Detail / Update / Delete |
| GET/POST | `/email-campaigns` | List / Create |
| GET/PATCH | `/email-campaigns/:id` | Detail / Update |
| POST | `/email-campaigns/:id/send` | Send / schedule |
| POST | `/email-campaigns/:id/cancel` | Cancel scheduled |
| GET/POST | `/forms` | List / Create |
| GET/PATCH/DELETE | `/forms/:id` | Detail / Update / Delete |
| POST | `/forms/:id/publish` | Publish form |
| POST | `/forms/:id/submit` | Record submission |
| GET | `/forms/:id/submissions` | List submissions |

### Components (12)
`ListListComponent`, `ListDetailComponent`, `ListFormComponent`, `EmailTemplateListComponent`, `EmailTemplateEditorComponent`, `EmailCampaignListComponent`, `EmailCampaignFormComponent`, `FormListComponent`, `FormBuilderComponent`, `FormDetailComponent`, `FormSubmissionListComponent`, `RichTextEditorComponent`

### UX Details
- **Active Lists:** JSON filter builder with AND/OR groups. Criteria: lifecycle stage, owner, tag, custom property, date ranges. Auto-refreshes membership on save.
- **Email Editor:** Rich text editor (ContentEditable + execCommand, no external lib). Template variable insertion `{{contact.firstName}}`. Preview mode.
- **Form Builder:** Drag-and-drop field ordering. Field types: text, email, phone, number, date, select, multi-select, checkbox, textarea. Preview + embed code.

---

## Phase 6: Sales Tools (Sequences, Meetings)

**Goal:** Multi-step email/task sequences for outbound sales, meeting scheduling links with availability.

### Database (Phase 6 migration)
Tables: `Sequence`, `SequenceStep`, `SequenceEnrollment`, `MeetingLink`, `MeetingBooking`

### API Endpoints (18 routes)

| Method | Path | Description |
|---|---|---|
| GET/POST | `/sequences` | List / Create |
| GET/PATCH/DELETE | `/sequences/:id` | Detail / Update / Delete |
| POST | `/sequences/:id/steps` | Add step |
| PATCH | `/sequences/:id/steps/:stepId` | Update step |
| DELETE | `/sequences/:id/steps/:stepId` | Delete step |
| POST | `/sequences/:id/enroll` | Enroll contact(s) |
| POST | `/sequences/:id/unenroll` | Unenroll contact |
| POST | `/sequences/:id/pause` | Pause enrollment |
| POST | `/sequences/:id/resume` | Resume enrollment |
| GET | `/sequences/:id/enrollments` | Enrollment list |
| GET/POST | `/meetings` | List / Create meeting link |
| GET/PATCH/DELETE | `/meetings/:id` | Detail / Update / Delete |
| GET | `/meetings/:slug/availability` | Available time slots |
| POST | `/meetings/:slug/book` | Book meeting |
| POST | `/meetings/:id/cancel` | Cancel booking |

### Components (8)
`SequenceListComponent`, `SequenceDetailComponent`, `SequenceFormComponent`, `SequenceStepEditorComponent`, `SequenceEnrollmentListComponent`, `MeetingLinkListComponent`, `MeetingLinkFormComponent`, `MeetingBookingComponent`

### UX Details
- **Sequence Builder:** Visual step list (vertical). Step types: automated email, manual email task, call task, general task, delay. Each step shows preview. Enrollment stats per step.
- **Meeting Scheduler:** Configurable availability per day. Buffer times. Max bookings per day. Shareable booking page.

---

## Phase 7: Automations (Workflows)

**Goal:** Visual workflow builder with triggers, actions, branching, and delay steps.

### Database (Phase 7 migration)
Tables: `Workflow`, `WorkflowStep`

### API Endpoints (10 routes)

| Method | Path | Description |
|---|---|---|
| GET/POST | `/workflows` | List / Create |
| GET/PATCH/DELETE | `/workflows/:id` | Detail / Update / Delete |
| POST | `/workflows/:id/activate` | Activate workflow |
| POST | `/workflows/:id/deactivate` | Deactivate |
| POST | `/workflows/:id/steps` | Add step |
| PATCH | `/workflows/:id/steps/:stepId` | Update step |
| DELETE | `/workflows/:id/steps/:stepId` | Delete step |
| POST | `/workflows/:id/test` | Test with sample record |

### Components (4)
`WorkflowListComponent`, `WorkflowDetailComponent`, `WorkflowBuilderComponent`, `WorkflowStepConfigComponent`

### UX Details
- **Workflow Builder:** Visual vertical flow chart. Trigger at top. Steps below with connectors. IF/THEN branching splits into two paths. Step config in slide-over panel. Actions: send email, create task, update property, add to list, send notification, delay, enroll in sequence.

---

## Phase 8: Service Hub (Knowledge Base, Chatflows, Feedback)

**Goal:** Knowledge base with categories and articles, live chat configuration, customer feedback surveys.

### Database (Phase 8 migration)
Tables: `KBCategory`, `KBArticle`, `Chatflow`, `FeedbackSurvey`, `FeedbackResponse`

### API Endpoints (20 routes)

| Method | Path | Description |
|---|---|---|
| GET/POST | `/kb/categories` | List / Create |
| GET/PATCH/DELETE | `/kb/categories/:id` | Detail / Update / Delete |
| GET/POST | `/kb/articles` | List / Create |
| GET/PATCH/DELETE | `/kb/articles/:id` | Detail / Update / Delete |
| POST | `/kb/articles/:id/publish` | Publish |
| POST | `/kb/articles/:id/helpful` | Record helpful vote |
| GET | `/kb/search` | Search articles |
| GET/POST | `/chatflows` | List / Create |
| GET/PATCH/DELETE | `/chatflows/:id` | Detail / Update / Delete |
| POST | `/chatflows/:id/activate` | Activate |
| GET/POST | `/feedback` | List / Create survey |
| GET/PATCH/DELETE | `/feedback/:id` | Detail / Update / Delete |
| POST | `/feedback/:id/publish` | Publish |
| POST | `/feedback/:id/respond` | Record response |
| GET | `/feedback/:id/responses` | List responses |

### Components (10)
`KBCategoryListComponent`, `KBArticleListComponent`, `KBArticleEditorComponent`, `KBArticleDetailComponent`, `ChatflowListComponent`, `ChatflowEditorComponent`, `FeedbackSurveyListComponent`, `FeedbackSurveyFormComponent`, `FeedbackSurveyDetailComponent`, `FeedbackResponseListComponent`

---

## Phase 9: Reporting (Dashboards, Reports, Goals)

**Goal:** Configurable dashboards with widgets, report builder, sales/service goals tracking.

### Database (Phase 9 migration)
Tables: `Dashboard`, `DashboardWidget`, `Report`, `Goal`

### API Endpoints (16 routes)

| Method | Path | Description |
|---|---|---|
| GET/POST | `/dashboards` | List / Create |
| GET/PATCH/DELETE | `/dashboards/:id` | Detail / Update / Delete |
| POST | `/dashboards/:id/widgets` | Add widget |
| PATCH | `/dashboards/:id/widgets/:wId` | Update widget |
| DELETE | `/dashboards/:id/widgets/:wId` | Remove widget |
| PATCH | `/dashboards/:id/layout` | Update widget positions |
| GET/POST | `/reports` | List / Create |
| GET/PATCH/DELETE | `/reports/:id` | Detail / Update / Delete |
| POST | `/reports/:id/run` | Execute report query |
| GET/POST | `/goals` | List / Create |
| GET/PATCH/DELETE | `/goals/:id` | Detail / Update / Delete |
| GET | `/goals/:id/progress` | Goal progress |

### Components (8)
`DashboardListComponent`, `DashboardDetailComponent`, `DashboardWidgetComponent`, `WidgetEditorComponent`, `ReportListComponent`, `ReportBuilderComponent`, `GoalListComponent`, `GoalFormComponent`

### UX Details
- **Dashboard:** CSS grid layout. Widgets: number cards, bar/line/pie/donut charts (Canvas API or Bootstrap progress bars for Phase 9, add Chart.js in future), tables, funnels.
- **Report Builder:** Select data source, choose dimensions/measures, add filters, pick chart type. Preview. Save.
- **Goals:** Track against metric (revenue, deals closed, etc.). Progress bar visualization. Period filtering.

---

## Phase 10: Settings + Global Search + WordPress Deploy

**Goal:** Admin settings, global search (Cmd+K), performance optimization, WordPress deployment.

### API Endpoints (14 routes)

| Method | Path | Description |
|---|---|---|
| GET | `/search` | Global search across all objects |
| GET/PATCH | `/settings/account` | Account settings |
| GET/POST/PATCH/DELETE | `/settings/users` | User management |
| GET | `/settings/properties/:objectType` | Properties for object |
| POST | `/settings/properties` | Create property |
| PATCH/DELETE | `/settings/properties/:id` | Update / Delete property |
| GET/PATCH | `/settings/pipelines/:objectType` | Pipeline settings |
| GET/POST/PATCH/DELETE | `/settings/tags` | Tag management |

### Components (8)
`GlobalSearchComponent`, `SettingsLayoutComponent`, `AccountSettingsComponent`, `UserSettingsComponent`, `PropertySettingsComponent`, `PipelineSettingsComponent`, `TagSettingsComponent`, `IntegrationsComponent` (placeholder)

### UX Details
- **Global Search:** `Cmd+K` / `Ctrl+K` opens modal. 300ms debounce. Results grouped by type (contacts, companies, deals, tickets, quotes, invoices). Keyboard navigation. Recent searches.
- **Settings:** Left sidebar with sections (Account, Users, Properties, Pipelines, Products, Tags). Each section is its own page.

### WordPress Deploy
```php
wp_enqueue_script_module('geek-crm-app',
  get_template_directory_uri() . '/assets/geek-elements/geek-crm-main.js', [], '1.0.0');
```
```html
<geek-crm-app></geek-crm-app>
```

### Performance
- All routes lazy-loaded (`loadComponent`)
- `OnPush` on all components
- Virtual scrolling for lists > 200 rows
- `outputHashing: "none"` for predictable filenames

---

## Angular Elements Registration (main.ts)

```typescript
const app = await createApplication({
  providers: [
    provideZonelessChangeDetection(),
    provideHttpClient(withInterceptors([authInterceptor])),
    provideRouter(geekCrmRoutes, withHashLocation()),
  ],
});
customElements.define('geek-crm-app',
  createCustomElement(GeekCrmAppComponent, { injector: app.injector }));
app.injector.get(Router).initialNavigation();
```

---

## Phase Summary

| Phase | New DB Tables | API Routes | AI Endpoints | Components | Deliverable |
|---|---|---|---|---|---|
| 1 | 17 | 28 | 3 | 20 | Auth + Contacts + Companies + Activities + Properties + AI Foundation |
| 2 | 5 | 24 | 4 | 10 | Deals + Pipelines + Tasks + Kanban + AI Deal Health/Coaching |
| 3 | 3 | 16 | 3 | 6 | Tickets + Help Desk + AI Classification/Reply |
| 4 | 5 | 22 | 2 | 10 | Products + Quotes + Invoices + Payments + AI Quote/Invoice |
| 5 | 6 | 22 | 3 | 12 | Lists + Email Campaigns + Forms + AI Marketing |
| 6 | 4 | 18 | 2 | 8 | Sequences + Meetings + AI Personalization/Briefings |
| 7 | 2 | 10 | 1 | 4 | Workflows + AI Suggestions |
| 8 | 5 | 20 | 2 | 10 | Knowledge Base + Chatflows + Feedback + AI Responses |
| 9 | 4 | 16 | 2 | 8 | Dashboards + Reports + Goals + AI Forecasting/Insights |
| 10 | 0 | 14 | 2 | 8 | Search + Settings + AI Assistant + WP Deploy |
| **Total** | **51** | **190** | **24** | **96** | **Full HubSpot + Agentforce Clone** |

---

## Key Technical Decisions

- **`cuid()` IDs:** URL-safe, time-sortable, no sequential-ID leakage
- **Separate Contact and Company models (not self-relation):** Mirrors HubSpot's distinct object types with explicit many-to-many associations
- **Stored `displayName`:** Full-text search performance without joins
- **EAV for custom properties:** `PropertyDefinition` + `PropertyValue` tables allow user-defined fields per object type without schema changes
- **Many-to-many junction tables for all associations:** Mirrors HubSpot's flexible association model where any contact can belong to multiple companies, deals, tickets
- **Pipeline model shared between Deals and Tickets:** `objectType` field distinguishes deal pipelines from ticket pipelines (HubSpot pattern)
- **Hash routing:** WordPress controls URLs; `#/contacts/123` avoids conflicts
- **HTML5 drag-and-drop (no CDK):** Fewer dependencies, sufficient for CRM
- **JSON columns for workflow/list config:** Flexible schema for filter criteria, workflow step configs, and form field options
- **No charting library in early phases:** Bootstrap progress bars + Canvas API for simple charts. Add Chart.js if needed in Phase 9+.
- **Claude API (Sonnet 4+) for all AI features:** Zero-config, no training data, no admin. Every AI feature is a single backend endpoint that gathers Prisma context + calls Claude. Graceful degradation when no API key is set.
- **AI audit trail:** `AIInteraction` table logs every AI call (action, prompt, response, tokens, duration) for transparency and cost tracking.
- **AI caching:** `AICache` table stores responses with TTL. Record summaries invalidated on record update. Reduces API costs for repeated queries.
- **User-in-the-loop:** AI drafts and suggests but never takes autonomous action. Users review every AI output before it becomes a real email, note, or classification.

## Verification

After each phase:
1. Run `npm run lint` in backend
2. Run `ng build geek-crm-library && ng build geek-crm-elements` — must compile clean
3. Test API endpoints via Playwright or curl
4. Verify custom element renders in a test HTML page
5. After Phase 10: FTP deploy to WordPress, verify `<geek-crm-app>` renders on live site

## Reference Files (from existing projects)

- `Acord-PCS-CRM-Workspace/projects/acord-pcs-crm-elements/src/main.ts` — `createApplication()` + `customElements.define()` pattern
- `Acord-PCS-CRM-Workspace/angular.json` — `outputHashing: "none"`, Bootstrap config, schematics
- `Acord-PCS-CRM-Workspace/projects/acord-pcs-crm-library/src/lib/core/services/api.service.ts` — Base HttpClient service pattern
- `Acord-PCS-CRM-Workspace/projects/acord-pcs-crm-library/src/lib/core/models/index.ts` — `ApiResponse<T>`, `PaginatedResponse<T>` interfaces
