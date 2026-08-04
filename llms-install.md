# Agency AI MCP Installation Guide

This guide will help you connect the Agency AI MCP Server to Claude or ChatGPT.

## Endpoint URL

The MCP endpoint URL is:
```
https://s.agencyai.app/mcp
```

Keep this URL handy—you'll need it for both Claude and ChatGPT installation.

## Installing in Claude

### Step 1: Open Claude Settings
1. Open Claude (https://claude.ai)
2. Click your profile icon in the top right corner
3. Select "Settings"

### Step 2: Navigate to Connectors
1. In the Settings sidebar, click "Connectors"
2. Look for the connectors section

### Step 3: Add Custom Connector
1. Click "Add custom connector" (or the "+" button to add a new connector)
2. In the prompt that appears, paste the endpoint URL: `https://s.agencyai.app/mcp`

### Step 4: Authenticate
1. You'll be redirected to the Agency AI authentication page
2. Sign in with your Agency AI account (https://agencyai.app)
3. When prompted, approve the connection request
4. You'll be redirected back to Claude

### Step 5: Verify Connection
1. Return to Claude's Connectors page
2. The Agency AI MCP connector should now appear in your list of active connectors
3. You can now use Agency AI in Claude by asking questions about your ads

## Installing in ChatGPT

### Step 1: Open ChatGPT Settings
1. Open ChatGPT (https://chatgpt.com)
2. Click your profile icon in the bottom left corner
3. Select "Settings"

### Step 2: Navigate to Connectors
1. In the Settings sidebar, click "Connectors"
2. Look for the connectors section

### Step 3: Add Connector
1. Click the "+" button or "Add connector"
2. In the prompt that appears, paste the endpoint URL: `https://s.agencyai.app/mcp`

### Step 4: Authenticate
1. You'll be redirected to the Agency AI authentication page
2. Sign in with your Agency AI account (https://agencyai.app)
3. When prompted, approve the connection request
4. You'll be redirected back to ChatGPT

### Step 5: Verify Connection
1. Return to ChatGPT's Connectors page
2. The Agency AI MCP connector should now appear in your list of active connectors
3. You can now use Agency AI in ChatGPT by asking questions about your ads

## Using Agency AI MCP

Once connected, you can ask your Claude or ChatGPT assistant:

- "How did my Meta campaigns perform last 30 days vs prior 30?"
- "Which ads should I iterate on and which should I cut?"
- "Where should I shift budget this week?"
- "Analyze my Google Ads ROAS by campaign"
- "Give me creative recommendations for my top performers"

The connector will access your Agency AI account data and provide real-time analysis, strategy recommendations, and execution capabilities.

## Troubleshooting

**Connector not appearing:** Make sure you've completed all authentication steps and that you're signed into your Agency AI account.

**Authentication failed:** Verify your Agency AI account credentials at https://agencyai.app. If you don't have an account, create one first.

**Connection errors:** Check that the endpoint URL is exactly: `https://s.agencyai.app/mcp`

**Need help?** Contact support at hello@agencyai.app

## Transport Type

This MCP uses HTTP transport. No additional configuration is needed beyond the endpoint URL and authentication.
