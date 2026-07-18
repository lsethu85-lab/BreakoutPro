# Ready for Breakout Pro

A single-file, professional dark-theme stock screening and sandbox trading dashboard designed to identify and monitor institutional-quality breakout candidates across the **Indian** and **US** equity markets.

The application is delivered as a standalone `index.html` file and includes:

- 🇮🇳 India breakout screener for NSE/BSE-style candidates
- 🇺🇸 USA breakout screener for NYSE/NASDAQ-style candidates
- 📊 Separate Sandbox Trading workspaces for India and USA
- 🚀 Proprietary "Ready for Breakout" scoring engine
- TradingView ticker links through mini chart buttons
- Watchlist/favorite support
- Paper-trading portfolio monitoring
- CSV and Excel export
- Responsive professional dashboard UI

---

## Important Disclaimer

This dashboard is a **front-end prototype and educational stock screening tool**. It does **not** provide financial advice, investment recommendations, or guaranteed trading signals.

The current version uses local sample data and deterministic scoring logic. For live trading or investment use, connect a reliable market-data provider, validate all calculations, backtest the strategy, and consult a qualified financial professional.

---

## Files

```text
index.html   # Main application file
README.md    # Project documentation
```

The application is intentionally built as a single HTML file with embedded CSS and JavaScript, as requested.

---

## Core Features

### 1. India Screener

The India tab screens NSE/BSE-style candidates using a weighted breakout scoring model.

Included features:

- Scan India button
- Refresh Technicals button
- Search and advanced filters
- Sortable screener table
- TradingView mini chart link per ticker
- Add to India Sandbox
- Favorite/watchlist button
- Individual ticker refresh button

Example TradingView format:

```text
NSE:RELIANCE
NSE:TATAMOTORS
NSE:HDFCBANK
```

---

### 2. USA Screener

The USA tab screens NYSE/NASDAQ-style candidates using the same institutional breakout engine.

Included features:

- Scan USA button
- Refresh Technicals button
- Search and advanced filters
- Sortable screener table
- TradingView mini chart link per ticker
- Add to USA Sandbox
- Favorite/watchlist button
- Individual ticker refresh button

Example TradingView format:

```text
NASDAQ:NVDA
NYSE:PLTR
NASDAQ:MSFT
```

---

### 3. Ready for Breakout Engine

The application includes a configurable weighted scoring model inspired by leading breakout and trend-following methodologies, including:

- CAN SLIM-style leadership and momentum concepts
- Minervini-style trend template logic
- Volatility Contraction Pattern concepts
- Darvas Box breakout logic
- Weinstein stage-analysis principles
- Wyckoff accumulation concepts
- Pocket Pivot-style volume behavior
- Relative strength ranking
- Smart money accumulation estimation

Default scoring weights:

```javascript
Trend:             20%
Pattern:           20%
Momentum:          15%
Volume:            15%
Smart Money:       15%
Relative Strength: 15%
```

Breakout tiers:

```text
95-100  = Elite Breakout Candidate
90-94   = High Probability
80-89   = Watchlist
70-79   = Early Setup
Below 70 = Ignore
```

---

## Screener Table Columns

The screener table includes institutional-style columns such as:

- Chart
- Company
- Ticker
- Exchange
- Sector
- Industry
- Current Price
- Breakout Score
- Buy Score
- Technical Score
- Volume Score
- Momentum Score
- Trend Score
- Relative Strength
- Smart Money Score
- Pattern Name
- Confidence %
- Buy / Wait / Avoid Signal
- Suggested Entry Price
- Stop Loss
- Target 1
- Target 2
- Risk Reward Ratio
- Days in Base
- RSI
- MACD
- SuperTrend
- EMA Alignment
- ATR
- Volume Ratio
- Market Cap
- Actions

---

## TradingView Mini Chart Links

Each stock row includes a compact chart-style button.

Clicking the button opens TradingView in a new browser tab using the ticker symbol.

Example generated URL:

```text
https://www.tradingview.com/chart/?symbol=NSE%3ARELIANCE
```

---

## Sandbox Trading

The Sandbox Trading tab is split into two independent workspaces:

### India Sandbox

Tracks only India/NSE-style positions.

### USA Sandbox

Tracks only USA/NYSE/NASDAQ-style positions.

When a stock is added from the India screener, it is automatically assigned to the India Sandbox. When a stock is added from the USA screener, it is automatically assigned to the USA Sandbox.

Each Sandbox workspace has its own:

- Total investment
- Current portfolio value
- Unrealized profit/loss
- Realized profit/loss
- Total return %
- Win rate
- Best performer
- Worst performer
- Active trades
- Market-specific monitoring button
- Market-specific CSV export
- Market-specific Excel export
- Clear current market button

---

## Sandbox Position Fields

Each sandbox position includes:

- Company
- Ticker
- Buy Date
- Entry Price
- Current Price
- Editable Quantity
- Investment Amount
- Current Value
- Unrealized Profit/Loss
- Realized Profit/Loss
- Total Return %
- Holding Days
- Highest Gain
- Lowest Drawdown
- Buy Signal
- Current Signal
- Recommended Action
- Stop Loss
- Target 1
- Target 2
- Breakout Score
- Technical Score
- Chart Button
- Position Actions

---

## Sandbox Actions

Supported actions include:

- Edit quantity
- Edit entry price
- Sell partial
- Sell all
- Delete position
- Monitor current market
- Export current market to CSV
- Export current market to Excel
- Clear current market
- Clear all Sandbox positions

---

## Automatic Monitoring Signals

The Sandbox monitoring engine generates action labels such as:

```text
🟢 Buy
🟡 Hold
🔵 Add on Pullback
🟠 Take Partial Profit
🟣 Target 2 Achieved
🔴 Stop Loss Hit
```

The current implementation simulates price updates locally. For production use, replace this behavior with live price updates from a market-data API.

---

## Local Persistence

The application stores watchlist and Sandbox data in browser `localStorage`.

Stored items include:

```text
rbp_favorites
rbp_sandbox
```

This means positions and favorites remain available after refreshing the browser, as long as browser storage is not cleared.

---

## How to Run

No server is required.

1. Download or copy `index.html`.
2. Open it directly in a modern browser.
3. Use the India or USA screener.
4. Click `⭐ Add` to add candidates to Sandbox Trading.
5. Open the Sandbox tab and switch between India Sandbox and USA Sandbox.

Recommended browsers:

- Microsoft Edge
- Google Chrome
- Brave
- Firefox

---

## Current Limitations

The current version is a polished single-file prototype and uses sample data.

It does not yet include:

- Live NSE/BSE market data
- Live NYSE/NASDAQ market data
- Real historical OHLCV candles
- Real-time indicator recalculation from candle data
- Backend database
- Authentication
- Cloud sync
- Broker integration
- Automated order placement
- Full backtesting engine
- News, earnings, or fundamental analysis

---

## Recommended Production Extensions

Future enhancements can be added without redesigning the UI architecture.

Suggested modules:

```text
MarketDataEngine
TechnicalAnalysisEngine
PatternRecognitionEngine
InstitutionalScoringEngine
RelativeStrengthEngine
ReadyForBreakoutEngine
PortfolioSandbox
WatchlistManager
TradingViewIntegration
ExportEngine
AlertEngine
BacktestingEngine
FundamentalAnalysisEngine
NewsAndEarningsEngine
AIRankingEngine
```

---

## Suggested Live Data Integrations

For a production system, connect one or more market-data providers.

Possible integrations:

- NSE/BSE data vendor
- Yahoo Finance-compatible API
- Alpha Vantage
- Twelve Data
- Polygon.io
- Finnhub
- IEX Cloud
- Interactive Brokers API
- Zerodha Kite Connect
- TradingView-compatible symbol mapping

Make sure to review each provider's terms of service, data limits, licensing restrictions, and exchange requirements.

---

## Technical Architecture

The single-file application is organized conceptually into these layers:

```text
UI Layer
  - Dashboard rendering
  - Screener table
  - Sandbox table
  - Filters and sorting
  - Toast notifications

Business Logic Layer
  - Scan actions
  - Refresh actions
  - Watchlist management
  - Export handling

Technical Engine Layer
  - Weighted breakout scoring
  - Pattern scoring
  - Entry/stop/target calculation
  - Relative strength display

Sandbox Layer
  - Position tracking
  - P/L calculation
  - Market-specific portfolio metrics
  - Monitoring alert logic

Persistence Layer
  - localStorage favorites
  - localStorage sandbox positions
```

---

## Customizing Scoring Weights

Inside `index.html`, locate the `Config` object:

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

You can adjust the weights to match your preferred trading methodology.

For example, to prioritize relative strength and trend:

```javascript
weights: {
  trend: .25,
  momentum: .15,
  volume: .10,
  pattern: .15,
  smartMoney: .15,
  rs: .20
}
```

Make sure the total weight equals `1.00`.

---

## Customizing Sample Stocks

The sample India and USA stock lists are defined in JavaScript arrays:

```javascript
const India = [ ... ];
const USA = [ ... ];
```

Each stock object contains fields such as:

```javascript
{
  company: 'Reliance Industries',
  ticker: 'NSE:RELIANCE',
  exchange: 'NSE',
  sector: 'Energy',
  industry: 'Integrated Oil & Gas',
  price: 1509.6,
  marketCap: '₹20.4T',
  base: 47,
  pattern: 'VCP',
  trend: 94,
  momentum: 88,
  volume: 91,
  smartMoney: 93,
  rs: 87,
  rsi: 64,
  macd: 'Bullish',
  supertrend: 'Bullish',
  ema: '20>50>100>200',
  atr: 32.4,
  volRatio: 1.9,
  bench: 'Nifty 50'
}
```

---

## Production Development Notes

For a true institutional-grade implementation, the following should be added:

1. Candle-data ingestion
2. Technical indicator calculation from OHLCV data
3. Pattern-recognition algorithms
4. Relative strength ranking versus benchmark indices
5. Sector and industry leadership model
6. Earnings and sales growth filters
7. Market regime filter
8. Backtesting and walk-forward validation
9. Database-backed portfolio tracking
10. User authentication and cloud sync
11. Alerting via email, Telegram, WhatsApp, or push notifications
12. Risk-management module
13. Strategy-performance analytics

---

## Suggested Folder Structure for Future Expansion

If the project is later converted from a single HTML file into a full application, a scalable structure could look like this:

```text
ready-for-breakout-pro/
  public/
    index.html
  src/
    components/
      Dashboard.jsx
      ScreenerTable.jsx
      SandboxPortfolio.jsx
      MiniChartLink.jsx
    engines/
      MarketDataEngine.js
      TechnicalAnalysisEngine.js
      PatternRecognitionEngine.js
      BreakoutScoringEngine.js
      RelativeStrengthEngine.js
    services/
      ApiClient.js
      ExportService.js
      StorageService.js
    styles/
      theme.css
  README.md
```

---

## License

This prototype is provided for educational and internal research purposes. Add your preferred license before public distribution.

---

## Version Notes

### Current Version

- Single-file `index.html`
- Professional dark dashboard
- India and USA screeners
- Separate India and USA Sandbox workspaces
- TradingView mini chart redirects
- Scan and refresh controls
- Local watchlist and Sandbox persistence
- CSV/Excel export
- Responsive layout

