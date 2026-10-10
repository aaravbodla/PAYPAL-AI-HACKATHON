# PAYPAL-AI-HACKATHON
PAYPAL AI HACKATHON Sai and Aarav 

The short version of the hackathon: you have until Thursday, Nov 12 at 1:00 p.m. Phoenix time (noon Pacific) to build a working app that centrally uses PayPal’s sandbox plus an AI tool or model of your choice. That’s about 4.5 weeks from today. There are no fixed tracks, so the idea is entirely open.
What judges score (five equal criteria): technical implementation, design, potential impact, innovation, and presentation. Ties break in that order, so a working, non-trivial integration matters most. Stage 1 is pass/fail on whether PayPal is genuinely central and the project fits the theme.
Where the money is: Besides 1st/2nd/3rd ($12k/$8k/$5k), there are five $5k honorable mentions, including Best Use of Agentic Commerce and Best Use of PayPal + AI. A project can win one main prize plus one sponsor prize, so using a sponsor tool like AG Grid, Channel3, or Render adds a second shot at winning.
Constraints that shape ideas: it needs to be a working demo (not a mockup), have a public repo with an open-source license, include a YouTube video under 3 minutes, and stay live and free for judges through Dec 15.
Idea directions
Agentic commerce (aligned with a named prize)
A shopping agent that finds products (Channel3 API gives 20k free searches), compares them, and checks out via PayPal sandbox with spending limits and approval steps.
A “household purchasing agent” that watches for restocks or price drops and buys within a budget you set.
Money movement for small businesses
A freelancer or contractor assistant that turns a chat message or email into an invoice, tracks payment, and chases late payers.
Automated payouts: upload timesheets or receipts, AI validates and pays a crew via Payouts.
AI dispute helper that gathers evidence and drafts responses using the Disputes API.
Consumer experiences
Receipt-photo bill splitting: snap the receipt, AI itemizes it, and PayPal requests go out to friends.
Chat-based money requests (“ask Sam for $20 for pizza”).
Creator tipping with AI-personalized thank-yous.
Social impact
Accessible donations (voice-driven, multilingual) or community funding pools with AI-generated transparency reports.
Mutual-aid or rent-sharing tools for neighbors and co-ops.
Dashboard-heavy (pairs with AG Grid or Bryntum prizes)
AI-powered cash-flow dashboard for a small shop using PayPal transaction data, with natural-language queries (“what were my slowest days?”).
Questions to narrow it down
Solo or team? And what’s everyone’s skill mix (frontend, backend, ML)?
Stack preference? Language and framework you already move fast in.
Which excites you more: a flashy agent demo, a practical business tool, or something with social good?
Starting fresh or extending an existing project? If existing, it has to be significantly updated after Oct 1.
How much time per week can you realistically give it?

---

# PayPal AI Hackathon — Participant Reference

Extracted and checked on **October 10, 2026** from the official Devpost rules, overview, FAQs, resources, and sponsor resource pages.

> This is a structured summary, not a replacement for the official rules or legal advice. The official rules control if other hackathon materials conflict, and the organizers may amend them. Recheck before submitting.

## 1. Event overview

| Item | Details |
|---|---|
| Name | PayPal AI Hackathon |
| Tagline | Build what’s next with PayPal and AI |
| Sponsor | PayPal Inc., 2211 North First Street, San Jose, California 95131, United States |
| Administrator | Devpost, Inc., 250 Broadway, Floor 24, New York, NY 10007 |
| Format | Online |
| Core challenge | Build or significantly update an application that centrally integrates the PayPal developer platform in its free sandbox environment together with an AI tool, model, or platform of your choice. |
| Formal tracks | No prescribed problem statements and no fixed tracks. Award categories are not mandatory project tracks. |
| Cost to enter | No purchase or payment is necessary to enter or win; a purchase does not improve chances of winning. |
| Advertised prizes | $67,500 in cash, plus $2,250 in Render award credits and APIMatic subscription awards. See the detailed prize table below. |

Sources: overview and official rules.[^1](https://paypalaihackathon.devpost.com/)[^2](https://paypalaihackathon.devpost.com/rules)

## 2. Dates and deadlines

The official rules specify **Pacific Time (PT)**. Arizona/Phoenix remains on MST year-round: it matches Pacific daylight time in October but is one hour ahead of Pacific standard time on the November and December dates below.

| Milestone | Official time — Pacific | Arizona/Phoenix time — MST |
|---|---|---|
| Submission period opens | Thursday, October 1, 2026, 9:15 a.m. | October 1, 2026, 9:15 a.m. |
| **Submission deadline** | **Thursday, November 12, 2026, noon** | **November 12, 2026, 1:00 p.m.** |
| Judging begins | Tuesday, December 1, 2026, 2:00 p.m. | December 1, 2026, 3:00 p.m. |
| Judging ends | Tuesday, December 15, 2026, 8:00 a.m. | December 15, 2026, 9:00 a.m. |
| Winners announced | On or around Monday, December 21, 2026, 2:00 p.m. | On or around December 21, 2026, 3:00 p.m. |

The Devpost banner displays the deadline as November 12 at 1:00 p.m. MST, consistent with the official noon-Pacific deadline. The Phoenix column is a time-zone conversion of the rules, not a separate organizer deadline.[^2](https://paypalaihackathon.devpost.com/rules)

## 3. Eligibility and teams

### Who may enter

- Individuals who have reached the age of majority where they reside **at the time of entry**.
- Teams consisting of eligible individuals.
- Organizations, including corporations, nonprofits, LLCs, partnerships, and other legal entities, that exist and are organized or incorporated at entry.
- The FAQ states there is **no maximum team size**.
- An eligible individual may join more than one team or organization and may also enter individually.
- Every team or organization must appoint and authorize one eligible **Representative** to enter and act on its behalf.[^2](https://paypalaihackathon.devpost.com/rules)[^3](https://paypalaihackathon.devpost.com/details/faqs)

### Who may not enter

- Residents of, or organizations domiciled in, jurisdictions where U.S. or local law prohibits participation or receipt of a prize. The rules explicitly name **Brazil, Quebec, Russia, Crimea, Cuba, Iran, and North Korea**, plus any country comprehensively sanctioned by the U.S. Treasury’s Office of Foreign Assets Control. This is not an exhaustive list.
- Organizations involved in designing, producing, paid promotion, executing, or distributing the hackathon, including the Sponsor and Administrator (“Promotion Entities”).
- Employees, representatives, or agents of those entities, and their immediate family or household members.
- Other individuals involved in designing, producing, promoting, executing, or distributing the event, and their immediate family or household members.
- Judges, companies or individuals employing a judge, and relevant parent companies, subsidiaries, or affiliates of excluded organizations.
- Anyone whose participation creates a real or apparent conflict of interest, as determined by the Sponsor or Administrator.

The rules define immediate family to include spouses, children/stepchildren, parents/stepparents, and siblings/stepsiblings. Household members include people sharing the same residence for at least three months of the year. Registering on Devpost does not override eligibility restrictions.[^2](https://paypalaihackathon.devpost.com/rules)[^3](https://paypalaihackathon.devpost.com/details/faqs)

## 4. Required technology and project scope

### Mandatory

1. An application integrating the **PayPal developer platform using the free sandbox environment**.
2. An **AI tool, model, or platform of your choice**.
3. PayPal integration must be **central to the project**, not merely mentioned.
4. A working, installable application that runs consistently on its intended platform and functions as shown in its video and description.[^2](https://paypalaihackathon.devpost.com/rules)

The rules include PayPal services such as Venmo, Braintree, Xoom, Honey, Zettle, Paidy, Hyperwallet, Simility, Swift Financial, and others. This is an allowed-services list, not a promise that every service has identical sandbox access or regional availability.[^2](https://paypalaihackathon.devpost.com/rules)

### Optional sponsor tools

AG Grid, APIMatic, Astropods, Bryntum, Channel3, Elastic, KERNEL, Postman, Render, and Zapier are optional. You may use multiple sponsor tools. Sponsor-prize entries must satisfy the relevant tool-use conditions; the main hackathon does not require one of these partners.[^2](https://paypalaihackathon.devpost.com/rules)[^3](https://paypalaihackathon.devpost.com/details/faqs)

### New versus existing work

- New projects are allowed.
- Projects that existed before October 1, 2026 must be **significantly updated after the submission period starts**.
- Explain the significant updates in the submission. The Sponsor and Administrator determine what qualifies at their sole discretion.
- A project must not have been developed, or derived from a project developed, with financial or preferential support from the Sponsor or Administrator. Examples include development funding/investment, contract development, or a commercial license received from them before the submission period ends. Ask the organizer about any potentially applicable relationship.[^2](https://paypalaihackathon.devpost.com/rules)

### Third-party components and ownership

- Be authorized to use third-party SDKs, APIs, and data under their applicable terms.
- The original submission work must belong to the entrant/team/organization and must not infringe others’ IP, contractual, privacy, or other rights.
- Open-source software and hardware are allowed when their licenses are followed and the project enhances/builds upon the underlying product.
- Contracted technical assistance is allowed only under the ownership and originality conditions stated in the rules.[^2](https://paypalaihackathon.devpost.com/rules)

## 5. Required submission materials

| Material | Requirement |
|---|---|
| Devpost entry | Register and complete all required fields on the submission form during the submission period. |
| Project description | Explain the features and functionality. The overview also asks entrants to identify the AI tools and models used. |
| Functional demo access | Either complete repository setup/run instructions that let judges build and run the project, **or** a hosted demo URL. Public hosting is not mandatory. |
| Public repository | GitHub, Bitbucket, or GitLab URL; include all necessary source code, assets, and instructions. |
| Open-source license | Include a license file that is detectable and visible at the top of the repository page, in its About area. |
| Demo video | Aim for **strictly under three minutes**. The rules say it “should be less than” three minutes and judges are not required to watch beyond three minutes. Show the project actually functioning on its intended device/platform. |
| Video publication | Upload to **YouTube**, make it **publicly visible**, and supply its link in the submission form. |
| Existing-project explanation | If applicable, explain significant updates made during the submission period. |
| Language | English, or full English translations of the video, description, testing instructions, and all other submission materials. |

A design mockup, static prototype, or non-functional demo does **not** satisfy the functional-demo requirement. The video must not use third-party trademarks, copyrighted music, or other protected material without permission.[^1](https://paypalaihackathon.devpost.com/)[^2](https://paypalaihackathon.devpost.com/rules)

### Testing and access

- Provide a working website, functioning demo, or test build.
- If the website is private, include test login credentials in the testing instructions.
- The project must remain available **free of charge and without restriction to the Sponsor, Administrator, and judges until judging ends on December 15, 2026**.
- The rules request any test-account credentials, API keys, or sandbox details necessary to test the project, plus clear special-access instructions in the README or description.
- Judges are not required to run it; they may evaluate only the submitted text, images, and video.
- If proprietary or uncommon third-party hardware is necessary, the organizers may request physical access. Smartphones, tablets, and desktop computers are distinguished from uncommon hardware in this provision.[^2](https://paypalaihackathon.devpost.com/rules)

**Practical security recommendation — not an additional rule:** Do not commit production secrets, unrestricted API keys, personal login credentials, or real payment data to the public repository. Use dedicated sandbox/test accounts, environment-variable setup, a hosted sandbox demo, or an organizer-approved secure access method. If the credential-sharing wording is unclear for your implementation, obtain written clarification before the deadline.

**Practical availability recommendation — not an additional rule:** Check that sponsor trials, credits, hosting, and API access last through December 15; a trial that covers development may expire before judging ends.

### Multiple submissions and modifications

- Multiple entries are permitted only if each is unique and substantially different from the others.
- Draft submissions may be saved before the deadline.
- Once the submission period ends, you may not alter the submission, although you may continue updating the project in your Devpost portfolio.
- The organizers may permit narrowly scoped post-deadline changes to remove infringing, personally identifiable, or inappropriate material. The submission must otherwise remain substantively the same.[^2](https://paypalaihackathon.devpost.com/rules)

## 6. Judging

### Stage 1 — pass/fail

Baseline viability: the project must reasonably fit the theme and reasonably apply the required APIs/SDKs.[^2](https://paypalaihackathon.devpost.com/rules)

### Stage 2 — five equally weighted criteria

| Criterion | What the judges assess |
|---|---|
| Technological Implementation | Thorough, skillful use of PayPal and AI; genuine effort and a working, non-trivial implementation. |
| Design | A complete, coherent product experience, not just a technical proof of concept. |
| Potential Impact | A credible, specific problem and audience, with a demonstrated solution that actually addresses that problem. |
| Innovation/Idea | Creativity, novelty, and differentiation from existing concepts. |
| Presentation | Clear end-to-end demonstration, explanation of problem/audience/value, and an easy-to-follow pitch. |

Ties are broken by comparing criterion scores in the listed order, beginning with Technological Implementation. If all applicable criteria remain tied, the judges vote. The organizers reserve discretion over judging methods, which may include expert panels, peer review, automated AI-driven analysis, or combinations; judge panels and rounds may change.[^2](https://paypalaihackathon.devpost.com/rules)

## 7. Prizes

All cash amounts are in **USD**. Quantities below follow the official rules, section 8.[^2](https://paypalaihackathon.devpost.com/rules)

### Grand prizes

| Prize | Award per winner | Winners |
|---|---:|---:|
| 1st Place | $12,000 cash | 1 |
| 2nd Place | $8,000 cash | 1 |
| 3rd Place | $5,000 cash | 1 |

### Honorable mentions

| Prize | Award per winner | Winners |
|---|---:|---:|
| Most Creative | $5,000 cash | 1 |
| Most Impactful | $5,000 cash | 1 |
| Best Demo Delivery | $5,000 cash | 1 |
| Best Use of PayPal + AI | $5,000 cash | 1 |
| Best Use of Agentic Commerce | $5,000 cash | 1 |

### Sponsor prizes

| Prize | Award per winner | Winners | Rules’ eligibility condition |
|---|---|---:|---|
| Best Use of AG Grid — 1st Place | $5,000 cash | 1 | Eligible project using AG Grid tools |
| Best Use of AG Grid — 2nd Place | $2,000 cash | 1 | Eligible project using AG Grid tools |
| Best Use of AG Grid — 3rd Place | $1,000 cash | **3** | Eligible project using AG Grid tools |
| Best Use of APIMatic | $1,000 cash + 6 months of APIMatic Business | 3 | Eligible project using APIMatic tools |
| Best Use of Bryntum | $1,000 cash | 3 | Eligible project using Bryntum tools |
| Best Use of Channel3 | $1,500 cash | 1 | Eligible project using Channel3 tools |
| Best Use of Render — 1st Place | $1,000 Render credits | 1 | Eligible project using Render tools |
| Best Use of Render — 2nd Place | $750 Render credits | 1 | Eligible project using Render tools |
| Best Use of Render — 3rd Place | $500 Render credits | 1 | Eligible project using Render tools |

The AG Grid third-place category has **three winners**, not one. The cash table sums to $67,500; Render awards add $2,250 in credits, giving $69,750 before assigning any value to APIMatic subscriptions. This is a calculation from the listed awards, not a guarantee of payout if no qualifying entries exist.[^1](https://paypalaihackathon.devpost.com/)[^2](https://paypalaihackathon.devpost.com/rules)

### Award limits and payment conditions

- A project can win up to **one Grand Prize + one Sponsor Prize**, **or one Honorable Mention + one Sponsor Prize**.
- It cannot combine a Grand Prize with an Honorable Mention or collect multiple Sponsor Prizes. Using several sponsor tools may make it eligible for consideration in several categories, but the award limit still applies.
- Winner status is contingent on verification of identity, qualifications, and role in creating the submission, and completion of required prize forms.
- Required forms must be returned within **10 business days** after they are sent.
- Prizes are delivered within **60 days after receipt of completed required forms**.
- Payment goes to the individual entrant, team Representative, or organization. The Representative is responsible for distributing a team award.
- Winners are responsible for applicable taxes, banking/currency-exchange/wiring fees, and relevant reporting obligations. Tax forms may include W-9 for U.S. residents or W-8BEN for non-U.S. residents; withholding may apply.
- Prizes are non-transferable by winners; the Sponsor may substitute equal-or-greater-value prizes and may decline to award a category without eligible entries.[^2](https://paypalaihackathon.devpost.com/rules)[^3](https://paypalaihackathon.devpost.com/details/faqs)

## 8. Sponsor access, credits, trials, and guidance

Offers below are what the linked resource pages list as of extraction. Actual redemption, account eligibility, provider terms, expiry, and usage billing should be checked before use. These are **participant offers**, separate from winner prizes unless explicitly stated.

| Sponsor | Access or offer | Claim/resource link and conditions |
|---|---|---|
| AG Grid | Free 45-day AG Studio trial | [Trial/pricing](https://www.ag-grid.com/studio/license-pricing/). Resource guidance emphasizes dashboard polish, custom widgets/theming/layout, and the Studio Agent Framework. A [PayPal + AG Grid starter](https://github.com/paypaldev/hackathon-paypal-ag-grid-boilerplate) is linked. [Source](https://paypalaihackathon.devpost.com/details/aggrid) |
| APIMatic | 1 month of Basic for participants using its Context Plugin to build their PayPal integration; 6 months of Business for qualifying winning teams | [Claim form](https://docs.google.com/forms/d/e/1FAIpQLScc2oCgAFACm7d6H3R4mgzv-O34DdyVnLIBNiwmUNggr_-hhA/viewform). The rules separately list the cash/subscription sponsor award. [Source](https://paypalaihackathon.devpost.com/details/apimatic) |
| Astropods | Free account; no explicit special credit amount or trial extension listed | [Website/sign-up](https://astropods.com/). [Source](https://paypalaihackathon.devpost.com/details/astropods) |
| Bryntum | 45-day trial | [Download](https://bryntum.com/download/). Sponsor guidance focuses on meaningful use of Gantt, Scheduler, and/or Calendar. [Source](https://paypalaihackathon.devpost.com/details/bryntum) |
| Channel3 | 20,000 free API credits per participant key, described as 20,000 product searches; no credit card required; free top-ups available by contacting the team | [Developer sign-up](https://trychannel3.com/developers), code `PAYPAL-HACKATHON-2026`. Prize guidance requires meaningful use of the API or MCP server, not just a reference to it. [Source](https://paypalaihackathon.devpost.com/details/channel3) |
| Elastic | 14-day serverless trial, with an additional 30-day extension; alternatively run Elasticsearch locally for free | [Hackathon sign-up](https://ela.st/elastic-paypal-hackathon), [extension form](https://www.elastic.co/trialextension), [local download](https://www.elastic.co/downloads/elasticsearch). [Source](https://paypalaihackathon.devpost.com/details/elastic) |
| KERNEL | $50 in credits for use during the hackathon | [Claim](https://dashboard.onkernel.com/hackathon?code=KERNELDEVPOST2026), code `KERNELDEVPOST2026`. [Source](https://paypalaihackathon.devpost.com/details/kernel) |
| Postman | Standard free access; no explicit special hackathon credits or extended trial listed | [Postman](https://www.postman.com/), [PayPal API collection](https://www.postman.com/paypal). [Source](https://paypalaihackathon.devpost.com/details/postman) |
| Render | $50 in participant credits | [Claim portal](https://credits-portal-mmdm.onrender.com/claim/paypal-ai-hackathon), [credit/billing documentation](https://render.com/docs/credits). Check charges beyond the credit balance. [Source](https://paypalaihackathon.devpost.com/details/render) |
| Zapier | New accounts receive a standard 14-day Professional trial; no credit card required, then transition to Free | [Sign-up](https://zapier.com/sign-up), [MCP quickstart](https://docs.zapier.com/mcp/get-started/quickstart). This is a standard new-account trial described on the resource page, not a verified exclusive hackathon promotion. [Source](https://paypalaihackathon.devpost.com/details/zapier) |

For AG Grid, APIMatic, and Bryntum, the sponsor pages offer additional implementation guidance beyond the broader “use any tools” eligibility language in the rules. Use the official rules as authoritative and consult the sponsor if prize qualification is unclear.

## 9. PayPal technical resources and project inspiration

### Core development links

- [PayPal Developer Portal](https://developer.paypal.com/)
- [Developer Dashboard](https://developer.paypal.com/dashboard/)
- [Getting started with PayPal sandbox](https://github.com/paypaldev/getting-started-with-paypal-sandbox)
- [Payments](https://developer.paypal.com/payments/)
- [Checkout](https://developer.paypal.com/checkout/)
- [JavaScript SDK](https://developer.paypal.com/sdk/js/)
- [REST APIs](https://developer.paypal.com/api/rest/)
- [Payment links/buttons — low-code/no-code](https://developer.paypal.com/payment-links-buttons/overview)
- [Subscriptions](https://developer.paypal.com/platforms/subscriptions)
- [Invoicing](https://developer.paypal.com/invoicing/about)
- [Payouts](https://developer.paypal.com/docs/payouts/)
- [Disputes](https://developer.paypal.com/platforms/disputes/overview)
- [Webhooks](https://developer.paypal.com/api/webhooks/overview)

### AI and agent tooling

- [PayPal AI Toolkit on GitHub](https://github.com/paypal/AI-Toolkit)
- [PayPal AI documentation](https://docs.paypal.ai/)
- [Agent Toolkit quickstart](https://docs.paypal.ai/developer/tools/ai/agent-toolkit-quickstart)
- [MCP Server quickstart](https://docs.paypal.ai/developer/tools/ai/mcp-quickstart)
- [Agent tools reference](https://docs.paypal.ai/developer/tools/ai/agent-tools-ref)

These links are collected from the official resource page; the linked technical documentation itself was not independently audited for current API availability.[^4](https://paypalaihackathon.devpost.com/resources)

### Suggested directions, not mandatory tracks

The resource page suggests AI agents/automation, money movement, business solutions, consumer experiences, and social impact. Examples include shopping agents, contractor payouts, freelancer dashboards, receipt-based bill splitting, invoicing, creator tipping, chat-based money requests, accessible donations, and community funding pools. These are inspiration, not additional submission requirements.[^4](https://paypalaihackathon.devpost.com/resources)

## 10. Community, support, and registration

- [Hackathon homepage and Join Hackathon](https://paypalaihackathon.devpost.com/)
- [Official rules](https://paypalaihackathon.devpost.com/rules)
- [FAQs](https://paypalaihackathon.devpost.com/details/faqs)
- [Resources and workshop links](https://paypalaihackathon.devpost.com/resources)
- [PayPal hackathon Discord](https://discord.gg/sJ2G6DyvSK)
- [Updates](https://paypalaihackathon.devpost.com/updates)
- [Discussions](https://paypalaihackathon.devpost.com/forum_topics)
- [Project gallery](https://paypalaihackathon.devpost.com/project-gallery)
- General rules questions: **support@devpost.com**.
- The overview lists hackathon manager contact: **janet@devpost.com**.

Register with a free Devpost account (or sign in to an existing one). Use an existing PayPal account or create a free developer account for sandbox access. The FAQ describes adding teammates through the Devpost project team section using email invitations or a team invite link. Country-specific PayPal sign-up issues can be raised in Discord.[^1](https://paypalaihackathon.devpost.com/)[^2](https://paypalaihackathon.devpost.com/rules)[^3](https://paypalaihackathon.devpost.com/details/faqs)[^4](https://paypalaihackathon.devpost.com/resources)

## 11. Important legal and administrative terms

- Entrants retain intellectual property in their submissions. Submission grants the Sponsor a non-exclusive license to use the entry for judging.
- The rules permit Sponsor/Devpost promotion of the submission and use of contributors’ names, likenesses, voices, and images during the hackathon and for three years afterward. Some submitted components may be displayed publicly.
- Entry consents to collection and retention of personal information to operate and publicize the hackathon. The publicity clause also covers name, photograph, opinions/comments, hometown, and country, worldwide without additional payment or review rights unless prohibited by law.
- The relationship with the Sponsor/Administrator is **not confidential**. Do not assume submitted business ideas, data, or materials will be kept secret.
- Submissions must not contain malicious code and must respect copyright, trademark, trade-secret, privacy, publicity, and other rights.
- The rules contain release, indemnification, and liability-limitation provisions, subject to applicable jurisdictional exceptions.
- Organizers may disqualify entrants or cancel, suspend, or modify the event under the stated conditions. Rules can change, and the official rules prevail over conflicting marketing/site materials.
- Ambiguous rules should be raised through a **written request for clarification before the deadline**.
- Except where prohibited, disputes are individual, binding AAA arbitration rather than class actions; the Federal Arbitration Act governs arbitration and New York substantive law governs the rules. The rules limit recoverable damages, subject to legal exceptions.
- [Devpost Terms of Service](https://info.devpost.com/terms) are incorporated; the hackathon rules control hackathon-specific conflicts. Personal information is subject to the [Devpost Privacy Policy](https://info.devpost.com/privacy).
- Late, incomplete, lost, or technically failed submissions are subject to the rules’ limitations; proof of sending is not proof of receipt. Confirm that Devpost actually receives the final entry.[^2](https://paypalaihackathon.devpost.com/rules)

## 12. Submission checklist

This checklist translates the sourced requirements above into participant actions. Security and availability checks are practical recommendations, not additional official eligibility conditions.

### Eligibility and build

- [ ] Confirm every participant meets age, residence, and conflict-of-interest requirements.
- [ ] Register on Devpost and appoint an authorized team Representative if needed.
- [ ] Use PayPal’s sandbox and make PayPal integration central to the product.
- [ ] Include real AI functionality and identify the tools/models used.
- [ ] Demonstrate a working application rather than a static mockup.
- [ ] If using an existing project, document significant changes made after October 1, 2026.
- [ ] Confirm rights and licenses for all code, APIs, data, assets, and media.
- [ ] If targeting a sponsor prize, verify actual tool use and sponsor-specific guidance.

### Repository and judge access

- [ ] Make the GitHub/GitLab/Bitbucket repository public.
- [ ] Include an appropriate open-source license detectable by the repository host.
- [ ] Supply complete source, permitted assets, setup steps, and run/test instructions.
- [ ] Provide a working hosted demo or a reproducible local build.
- [ ] Arrange necessary sandbox/test access without exposing production credentials.
- [ ] Test access from a clean environment or separate browser session.
- [ ] Keep judge access free and functional through December 15, 2026.
- [ ] Check trial expirations, provider charges, usage limits, and credit balances.

### Final submission

- [ ] Write the project’s features/functionality and explain its intended audience and value.
- [ ] Record a clear end-to-end demo shorter than three minutes.
- [ ] Upload to YouTube with public visibility and include the URL.
- [ ] Remove unlicensed music, trademarks, private data, and accidental secrets.
- [ ] Provide English materials or complete English translations.
- [ ] Complete all Devpost submission fields.
- [ ] Recheck the official rules and updates for amendments.
- [ ] Submit before **November 12, 2026, noon Pacific / 1:00 p.m. Arizona MST** and verify receipt.
- [ ] If selected, return required winner forms within 10 business days and plan for taxes/fees.

## 13. Source index and extraction notes

### Primary pages checked

1. [Overview](https://paypalaihackathon.devpost.com/)
2. [Official rules — full page checked directly](https://paypalaihackathon.devpost.com/rules)
3. [FAQs](https://paypalaihackathon.devpost.com/details/faqs)
4. [Resources](https://paypalaihackathon.devpost.com/resources)

### Sponsor pages checked

- [AG Grid](https://paypalaihackathon.devpost.com/details/aggrid)
- [APIMatic](https://paypalaihackathon.devpost.com/details/apimatic)
- [Astropods](https://paypalaihackathon.devpost.com/details/astropods)
- [Bryntum](https://paypalaihackathon.devpost.com/details/bryntum)
- [Channel3](https://paypalaihackathon.devpost.com/details/channel3)
- [Elastic](https://paypalaihackathon.devpost.com/details/elastic)
- [KERNEL](https://paypalaihackathon.devpost.com/details/kernel)
- [Postman](https://paypalaihackathon.devpost.com/details/postman)
- [Render](https://paypalaihackathon.devpost.com/details/render)
- [Zapier](https://paypalaihackathon.devpost.com/details/zapier)

### Clarifications

- The deadline shown in MST is a localized display of the Pacific deadline, not a conflicting deadline.
- The rules do not state a numeric team-size limit; the FAQ explicitly says there is no maximum.
- Prize categories are not prescribed project tracks.
- The AG Grid third-place award quantity is three in the rules and checked overview.
- Special-access instructions should be clarified with the organizer if complying would otherwise expose sensitive credentials publicly.
- The overview, FAQ, and sponsor guidance supplement the rules but do not override them. Offers and trials were extracted from resource pages, not redeemed or independently tested.
