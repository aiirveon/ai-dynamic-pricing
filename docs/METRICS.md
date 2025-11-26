# Success Metrics Framework
## AI Dynamic Pricing - How We Measure Impact

**Purpose:** Define what "success" means for this AI product, how we measure it, and when we'd kill the feature.

---

## 1. Metrics Hierarchy
```
North Star Metric: Revenue Per Transaction (RPT)
    ↓
Primary Metrics (Must improve)
    • Revenue Lift
    • Customer Retention
    • Price Acceptance Rate
    ↓
Secondary Metrics (Monitor closely)
    • Model Performance (RMSE)
    • Explainability Score
    • System Reliability
    ↓
Counter-Metrics (Must NOT degrade)
    • Customer Satisfaction
    • Basket Size
    • Brand Perception
```

---

## 2. North Star Metric

### Revenue Per Transaction (RPT)

**Definition:** Average revenue generated per customer transaction

**Why This Metric:**
- Captures value creation without punishing traffic fluctuations
- Aligns business goal (revenue) with customer experience (transaction value)
- Can be optimized through better pricing without increasing costs

**Formula:**
```
RPT = Total Revenue / Total Transactions
```

**Baseline:** £4.20 (current static pricing)  
**Target:** £4.62 (+10% lift by Month 3)  
**Stretch:** £4.70 (+12% lift by Month 6)

**How We Track:**
- Daily calculation: Compare dynamic pricing days vs. baseline
- Weekly trend analysis: Is lift sustained or fading?
- Cohort analysis: RPT by customer segment (new vs. repeat)

---

## 3. Primary Metrics (OKR Framework)

### Objective 1: Increase Revenue Without Hurting Retention

**Key Result 1.1:** Achieve 8-12% revenue lift vs. static pricing baseline
- **Measurement:** A/B test (50% transactions static, 50% dynamic) for 4 weeks
- **Target:** ≥8% lift with 95% confidence
- **Tracking:** Daily revenue dashboard (Streamlit app)
- **Owner:** Product Manager (me)

**Key Result 1.2:** Maintain ≥85% monthly customer retention
- **Measurement:** % of customers who transact in Month N and Month N+1
- **Target:** No degradation vs. pre-dynamic-pricing baseline
- **Red Flag:** <83% retention = immediate investigation
- **Tracking:** Monthly cohort analysis
- **Owner:** Customer Success

**Key Result 1.3:** Achieve 80% price acceptance rate from shop owners
- **Measurement:** % of AI recommendations accepted without modification
- **Target:** ≥80% by Month 2
- **Insight:** Low acceptance = explainability problem or poor recommendations
- **Tracking:** Recommendation logs (every override is logged with reason)
- **Owner:** Product Manager

---

### Objective 2: Build Trust Through Explainability

**Key Result 2.1:** 90%+ of shop owners understand SHAP explanations
- **Measurement:** Post-onboarding survey (5-point Likert scale)
  - Q: "I understand why the AI recommends each price" (Agree/Strongly Agree = pass)
- **Target:** 90% agree or strongly agree
- **Red Flag:** <70% = redesign explainability UI
- **Tracking:** Quarterly survey
- **Owner:** Product Designer + PM

**Key Result 2.2:** <3% customer complaint rate about pricing
- **Measurement:** Customer complaints per 1,000 transactions (owner-reported)
- **Baseline:** 2% (typical for any pricing)
- **Target:** <3% (dynamic pricing doesn't increase complaints)
- **Red Flag:** >5% = pause rollout, investigate
- **Tracking:** Weekly complaint log review
- **Owner:** Customer Success

---

## 4. Secondary Metrics (Leading Indicators)

### Model Performance

| Metric | Target | Why It Matters |
|--------|--------|----------------|
| **Test RMSE** | <£0.20 | Predictions within 20p of actual price = owners trust recommendations |
| **R² Score** | >0.95 | Model explains >95% of price variance = captures key patterns |
| **Training Time** | <10 min | Weekly retraining must be fast to adapt to new patterns |
| **Feature Stability** | Top 5 features consistent week-to-week | Prevents "random" recommendation changes |

**How We Monitor:**
- Weekly model retraining report (automated)
- SHAP feature importance tracking (detect drift)
- Prediction confidence distribution (flag low-confidence periods)

### System Reliability

| Metric | Target | Why It Matters |
|--------|--------|----------------|
| **API Uptime** | 99.5% | Downtime = owners can't get recommendations = manual pricing |
| **Response Latency** | <500ms | Fast predictions enable real-time POS integration |
| **Dashboard Load Time** | <2 sec | Slow UI = owners won't use product |
| **Error Rate** | <0.1% | Failed predictions = lost revenue opportunities |

**How We Monitor:**
- Datadog/Sentry for API monitoring
- Weekly uptime report
- Automated alerts for latency spikes

---

## 5. Counter-Metrics (Guardrails)

These metrics must NOT degrade, or we stop rollout immediately.

### Customer Satisfaction (CSAT)

**Definition:** "How satisfied are you with your purchase today?" (1-5 scale)

**Baseline:** 4.2/5 (industry average for coffee shops)  
**Threshold:** Must stay ≥4.0/5  
**Red Line:** <3.8/5 = immediate rollback to static pricing

**Why This Matters:**
- Low CSAT = customers feel "ripped off" by dynamic pricing
- Even if revenue increases, unhappy customers will churn long-term

**How We Track:**
- Optional post-transaction survey (10% sample rate)
- Weekly CSAT trend analysis
- Correlate CSAT with price change magnitude (does +£0.50 hurt satisfaction?)

### Average Basket Size

**Definition:** Average # of items purchased per transaction

**Baseline:** 1.8 items/transaction  
**Threshold:** Must stay ≥1.7 items/transaction  
**Red Line:** <1.6 = dynamic pricing is cannibalizing upsells

**Why This Matters:**
- Higher prices might increase RPT but reduce basket size
- Net effect could be negative if customers buy fewer items

**How We Track:**
- Daily basket size analysis
- Segment by price change magnitude (high vs. low dynamic adjustments)

### Brand Perception (NPS)

**Definition:** Net Promoter Score - "How likely are you to recommend this coffee shop?" (0-10)

**Baseline:** NPS 45 (good for food/beverage)  
**Threshold:** Must stay ≥40  
**Red Line:** <30 = brand damage from dynamic pricing

**Why This Matters:**
- Dynamic pricing could be perceived as "greedy" or "unfair"
- Long-term brand damage > short-term revenue gain

**How We Track:**
- Quarterly NPS survey (email to loyalty program members)
- Social media sentiment analysis (mentions of "price" or "expensive")

---

## 6. Experimental Framework

### A/B Testing Methodology

**Hypothesis:** Dynamic pricing increases RPT by 8-12% without hurting retention

**Test Design:**
- **Control Group:** 50% of transactions use static pricing (baseline)
- **Treatment Group:** 50% of transactions use AI-recommended dynamic pricing
- **Randomization:** By hour (e.g., 8-9 AM = control, 9-10 AM = treatment)
- **Duration:** 4 weeks minimum (capture weekly seasonality)
- **Sample Size:** 3,547 transactions (current dataset) = sufficient for 95% confidence

**Success Criteria:**
- RPT lift ≥8% in treatment group
- No statistically significant decrease in retention
- No increase in complaints (tracked via owner logs)

**Rollout Decision:**
- Pass all 3 criteria → Full rollout
- Fail any 1 → Iterate and re-test
- Major failure (CSAT <4.0) → Kill feature

---

## 7. Reporting Cadence

### Daily Dashboard (Streamlit App)
- Revenue: Today vs. Yesterday vs. Last Week
- RPT trend (7-day rolling average)
- Top 3 revenue-driving time slots
- Model confidence distribution
- System health (uptime, latency)

### Weekly Business Review (Email Report)
- Sent every Monday 8 AM to shop owner
- Includes:
  - Revenue lift % vs. static baseline
  - Top 5 recommendations that drove revenue
  - Customer satisfaction score
  - Price acceptance rate
  - Action items (if any)

### Monthly Deep Dive (PDF Report)
- Sent first Monday of each month
- Includes:
  - Cohort retention analysis
  - Seasonal pattern analysis (month-over-month trends)
  - Feature importance changes (SHAP analysis)
  - Competitor benchmarking (if data available)
  - Recommendations for next month

### Quarterly Business Review (Executive Presentation)
- Presented to franchise owner / finance director
- Includes:
  - OKR progress (all key results)
  - ROI calculation (revenue lift vs. subscription cost)
  - Customer case studies (testimonials)
  - Roadmap for next quarter

---

## 8. Metrics-Driven Decision Framework

### When to Double Down (Positive Signals)
- ✅ RPT lift >12% (exceeds target)
- ✅ Retention stable or improving
- ✅ High price acceptance rate (>85%)
- ✅ CSAT stable (≥4.2)
- **Action:** Expand to more locations, invest in V2 features

### When to Iterate (Mixed Signals)
- ⚠️ RPT lift 5-8% (below target but positive)
- ⚠️ Price acceptance 60-80% (owners hesitant)
- ⚠️ CSAT 4.0-4.2 (slight decline)
- **Action:** Improve explainability, gather qualitative feedback, A/B test new strategies

### When to Pivot (Negative Signals)
- ❌ RPT lift <5% (not worth complexity)
- ❌ Retention declining
- ❌ Price acceptance <60% (owners don't trust model)
- **Action:** Pause rollout, conduct user interviews, redesign core value prop

### When to Kill (Failure Signals)
- 🛑 CSAT <3.8 (customers actively unhappy)
- 🛑 Retention <80% (losing customers)
- 🛑 NPS <30 (brand damage)
- 🛑 Complaints >5% (vocal backlash)
- **Action:** Immediate rollback to static pricing, post-mortem analysis

---

## 9. Attribution & Causality

### Challenge: How do we know revenue lift is due to dynamic pricing, not external factors?

**External Factors to Control For:**
- Seasonality (holidays, summer vs. winter)
- Marketing campaigns (shop runs Instagram ad)
- Competitor actions (new cafe opens nearby)
- Weather (unusually cold week = higher demand anyway)
- Events (concert nearby = foot traffic spike)

**Our Approach:**
1. **A/B Testing:** Control group isolates dynamic pricing effect
2. **Regression Analysis:** Control for weather, day-of-week, holidays in model
3. **Synthetic Control:** Build counterfactual baseline using similar shops without dynamic pricing
4. **Time-Series Decomposition:** Separate trend, seasonality, and dynamic pricing effect

**Confidence Level:**
- High confidence: A/B test + regression both show 8%+ lift
- Medium confidence: Positive trend but noisy data (need longer test)
- Low confidence: External event (holiday) confounds results

---

## 10. Metrics Dashboard (Visual Mockup)
```
┌─────────────────────────────────────────────────────────────┐
│  AI DYNAMIC PRICING - METRICS DASHBOARD                     │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  📈 Revenue Per Transaction (RPT)                           │
│  Today: £4.65 (+10.7% vs baseline)  ✅ TARGET MET           │
│  7-Day Avg: £4.58 (+9.0%)                                   │
│  [───────▓▓▓▓▓▓─────] 90% confidence                        │
│                                                              │
│  👥 Customer Retention                                       │
│  This Month: 86% (baseline: 85%)  ✅ STABLE                 │
│                                                              │
│  🎯 Price Acceptance Rate                                    │
│  This Week: 78% (target: 80%)  ⚠️ SLIGHTLY BELOW            │
│                                                              │
│  ⭐ Customer Satisfaction (CSAT)                             │
│  This Week: 4.3/5 (baseline: 4.2)  ✅ IMPROVED              │
│                                                              │
│  🤖 Model Performance                                        │
│  RMSE: £0.18 (target: <£0.20)  ✅                           │
│  Uptime: 99.7%  ✅                                           │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

---

## 11. Key Takeaways for Stakeholders

**For Shop Owners:**
- We track revenue, customer happiness, and your trust in the system
- If any metric goes red, we pause and fix before continuing
- You'll get weekly reports showing exactly how much more you're making

**For Finance Directors:**
- ROI is transparent: Revenue lift vs. subscription cost
- We measure both short-term gains and long-term brand health
- Monthly reports include confidence intervals (we don't oversell results)

**For Operations Managers:**
- We monitor complaint rates and staff confidence in explaining prices
- If complaints spike, we investigate root cause immediately
- Dashboard shows which time slots are most profitable

---

## 12. Appendix: Metric Definitions

### Revenue Lift
```
Revenue Lift % = ((Dynamic Revenue - Baseline Revenue) / Baseline Revenue) * 100
```

### Customer Retention
```
Retention % = (Customers in Month N who return in Month N+1) / Total Customers in Month N * 100
```

### Price Acceptance Rate
```
Acceptance % = Recommendations Accepted / Total Recommendations * 100
```

### CSAT Score
```
CSAT = Average rating (1-5 scale) from post-transaction surveys
```

### NPS
```
NPS = % Promoters (9-10) - % Detractors (0-6)
```

---

**Document Owner:** Ogbebor Osaheni  
**Last Updated:** November 26, 2025  
**Review Cycle:** Quarterly  
**Next Review:** February 2026