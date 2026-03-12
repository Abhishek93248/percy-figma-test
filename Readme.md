# Percy Visual Testing Implementation Guide

This repository demonstrates how to perform **automated visual regression testing using Percy CLI**.

The goal is to capture UI snapshots and compare them against **design references (such as Figma exports)** to ensure visual consistency between design and implementation.

---

# Percy Visual Testing Demo

Percy CLI allows teams to:

- Capture **full-page visual snapshots**
- Capture **specific UI components**
- Maintain **design consistency with Figma**
- Run visual tests using **simple YAML configuration**

This approach avoids writing complex automation scripts while still achieving strong visual coverage.

---

# 1. Prerequisites

Install Percy CLI in your project directory:

```bash
npm install --save-dev @percy/cli
```

Set the Percy token for authentication.  
You can find the token in your **Percy project settings**.

```bash
export PERCY_TOKEN=your_project_token_here
```

---

# 2. Branch Management

To keep snapshots organized across feature branches or testing environments, use the `PERCY_BRANCH` environment variable.

this gives each branch a name

Example:

```bash
export PERCY_BRANCH=customer-review-v1
```

---

# 3. Running Snapshots (YAML Configuration)

Snapshots are defined in a `snapshots.yml` file.  
This file specifies the **URLs and optional selectors** for the elements you want Percy to capture.

Run the snapshots using:

```bash
npx percy snapshot snapshots.yml
```

---

# 4. Targeted Component Testing

Percy allows you to capture **specific UI components instead of the full page** by using the `scope` attribute with a CSS selector.

Example components from the BrowserStack demo site:

| Component | CSS Selector |
|----------|-------------|
| Galaxy S20 Card | `.shelf-item[id='10']` |
| Apple Filter Checkbox | `.checkmark` |

This ensures Percy captures **only the relevant UI element**, making visual comparisons more focused and meaningful.

---

# 5. Design Comparison with Figma

To compare live UI with design references:

1. Export the required component or screen from **Figma export**.


Matching snapshot names allows Percy to align the **design reference with the live UI snapshot** during review.

### Example

Figma export filename:

```
BStack - Galaxy S20 Component.png
```

Matching YAML entry:

```yaml
name: "BStack - Galaxy S20 Component"
```

Maintaining the same name ensures **consistent comparison between the Figma design and the Percy snapshot**.

---

# 6. File Structure

| File | Purpose |
|-----|------|
| `snapshots.yml` | Defines URLs and CSS selectors for visual testing |
| `snapshots.json` | Alternative JSON format for defining snapshots |
| `.percy.yml` | Global Percy configuration such as browser widths or CSS overrides |

---

# Example `snapshots.yml`

```yaml
# Capture the specific Galaxy S20 product card
- name: "BStack - Galaxy S20 Component"
  url: "https://bstackdemo.com/"
  scope: ".shelf-item[id='10']"

# Capture the Apple vendor filter checkbox
- name: "BStack - Apple Filter Checkmark"
  url: "https://bstackdemo.com/"
  scope: ".checkmark"

# Full page snapshot
- name: "screenshot1"
  url: "https://miro.com/"
  waitForTimeout: 10000
```
# 6. Uploading  Images

Percy allows uploading static images which can then be visually compared with snapshots.

### Command

```bash
npx percy upload <dirname>
```

### Arguments

| Argument | Description |
|--------|-------------|
| `<dirname>` | Directory containing images to upload |

### Options

| Option | Description |
|------|-------------|
| `-f, --files [pattern]` | One or more glob patterns matching image files to upload (default: `**/*.{png,jpg,jpeg}`) |
| `-i, --ignore <pattern>` | One or more glob patterns matching image files to ignore |
| `-e, --strip-extensions` | Removes file extensions from snapshot names |
