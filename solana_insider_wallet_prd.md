# Solana Insider Wallet Detection App - Product Requirements Document

## 1. Executive Summary

**Product Name:** SolanaScope - Whale Hunter  
**Version:** 1.0  
**Platform:** Web Application (Next.js)  
**Purpose:** Identify profitable wallet addresses that consistently achieve 2x+ returns across multiple meme coin investments on Solana blockchain.

## 2. Product Overview

### 2.1 Core Functionality
The application analyzes multiple Solana meme coin token addresses to identify "insider wallets" - addresses that have consistently bought multiple tokens early and achieved minimum 2x profits.

### 2.2 Key Value Proposition
- Discover profitable wallet patterns across multiple meme coins
- Identify potential insider trading activity
- Provide actionable insights for crypto investment strategies

## 3. User Requirements

### 3.1 Primary User Flow
1. User inputs multiple meme coin contract addresses (CA)
2. System analyzes each token's trading history via Solscan.io
3. System identifies top traders for each token
4. System calculates profit margins (minimum 2x filter)
5. System finds common wallet addresses across all tokens
6. System outputs top 5 insider wallet addresses

### 3.2 Input Requirements
- **Token Addresses:** Accept multiple Solana token contract addresses
- **Input Format:** Text area or individual input fields
- **Validation:** Verify valid Solana address format (Base58, 32-44 characters)
- **Minimum Tokens:** At least 2 token addresses required

### 3.3 Output Requirements
- **Top 5 Insider Wallets:** Ranked by consistency and profit margins
- **Wallet Details:** Address, total tokens traded, average profit multiplier
- **Performance Metrics:** Win rate, total profit, number of successful trades

## 4. Technical Specifications

### 4.1 Technology Stack
- **Frontend:** Next.js 14+ with TypeScript
- **Styling:** Tailwind CSS
- **State Management:** React hooks (useState, useEffect)
- **API Integration:** Solscan.io API / Web scraping
- **Data Processing:** Client-side analysis algorithms

### 4.2 Core Algorithms

#### 4.2.1 Trader Discovery Algorithm
```
FOR each token_address:
    1. Fetch transaction history from Solscan.io
    2. Identify buy transactions (first 100-500 transactions)
    3. Track wallet addresses and purchase amounts
    4. Calculate current/exit profits for each wallet
    5. Filter wallets with 2x+ profit
    6. Store profitable wallets with metadata
```

#### 4.2.2 Common Wallet Detection Algorithm
```
1. Create intersection of profitable wallets across all tokens
2. Calculate frequency score (how many tokens each wallet traded)
3. Calculate average profit multiplier per wallet
4. Rank by: (frequency_score * average_profit) / total_tokens
5. Return top 5 wallets
```

### 4.3 Data Sources
- **Primary:** Solscan.io API/scraping
- **Backup:** Solana RPC endpoints
- **Token Prices:** CoinGecko API or similar

## 5. Functional Requirements

### 5.1 Core Features

#### 5.1.1 Token Input System
- Multiple token address input (minimum 2, maximum 10)
- Real-time address validation
- Duplicate address detection
- Clear/reset functionality

#### 5.1.2 Analysis Engine
- Parallel processing for multiple tokens
- Progress indicator for analysis
- Error handling for invalid/delisted tokens
- Configurable profit threshold (default: 2x)

#### 5.1.3 Results Display
- Top 5 insider wallet addresses
- Wallet performance metrics:
  - Total tokens matched
  - Average profit multiplier
  - Win rate percentage
  - Total estimated profit (USD)
- Clickable wallet addresses (link to Solscan)

#### 5.1.4 Additional Features
- Export results to CSV/JSON
- Share results via URL
- Historical analysis (last 30/60/90 days)
- Minimum investment threshold filter

### 5.2 Performance Requirements
- Analysis completion within 30-60 seconds
- Support for concurrent user requests
- Responsive design for mobile/desktop
- Loading states and progress indicators

## 6. User Interface Requirements

### 6.1 Layout Structure
```
Header: App Title + Navigation
Main Section:
  - Input Form (Token Addresses)
  - Configuration Panel (Profit threshold, timeframe)
  - Action Button (Analyze)
  - Results Section (Top 5 wallets)
Footer: Credits/Links
```

### 6.2 UI Components
- **Token Input:** Multi-line textarea or dynamic input fields
- **Configuration Panel:** Sliders/dropdowns for settings
- **Progress Bar:** Real-time analysis progress
- **Results Cards:** Wallet information in card format
- **Export Buttons:** Download/share functionality

### 6.3 Responsive Design
- Mobile-first approach
- Breakpoints: 320px, 768px, 1024px, 1440px
- Touch-friendly buttons and inputs
- Collapsible sections for mobile

## 7. API Requirements

### 7.1 Solscan.io Integration
- **Endpoint:** Transaction history for token addresses
- **Rate Limiting:** Respect API limits (implement delays)
- **Error Handling:** Network failures, API errors
- **Data Parsing:** Extract wallet addresses, amounts, timestamps

### 7.2 Price Data Integration
- **Real-time Prices:** Current token prices
- **Historical Prices:** Price at purchase time
- **Profit Calculation:** (current_price - buy_price) / buy_price

## 8. Data Processing Specifications

### 8.1 Transaction Analysis
- **Buy Detection:** Identify initial purchase transactions
- **Sell Detection:** Track exit transactions (if available)
- **Profit Calculation:** 
  - Unrealized: (current_price - buy_price) / buy_price
  - Realized: (sell_price - buy_price) / buy_price

### 8.2 Wallet Scoring Algorithm
```javascript
function calculateWalletScore(wallet, tokens) {
  const matchedTokens = wallet.tokens.length;
  const totalTokens = tokens.length;
  const averageProfit = wallet.totalProfit / wallet.trades;
  const frequencyScore = matchedTokens / totalTokens;
  
  return frequencyScore * averageProfit * 100;
}
```

## 9. Security & Privacy Requirements

### 9.1 Data Handling
- No storage of sensitive user data
- Client-side processing when possible
- Rate limiting to prevent abuse
- CORS handling for API requests

### 9.2 User Privacy
- No user authentication required
- No tracking of user queries
- Optional analytics (anonymized)

## 10. Error Handling

### 10.1 Input Validation Errors
- Invalid token addresses
- Insufficient token count
- Duplicate addresses

### 10.2 API/Network Errors
- Solscan.io API failures
- Network timeouts
- Rate limiting exceeded

### 10.3 Data Processing Errors
- Insufficient transaction data
- No profitable wallets found
- Analysis timeout

## 11. Development Phases

### Phase 1: Core Functionality (Week 1-2)
- Basic UI with token input
- Solscan.io integration
- Simple wallet detection algorithm

### Phase 2: Advanced Features (Week 3-4)
- Profit calculation improvements
- Enhanced UI/UX
- Export functionality

### Phase 3: Optimization (Week 5-6)
- Performance improvements
- Advanced filtering options
- Mobile optimization

## 12. Success Metrics

### 12.1 Technical Metrics
- Analysis completion time < 60 seconds
- 95% uptime
- Error rate < 5%

### 12.2 User Experience Metrics
- User completion rate > 80%
- Average session duration
- Feature usage statistics

## 13. Future Enhancements

### 13.1 Version 2.0 Features
- Real-time wallet monitoring
- Alert system for new trades
- Portfolio tracking
- Advanced analytics dashboard

### 13.2 Integration Possibilities
- Telegram bot integration
- Discord notifications
- Mobile app version
- API for third-party access

## 14. Implementation Notes

### 14.1 File Structure
```
src/
├── components/
│   ├── TokenInput.tsx
│   ├── AnalysisEngine.tsx
│   ├── ResultsDisplay.tsx
│   └── WalletCard.tsx
├── lib/
│   ├── solscan.ts
│   ├── analysis.ts
│   └── utils.ts
├── pages/
│   ├── index.tsx
│   └── api/
└── styles/
```

### 14.2 Key Dependencies
```json
{
  "next": "^14.0.0",
  "react": "^18.0.0",
  "typescript": "^5.0.0",
  "tailwindcss": "^3.0.0",
  "axios": "^1.0.0",
  "bs58": "^5.0.0"
}
```

This PRD provides a comprehensive blueprint for building your Solana insider wallet detection app. The AI can use this to implement each component systematically.