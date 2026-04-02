# Finium — Finance Dashboard

A polished, interactive personal finance dashboard built with **vanilla HTML/CSS/JavaScript**. Zero build tools, zero install steps — open `index.html` in any browser and it works instantly.

---

## 🚀 Setup & Running

```bash
# Option 1 — just open the file
open index.html

# Option 2 — serve locally (any static server)
npx serve .
# or
python -m http.server 8080
```

No Node, no npm, no bundler required.

---

## 📋 Assignment Coverage

| Requirement | Status | Details |
|---|---|---|
| Summary cards | ✅ | Balance, Income, Expenses, Savings Rate with animated count-up |
| Time-based chart | ✅ | Balance trend line chart — switchable 3M / 6M / 1Y |
| Category chart | ✅ | Donut chart with live legend, top 6 spend categories |
| Transaction list | ✅ | Paginated table (10/page), all fields shown |
| Filtering | ✅ | Search, type, category, month dropdown + date range picker |
| Sorting | ✅ | Click any column header — toggles asc/desc |
| Role-based UI | ✅ | Admin (full CRUD + export) vs Viewer (read-only) |
| Insights section | ✅ | 6 smart cards + bar chart + MoM chart + category spend bars |
| State management | ✅ | Central S state object, no framework needed |
| Responsiveness | ✅ | Mobile hamburger menu, adaptive grids, hidden columns on small screens |
| Dark mode | ✅ | Toggle in topbar, persisted to localStorage |
| Data persistence | ✅ | All transactions + budgets + theme + role saved to localStorage |
| Export | ✅ | CSV and JSON export of current filtered results |
| Empty states | ✅ | Handled on all pages with friendly messages |
| Animations | ✅ | Page transitions, number count-up, budget bar fills, toast slide-ins |

---

## ✨ Features by Page

### 1. Overview
- 4 animated summary cards (Balance, Income, Expenses, Savings Rate)
- Balance Trend line chart switchable between 3M / 6M / 1Y
- Spending Donut chart — top 6 categories with color legend
- Recent Activity list — last 6 transactions

### 2. Transactions
- Paginated table, 10 rows/page, smart pagination controls
- 5 filters: text search, type, category, month, from/to date range
- Sortable columns: Description, Date, Category, Amount
- Admin: Edit and Delete per row, Add Transaction button, CSV and JSON export
- Viewer: read-only mode, action buttons hidden, info banner shown
- Confirmation dialog before deletes
- Inline form validation with toast error messages

### 3. Budget Tracker
- Set monthly limits per category (Admin only — click edit icon on card)
- Progress bars with color states: on-track, near-limit (75%+), over-budget
- 4 summary cards: Total Budget, Month Spent, Remaining, % Used
- Alert banners and sidebar badge for over/near-limit categories
- Reset to default budgets option

### 4. Insights
- 6 insight cards: Top Category, Savings Rate, Avg Monthly Spend, Month-over-Month, Largest Transaction, Income Diversity
- Status badges (healthy / warning / neutral) with contextual descriptions
- Monthly Income vs Expenses grouped bar chart (6 months)
- Month-over-Month expense comparison chart (3 months)
- Top expense categories with horizontal progress bars

### 5. Role-Based UI

| Feature | Admin | Viewer |
|---|---|---|
| View all data | Yes | Yes |
| Add / Edit / Delete transaction | Yes | No |
| Export CSV / JSON | Yes | No |
| Edit budget limits | Yes | No |
| Read-only banner | No | Yes |

Switch via the Role dropdown in the sidebar. Selection persists across refreshes.

---

## 🏗 Technical Approach

### State Management

All UI state lives in a single `S` object:

```javascript
S = {
  txns: [],           // transaction records (persisted to localStorage)
  budgets: {},        // category budget limits (persisted)
  role: 'admin',      // 'admin' | 'viewer' (persisted)
  theme: 'dark',      // 'dark' | 'light' (persisted)
  sort: { key, dir }, // active sort + direction
  filters: { search, type, category, month, from, to },
  page: 1,            // pagination page
  pp: 10,             // rows per page
  period: '3M',       // trend chart period
  editId: null,
  delId: null,
}
```

Mutations call `sv()` or `svB()` to write to localStorage. A single `refresh()` function re-renders all panels after any data change.

### RBAC

CSS rule `body.viewer .admin-only { display: none }` hides all mutating UI on role switch instantly. No JavaScript overhead for the layout changes.

### Charts

All charts use Chart.js 4.4. Instances are stored in a `CH` object and destroyed before re-render to avoid canvas memory leaks. Colors adapt automatically on theme switch.

---

## 🎨 Design

- Dark theme: `#0b0d12` background, `#c8f250` lime green accent
- Light theme: `#f2f4f8` background, same accent
- Fonts: Playfair Display (headings) + DM Sans (body) + DM Mono (numbers/dates)
- Color language: Teal = income, Red = expense, Green = balance/savings

---

## 📂 Structure

```
finance-dashboard/
├── index.html    ← entire app (self-contained, ~800 lines)
└── README.md
```

Single-file app for maximum portability. No build step, no external assets beyond Google Fonts and Chart.js CDN.

---

## 📦 Seed Data

57 realistic transactions pre-loaded across 6 months (Nov 2023–Apr 2024) covering: Salary, Freelance, Rent, Food, Transport, Utilities, Health, Shopping, Entertainment, Education, Investment.

Data persists via `localStorage`. Clear browser storage to reset to defaults.
