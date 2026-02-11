# Example: Creating a Login + Dashboard Demo

This shows the complete sequence of commands to create a demo that walks through logging in and exploring a dashboard.

## Setup

```bash
# Ensure the app is running at localhost:3000
# Start rodney's browser
rodney start
rodney open http://localhost:3000

# Create image directory
mkdir -p login-demo-images

# Initialize the document
showboat init login-demo.md "App Login and Dashboard Tour"
```

## Step 1: Show the Login Page

```bash
# Add narrative
showboat note login-demo.md "## Step 1: Sign In

Navigate to the app and sign in with your credentials."

# Screenshot the login page
showboat image login-demo.md "rodney screenshot -w 1280 -h 720 login-demo-images/01-login.png"
```

## Step 2: Fill and Submit Login Form

```bash
# Perform the login
showboat exec login-demo.md bash "rodney input 'input#email' 'demo@example.com' && rodney input 'input#password' 'demo123' && rodney click 'button[type=\"submit\"]' && rodney waitstable && rodney sleep 2 && echo 'Signed in successfully'"

# Check the output printed to stdout - it should show:
#   Typed: demo@example.com
#   Typed: demo123
#   Clicked
#   DOM stable
#   Signed in successfully

# Add narrative about result
showboat note login-demo.md "After signing in, we land on the main dashboard."

# Screenshot the dashboard
showboat image login-demo.md "rodney screenshot -w 1280 -h 720 login-demo-images/02-dashboard.png"
```

## Step 3: Navigate to a Feature

```bash
# Add narrative
showboat note login-demo.md "## Step 2: Explore Reports

Click **Reports** in the sidebar to view available reports."

# Click and screenshot
showboat exec login-demo.md bash "rodney click 'a[href=\"/reports\"]' && rodney waitstable && rodney sleep 1 && echo 'Navigated to Reports'"

showboat image login-demo.md "rodney screenshot -w 1280 -h 720 login-demo-images/03-reports.png"
```

## Handling Errors

If a step goes wrong:

```bash
# The click failed - wrong selector
showboat exec login-demo.md bash "rodney click '.wrong-selector' && echo 'Clicked'"
# Output shows an error...

# Remove the failed entry
showboat pop login-demo.md

# Try again with the correct selector
showboat exec login-demo.md bash "rodney click '.correct-selector' && rodney waitstable && echo 'Clicked'"
```

## Adding a Summary

```bash
showboat note login-demo.md "---

## Summary

In this demo we:

1. **Signed in** with email and password
2. **Explored the dashboard** with key metrics
3. **Navigated to Reports** to view analytics

The app provides an intuitive interface for data exploration."
```

## Verify

```bash
# Verify all code blocks still produce the same output
showboat verify login-demo.md
```

## Key Patterns Used

1. **Screenshot before action** - Capture the starting state first
2. **Chain rodney commands** - Use `&&` to chain related actions
3. **Echo at end of chains** - Provides readable success marker
4. **waitstable after clicks** - Ensures page has settled
5. **sleep for async content** - Extra delay for API responses
6. **Pop on failure** - Remove bad entries and retry
7. **Numbered image files** - Maintain clear ordering
