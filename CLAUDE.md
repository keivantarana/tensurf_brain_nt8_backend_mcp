# CLAUDE.md - TenSurf Brain NT8 Development Guide

## 🚨 MANDATORY WORKFLOW
Always read PLANNING.md at the start of every new conversation, check TASKS.md before starting your work, mark completed tasks to TASKS.md immediately, and add newly discovered tasks to TASKS.md when found.

## Project Overview

**Product Name:** TenSurf Brain NT8 MCP Server  
**Product Type:** AI-Powered Trading Assistant for NinjaTrader 8  
**Primary Goal:** Transform NinjaTrader 8 into an intelligent trading platform through conversational AI

### Core Value Proposition
Enable every NinjaTrader user to harness AI for smarter trading decisions through natural language interaction - converting trading ideas to executable NinjaScript strategies in minutes instead of weeks.

## Technical Architecture

### System Components
```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   AI Client     │    │   MCP Server    │    │  NT8 AddOn      │
│  (Claude/etc.)  │◄──►│  (External)     │◄──►│  (NinjaScript)  │
│                 │    │  - Strategy Gen │    │  - UI Integration│
│                 │    │  - AI Processing│    │  - Data Export  │
│                 │    │  - Risk Analysis│    │  - Order Mgmt   │
└─────────────────┘    └─────────────────┘    └─────────────────┘
                                │                       │
                                ▼                       ▼
                       ┌─────────────────┐    ┌─────────────────┐
                       │   ML.NET        │    │  NinjaTrader 8  │
                       │   Models        │    │  Platform       │
                       └─────────────────┘    └─────────────────┘
```

### Technology Stack
- **NT8 Integration:** .NET Framework 4.8, C#, NinjaScript
- **MCP Server:** Node.js, TypeScript, FastMCP framework
- **AI Integration:** OpenAI GPT-4, Claude API
- **ML Processing:** ML.NET for local AI operations
- **Communication:** TCP sockets, HTTP REST APIs
- **Database:** PostgreSQL for user data and strategy storage

## Core Features & Implementation

### 1. Natural Language Strategy Generation
**Goal:** Convert plain English trading ideas into working NinjaScript code

**Key Components:**
- Natural language processing for strategy description parsing
- NinjaScript code generation with proper syntax and structure
- Strategy validation and compilation checking
- Template library for common trading patterns

**Implementation Pattern:**
```csharp
// NT8 AddOn
public async Task<string> GenerateStrategy(string description)
{
    var request = new StrategyRequest
    {
        Description = description,
        UserPreferences = GetUserPreferences(),
        RiskParameters = GetDefaultRiskSettings()
    };
    
    var response = await mcpClient.SendAsync("generate_strategy", request);
    return response.NinjaScriptCode;
}
```

### 2. Market Data Access & Analysis
**Goal:** Provide comprehensive market data access with AI-powered insights

**Core Capabilities:**
- **Historical Data Export:** OHLCV data for all NT8 instruments and timeframes
- **Trade History Analysis:** Complete execution records with performance metrics
- **Live Price Monitoring:** Real-time feeds with AI market condition analysis
- **Market Scanning:** Custom criteria with intelligent alerts

**Implementation Pattern:**
```csharp
// Historical Data Export
public async Task<List<BarData>> GetHistoricalData(
    string instrument, 
    int barsPeriod, 
    BarsPeriodType periodType,
    DateTime fromDate, 
    DateTime toDate)
{
    // Access NT8 historical data and export
}

// Live Price Monitoring
public async Task<LivePriceData> GetLivePrice(string instrument)
{
    // Access real-time market data from NT8
}
```

### 3. Intelligent Risk Management
**Goal:** ML-powered position sizing and portfolio risk monitoring

**Features:**
- Dynamic position sizing based on account equity and volatility
- Real-time portfolio risk assessment across multiple strategies
- Correlation analysis to prevent overexposure
- Automated stop-loss and take-profit suggestions

### 4. Strategy Deployment & Management
**Goal:** Seamless strategy deployment with performance tracking

**Features:**
- One-click strategy deployment to NT8
- Real-time performance monitoring and analytics
- Automated parameter optimization suggestions
- Version control for strategy iterations

## Development Guidelines

### File Structure & Organization
```
TenSurfBrainNT8/
├── NT8AddOn/                 # NinjaScript AddOn
│   ├── Core/                 # Core functionality
│   ├── DataExport/           # Market data export
│   ├── Communication/        # External API communication
│   ├── UI/                   # User interface components
│   └── Utils/                # Utility functions
├── MCPServer/                # External MCP server
│   ├── src/
│   │   ├── tools/            # MCP tool implementations
│   │   ├── ai/               # AI model integrations
│   │   ├── models/           # Data models and types
│   │   └── utils/            # Utility functions
│   ├── tests/                # Test suite
│   └── docs/                 # Documentation
├── MLModels/                 # ML.NET models and training
├── docs/                     # Project documentation
├── tests/                    # Integration tests
└── deployment/               # Deployment scripts and configs
```

### Coding Standards

#### C# / NinjaScript Standards
- Follow Microsoft C# coding conventions
- Use async/await for all external communications
- Implement comprehensive error handling and logging
- Document all public methods with XML comments
- Use dependency injection where appropriate

#### TypeScript / MCP Server Standards
- Follow TypeScript strict mode guidelines
- Use ESLint and Prettier for code formatting
- Implement proper error handling with typed exceptions
- Use JSDoc comments for all public functions
- Follow RESTful API design principles

### Testing Requirements
- **Unit Tests:** >90% code coverage for all business logic
- **Integration Tests:** Full end-to-end testing of NT8 ↔ MCP communication
- **Performance Tests:** Validate response times meet specifications
- **User Acceptance Tests:** Validate with real NT8 environment

## Implementation Phases

### Phase 1: Foundation (Months 1-2)
**Milestone:** Working MVP with core functionality

**Key Deliverables:**
- NT8 AddOn with external communication
- Basic strategy generation from natural language
- Market data export functionality
- Strategy deployment pipeline

**Critical Success Factors:**
- Establish reliable NT8 ↔ MCP communication
- Validate strategy generation accuracy (>90% compilable code)
- Ensure data export performance meets requirements

### Phase 2: Core Features (Months 3-4)
**Milestone:** Production-ready core features

**Key Deliverables:**
- Advanced strategy generation with optimization
- Comprehensive market analysis tools
- Professional risk management system
- Performance tracking and analytics

### Phase 3: Advanced Features (Months 5-6)
**Milestone:** Feature-complete product ready for scale

**Key Deliverables:**
- Enterprise-grade security and compliance
- Advanced UI/UX with NT8 integration
- Multi-user collaboration features
- Production deployment with monitoring

## Technical Constraints & Considerations

### NT8 Platform Limitations
- **Strategy Analyzer:** No API access - build custom backtesting engine
- **Data Streaming:** Broker restrictions (typically 100-500 instruments)
- **Platform Threading:** Must handle NT8's event-driven architecture
- **File System Access:** Use appropriate NT8 directories for strategy files

### Performance Requirements
- **Strategy Generation:** <30 seconds for complex strategies
- **Market Analysis:** <60 seconds for 100+ instrument scan
- **Risk Calculations:** <5 seconds for portfolio-wide analysis
- **UI Responsiveness:** <2 seconds for all user interactions

### Security Requirements
- End-to-end encryption for all communications
- OAuth 2.1 with multi-factor authentication
- Rate limiting and DDoS protection
- SOC 2 Type II compliance for enterprise clients

## Development Environment Setup

### Prerequisites
- **Windows 10/11** (NT8 requirement)
- **Visual Studio 2022** with NinjaScript support
- **NinjaTrader 8** installed with developer license
- **Node.js 18+** for MCP server development
- **PostgreSQL 14+** for data storage

### Initial Setup Steps
1. Clone repository and set up development branches
2. Install NT8 and configure development environment
3. Set up MCP server with required dependencies
4. Configure database and initialize schema
5. Establish secure communication between components
6. Run initial integration tests to validate setup

## Testing Strategy

### NT8 Integration Testing
- Use NT8 Simulation environment for strategy testing
- Validate all market data export functions
- Test UI integration with different NT8 configurations
- Performance testing under various market conditions

### MCP Server Testing
- Unit tests for all MCP tools and AI integrations
- Load testing for concurrent user scenarios
- API integration tests with external services
- Security penetration testing

### End-to-End Testing
- Complete workflow testing from natural language to strategy deployment
- Multi-user collaboration testing
- Cross-platform compatibility testing
- Disaster recovery and failover testing

## Deployment Guidelines

### Development Environment
- Local development with NT8 simulation
- Dedicated test database and mock external services
- Comprehensive logging and debugging tools

### Staging Environment
- Mirror production configuration
- Real market data integration testing
- User acceptance testing with beta users
- Performance monitoring and optimization

### Production Environment
- AWS/Azure cloud infrastructure
- Auto-scaling containers for MCP server
- Managed database with backup and recovery
- Comprehensive monitoring and alerting

## Documentation Requirements

### User Documentation
- Installation and setup guide for NT8 AddOn
- User manual with feature explanations and examples
- Video tutorials for common workflows
- Troubleshooting guide and FAQ

### Developer Documentation
- API documentation for all MCP tools
- NT8 integration guide for developers
- Architecture overview and design decisions
- Contributing guidelines for open-source components

### Operational Documentation
- Deployment and configuration guides
- Monitoring and alerting procedures
- Backup and recovery procedures
- Security incident response plan

## Quality Assurance

### Code Quality
- Automated code review and static analysis
- Peer review for all critical components
- Regular architecture reviews and refactoring
- Performance profiling and optimization

### User Experience
- Regular user feedback collection and analysis
- A/B testing for key user flows
- Accessibility compliance (WCAG 2.1)
- Cross-browser and cross-platform testing

## Success Metrics & KPIs

### Technical Metrics
- System uptime >99.5%
- Strategy generation accuracy >90%
- Response time compliance >95%
- Error rate <1% for user operations

### User Engagement
- Monthly active users (target 80% of subscribers)
- Feature adoption rate >60% within 30 days
- User session duration >45 minutes average
- Customer satisfaction score >4.5/5

### Business Metrics
- Monthly recurring revenue growth >20%
- Customer acquisition cost <$200 (individual)
- Customer lifetime value >$2,000
- Net promoter score >50

## Risk Management

### Technical Risks
- **NT8 API Changes:** Maintain NT relationship, implement abstraction layers
- **AI Model Performance:** Multiple providers, human review process
- **Scalability Issues:** Horizontal architecture, performance monitoring

### Business Risks
- **Slow Adoption:** Freemium model, strong ROI demonstration
- **Competition:** IP protection, superior UX, rapid development
- **Regulatory Changes:** Legal counsel, proactive compliance

### Operational Risks
- **Security Breaches:** Comprehensive security measures, regular audits
- **Data Loss:** Multiple backups, disaster recovery procedures
- **Team Scaling:** Clear processes, comprehensive documentation

## Communication & Collaboration

### Development Team Communication
- Daily standups for coordination and blockers
- Weekly sprint planning and retrospectives
- Monthly architecture reviews and technical discussions
- Quarterly goal setting and performance reviews

### Stakeholder Communication
- Bi-weekly progress reports to leadership
- Monthly customer feedback reviews
- Quarterly business reviews and strategy sessions
- Annual planning and budget reviews

## Continuous Improvement

### Feedback Loops
- User feedback integration into development cycle
- Performance monitoring and optimization
- Regular architecture reviews and updates
- Market research and competitive analysis

### Innovation Pipeline
- Regular AI model updates and improvements
- New feature development based on user needs
- Integration with emerging trading technologies
- Research into advanced AI and ML techniques

---

## Quick Reference Commands

### Development Workflow
```bash
# Start NT8 development environment
./scripts/start-nt8-dev.sh

# Run MCP server in development mode
npm run dev

# Run full test suite
npm run test:all

# Deploy to staging
./scripts/deploy-staging.sh
```

### Common Tasks
- **Generate Strategy:** Use `generate_strategy` MCP tool
- **Export Data:** Use `getHistoricalData` or `getLivePrice` tools
- **Deploy Strategy:** Use NT8 AddOn deployment pipeline
- **Monitor Performance:** Check dashboard at `/monitoring`

### Emergency Contacts
- **Technical Lead:** [Contact Info]
- **DevOps Engineer:** [Contact Info]
- **Security Team:** [Contact Info]
- **Customer Support:** [Contact Info]

Remember: This is a high-stakes financial technology project. Always prioritize security, reliability, and user safety in all development decisions.