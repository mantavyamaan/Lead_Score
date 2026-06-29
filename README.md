# Lead Scoring Model

You can access my models here:

[Lead Score](https://mantavyamaan.github.io/Lead_Score/lead-score-console.html)

[Simplified Lead Score](https://mantavyamaan.github.io/Lead_Score/simplified-lead-score-console.html)

## Overview

The Lead Scoring Model is a multi-dimensional scoring framework designed to evaluate, rank, and prioritize sales leads based on their likelihood of conversion and long-term business value.

Unlike traditional lead scoring systems that rely on a few simple metrics, this model combines financial qualification, buying intent, engagement behaviour, organizational fit, risk assessment, and historical performance into a single standardized score ranging from **0–100**.

The objective is to help sales teams:

- Prioritize high-quality leads
- Reduce time spent on low-value prospects
- Improve conversion rates
- Allocate sales resources efficiently
- Create a consistent and explainable decision-making process

The model is entirely transparent, modular, and customizable, making it suitable for organizations with different sales strategies and customer segments.

---

# Objectives

The primary goals of this project are:

- Build an objective lead qualification framework
- Eliminate subjective sales decisions
- Standardize lead evaluation across teams
- Improve pipeline quality
- Predict purchase readiness
- Increase sales efficiency
- Maximize expected revenue from sales efforts

---

# Model Architecture

The model evaluates every lead across multiple independent dimensions.

Each dimension captures one important aspect of a lead's quality.

These dimensions are individually normalized before being combined into an overall score.

The architecture follows the pipeline below:

Raw Lead Data
↓

Feature Engineering
↓

Dimension-wise Scores
↓

Normalization
↓

Weighted Aggregation
↓

Modifiers & Penalties
↓

Final Lead Score (0–100)

---

# Core Evaluation Dimensions

The scoring framework evaluates leads across several business factors, including:

## Financial Qualification

Measures whether the lead has the financial capability to purchase.

Examples include:

- Budget
- Revenue
- Company size
- Financial stability

---

## Need & Product Fit

Evaluates how well the organization's needs match the offered solution.

Factors include:

- Business pain points
- Product relevance
- Expected value creation
- Use-case alignment

---

## Authority & Decision Structure

Determines whether the contact can influence purchasing decisions.

Examples:

- Decision maker
- Budget owner
- Technical evaluator
- Procurement involvement

---

## Engagement Quality

Measures how actively the lead interacts with the company.

Signals include:

- Website visits
- Email opens
- Email clicks
- Form submissions
- Webinar participation
- Content downloads
- Product demonstrations

Higher engagement generally indicates stronger buying intent.

---

## Sales Readiness

Measures how close the lead is to making a purchase.

Typical indicators include:

- Demo requested
- Pricing discussion
- Proposal requested
- Sales meetings completed
- Trial usage

---

## Historical Performance

Incorporates historical conversion behaviour into the scoring system.

Examples include:

- Similar customer conversions
- Historical close rates
- Industry conversion statistics

Confidence-adjusted historical metrics help reduce uncertainty caused by small sample sizes.

---

## Risk Assessment

Identifies factors that reduce the probability of successful conversion.

Possible risks include:

- Budget uncertainty
- Long procurement cycles
- Low response rates
- Competitive threats
- Contract complexity

Higher risk results in score penalties.

---

# Feature Engineering

The model transforms raw CRM data into meaningful numerical features.

Examples include:

- Email engagement rate
- Meeting completion rate
- Website activity score
- Sales interaction frequency
- Product interest level
- Buying intent indicators
- Historical conversion rate
- Organization quality metrics

These engineered features provide stronger predictive capability than raw variables alone.

---

# Statistical Components

Several mathematical techniques are incorporated to improve reliability and robustness.

## Normalization

Since different variables exist on different scales, all scores are normalized before aggregation.

This ensures:

- Fair comparison
- Stable weighting
- Consistent interpretation

---

## Confidence Adjustment

Historical conversion rates are adjusted using confidence intervals.

Benefits include:

- Prevents overconfidence from small sample sizes
- Produces more reliable estimates
- Reduces noisy predictions

---

## Exponential Decay

Recent activities receive greater importance than older interactions.

For example:

- Recent demo request > Demo six months ago
- Recent website visit > Historical visit

This allows the score to reflect current buying intent.

---

## Weighted Aggregation

Each scoring dimension contributes according to its business importance.

The weighted combination produces a comprehensive lead quality estimate while allowing organizations to customize priorities.

---

# Modifiers

Additional modifiers refine the overall score.

Possible modifiers include:

- Recent activity bonus
- Engagement consistency
- Sales velocity
- Positive buying signals
- Negative behavioural signals
- Risk penalties

These modifiers allow the score to capture real-world business behaviour beyond static feature weights.

---

# Final Lead Score

The final score is standardized onto a **0–100 scale**.

Typical interpretation:

| Score | Interpretation |
|--------|----------------|
| 90–100 | Exceptional lead |
| 75–89 | High-priority lead |
| 60–74 | Qualified lead |
| 40–59 | Moderate potential |
| 20–39 | Low priority |
| 0–19 | Very low conversion probability |

Organizations may customize these thresholds according to business needs.

---

# Advantages

- Transparent scoring methodology
- Fully explainable decisions
- Modular architecture
- Easy to customize
- CRM compatible
- Scalable to large datasets
- Reduces subjective bias
- Supports sales prioritization
- Improves pipeline efficiency
- Encourages data-driven decision making

---

# Technologies Used

- Python
- NumPy
- Pandas
- SciPy
- Jupyter Notebook

---

# Possible Future Improvements

- Machine Learning based weight optimization
- Gradient Boosting / XGBoost integration
- Logistic Regression calibration
- Real-time CRM integration
- Auto-learning feature weights
- Explainable AI (SHAP values)
- Time-series behavioural modelling
- Dynamic score recalibration
- Industry-specific scoring profiles

---

# Business Impact

This Lead Scoring Model enables organizations to prioritize opportunities based on measurable business signals rather than intuition.

The framework combines financial capability, organizational fit, engagement behaviour, buying readiness, historical performance, and risk into a single interpretable score, enabling sales teams to improve efficiency, focus on high-value prospects, and increase overall conversion performance while maintaining a transparent and explainable scoring methodology.

