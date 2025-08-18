One-line description

Segment customers into actionable groups using Recency, Frequency and Monetary signals, producing labels like Retain, Re-Engage, Nurture, and Reward (plus a few outlier groups) to drive targeted marketing and product decisions.

What we use (RFM)

Recency — how long since a customer’s last purchase (lower → more recent).

Frequency — how often they buy (counts of invoices/visits).

Monetary — total spend (lifetime revenue).

RFM compresses purchase history into three business-friendly dimensions that explain who to contact, how to contact them, and what offers to send.

Key features / capabilities

Cleans and prepares transaction records for customer-level analysis.

Builds interpretable RFM features and handles skew/outliers (e.g., log transform, scaling).

Uses K-Means to produce stable clusters supported by elbow & silhouette diagnostics.

Produces visual diagnostics (RFM distributions, 3D scatter, PCA/cluster plots, violin/violin/line summaries).

Exports: customer → cluster table, cluster profiles, and optional model/scaler artifacts for offline or production scoring.

Discovered segments (business labels & recommendations)
Cluster 0 — Retain (Blue)

Profile: High value, regular purchasers; not always the most recent buyers.

Business goal: Keep them loyal and engaged.

Tactics: Loyalty program perks, personalized upsells, priority support, retention A/B tests.

Cluster 1 — Re-Engage (Orange)

Profile: Low value, infrequent buyers who haven’t purchased recently.

Business goal: Bring them back to purchase.

Tactics: Targeted reactivation emails, time-limited discounts, cart/visit reminders.

Cluster 2 — Nurture (Green)

Profile: Lower overall value but recent activity (new or sporadic buyers).

Business goal: Grow their lifetime value.

Tactics: Onboarding journeys, educational content, small incentives to increase frequency.

Cluster 3 — Reward (Red)

Profile: Very frequent, high monetary customers — your most loyal buyers.

Business goal: Protect and reward top customers.

Tactics: VIP programs, exclusive previews, dedicated service, referral incentives.

Outlier groups (special handling)

These are small but strategically important extremes that may be separated from the main clusters.

Cluster -1 — PAMPER (Monetary Outliers)
Traits: Very high spenders but purchase infrequently.
Strategy: White-glove offers, premium experiences, retain via exclusivity.

Cluster -2 — UPSELL (Frequency Outliers)
Traits: Very frequent buyers with lower spend per order.
Strategy: Bundles, cross-sell, value packs, loyalty nudges to increase basket value.

Cluster -3 — DELIGHT (Monetary & Frequency Outliers)
Traits: Extremely high frequency and spend — top-tier customers.
Strategy: Dedicated VIP treatment, concierge offers, invitations to beta/pilot programs.

What this achieves for the business

Actionable targeting: Clear, named segments that marketing/product teams can use immediately.

Efficient spend: Allocate budget to segments with the highest expected ROI (e.g., Reward vs. Re-Engage).

Personalization: Tailored messages/offers match each segment’s behavior and value.

Measurement: Segments are testable — run A/B tests and track movement between segments over time.
