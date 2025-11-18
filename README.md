# Appose Workshop Slides

Interactive slides for the I2K 2025 Appose workshop, built with [Slidev](https://sli.dev/).

## Local Development

Install dependencies:

```bash
npm install
```

Start the dev server with hot reload:

```bash
npm run dev
```

This will open the presentation in your browser at http://localhost:3030

## Build for Production

Build static files for deployment:

```bash
npm run build
```

The built files will be in the `dist` folder, ready for deployment.

## Export to PDF

Export slides to PDF:

```bash
npm run export
```

## Presenter Mode

When running locally, press `?` to see keyboard shortcuts, or click the presenter icon to enter presenter mode with:
- Speaker notes
- Next slide preview
- Timer

## Key Features

- **Interactive code blocks** - Click to highlight different lines
- **v-click animations** - Progressive content reveal
- **Two-column layouts** - Side-by-side comparisons
- **Syntax highlighting** - For Bash, Python, Groovy, Java
- **Offline support** - Works without internet after initial setup

## Deployment

### GitHub Pages

This repository is configured to automatically deploy to GitHub Pages via GitHub Actions whenever you push to the `main` branch.

The slides will be available at: `https://[username].github.io/[repo-name]/`

To enable GitHub Pages:
1. Go to repository Settings > Pages
2. Set Source to "GitHub Actions"
3. Push to main branch - deployment happens automatically

### Manual Deployment

Build and deploy to any static hosting:

```bash
npm run build
# Upload the dist/ folder to your host
```

## Customization

Edit `slides.md` to modify the presentation content. Slidev uses Markdown with special directives:

- `---` separates slides
- `layout: name` sets slide layout
- `<v-click>` adds click-to-reveal
- `<v-clicks>` adds multiple reveals
- Code blocks with `{1|2|3}` for line highlighting

See [Slidev documentation](https://sli.dev/) for more features.
