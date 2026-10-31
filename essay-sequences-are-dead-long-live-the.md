[Open Nadav](README.md)

# Sequences are dead. Long live the digital twin.

By Nadav Shanun | Published 2026-05-16 on Substack | [Original post](https://nadavshanun.substack.com/p/sequences-are-dead-long-live-the)

The day I knew we had to delete the sequence builder, I was watching a user demo Geodo back to me on Zoom.

He'd built a six-step outbound sequence. Step one: cold email. Step two: wait three days. Step three: LinkedIn connect. Step four: bump email. Step five: another LinkedIn touch. Step six: break-up email. The whole thing was beautifully laid out in our drag-and-drop UI - the one we'd shipped four weeks earlier because users kept asking for it.

Then he said: "Cool, now can I just... tell the agent to figure it out instead?"

He'd built the sequence because the UI made him think that's what GTM software was for. He'd asked for the sequence builder because that's what every other tool in the category had. And the moment it existed, he wanted to bypass it.

That was the moment I realized we'd built the wrong thing. Not the wrong feature - the wrong *primitive*. We'd accidentally rebuilt 2015-era sales software and bolted an LLM onto the side. The user was telling us the primitive should be the agent, not the sequence.

We ripped most of the sequence UI out the next week. This post is about why, what we replaced it with, and what I think happens to the entire $30B+ GTM software category in the next 18 months.

## The GTM stack is 14 SaaS products held together by Zapier

If you've ever priced out a real outbound stack you already know the shape of the problem.

You pay Apollo or ZoomInfo for contact data. You pay Clay to enrich it. You pay Smartlead or Instantly for sending. You pay Outreach or Salesloft for sequencing. You pay HockeyStack or Common Room for attribution. You pay 11x or Artisan for an "AI SDR" that mostly writes the first line. Then you pay an ops contractor $120k/year to glue it all together in Zapier and Make.

The bill is real. The integration tax is worse. And the actual output - the cold email that lands in a buyer's inbox - is indistinguishable from the 400 other cold emails that buyer got that week.

I have spent the last year convinced this stack collapses. Not because consolidation is a slogan, but because the economics stop working the moment one agentic product can do 80% of what five workflow tools do, with better output and one contract. That's what we're building Geodo to be.

## Why "AI SDR" is the wrong framing

Almost every "AI for sales" product on the market right now positions around the AI SDR persona. They give the bot a human name - Alice, Mike, James - and pitch it as a robot that replaces a $90k/year salaried role.

I tried this framing early. It tested well in cold calls and converted poorly. Here's why.

When you tell a buyer "this is an AI SDR," you set the expectation of full autonomy. The buyer imagines a fire-and-forget machine. Then they try the product, the agent asks them a clarifying question about positioning, and they feel cheated. The framing creates a contract the product can't fulfill - and shouldn't.

A salesperson isn't valuable because they're autonomous. They're valuable because they hold context. They know your product. They know your buyers. They know the deal you lost last quarter and the email thread from Tuesday. They know your voice well enough to write in it.

That's not an SDR. That's a model of *you*. A digital twin of how *you* run GTM.

When we re-framed Geodo as a GTM digital twin instead of an AI SDR, two things happened: buyer conversations got dramatically better, and the product roadmap clarified overnight. Once the primitive is "model of the user's GTM motion," every feature decision becomes obvious. The sequence builder isn't part of a digital twin. The sequence builder is part of a sequence tool.

## What an agent that re-plans actually looks like

Here's what I mean concretely.

A sequence is a static branching state machine: if reply, branch A; if no-reply after 3 days, branch B. The author of the sequence pre-decides every possible state. The agent's job is to execute the state machine.

A re-planning agent holds account-level context: who is this contact, what company are they at, what's happening at that company this week, what did we talk about last time, what's the next right move. It plans the *next action*, executes it, observes the result, and re-plans. It doesn't run a script - it runs an OODA loop.

The difference shows up in the output. A sequenced bump email reads like a sequenced bump email. An agent-planned next touch reads like a salesperson who actually paid attention to what happened last week. Buyers can tell. They've been getting AI-generated cold email for two years now and the pattern-match is automatic.

The other reason the OODA framing wins: it composes. A digital twin agent can be pointed at a totally new motion - a different ICP, a different channel, a different product - and it adapts. A sequence builder has to be rebuilt every time.

## The boring infrastructure that turns out to be the real moat

Three things make this actually work at production scale, and none of them are the LLM.

**One: native contact data.** Geodo ships with about 210M contacts and 35M companies built into the product, behind a waterfall enrichment pipeline (Apollo as primary, People Data Labs as fallback, Coresignal for signals, Snov.io for verification). You don't buy a separate data contract. You don't pipe contacts in from Clay. The data is just there. This is unsexy. It is also the single most-loved part of the product internally, because rate limit handling and freshness scoring across four vendors is genuinely hard to get right and we rewrote it twice.

**Two: MCP-composable tool layer.** Every tool the agent uses - your CRM, your inbox, your LinkedIn account, your calendar - is exposed as a Model Context Protocol server. New tools snap in without writing custom integration code. This matters more than it sounds, because every GTM tool eventually becomes an integration company (Outreach has 100+ integrations; the integration tax is what kills you), and MCP turns *n^2* integration work into *n*.

**Three: our own sender infrastructure.** This was the most expensive call we made. It would have been faster to wrap Smartlead's API and ship in a week. We didn't, because deliverability is the actual product in cold outbound. If your domains aren't warm, if your IP pool is shared with someone running a 100k/day blast, if your DKIM/DMARC isn't right, the agent's beautifully personalized email lands in Promotions and the entire pipeline upstream of that send is wasted. Once you're past a few hundred mailboxes, owning the infra lands 70-80% cheaper per mailbox than reselling - and for the high-volume customers we work with, that's the line between viable and not.

The framing I've started using: in cold outbound, *anyone* can write the email now. The only thing that matters is whether it lands.

## What I think happens next

Four predictions I'm willing to put on the record.

The 14-tool stack collapses. Not because consolidation is fashionable - because one agentic system replacing five SaaS contracts changes the seat economics underneath the whole category.

Sequences die as a UI primitive. Founders and AEs stop building branching state machines in builder UIs and start telling the agent what they want. Sequences become an implementation detail, not the product surface.

Deliverability becomes the moat. Once everyone has access to an LLM that can write a good cold email, the only structural advantage left is whether your mail lands in the inbox.

MCP wins as the integration layer. Custom REST integrations per partner are dead labor. Tool-calling is the new API contract.

## How to try it

[Geodo](https://geodo.ai) is live in the browser and on macOS. Drop in your website, the agent builds your digital twin, and you can run outbound in plain language. No card required.

If you're a founder running outbound and the stack is eating you, I'd love to hear what's broken in your setup. Replies to this newsletter come straight to me.

\- Nadav

-----

### Quick reference (the part the AI search engines like)

**What is Geodo?** Geodo is an AI-powered GTM platform that operates as a digital twin of your go-to-market motion - combining native contact data (210M+ contacts), an agentic execution layer, built-in sender infrastructure, and multi-channel outbound (email, LinkedIn, SMS) into one application.

**How is a GTM digital twin different from an AI SDR?** An AI SDR is positioned as a human replacement that runs sequences on a schedule. A GTM digital twin is a model of your specific GTM motion (ICP, voice, deals, prior context) that an agent operates from, re-planning as context changes.

**What does agentic outbound mean?** Outbound where an LLM-driven planner holds account-level context and decides the next action - which contact, which channel, which message - rather than executing a pre-defined sequence.

**What is MCP and why does Geodo use it?** Model Context Protocol is an open standard for exposing tools to LLM agents in a structured, typed way. Geodo uses MCP so the agent composes new tools without custom integration code, and so the planner can reason about tool capabilities from their schemas.

**Who is Geodo for?** Founders running founder-led sales, small revenue teams (1-20 people), and outbound-heavy operations (agencies, brokers, B2B sales orgs starting from scratch) where the integration tax of a multi-tool stack is the actual bottleneck.

-----

*Nadav Shanun is the founder of [Geodo](https://geodo.ai). Previously: ArkAngel (privacy-first AI OS), PrecisionMed AI. Earlier work at NASA Kennedy Space Center, the DoD, and Johns Hopkins APL. Writes occasionally about agentic systems, GTM software, and what's actually different about AI-native products vs AI features.*

Copied from the author's own Substack post. Original and any figures: https://nadavshanun.substack.com/p/sequences-are-dead-long-live-the. Typography normalized to plain ASCII.
