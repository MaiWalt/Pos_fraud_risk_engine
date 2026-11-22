# POS Fraud & Risk Analytics Engine

## Project Overview
This project is a comprehensive SQL-based risk analytics engine designed to detect fraudulent activities and operational anomalies in Point-of-Sale (POS) transaction data. Built entirely using **SQLite**, the engine moves beyond static rule-based flagging to employ advanced behavioral profiling and evasion detection.

**Core Technologies:** SQLite, Common Table Expressions (CTEs), Advanced Window Functions (`LAG`, `SUM OVER`, `AVG OVER`), Date Parsing.

---

## Architecture & Business Logic

### Tier 1: Foundational Operational Anomalies
These modules handle daily transaction hygiene and static risk rules.

1. **Duplicate Settlement / Double-Debit Detection:** 
   * **Logic:** Uses Self-Joins to catch accidental rapid double-swipes on the same terminal within seconds.
2. **Card-Testing Velocity Spikes:** 
   * **Logic:** Uses multi-lag window functions (`LAG()`) to track consecutive `FAILED_PIN` attempts leading directly into a `SUCCESS`, indicating stolen card brute-forcing.
3. **Odd-Hour High-Value Outliers:** 
   * **Logic:** Uses `strftime()` to extract hours and flag exceptionally high withdrawals during deep-night closed hours (01:00 - 04:00).

### Tier 2: Advanced Behavioral Risk Models
These modules identify sophisticated fraud tactics designed to bypass static banking limits.

4. **Sub-Threshold Split Transactions (Evasion Attack):**
   * **Logic:** Employs rolling cumulative sums (`SUM() OVER ... ROWS BETWEEN 2 PRECEDING`) to catch fraudsters splitting large withdrawals into 3 smaller chunks to stay under automated single-transaction limits.
5. **Merchant Terminal Baseline Profiling:**
   * **Logic:** Uses `AVG() OVER (PARTITION BY terminal_id)` to calculate each terminal's historical baseline, flagging new transactions that spike at least 3x above that specific terminal's normal volume (indicating possible terminal hijacking or collusion).

---

## SQL Query Modules
*(Add your 5 SQL queries below)*

### 1. Sub-Threshold Split Transactions 
```sql
-- [Paste your Challenge 4 SQL query here]
