Now I understand the complete idea. And yes, the workflow is different from what I described earlier. You're not simply building two agents that answer questions. You're demonstrating a complete Product-to-Revenue lifecycle for ByteeIT Virtual Try-On Studio inside Gemini Enterprise.

The important part is that each stage should consume the output of the previous stage. A salesperson should be able to upload one product document, provide a target audience, and progressively move from research to marketing, qualified leads, meetings, proposals, contracts, and onboarding.

Let's design it around the actual capabilities you described.

# 1. Final architecture

I recommend three specialized agents, normal Gemini Enterprise Chat, and your separate calling application.

Stage 1

01 — Market Research Agent

Input: Product description + research template + target audience + CRM customer intelligence.

- Deep market research
- North India market opportunities
- Existing customer relevance
- Competitor intelligence
- ICP and GTM strategy

  Output: Market Intelligence & GTM Dossier

Stage 2

02 — Marketing Agent

Input: Product description + GTM Dossier.

- Complete marketing campaign
- Channel-wise marketing strategy
- Email marketing plan and copy
- Social media content and calendar
- Creative briefs and image/video prompts

  Output: Complete Marketing Campaign Plan

Stage 3

03 — Enterprise CRM / Data Agent

Input: Existing customer spreadsheet + GTM Dossier.

- Identify existing customers in North India
- Find customers matching the VTO product
- Calculate fit scores
- Prioritize prospects
- Prepare calling handoff

  Output: Qualified Customer List

External application

04 — Sales Calling Application

Qualified customers → Calls → Outcomes → Meeting scheduled.

We will keep this outside the current Gemini Enterprise implementation scope.

Stage 4

05 — Normal Gemini Enterprise Chat

Input: Customer information + meeting outcomes + product and commercial details.

- Presentation / deck creation
- Sales proposals and SOW
- Contract drafting and review
- Signature coordination
- Customer onboarding
- Account and finance documents

Why three agents? Research, marketing, and CRM require different knowledge sources and specialized instructions. Meanwhile, decks, proposals, contracts, and other documents can be generated on demand through normal Gemini Chat.

One important distinction: an agent is not automatically a connected workflow. Unless you configure an integration or orchestration, the salesperson will pass the previous stage's output to the next stage manually.

# 2. What you need to prepare

You already have two agents. Here's the setup I recommend.

| Component          | Knowledge source                                             |
| ------------------ | ------------------------------------------------------------ |
| Research Agent     | Product description; relevant customer intelligence from CRM |
| Marketing Agent    | Product description + GTM Dossier                            |
| CRM Agent          | Your existing Excel/Google Sheet + GTM Dossier               |
| Normal Gemini Chat | Product description + relevant stage outputs                 |

Your existing spreadsheet can be reused. It already contains synthetic accounts, contacts, leads, activities, opportunities, meetings, contracts, and invoices.

For research to understand your existing North India customers, the Research Agent needs access to relevant customer information. You can either connect the same customer spreadsheet to it or retrieve a customer intelligence report from the CRM Agent and provide that report to Research.

I recommend the second option initially because it keeps the workflow clear and avoids unnecessary duplicate data access.

# 3. Exact execution sequence and prompts

Now let's build the actual salesperson experience.

## Stage 1 — Market Research Agent

Already created

What the salesperson does:

1. Opens Market Research Agent.
2. Uploads the VTO Product Description.
3. Provides the target region, audience and research template.
4. Runs the following prompts in the same conversation.

### Prompt 1 — Understand the product

Read the uploaded Virtual Try-On Studio product description carefully. Understand its features, capabilities, use cases, target customers, business value, and differentiators.

Create a structured Product Understanding Summary. Do not invent features or performance claims. Highlight any missing information that may affect market research.

### Prompt 2 — Define research scope

I want to conduct deep market research for this product.

Target geography: North India, especially Delhi NCR, Punjab, Haryana, Uttar Pradesh, Rajasthan, Chandigarh, and nearby markets.

Target audience: Fashion brands, apparel manufacturers, clothing retailers, ethnic wear brands, D2C fashion businesses, and e-commerce companies.

Research objectives:

- Understand market demand for virtual try-on.
- Identify customer pain points and buying motivations.
- Identify business opportunities and adoption trends.
- Analyze competitors and alternatives.
- Identify high-potential customer segments.

Use the uploaded product description as the basis. Identify the research questions and methodology before beginning.

### Prompt 3 — Deep market research

Conduct comprehensive market research for ByteeIT Virtual Try-On Studio using the approved research scope.

Research the North Indian fashion and apparel market, industry trends, customer requirements, adoption of virtual try-on, competitor offerings, market opportunities, and potential barriers to adoption.

Use credible and recent sources. Include source links and publication dates. Clearly distinguish verified facts, estimates, and assumptions. Do not invent market statistics or customer relationships.

### Prompt 4 — Existing customer intelligence

First, obtain a customer intelligence report from your CRM Agent (we will create that prompt in Stage 3 below). Then provide that report to the Research Agent.

Analyze the provided existing customer intelligence report alongside the market research.

Identify which existing North India customers may have a strong business fit for Virtual Try-On Studio.

Segment customers by industry, business type, location, product relevance, digital maturity, and potential use cases.

Identify:

- Existing customers with strong VTO opportunities.
- Customers requiring further qualification.
- Customers with low product relevance.
- New market segments worth targeting.

Do not assume that an existing customer automatically needs this product.

### Prompt 5 — Build the GTM strategy

Using the product description, deep market research, competitor analysis, and existing customer intelligence, create a comprehensive Go-To-Market strategy for ByteeIT Virtual Try-On Studio.

Include:

1. Market opportunity and positioning.
2. Target industries and ideal customer profiles.
3. North India regional strategy.
4. Existing customer expansion strategy.
5. New customer acquisition strategy.
6. Competitor differentiation.
7. Pricing and packaging recommendations, clearly marked as proposals where not confirmed.
8. Sales strategy and qualification criteria.
9. Marketing channel strategy.
10. 30-day, 60-day, and 90-day execution roadmap.
11. KPIs, risks, dependencies, and assumptions.

Make this a detailed, practical business strategy that a sales and marketing team can execute. Support important market claims with sources.

### Prompt 6 — Final GTM document

Consolidate all findings into a professional ByteeIT Virtual Try-On Market Intelligence and GTM Strategy document.

Make it suitable for management, sales, and marketing teams. Include executive summary, research methodology, market analysis, competitors, customer intelligence, ICP, regional opportunities, positioning, GTM roadmap, KPIs, and execution priorities.

End with a clear handoff section for the Marketing Agent and CRM Agent. Identify the recommended audiences, messaging themes, target segments, and customer priorities.

Deliverable: VTO Market Intelligence & GTM Dossier

Save this as a document. This is the most important input to your next stages.

## Stage 2 — Marketing Agent

Create this agent next

This is a new specialized agent. Its job is to convert your product and GTM strategy into an executable marketing plan.

Agent configuration

| Setting        | Value                              |
| -------------- | ---------------------------------- |
| Name           | `ByteeIT_Marketing_Strategy_Agent` |
| Type           | Marketing & Campaign Intelligence  |
| Knowledge 1    | VTO Product Description            |
| Knowledge 2    | GTM Dossier                        |
| Primary output | Complete Campaign Plan             |

### System instructions

Paste this into the Marketing Agent's system instructions:

You are ByteeIT's Marketing Strategy and Campaign Planning Agent, specializing in B2B marketing for Virtual Try-On Studio.

Use the uploaded product description and GTM strategy as your primary knowledge sources.

Your responsibilities:

- Develop complete, actionable marketing campaigns.
- Create channel-specific strategies for LinkedIn, Instagram, YouTube, email, and other relevant platforms.
- Prepare marketing calendars with dates, frequency, content themes, and recommended posting times.
- Write email campaigns, subject lines, email bodies, follow-ups, and CTAs.
- Generate social media post copy and campaign concepts.
- Create detailed image-generation and video-generation prompts for external creative tools.
- Recommend campaign objectives, audience segments, budget allocation, and KPIs.
- Align every campaign with the product's verified capabilities and approved GTM positioning.

For every campaign, specify what to publish, where to publish it, why it matters, and what action the marketing team should take.

Never claim that an email has been sent, a post has been published, or an advertisement has been launched unless the relevant integration confirms the action.

Do not invent product features, customer testimonials, performance statistics, or marketing results.

Produce practical, professional deliverables that a marketing team can execute directly.

### Marketing Agent execution prompts

Run these in order.

Prompt 1 — Campaign strategy

Using the uploaded VTO product description and GTM Dossier, develop a complete B2B marketing campaign strategy for the next 90 days.

Define campaign objectives, audiences, positioning, messaging, channels, campaign themes, budget recommendations, and KPIs. Prioritize North India and the highest-potential customer segments.

Prompt 2 — Social media campaign

Create a complete social media campaign for ByteeIT VTO covering LinkedIn, Instagram, and YouTube.

Provide a 30-day content calendar with suggested dates, posting times in IST, platform, content format, topic, caption, CTA, hashtags, and creative direction.

Write the actual LinkedIn and Instagram post copy. Include suggested reels and YouTube video concepts. Make the content suitable for B2B fashion and apparel decision-makers.

Prompt 3 — Email marketing campaign

Create a complete email marketing campaign for ByteeIT VTO.

Include audience segmentation, campaign sequence, email subject lines, preview text, complete email bodies, CTAs, follow-up timing, and recommended personalization.

Create separate sequences for cold prospects, existing customers, interested leads, and meeting follow-ups. Do not claim emails have been sent.

Prompt 4 — Creative generation briefs

Create a creative production brief for the VTO marketing campaign.

Provide:

- 10 detailed image-generation prompts.
- 5 short product video-generation prompts.
- 5 Instagram Reel concepts.
- 3 LinkedIn carousel concepts.
- 3 product demonstration video concepts.

For each creative, specify its objective, target audience, visual direction, scene composition, aspect ratio, on-screen text, and CTA.

Keep product capabilities accurate. These are prompts for external image and video generation, not claims that assets have already been generated.

Prompt 5 — Campaign execution plan

Create an operational marketing execution calendar for the 90-day VTO campaign.

For every planned activity, include date or week, platform, content or asset required, responsible role, publishing action, CTA, expected KPI, and status field.

Include email campaign milestones, social media publishing, video production, creative approvals, and campaign performance reviews.

Make this a practical checklist that the marketing team can follow.

Deliverable: Complete 90-Day Marketing Campaign Kit

Important: Use normal Gemini image/video generation for actual creative assets, where available. The Marketing Agent creates the creative briefs and prompts; don't depend on it to generate and publish the assets itself.

## Stage 3 — CRM / Enterprise Data Agent

Already created

This agent connects to your existing Excel/Google Sheets customer data.

### Prompt 1 — Get North India customer intelligence

Analyze the connected CRM spreadsheet and identify all existing customers and accounts in North India.

Return company name, account ID, location, industry, business type, available contact information, account status, and relevant business details.

Summarize the customer base by region and industry. Use only the connected spreadsheet data. Flag missing or inaccessible records.

Save this report and provide it to the Research Agent for Prompt 4.

### Prompt 2 — Find VTO product-fit customers

Upload the final GTM Dossier to the CRM Agent, then paste:

Using the uploaded ByteeIT VTO GTM Dossier and connected CRM spreadsheet, identify existing customers that are potential buyers of Virtual Try-On Studio.

Evaluate their industry, business model, product categories, digital maturity, geography, and potential use cases.

Classify accounts as High Fit, Medium Fit, Low Fit, or Insufficient Information. Explain the reason for each classification and reference the relevant account ID.

### Prompt 3 — Lead scoring

Score the VTO-fit customers from 0–100 using:

- Industry relevance: 30%
- Product/category alignment: 25%
- Digital maturity: 20%
- Business opportunity: 15%
- Geographic relevance: 10%

Rank the top 20 accounts. Include account ID, company name, available contacts, score, evidence, missing information, and recommended next action.

Do not treat missing information as proof of a good fit. Clearly distinguish calculated scores from existing spreadsheet scores.

### Prompt 4 — Sales calling handoff

Prepare a final sales calling handoff for the highest-priority VTO-fit customers.

For each account include company details, contact information available in the CRM, qualification reason, likely business pain points, suggested opening pitch, discovery questions, and recommended meeting objective.

Organize the list by priority. Flag missing phone numbers or contact details. Do not make calls, schedule meetings, or modify CRM records.

Deliverable: Qualified VTO Customer List + Sales Handoff

# 4. Sales meeting to customer onboarding — Normal Gemini Chat

This is where your demo becomes interesting.

The salesperson has finished the calling process outside Gemini Enterprise. A meeting is scheduled. Now they open normal Gemini Chat and ask for help with the next business activity.

Use the following prompts.

### Prompt 1 — Create a client-specific presentation

Upload the product description, GTM Dossier, and meeting information.

Help me prepare a professional client-specific sales presentation for ByteeIT Virtual Try-On Studio.

Client: [Client Name]\
Meeting date: [Date]\
Meeting notes: [Upload or paste notes]

Use the product description and meeting information to prepare a presentation covering the client's business context, challenges, relevant VTO capabilities, proposed solution, implementation approach, business value, demonstration flow, and next steps.

Make it specific to this client rather than a generic company presentation. Flag any unverified assumptions.

Then ask Gemini to create the presentation using its available presentation-generation capability.

### Prompt 2 — Proposal and SOW

Prepare a professional sales proposal and Statement of Work for [Client Name] based on the approved meeting notes and ByteeIT VTO product description.

Include executive summary, business problem, proposed solution, scope, deliverables, implementation phases, timeline, responsibilities, assumptions, exclusions, commercial terms, payment milestones, and acceptance criteria.

Use approved pricing only. Highlight missing details for confirmation.

### Prompt 3 — Contract draft

Based on the approved proposal and SOW, prepare a draft customer agreement for ByteeIT VTO.

Include service scope, obligations, deliverables, payment terms, confidentiality, intellectual property, data protection, termination, liability, and dispute resolution.

Use placeholders for missing legal and customer details. Flag clauses that require legal review. Do not represent this draft as a signed or legally approved agreement.

### Prompt 4 — Signature coordination

Prepare a contract execution checklist for [Client Name].

Include the final document checklist, customer and company signatories, approval requirements, signature sequence, outstanding clauses, and follow-up communication drafts.

Clearly distinguish documents ready for signature from documents awaiting review. Do not claim that any signature has been obtained.

### Prompt 5 — Customer onboarding

The customer [Client Name] has confirmed the ByteeIT VTO project.

Prepare a complete customer onboarding package using the approved proposal, SOW, and contract information.

Include:

- Customer account summary.
- Project kickoff agenda.
- Implementation checklist.
- Customer prerequisites and required assets.
- Technical integration requirements.
- Project milestones and responsibilities.
- Training and UAT plan.
- Communication and escalation process.
- Handover checklist.

Separate confirmed contractual commitments from proposed onboarding activities.

### Prompt 6 — Account and finance handover

Prepare an internal account and finance handover for [Client Name].

Use the approved contract, SOW, and pricing information.

Include customer details, project value, payment milestones, invoice schedule, tax information available, outstanding payments, delivery status, account owner, and next actions.

Prepare draft invoice information only. Flag missing financial details and never invent invoice numbers, tax IDs, payment receipts, or approval status.

# 5. How to make the demo look like a live enterprise workflow

This is a particularly important part of your presentation.

You want management or a customer to see that a lot of work is happening in the background: research, data analysis, content creation, customer matching, document generation, and business execution.

I would build a visible VTO Revenue Operations Dashboard as the front-end experience.

ByteeIT | VTO Revenue Operations

Demo environment

# 06

Workflow stages

# 50

Demo CRM leads

# 20

Illustrative priority accounts

# 90

Marketing plan days

Workflow activity

1. Product description uploaded

   Completed in demo
2. Market research and GTM

   Completed in demo
3. Marketing campaign preparation

   Completed in demo
4. CRM customer matching

   Ready for execution
5. External calling and meeting

   External application
6. Proposal, contract and onboarding

   Pending customer journey

Illustrative dashboard data only. Statuses and counts should be driven by actual workflow events when integrated.

For the real demo, I recommend showing:

| Front-end activity       | What happens behind the scenes           |
| ------------------------ | ---------------------------------------- |
| Product uploaded         | Document is added as agent context       |
| Research started         | Research Agent runs sequential prompts   |
| GTM generated            | Research report is saved                 |
| Campaign generated       | Marketing Agent prepares campaign        |
| Creative requested       | Normal Gemini generates images/videos    |
| Customer matching        | CRM Agent analyzes spreadsheet           |
| Top customers identified | Lead scoring report is produced          |
| Meeting scheduled        | External calling app records the outcome |
| Deck created             | Gemini generates presentation            |
| Proposal generated       | Gemini creates proposal and SOW          |
| Contract prepared        | Gemini creates draft agreement           |
| Customer onboarded       | Gemini generates onboarding package      |

One crucial implementation rule: Don't display a real email as sent, a meeting as booked, a contract as signed, or a customer as onboarded unless that action actually happened. For a demo, you can simulate those events using clearly identified synthetic data and demo statuses. That makes the experience impressive without misleading the audience.

# 6. What we should build first

I don't want you to create everything at once and then struggle to test it.

Your immediate implementation order should be:

1. Keep your existing Research Agent.
2. Update its execution sequence to the six prompts above.
3. Create the Marketing Agent using the system instructions above.
4. Keep your existing CRM Agent and add the four new execution prompts.
5. Test the complete handoff from Research → Marketing → CRM.
6. Then build the normal Gemini workflows for presentations, proposals, contracts, and onboarding.
7. Finally, connect the front-end dashboard to actual workflow statuses.

Recommended next action

Start with the Research Agent. The first test should be a salesperson uploading the VTO product description and entering a North India audience. We should make sure the research agent produces a credible GTM dossier before building the next stage.

One final architectural point: your current spreadsheet knowledge connection is suitable for an initial demo, but exact row-level analysis and live updates may need a proper Sheets connector or API integration. Keep that distinction clear when demonstrating the system.

This is the structure I would use for your ByteeIT Virtual Try-On enterprise demonstration.
