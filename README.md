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

**Milestone Progress:** 5 of 8 milestones complete (62.5%)

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

- ✅ **Results Display** (Milestone 4)
  - Interactive data tables with sorting and filtering
  - Automatic chart generation based on data types
  - Export to Excel, CSV, and PDF formats
  - Report saving and management
  - Query history tracking

- ✅ **Saved Reports** (Milestone 5)
  - Report gallery with grid layout
  - Dedicated report view page with full-screen layout
  - Previous/Next navigation through saved reports
  - Collapsible SQL query display with formatting
  - Smart caching strategy (cache SQL generation, not results)
  - Delete reports with confirmation

- ✅ **Admin Panel** (Milestone 6)
  - User Management: View, approve, edit users with real-time statistics
  - Usage Statistics: System-wide metrics, query activity charts, top users
  - Database Schema: View and edit database structure
  - SQL Templates: Create, test, verify templates with DynamoDB storage
  - All data is real-time from DynamoDB (no mock data)

### Code Statistics
- **122 files** of TypeScript/React code
- **15,600+ lines** of code (excluding node_modules)
- **TypeScript strict mode** enabled
- **Property-based testing** with fast-check
- **Zero build errors** with comprehensive type safety

### Current Phase
- 🔄 **Phase 2: Production Deployment** (Milestone 8 - In Progress)
  - CloudWatch alarms configured
  - Comprehensive README and troubleshooting guide
  - Amplify hosting setup in progress
  - CI/CD pipeline configuration

### Not Yet Implemented
- ⏳ Property tests and comprehensive test coverage (Milestone 7)

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

   **Note:** Cognito configuration is now automatically loaded from `amplify_outputs.json` - no need to manually configure user pool IDs.

4. **Set up AWS Amplify backend**
   ```bash
   npm run amplify:sandbox
   ```

   This will:
   - Deploy Cognito User Pool with custom attributes
   - Set up authentication resources
   - Create AppSync API with DynamoDB tables (UserProfile, SavedReport, QueryHistory)
   - Generate `amplify_outputs.json` with configuration

5. **Seed admin user**
   ```bash
   npx tsx scripts/seed-admin-user.ts
   ```

   This creates the initial admin user with:
   - Full admin and audit permissions
   - UserProfile in DynamoDB
   - Cognito user with custom attributes
   - Default credentials (change after first login)

6. **Upload initial configuration to S3**
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

7. **Run development server**
   ```bash
   npm run dev
   ```

   Open [http://localhost:3000](http://localhost:3000) in your browser.

## �️I Server-Side Admin Utilities

### DynamoDB Direct Access

For server-side operations (API routes), the application provides direct DynamoDB access utilities that bypass Amplify's authorization layer. This is useful for:

- Admin operations on user profiles
- Updating query counts and statistics
- Managing templates, examples, and guidelines
- Bulk operations and migrations

**Usage Example:**
```typescript
import { 
  updateUserProfile, 
  incrementUserQueryCount,
  createQueryHistory 
} from '@/lib/dynamodb-admin-utils';

// Update user profile
await updateUserProfile(userId, {
  queryCount: 10,
  lastLoginAt: new Date().toISOString(),
});

// Increment query count
await incrementUserQueryCount(userId);

// Log query to history
await createQueryHistory({
  userId,
  question: "Show me test results",
  sql: "SELECT * FROM Tests",
  executionTime: 234,
  rowCount: 100,
  success: true,
});
```

**Important:** These utilities bypass authorization checks, so ensure proper authentication is verified before calling them.

## 📋 SQL Template Management

### DynamoDB Storage

SQL templates are now stored in DynamoDB for better scalability and real-time updates:

**Features:**
- ✅ **Test SQL Button** - Execute templates to verify they work correctly
- ✅ **Verification Status** - Green "Verified" or yellow "Unverified" badges
- ✅ **Create Report Button** - Generate saved reports from templates
- ✅ **Real-time Updates** - No cache invalidation issues
- ✅ **Full CRUD Operations** - Create, read, update, delete templates
- ✅ **Formatted SQL** - Support for comments and newlines in templates

**Template Structure:**
```typescript
{
  id: string;
  name: string;
  description: string;
  sql: string;
  category: string;
  keywords: string[];
  verified: boolean;
  lastTestedAt: string;
  lastTestedBy: string;
  createdBy: string;
  createdAt: string;
  updatedAt: string;
}
```

**Migration from S3:**
```bash
# Migrate existing templates from S3 to DynamoDB
npx tsx scripts/migrate-templates-to-dynamodb.ts
```

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

### Production Deployment to AWS Amplify

**App ID:** `d3tlvc2i7h5gsh`  
**Default Domain:** `d3tlvc2i7h5gsh.amplifyapp.com`

#### Step 1: Connect GitHub Repository

1. Go to [AWS Amplify Console](https://console.aws.amazon.com/amplify/)
2. Click on "imirs-ai-reporting" app
3. Click "Host web app" or "Connect branch"
4. Select GitHub as source and authorize AWS Amplify
5. Select your repository and the `main` branch
6. Amplify will auto-detect `amplify.yml` and Next.js configuration

#### Step 2: Configure Environment Variables

In Amplify Console → App Settings → Environment variables, add:

```env
# AWS Configuration
AWS_REGION=us-east-1
NEXT_PUBLIC_AWS_REGION=us-east-1

# Lambda Function
SQLSERVER_QUERY_LAMBDA_ARN=arn:aws:lambda:us-east-1:097056675955:function:sqlserver_query_js

# S3 Configuration
S3_BUCKET_NAME=imirs-ai-reporting
S3_BUCKET_REGION=us-east-1
S3_SCHEMA_KEY=schema/database-schema.json
S3_TEMPLATES_KEY=templates/sql-templates.json
S3_EXAMPLES_KEY=examples/one-shot-examples.json
S3_GUIDELINES_KEY=guidelines/formatting-guidelines.md

# Bedrock Models
AWS_BEDROCK_REGION=us-east-1
BEDROCK_MODEL_HAIKU=anthropic.claude-3-haiku-20240307-v1:0
BEDROCK_MODEL_SONNET=anthropic.claude-3-5-sonnet-20240620-v1:0

# Email (SES)
SES_FROM_EMAIL=chuck@axiomaticinfo.com
SES_ADMIN_EMAIL=chuck@axiomaticinfo.com

# Application URL (update after first deployment)
NEXT_PUBLIC_APP_URL=https://main.d3tlvc2i7h5gsh.amplifyapp.com
```

**Important:** Do NOT add `AWS_PROFILE` in production - Amplify uses IAM roles.

#### Step 3: Configure IAM Permissions

Attach this policy to the Amplify service role:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": ["lambda:InvokeFunction"],
      "Resource": "arn:aws:lambda:us-east-1:097056675955:function:sqlserver_query_js"
    },
    {
      "Effect": "Allow",
      "Action": ["s3:GetObject", "s3:PutObject", "s3:ListBucket"],
      "Resource": [
        "arn:aws:s3:::imirs-ai-reporting",
        "arn:aws:s3:::imirs-ai-reporting/*"
      ]
    },
    {
      "Effect": "Allow",
      "Action": ["bedrock:InvokeModel"],
      "Resource": "*"
    },
    {
      "Effect": "Allow",
      "Action": ["ses:SendEmail", "ses:SendRawEmail"],
      "Resource": "*"
    },
    {
      "Effect": "Allow",
      "Action": [
        "dynamodb:GetItem",
        "dynamodb:PutItem",
        "dynamodb:UpdateItem",
        "dynamodb:DeleteItem",
        "dynamodb:Query",
        "dynamodb:Scan"
      ],
      "Resource": "arn:aws:dynamodb:us-east-1:*:table/*"
    },
    {
      "Effect": "Allow",
      "Action": [
        "logs:CreateLogGroup",
        "logs:CreateLogStream",
        "logs:PutLogEvents"
      ],
      "Resource": "arn:aws:logs:us-east-1:*:log-group:/imirs-ai-reporting/*"
    }
  ]
}
```

#### Step 4: Deploy Backend

```bash
# Deploy Amplify backend (Cognito, DynamoDB)
npx ampx pipeline-deploy --branch main --app-id d3tlvc2i7h5gsh
```

This creates:
- Cognito User Pool with custom attributes
- DynamoDB tables (UserProfile, SavedReport, QueryHistory, SQLTemplate)
- AppSync API
- `amplify_outputs.json` configuration

#### Step 5: Seed Admin User

```bash
# Create initial admin user
npx tsx scripts/seed-admin-user.ts
```

#### Step 6: Upload Configuration to S3

```bash
aws s3 cp config-templates/database-schema.json s3://imirs-ai-reporting/schema/database-schema.json --profile tdmadmin
aws s3 cp config-templates/sql-templates.json s3://imirs-ai-reporting/templates/sql-templates.json --profile tdmadmin
aws s3 cp config-templates/one-shot-examples.json s3://imirs-ai-reporting/examples/one-shot-examples.json --profile tdmadmin
aws s3 cp config-templates/formatting-guidelines.md s3://imirs-ai-reporting/guidelines/formatting-guidelines.md --profile tdmadmin
```

#### Step 7: Migrate Templates to DynamoDB

```bash
npx tsx scripts/migrate-templates-to-dynamodb.ts
```

#### Step 8: Configure CloudWatch Alarms

```bash
npm run setup:alarms
```

This creates alarms for:
- Query execution time > 4 seconds
- Bedrock API failures
- Lambda invocation errors
- High error rate (> 10%)

Check your email to confirm SNS subscription.

#### Step 9: Deploy Frontend

Push to main branch to trigger automatic deployment:

```bash
git push origin main
```

Or deploy manually:

```bash
npm run amplify:deploy
```

#### Step 10: Verify Deployment

1. Visit the deployed URL
2. Test authentication (login/register)
3. Test query execution
4. Test saved reports
5. Test admin panel (if admin user)

### Build Configuration

The `amplify.yml` file is configured for:
- Node.js 20 via nvm
- Legacy peer deps for compatibility
- Linux-specific Parcel watcher
- Environment variable injection
- Next.js standalone build verification
- Comprehensive caching

### Continuous Deployment

Amplify automatically deploys when you push to `main`:
- Runs backend deployment
- Installs dependencies
- Builds Next.js application
- Deploys to production
- Invalidates CDN cache

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

## 🔧 Troubleshooting

### Build Failures

**Issue:** Build fails in Amplify Console  
**Solution:**
- Check build logs in Amplify Console
- Verify all environment variables are set
- Ensure Node.js version is 20+
- Check for TypeScript errors: `npm run build` locally
- Verify `amplify_outputs.json` is generated

**Issue:** "Module not found" errors  
**Solution:**
- Clear cache: Delete `.next` folder and rebuild
- Check `serverExternalPackages` in `next.config.ts`
- Verify all dependencies are in `package.json`

### Authentication Issues

**Issue:** Users can't log in  
**Solution:**
- Verify Cognito User Pool exists
- Check `amplify_outputs.json` is present
- Verify redirect URLs in Cognito settings
- Check user status is ACTIVE (not PENDING_APPROVAL)

**Issue:** "User does not exist" error  
**Solution:**
- Verify user was created in Cognito
- Check UserProfile exists in DynamoDB
- Run seed script: `npx tsx scripts/seed-admin-user.ts`

### Query Execution Errors

**Issue:** Queries fail with Lambda errors  
**Solution:**
- Verify Lambda ARN is correct in environment variables
- Check IAM permissions for Lambda invocation
- Verify Lambda is in same region (us-east-1)
- Check Lambda logs in CloudWatch for detailed errors
- Test Lambda directly: `npm run test:aws`

**Issue:** "Access denied" errors  
**Solution:**
- Verify user has proper SQL filter configured
- Check Amplify service role has Lambda invoke permissions
- Verify Lambda has database access

### DynamoDB Access Issues

**Issue:** Can't read/write DynamoDB  
**Solution:**
- Verify table names match `amplify_outputs.json`
- Check IAM permissions for DynamoDB operations
- Verify tables exist in correct region
- Check table indexes are created

**Issue:** "ResourceNotFoundException"  
**Solution:**
- Run `npm run amplify:sandbox` to create tables
- Verify backend deployment completed successfully
- Check table names in AWS Console

### S3 Configuration Issues

**Issue:** Can't load schema/templates  
**Solution:**
- Verify S3 bucket exists: `imirs-ai-reporting`
- Check files are uploaded to correct paths
- Verify IAM permissions for S3 read access
- Test S3 access: `aws s3 ls s3://imirs-ai-reporting/ --profile tdmadmin`

### Bedrock API Errors

**Issue:** "Access denied" to Bedrock  
**Solution:**
- Verify Bedrock is enabled in your AWS account
- Check IAM permissions for `bedrock:InvokeModel`
- Verify model IDs are correct
- Check region is us-east-1

**Issue:** "Throttling" errors  
**Solution:**
- Implement exponential backoff (already in code)
- Request quota increase from AWS
- Use caching to reduce API calls

### Performance Issues

**Issue:** Slow query execution  
**Solution:**
- Check CloudWatch logs for execution times
- Verify Lambda has adequate memory/timeout
- Check database query performance
- Review SQL query complexity

**Issue:** High costs  
**Solution:**
- Enable caching (already implemented)
- Use Haiku model by default (cheaper)
- Monitor Bedrock usage in CloudWatch
- Review query patterns for optimization

### Email Notification Issues

**Issue:** Emails not sending  
**Solution:**
- Verify SES is configured and email addresses are verified
- Check IAM permissions for SES
- Review SES sending limits
- Check CloudWatch logs for SES errors

### Local Development Issues

**Issue:** Can't connect to AWS services locally  
**Solution:**
- Verify AWS CLI is configured: `aws configure`
- Check AWS profile is set: `export AWS_PROFILE=tdmadmin`
- Test credentials: `aws sts get-caller-identity`
- Verify `.env.local` has correct values

**Issue:** "amplify_outputs.json not found"  
**Solution:**
- Run `npm run amplify:sandbox` to generate it
- Verify file exists in project root
- Don't add to `.gitignore` (it's needed for builds)

### Common Error Messages

**"Invalid SQL: Must be a SELECT statement"**  
- Only SELECT queries are allowed for security
- Remove INSERT, UPDATE, DELETE, DROP statements

**"Invalid SQL: Must include TOP clause"**  
- Add `TOP (1000)` to limit results
- Example: `SELECT TOP (1000) * FROM Table`

**"Row-level security filter injection failed"**  
- Check user's SQL filter is valid SQL syntax
- Verify filter doesn't conflict with query structure

**"Bedrock model escalation failed"**  
- Check both Haiku and Sonnet models are accessible
- Verify model IDs in environment variables
- Review CloudWatch logs for detailed error

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

This is a private project for the Illinois Tollway. For questions or issues, contact sbandi@interraservices.com or chuck@axiomaticinfo.com.

## 📄 License

Proprietary - Illinois Tollway Internal Use Only

## 🙏 Acknowledgments

- AWS Amplify Gen 2 for full-stack development platform
- Anthropic Claude for AI-powered query generation
- Next.js and React teams for excellent frameworks
- shadcn/ui for beautiful component library
