# CLAUDE.md - AI Assistant Guide for CV Repository

## Repository Overview

This repository is a **CV/Resume Generator** for Iosif Vigh using the [JSON Resume](https://jsonresume.org/) standard. It automates the generation and deployment of HTML and PDF versions of a professional CV from a structured JSON data file.

**Key Purpose**: Maintain a single source of truth (`resume.json`) that automatically generates multiple output formats (HTML, PDF) and publishes them to GitHub Pages and GitHub Gist.

## Repository Structure

```
cv/
├── resume.json                           # Main CV data (JSON Resume schema)
├── resume-2023-backup.json               # Backup of previous version
├── package.json                          # Node.js dependencies & scripts
├── test.sh                               # Quick preview generation script
├── index.html                            # Landing page with links to CV
├── dl.html                               # Legacy download page
├── README.md                             # Repository documentation
├── .github/
│   └── workflows/
│       └── generate-and-sync.yml         # CI/CD automation workflow
└── themes/
    └── my-stackoverflow/                 # Custom Handlebars theme
        ├── index.js                      # Theme entry point & config
        ├── package.json                  # Theme dependencies
        ├── style.css                     # Theme styling
        ├── resume.hbs                    # Main Handlebars template
        ├── theme/
        │   ├── hbs-helpers/              # Custom Handlebars helpers
        │   │   ├── birth-date.js
        │   │   ├── date-helpers.js
        │   │   ├── paragraph-split.js
        │   │   ├── space-to-dash.js
        │   │   └── to-lower-case.js
        │   └── partials/                 # Handlebars partial templates
        │       ├── awards.hbs
        │       ├── basics.hbs
        │       ├── certificates.hbs
        │       ├── education.hbs
        │       ├── interests.hbs
        │       ├── languages.hbs
        │       ├── projects.hbs
        │       ├── publications.hbs
        │       ├── references.hbs
        │       ├── skills.hbs
        │       ├── volunteer.hbs
        │       └── work.hbs
        └── public/
            └── index.html
```

## Core Files and Their Roles

### 1. `resume.json` (CRITICAL)
- **The single source of truth** for all CV content
- Follows [JSON Resume Schema](https://jsonresume.org/schema/)
- Contains sections: basics, work, skills, education, projects, etc.
- **IMPORTANT**: Any CV content changes MUST be made here
- Located at: `/home/user/cv/resume.json`

### 2. `themes/my-stackoverflow/`
Custom theme based on [jsonresume-theme-stackoverflow](https://github.com/francescoes/jsonresume-theme-stackoverflow):
- **index.js**: Theme renderer with Handlebars setup and PDF configuration
- **resume.hbs**: Main template that assembles all partials
- **style.css**: Complete styling (Stack Overflow-inspired design)
- **theme/partials/**: Modular templates for each resume section
- **theme/hbs-helpers/**: Custom Handlebars helpers for formatting

### 3. GitHub Actions Workflow
Location: `.github/workflows/generate-and-sync.yml`

**Triggers**: Push to `main` branch

**Steps**:
1. Install dependencies (`npm install`)
2. Generate HTML and PDF using resume-cli
3. Deploy `resume.json` to GitHub Gist (ID: `bdfc617628bc7a2fc8763a2be6b1a816`)
4. Deploy PDF to GitHub Gist
5. Commit generated files to `generated-page` branch:
   - `docs/view.html` - Generated CV HTML
   - `docs/cv.pdf` - Generated CV PDF
   - `docs/Iosif_Vigh-Senior_Software_Engineer.pdf` - Named PDF copy
   - `docs/index.html` - Copy of main index.html
   - `docs/dl.html` - Legacy download page

**Required Secrets**:
- `GHA_TOKEN_FOR_GISTS` - Token for deploying to gists
- `GHA_TOKEN_FOR_PUSH_EXPIRES_FEB_2026` - Token for pushing to generated-page branch

### 4. `package.json`
**Dependencies**:
- `resume-cli` (v3.0.7) - JSON Resume CLI tool
- `jsonresume-theme-macchiato` (1.1)
- `jsonresume-theme-stackoverflow` (2.1)

**NPM Scripts**:
- `preview`: Generate HTML and PDF locally using custom theme
- `preview-original`: Generate using original stackoverflow theme

### 5. `index.html`
- Landing page served at the root of GitHub Pages
- Contains links to view CV online, download PDF, schedule meetings, contact info
- Has dark mode support via `prefers-color-scheme`
- Includes Beam Analytics tracking

## Development Workflow

### Making Changes to CV Content

1. **Edit `resume.json`** on the `main` branch
   - This is the ONLY file that should be edited for content changes
   - Follow JSON Resume schema: https://jsonresume.org/schema/

2. **Test Locally** (optional):
   ```bash
   npm install
   npm run preview
   # Or use the quick script:
   ./test.sh
   ```
   Generated files will be in `exports/` directory

3. **Commit and Push to `main`**:
   ```bash
   git add resume.json
   git commit -m "Updated [section]: [brief description]"
   git push origin main
   ```

4. **Automated Pipeline Runs**:
   - GitHub Actions generates HTML/PDF
   - Updates GitHub Gist
   - Pushes to `generated-page` branch
   - GitHub Pages auto-deploys from `generated-page` branch

### Making Theme Changes

**Files to modify**:
- `themes/my-stackoverflow/style.css` - Styling changes
- `themes/my-stackoverflow/resume.hbs` - Template structure
- `themes/my-stackoverflow/theme/partials/*.hbs` - Section-specific templates
- `themes/my-stackoverflow/theme/hbs-helpers/*.js` - Custom formatting logic
- `themes/my-stackoverflow/index.js` - PDF configuration or helper registration

**Test locally**:
```bash
npm run preview
```

**Commit theme changes** to `main` branch - the pipeline will use the updated theme.

### Git Branch Strategy

- **`main`**: Source branch (resume.json, theme, workflows)
  - All development happens here
  - Triggers the build pipeline on push

- **`generated-page`**: Auto-generated output branch
  - Contains `docs/` directory with generated files
  - GitHub Pages serves from this branch
  - **DO NOT manually edit this branch**

- **Feature branches**: Use format `claude/claude-md-[session-id]`
  - For AI assistant development work
  - Create PRs to merge into `main`

## Key Conventions and Standards

### JSON Resume Schema Compliance

The `resume.json` file MUST follow the standard schema. Key sections:

```json
{
  "meta": { "lastModified": "YYYY-MM-DD", "theme": "stackoverflow" },
  "basics": { "name", "label", "email", "phone", "website", "summary", "location", "profiles" },
  "work": [{ "company", "position", "startDate", "endDate", "summary", "highlights", "keywords" }],
  "skills": [{ "name", "keywords" }],
  "education": [{ "institution", "area", "studyType", "startDate", "endDate" }],
  "projects": [{ "name", "description", "highlights", "keywords", "url" }],
  "awards": [{ "title", "date", "awarder", "summary" }],
  "certificates": [{ "name", "date", "issuer", "url" }],
  "publications": [{ "name", "publisher", "releaseDate", "url", "summary" }],
  "volunteer": [{ "organization", "position", "startDate", "endDate", "summary", "highlights" }],
  "languages": [{ "language", "fluency" }],
  "interests": [{ "name", "keywords" }],
  "references": [{ "name", "reference" }]
}
```

**Extended Fields** (supported by stackoverflow theme):
- `keywords` in work, publication, volunteer items
- `summary` in interests and education items
- `birth` object in basics (place, state, date)

### Code Style and Formatting

**JavaScript (Theme Files)**:
- Use `const` for requires and immutable values
- Use CommonJS modules (`module.exports`, `require()`)
- File naming: kebab-case (e.g., `space-to-dash.js`)
- Handlebars helpers should be pure functions

**Handlebars Templates**:
- Use partials for each resume section
- Keep logic minimal in templates (move to helpers)
- Use semantic HTML5 elements
- Follow existing indentation (2 spaces)

**CSS**:
- Follow existing naming conventions from stackoverflow theme
- Use CSS custom properties for theming
- Keep print-specific styles separate

### Date Formatting

The theme provides custom date helpers in `theme/hbs-helpers/date-helpers.js`:
- `MY` - Month Year format
- `Y` - Year only
- `DMY` - Day Month Year format

Use these in templates: `{{MY startDate}}`

### Theme Versioning Note

**CRITICAL**: `package.json` pins `jsonresume-theme-stackoverflow` to version `2.0`, NOT `2.1.0`.

**Reason**: Version 2.1.0 has a bug that doesn't show company names. This is documented in README.md:60-62.

## Testing and Validation

### Local Testing

```bash
# Install dependencies
npm install

# Generate preview (custom theme)
npm run preview
# Output: exports/so-preview.html, exports/so-preview.pdf

# Generate with original stackoverflow theme
npm run preview-original
# Output: exports/so-original.html, exports/so-original.pdf

# Quick test with shell script
./test.sh
```

### Validate JSON Resume Schema

Use the official schema validator:
- Schema: https://github.com/jsonresume/resume-schema/blob/master/schema.json
- Sample: https://github.com/jsonresume/resume-schema/blob/master/sample.resume.json

### CI/CD Testing

After pushing to `main`:
1. Check GitHub Actions status: https://github.com/iosifv/cv/actions
2. Verify gist updated: https://gist.github.com/iosifv/bdfc617628bc7a2fc8763a2be6b1a816
3. Check GitHub Pages deployment
4. Test live URLs (listed in README.md)

## Published URLs

The CV is published at multiple locations:

**GitHub Pages**:
- https://iosifv.github.io/cv - Landing page
- https://iosifv.github.io/cv/view.html - HTML CV
- https://iosifv.github.io/cv/cv.pdf - PDF download
- https://iosifv.github.io/cv/Iosif_Vigh-Senior_Software_Engineer.pdf - Named PDF

**Custom Domain** (CNAME configured):
- https://cv.iosifv.com/ - Landing page
- https://cv.iosifv.com/view - HTML CV
- https://cv.iosifv.com/cv.pdf - PDF download
- https://cv.iosifv.com/Iosif_Vigh-Senior_Software_Engineer.pdf - Named PDF

**JSON Resume Registry** (from gist):
- https://registry.jsonresume.org/iosifv?theme=caffeine
- https://registry.jsonresume.org/iosifv?theme=stackoverflow

**GitHub Gist**:
- https://gist.github.com/iosifv/bdfc617628bc7a2fc8763a2be6b1a816

## Common Tasks for AI Assistants

### Adding a New Work Experience

1. Read current `resume.json`: `/home/user/cv/resume.json`
2. Add new entry to `work` array (newest first - reverse chronological)
3. Include: company, position, website, startDate, endDate, summary, highlights, keywords
4. Use ISO date format: `YYYY-MM-DD`
5. Commit with message: `Updated work experience: added [company]`

### Updating Skills

1. Edit `skills` array in `resume.json`
2. Group by category (name field): Languages, Backend, Frontend, Cloud, etc.
3. List specific technologies in `keywords` array
4. Order by proficiency/relevance (most important first)

### Modifying Theme Styling

1. Edit `themes/my-stackoverflow/style.css`
2. Test locally with `npm run preview`
3. Check both HTML output and PDF output (PDF uses Puppeteer)
4. Consider print styles - PDF generation respects print media queries
5. Margin configuration is in `themes/my-stackoverflow/index.js:44-52`

### Adding New Handlebars Helper

1. Create new file in `themes/my-stackoverflow/theme/hbs-helpers/`
2. Export function with helper logic
3. Import and register in `themes/my-stackoverflow/index.js`
4. Use in templates: `{{helperName value}}`

### Troubleshooting Build Failures

**GitHub Actions fails with authentication error**:
- Check token expiration (noted in README.md)
- Update `GHA_TOKEN_FOR_PUSH_EXPIRES_FEB_2026` secret

**PDF generation fails**:
- Ensure `RESUME_PUPPETEER_NO_SANDBOX: 1` env var is set (workflow:30)
- Check Puppeteer compatibility with Node.js version (currently 20)

**Theme not rendering correctly**:
- Verify all Handlebars partials are registered (index.js:26-36)
- Check for syntax errors in .hbs files
- Ensure all helpers are registered before compile

## Dependencies and Tools

### Core Dependencies

- **resume-cli** (3.0.7): Official JSON Resume CLI
  - Commands: `export`, `serve`, `validate`
  - Used for generating HTML and PDF

- **Handlebars**: Template engine for theme rendering
- **Puppeteer**: Headless browser for PDF generation (indirect dependency)

### Font Awesome Integration

The theme supports [Font Awesome brand icons](https://fontawesome.com/search?s=brands) for social profiles.

**Supported networks with brand colors** (see themes/my-stackoverflow/README.md:43-49):
github, stack-overflow, linkedin, dribbble, twitter, facebook, pinterest, instagram, soundcloud, wordpress, youtube, flickr, google plus, tumblr, foursquare

Network names are:
- Converted to lowercase
- Spaces replaced with dashes
- Matched to Font Awesome icon names

Example: "Stack Overflow" → "stack-overflow" icon

## Environment Notes

- **Node.js Version**: 20 (specified in GitHub Actions)
- **OS**: Ubuntu 24.04 for CI/CD
- **Build Tool**: npm (no yarn or other package managers)
- **Git Config**: Bot user for automated commits

## Important Warnings

### DO NOT:
1. ❌ Edit files in `generated-page` branch manually
2. ❌ Upgrade `jsonresume-theme-stackoverflow` to 2.1.0 (company names bug)
3. ❌ Push directly to `generated-page` branch
4. ❌ Edit HTML/PDF output files (edit resume.json instead)
5. ❌ Skip testing locally before pushing theme changes
6. ❌ Use `resume-cli` v4.x (not tested, use 3.0.7)

### DO:
1. ✅ Edit `resume.json` for all content changes
2. ✅ Test locally with `npm run preview` before pushing
3. ✅ Keep theme version pinned to 2.0
4. ✅ Follow JSON Resume schema strictly
5. ✅ Check GitHub Actions status after pushing
6. ✅ Verify published URLs after deployment
7. ✅ Use ISO date formats (YYYY-MM-DD)
8. ✅ Maintain reverse chronological order (newest first)

## Quick Reference Commands

```bash
# Local preview generation
npm install
npm run preview

# Test script
./test.sh

# Resume CLI direct usage
npx resume-cli export output.html --resume resume.json --theme ./themes/my-stackoverflow
npx resume-cli export output.pdf --resume resume.json --theme ./themes/my-stackoverflow

# Validate resume.json (if validator installed)
resume validate resume.json

# Git workflow
git status
git add resume.json
git commit -m "Updated [section]: [description]"
git push origin main

# View GitHub Actions logs
# Visit: https://github.com/iosifv/cv/actions
```

## Additional Resources

- JSON Resume Schema: https://jsonresume.org/schema/
- Resume Schema Samples: https://github.com/jsonresume/resume-schema/blob/master/sample.resume.json
- Original Theme: https://www.npmjs.com/package/jsonresume-theme-stackoverflow
- Font Awesome Icons: https://fontawesome.com/search?o=r&ip=brands
- GitHub Actions Used:
  - https://github.com/marketplace/actions/jsonresume-convert
  - https://github.com/marketplace/actions/deploy-to-gist
  - https://github.com/ad-m/github-push-action

## Contact Information

**Repository Owner**: Iosif Vigh
- Email: hire-me@iosifv.com
- Phone: (+44) 759 713 7739
- Website: https://iosifv.com
- GitHub: https://github.com/iosifv
- LinkedIn: https://www.linkedin.com/in/iosifv/

---

*Last Updated: 2025-11-18*
*Repository: https://github.com/iosifv/cv*
