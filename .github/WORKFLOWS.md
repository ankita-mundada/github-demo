# GitHub Workflows Setup Guide

This repository includes GitHub Actions workflows for automated Shopify theme deployment and validation.

## 🔧 Setup Instructions

### 1. Required GitHub Secrets

To enable the workflows, you need to add the following secrets to your GitHub repository:

1. Go to your GitHub repository
2. Navigate to **Settings** → **Secrets and variables** → **Actions**
3. Add the following repository secrets:

#### Required Secrets:
- `SHOPIFY_CLI_THEME_TOKEN`: Your Shopify CLI theme access token
- `SHOPIFY_STORE`: Your Shopify store URL (e.g., `your-store.myshopify.com`)

#### Optional Secrets (for advanced deployments):
- `SHOPIFY_DEV_STORE`: Development store URL
- `SHOPIFY_DEV_THEME_TOKEN`: Development theme token
- `SHOPIFY_STAGING_STORE`: Staging store URL  
- `SHOPIFY_STAGING_THEME_TOKEN`: Staging theme token
- `SHOPIFY_STAGING_THEME_ID`: Staging theme ID
- `SHOPIFY_PROD_STORE`: Production store URL
- `SHOPIFY_PROD_THEME_TOKEN`: Production theme token
- `SHOPIFY_PROD_THEME_ID`: Production theme ID

### 2. How to Get Shopify CLI Theme Token

1. **Install Shopify CLI** (if not already installed):
   ```bash
   npm install -g @shopify/cli @shopify/theme
   ```

2. **Authenticate with Shopify**:
   ```bash
   shopify auth login
   ```

3. **Generate Theme Access Token**:
   - Go to your Shopify Admin
   - Navigate to **Apps** → **App and sales channel settings**
   - Click **Develop apps**
   - Create a new app or use existing one
   - Configure **Admin API scopes**: `read_themes`, `write_themes`
   - Install the app and copy the access token

### 3. Environment Setup

Create a `.env` file for local development (add to .gitignore):

```env
SHOPIFY_CLI_THEME_TOKEN=your_theme_token_here
SHOPIFY_STORE=your-store.myshopify.com
```

## 🚀 Workflows Overview

### 1. Theme Validation (`deploy.yml`)
- **Triggers**: Push to `main` or `develop`, Pull requests to `main`
- **Actions**:
  - Validates theme structure
  - Runs `shopify theme check` for Liquid linting
  - Checks for security issues

### 2. Deployment Workflows
- **Development**: Automatically deploys to development theme on `develop` branch
- **Production**: Deploys to live theme on `main` branch (requires manual approval)

### 3. Advanced Workflow (`shopify-theme.yml`)
Includes additional features:
- Security scanning
- Performance testing with Lighthouse
- Multi-environment deployments
- Backup creation before production deployment

## 🔄 Workflow Usage

### Development Workflow:
1. Create feature branch from `develop`
2. Make changes and push
3. Create PR to `develop`
4. Merge to `develop` → Auto-deploy to dev environment
5. Create PR from `develop` to `main`
6. Merge to `main` → Deploy to production (with approval)

### Production Deployment:
1. Push to `main` branch
2. Workflow runs validation
3. Manual approval required for production deployment
4. Theme deploys to live store

## ⚙️ GitHub Environments (Optional)

For enhanced control, set up GitHub environments:

1. Go to **Settings** → **Environments**
2. Create environments: `development`, `staging`, `production`
3. Configure protection rules:
   - **Production**: Require manual approval
   - **Staging**: Auto-deploy after validation
   - **Development**: Auto-deploy

## 🔍 Monitoring

- Check **Actions** tab for workflow status
- Review deployment logs for any issues
- Monitor theme performance after deployments

## 📝 Notes

- The workflows use the latest Shopify CLI
- Theme validation runs on every PR
- Production deployments require manual approval for safety
- All workflows support both Unix and Windows runners

## 🆘 Troubleshooting

### Common Issues:

1. **Authentication Failed**:
   - Verify `SHOPIFY_CLI_THEME_TOKEN` is correct
   - Check token permissions include `read_themes` and `write_themes`

2. **Store Not Found**:
   - Verify `SHOPIFY_STORE` format (include `.myshopify.com`)
   - Check store is accessible

3. **Theme Push Failed**:
   - Verify theme structure is valid
   - Check for liquid syntax errors
   - Ensure no conflicting themes

For more help, check the [Shopify CLI documentation](https://shopify.dev/docs/themes/tools/cli).