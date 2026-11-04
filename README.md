Revenue Intelligence Dashboard — NovaTech Solutions

A Business Intelligence project built in Amazon QuickSight: three disconnected sources (CRM deals, marketing campaigns, support tickets) are cleaned, joined on account_id, and turned into a three-sheet interactive dashboard, with natural-language querying through Amazon Q and a written report for the VP of Revenue.

Read first: the unified-dataset join multiplies rows (499 deals → 63,420 rows), so some dashboard KPIs are inflated versus the source files. See Data notes. All figures in the "Key findings" section below are computed directly from the source CSVs.

Business context

NovaTech's VP of Revenue, Sarah Chen, was copying numbers from three tools into slides every Monday. The goal: one view of Sales, Marketing and Customer Support, so revenue decisions start from one set of numbers.

Repository contents
File	Purpose
Marketing_Funnel.pdf	Dashboard sheet 1: spend, attributed revenue, ROI, funnel stages, channel performance
Sales_Pipeline.pdf	Dashboard sheet 2: deal value, win rate, closing time, region and rep performance
Customer_Health.pdf	Dashboard sheet 3: ticket volume, sentiment, downtime, at-risk accounts
Sarah_Chen_Stakeholder_Report.md	Written report for the VP of Revenue
Step 1 - 7a_verification_log.md	Verification of the pre-indexed knowledge bases (Quick Chat vs expected answers)
Step 4 - 16_Q_Exploration_Log.md	Amazon Q exploration log with cross-checks and reflection
project-starter-resources.zip	Source CSVs, data dictionary, company background and brief, templates
Data sources
File	Rows	Content
novatech_crm_deals.csv	499	Opportunities: stage (Won/Lost), deal_value, rep, region, product, loss reason; 85 accounts
novatech_marketing_campaigns.csv	2,240	Leads: campaign, channel, funnel stage, spend, attributed revenue, response; 100 account IDs
novatech_support_tickets.csv	3,000	Tickets Jun 2023 – Feb 2025: priority, product area, sentiment, users affected, downtime; 100 account IDs

Join key: account_id (85 accounts appear in all three sources).

Workflow
Prepare data. Checked completeness and types in QuickSight; set date and numeric types for tickets; added calculated fields days_to_close (created → closed, in days) and campaign_roi ((revenue − spend) / spend).
Unify. novatech_crm_deals as anchor, left-joined to marketing and support on account_id → 63,420-row dataset.
Build the dashboard. Three sheets with filter controls (region, industry, deal stage, company size, channel, priority).
Explore with Amazon Q. Tested baseline answers, configured a QuickSight Topic (synonyms, calculated fields), and cross-checked answers against the dashboard and source data.
Communicate. Stakeholder report with findings, implications and actions.
Key findings (computed from the source CSVs)

Sales (novatech_crm_deals.csv)

499 deals: 315 won, 184 lost → 63.1% win rate.
Total deal value is $707,201, all on won deals (lost deals carry a value of 0). Average value per deal: $1,417.
Average time to close: 66.8 days (won 71.9, lost 57.9).
Loss reasons (184 lost): No Decision Made 23.4%, Poor Product Fit 23.4%, Competitor Won 18.5%, Timing Not Right 17.9%, Budget Constraints 16.8%.
Won value by region: Central $274K, West $248K, East $185K.

Marketing (novatech_marketing_campaigns.csv)

2,240 leads across 6 campaigns and 5 channels; 609 responded (27.2%).
Spend $12.36M vs attributed revenue $1.13M → ROI −90.9%. Every one of the six campaigns has negative return.
Channels: Direct Mail has the highest response rate (53%) and the least negative ROI (−70.7%); Partner Referral takes 59% of spend ($7.27M) at −90.7%; Organic Search has the lowest ROI (−97.8%) and 14.5% response; Email responds at 8.7%.

Customer support (novatech_support_tickets.csv)

3,000 tickets: low 1,500, medium 1,050, high 400, critical 50.
Sentiment: neutral 1,953, negative 684 (22.8%), positive 304, missing 59.
Volume is spread across product areas (Notifications 601, Analytics Dashboard 597, Authentication 582, Mobile App 581, Billing 329, Data Pipeline 310). Total downtime is 47,255 minutes; Authentication (12,188) and Data Pipeline (11,461) account for half of it.
Support load is concentrated: ACCT-041 alone filed 334 tickets (11%), and the top 10 accounts (all Enterprise tier) account for 1,514 tickets (50%) against $154,709 in won deal value.
2,796 tickets map to accounts in the CRM.
Data notes: join fan-out (read before quoting any dashboard total)

account_id is not unique in any of the three tables, so joining deals to leads to tickets produces one row per deal × lead × ticket combination per account. This inflates any sum taken from the unified dataset.

Metric	Unified dataset / dashboard PDF	Source file
Total deal value	$100.9M	$707,201
Total campaign spend	$353.51M	$12.36M
Attributed revenue	$25.05M	$1.13M
Opportunity count	496 (distinct)	499
Total tickets (distinct)	2.79K	3,000 (2,796 mapped to CRM accounts)
Total downtime	1.24M min	47,255 min
Users affected	13.74M	758,557

Row-level counts, ratios and rankings are broadly usable, but additive totals from the unified dataset are not. The correct approach is to aggregate each source table separately, or pre-aggregate to account level before joining (one row per account per source).

Amazon Q

The Q answers recorded in the exploration log for counts match the source data: 499 deals (315 won / 184 lost), 2,240 leads, 27.2% response rate, 3,000 tickets, 50 critical. Use Q for quick ad-hoc questions and the dashboard for multi-filter and trend analysis. If the Topic sits on the unified dataset, summed measures (deal value, spend) will carry the fan-out issue above.

Tools

Amazon QuickSight (SPICE, joins, calculated fields, dashboards, filter controls) · Amazon Q in QuickSight (Topics, natural-language Q&A) · CSV source data.

Rubric coverage
Verify data quality and completeness across knowledge bases
Transform and join datasets with no-code data preparation
Design a multi-page interactive dashboard for business decisions
Configure AI-powered natural-language querying
Communicate data-driven insights to stakeholders

Built for the AWS AI & ML Scholarship / Udacity "Revenue Intelligence Dashboard" project.
