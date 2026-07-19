# Ready for Breakout Pro

**Ready for Breakout Pro** is a single-file `index.html` web application for screening potential stock breakout candidates and tracking shortlisted positions in a cloud-synced Sandbox Trading portfolio.

The application is designed as a professional, dark-theme stock screener inspired by institutional technical analysis workflows, breakout investing principles, and trend-following methodologies such as CAN SLIM, Minervini-style trend templates, VCP, Darvas Box, Wyckoff accumulation concepts, Pocket Pivot behavior, and relative strength analysis.

The current version integrates **Supabase Auth** and **Supabase Database** for real login, email verification, password reset, cloud favorites, and cloud Sandbox Trading sync.

---

## Table of Contents

1. [Project Overview](#project-overview)
2. [Live App](#live-app)
3. [Main Features](#main-features)
4. [Technology Stack](#technology-stack)
5. [File Structure](#file-structure)
6. [Supabase Integration](#supabase-integration)
7. [Database Tables](#database-tables)
8. [Row Level Security Policies](#row-level-security-policies)
9. [Authentication Flow](#authentication-flow)
10. [Password Reset Flow](#password-reset-flow)
11. [Sandbox Trading Cloud Sync](#sandbox-trading-cloud-sync)
12. [India and USA Sandbox Separation](#india-and-usa-sandbox-separation)
13. [TradingView Integration](#tradingview-integration)
14. [Breakout Scoring Engine](#breakout-scoring-engine)
15. [User Actions](#user-actions)
16. [Export and Backup](#export-and-backup)
17. [Deployment to GitHub Pages](#deployment-to-github-pages)
18. [Testing Checklist](#testing-checklist)
19. [Known Limitations](#known-limitations)
20. [Future Enhancement Roadmap](#future-enhancement-roadmap)
21. [Security Notes](#security-notes)
22. [Troubleshooting](#troubleshooting)
23. [Disclaimer](#disclaimer)

---

## Project Overview

Ready for Breakout Pro is built as a **single standalone HTML file**:

```text
index.html
```

The file contains:

- HTML layout
- CSS styling
- JavaScript application logic
- Supabase authentication integration
- Supabase database sync logic
- Screener data model
- Breakout scoring logic
- Sandbox portfolio logic
- Export logic

The project does not require a local build system, Node.js, React, Vite, Webpack, or backend server.

It can be hosted directly on:

- GitHub Pages
- Netlify
- Vercel static hosting
- Cloudflare Pages
- Any static web hosting platform

---

## Live App

Current expected GitHub Pages URL:

```text
https://lsethu85-lab.github.io/BreakoutPro/
```

If the GitHub username or repository name changes, update the Supabase redirect URL accordingly.

---

## Main Features

### Authentication

- Supabase email login
- Supabase account registration
- Email verification
- Password reset email
- New password update flow
- Session restore after refresh
- Logout

### Screeners

- India screener for NSE-style tickers
- USA screener for NASDAQ/NYSE-style tickers
- Search filter
- Minimum score filter
- Signal filter
- Sector filter
- Sortable table columns
- Scan button
- Refresh technicals button

### Breakout Engine

The app calculates a weighted breakout score using:

- Trend score
- Momentum score
- Volume score
- Pattern score
- Smart money score
- Relative strength score

### Sandbox Trading

- Add stocks to Sandbox
- Separate India Sandbox
- Separate USA Sandbox
- Edit entry price
- Edit quantity
- Sell partial
- Sell all
- Delete position
- Monitor positions
- Calculate unrealized profit/loss
- Calculate realized profit/loss
- Calculate total return
- Track highest gain
- Track lowest drawdown
- Display recommended action

### Cloud Sync

Cloud sync is handled through Supabase tables:

- `sandbox_positions`
- `favorites`

Each user sees only their own data through Row Level Security.

### Other Features

- TradingView chart links
- Favorite/watchlist sync
- CSV export
- Excel-compatible export
- JSON backup export
- JSON backup import
- Responsive design
- Professional dark theme

---

## Technology Stack

```text
Frontend:       HTML, CSS, JavaScript
Auth:           Supabase Auth
Database:       Supabase PostgreSQL
Cloud Sync:     Supabase JavaScript SDK
Hosting:        GitHub Pages
Charts:         TradingView redirect links
Storage:        Supabase cloud database
Backup:         JSON export/import
```

The app uses the Supabase JavaScript SDK from CDN:

```html
<script src="https://cdn.jsdelivr.net/npm/@supabase/supabase-js@2"></script>
```

---

## File Structure

Current project structure:

```text
BreakoutPro/
  index.html
  README.md
```

The entire application is inside `index.html`.

Recommended future structure if the app grows:

```text
BreakoutPro/
  index.html
  README.md
  docs/
    database-schema.sql
    supabase-setup.md
  assets/
    screenshots/
```

If the application is later converted into a larger framework project:

```text
BreakoutPro/
  public/
    index.html
  src/
    components/
    engines/
    services/
    styles/
  README.md
```

---

## Supabase Integration

The following Supabase project is configured in `index.html`:

```javascript
const SUPABASE_URL = "https://ezqhoxwvlgxvbkcrsjrk.supabase.co";
```

The app also includes the Supabase anon public key:

```javascript
const SUPABASE_ANON_KEY = "YOUR_SUPABASE_ANON_KEY";
```

The anon key is allowed in frontend code only when Row Level Security is properly configured.

Never place the Supabase `service_role` key inside `index.html`.

---

## Database Tables

The application depends on two Supabase tables.

### `sandbox_positions`

Stores Sandbox Trading positions.

Main fields:

```text
id
user_id
market
ticker
company
exchange
entry_price
current_price
quantity
stop_loss
target_1
target_2
breakout_score
technical_score
buy_signal
current_signal
realized_profit
highest_price
lowest_price
buy_date
created_at
updated_at
```

### `favorites`

Stores favorite/watchlist tickers.

Main fields:

```text
id
user_id
ticker
market
created_at
```

---

## SQL Setup

Use the following SQL in Supabase SQL Editor.

### Create Tables

```sql
create table if not exists sandbox_positions (
  id uuid primary key default gen_random_uuid(),
  user_id uuid not null references auth.users(id) on delete cascade,

  market text not null,
  ticker text not null,
  company text,
  exchange text,

  entry_price numeric,
  current_price numeric,
  quantity numeric,

  stop_loss numeric,
  target_1 numeric,
  target_2 numeric,

  breakout_score numeric,
  technical_score numeric,

  buy_signal text,
  current_signal text,

  realized_profit numeric default 0,
  highest_price numeric,
  lowest_price numeric,

  buy_date date default current_date,
  created_at timestamptz default now(),
  updated_at timestamptz default now()
);

create table if not exists favorites (
  id uuid primary key default gen_random_uuid(),
  user_id uuid not null references auth.users(id) on delete cascade,
  ticker text not null,
  market text not null,
  created_at timestamptz default now(),

  unique(user_id, ticker)
);
```

---

## Row Level Security Policies

Row Level Security ensures every authenticated user can only read and modify their own rows.

### Enable RLS

```sql
alter table sandbox_positions enable row level security;
alter table favorites enable row level security;
```

### Sandbox Policies

```sql
create policy "Users can read own sandbox positions"
on sandbox_positions
for select
using (auth.uid() = user_id);

create policy "Users can insert own sandbox positions"
on sandbox_positions
for insert
with check (auth.uid() = user_id);

create policy "Users can update own sandbox positions"
on sandbox_positions
for update
using (auth.uid() = user_id)
with check (auth.uid() = user_id);

create policy "Users can delete own sandbox positions"
on sandbox_positions
for delete
using (auth.uid() = user_id);
```

### Favorites Policies

```sql
create policy "Users can read own favorites"
on favorites
for select
using (auth.uid() = user_id);

create policy "Users can insert own favorites"
on favorites
for insert
with check (auth.uid() = user_id);

create policy "Users can delete own favorites"
on favorites
for delete
using (auth.uid() = user_id);
```

### Verify Policies

```sql
select
  tablename,
  policyname,
  cmd
from pg_policies
where schemaname = 'public'
order by tablename, policyname;
```

Expected tables:

```text
favorites
sandbox_positions
```

---

## Authentication Flow

### Login

The login page uses:

```text
Email
Password
```

When the user logs in, the app:

1. Calls Supabase Auth.
2. Receives the logged-in user session.
3. Stores the user ID in app state.
4. Loads cloud Sandbox positions from Supabase.
5. Loads cloud favorites from Supabase.
6. Opens the dashboard.

Relevant function:

```javascript
loginWithSupabase(email, password)
```

### Registration

The registration page uses:

```text
Email
Password
```

When the user creates an account, Supabase sends a verification email.

Relevant function:

```javascript
registerWithSupabase(email, password)
```

### Email Verification

After registration:

1. The user receives a Supabase verification email.
2. The user clicks the verification link.
3. Supabase redirects the user back to the GitHub Pages app.
4. The user can then log in.

Supabase redirect URL must be configured correctly.

---

## Password Reset Flow

The password reset flow has two screens.

### Step 1: Request Reset Email

The reset request screen shows:

```text
Email only
Send Reset Email
```

Relevant function:

```javascript
resetPasswordSupabase(email)
```

Expected behavior:

1. User clicks **Forgot password?**
2. User enters email.
3. User clicks **Send Reset Email**.
4. Supabase sends a password reset email.

### Step 2: Set New Password

After clicking the reset email link, Supabase redirects back to the app.

The app listens for:

```javascript
PASSWORD_RECOVERY
```

Then the screen changes to:

```text
New Password
Update Password
```

Relevant function:

```javascript
updatePasswordSupabase(newPassword)
```

Expected behavior:

1. User enters new password.
2. User clicks **Update Password**.
3. Password is updated in Supabase.
4. User logs in again with the new password.

---

## Supabase Redirect Configuration

In Supabase dashboard:

```text
Authentication → URL Configuration
```

Set **Site URL**:

```text
https://lsethu85-lab.github.io/BreakoutPro/
```

Add **Redirect URL**:

```text
https://lsethu85-lab.github.io/BreakoutPro/
```

If testing locally, add:

```text
http://localhost:5500
http://127.0.0.1:5500
```

---

## Sandbox Trading Cloud Sync

Sandbox data is saved to Supabase whenever the user performs one of these actions:

- Add to Sandbox
- Edit entry price
- Edit quantity
- Sell partial
- Sell all
- Delete position
- Monitor Sandbox
- Clear current market Sandbox
- Clear all Sandbox positions
- Import backup

Local browser storage is no longer the primary source of truth. Supabase is the cloud source of truth.

---

## India and USA Sandbox Separation

Sandbox Trading is split into two independent views:

```text
India Sandbox
USA Sandbox
```

### India Sandbox

Stores positions where:

```text
market = 'india'
exchange = 'NSE'
```

### USA Sandbox

Stores positions where:

```text
market = 'usa'
exchange = 'NASDAQ' or 'NYSE'
```

The user can monitor, export, and clear each market independently.

---

## TradingView Integration

Every screener row and Sandbox row includes a mini chart link.

The link opens TradingView using the ticker symbol.

Examples:

```text
NSE:RELIANCE
NASDAQ:NVDA
NYSE:PLTR
```

Generated URL format:

```text
https://www.tradingview.com/chart/?symbol=NSE%3ARELIANCE
```

---

## Breakout Scoring Engine

The app uses a weighted scoring model:

```javascript
const Config = {
  weights: {
    trend: .20,
    momentum: .15,
    volume: .15,
    pattern: .20,
    smartMoney: .15,
    rs: .15
  },
  risk: {
    atrStopMultiple: 1.65,
    target1R: 2,
    target2R: 3.5
  }
};
```

Scoring categories:

```text
Trend:             20%
Pattern:           20%
Momentum:          15%
Volume:            15%
Smart Money:       15%
Relative Strength: 15%
```

Breakout score tiers:

```text
95-100  Elite Breakout Candidate
90-94   High Probability
80-89   Watchlist
70-79   Early Setup
Below 70 Ignore
```

---

## Screener Columns

The screener table includes:

```text
Chart
Company
Ticker
Exchange
Sector
Industry
Current Price
Breakout Score
Buy Score
Technical Score
Volume Score
Momentum Score
Trend Score
Relative Strength
Smart Money Score
Pattern Name
Confidence %
Signal
Suggested Entry
Stop Loss
Target 1
Target 2
Risk Reward
Days in Base
RSI
MACD
SuperTrend
EMA Alignment
ATR
Volume Ratio
Market Cap
Actions
```

---

## User Actions

### Screener Actions

Each stock row has:

```text
Chart link
Add to Sandbox
Favorite
Refresh ticker
```

### Sandbox Actions

Each Sandbox row has:

```text
Edit entry price
Edit quantity
Sell partial
Sell all
Delete position
Open TradingView chart
```

### Portfolio Actions

Available actions:

```text
Monitor current market
Export current market CSV
Export current market Excel
Clear current market
Clear all
Export backup JSON
Import backup JSON
```

---

## Export and Backup

The app supports:

```text
CSV export
Excel-compatible export
JSON backup export
JSON backup import
```

### CSV Export

Exports visible screener or Sandbox data to `.csv`.

### Excel Export

Exports HTML table content as `.xls` for Excel-compatible opening.

### JSON Backup Export

Creates a complete backup of:

```text
favorites
sandbox
sandboxMarket
export timestamp
user email
```

### JSON Backup Import

Imports backup data and syncs positions/favorites to Supabase.

---

## Deployment to GitHub Pages

### Step 1: Upload Files

Upload these files to the root of your GitHub repository:

```text
index.html
README.md
```

### Step 2: Enable GitHub Pages

In GitHub:

```text
Repository → Settings → Pages
```

Set:

```text
Source: Deploy from branch
Branch: main
Folder: /root
```

### Step 3: Confirm URL

Expected app URL:

```text
https://lsethu85-lab.github.io/BreakoutPro/
```

### Step 4: Hard Refresh

After uploading a new `index.html`, hard refresh:

```text
Ctrl + F5
```

Or open in an incognito/private browser window.

---

## Testing Checklist

### Authentication

```text
[ ] Create account works
[ ] Verification email arrives
[ ] Verification link returns to app
[ ] Login works
[ ] Session persists after refresh
[ ] Logout works
```

### Password Reset

```text
[ ] Forgot password screen shows email only
[ ] Reset email is sent
[ ] Reset link returns to app
[ ] Set New Password screen appears
[ ] New password saves successfully
[ ] Login works with new password
```

### Supabase Data

```text
[ ] Add NSE stock to India Sandbox
[ ] Add NASDAQ/NYSE stock to USA Sandbox
[ ] Refresh page
[ ] Login again
[ ] Positions reload from Supabase
[ ] Favorite ticker saves to Supabase
[ ] Favorite ticker reloads after login
```

### Sandbox

```text
[ ] Edit quantity
[ ] Edit entry price
[ ] Sell partial
[ ] Sell all
[ ] Delete position
[ ] Monitor India Sandbox
[ ] Monitor USA Sandbox
[ ] Clear India only
[ ] Clear USA only
[ ] Clear all
```

### Export

```text
[ ] Export screener CSV
[ ] Export screener Excel
[ ] Export Sandbox CSV
[ ] Export Sandbox Excel
[ ] Export JSON backup
[ ] Import JSON backup
```

---

## Known Limitations

The current app is a professional prototype and does not yet use live market data.

Current limitations:

```text
Sample stock data only
No live NSE/BSE feed
No live NASDAQ/NYSE feed
No real historical OHLCV indicator calculation
No broker integration
No automated trading
No server-side backtesting
No real notification system
No AI ranking engine
No fundamental analysis engine
```

The screener logic is deterministic and based on predefined sample data.

---

## Future Enhancement Roadmap

Recommended next features:

```text
Live market data integration
Real OHLCV technical indicator engine
Historical candle storage
Backtesting module
Sector strength model
Earnings and revenue growth filters
News and sentiment module
AI ranking model
Push/email/Telegram alerts
Admin dashboard
User settings page
Portfolio analytics dashboard
Watchlist grouping
Import custom tickers
Broker API integration
```

Recommended future modules:

```text
MarketDataEngine
TechnicalAnalysisEngine
PatternRecognitionEngine
BreakoutScoringEngine
RelativeStrengthEngine
InstitutionalActivityEngine
SandboxPortfolioService
SupabaseDataService
AuthService
ExportService
AlertService
BacktestingEngine
```

---

## Security Notes

### Safe to expose in frontend

```text
Supabase Project URL
Supabase anon public key
```

Only safe when RLS is enabled and correct policies are configured.

### Never expose in frontend

```text
Supabase service_role key
Database password
Private API keys
Broker API secrets
Paid data vendor secret keys
```

### Important

If the `service_role` key is ever accidentally committed to GitHub, rotate the key immediately in Supabase.

---

## Troubleshooting

### Password reset screen shows password field

Use the latest `index.html`. The fixed reset screen shows only the email field during reset request.

### Email verification not received

Check:

```text
Spam folder
Supabase Authentication → Providers → Email
Correct email address
Supabase email rate limits
```

### Reset link does not return to the app

Check:

```text
Authentication → URL Configuration
Site URL
Redirect URLs
```

The GitHub Pages URL must match exactly.

### Supabase says row violates RLS policy

Check:

```text
user_id is being inserted as auth user ID
RLS policies exist
User is logged in
Anon key is being used, not service_role key
```

### Sandbox does not reload after login

Check:

```text
sandbox_positions table exists
RLS select policy exists
User is logged in
Browser console for Supabase errors
```

### Favorites do not save

Check:

```text
favorites table exists
unique(user_id, ticker) exists
insert/delete policies exist
User is logged in
```

### Website still shows old version

Try:

```text
Ctrl + F5
Incognito window
Clear browser cache
Wait 1-2 minutes after GitHub Pages deployment
```

---

## Development Notes

The app is intentionally written as a single file.

Main JavaScript sections:

```text
Supabase config
Auth state
Sample market data
Scoring engine
Cloud load/save functions
Screener rendering
Sandbox rendering
Portfolio calculations
Export/backup logic
Utility functions
App initialization
```

If the file becomes too large, the app can later be split into modules.

---

## Disclaimer

This application is for educational, research, and paper-trading purposes only.

The screener does not provide financial advice, investment recommendations, or guaranteed trading signals.

All trading and investing decisions involve risk. Validate all strategies independently before using real capital.

---

## Version Summary

Current version includes:

```text
Single-file index.html
Professional dark dashboard
Supabase Auth
Email verification
Password reset
Supabase cloud Sandbox sync
Supabase cloud favorites sync
India Screener
USA Screener
India Sandbox
USA Sandbox
TradingView links
CSV export
Excel export
JSON backup/import
Responsive layout
```
