# Multi-Agent Project Scaffolding Workflow

## Overview

This workflow transforms the comprehensive implementation plan into a fully scaffolded, runnable project. It sets up the complete development environment, installs frameworks and libraries, creates the project structure, implements basic configurations, and verifies everything works using Playwright MCP.

**Target Audience**: This document guides Claude Code to execute multi-agent project scaffolding that creates a production-ready project foundation from technical specifications.

**Prerequisites**:
- Approved client proposal (`client-proposal.md`)
- Completed implementation plan (`comprehensive-implementation-plan.md`)
- API specifications (`api-specification.yaml`)
- Database schema (`database-schema.sql`)
- Frontend architecture plan
- Design system specifications

**Key Deliverable**: A fully scaffolded, locally runnable project with all configurations and basic structure in place.

**Workflow Position**: This is the final workflow in the sequence:
1. Research → 2. Proposal → 3. Implementation Plan → 4. **Scaffolding (THIS WORKFLOW)**

## Workflow Components

### 1. Environment Analyzer Agent
- **Role**: Analyze implementation plan and determine setup requirements
- **Model**: Sonnet
- **Deliverables**: 
  - `environment-requirements.md`
  - `dependency-list.md`
- **Chat Name**: EnvAnalyzer

### 2. Backend Scaffolder Agent
- **Role**: Set up backend API structure and configurations
- **Model**: Sonnet
- **Deliverables**: 
  - Backend project structure
  - API boilerplate
  - Database connections
- **Chat Name**: BackendScaffolder

### 3. Frontend Scaffolder Agent
- **Role**: Set up frontend framework and component structure
- **Model**: Sonnet
- **Deliverables**: 
  - Frontend project structure
  - Component hierarchy
  - Routing setup
- **Chat Name**: FrontendScaffolder

### 4. Database Setup Agent
- **Role**: Initialize database and run migrations
- **Model**: Haiku
- **Deliverables**: 
  - Database initialized
  - Initial migrations
  - Seed data (if applicable)
- **Chat Name**: DatabaseSetup

### 5. Configuration Agent
- **Role**: Set up environment configs, build tools, and CI/CD
- **Model**: Sonnet
- **Deliverables**: 
  - Environment files
  - Build configurations
  - CI/CD templates
- **Chat Name**: ConfigAgent

### 6. Integration Agent
- **Role**: Connect frontend to backend, verify communication
- **Model**: Sonnet
- **Deliverables**: 
  - API client setup
  - Authentication flow
  - Basic data flow
- **Chat Name**: IntegrationAgent

### 7. Testing Setup Agent
- **Role**: Configure testing frameworks and write initial tests
- **Model**: Sonnet
- **Deliverables**: 
  - Test configurations
  - Sample tests
  - Test commands
- **Chat Name**: TestingAgent

### 8. Verification Agent
- **Role**: Use Playwright to verify the application runs correctly
- **Model**: Opus
- **Deliverables**: 
  - `verification-report.md`
  - Screenshots of running app
- **Chat Name**: VerificationAgent

### 9. Documentation Agent
- **Role**: Create initial README and setup documentation
- **Model**: Haiku
- **Deliverables**: 
  - `README.md`
  - `SETUP.md`
  - `DEVELOPMENT.md`
- **Chat Name**: DocAgent

### 10. Coordinator (Main Agent)
- **Role**: Orchestrate scaffolding and handle issues
- **Chat Name**: ScaffoldCoordinator

## Project Structure Templates

### Full-Stack Web Application
```
project-root/
├── frontend/
│   ├── src/
│   │   ├── components/
│   │   │   ├── common/
│   │   │   ├── layout/
│   │   │   └── features/
│   │   ├── pages/
│   │   ├── services/
│   │   ├── hooks/
│   │   ├── utils/
│   │   ├── styles/
│   │   ├── App.tsx
│   │   └── index.tsx
│   ├── public/
│   ├── package.json
│   └── tsconfig.json
├── backend/
│   ├── src/
│   │   ├── controllers/
│   │   ├── models/
│   │   ├── routes/
│   │   ├── middleware/
│   │   ├── services/
│   │   ├── utils/
│   │   ├── config/
│   │   └── index.ts
│   ├── tests/
│   ├── package.json
│   └── tsconfig.json
├── database/
│   ├── migrations/
│   ├── seeds/
│   └── schema.sql
├── shared/
│   ├── types/
│   └── constants/
├── .github/
│   └── workflows/
├── docker/
├── scripts/
├── docs/
├── .env.example
├── docker-compose.yml
├── README.md
└── package.json
```

## Detailed Workflow

### Phase 0: Setup and Analysis

```bash
# Create project root
PROJECT_NAME=$(grep "project_name" comprehensive-implementation-plan.md | head -1 | cut -d: -f2 | tr -d ' ')
mkdir -p "./$PROJECT_NAME"
cd "./$PROJECT_NAME"

# Set up chat room
ROOM_NAME="scaffold-$PROJECT_NAME-$(date +%Y%m%d-%H%M%S)"

# Chat message standards
[ANALYZING] Reading implementation plan
[SCAFFOLDING] Creating [component]
[INSTALLING] Installing [package/dependency]
[CONFIGURING] Setting up [configuration]
[TESTING] Verifying [component]
[SUCCESS] [Component] ready
[ERROR] Issue with [component]: [description]
[COMPLETE] Scaffolding phase complete
```

### Phase 1: Environment Analysis

#### Step 1.1: Launch Environment Analyzer
```
mcp__ccm__claude_code:
  model: "sonnet"
  prompt: |
    You are the Environment Analyzer for project scaffolding.
    
    Join chat "$ROOM_NAME" as "EnvAnalyzer"
    
    1. Read comprehensive-implementation-plan.md
    2. Read frontend-architecture.md
    3. Read api-specification.yaml
    4. Extract technology choices:
       - Frontend framework and version
       - Backend framework and version
       - Database system
       - Additional libraries
    
    Create environment-requirements.md:
    ## Technology Stack
    - Frontend: [e.g., React 18.2 + TypeScript]
    - Backend: [e.g., Node.js + Express + TypeScript]
    - Database: [e.g., PostgreSQL 15]
    - State Management: [e.g., Redux Toolkit]
    - UI Library: [e.g., Material-UI]
    - Testing: [e.g., Jest, React Testing Library]
    
    Create dependency-list.md with exact versions
    
    Report: "[ANALYZING] Stack: [summary]"
    Report: "[COMPLETE] Requirements analyzed"
```

### Phase 2: Backend Scaffolding

#### Step 2.1: Launch Backend Scaffolder
```
mcp__ccm__claude_code:
  model: "sonnet"
  prompt: |
    You are the Backend Scaffolder.
    
    Join chat "$ROOM_NAME" as "BackendScaffolder"
    Monitor chat for EnvAnalyzer completion
    
    When ready:
    1. Create backend directory structure
    2. Initialize package.json with dependencies
    3. Set up TypeScript configuration
    4. Create Express server boilerplate
    5. Implement basic routes from API spec
    6. Set up middleware (cors, auth, error handling)
    7. Create database connection module
    8. Add environment variable handling
    
    Example structure:
    backend/
    ├── src/
    │   ├── index.ts (server entry point)
    │   ├── app.ts (Express app setup)
    │   ├── routes/
    │   │   ├── index.ts
    │   │   ├── auth.routes.ts
    │   │   └── [feature].routes.ts
    │   ├── controllers/
    │   ├── models/
    │   ├── middleware/
    │   │   ├── auth.middleware.ts
    │   │   └── error.middleware.ts
    │   ├── config/
    │   │   └── database.ts
    │   └── types/
    ├── package.json
    ├── tsconfig.json
    └── .env.example
    
    Commands to run:
    - npm init -y
    - npm install [dependencies]
    - npm install -D [dev dependencies]
    
    Report progress:
    "[SCAFFOLDING] Backend structure created"
    "[INSTALLING] Backend dependencies"
    "[SUCCESS] Backend ready at port 3001"
```

### Phase 3: Frontend Scaffolding

#### Step 3.1: Launch Frontend Scaffolder
```
mcp__ccm__claude_code:
  model: "sonnet"
  prompt: |
    You are the Frontend Scaffolder.
    
    Join chat "$ROOM_NAME" as "FrontendScaffolder"
    Monitor chat for EnvAnalyzer completion
    
    Based on frontend framework choice:
    
    For React:
    1. Create React app with TypeScript
       - npx create-react-app frontend --template typescript
       OR for Vite:
       - npm create vite@latest frontend -- --template react-ts
    2. Install additional dependencies
    3. Set up folder structure per architecture plan
    4. Create base components
    5. Set up routing
    6. Configure state management
    7. Set up API client
    8. Apply design system basics
    
    Structure to create:
    - Layout components (Header, Footer, Navigation)
    - Route definitions
    - API service layer
    - Authentication context/store
    - Theme configuration
    - Common components (Button, Input, Card)
    
    Report:
    "[SCAFFOLDING] Frontend created with [framework]"
    "[CONFIGURING] Setting up routing"
    "[SUCCESS] Frontend ready at port 3000"
```

### Phase 4: Database Setup

#### Step 4.1: Launch Database Setup Agent
```
mcp__ccm__claude_code:
  model: "haiku"
  prompt: |
    You are the Database Setup Agent.
    
    Join chat "$ROOM_NAME" as "DatabaseSetup"
    Wait for Backend scaffolding success
    
    1. Read database-schema.sql
    2. Create database setup script
    3. Create initial migration files
    4. Set up migration tool (e.g., Knex, Prisma)
    5. Create seed data for development
    
    Tasks:
    - Install database client library
    - Create migrations directory
    - Convert schema.sql to migrations
    - Create database connection test
    - Add database scripts to package.json:
      - "db:create": Create database
      - "db:migrate": Run migrations
      - "db:seed": Seed development data
      - "db:reset": Reset database
    
    For PostgreSQL:
    ```
    npm install pg knex
    npx knex init
    ```
    
    Report:
    "[CONFIGURING] Database setup initiated"
    "[SUCCESS] Database migrations ready"
```

### Phase 5: Configuration and Integration

#### Step 5.1: Launch Configuration Agent
```
mcp__ccm__claude_code:
  model: "sonnet"
  prompt: |
    You are the Configuration Agent.
    
    Join chat "$ROOM_NAME" as "ConfigAgent"
    Wait for frontend and backend scaffolding
    
    Create configurations:
    
    1. Environment files:
       - .env.example (template)
       - .env.development (local dev)
       
    2. Build configurations:
       - Webpack/Vite config updates
       - TypeScript configs
       - ESLint + Prettier
       
    3. Docker setup:
       - Dockerfile for frontend
       - Dockerfile for backend
       - docker-compose.yml
       
    4. CI/CD templates:
       - .github/workflows/ci.yml
       - Basic test and build pipeline
       
    5. Git configuration:
       - .gitignore
       - .gitattributes
    
    Report:
    "[CONFIGURING] Environment setup"
    "[SUCCESS] All configurations ready"
```

#### Step 5.2: Launch Integration Agent
```
mcp__ccm__claude_code:
  model: "sonnet"
  prompt: |
    You are the Integration Agent.
    
    Join chat "$ROOM_NAME" as "IntegrationAgent"
    Wait for ConfigAgent completion
    
    Connect frontend to backend:
    
    1. Set up API client in frontend
       - Axios or Fetch wrapper
       - Base URL configuration
       - Auth token handling
       
    2. Create sample integration:
       - Health check endpoint
       - Basic auth flow (login/register)
       - Sample data fetch
       
    3. Configure CORS properly
    
    4. Set up proxy for development:
       - Frontend proxy to backend
       - Hot reload preservation
    
    Example API client:
    ```typescript
    // frontend/src/services/api.ts
    const API_BASE_URL = process.env.REACT_APP_API_URL || 'http://localhost:3001/api';
    
    class ApiClient {
      // Implementation
    }
    ```
    
    Report:
    "[INTEGRATING] Connecting frontend to backend"
    "[SUCCESS] Full-stack integration complete"
```

### Phase 6: Testing Setup

#### Step 6.1: Launch Testing Setup Agent
```
mcp__ccm__claude_code:
  model: "sonnet"
  prompt: |
    You are the Testing Setup Agent.
    
    Join chat "$ROOM_NAME" as "TestingAgent"
    
    Set up testing frameworks:
    
    Frontend Testing:
    1. Jest + React Testing Library
    2. Sample component tests
    3. API mock setup
    4. Test scripts in package.json
    
    Backend Testing:
    1. Jest or Mocha
    2. Supertest for API testing
    3. Sample route tests
    4. Database test utilities
    
    E2E Testing:
    1. Playwright or Cypress setup
    2. Basic user flow test
    3. Test scripts
    
    Add to package.json:
    - "test": Run all tests
    - "test:frontend": Frontend only
    - "test:backend": Backend only
    - "test:e2e": E2E tests
    
    Report:
    "[CONFIGURING] Test frameworks"
    "[SUCCESS] Testing setup complete"
```

### Phase 7: Verification with Playwright

#### Step 7.1: Launch Verification Agent
```
mcp__ccm__claude_code:
  model: "opus"
  prompt: |
    You are the Verification Agent using Playwright MCP.
    
    Join chat "$ROOM_NAME" as "VerificationAgent"
    Wait for all scaffolding complete messages
    
    Verification steps:
    
    1. Start the application:
       - Run backend: npm run dev (port 3001)
       - Run frontend: npm run dev (port 3000)
       - Wait for both to be ready
    
    2. Use Playwright MCP tools:
       - mcp__playwright__browser_navigate to http://localhost:3000
       - mcp__playwright__browser_take_screenshot
       - Check for expected elements
       - Verify no console errors
       - Test basic navigation
    
    3. Verify API connection:
       - Check network requests
       - Verify API health endpoint
    
    4. Create verification-report.md:
       - Screenshot paths
       - Elements found
       - API connection status
       - Any errors encountered
    
    Expected results:
    - Homepage loads
    - No console errors
    - API connection successful
    - Basic styling applied
    
    Report:
    "[TESTING] Starting application verification"
    "[SUCCESS] Application running successfully!"
    OR
    "[ERROR] Verification failed: [details]"
```

### Phase 8: Documentation

#### Step 8.1: Launch Documentation Agent
```
mcp__ccm__claude_code:
  model: "haiku"
  prompt: |
    You are the Documentation Agent.
    
    Join chat "$ROOM_NAME" as "DocAgent"
    Wait for verification success
    
    Create initial documentation:
    
    1. README.md:
       # [Project Name]
       
       ## Overview
       [Brief description from requirements]
       
       ## Tech Stack
       - Frontend: [List]
       - Backend: [List]
       - Database: [List]
       
       ## Getting Started
       ### Prerequisites
       - Node.js >= 16
       - PostgreSQL >= 14
       - npm or yarn
       
       ### Installation
       \`\`\`bash
       # Clone repository
       git clone [repo-url]
       
       # Install dependencies
       npm run install:all
       
       # Set up environment
       cp .env.example .env
       # Edit .env with your values
       
       # Set up database
       npm run db:setup
       
       # Start development
       npm run dev
       \`\`\`
       
       ### Available Scripts
       [List all npm scripts]
       
    2. SETUP.md:
       - Detailed setup instructions
       - Troubleshooting guide
       - Environment variables explained
    
    3. DEVELOPMENT.md:
       - Code structure
       - Development workflow
       - Contribution guidelines
       - API documentation link
    
    Report:
    "[DOCUMENTING] Creating project documentation"
    "[COMPLETE] Documentation ready"
```

### Phase 9: Final Checkpoint

```
Project Scaffolding Complete! 🎉

Project: $PROJECT_NAME
Location: ./$PROJECT_NAME

✅ Backend API running on http://localhost:3001
✅ Frontend app running on http://localhost:3000
✅ Database configured and migrations ready
✅ Tests configured and passing
✅ Documentation created

Verification Results:
- Homepage loads successfully
- API health check passing
- No console errors
- Basic authentication ready

Next Steps:
1. Review the README.md
2. Check .env.example for required variables
3. Run tests: npm test
4. Start developing features!

Quick Commands:
- Start all: npm run dev
- Run tests: npm test
- Build prod: npm run build
- Database reset: npm run db:reset

Would you like to:
1. See the running application
2. Review project structure
3. Continue to development

Response (1/2/3):
```

## Framework-Specific Templates

### React + Node.js + PostgreSQL
```yaml
commands:
  frontend:
    - npm create vite@latest frontend -- --template react-ts
    - cd frontend && npm install axios react-router-dom @mui/material
  
  backend:
    - mkdir backend && cd backend
    - npm init -y
    - npm install express cors dotenv bcrypt jsonwebtoken
    - npm install -D typescript @types/node @types/express nodemon
  
  database:
    - npm install pg knex
    - npx knex init
    - npx knex migrate:make initial_schema
```

### Next.js + Prisma + PostgreSQL
```yaml
commands:
  setup:
    - npx create-next-app@latest . --typescript --tailwind --app
    - npm install @prisma/client prisma
    - npx prisma init
  
  configuration:
    - Update schema.prisma with models
    - Create API routes in app/api
    - Set up authentication
```

### Vue + FastAPI + MongoDB
```yaml
commands:
  frontend:
    - npm create vue@latest frontend
    - cd frontend && npm install pinia axios
  
  backend:
    - python -m venv venv
    - pip install fastapi uvicorn pymongo python-jose
    - Create main.py with FastAPI app
  
  database:
    - Set up MongoDB connection
    - Create Pydantic models
```

## Error Recovery Patterns

### Common Issues and Solutions

```yaml
port_conflicts:
  issue: "Port 3000 already in use"
  solution:
    - Change port in .env
    - Kill existing process
    - Use different port

dependency_errors:
  issue: "Module not found"
  solution:
    - Clear node_modules
    - Clear package-lock.json
    - Reinstall dependencies

database_connection:
  issue: "Cannot connect to database"
  solution:
    - Check database is running
    - Verify credentials in .env
    - Check firewall settings

build_failures:
  issue: "Build failed"
  solution:
    - Check Node version
    - Clear cache
    - Review error logs
```

## Scaffolding Optimizations

### Performance Considerations
```yaml
parallel_setup:
  - Frontend and backend setup can run in parallel
  - Database migrations after backend ready
  - Testing setup can start early

caching:
  - Cache npm dependencies
  - Use npm ci for faster installs
  - Docker layer caching

templates:
  - Pre-built component templates
  - Snippet libraries
  - Boilerplate reducers
```

### Security Defaults
```yaml
authentication:
  - JWT setup with refresh tokens
  - Bcrypt for password hashing
  - Session management

api_security:
  - Rate limiting configured
  - CORS properly set
  - Input validation
  - SQL injection prevention

environment:
  - Secrets in .env
  - .env never committed
  - Production configs separate
```

## Success Metrics

```yaml
scaffolding_success:
  setup_time: < 30 minutes
  components_created:
    - All directories present
    - Dependencies installed
    - Configs in place
    - Basic routes working
  
  verification:
    - App loads in browser
    - API responds
    - No console errors
    - Tests pass
  
  documentation:
    - Clear setup instructions
    - All scripts documented
    - Architecture explained
```

This workflow ensures that the implementation plan is transformed into a working project foundation that developers can immediately start building upon. The automated verification with Playwright ensures quality and saves significant setup time.