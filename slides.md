---
theme: the-unnamed
layout: cover
background: https://images.unsplash.com/photo-1682905926517-6be3768e29f0?q=80&w=3387&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDF8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D
title: 'ML/AI Engineering Leadership: Real Stories, Real Systems'
transition: fade
---

# ML/AI Engineering Leadership
## Real Stories, Real Systems

<p class="fragment">Interview Presentation - Cloud Engineering Director Panel</p>

---
layout: about-me
helloMsg: Hello!
name: Josh Phillips
imageSrc: /Pirate_Dog.png
line1: Senior Engineering Manager - AI Enablement & Continuous Delivery
line2: Capital One | Modern Financial Core
---

::right::

### Career Journey
- **Capital One** (Current): Reduced SDLC 8-12 days → <1 day for 300 engineers
- **Excella**: Won $65M USCIS contract with 45-min live AWS deployment
- **5am Solutions**: Head of SRE, learned from 3GB Docker disasters

### What I'll Show You
**Real stories** from production systems + **TrialForge** (proof I still code hands-on)

---
layout: section
---

# Q1: Architecture
## Prototype → Production at Scale

---
layout: default
---

# Excella RFAD: The 45-Minute Demo That Won $65M

<div style="font-size: 24px; line-height: 1.6;">

**The Challenge:**
USCIS immigration fraud detection system. Technical evaluation: Deploy working system to AWS GovCloud **during our presentation**.

**What We Deployed (45 minutes):**
- ✅ Fraud detection model (NLP on provided dataset)
- ✅ Frontend web UI for case review
- ✅ Backend APIs
- ✅ Login portal (authentication)
- ✅ **All in AWS GovCloud using approved tech**

**My Role:** Infrastructure & CI/CD (the DevSecOps layer)

**Outcome:** We won. Competitors brought slides. We brought working code.

</div>

---
layout: default
---

# Bento Platform: Production Architecture

<div style="font-size: 22px;">

**After winning, I built Bento**—a reusable ML platform for government projects.

**Multi-Backend Support** (different agencies, different constraints):
- EKS, ECS, vanilla K8s, OpenShift
- FedRAMP requires AWS. Some agencies require on-prem. Some mandate Red Hat OpenShift.

**Architecture:**
- **Researcher workstations**: EC2 + Jupyter with authentication
- **Model storage**: SageMaker (when allowed) or S3
- **Data**: S3 (each researcher isolated, no shared filesystem)
- **Images**: From-scratch Docker (300-500MB vs 1.5GB+ base images)
- **Deployment**: Ephemeral environments via CloudFormation (IaC)

**The Achievement:** First continuous deployment at a government site. Ephemeral environments—tear down and rebuild weekly using IaC. **Groundbreaking in 2016-2018 for government.**

**Reuse:** Bento used across **8 large projects** (USCIS, GEICO, DoD, ICE, eVerify, others)

</div>

---
layout: default
---

# TrialForge: Same Pattern, Still Hands-On

<div style="font-size: 24px;">

**Prototype First** → Local Streamlit demo validated RAG concept

**Production Architecture** → 7 AWS stacks:
- Auth (Cognito multi-tenant)
- OpenSearch (vector DB)
- Bedrock (Claude + Titan)
- S3 Data pipeline
- Letta (conversation memory)
- Observability (CloudWatch + X-Ray)
- SageMaker (model registry ready)

**Cost:** $116/month with compliance and observability from day one

**Performance:** 280ms Lambda cold start, <2s P99 latency

**Same discipline:** Scalability, reliability, performance designed in

</div>

---
layout: center
---

# Live Demo: TrialForge Architecture

**[AWS Console Links]**
- 📊 [CloudWatch Dashboard](https://us-east-2.console.aws.amazon.com/cloudwatch/home?region=us-east-2#dashboards/dashboard/TrialForge-Production)
- 🔍 [X-Ray Service Map](https://us-east-2.console.aws.amazon.com/cloudwatch/home?region=us-east-2#xray:service-map/)
- 🚀 [CodePipeline](https://us-east-2.console.aws.amazon.com/codesuite/codepipeline/pipelines/TrialForge-CDK-Pipeline/view?region=us-east-2)
- 🌐 [Live UI](https://main.d1ovxpzawsegze.amplifyapp.com)
- 💻 [GitHub Repository](https://github.com/Josh-thephillipsequation/trialforge)

---
layout: section
---

# Q2: Deployment
## Honest Lessons from Failure

---
layout: default
---

# 5am Solutions: The Docker Disaster

<div style="font-size: 24px; line-height: 1.6;">

**Context:** Bioinformatics startup building clinical trial management system for Pfizer. I was Head of SRE.

**The Disaster:** **3GB+ Docker images**
- Data scientists embedding pickle files directly in images
- Every model update = rebuild entire 3GB image
- Build times: 20-30 minutes
- Deploy times: 10+ minutes just to pull images
- Team working **60-70 hour weeks, through weekends**

**The Honest Outcome:** Project failed. Funding pulled after phase 2. Out-competed by IBM.

**Who Decided Microservices?** VP of Tech. Felt like **resume-driven development**—10-15 microservices for a team that barely knew Docker. Should've started with a monolith.

</div>

---
layout: default
---

# What I Learned (Applied Ever Since)

<div style="font-size: 26px; line-height: 2;">

✅ **Lesson 1:** Separate model artifacts from container images (models in S3, pull at startup)

✅ **Lesson 2:** Don't start with microservices (master the basics first)

✅ **Lesson 3:** As a leader, push back on unrealistic timelines (60-70 hour weeks = unsustainable)

✅ **Lesson 4:** Know what you don't know (should've brought in Docker expert)

**The Silver Lining:** That failure taught me everything for Excella. At Bento: from-scratch Docker images, models in S3, reusable platform.

**Plus:** I learned from world-class people. My boss literally wrote the O'Reilly book on Continuous Integration. Still friends with them today.

</div>

---
layout: default
---

# TrialForge: Recent Deployment Challenges

<div style="font-size: 24px;">

Even with all that experience, I still hit deployment challenges:

**Challenge 1: HTML Entities Breaking LLM**
```typescript
function normalizeText(text: string): string {
  return text
    .replace(/&#x([0-9A-Fa-f]+);/g, (_, hex) => 
      String.fromCharCode(parseInt(hex, 16)))
    .normalize('NFKC').trim();
}
```
Clinical trial data had HTML entities. LLM saw gibberish. Discovered via CloudWatch Logs. Added normalization.

**Challenge 2: Embedding Dimension Mismatch**
Local: 768-dim. Bedrock: 1024-dim. Confidence scores dropped in prod. Re-indexed all data with Bedrock.

**Challenge 3: Metadata Filters**
Router filtered for publication sources, but data was clinical_trial type. Zero results. Debug logs caught it.

</div>

---
layout: section
---

# Q3: CI/CD
## Transforming Team Identity

---
layout: default
---

# Capital One Release Team: 8-12 Days → <1 Day

<div style="font-size: 24px; line-height: 1.6;">

**The Challenge:**
Release Team stuck in critical path. Manual orchestration, tribal knowledge, ad-hoc each release. Their identity: **"We're the heroes who make releases happen."**

**The Conflict:**
We needed them to adopt new tooling. Problem? **Our tool was slower initially**. Tech leads had **verbal confrontation**: "Why adopt something slower?" vs "Your way doesn't scale."

**How We Won:**
1. **Recognized their work** - made them co-owners, not "the problem"
2. **Reframed**: "Free you up for innovation" not "replace you"
3. **6-9 months persistence**: Tool went "not great" → "forgot it was a problem"
4. **Made it undeniably better**: Faster AND more reliable

**The Guardrails:** Security scans, e2e tests, performance tests, cost controls, compliance. **Guardrails built INTO the platform, not into the process**. Engineers ship faster BECAUSE safety is automatic.

</div>

---
layout: default
---

# Capital One Outcome

<div style="font-size: 32px; line-height: 2;">

📉 **8-12 days → <1 day**

👥 **300 engineers** self-service

💬 **"We forgot releases were a problem"** ← killer quote

</div>

---
layout: center
---

# TrialForge CI/CD: Same Principle

**CodePipeline with cdk-nag compliance checks**

**Stages:** Source → Build (cdk-nag) → Deploy Dev → Approval → Deploy Prod

**cdk-nag validates HIPAA controls on every commit.** Can't deploy non-compliant infrastructure.

**Guardrails built into the platform.**

**[Live Pipeline](https://us-east-2.console.aws.amazon.com/codesuite/codepipeline/pipelines/TrialForge-CDK-Pipeline/view?region=us-east-2)**

---
layout: section
---

# Q4: Monitoring
## Alert Bankruptcy → Signal

---
layout: default
---

# Brivo: The Day I Declared Alert Bankruptcy

<div style="font-size: 24px; line-height: 1.6;">

**Context:** Brivo—cloud-based access control for Apple, Meta, Chick-fil-a. Mission-critical (people can't get into buildings if we're down).

**When I Joined:**
- **500 alerts**
- On-call getting **25 pings/day** (200+ during incidents)
- Example alert: **"g4data not doing super excellent"** (no runbook, no explanation)
- Everyone piled on incident calls (CTO, multiple directors all troubleshooting together)
- Engineers asking for **2-3 days off after every on-call shift**

**What I Did:**
1. Inventory: Listed all 500 alerts
2. Asked teams: "Do you need this? What action do you take?"
3. Set requirements: POC, description, runbook, proper severity
4. Set deadline: 1 month—delete everything not justified

</div>

---
layout: default
---

# Brivo Severity Framework

<div style="font-size: 26px; line-height: 2;">

**Sev1:** >5% customers impacted (CTO in Slack, not dominating call)

**Sev2:** Single customer issue

**Sev3:** Internal, no customer impact

Forced teams to justify: **Is this a Slack message, a weekly report, or a 3am page?**

</div>

---
layout: default
---

# Brivo Outcome

<div style="font-size: 28px; line-height: 2;">

📉 **500 → 25 alerts** (95% reduction)

📉 **25 pings/day → 2-3/week** (90%+ reduction)

✅ **Engineers stopped asking for days off**

✅ On-call became a **non-event**

**incident.io Rollout:** Slack-based incident management. `/incident` command opens channel, assigns roles (Commander, Scribe), smart routing, escalation policies.

**Lesson:** More alerts ≠ better visibility. **Signal matters.** If engineers ignore alerts, you have zero visibility. On-call experience is a retention issue.

</div>

---
layout: center
---

# TrialForge Monitoring: Same Discipline

**[CloudWatch Dashboard](https://us-east-2.console.aws.amazon.com/cloudwatch/home?region=us-east-2#dashboards/dashboard/TrialForge-Production)**
- Lambda invocations, duration, errors
- Bedrock token usage
- **Custom: Confidence scores** (answer quality metric for RAG)

**CloudWatch Alarms:**
- High error rate >5%
- High latency P99 >3s
- Cost anomaly >$10/day
- OpenSearch cluster red

**Every alarm has a threshold I can defend. Every alarm has an action.**

**No "g4data not doing super excellent" here.**

---
layout: section
---

# Q5: Scaling
## Multi-Architecture Experience

---
layout: default
---

# Scaling ML Inference: Standard Pattern

<div style="font-size: 24px; line-height: 1.6;">

**Experience:** 2 data scientists at Brivo → 50 at AQR → enterprise platforms serving millions

**Standard Pattern Across All Projects:**
1. **CloudWatch metrics**: CPU, memory, request count
2. **Scale application tier:**
   - K8s: HPA (Horizontal Pod Autoscaler) adjusts pod counts
   - ECS: Service Auto Scaling adjusts container counts
3. **When hitting limits, scale compute:**
   - Fargate: Automatic
   - K8s: Cluster Autoscaler adds nodes
   - Lambda: Auto-scales by default

**This worked across EKS, ECS, vanilla K8s, OpenShift. Same pattern, different orchestrators.**

</div>

---
layout: default
---

# Real Challenge: EFS Performance

<div style="font-size: 26px; line-height: 1.8;">

**At AQR:** ~50 researchers on shared EFS. **Big mistake.**

**Issues:**
- Latency spikes (concurrent writes)
- File locks (models reading/writing same folders)
- Failures (concurrent writes causing corruption)

**Solution (Excella Projects):**
Went straight to S3:
- Each researcher gets own S3 prefix: `s3://models/researcher-name/`
- No shared filesystem, no locks
- Integration at model level (CI/CD pulls from S3)
- Golden datasets published via pipeline

**Lesson:** **Don't use shared filesystems for ML at scale**. S3 is boring and reliable—that's what you want.

</div>

---
layout: default
---

# Bento Platform Scale

<div style="font-size: 28px; line-height: 2;">

**The Numbers:**
- ~50 researchers at AQR
- ~25 researchers per project at Excella
- 8 large projects (USCIS, GEICO, DoD, ICE, eVerify, others)
- Millions of end users impacted

**Platform Thinking:** Built **one platform, multiple deployment targets**. Each project had different constraints, but core architecture was reusable.

Platform thinking scales better than custom solutions.

</div>

---
layout: default
---

# TrialForge Scaling Plan

<div style="font-size: 24px;">

**Current:** ~100 queries/day, <5 qps

**At 1000 qps** (my scaling plan):
- Lambda: ~300 concurrent (within 1000 limit)
- OpenSearch: Multi-node cluster (change CDK 1→3 instances)
- Bedrock: Request quota increase (400 req/min → 60k)

**Optimizations** (learned from past projects):
1. **Embedding caching** (Redis) - like artifact caching at Excella
2. **Model separation** - already doing (models in S3, learned from 5am)
3. **CloudWatch auto-scaling** - same as Bento
4. **From-scratch images** - learned from Excella (but hit TrialForge Lambda size limits)

**Cost impact:** $116/month → $700k/month at 1000 qps (negotiate enterprise pricing)

</div>

---
layout: center
background: https://images.unsplash.com/photo-1451187580459-43490279c0fa?q=80&w=3540&auto=format&fit=crop&ixlib=rb-4.0.3
---

# Summary: How I Answered Your Five Questions

<div style="font-size: 26px; line-height: 2;">

**Q1:** Won $65M contract with 45-min demo. Built Bento platform reused across 8 projects. TrialForge shows I still build hands-on.

**Q2:** Learned from 3GB Docker disaster at 5am. Applied lessons at Excella and TrialForge. Failures teach more than successes.

**Q3:** Transformed Capital One Release Team from toil heroes to platform engineers. 8-12 days → <1 day for 300 engineers. Same guardrails in TrialForge.

**Q4:** Declared alert bankruptcy at Brivo. 500 → 25 alerts. On-call became non-event. Built incident.io Slack-based response. TrialForge has same discipline.

**Q5:** Scaled across EKS, ECS, K8s, OpenShift. Learned don't use EFS for ML, use S3. Bento reused 8 times. TrialForge architected for scale.

</div>

---
layout: cover
background: https://images.unsplash.com/photo-1682905926517-6be3768e29f0?q=80&w=3387&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDF8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D
---

# Questions?

**I've led ML platforms for government and enterprise.**

**TrialForge proves I can still execute.**

**I lead through teams, but I understand the tech deeply because I still build.**

---
layout: center
---

# Live Demo Links

**TrialForge System:**
- 🌐 [Live UI](https://main.d1ovxpzawsegze.amplifyapp.com)
- 💻 [GitHub Repository](https://github.com/Josh-thephillipsequation/trialforge)

**AWS Console:**
- 📊 [CloudWatch Dashboard](https://us-east-2.console.aws.amazon.com/cloudwatch/home?region=us-east-2#dashboards/dashboard/TrialForge-Production)
- 🔍 [X-Ray Service Map](https://us-east-2.console.aws.amazon.com/cloudwatch/home?region=us-east-2#xray:service-map/)
- 🚀 [CodePipeline](https://us-east-2.console.aws.amazon.com/codesuite/codepipeline/pipelines/TrialForge-CDK-Pipeline/view?region=us-east-2)
