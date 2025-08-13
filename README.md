# ChipLab Theme

Shopify theme for ChipLab - Horizon 2.0 based theme.

## Development Workflow

### Local Development
```bash
# Start development server
shopify theme dev --store=printlabs-app-dev.myshopify.com

# Push changes to dev store
shopify theme push --store=printlabs-app-dev.myshopify.com
```

### Production Deployment
Changes pushed to `main` branch automatically deploy to live store via GitHub Actions.

### Manual Production Deploy
```bash
shopify theme push --store=[live-store].myshopify.com --allow-live
```

## Theme Structure
- `/theme/assets/` - CSS, JS, images
- `/theme/blocks/` - Theme blocks
- `/theme/config/` - Theme settings
- `/theme/layout/` - Theme layouts
- `/theme/locales/` - Translations
- `/theme/sections/` - Theme sections
- `/theme/snippets/` - Reusable liquid snippets
- `/theme/templates/` - Page templates

## Important Notes
- Always test changes on dev store first
- Production deployments happen automatically on merge to main
- Keep Designer app extensions in the designer-konva repository

## GitHub Secrets Required
Configure these secrets in your GitHub repository settings:
- `SHOPIFY_CLI_THEME_TOKEN` - Theme access token for live store
- `LIVE_STORE_URL` - Your live store URL (e.g., your-store.myshopify.com)
- `LIVE_THEME_ID` - Theme ID on live store (obtain after first manual push)

## Initial Setup
1. Clone this repository
2. Run `shopify theme dev` to start local development
3. Make changes and test locally
4. Push changes to dev store with `shopify theme push`
5. Create PR and merge to main for production deployment