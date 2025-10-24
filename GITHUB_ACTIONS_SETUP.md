# GitHub Actions CI/CD Pipeline Setup Instructions

This document provides instructions for setting up the GitHub Actions CI/CD pipeline for the Ticket System Backend.

## Overview

The CI/CD pipeline includes the following steps:
1. **Linting** - ESLint code quality checks (`npm run lint`)
2. **Testing** - Jest unit tests (`npm run test`)
3. **Publishing** - Publishes package to GitHub Packages (only on release)

## Pipeline Triggers

The pipeline automatically runs on:
- **Push** to `main` or `develop` branches
- **Pull Requests** targeting `main` or `develop` branches
- **Release** creation
- **Manual trigger** via workflow_dispatch in GitHub Actions tab

## Required Environment Setup

### GitHub Secrets and Variables

You need to configure the following secrets and variables in your GitHub repository settings:

#### Navigate to Settings
1. Go to your repository on GitHub
2. Click on **Settings**
3. In the left sidebar, click on **Secrets and variables** → **Actions**

#### Required Secrets

Create the following **Repository secrets**:

1. **MONGO_URL**
   - Description: MongoDB Atlas connection string
   - Format: `mongodb+srv://username:password@cluster.mongodb.net/database?retryWrites=true&w=majority`
   - How to get it:
     - Log in to [MongoDB Atlas](https://www.mongodb.com/cloud/atlas)
     - Select your cluster
     - Click "Connect" → "Connect your application"
     - Copy the connection string
     - Replace `<password>` with your actual password
     - Replace `<database>` with your database name (e.g., `ticketsystem`)

#### Required Variables

Create the following **Repository variables**:

1. **PORT**
   - Description: Port number for the application
   - Value: `6001` (or your preferred port)

### Environment Configuration

The pipeline uses the **production** environment. To set this up:

1. Go to **Settings** → **Environments**
2. Click **New environment**
3. Name it: `production`
4. (Optional) Add environment protection rules:
   - Required reviewers
   - Wait timer
   - Deployment branches (restrict to `main` branch)

### MongoDB Atlas Setup

If you haven't set up MongoDB Atlas yet:

1. **Create Account**
   - Go to [MongoDB Atlas](https://www.mongodb.com/cloud/atlas)
   - Sign up for a free account

2. **Create Cluster**
   - Click "Build a Database"
   - Choose the free tier (M0)
   - Select your preferred cloud provider and region
   - Click "Create"

3. **Configure Network Access**
   - Go to **Network Access**
   - Click **Add IP Address**
   - Click **Allow Access from Anywhere** (0.0.0.0/0)
   - ⚠️ **Security Warning**: Allowing access from anywhere is not recommended for production
   - For better security, consider:
     - Using MongoDB Atlas's built-in VPC peering
     - Restricting to specific IP ranges
     - Using temporary credentials with IP restrictions

4. **Create Database User**
   - Go to **Database Access**
   - Click **Add New Database User**
   - Choose authentication method (Username/Password)
   - Create a username and strong password
   - Set privileges (e.g., "Read and write to any database")
   - Click **Add User**

5. **Get Connection String**
   - Go to **Clusters**
   - Click **Connect**
   - Select "Connect your application"
   - Copy the connection string
   - Add it as the `MONGO_URL` secret in GitHub

## Local Development Setup

For local development, create a `.env` file in `ticketsystem/backend/`:

```env
MONGO_URL=mongodb+srv://username:password@cluster.mongodb.net/ticketsystem?retryWrites=true&w=majority
PORT=6001
NODE_ENV=development
```

**Note**: The `.env` file is gitignored and should never be committed.

## Testing the Pipeline

### Test Locally

Before pushing, you can test the pipeline steps locally:

```bash
cd ticketsystem/backend

# Install dependencies
npm ci

# Run linting
npm run lint

# Run tests (requires MONGO_URL in .env)
npm test

# Start the server
npm start
```

### Test in GitHub Actions

1. **Push to a feature branch**:
   ```bash
   git checkout -b test-pipeline
   git push origin test-pipeline
   ```

2. **Create a Pull Request** targeting `main` or `develop`
   - The pipeline will automatically run
   - Check the Actions tab to see the results

3. **Manual trigger**:
   - Go to **Actions** tab
   - Select "CI/CD Pipeline"
   - Click "Run workflow"
   - Select branch and click "Run workflow"

## Pipeline Jobs

### 1. Lint Job
- Runs ESLint to check code quality
- Fails if there are any errors (warnings are allowed)
- Runs first, before testing

### 2. Test Job
- Depends on successful lint job
- Installs dependencies
- Verifies MongoDB connection string is set
- Runs Jest tests
- Requires access to MongoDB Atlas

### 3. Publish Job
- Only runs on `release` events
- Depends on successful test job
- Publishes package to GitHub Packages Registry
- Requires write permissions to packages

## Troubleshooting

### Pipeline Fails with "MONGO_URL is not set"
- Ensure `MONGO_URL` secret is configured in repository settings
- Check that the environment is set to `production` in the workflow
- Verify the secret name matches exactly (case-sensitive)

### Tests Fail to Connect to MongoDB
- Verify MongoDB Atlas network access allows GitHub Actions IPs
- Check that the connection string is correct
- Ensure the database user has proper permissions
- ⚠️ **Last Resort Only**: As a temporary debugging step, you can try setting Network Access to "Allow Access from Anywhere" (0.0.0.0/0)
  - This poses a security risk and should only be used temporarily
  - Make sure to restrict access again once debugging is complete
  - Never use this configuration in production without proper security measures

### Linting Fails
- Run `npm run lint` locally to see errors
- Fix any ESLint errors (not warnings)
- Commit and push the fixes

### Package Publishing Fails
- Check that the workflow has proper permissions
- Verify package name doesn't conflict with existing packages
- Ensure the release event triggered the workflow

## Additional Notes

- The pipeline uses Node.js version 20
- Tests run with `CI=true` environment variable
- The working directory is set to `ticketsystem/backend`
- ESLint is configured to recognize Jest globals in test files
- The application loads `.env` in non-production environments only

## Support

If you encounter issues not covered in this document:
1. Check the Actions tab for detailed error logs
2. Review the workflow file at `.github/workflows/ci.yml`
3. Verify all secrets and variables are correctly configured
4. Ensure MongoDB Atlas is properly configured and accessible
