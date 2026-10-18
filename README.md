# hourly vps: how pay-by-the-hour billing actually works, what short jobs really cost, and when Vultr's capped hourly model beats a flat monthly plan

An hourly VPS is a server you pay for by the hour instead of by the month: deploy it when a job starts, destroy it when the job ends, and the meter only runs in between. A three-hour test costs cents. What actually decides your invoice, though, is rarely the headline rate. It's the rules around the meter: whether a partial hour gets rounded up, where the monthly cap sits, and whether a powered-off server keeps ticking. Product pages tend to mention those rules in passing, so this guide puts them front and center.

Vultr is the reference point here because its entire cloud compute line is metered hourly by default and capped at each plan's monthly price. You don't opt into hourly billing; it's simply how the platform charges. Along the way, the guide compares it against five dedicated hourly-billing brands on the details that move the invoice: rounding, caps, and what happens when you stop a server.

> **Quick pick:** for most short jobs, CI runs, client demos, weekend builds, Vultr's $5/mo Cloud Compute plan (1 vCPU, 1 GB RAM, 25 GB SSD, 1 TB traffic) is enough: **$0.007/hour**, and even a server you forget about for a month never costs more than $5. 👉 [See Vultr's hourly rates and current plans](https://www.vultr.com/pricing/?ref=7268283)

## What an hourly VPS is (and the billing rules that decide your invoice)

It's a normal virtual server, with dedicated vCPU, RAM, storage, its own IP and root access, and one difference on the invoice: instead of a flat monthly fee, a fixed hourly rate is deducted for every hour the server exists, until you delete it. Nothing else about the machine changes.

Four rules do all the work:

- Every existing hour bills. On Vultr and on every specialist compared below, a server is billed whether it's powered on or off, from deploy until deletion. Deleting it is the only thing that stops the meter.
- Partial hours get rounded. Vultr counts a started hour as a full hour, so a 10-minute job bills one hour.
- There's a monthly cap. Vultr meters hourly up to 672 hours (28 days) in a monthly period. After the cap, the rest of the period is free, so an always-on server pays exactly the monthly price and never more.
- Charges accrue hourly against your account balance, with your payment method charged as the balance runs down.

This suits work with a known end date: test runs, demos, migrations, CI runners, a weekend project. The specialists in this comparison build their entire product around that idea. Vultr treats it as the default billing mode for cloud compute, which means there's no separate plan list and no "hourly" checkbox to hunt for at checkout. ServerNet, for example, makes you tick an hourly billing box when ordering; on Vultr the meter is simply built in.

## Vultr's plans and hourly rates: the full list

All of Vultr's Cloud Compute plans are billed by the hour, from $0.004/hr on the smallest tier up to $0.952/hr, and every plan is capped at its own monthly price. The Regular Performance line looks like this:

| Plan | vCPU / RAM | Storage | Traffic | Hourly → monthly cap | Deploy |
| --- | --- | --- | --- | --- | --- |
| $2.50 | 1 vCPU / 512 MB | 10 GB SSD | 0.5 TB | $0.004 → $2.50/mo | [Deploy](https://www.vultr.com/pricing/?ref=7268283) |
| $3.50 | 1 vCPU / 512 MB | 10 GB SSD | 0.5 TB | $0.005 → $3.50/mo | [Deploy](https://www.vultr.com/pricing/?ref=7268283) |
| $5.00 | 1 vCPU / 1 GB | 25 GB SSD | 1 TB | $0.007 → $5.00/mo | [Deploy](https://www.vultr.com/pricing/?ref=7268283) |
| $10.00 | 1 vCPU / 2 GB | 55 GB SSD | 2 TB | $0.015 → $10.00/mo | [Deploy](https://www.vultr.com/pricing/?ref=7268283) |
| $20.00 | 2 vCPU / 4 GB | 80 GB SSD | 3 TB | $0.030 → $20.00/mo | [Deploy](https://www.vultr.com/pricing/?ref=7268283) |
| $40.00 | 2 vCPU / 8 GB | 160 GB SSD | 4 TB | $0.060 → $40.00/mo | [Deploy](https://www.vultr.com/pricing/?ref=7268283) |
| $80.00 | 4 vCPU / 16 GB | 320 GB SSD | 5 TB | $0.119 → $80.00/mo | [Deploy](https://www.vultr.com/pricing/?ref=7268283) |
| $160.00 | 8 vCPU / 32 GB | 640 GB SSD | 6 TB | $0.238 → $160.00/mo | [Deploy](https://www.vultr.com/pricing/?ref=7268283) |
| $320.00 | 16 vCPU / 64 GB | 1280 GB SSD | 7 TB | $0.476 → $320.00/mo | [Deploy](https://www.vultr.com/pricing/?ref=7268283) |
| $640.00 | 24 vCPU / 128 GB | 2560 GB SSD | 8 TB | $0.952 → $640.00/mo | [Deploy](https://www.vultr.com/pricing/?ref=7268283) |

One caveat on the cheapest row: the $2.50 tier includes no IPv4 address (IPv6 only). It's fine for scripts and internal tools, but anything that needs to be reachable over the regular internet starts at $3.50.

Two faster families sit alongside it:

| Plan | vCPU / RAM | High Frequency (NVMe) | High Performance | Hourly → cap |
| --- | --- | --- | --- | --- |
| $6 | 1 vCPU / 1 GB | 25 GB NVMe | 25 GB | $0.009 → $6/mo |
| $12 | 1 vCPU / 2 GB | 55 GB NVMe | 55 GB | $0.018 → $12/mo |
| $24 | 2 vCPU / 4 GB | 80 GB NVMe | 80 GB | $0.036 → $24/mo |
| $48 | 4 vCPU / 8 GB | 160 GB NVMe | 160 GB | $0.071 → $48/mo |
| $96 | 8 vCPU / 16 GB | 320 GB NVMe | 320 GB | $0.143 → $96/mo |
| $192 | 16 vCPU / 32 GB | 384 GB NVMe | 640 GB | $0.286 → $192/mo |

High Frequency pairs NVMe storage with higher clock speeds; High Performance runs on AMD EPYC CPUs. Traffic scales the same way on both, from 1 TB on the $6 tier to 6 TB at $192. Every plan also includes the same platform around it: 32 data center locations (New York, Los Angeles, Toronto, São Paulo, London, Frankfurt, Amsterdam, Tokyo, Singapore, Mumbai, Seoul, Sydney and Johannesburg among them), deploys that finish in about a minute, images for Ubuntu, Debian, Rocky, FreeBSD and Windows, and one-click marketplace apps like Docker and WordPress. Snapshots cost $0.05/GB/month while stored, and automatic backups add 20% to the instance price ($1/month on the $5 plan). 👉 [Browse Vultr's locations, images and full plan list](https://www.vultr.com/pricing/?ref=7268283)

For honest price context: X-ZoneServers and NODED.CLOUD both start at €0.0056/hour for a 2 GB, 1 vCPU plan with a €4/month cap, which undercuts Vultr's cheapest IPv4 tier. If bottom-dollar pricing for a few hours is the only thing that matters, they win that line item. Vultr's 2 GB plan at $0.015/hour, however, comes in below HourlyVPS's 2 GB tier ($0.02/hour), and it brings 32 locations and the platform features above instead of a single-region focus. Plan details do get refreshed from time to time, so the live pricing page is the source of truth before you order.

## What short jobs actually cost, rounding included

Most short jobs on Vultr bill in cents; here's the arithmetic at current Regular Performance rates:

| Job | Vultr plan | Hours | Total |
| --- | --- | --- | --- |
| CI pipeline run | $5 · 1 GB | 3 | ~$0.02 |
| Client demo | $10 · 2 GB | 6 | $0.09 |
| Load test on a big box | $40 · 8 GB | 3 | $0.18 |
| Weekend hackathon server | $20 · 4 GB | 48 | $1.44 |
| Two-week staging sprint | $10 · 2 GB | 336 | $5.04 |

Rounding deserves one honest note. Vultr rounds each started hour up to a full hour, so a 20-minute job bills a full hour. NODED.CLOUD meters per second and Vozora bills by the minute, which is technically fairer for sub-hour work. On a single job the difference is fractions of a cent. It starts to matter when your month is made of many tiny back-to-back jobs, each one restarting the hour; a per-second meter would split those, hour-rounded billing stacks them. A handful of multi-hour jobs per week? The difference is noise. 👉 [Check Vultr's hourly pricing against your own job lengths](https://www.vultr.com/pricing/?ref=7268283)

## Monthly caps: where the six providers really differ

A monthly cap is the ceiling that stops hourly charges within a billing period, and it's the biggest structural difference between the providers in this comparison:

| Provider | Bills by | Monthly cap | Cheapest hourly | Cheapest plan |
| --- | --- | --- | --- | --- |
| Vultr | hour (started hour = full hour) | yes, 672 h (28 days), then free | $0.007 | 1 vCPU / 1 GB / 25 GB · $5/mo cap |
| HourlyVPS | hour | yes, 500 h per billing period (~20.8 days) | $0.01 | 1 vCPU / 1 GB / 25 GB · $5/mo cap |
| ServerNet | hour | rates derived as monthly ÷ 720, so a full month ≈ the monthly price | €0.01 | 1 vCPU / 1 GB / 25 GB · Tehran |
| Vozora | minute | none; the monthly figure is a 730-hour estimate, not a cap | £0.011 (1.10p) | 1 vCPU / 2 GB / 30 GB NVMe · ~£7.99 est. |
| X-ZoneServers | hour | yes, stops at the flat monthly price | €0.0056 | 1 vCPU / 2 GB / 40 GB NVMe · €4/mo cap |
| NODED.CLOUD | second | yes, monthly cap | €0.0056 | 1 vCPU / 2 GB / 40 GB · €4/mo cap |

In practice, a cap is free insurance against the deploy-and-forget mistake: the worst case for a forgotten server is the monthly price. Vultr's cap lands after 28 days of metered time; HourlyVPS's lands after about 20.8 days, so its meter switches off slightly earlier in the month.

Vozora is the outlier. It has no cap, and its advertised monthly price is an estimate for a server kept alive through a 730-hour month. Run nonstop and you'll land near that estimate anyway; the gap opens up only if you expected a ceiling. Vozora does put a spend cap on the whole account, with alerts at 50, 80 and 90 percent, which catches runaway accounts even when individual servers keep metering. Entry barriers differ too: HourlyVPS wants a $5 minimum top-up, while ServerNet lets you start with about €0.24, just 24 hours of its cheapest plan rate.

## Powering off does not stop the meter

No. This is the rule that surprises people, and it's identical across Vultr and every specialist above: a powered-off server keeps billing, because its CPU, RAM, disk and IP addresses stay reserved for you. Destroying the instance is the only thing that ends the charges.

The workflow that avoids paying for idle machines:

1. Snapshot anything worth keeping. On Vultr that's $0.05/GB/month while stored, so a 25 GB image costs about $1.25/month to park.
2. Destroy the instance. Billing stops immediately, with the final partial hour counted. On Vultr this works from the panel or through the API, which makes it scriptable for CI pipelines.
3. Redeploy when you need the machine again. Deploys finish in about a minute, and you can restore from the snapshot.

The classic failure mode is "I'll just power it off for the weekend." The meter doesn't care about weekends. Build the destroy step into whatever script or runbook starts the server, and the problem never appears.

## Hourly vs monthly: which is cheaper?

Under roughly half a month of runtime, hourly wins. Past that the two converge, and on any provider with a monthly cap, a server that never switches off costs exactly the monthly price. On Vultr's $5 plan: 10 days of runtime (240 hours) costs $1.68, 15 days costs $2.52, a full month costs $5.00, the same figure the plan advertises.

There's also nothing to choose at checkout on Vultr. Every cloud compute instance is metered hourly with the cap built in, so "monthly" simply describes what an always-on instance ends up costing. That removes one decision from the ordering flow; the discipline that actually saves money is destroying servers you're done with.

## When an hourly VPS is the wrong tool

Skip it in three situations. First, always-on production sites: on a capped provider, an always-on hourly server costs the monthly price anyway, so hourly billing adds destroy-risk without saving money. Deploy it and leave it alone. Second, stateful servers with slow setup: a database holding real data or a carefully tuned stack costs you time on every rebuild, and while snapshots help, restore time can eat the savings. Hourly billing shines for disposable machines. Third, if your workload is dozens of tiny sub-hour jobs per day, per-second or per-minute metering (NODED.CLOUD, Vozora) tracks usage more precisely than hour-rounded billing.

Regional requirements can point elsewhere too. Vozora is a UK-focused provider, ServerNet spans 11 countries including Iran, and HourlyVPS runs from Istanbul. If one of those is exactly where your box needs to sit, a local specialist can be the shorter path even at a slightly higher rate.

## Hourly VPS FAQ

**Does powering off a Vultr instance stop billing?**
No. A stopped instance keeps its CPU, RAM, storage and IP reserved, so charges continue. Destroy the instance to stop the meter and deploy a fresh one when you need it.

**How is a partial hour billed?**
Vultr counts a started hour as a full hour, so a 10-minute job on the $5 plan bills $0.007. NODED.CLOUD meters per second and Vozora bills by the minute, which trims the rounding on sub-hour jobs.

**Is hourly-billed hardware weaker than the monthly plans?**
No. On Vultr the hourly meter applies to the same cloud compute plans and the same nodes; there's no separate hourly hardware. The specialists above work the same way, with hourly servers using identical hardware to their monthly offering.

**What happens to my data when I destroy the server?**
It's gone, and deletion can't be undone. Take a snapshot first ($0.05/GB/month on Vultr while stored) if there's any chance you'll want the machine back in a similar state.

**Where can I deploy?**
Vultr runs 32 data center locations, including New York, Los Angeles, Toronto, São Paulo, London, Frankfurt, Amsterdam, Tokyo, Singapore, Mumbai, Seoul, Sydney and Johannesburg. For comparison, X-ZoneServers covers 12 EU/US locations, NODED.CLOUD runs 13 POPs across Europe and the US, and ServerNet spans 11 countries.

## Picking between them: the short version

A few hours of compute scattered through the month: Vultr's $5 plan, destroyed after each job, bills in single-digit cents and can never exceed $5.

A two-to-four week project: the $10–$20 tiers land between roughly $5 and $10 for a fortnight of 24/7 use, with the cap as the safety net.

An always-on site or service: deploy the same Vultr plan and let the cap work; the monthly price is the ceiling either way.

The absolute cheapest 2 GB box for a few hours, per-second metering, or a specific region like the UK, Iran or Istanbul: the specialists covered above fit those narrower briefs better, and that's a perfectly good outcome. Match the tool to the job.

One last check before you deploy anything hourly: confirm what stops the meter (destroying the server, not powering it off), and snapshot anything you'd hate to rebuild. 👉 [Check Vultr's current hourly rates and deploy your first instance](https://bit.ly/VulTR)
