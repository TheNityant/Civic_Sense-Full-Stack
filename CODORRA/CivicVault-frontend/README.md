# CivicVault Frontend

React frontend application for the CivicVault secure civic document management system.

## About

CivicVault is a comprehensive platform that helps users securely manage and control access to their sensitive civic documents and personal records. This frontend provides an intuitive user interface for document management, threat assessment, emergency mode activation, and access control.

## Features

- **Document Dashboard**: View and manage organized documents across categories
- **Access Control**: Grant/deny access requests from authorized personnel
- **Threat Assessment**: Evaluate exposure and threat levels
- **Emergency Mode**: Activate emergency lockdown for documents
- **Evidence Upload**: Securely upload and manage threat evidence
- **Audit Trail**: Track all access requests and actions
- **Admin Panel**: (Admin users) Manage user accounts and access requests

## Tech Stack

- **React 19.x** - UI library
- **React Router 7.x** - Client-side routing
- **Vite 8.x** - Build tool
- **ESLint 10.x** - Code quality
- **Vanilla CSS** - Styling

## Prerequisites

- Node.js 18+ and npm
- Backend API running at http://localhost:3001 (default)

## Quick Start

### Installation

```bash
# Install dependencies
npm install
```

### Development

```bash
# Start development server (Vite hot module replacement enabled)
npm run dev
```

The frontend will be available at `http://localhost:5173`

### Build for Production

```bash
# Build optimized production bundle
npm run build
```

Outputs to `dist/` directory. Deploy this folder to your hosting service.

### Preview Production Build

```bash
# Preview the production build locally
npm run preview
```

### Linting

```bash
# Run ESLint to check code quality
npm run lint

# Fix linting issues automatically (if available)
npm run lint --fix
```

## Configuration

### Backend API URL

The frontend expects the backend API at `http://localhost:3001/api` by default.

To configure a different API URL, edit your environment or API client configuration:

```javascript
// src/config/api.js (or similar)
const API_BASE = process.env.REACT_APP_API_URL || 'http://localhost:3001/api';
```

### Development Mode with Demo Auth

When running with the backend in demo mode:

1. Ensure backend has `ALLOW_DEMO_AUTH=true` in `.env`
2. Include demo auth headers in API requests:

```javascript
const headers = {
  'Content-Type': 'application/json',
  'x-demo-user-id': 'alice',
  'x-demo-user-email': 'alice@example.com',
  'x-demo-user-name': 'Alice',
  'x-demo-role': 'user'
};
```

## Project Structure

```
src/
├── components/        # Reusable React components
├── pages/             # Page-level components
├── hooks/             # Custom React hooks
├── utils/             # Utility functions
├── styles/            # CSS stylesheets
├── App.jsx            # Main app component
└── main.jsx           # React entry point
```

## Available Routes

- `/` - Home/Dashboard
- `/documents` - Document management
- `/assess` - Threat assessment
- `/emergency` - Emergency mode
- `/evidence` - Evidence upload and management
- `/admin` - Admin panel (admin users only)

## API Integration

The frontend communicates with the backend via REST API. Key endpoints:

- `POST /assessments/exposure` - Evaluate digital exposure
- `POST /assessments/threat` - Assess threat level
- `POST /emergency/activate` - Activate emergency lockdown
- `POST /evidence` - Upload evidence files
- `PATCH /admin/requests/:id` - Manage access requests

See [Backend README](../README.md) for complete API documentation.

## Building & Deployment

### Development Build

```bash
npm run dev
```

### Production Build

```bash
npm run build
```

### Deploy to Common Platforms

**Vercel:**
```bash
npm i -g vercel
vercel
```

**Netlify:**
```bash
npm run build
# Deploy dist/ folder to Netlify
```

**Docker:**
```dockerfile
FROM node:18-alpine
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build
EXPOSE 3000
CMD ["npm", "run", "preview"]
```

**Traditional Server:**
```bash
npm run build
# Copy dist/ folder contents to web root
# Configure server to serve index.html for all routes (SPA)
```

## Troubleshooting

### Port Already in Use

```bash
# Change port in vite.config.js or use environment variable
npm run dev -- --port 5174
```

### Module Not Found Errors

```bash
# Clear and reinstall dependencies
rm -rf node_modules package-lock.json
npm install
```

### Hot Module Replacement Not Working

```bash
# Restart dev server
npm run dev
```

### Build Errors

```bash
# Check for TypeScript/ESLint issues
npm run lint

# Clear build cache and rebuild
rm -rf dist
npm run build
```

### API Connection Issues

1. Ensure backend is running: `curl http://localhost:3001/health`
2. Check `CORS_ORIGIN` in backend `.env` includes frontend URL
3. Verify API URL in frontend configuration
4. Check browser console for CORS errors

## Development Tips

- Use React DevTools browser extension for component debugging
- Check Network tab in DevTools for API request/response debugging
- Use `console.log()` for quick debugging
- ESLint will help catch common errors while typing

## Contributing

When contributing to the frontend:

1. Run `npm run lint` before committing
2. Keep components small and focused
3. Use meaningful variable and function names
4. Document complex logic with comments
5. Test changes in both development and production builds

## License

Part of the CivicVault project. See main README for license information.

---

**For backend documentation, see [Backend README](../README.md)**