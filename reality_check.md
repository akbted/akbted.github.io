# Anthropic's New Models, and the Heat Engineering Teams Are Taking

Every week, models get better. Demos get smoother. Benchmarks go up.

But inside companies, the conversation is shifting from *"Can we build it?"* to *"Who's going to own it when it goes wrong?"*

Because deploying AI at scale is not like shipping a feature. When AI fails, it does not just return an error—it can break your SLA, your compliance posture, or your customer's trust. And regulators are now asking companies to **prove how they govern AI systems**, not just whether they work on demo day.

---

## A Real Problem: Processing 3 Lakh Videos a Day

Let's make this concrete.

You need to process **300,000 videos per day**. That is about **3.5 videos per second** (300k ÷ 86,400 seconds). Sounds manageable—until you realize this isn't a *"write some Python"* problem. It's a **Big Data Architecture** problem.

The first question everyone asks: *"Which AI model should I use?"*

But the real questions are:

- Streaming or batch?
- Who decides the trade-offs?
- What happens when the system is wrong?
- How much does the company lose?

---

## CCTV vs. Upload — Two Different Architectures

Here's where design judgment matters.

### Scenario 1: Live CCTV Monitoring *(Security, Traffic, Proctoring)*

You need real-time answers. A person fell—alert security *now*.

→ Use **streaming**: `RTSP/WebRTC → Media Server → Kafka → AI Workers`

### Scenario 2: User Uploads *(Insurance Claims, Medical Records)*

Videos arrive in bursts. Users don't expect instant answers.

→ Use **batch processing** with presigned URLs:
`Client uploads → S3 → SQS trigger → GPU workers pull from queue`

> **The queue is your shock absorber.** When 10,000 uploads land at once, your system bends instead of breaking.

---

## Can AI Decide This for You?

**No.**

AI can't tell you whether to use streaming or batch. It can't weigh your latency SLA against your budget. It can't know whether your compliance team will accept a 2% false positive rate.

> *AI can write the code. It cannot design the system.*

Architecture decisions require judgment about trade-offs: cost vs. speed, explainability vs. accuracy, risk vs. scale. These are **business and operational decisions**, not just technical ones. And when things go wrong—when the model drifts, when a critical edge case fails—**someone human has to own the consequences**.

This is why system design remains a core competency, even as AI automates more of the coding.

---

## Two Techniques That Change the Economics

If you try to run AI on every pixel of 300,000 videos, you'll burn your GPU budget and still miss your SLA.

**The secret is filtration, not brute force.**

### 1. Keyframe Extraction

Process only the frames that change (e.g., scene cuts, motion events).

- A 1-minute video has ~1,800 frames
- Extract just 5 keyframes → **99% reduction in compute cost**

### 2. Cascade Filtering

Don't send everything to your expensive model. Filter in stages:

| Stage | Processor | Question | Action |
|-------|-----------|----------|--------|
| 1 | CPU | Is there motion? | No → discard |
| 2 | Tiny model | Is there a person? | No → discard |
| 3 | Heavy model | Is the person doing X? | Only ~5% of data reaches here |

---

### But which technique should you use?

AI can't answer that. You need to know:

- **What are you detecting?** (anomalies, people, weapons, text?)
- **What's the cost of a false negative?** (safety, money, reputation?)
- **What's your latency budget?**

That decision is **architecture judgment**, not prompt engineering.

---

## The "AI Goes Wrong" Question *(The One Nobody Wants to Own)*

This is where the real heat shows up. When your AI makes a mistake at scale:

- Who decides whether to trust the output: engineering, product, compliance, or the customer?
- What's the rollback plan when the model drifts?
- What's the cost of false positives vs. false negatives—in rupees, time, brand, and legal exposure?

---

## My Takeaway

New models (from Anthropic and others) are making it easier to **start**. But the hard problems are still human problems:

- **Who decides?**
- **What if it goes wrong?**
- **How much do we lose?**

AI can automate the coding. It can suggest optimizations. But it can't make the architectural trade-offs or carry the accountability.

**Designing these systems will remain a core competency—maybe not the coding, but the judgment.**
