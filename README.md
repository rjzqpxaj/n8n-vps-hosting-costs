# n8n vps hosting: what self-hosting n8n really costs per month, and how to pick a VPS that fits your workflows

Self-hosting n8n doesn't need much: any Linux KVM VPS with 2 GB of RAM, root access, and Docker can run it, and a plan in the **$6–20/month** range covers most solo users and small teams. That's the whole pitch of `n8n vps hosting` searches — pay a flat server fee, run unlimited workflow executions, and stop feeding a per-execution bill. One of the established options here is GreenCloud (GreenCloudVPS), a hosting provider running since 2013 whose Ryzen KVM line starts at $6/month with NVMe storage and 10Gbps ports — you install n8n yourself via Docker, which takes about ten minutes.

If you're in a hurry, here's the short version:

> **Most people searching for n8n hosting should start with GreenCloud's RyzenKVM-2** (1 core, 2 GB RAM, 30 GB NVMe, $10/month flat, no contract). Light personal automation runs fine on the $6 RyzenKVM-1 (1 GB RAM). Heavy queue-mode setups want 8 GB. Unlike n8n Cloud's $24/month Starter plan (2,500 executions), a VPS has no execution cap — the software is free, you only pay for the server. 👉 [Check GreenCloud's current Ryzen KVM plans and prices](https://greencloudvps.com/billing/aff.php?aff=6679)

## What n8n actually demands from a VPS

n8n's community edition is free, open-source, and self-hosted via Docker. The server needs to stay online 24/7 because webhooks and cron triggers fire whether you're awake or not, and it needs port 5678 open for the web UI.

On sizing: a 1 GB instance handles a handful of simple workflows for one user. 2 GB is the practical sweet spot for most people — IONOS's cheapest n8n VPS ships with 4 GB, which tells you where the comfort zone starts. Queue mode (n8n's multi-worker scaling setup with PostgreSQL and Redis) is where you actually need 8 GB and up. Storage is rarely the bottleneck; 15–30 GB of NVMe holds the database and logs for years of typical use.

The economics are the real argument. n8n Cloud charges $24/month for Starter (2,500 executions) and $60/month for Pro (10,000 executions). A single workflow that triggers every five minutes burns 8,640 executions a month — more than double the Starter quota — before you've built anything interesting. A $6–10 VPS makes that same workflow a rounding error on your bill.

## GreenCloud's Ryzen KVM plans: the full lineup

GreenCloud organizes its Linux VPS offerings into several lines on its KVM VPS page: EPYC VDS (dedicated-resource virtual servers), Ryzen VPS, Budget KVM VPS, 10Gbps VPS, and Storage VPS. The line with fully published, current pricing on the official site is the Ryzen KVM series — Ryzen CPUs clocked at 3.5+ GHz with NVMe storage. Here it is in full:

| Plan | CPU | RAM | NVMe | Bandwidth (US/EU) | Price | Order |
| --- | --- | --- | --- | --- | --- | --- |
| RyzenKVM-1 | 1 core @ 3.5+ GHz | 1 GB | 15 GB | 1.5 TB | $6/mo | [Order RyzenKVM-1](https://greencloudvps.com/billing/aff.php?aff=6679) |
| RyzenKVM-2 | 1 core @ 3.5+ GHz | 2 GB | 30 GB | 2 TB | $10/mo | [Order RyzenKVM-2](https://greencloudvps.com/billing/aff.php?aff=6679) |
| RyzenKVM-3 | 2 cores @ 3.5+ GHz | 4 GB | 50 GB | 4 TB | $20/mo | [Order RyzenKVM-3](https://greencloudvps.com/billing/aff.php?aff=6679) |
| RyzenKVM-4 | 4 cores @ 3.5+ GHz | 8 GB | 80 GB | 8 TB | $40/mo | [Order RyzenKVM-4](https://greencloudvps.com/billing/aff.php?aff=6679) |
| RyzenKVM-5 | 8 cores @ 3.5+ GHz | 16 GB | 150 GB | 12 TB | $80/mo | [Order RyzenKVM-5](https://greencloudvps.com/billing/aff.php?aff=6679) |

All plans run Linux on KVM virtualization with a 10Gbps port, full root access via SSH, and IPv6 included. The Budget KVM line (SSD instead of NVMe) mirrors the same price ladder — $6/$10/$20/$40/$80 for 1/2/4/8/16 GB — according to independent reviews, though you should treat those figures as secondhand.

Two useful details the pricing table doesn't show. First, you can customize a plan after ordering: extra RAM, CPU cores, or storage cost **$3 per unit per month**, capped at doubling your original configuration. A 4 GB RyzenKVM-3 can become an 8 GB machine for +$12/month, which is handy when a workflow outgrows its plan mid-life. Second, the Ryzen line is Linux-only; if you need Windows for other tooling, GreenCloud sells separate Windows VPS plans (from around $8/month per its own promotional material), but n8n belongs on the Linux boxes.

👉 [Browse GreenCloud's full plan list and current pricing](https://greencloudvps.com/billing/aff.php?aff=6679)

## The detail most reviews skip: APAC bandwidth is half of US/EU

This one matters if you're hosting n8n for Asian-market workflows. On GreenCloud's official Ryzen KVM page, the same-priced plans ship with **double the bandwidth in US/EU locations versus APAC**. RyzenKVM-1 gets 1.5 TB of monthly traffic in Los Angeles, Chicago, Amsterdam, and so on — but only 750 GB in Singapore, Tokyo, Hong Kong, or Vietnam. RyzenKVM-2 is 2 TB versus 1 TB.

For n8n specifically this is usually a non-issue — API calls and webhooks generate tiny payloads, and even the 750 GB APAC allowance is far beyond what a typical automation stack consumes. It becomes relevant only if your workflows shuttle files around. But it's a real asymmetry that most comparison articles flatten into a single "bandwidth" column, and it should shape your location choice: pick the APAC node for latency to Asian APIs (and note that Hanoi and Ho Chi Minh City are locations you rarely see at this price tier), pick US/EU if raw traffic headroom matters more.

## What a year of self-hosted n8n actually costs

Sticker prices mislead here, because several competitors run intro discounts that expire. Running the numbers on verified published prices:

| Provider / plan | Config | Price structure | First-year cost |
| --- | --- | --- | --- |
| GreenCloud RyzenKVM-2 | 1 core / 2 GB / 30 GB NVMe | $10/mo flat, monthly billing | $120 |
| IONOS VPS M+ (n8n preinstalled) | 2 vCores / 4 GB / 120 GB NVMe | $5/mo for 3 months, then $14/mo, 1-year contract | $141 |
| Bluehost n8n VPS Standard | 2 vCPU / 4 GB / 80 GB SSD | $4.69/mo starting, self-managed | ~$56–$60 |
| OBHost 2 GB (Germany) | 1 vCPU / 2 GB / 60 GB | €8.02/mo effective, but 36 months prepaid upfront (€288.86) | €288.86 lump sum |
| n8n Cloud Starter | Managed SaaS | $24/mo, 2,500 executions | $288 |

A few things fall out of this table. The IONOS deal looks cheaper than GreenCloud at $5/month, but it's a three-month teaser on a one-year contract — it renews at $14/month, or $168 in year two, versus GreenCloud's unchanged $120. n8n Cloud costs roughly 2.4× the GreenCloud box per year and starts billing you extra the moment your executions pile up. OBHost's effective monthly rate is decent but demands €288.86 on day one for a three-year lock-in, which is a lot of faith to put in a provider you haven't used.

Bluehost's $4.69 headline deserves a caveat from its own product page: bandwidth is unmetered, but sustained usage above 25% of system resources for more than 90 seconds triggers an overuse notice. Bursty n8n workloads — a batch job processing a few hundred records — can brush against that. GreenCloud's KVM plans advertise dedicated resources with no equivalent clause in their published plan pages.

One honesty note on the provider itself: GreenCloud has genuine community credentials — it claims the #1 Top Provider and #1 Most Stable Provider awards on the LowEndTalk forum, and its official pages cite a 99.99% network and power uptime record backed by a public status page. But a 2026 editorial review at itechguides ranked it 22nd of 23 in US VPS hosting with a 7.6/10 score, mostly reflecting its budget positioning rather than complaints about the product. It's a budget host with unusually good community word-of-mouth, not an enterprise cloud, and it's fair to know both things before ordering.

## Setting up n8n on a GreenCloud VPS: the actual steps

GreenCloud doesn't preinstall n8n — its VPS plans are general-purpose Linux machines, so you handle the install. If "preinstalled, zero setup" is your priority, IONOS and Bluehost both ship n8n-ready images, and their prices are in the table above. If you're comfortable pasting two commands into a terminal, here's the whole process:

1. **Order a plan and pick your location.** Choose a Linux OS at checkout (most n8n guides assume Ubuntu or Debian), then SSH in as root.
2. **Install Docker** from Docker's official repository script, then run n8n with a persistent volume:

bash
docker run -d --restart unless-stopped \
  --name n8n \
  -p 5678:5678 \
  -v n8n_data:/home/node/.n8n \
  n8nio/n8n


3. **Open port 5678** in your firewall. You can reach the setup wizard at `http://your-server-ip:5678` and create the owner account. For anything beyond testing, put n8n behind a domain with a reverse proxy (Caddy or Nginx) and an SSL certificate so it runs on HTTPS.
4. **Scale when needed.** For high-volume setups, switch to Docker Compose with PostgreSQL and Redis in queue mode so multiple workers share the load.

The restart policy in that command is the piece beginners forget — without it, a server reboot silently takes your automations down until you notice the missed webhooks.

👉 [Order the RyzenKVM-2 plan (1 core / 2 GB / 30 GB NVMe, $10/mo)](https://greencloudvps.com/billing/aff.php?aff=6679)

## FAQ

**How much RAM does a self-hosted n8n server need?** 1 GB works for a light, single-user setup; 2 GB is the sensible default; 4 GB+ is where queue mode and dozens of active workflows live comfortably.

**Does GreenCloud support n8n officially?** Not as a managed product — you install and maintain it yourself on a standard KVM VPS. There are no execution caps or n8n-specific restrictions imposed by the host; what you run on your root-access machine is your business.

**What about refunds and payment methods?** GreenCloud's official promotional material lists a 7-day money-back guarantee and payment options that include PayPal, credit cards, Bitcoin, Alipay, and WeChat Pay. Verify the current terms on the checkout page before paying, since these details come from the provider's own listings rather than a standalone policy page I could independently confirm.

**Will my workflows hit any usage limits?** GreenCloud's plan limits are bandwidth and hardware, not executions. Compare that with Bluehost's documented 25%-for-90-seconds resource clause and n8n Cloud's hard execution quotas — for automation specifically, the flat VPS model has fewer tripwires.

## Which plan should you actually pick

Running a few personal automations — Telegram bots, RSS-to-email, a couple of CRM syncs? The $6 RyzenKVM-1 is enough; don't buy RAM you won't use. Production workflows with webhooks you actually depend on, or an AI agent pipeline hitting an OpenAI-style API? RyzenKVM-2 at $10/month is the one to get, and it undercuts the IONOS renewal price while carrying no contract. Queue mode, many workers, or company-wide automation? RyzenKVM-4 (8 GB, $40) — and remember you can start at RyzenKVM-3 and add 4 GB of RAM for $12/month when you get there. Want everything preinstalled and are willing to trade a contract for convenience? That's the IONOS/Bluehost territory described above; they're legitimately good at that one thing.

One last check before you order: confirm the datacenter location matches where your APIs live, and remember the APAC bandwidth halving if your workflows move files around. Otherwise, 👉 [start with the $6 RyzenKVM-1 or $10 RyzenKVM-2 plan on GreenCloud](https://greencloudvps.com/billing/aff.php?aff=6679) and have Docker running n8n before your coffee goes cold.
