# Shotlist Maker

A lightweight, responsive shot-list planner for film and video production. Organize scenes and individual shots, classify each shot as **Still** or **Moving**, combine location and movement filters, and export a production-ready PDF.

## Features

- **Scene planning** — record a scene title, location, time of day, and its shots.
- **Shot details** — keep framing/type (for example, CU, WS, or POV) separate from shot movement (**Still** or **Moving**), with a reusable action/description field and completion tracking.
- **Composable filters** — search scenes and shots, filter by completion status or location, and switch Still and Moving filters on independently. Turn both movement switches off to include every movement category; turn both on to include all classified Still and Moving shots. These filters combine, so you can, for example, view Moving shots at one location.
- **Location sorting** — sort scenes by location and restore the original order.
- **PDF export** — export only the scenes and shots currently matching the filters. PDFs include the project name and date, active filter summary, scene/location details, and the Still/Moving movement for each shot. Location-sorted exports can be downloaded per location or as one combined PDF.
- **Project backup and restore** — import and export `.slp` project files (JSON format).
- **Automatic local saving** — projects are saved in the browser, with optional Google Drive sync.
- **Responsive interface** — designed for desktop and mobile, with light and dark themes.

## Quick start

There is no build step or package installation. Serve the repository root with any static web server. For example, with Python:

```bash
python3 -m http.server 8000
```

Then open <http://localhost:8000> in a browser. You can also deploy the repository root directly to GitHub Pages or another static host.

> Google sign-in and Drive features require a browser origin configured for the OAuth client. PDF generation and Google integration load their libraries from third-party CDNs, so those features need an internet connection.

## Using the shot movement field and filters

1. Add a scene and enter its location in **Location Details**.
2. Add shots. Use **Framing / Type** for a size or composition such as `CU` or `OTS`, then choose **Still**, **Moving**, or **Not set** in **Shot Movement**.
3. Use **All locations** to select a location and switch on Still and/or Moving. The location, movement, completion, and text-search filters work together.
4. Export a PDF to preview and download the filtered list. Each scene heading summarizes its included movement categories, and each shot row includes its own movement value.

Active filters are cleared when you add a new scene or shot, so the new item cannot disappear behind the current filter.

Existing project files remain importable. Shots created before the movement field was introduced are kept intact and appear as **Not set** until classified. Movement filters include only shots explicitly marked Still or Moving.

## Data and privacy

- The active project and settings are stored in the current browser's `localStorage`; they are not automatically uploaded to a server.
- Use **Settings → Data → Export JSON** to keep a portable project backup. Importing a project normalizes older shot records without removing their existing fields.
- Google Drive is optional. When used, the app requests access through Google Identity Services and stores the project in the signed-in user's Drive.
- PDF and Google integrations use external libraries/services. See the script and stylesheet references in `index.html` for their providers.

## Repository structure

```text
.
├── index.html   # Complete static application (HTML, CSS, and JavaScript)
├── README.md    # Project documentation
└── LICENSE      # Apache License 2.0
```

## Development and verification

The app is intentionally dependency-light and has no compilation step. Check the inline JavaScript syntax with Node.js:

```bash
python3 - <<'PY' | node --check
from pathlib import Path
import re
html = Path("index.html").read_text(encoding="utf-8")
scripts = re.findall(r"<script(?:\s[^>]*)?>(.*?)</script\s*>", html, re.S | re.I)
print(scripts[-1])
PY
```

Before submitting a change, also manually check the main flows in a browser:

- create, edit, duplicate, and delete scenes/shots;
- combine location, completion, search, and movement filters;
- import an older `.slp` project and verify its shots remain editable;
- preview and download a PDF with a movement filter active, both in normal and location-sorted mode;
- check the filter layout at desktop and mobile widths.

The project is distributed under the terms of the [Apache License 2.0](LICENSE).
