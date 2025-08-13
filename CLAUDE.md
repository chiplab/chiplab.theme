# ChipLab Theme Repository

## Repository Overview
This is the standalone Shopify theme repository for ChipLab, separated from the main designer-konva application repository. The theme is based on Horizon 2.0 and contains all the Liquid templates, assets, and configurations needed for the ChipLab storefront.

## Repository Structure

### Why This Separation?
Previously, the theme was bundled with the designer-konva app repository. We separated them to:
- Enable independent deployment pipelines for theme and app
- Allow theme updates without affecting the app and vice versa
- Simplify CI/CD workflows for both components
- Make theme development more accessible to frontend developers who don't need the full app context

### Current Setup
- **This Repository (chiplab.theme)**: Contains only the Shopify theme files
- **Sister Repository (designer-konva)**: Contains the Designer app with theme extensions
- **Development Store**: printlabs-app-dev.myshopify.com
- **Theme ID**: 178482970919

## Development Workflow

### Local Development
```bash
# Start the theme development server
shopify theme dev --store=printlabs-app-dev.myshopify.com

# The dev server will:
# - Sync changes in real-time to the dev store
# - Provide a local preview URL
# - Hot reload on file changes
```

### Pushing Changes to Dev Store
```bash
# Push all theme files to dev store
shopify theme push --store=printlabs-app-dev.myshopify.com

# Push specific files only
shopify theme push --store=printlabs-app-dev.myshopify.com --only templates/product.json

# Pull latest changes from dev store
shopify theme pull --store=printlabs-app-dev.myshopify.com
```

### Testing Theme Changes
1. Always test on the dev store first
2. Use the theme preview link to test without affecting live customers
3. Check responsive design across devices
4. Test with actual product data and checkout flow

## Deployment Pipeline

### Branch Strategy
- **`main` branch**: Deploys to production store automatically
- **`dev` branch**: Deploys to development store automatically
- **Feature branches**: For local development only

### Automatic Production Deployment
- **Trigger**: Push or merge to `main` branch
- **Action**: GitHub Actions workflow automatically deploys to live store
- **Configuration**: `.github/workflows/deploy-theme.yml`

### Automatic Development Deployment
- **Trigger**: Push or merge to `dev` branch
- **Action**: GitHub Actions workflow automatically deploys to dev store
- **Configuration**: `.github/workflows/deploy-dev.yml`

### Manual Production Deployment
```bash
# One-time setup: Push theme to live store
shopify theme push --store=[live-store].myshopify.com --unpublished

# Note the theme ID from the output for GitHub secrets

# Emergency manual deploy (if CI/CD fails)
shopify theme push --store=[live-store].myshopify.com --allow-live --theme=[THEME_ID]
```

## GitHub Configuration

### Required Secrets for Production
Set these in GitHub repository settings → Secrets and variables → Actions:
- `SHOPIFY_ACCESS_TOKEN`: Custom app admin API token from production store
- `LIVE_STORE_URL`: Production store URL (e.g., chiplab.myshopify.com)
- `LIVE_THEME_ID`: Theme ID on production store

### Required Secrets for Development
- `DEV_SHOPIFY_ACCESS_TOKEN`: Custom app admin API token from dev store
- `DEV_STORE_URL`: Development store URL (printlabs-app-dev.myshopify.com)
- `DEV_THEME_ID`: Theme ID on dev store (178482970919)

### Creating Custom App Tokens (For Store Owner/Partners)
Since you're both the Store Owner and Partner, use custom apps instead of Partner tokens:

1. Go to your store's admin (both production and dev stores)
2. Navigate to Settings → Apps and sales channels → Develop apps
3. Create a new app (e.g., "Theme Deployment")
4. Configure Admin API scopes:
   - `read_themes`
   - `write_themes`
5. Install the app
6. Copy the Admin API access token for GitHub secrets

## Theme Architecture

### Directory Structure
```
theme/
├── assets/          # CSS, JS, images, SVG icons
├── blocks/          # Reusable theme blocks (used in sections)
├── config/          # Theme settings and data
├── layout/          # Main layout templates
├── locales/         # Translation files
├── sections/        # Page sections (customizable in theme editor)
├── snippets/        # Reusable Liquid code snippets
└── templates/       # Page-specific templates
```

### Key Files
- `layout/theme.liquid`: Main layout wrapper for all pages
- `config/settings_data.json`: Theme customization settings
- `config/settings_schema.json`: Theme settings structure
- `assets/base.css`: Core styles
- `assets/global.js`: Main JavaScript functionality

### JavaScript Architecture
- Modern ES6+ modules with TypeScript definitions
- Component-based architecture (see `assets/component.js`)
- Event-driven system for inter-component communication
- Performance optimized with lazy loading

## Common Tasks

### Adding a New Section
1. Create file in `sections/` directory
2. Define section schema at bottom of file
3. Add to relevant template or make it dynamic
4. Test in theme customizer

### Modifying Styles
1. Main styles are in `assets/base.css`
2. Component-specific styles are in their respective JS files
3. Use CSS custom properties defined in theme settings
4. Test across different color schemes

### Working with Product Templates
1. Product templates are in `templates/product.json`
2. Sections are defined in `sections/product-information.liquid`
3. Product card component in `snippets/product-card.liquid`
4. Gallery handled by `assets/media-gallery.js`

### Updating Navigation
1. Menus are managed in Shopify admin, not in code
2. Header menu logic in `sections/header.liquid`
3. Mobile menu in `snippets/header-drawer.liquid`
4. Mega menu support in `snippets/mega-menu.liquid`

## Integration with Designer App

### Relationship
- This theme runs independently of the designer-konva app
- The app adds functionality through theme app extensions
- Both can be developed and deployed separately

### Development Coordination
When working on features that span both:
1. Develop theme changes in this repository
2. Develop app changes in designer-konva repository
3. Test integration on dev store
4. Deploy theme first, then app (or vice versa if independent)

## Best Practices

### Code Quality
- Follow existing Liquid patterns and conventions
- Use semantic HTML and accessibility best practices
- Optimize images before adding to assets
- Keep JavaScript modular and performant

### Version Control
- Make atomic commits with clear messages
- Create feature branches for new work
- Use pull requests for code review
- Tag releases for major theme updates

### Performance
- Lazy load images and non-critical resources
- Minimize render-blocking resources
- Use Shopify's CDN for all assets
- Implement critical CSS inline when necessary

### Testing Checklist
- [ ] Desktop and mobile responsive design
- [ ] Cross-browser compatibility
- [ ] Theme editor customization options work
- [ ] Cart and checkout flow functional
- [ ] Search and filtering work correctly
- [ ] Accessibility standards met
- [ ] Performance metrics acceptable

## Troubleshooting

### Common Issues

**Theme won't push**
- Check authentication: `shopify auth logout` then login again
- Verify store URL and theme ID in `.shopify-cli.yml`
- Ensure you have proper permissions on the store

**Changes not appearing**
- Clear browser cache
- Check if viewing the correct theme (might be viewing a different theme preview)
- Verify files were actually saved and pushed

**CI/CD Pipeline Fails**
- Check GitHub Actions logs for specific errors
- Verify all secrets are correctly set
- Ensure theme ID exists on production store
- Check Shopify API rate limits

## Quick Commands Reference

```bash
# Development
shopify theme dev                    # Start dev server
shopify theme push                   # Push to dev store
shopify theme pull                   # Pull from dev store
shopify theme list                   # List all themes

# Debugging
shopify theme check                  # Run theme linter
shopify theme console                # Interactive Liquid console

# Git Workflow
git checkout -b feature/new-feature  # Create feature branch from dev
git add .                            # Stage changes
git commit -m "message"              # Commit changes
git push origin feature/new-feature  # Push feature branch
git checkout dev                     # Switch to dev branch
git merge feature/new-feature        # Merge feature to dev (auto-deploys to dev store)
git checkout main                    # Switch to main branch
git merge dev                        # Merge dev to main (auto-deploys to production)
```

## Support and Resources

### Documentation
- [Shopify Theme Development](https://shopify.dev/themes)
- [Liquid Reference](https://shopify.dev/docs/api/liquid)
- [Theme Architecture](https://shopify.dev/docs/themes/architecture)

### Internal Resources
- Dev Store: printlabs-app-dev.myshopify.com
- Main App Repository: designer-konva
- Production Store: [To be configured]

## Notes for Future Development

### Planned Improvements
- Implement theme versioning strategy
- Add automated testing for critical paths
- Optimize bundle sizes further
- Enhance accessibility features

### Known Limitations
- Theme customizer has limits on section nesting
- Some features require app integration
- Performance impacts with too many sections

---

*Last Updated: Initial repository creation - Separated from designer-konva repository*