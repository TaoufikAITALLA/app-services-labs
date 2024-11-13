# Azure Web App Deployment Guide with Staging Slots

## Table of Contents
- [Prerequisites](#prerequisites)
- [Part 1: Setting Up Your Development Environment](#part-1-setting-up-your-development-environment)
- [Part 2: Creating an Azure Web App](#part-2-creating-an-azure-web-app)
- [Part 3: Working with VSCode](#part-3-working-with-vscode)
- [Part 4: Deployment Slots Management](#part-4-deployment-slots-management)
- [Part 5: Implementing Slot Switching](#part-5-implementing-slot-switching)
- [Troubleshooting](#troubleshooting)

## Prerequisites

Before starting, ensure you have the following:

- [ ] Node.js (LTS version)
- [ ] Visual Studio Code
- [ ] Azure CLI
- [ ] Azure Account
- [ ] Git

## Part 1: Setting Up Your Development Environment

### 1.1 Install Required Extensions in VSCode

1. Open VSCode and install these extensions:
   - Azure Tools
   - Azure App Service
   - Azure Resources

### 1.2 Install Azure CLI

```bash
# For Windows (Using PowerShell as administrator)
winget install -e --id Microsoft.AzureCLI

# For macOS
brew install azure-cli

# For Linux
curl -sL https://aka.ms/InstallAzureCLIDeb | sudo bash
```

### 1.3 Login to Azure

```bash
# Login to Azure
az login

# Verify your subscription
az account show
```

## Part 2: Creating an Azure Web App

### 2.1 Create Resource Group

```bash
# Create a resource group
az group create --name myWebAppResourceGroup --location francecentral

# Create an App Service plan
az appservice plan create \
    --name myAppServicePlan \
    --resource-group myWebAppResourceGroup \
    --sku S1 \
    --is-linux
```

### 2.2 Create Web App

```bash
# Create the web app
myWebAppName="myWebApp-$(openssl rand -hex 4)"
az webapp create \
    --resource-group myWebAppResourceGroup \
    --plan myAppServicePlan \
    --name $myWebAppName \
    --runtime "NODE:20-lts" \
    --deployment-local-git
```

## Part 3: Working with VSCode

### 3.1 Create Sample Node.js Application

1. Create a new directory and initialize project:

```bash
# Install Express Generator
npx express-generator myExpressApp --view ejs
```

2. Create basic Express application:

```bash

# Install dependencies
cd myExpressApp && npm install

# Start the development server with debug information.
DEBUG=myexpressapp:* npm start

# add this code to app.js

```


3. Modify app.js
Open `app.js` in your preferred text editor and make the following changes:

* Locate the middleware section (after the app declarations)
* Add the following middleware code before the route declarations (after line 20):
```javascript
// Add environment information middleware
app.use((req, res, next) => {
  res.locals.environment = process.env.ENVIRONMENT || 'development';
  res.locals.version = process.env.VERSION || '1.0.0';
  next();
});
```

4. Update index.ejs
* Navigate to `views/index.ejs`
* Add the environment and version information display (after line 10):
```html
<!-- Add this before the closing </body> tag -->
<div class="env-info">
  <p>Environment: <%= environment %></p>
  <p>Version: <%= version %></p>
</div>
```

5. Add Styling (Optional)
* Open `public/stylesheets/style.css`
* Add styling for the environment information:
```css
.env-info {
  margin-top: 20px;
  padding: 10px;
  background-color: #f5f5f5;
  border-radius: 4px;
}

.env-info p {
  margin: 5px 0;
  color: #333;
}
```

6. Start the Application

```bash
ENVIRONMENT=production VERSION=2.0.0 DEBUG=myexpressapp:* npm start

```
## 9. Verify Installation
* Open your web browser
* Navigate to `http://localhost:3000`
* You should see the Express welcome page with environment and version information

7.  Project Structure
Your project structure should now look like this:
```
myExpressApp/
├── bin/
│   └── www
├── public/
│   ├── images/
│   ├── javascripts/
│   └── stylesheets/
│       └── style.css
├── routes/
│   ├── index.js
│   └── users.js
├── views/
│   ├── error.ejs
│   └── index.ejs
├── app.js
├── package.json
└── package-lock.json
```


### 3.2 Configure Deployment in VSCode

1. Open Command Palette (Ctrl+Shift+P)
2. Type "Azure: Sign In"
3. Select your subscription
4. Right-click on your web app in Azure Explorer
5. Select "Deploy to Web App"
6. Choose "myExpressApp folder"

## Part 4: Deployment Slots Management

### 4.1 Create Staging Slot

```bash
# Create staging slot
az webapp deployment slot create \
    --name $myWebAppName \
    --resource-group myWebAppResourceGroup \
    --slot staging
```

### 4.2 Configure Slot-specific Settings

```bash
# Configure staging environment variables
az webapp config appsettings set \
    --resource-group myWebAppResourceGroup \
    --name $myWebAppName \
    --slot staging \
    --settings ENVIRONMENT="staging" VERSION="1.0.1"

# Configure production environment variables
az webapp config appsettings set \
    --resource-group myWebAppResourceGroup \
    --name $myWebAppName \
    --settings ENVIRONMENT="production" VERSION="1.0.0"
```

## Part 5: Implementing Slot Switching

### 5.1 Deploy to Staging Slot

1. In VSCode Azure Explorer:
   - Expand your web app
   - Right-click on "staging" slot
   - Select "Deploy to Slot"

### 5.2 Test Staging Deployment

1. Access staging slot URL: `https://mywebapp-staging.azurewebsites.net`
2. Verify environment variables and new features

### 5.3 Perform Slot Swap

#### Using Azure Portal:
1. Go to Azure Portal
2. Navigate to your Web App
3. Click on "Deployment slots"
4. Click "Swap" button
5. Verify source and target slots
6. Click "Swap" to confirm

#### Using Azure CLI:
```bash
# Swap staging to production
az webapp deployment slot swap \
    --resource-group myWebAppResourceGroup \
    --name myWebApp \
    --slot staging \
    --target-slot production
```

### 5.4 Verify Swap Operation

1. Monitor swap operation in Azure Portal
2. Check application logs
3. Verify production site reflects changes
4. Monitor application metrics

## Troubleshooting

### Common Issues and Solutions

1. **Deployment Fails**
   - Check application logs
   - Verify node version compatibility
   - Check deployment credentials

```bash
# View application logs
az webapp log tail \
    --resource-group myWebAppResourceGroup \
    --name myWebApp
```

2. **Slot Swap Fails**
   - Check for slot-specific settings
   - Verify application health in staging
   - Check for configuration differences

3. **Application Not Loading**
   - Verify startup command
   - Check application settings
   - Review WEBSITE_NODE_DEFAULT_VERSION setting

### Best Practices

1. **Before Swap:**
   - Test thoroughly in staging
   - Warm up staging slot
   - Verify all dependencies
   - Check application health

2. **After Swap:**
   - Monitor application metrics
   - Check error rates
   - Verify functionality
   - Keep staging slot ready for rollback

3. **General Guidelines:**
   - Use slot-specific settings appropriately
   - Implement health check endpoints
   - Maintain deployment documentation
   - Regular backup of configuration

### Auto-Swap Configuration (Optional)

Enable automatic swap for continuous deployment:

```bash
# Configure auto-swap
az webapp deployment slot auto-swap \
    --resource-group myWebAppResourceGroup \
    --name myWebApp \
    --slot staging \
    --auto-swap-slot production
```

## Additional Resources

- [Azure Web Apps Documentation](https://docs.microsoft.com/azure/app-service/)
- [Deployment Slots Reference](https://docs.microsoft.com/azure/app-service/deploy-staging-slots)
- [Node.js on Azure Reference](https://docs.microsoft.com/azure/app-service/quickstart-nodejs)
- [VSCode Azure Tools](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-node-azure-pack)
