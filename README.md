# 🏪 Branch Daily Action Report (BDAR) — Control Tower
## 🏢 Business Problem & Objective
### The Business Problem
In a multi-branch distribution network, branch staff handle an overwhelming volume of daily tasks across different functional areas—sales, stock management, procurement, housekeeping, and accounting. Without a centralized view, operational workflow suffers from critical blind spots:
* **Reactive Problem Solving:** Staff frequently spent their days reactively responding to emergencies rather than proactively addressing system anomalies before they caused transactional bottlenecks.
* **Information Overload:** ERP systems house vast quantities of data, but identifying *which* specific tasks require immediate attention (e.g., negative stock, unbooked stock movements, or major margin drops) required running dozens of disparate reports.
* **Lack of Standardized Prioritization:** Branch managers lacked a unified method to gauge the urgency of outstanding items, leading to high-risk errors (such as missing a credit deadline or leaving un-invoiced stock in limbo) being overlooked.
### The Objective
The **Branch Daily Action Report (BDAR)** serves as a comprehensive, ERP-driven operational "Control Tower." It consolidates complex operational anomalies from sales, stock, and ordering into a single interactive menu.
By applying strict Red-Amber-Green (RAG) conditional formatting to localized data, the report gives branch teams an immediate, prioritized action plan the moment they log in each morning—significantly reducing financial risk and streamlining inventory control.
---
## 📈 Report Performance & Engagement
Unlike standard static reports, the BDAR is a mission-critical, high-traffic application deeply embedded in the daily corporate workflow. Power BI service metrics demonstrate exceptional user adoption and engagement across the organizational network:
* **🔥 Total Views:** Over **100,000+ views** accumulated within a active 3-week window.
* **👥 Active Audience:** Utilized consistently by **583 unique users** across the branch and regional network.
* **⚡ Operational Impact:** High view-to-user ratios prove that branch personnel rely on this dashboard as a living, breathing utility throughout their shifts to clear down exceptions.
---
## 🛠️ Tech Stack & Architecture
* **BI Platform:** Microsoft Power BI Desktop / Power BI Service Cloud
* **Data Core:** ERP System Transactional Database
* **UX Framework:** Centralized, metric-driven main menu utilizing custom dynamic bookmarks and targeted page navigation.
* **Architecture Design:** Alphanumeric task classification mapped across 10 core business streams (Accounts, Customers, Security, Terms, Housekeeping, Stock, Returns, Credits, Interbranch, and Management).
---
## 🕹️ Interface & UX Breakdown
The dashboard’s layout (visible in the repository documentation) relies on a high-density, centralized navigation matrix designed for quick decision-making:
### 1. The Central Control Tower Menu
The homepage features a structured grid of clickable KPI cards acting as an intuitive launchpad.
* **Alphanumeric Indexing:** Every business metric is assigned a strict identifier (e.g., `A1. O/S Managers Acc`, `E4. Negative Stock`, `F1. Invoice Queries`) allowing team members to communicate cleanly across branches using unified codes.
* **Drill-Through Actions:** Clicking any individual card instantly navigates the user away from the high-level overview and directly into a dedicated detail page, exposing the raw, line-by-line transactions that require attention.
### 2. High-Impact RAG Prioritization
To prevent critical issues from being lost in a dense layout, the cards are integrated with an automated RAG alert engine:
* <span style="color:#d9534f">**🔴 Red Alerts (Critical Risk):**</span> Immediate threats to margin or compliance (e.g., *Negative Stock*, *Unpaid Invoices*, *No Terms Set*, or *Exhausted Locations*). These demand attention within the first hour of the shift.
* <span style="color:#f0ad4e">**🟡 Amber Alerts (Operational Action Required):**</span> Intermediate anomalies requiring standard daily processing (e.g., *Auto Terms Margins*, *Overdue Quotes*, or *Unbooked Purchase Orders*).
* <span style="color:#5bc0de">**🔵 Blue/Green Cards (Status Tracking):**</span> Baseline informational counters or informational audits (e.g., *Staff Purchases* or *Sample Accounts*).
---
## 📐 Key Functional Streams Tracked
| **Category** | **High-Impact Metrics Captured** |

| **Accounts & Credit** | Unpaid invoices, accounts on stop with inventory pending, over-limit accounts, and rebook tracking. |
| **Terms & Overrides** | Tracking non-standard pricing behaviors, parts sold below 10% margin, and manual override visibility to protect profitability. |
| **Housekeeping & Stock** | Critical pipeline blockages including parts booked in with no location assigned, negative stock values, and unbooked purchase orders. |
| **Returns & Credits** | Processing visibility for RMAs, items on return older than 180 days, and pending inter-branch transfer disputes. |
---
## 💻 Technical Code Highlights
The dashboard needs a reliable count for regional managers to assess how their region is performing by counting the number of reds, Ambers and Blues. Below is an example of the Red count DAX.
```dax
// Example: Dynamically counts the number of buttons that are red, filtered down by Region and Branch
​Total_Reds_Count = 
-- 1. Check if we are looking at a specific branch row or the Total row
IF(
    HASONEVALUE('Branches'[Branch]),
    
    -- --- CODE FOR INDIVIDUAL ROWS ---
    VAR CurrentBranch = SELECTEDVALUE('Branches'[Branch])

    VAR A4_Val = CALCULATE(SUM('BDAR Aggregation Table CUST/Accts'[A4. Unpaid Invoices]), 'BDAR Aggregation Table CUST/Accts'[Branch] = CurrentBranch)
    VAR A5_Val = CALCULATE(SUM('BDAR Aggregation Table CUST/Accts'[A5. Acct off stop then inv]), 'BDAR Aggregation Table CUST/Accts'[Branch] = CurrentBranch)
    VAR C2_Val = CALCULATE(SUM('BDAR Aggregation Table Security'[C2. Hash lowered Cost]), 'BDAR Aggregation Table Security'[Branch] = CurrentBranch)
    VAR C4_Val = CALCULATE(SUM('BDAR Aggregation Table Security'[C4. Crn MAN sell STAFF]), 'BDAR Aggregation Table Security'[Branch] = CurrentBranch)
    VAR D1_Val = CALCULATE(SUM('BDAR Aggregation Table Terms / Overrides'[D1. No Terms set]), 'BDAR Aggregation Table Terms / Overrides'[Branch] = CurrentBranch)
    VAR E1_Val = CALCULATE(SUM('BDAR Aggregation Table Housekeeping'[E1. FPS MAR poss RTN]), 'BDAR Aggregation Table Housekeeping'[Branch] = CurrentBranch)
    VAR E2_Val = CALCULATE(SUM('BDAR Aggregation Table Housekeeping'[E2. PO not booked in]), 'BDAR Aggregation Table Housekeeping'[Branch] = CurrentBranch)
    VAR E3_Val = CALCULATE(SUM('BDAR Aggregation Table Housekeeping'[E3. Outstanding IBTs]), 'BDAR Aggregation Table Housekeeping'[Branch] = CurrentBranch)
    VAR E4_Val = [Count of POs No]
    VAR E5_Val = CALCULATE(SUM('BDAR Aggregation Table Housekeeping'[E5. No Level Stock]), 'BDAR Aggregation Table Housekeeping'[Branch] = CurrentBranch)
    VAR E6_Val = CALCULATE(SUM('BDAR Aggregation Table Housekeeping'[E6. Supply & Cancel POS]), 'BDAR Aggregation Table Housekeeping'[Branch] = CurrentBranch)
    VAR E7_Val = CALCULATE(SUM('BDAR Aggregation Table Housekeeping'[E7. Carriage Charge]), 'BDAR Aggregation Table Housekeeping'[Branch] = CurrentBranch)
    VAR E8_Val = CALCULATE(SUM('BDAR Aggregation Table Housekeeping'[E8. Unused Op Codes]), 'BDAR Aggregation Table Housekeeping'[Branch] = CurrentBranch)
    VAR E9_Val = CALCULATE(SUM('BDAR Aggregation Table Housekeeping'[E9. ZZZ Stock]), 'BDAR Aggregation Table Housekeeping'[Branch] = CurrentBranch)
    VAR E10_Val = CALCULATE(SUM('BDAR Aggregation Table Housekeeping'[E10. Branch Created POs]), 'BDAR Aggregation Table Housekeeping'[Branch] = CurrentBranch)
    VAR E11_Val = CALCULATE(SUM('STOCKTAKING'[Perc]), 'STOCKTAKING'[Branch] = CurrentBranch)
    VAR F1_Val = CALCULATE(SUM('BDAR Aggregation Table Stock'[F1. Invoice Queries]), 'BDAR Aggregation Table Stock'[Branch] = CurrentBranch)
    VAR F2_Val = CALCULATE(SUM('BDAR Aggregation Table Stock'[F2. Invq (Line Level)]), 'BDAR Aggregation Table Stock'[Branch] = CurrentBranch)
    VAR F3_Val = [< 1 Month]
    VAR F4_Val = CALCULATE(SUM('BDAR Aggregation Table Stock'[F4. Star Parts booked in]), 'BDAR Aggregation Table Stock'[Branch] = CurrentBranch)
    VAR F5_Val = CALCULATE(SUM('BDAR Aggregation Table Stock'[F5. Pick Deallocate]), 'BDAR Aggregation Table Stock'[Branch] = CurrentBranch)
    VAR F6_Val = CALCULATE(SUM('BDAR Aggregation Table Stock'[F6. Rem Supersessions]), 'BDAR Aggregation Table Stock'[Branch] = CurrentBranch)
    VAR F7_Val = CALCULATE(SUM('BDAR Aggregation Table Stock'[F7. Exhaust no Location]), 'BDAR Aggregation Table Stock'[Branch] = CurrentBranch)
    VAR F8_Val = CALCULATE(SUM('BDAR Aggregation Table Stock'[F8. Never Stock checked]), 'BDAR Aggregation Table Stock'[Branch] = CurrentBranch)
    VAR F9_Val = CALCULATE(SUM('BDAR Aggregation Table Stock'[F9.  Needs stocktaking]), 'BDAR Aggregation Table Stock'[Branch] = CurrentBranch)
    VAR H1_Val = CALCULATE(SUM('BDAR Aggregation Table Returns Note'[H1. Surch to RTN to CSD]), 'BDAR Aggregation Table Returns Note'[Branch] = CurrentBranch)
    VAR H2_Val = CALCULATE(SUM('BDAR Aggregation Table Returns Note'[H2. RMA missing]), 'BDAR Aggregation Table Returns Note'[Branch] = CurrentBranch)
    VAR H3_Val = CALCULATE(SUM('BDAR Aggregation Table Returns Note'[H3. Returns no Acct No.]), 'BDAR Aggregation Table Returns Note'[Branch] = CurrentBranch)
    VAR H4_Val = CALCULATE(SUM('BDAR Aggregation Table Returns Note'[H4. Rtns & adj back in]), 'BDAR Aggregation Table Returns Note'[Branch] = CurrentBranch)
    VAR H5_Val = CALCULATE(SUM('BDAR Aggregation Table Returns Note'[H5. O/S RTN Notes]), 'BDAR Aggregation Table Returns Note'[Branch] = CurrentBranch)
    VAR H6_Val = CALCULATE(SUM('BDAR Aggregation Table Returns Note'[H6. Return Labour/Own]), 'BDAR Aggregation Table Returns Note'[Branch] = CurrentBranch)
    VAR I2_Val = CALCULATE(SUM('BDAR Aggregation Table BDM'[I2. Sample Account]), 'BDAR Aggregation Table BDM'[Branch] = CurrentBranch)
    VAR K3_Val = CALCULATE(SUM('BDAR Aggregation Table CSD / Interbranch'[K3. Transfer Disputes]), 'BDAR Aggregation Table CSD / Interbranch'[Branch] = CurrentBranch)
    VAR J1_Val = CALCULATE(SUM('BDAR Aggregation Table Credits'[J1 Paid Card / Credited Cash]), 'BDAR Aggregation Table Credits'[Branch] = CurrentBranch)
    VAR J2_Val = CALCULATE(SUM('BDAR Aggregation Table Credits'[J2. Wty Crn no adj/RTN]), 'BDAR Aggregation Table Credits'[Branch] = CurrentBranch)
    VAR J3_Val = CALCULATE(SUM('BDAR Aggregation Table Credits'[J3. Crn Surch then part]), 'BDAR Aggregation Table Credits'[Branch] = CurrentBranch)

    RETURN
        IF(A4_Val > 0, 1, 0) + IF(A5_Val > 0, 1, 0) + IF(C2_Val > 0, 1, 0) + IF(C4_Val > 0, 1, 0) +
        IF(D1_Val > 0, 1, 0) + IF(E1_Val > 0, 1, 0) + IF(E2_Val > 0, 1, 0) + IF(E3_Val > 0, 1, 0) + IF(E4_Val > 0, 1, 0) + IF(E5_Val > 0, 1, 0) +
        IF(E6_Val > 0, 1, 0) + IF(E7_Val > 0, 1, 0) + IF(E8_Val > 0, 1, 0) + IF(E9_Val > 0, 1, 0) + IF(E10_Val > 0, 1, 0) +
        IF(NOT(ISBLANK(E11_Val)) && E11_Val <= 0.9, 1, 0) + IF(F1_Val > 0, 1, 0) + IF(F2_Val > 0, 1, 0) + IF(F3_Val > 0, 1, 0) + IF(F4_Val > 0, 1, 0) +
        IF(F5_Val > 0, 1, 0) + IF(F6_Val > 0, 1, 0) + IF(F7_Val > 0, 1, 0) + IF(F8_Val > 0, 1, 0) + IF(F9_Val > 0, 1, 0) +
        IF(H1_Val > 0, 1, 0) + IF(H2_Val > 0, 1, 0) + IF(H3_Val > 0, 1, 0) + IF(H4_Val > 0, 1, 0) + IF(H5_Val > 0, 1, 0) +
        IF(H6_Val > 0, 1, 0) + IF(I2_Val > 0, 1, 0) + IF(K3_Val > 0, 1, 0) + IF(J1_Val > 0, 1, 0) + IF(J2_Val > 0, 1, 0) +
        IF(J3_Val > 0, 1, 0),

    -- --- CODE FOR THE TOTAL ROW ---
    -- This loops through every branch, calculates their individual row score, and sums them up
    SUMX(
        VALUES('Branches'[Branch]),
        VAR CurrentBranch = 'Branches'[Branch]
        
        VAR A4_Val = CALCULATE(SUM('BDAR Aggregation Table CUST/Accts'[A4. Unpaid Invoices]), 'BDAR Aggregation Table CUST/Accts'[Branch] = CurrentBranch)
        VAR A5_Val = CALCULATE(SUM('BDAR Aggregation Table CUST/Accts'[A5. Acct off stop then inv]), 'BDAR Aggregation Table CUST/Accts'[Branch] = CurrentBranch)
        VAR C2_Val = CALCULATE(SUM('BDAR Aggregation Table Security'[C2. Hash lowered Cost]), 'BDAR Aggregation Table Security'[Branch] = CurrentBranch)
        VAR C4_Val = CALCULATE(SUM('BDAR Aggregation Table Security'[C4. Crn MAN sell STAFF]), 'BDAR Aggregation Table Security'[Branch] = CurrentBranch)
        VAR D1_Val = CALCULATE(SUM('BDAR Aggregation Table Terms / Overrides'[D1. No Terms set]), 'BDAR Aggregation Table Terms / Overrides'[Branch] = CurrentBranch)
        VAR E1_Val = CALCULATE(SUM('BDAR Aggregation Table Housekeeping'[E1. FPS MAR poss RTN]), 'BDAR Aggregation Table Housekeeping'[Branch] = CurrentBranch)
        VAR E2_Val = CALCULATE(SUM('BDAR Aggregation Table Housekeeping'[E2. PO not booked in]), 'BDAR Aggregation Table Housekeeping'[Branch] = CurrentBranch)
        VAR E3_Val = CALCULATE(SUM('BDAR Aggregation Table Housekeeping'[E3. Outstanding IBTs]), 'BDAR Aggregation Table Housekeeping'[Branch] = CurrentBranch)
        VAR E4_Val = [Count of POs No]
        VAR E5_Val = CALCULATE(SUM('BDAR Aggregation Table Housekeeping'[E5. No Level Stock]), 'BDAR Aggregation Table Housekeeping'[Branch] = CurrentBranch)
        VAR E6_Val = CALCULATE(SUM('BDAR Aggregation Table Housekeeping'[E6. Supply & Cancel POS]), 'BDAR Aggregation Table Housekeeping'[Branch] = CurrentBranch)
        VAR E7_Val = CALCULATE(SUM('BDAR Aggregation Table Housekeeping'[E7. Carriage Charge]), 'BDAR Aggregation Table Housekeeping'[Branch] = CurrentBranch)
        VAR E8_Val = CALCULATE(SUM('BDAR Aggregation Table Housekeeping'[E8. Unused Op Codes]), 'BDAR Aggregation Table Housekeeping'[Branch] = CurrentBranch)
        VAR E9_Val = CALCULATE(SUM('BDAR Aggregation Table Housekeeping'[E9. ZZZ Stock]), 'BDAR Aggregation Table Housekeeping'[Branch] = CurrentBranch)
        VAR E10_Val = CALCULATE(SUM('BDAR Aggregation Table Housekeeping'[E10. Branch Created POs]), 'BDAR Aggregation Table Housekeeping'[Branch] = CurrentBranch)
        VAR E11_Val = CALCULATE(SUM('STOCKTAKING'[Perc]), 'STOCKTAKING'[Branch] = CurrentBranch)
        VAR F1_Val = CALCULATE(SUM('BDAR Aggregation Table Stock'[F1. Invoice Queries]), 'BDAR Aggregation Table Stock'[Branch] = CurrentBranch)
        VAR F2_Val = CALCULATE(SUM('BDAR Aggregation Table Stock'[F2. Invq (Line Level)]), 'BDAR Aggregation Table Stock'[Branch] = CurrentBranch)
        VAR F3_Val = [< 1 Month]
        VAR F4_Val = CALCULATE(SUM('BDAR Aggregation Table Stock'[F4. Star Parts booked in]), 'BDAR Aggregation Table Stock'[Branch] = CurrentBranch)
        VAR F5_Val = CALCULATE(SUM('BDAR Aggregation Table Stock'[F5. Pick Deallocate]), 'BDAR Aggregation Table Stock'[Branch] = CurrentBranch)
        VAR F6_Val = CALCULATE(SUM('BDAR Aggregation Table Stock'[F6. Rem Supersessions]), 'BDAR Aggregation Table Stock'[Branch] = CurrentBranch)
        VAR F7_Val = CALCULATE(SUM('BDAR Aggregation Table Stock'[F7. Exhaust no Location]), 'BDAR Aggregation Table Stock'[Branch] = CurrentBranch)
        VAR F8_Val = CALCULATE(SUM('BDAR Aggregation Table Stock'[F8. Never Stock checked]), 'BDAR Aggregation Table Stock'[Branch] = CurrentBranch)
        VAR F9_Val = CALCULATE(SUM('BDAR Aggregation Table Stock'[F9.  Needs stocktaking]), 'BDAR Aggregation Table Stock'[Branch] = CurrentBranch)
        VAR H1_Val = CALCULATE(SUM('BDAR Aggregation Table Returns Note'[H1. Surch to RTN to CSD]), 'BDAR Aggregation Table Returns Note'[Branch] = CurrentBranch)
        VAR H2_Val = CALCULATE(SUM('BDAR Aggregation Table Returns Note'[H2. RMA missing]), 'BDAR Aggregation Table Returns Note'[Branch] = CurrentBranch)
        VAR H3_Val = CALCULATE(SUM('BDAR Aggregation Table Returns Note'[H3. Returns no Acct No.]), 'BDAR Aggregation Table Returns Note'[Branch] = CurrentBranch)
        VAR H4_Val = CALCULATE(SUM('BDAR Aggregation Table Returns Note'[H4. Rtns & adj back in]), 'BDAR Aggregation Table Returns Note'[Branch] = CurrentBranch)
        VAR H5_Val = CALCULATE(SUM('BDAR Aggregation Table Returns Note'[H5. O/S RTN Notes]), 'BDAR Aggregation Table Returns Note'[Branch] = CurrentBranch)
        VAR H6_Val = CALCULATE(SUM('BDAR Aggregation Table Returns Note'[H6. Return Labour/Own]), 'BDAR Aggregation Table Returns Note'[Branch] = CurrentBranch)
        VAR I2_Val = CALCULATE(SUM('BDAR Aggregation Table BDM'[I2. Sample Account]), 'BDAR Aggregation Table BDM'[Branch] = CurrentBranch)
        VAR K3_Val = CALCULATE(SUM('BDAR Aggregation Table CSD / Interbranch'[K3. Transfer Disputes]), 'BDAR Aggregation Table CSD / Interbranch'[Branch] = CurrentBranch)
        VAR J1_Val = CALCULATE(SUM('BDAR Aggregation Table Credits'[J1 Paid Card / Credited Cash]), 'BDAR Aggregation Table Credits'[Branch] = CurrentBranch)
        VAR J2_Val = CALCULATE(SUM('BDAR Aggregation Table Credits'[J2. Wty Crn no adj/RTN]), 'BDAR Aggregation Table Credits'[Branch] = CurrentBranch)
        VAR J3_Val = CALCULATE(SUM('BDAR Aggregation Table Credits'[J3. Crn Surch then part]), 'BDAR Aggregation Table Credits'[Branch] = CurrentBranch)
        
        RETURN
            IF(A4_Val > 0, 1, 0) + IF(A5_Val > 0, 1, 0) + IF(C2_Val > 0, 1, 0) + IF(C4_Val > 0, 1, 0) +
            IF(D1_Val > 0, 1, 0) + IF(E1_Val > 0, 1, 0) + IF(E2_Val > 0, 1, 0) + IF(E3_Val > 0, 1, 0) + IF(E4_Val > 0, 1, 0) + IF(E5_Val > 0, 1, 0) +
            IF(E6_Val > 0, 1, 0) + IF(E7_Val > 0, 1, 0) + IF(E8_Val > 0, 1, 0) + IF(E9_Val > 0, 1, 0) + IF(E10_Val > 0, 1, 0) +
            IF(NOT(ISBLANK(E11_Val)) && E11_Val <= 0.9, 1, 0) + IF(F1_Val > 0, 1, 0) + IF(F2_Val > 0, 1, 0) + IF(F3_Val > 0, 1, 0) + IF(F4_Val > 0, 1, 0) +
            IF(F5_Val > 0, 1, 0) + IF(F6_Val > 0, 1, 0) + IF(F7_Val > 0, 1, 0) + IF(F8_Val > 0, 1, 0) + IF(F9_Val > 0, 1, 0) +
            IF(H1_Val > 0, 1, 0) + IF(H2_Val > 0, 1, 0) + IF(H3_Val > 0, 1, 0) + IF(H4_Val > 0, 1, 0) + IF(H5_Val > 0, 1, 0) +
            IF(H6_Val > 0, 1, 0) + IF(I2_Val > 0, 1, 0) + IF(K3_Val > 0, 1, 0) + IF(J1_Val > 0, 1, 0) + IF(J2_Val > 0, 1, 0) +
            IF(J3_Val > 0, 1, 0)
    )
)
