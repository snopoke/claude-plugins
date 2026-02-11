---
name: demo-creation
description: This skill teaches Claude how to create executable demo documents using the showboat CLI (document assembly) and rodney CLI (browser automation). Use when the user asks to "create a demo", "make a walkthrough", "document a workflow with screenshots", "use showboat", "use rodney", "create a product demo", or "record a demo".
---

# Creating Demos with Showboat and Rodney

Create executable demo documents that combine narrative text, browser automation, and screenshots into reproducible markdown walkthroughs.

## Tools Overview

**showboat** assembles the markdown document. It manages the file, runs code blocks, captures output, and embeds images.

**rodney** automates Chrome. It clicks, types, navigates, waits, and takes screenshots.

Together: showboat structures the document, rodney drives the browser, and `showboat image` captures screenshots via rodney.

## Workflow

### 1. Plan the Demo

Before writing any commands, outline the demo steps:
- What pages/screens need to be shown?
- What user actions need to happen (login, form fill, navigation)?
- What screenshots should be captured and when?
- What is the narrative arc? (setup, action, result)

### 2. Prerequisites

Ensure the app is running and accessible. Start rodney's browser if needed:

```bash
rodney start
rodney open http://localhost:5173
```

Create an images directory for the demo:

```bash
mkdir -p demo-images
```

### 3. Initialize the Document

```bash
showboat init demo.md "Demo Title"
```

### 4. Build the Demo

Alternate between these operations:

**Add narrative text:**
```bash
showboat note demo.md "## Step 1: Sign In

Navigate to the app and sign in with your credentials."
```

**Capture a screenshot (before an action, to show the starting state):**
```bash
showboat image demo.md "rodney screenshot -w 1280 -h 720 demo-images/01-login.png"
```

**Execute browser actions:**
```bash
showboat exec demo.md bash "rodney input 'input#email' 'user@example.com' && rodney input 'input#password' 'password123' && rodney click 'button[type=\"submit\"]' && rodney waitstable && rodney sleep 2 && echo 'Signed in'"
```

**Capture result screenshot:**
```bash
showboat image demo.md "rodney screenshot -w 1280 -h 720 demo-images/02-signed-in.png"
```

### 5. Error Recovery

If a command fails or produces bad output, remove it and retry:

```bash
showboat pop demo.md
```

Then re-run the corrected command. Pop removes the most recent entry (code block + output, or note).

### 6. Verify

After completing the demo, verify all code blocks still produce the same output:

```bash
showboat verify demo.md
```

## Command References

Before starting a demo, run `showboat --help` and `rodney --help` to get the full, up-to-date list of commands and options. For help on a specific command, run `showboat <command> --help` or `rodney <command> --help`.

Key showboat behaviors:
- `exec` prints captured output to stdout and exits with the command's exit code. Read this output to verify the step worked.
- `image` runs a script that should produce an image file. The image reference is appended to the markdown.
- `pop` removes the most recent entry (code block + output pair, or a note block).

## Patterns and Best Practices

### Credentials

Never embed real credentials in demo scripts. Use dummy data or environment variables. Demo documents are typically committed to version control.

### Screenshot Naming

Use numbered prefixes for ordering: `01-login.png`, `02-dashboard.png`, `03-results.png`. Put all images in a dedicated subdirectory: `demo-images/` or `<demo-name>-images/`.

### Screenshot Dimensions

Always specify width and height for consistency: `rodney screenshot -w 1280 -h 720 file.png`

### Reliable Clicking

For buttons identifiable by text content rather than a unique selector, use JavaScript:

```bash
rodney js "Array.from(document.querySelectorAll('button')).find(b => b.textContent.includes('Submit')).click()"
```

### Waiting Strategy

After every interaction that triggers page changes, add waits:

```bash
rodney click 'button' && rodney waitstable && rodney sleep 1
```

Use `waitstable` after clicks/submits. Add `sleep 1-2` for animations or async responses. Use `wait <selector>` when waiting for specific content to appear.

### Chaining Commands

Chain related rodney commands with `&&` in a single `showboat exec`:

```bash
showboat exec demo.md bash "rodney input '#name' 'value' && rodney input '#email' 'email@test.com' && rodney click 'button[type=submit]' && rodney waitstable && echo 'Form submitted'"
```

End chains with an `echo` describing what happened - this serves as both a human-readable marker in the output and a success confirmation.

### Narrative Structure

Structure demos with markdown headers in notes. Each major step gets a `## Step N: Title` header. Add narrative before actions explaining what will happen and after results explaining what was observed.

### Escaping in Selectors

When selectors contain quotes, use the opposite quote style:
- `rodney click 'button[type="submit"]'` (single outside, double inside)
- `rodney js "document.querySelector('input')"` (double outside, single inside)

### Finding Selectors

Use the accessibility tree to discover elements when selectors aren't obvious:

```bash
rodney ax-tree --depth 3
```

Or inspect specific elements:

```bash
rodney html 'form'
```

## Example

See `examples/demo-workflow.md` for a complete annotated example of a demo creation session.
