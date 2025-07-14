# RoyaltyAI Development Agreement \- Revised Milestone Structure

**Client:** MariNation Music, LLC (Justin Longo)  
**Developer:** NexAIAdvisors LLC (David He)  
**Project:** RoyaltyAI \- Royalty Statement Analyzer & Portfolio Management Platform  
**Effective Date:** 7/1/25

## 1\. Project Scope

NexAIAdvisors LLC will design, build, and deliver a production-ready platform for royalty statement analysis and portfolio management. The platform will include:

### Core Features

- **Authentication & Security:** Secure sign-in using OAuth (Google, Apple, Amazon, and email), managed by Supabase with user profile management and password reset functionality  
- **Database Architecture:** Comprehensive PostgreSQL schema for normalized royalty data storage with proper indexing and relationships  
- **Statement Parsing & Normalization:** Support for core formats: WMG (TXT), UMG (CSV/XLS), Warner Chapel (CSV), and STIM (CSV). OCR via Amazon Textract for scanned/handwritten statements when required  
- **Secure Upload Interface:** File upload system (PDF, Excel, CSV) with real-time status feedback, metadata tracking, and secure S3 storage  
- **Analytics Dashboard:** Revenue visualizations including total income trends, DSP breakdowns, geographic heatmaps, top track performance, and growth analytics with mobile-responsive design  
- **AI Intelligence:** Natural language query system using GPT-4/LangChain, basic forecasting capabilities, and user query history  
- **Export Functionality:** Support for CSV and PDF export formats  
- **Portfolio Management:** Multi-catalog support with consolidated views across different distributors

### Stretch Goals (Optional Deliverables)

- **Extended Format Support:** Early support for BMG, Kobalt, and ASCAP statements (targeting 60-70% parsing accuracy)  
- **Advanced Forecasting:** Seasonal and trend-based predictive models  
- **Vector Search:** Pinecone integration for track discovery and pattern matching

All work will align with the technical specifications outlined in the Detailed Implementation Plan and any additional requirements agreed upon by both parties.

## 2\. Client Responsibilities

Justin Longo (Client) agrees to:

- **Timely Response:** Provide responses to Developer requests within 48 hours to maintain project momentum  
- **Weekly Sync Meetings:** Participate in scheduled progress reviews, feedback sessions, and technical discussions  
- **Sample Data:** Provide representative royalty statements for testing and validation during development  
- **Infrastructure Costs:** Cover all additional costs including server hosting, cloud resources, and third-party tools as detailed in separate documentation  
- **Feedback & Testing:** Conduct acceptance testing within 3 business days of milestone delivery

## 3\. Revised Milestone Structure & Timeline

**Estimated Duration:** 8–10 weeks  
**Total Compensation:** $8,000 USD  
**Payment Structure:** Four milestone payments of $2,000 each

### Milestone 1 – Authentication & Infrastructure Foundation ($2,000)

**Due:** Week 2–3  
**Technical Focus:** Core platform infrastructure and user management

**Deliverables:**

- **Authentication System:** Complete OAuth implementation with Supabase (Google, Apple, Amazon, email)  
- **User Management:** Profile management, password reset, account settings  
- **Database Schema:** Full PostgreSQL implementation of normalized royalty data structure   
- **Basic Dashboard Framework:** Next.js \+ Tailwind CSS foundation with navigation structure  
- **File Upload Infrastructure:** AWS S3 integration with KMS encryption and metadata tracking  
- **Admin Interface:** Basic validation and monitoring tools for system health

**Acceptance Criteria:**

- Users can register, login, and manage profiles securely  
- Database schema handles sample data without errors  
- File upload accepts PDF, CSV, XLS formats and stores securely  
- Admin can view system status and uploaded files

### Milestone 2 – Parsing Engine & Analytics Dashboard ($2,000)

**Due:** Week 4–6  
**Technical Focus:** Statement processing and data visualization

**Deliverables:**

- **Core Statement Parsers:**  
  - WMG (TXT format) with 80%+ accuracy  
  - UMG (CSV/XLS format) with 80%+ accuracy  
  - Warner Chapel (CSV format) with 80%+ accuracy  
  - STIM (CSV format) with 80%+ accuracy  
- **Data Normalization Engine:** Automatic mapping to standardized schema  
- **OCR Integration:** Amazon Textract for scanned statements when needed  
- **Analytics Dashboard:** Interactive visualizations using Recharts/Chart.js  
  - Revenue over time (monthly, quarterly, yearly views)  
  - Platform breakdown (Spotify, Apple Music, YouTube, etc.)  
  - Geographic revenue distribution with heatmap  
  - Top performing tracks analysis  
- **Error Handling:** Comprehensive logging and user feedback for parsing issues  
- **Export Functionality:** CSV export of filtered and processed data  
- **Mobile Responsiveness:** Full mobile/tablet compatibility

**Acceptance Criteria:**

- Core parsers achieve 80%+ accuracy on sample statements  
- Dashboard displays accurate data from parsed statements  
- Users can export data in CSV format  
- Interface works seamlessly on mobile devices

### Milestone 3 – AI Intelligence & Advanced Features ($2,000)

**Due:** Week 7–8  
**Technical Focus:** Natural language processing and intelligent analysis

**Deliverables:**

- **AI Query System:** GPT-4 \+ LangChain integration for natural language queries  
- **Supported Query Types:**  
  - Revenue analysis ("What was my Spotify income in Q2 2023?")  
  - Trend analysis ("Show revenue growth for hip-hop tracks")  
  - Comparative analysis ("Compare streaming vs download trends")  
  - Territory performance ("Which regions are growing fastest?")  
- **Basic Forecasting:** Linear trend projection for revenue prediction  
- **Query History:** User dashboard showing previous queries and results  
- **Data Security:** Ensure no sensitive data exposure to external AI services  
- **Performance Optimization:** Query response times under 10 seconds

**Acceptance Criteria:**

- AI successfully answers 90%+ of test queries accurately  
- Query history properly tracks and displays user interactions  
- Forecasting provides reasonable projections based on historical data  
- No sensitive customer data exposed to external services

### Milestone 4 – Portfolio Management & Production Deployment ($2,000)

**Due:** Week 9–10  
**Technical Focus:** Multi-catalog support and production readiness

**Deliverables:**

- **Portfolio Analytics:** Consolidated views across multiple statement sources  
- **Cross-Catalog Analysis:** Performance comparison between different distributors  
- **Advanced Dashboard Features:**  
  - Customizable date ranges and filters  
  - Track user actions for analysis  
- **Production Deployment:**  
  - Secure hosting environment setup  
  - SSL certificates and domain configuration  
- **Documentation Package:**  
  - User guide for platform features  
  - API documentation for future integrations  
  - System administration guide  
- **Final Testing:** End-to-end testing, performance optimization**Source Code Handoff:** Complete codebase with deployment instructions

**Stretch Goal Implementation (if time permits):**

- Basic BMG, Kobalt, or ASCAP statement parsing (60-70% target accuracy)  
- Enhanced forecasting with seasonal adjustments  
- Pinecone vector search for track pattern analysis

**Acceptance Criteria:**

- Portfolio view accurately aggregates data from multiple sources  
- Production environment handles expected load without performance issues  
- Complete documentation enables client self-management  
- All security requirements met for production deployment

## 4\. Technical Specifications

### Frontend Architecture

- **Framework:** Next.js 14+ with TypeScript  
- **Styling:** Tailwind CSS with responsive design principles  
- **State Management:** React Context or Zustand for complex state  
- **Charts:** Recharts for data visualization  
- **Authentication:** Supabase Auth integration

### Backend Architecture

- **API Framework:** FastAPI with Python 3.11+  
- **Database:** PostgreSQL 15+ with proper indexing  
- **File Processing:** Pandas, PyMuPDF, openpyxl for statement parsing  
- **Task Queue:** Background processing for large files  
- **Storage:** AWS S3 with KMS encryption

### AI/ML Integration

- **Language Model:** OpenAI GPT-4 API with no-training configuration  
- **Embeddings:** OpenAI text-embedding-ada-002 for semantic search  
- **Vector Database:** Pinecone (if stretch goals achieved)  
- **OCR Service:** Amazon Textract for document processing

### Security & Compliance

- **Encryption:** All data encrypted at rest and in transit  
- **Authentication:** OAuth 2.0 with multi-factor authentication support  
- **Data Privacy:** No user data sent to training models  
- **Audit Logging:** Complete audit trail for all data access  
- **Geographic Compliance:** US/EU data residency options

## 5\. Acceptance & Testing Criteria

Each milestone will be considered complete when:

- All specified deliverables are functional and tested  
- Acceptance criteria are met with documented validation  
- Client approval received within 3 business days of delivery  
- Any critical bugs identified during testing are resolved

**Testing Standards:**

- Parsing accuracy validated against sample statements  
- Dashboard functionality tested across major browsers  
- AI query accuracy validated against predefined test cases  
- Security testing for authentication and data protection  
- Performance testing for acceptable response times

## 6\. Ownership & Intellectual Property

All code, models, visual designs, and outputs created under this contract are the sole property of MariNation Music, LLC. NexAIAdvisors LLC will not reuse, distribute, or create derivative works from any part of this system.

**Open Source Components:** Any open-source or third-party components used remain subject to their original licenses. Developer will document all dependencies and ensure license compliance.

## 7\. Data Security & Confidentiality

NexAIAdvisors LLC agrees to maintain strict confidentiality of all proprietary and user data. Specific protections include:

- **No Data Reuse:** User data will never be reused, shared, or accessed beyond project scope  
- **Secure Processing:** All sensitive data processed in secure, encrypted environments  
- **Privacy by Design:** System architecture prevents accidental data exposure  
- **Compliance:** Implementation follows GDPR and US privacy regulation requirements

## 8\. Change Management & Scope Control

**Scope Changes:** Any modifications to project scope require written agreement from both parties. Minor clarifications within existing scope will be implemented at no additional cost.

**Stretch Goal Evaluation:** BMG, Kobalt, and ASCAP support will be evaluated during Milestone 4 based on available time and complexity assessment.

**Timeline Adjustments:** If unforeseen technical complexities arise, timeline adjustments will be discussed and agreed upon in writing.

## 9\. Payment Terms & Infrastructure Costs

**Milestone Payments:** Each $2,000 payment due upon successful completion and acceptance of corresponding deliverables.

**Infrastructure Costs:** Client responsible for all third-party service costs including:

- AWS hosting and storage fees  
- OpenAI API usage charges  
- Supabase subscription costs  
- Third-party service subscriptions

Client may pay these costs directly or reimburse Developer upon invoice submission.

## 10\. Post-Launch Support & Maintenance

**Included Support:** 2 weeks of post-launch support for critical bugs and deployment issues.

**Response Times:**

- Critical issues (system down): 4 hours  
- High priority (major functionality broken): 24 hours  
- Medium priority (minor bugs): 72 hours

**Ongoing Maintenance:** Post-support maintenance and feature additions subject to separate agreement.

## 11\. Risk Management & Contingencies

**Technical Risks:**

- Complex statement formats may require additional parsing time  
- AI integration complexity may affect Milestone 3 timeline  
- Third-party service availability dependencies

**Mitigation Strategies:**

- Regular milestone check-ins to identify issues early  
- Flexible stretch goal implementation based on progress  
- Backup service providers identified for critical dependencies

## 12\. Success Metrics & Quality Standards

**Performance Targets:**

- Statement parsing: 80%+ accuracy for core formats  
- Query response time: \<10 seconds for AI requests  
- Dashboard load time: \<3 seconds for standard views  
- System uptime: 99.5% availability during business hours

**Quality Assurance:**

- Code review and testing for all deliverables  
- Security audit before production deployment  
- Performance optimization throughout development  
- Comprehensive documentation and handoff materials

## 13\. Legal & General Provisions

**Independent Contractor:** Developer operates as independent contractor, not employee of MariNation Music, LLC.

**Governing Law:** Agreement governed by laws of \[State\], without regard to conflict of law principles.

**Dispute Resolution:** Good faith negotiation followed by mediation if necessary.

**Assignment:** Neither party may assign agreement without prior written consent.

**Termination:**

- Developer termination without cause: Forfeit current milestone payment, receive payment for completed milestones  
- Client termination: Pay for completed work based on deliverable value assessment

**Entire Agreement:** This document constitutes complete understanding between parties and supersedes all prior agreements.

## 14\. Signatures

**MariNation Music, LLC**

Signature: \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_ Date: \_\_\_\_\_\_\_\_\_\_\_\_\_  
Justin Longo

**NexAiAdvisors LLC**

Signature: \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_ Date: \_\_\_\_\_\_\_\_\_\_\_\_\_  
David He

---

