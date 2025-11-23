# IMIRS AI Reporting System

> Transform natural language questions into SQL queries against the Illinois Tollway's materials testing database using AI-powered intelligence.

[![Next.js](https://img.shields.io/badge/Next.js-16.0-black)](https://nextjs.org/)
[![React](https://img.shields.io/badge/React-19.2-blue)](https://react.dev/)
[![AWS Amplify](https://img.shields.io/badge/AWS%20Amplify-Gen%202-orange)](https://docs.amplify.aws/)
[![TypeScript](https://img.shields.io/badge/TypeScript-5.7-blue)](https://www.typescriptlang.org/)

## 🎯 Purpose

The IMIRS AI Reporting System is a production-ready generative AI web application that enables Illinois Tollway staff and auditors to query construction materials testing data using natural language. The system transforms conversational questions into SQL queries, executes them against the existing IMIRS SQL Server database, and presents results through intelligent visualizations with export capabilities.

**Key Benefits:**
- 🚀 **Sub-second query latency** with intelligent caching
- 💰 **Cost-efficient** - Target < $150/month for 10 concurrent users
- 🔒 **Enterprise security** with row-level access controls
- 🎨 **Executive-grade UI** comparable to ChatGPT
- 📊 **Smart visualizations** with automatic chart type detection
- 📱 **Mobile-first** responsive design

## 📊 Project Stats

**Milestone Progress:** 3 of 8 milestones complete (37.5%)

### Completed Features
- ✅ **Foundation & Authentication** (Milestone 1)
  - AWS Amplify Gen 2 backend with Cognito authentication
  - User registration with admin approval workflow
  - Landing page and login interface
  - Protected routes with role-based access
  
- ✅ **Chat Interface** (Milestone 2)
  - Interactive chat UI with streaming responses
  - Message history and conversation flow
  - Query input with voice support (optional)

- ✅ **Backend Connection** (Milestone 3)
  - AI-powered SQL generation using AWS Bedrock (Claude 3 Haiku/Sonnet)
  - Row-level security with automatic SQL filter injection
  - Query execution via Lambda function
  - Comprehensive audit logging to CloudWatch
  - User management API endpoints

### Code Statistics
- **113 files** committed
- **63,859 insertions** in initial commit
- **TypeScript strict mode** enabled
- **Property-based testing** with fast-check
- **Zero build errors** after alignment fixes

### In Progress
- 🔄 Results display with tables and charts (Milestone 4)
- 🔄 Saved reports functionality (Milestone 5)
- 🔄 Admin panel for system management (Milestone 6)

## 🏗️ Architecture

```
┌─────────────┐
│   Browser   │
│  (React 19) │
└──────┬──────┘
       │ HTTPS
       ▼
┌─────────────────────────────────────┐
│   AWS Amplify Hosting (Free Tier)  │
│   - Next.js 16 App Router           │
│   - Static Assets + SSR             │
└──────┬──────────────────────────────┘
       │ API Calls
       ▼
┌─────────────────────────────────────┐
│   Amplify Gen 2 Backend             │
│   - API Routes (Next.js)            │
│   - Server Actions                  │
└──────┬──────────────────────────────┘
       │
       ├─────────────┬──────────────┬────────────┐
       │             │              │            │
       ▼             ▼              ▼            ▼
┌──────────┐  ┌──────────┐  ┌──────────┐  ┌──────────┐
│ Cognito  │  │ Bedrock  │  │ Lambda   │  │CloudWatch│
│User Pool │  │ Claude   │  │ Function │  │   Logs   │
└──────────┘  └──────────┘  └────┬─────┘  └──────────┘
                                  │
                                  ▼
                          ┌──────────────┐
                          │  IMIRS SQL   │
                          │  Server DB   │
                          └──────────────┘
```

## 🚀 Getting Started

### Prerequisites

- **Node.js** >= 20.0.0
- **npm** or **yarn**
- **AWS Account** with appropriate permissions
- **AWS CLI** configured with profile
- **Git**

### Required AWS Services

1. **AWS Amplify** (Free tier)
   - Hosting for Next.js application
   - CI/CD pipeline

2. **Amazon Cognito**
   - User Pool for authentication
   - Custom attributes: `AdministerImirs`, `AuditImirs`, `SqlFilter`

3. **AWS Bedrock**
   - Claude 3 Haiku (default model)
   - Claude 3.5 Sonnet (escalation model)
   - Pay-per-use pricing

4. **AWS Lambda**
   - Existing `sqlserver_query_js` function with database access
   - Executes SQL queries against IMIRS database

5. **Amazon S3**
   - Configuration storage (schema, templates, examples)
   - Bucket: `imirs-ai-reporting-config`

6. **Amazon SES**
   - Email notifications for user registration
   - Admin approval/denial emails

7. **CloudWatch Logs**
   - Audit logging for all queries
   - Usage statistics and monitoring

### Installation

1. **Clone the repository**
   ```bash
   git clone https://github.com/ChuckChekuri/imirs-ai-reporting.git
   cd imirs-ai-reporting
   ```

2. **Install dependencies**
   ```bash
   npm install
   ```

3. **Configure environment variables**
   ```bash
   cp .env.example .env.local
   ```

   Edit `.env.local` with your AWS configuration:
   ```env
   # AWS Configuration
   NEXT_PUBLIC_AWS_REGION=us-east-1
   AWS_PROFILE=your-aws-profile
   AWS_REGION=us-east-1

   # Cognito Configuration
   NEXT_PUBLIC_COGNITO_USER_POOL_ID=your-user-pool-id
   NEXT_PUBLIC_COGNITO_CLIENT_ID=your-client-id

   # AWS Bedrock
   AWS_BEDROCK_REGION=us-east-1
   BEDROCK_MODEL_HAIKU=anthropic.claude-3-haiku-20240307-v1:0
   BEDROCK_MODEL_SONNET=anthropic.claude-3-5-sonnet-20240620-v1:0

   # Lambda Function
   SQLSERVER_QUERY_LAMBDA_ARN=arn:aws:lambda:region:account:function:sqlserver_query_js

   # S3 Configuration Storage
   S3_BUCKET_NAME=imirs-ai-reporting-config
   S3_SCHEMA_KEY=schema/database-schema.json
   S3_TEMPLATES_KEY=templates/sql-templates.json
   S3_EXAMPLES_KEY=examples/one-shot-examples.json
   S3_GUIDELINES_KEY=guidelines/formatting-guidelines.md

   # Email Configuration (AWS SES)
   SES_FROM_EMAIL=your-email@domain.com
   SES_ADMIN_EMAIL=admin@domain.com

   # Application URL
   NEXT_PUBLIC_APP_URL=http://localhost:3000
   ```

4. **Set up AWS Amplify backend**
   ```bash
   npm run amplify:sandbox
   ```

   This will:
   - Deploy Cognito User Pool with custom attributes
   - Set up authentication resources
   - Create AppSync API with DynamoDB tables
   - Generate `amplify_outputs.json`

5. **Upload initial configuration to S3**
   ```bash
   # Upload database schema
   aws s3 cp config-templates/database-schema.json s3://imirs-ai-reporting-config/schema/database-schema.json

   # Upload SQL templates
   aws s3 cp config-templates/sql-templates.json s3://imirs-ai-reporting-config/templates/sql-templates.json

   # Upload one-shot examples
   aws s3 cp config-templates/one-shot-examples.json s3://imirs-ai-reporting-config/examples/one-shot-examples.json

   # Upload formatting guidelines
   aws s3 cp config-templates/formatting-guidelines.md s3://imirs-ai-reporting-config/guidelines/formatting-guidelines.md
   ```

6. **Run development server**
   ```bash
   npm run dev
   ```

   Open [http://localhost:3000](http://localhost:3000) in your browser.

## 🔧 IMIRS Database Connectivity

### Lambda Function Setup

The application connects to the IMIRS SQL Server database through a pre-existing Lambda function (`sqlserver_query_js`). This Lambda:

1. **Has database credentials** stored in AWS Secrets Manager
2. **Maintains connection pooling** for optimal performance
3. **Executes SQL queries** with proper error handling
4. **Returns JSON responses** with results and metadata

### Lambda Invocation

```typescript
// Example payload sent to Lambda
{
  "sql_query": "SELECT TOP (1000) * FROM dbo.TestResults WHERE ...",
  "userId": "user@example.com",
  "serialize_dates": true,
  "metadata": {
    "question": "Show me recent test results",
    "timestamp": "2025-11-22T10:30:00Z"
  }
}

// Expected response
{
  "statusCode": 200,
  "body": {
    "data": [...],
    "columns": ["Column1", "Column2", ...],
    "rowCount": 847,
    "executionTime": 234
  }
}
```

### Row-Level Security

The system automatically injects SQL filters based on user permissions:

- **Regular Users**: Custom SQL filter defined by admin (e.g., `"ProjectID IN (1,2,3)"`)
- **Admin/Audit Users**: No filter restrictions

```typescript
// Example: User with filter "ProjectID IN (1,2,3)"
// Generated SQL: SELECT * FROM Tests WHERE Status = 'Complete'
// Injected SQL: SELECT * FROM Tests WHERE Status = 'Complete' AND ProjectID IN (1,2,3)
```

## 📦 Deployment

### Deploy to AWS Amplify

1. **Connect repository to Amplify**
   ```bash
   # Using Amplify CLI
   amplify init
   amplify push
   ```

2. **Configure build settings** in Amplify Console:
   ```yaml
   version: 1
   frontend:
     phases:
       preBuild:
         commands:
           - npm ci
       build:
         commands:
           - npm run build
     artifacts:
       baseDirectory: .next
       files:
         - '**/*'
     cache:
       paths:
         - node_modules/**/*
   ```

3. **Set environment variables** in Amplify Console
   - Add all variables from `.env.local`
   - Ensure `amplify_outputs.json` is generated during build

4. **Deploy**
   ```bash
   npm run amplify:deploy
   ```

## 🧪 Testing

```bash
# Run all tests
npm test

# Run tests with UI
npm run test:ui

# Run tests once (CI mode)
npm run test:run

# Test AWS access
npm run test:aws
```

## 📝 Development Scripts

```bash
# Development
npm run dev              # Start dev server
npm run build            # Build for production
npm run start            # Start production server

# Code Quality
npm run lint             # Run Biome linter
npm run format           # Format code with Biome

# Amplify
npm run amplify:sandbox  # Start Amplify sandbox
npm run amplify:deploy   # Deploy to production

# Testing
npm run test             # Run Vitest
npm run test:ui          # Run Vitest with UI
npm run test:aws         # Test AWS connectivity
```

## 🔐 Security

- **Authentication**: Amazon Cognito with email/password
- **Authorization**: Custom claims for role-based access
- **Row-Level Security**: Automatic SQL filter injection
- **Audit Logging**: All queries logged to CloudWatch
- **SQL Validation**: Only SELECT statements with TOP clause allowed
- **No Direct DB Access**: All queries through Lambda function

## 📚 Documentation

- [Requirements](./specs/requirements.md) - Detailed functional requirements
- [Design](./specs/design.md) - Architecture and technical design
- [Tasks](./specs/tasks_in_order.md) - Implementation plan and progress

## 🤝 Contributing

This is a private project for the Illinois Tollway. For questions or issues, contact sbandi@interraservices.com.

## 📄 License

Proprietary - Illinois Tollway Internal Use Only

## 🙏 Acknowledgments

- AWS Amplify Gen 2 for full-stack development platform
- Anthropic Claude for AI-powered query generation
- Next.js and React teams for excellent frameworks
- shadcn/ui for beautiful component library
