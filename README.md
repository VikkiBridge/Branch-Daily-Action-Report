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
| Category | High-Impact Metrics Captured |
| :--- | :--- |
| **Accounts & Credit** | Unpaid invoices, accounts on stop with inventory pending, over-limit accounts, and rebook tracking. |
| **Terms & Overrides** | Tracking non-standard pricing behaviors, parts sold below 10% margin, and manual override visibility to protect profitability. |
| **Housekeeping & Stock** | Critical pipeline blockages including parts booked in with no location assigned, negative stock values, and unbooked purchase orders. |
| **Returns & Credits** | Processing visibility for RMAs, items on return older than 180 days, and pending inter-branch transfer disputes. |
---
## 💻 Technical Code Highlights
The dashboard relies on clean DAX measures that parse transactional tables to return localized exception counts based on the active branch filter context:
```dax
// Example: Dynamically flags active items where the physical stock has dropped below zero
Negative Stock Count =
CALCULATE(
   COUNTROWS('StockItem'),
   'StockItem'[PhysicalStock] < 0,
   'StockItem'[Status] = "Active"
)
