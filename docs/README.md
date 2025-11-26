# Event Management Handbook - GitHub Pages

This directory contains the GitHub Pages site for the Event Management Handbook.

## Site Structure

- `index.md` - Homepage
- `chapters.md` - Chapters overview page
- `_chapters/` - All 19 book chapters in Jekyll format
- `_config.yml` - Jekyll configuration
- `Gemfile` - Ruby dependencies

## Local Development

To run the site locally:

```bash
cd docs
bundle install
bundle exec jekyll serve
```

Then visit `http://localhost:4000/EventManagementHandbook/`

## Deployment

This site is configured to deploy to GitHub Pages automatically when pushed to the main branch.

### Enable GitHub Pages

1. Go to repository Settings
2. Navigate to Pages section
3. Set Source to "Deploy from a branch"
4. Set Branch to "main" and folder to "/docs"
5. Click Save

The site will be available at: `https://zettai-seigi.github.io/EventManagementHandbook/`

## Theme

This site uses the "Just the Docs" theme for optimal documentation presentation:
- Clean, professional design
- Built-in search functionality
- Mobile-responsive
- Easy navigation
- Syntax highlighting

## Content Updates

To update content:

1. Edit files in `_chapters/` or root pages
2. Commit and push changes
3. GitHub Pages will automatically rebuild and deploy

## License

Content distributed under MIT License. See main repository LICENSE file.
