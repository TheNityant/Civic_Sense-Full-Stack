# CivicVault Backend

This backend powers the CivicVault secure civic document management system with emergency privacy lockdown capabilities.

## What it covers

- Email-based auth through Supabase, with a local demo fallback for hackathon mode
- Emergency mode activation and status management
- Exposure and threat scoring algorithms
- Safety action plan generation with recommendations
- Evidence upload with AES-256-GCM encryption before storage
- Admin access request workflow with approval/denial
- Comprehensive audit log capture for every action
- Rate limiting and security headers for protection

## Quick start

1. Copy `.env.example` to `.env`.
2. Install dependencies: `npm install`
3. Run development server: `npm run dev`
4. Server will be available at `http://localhost:3001`

Health check:
```bash
curl http://localhost:3001/health
```

## API Endpoints Overview

### Authentication
- `POST /api/auth/login` - User login
- `POST /api/auth/logout` - User logout
- `GET /api/auth/profile` - Get current user profile

### Assessments
- `POST /api/assessments/exposure` - Evaluate digital exposure
- `POST /api/assessments/threat` - Assess threat level
- `POST /api/assessments/checklist` - Get safety recommendations

### Emergency Mode
- `POST /api/emergency/activate` - Activate emergency lockdown
- `GET /api/emergency/status` - Get current emergency status
- `POST /api/emergency/deactivate` - Deactivate emergency mode

### Evidence Management
- `POST /api/evidence` - Upload evidence (multipart/form-data)
- `GET /api/evidence` - List all evidence
- `GET /api/evidence/:id` - Get specific evidence details
- `DELETE /api/evidence/:id` - Delete evidence

### Admin & Access Control
- `POST /api/admin/requests` - Submit admin access request
- `GET /api/admin/requests` - List pending access requests
- `PATCH /api/admin/requests/:id` - Approve/deny access request

### Audit & Logging
- `GET /api/audit` - View complete audit logs

## Quick Testing & Frontend Integration

### Setup Environment

Copy the example env and set at minimum `APP_ENCRYPTION_SECRET`. For persistent storage, configure Supabase credentials:

```bash
cp .env.example .env
# Edit .env and configure:
# - APP_ENCRYPTION_SECRET (required for encryption)
# - SUPABASE_URL, SUPABASE_SERVICE_ROLE_KEY (optional if using demo auth)
# - ALLOW_DEMO_AUTH=true (for development without Supabase)
```

### Demo Auth Headers (Development Only)

For hackathon/demo mode, use header-based authentication without Supabase. Add these headers to requests:

```
x-demo-user-id: <user_id>              # e.g., "alice"
x-demo-user-email: <user_email>        # e.g., "alice@example.com"
x-demo-user-name: <display_name>       # e.g., "Alice"
x-demo-role: user|admin                # User role
```

### Example: Frontend Integration with Fetch API

```javascript
const apiBase = 'http://localhost:3001/api';

fetch(`${apiBase}/assessments/exposure`, {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
    'x-demo-user-id': 'alice',
    'x-demo-user-email': 'alice@example.com',
    'x-demo-user-name': 'Alice',
    'x-demo-role': 'user'
  },
  body: JSON.stringify({ 
    publicInstagram: true,
    locationSharing: false,
    socialMediaCheck: true
  })
});
```

### Example: Curl Commands for Testing

**Exposure Assessment:**
```bash
curl -X POST http://localhost:3001/api/assessments/exposure \
  -H "Content-Type: application/json" \
  -H "x-demo-user-id: alice" \
  -d '{"publicInstagram":true,"locationSharing":false}'
```

**Threat Assessment:**
```bash
curl -X POST http://localhost:3001/api/assessments/threat \
  -H "Content-Type: application/json" \
  -H "x-demo-user-id: alice" \
  -d '{"directThreats":true,"onlineHarassment":false,"harassment":true}'
```

**Activate Emergency Mode:**
```bash
curl -X POST http://localhost:3001/api/emergency/activate \
  -H "Content-Type: application/json" \
  -H "x-demo-user-id: alice" \
  -d '{
    "reason":"Threat received",
    "exposureAnswers":{"publicInstagram":true},
    "threatAnswers":{"directThreats":true}
  }'
```

**Upload Evidence (File):**
```bash
curl -X POST http://localhost:3001/api/evidence \
  -H "x-demo-user-id: alice" \
  -F "file=@/path/to/screenshot.png" \
  -F "label=Threat screenshot from attacker"
```

**Submit Admin Access Request:**
```bash
curl -X POST http://localhost:3001/api/admin/requests \
  -H "Content-Type: application/json" \
  -H "x-demo-user-id: alice" \
  -d '{"reason":"Emergency investigation","caseId":"CASE-001"}'
```

**Get Audit Logs:**
```bash
curl -X GET http://localhost:3001/api/audit \
  -H "x-demo-user-id: alice"
```

### Automated Testing Scripts

For quick testing of all endpoints:

**macOS/Linux/WSL:**
```bash
./scripts/test.sh
```

**Windows (PowerShell):**
```bash
./scripts/test.ps1
```

These scripts contain all common curl commands for testing the API flows.

### Frontend Configuration

When connecting your frontend:

1. Set your API base URL to `http://localhost:3001/api`
2. During development, include demo headers in all requests
3. In production, ensure frontend uses Supabase auth credentials
4. The backend CORS origin is controlled by `CORS_ORIGIN` in `.env`

Example configuration:
```javascript
// frontend/config.js
const API_BASE = process.env.NODE_ENV === 'production' 
  ? 'https://api.civicvault.app/api'
  : 'http://localhost:3001/api';

const DEMO_AUTH = {
  userId: 'alice',
  userEmail: 'alice@example.com',
  userName: 'Alice',
  role: 'user'
};
```

## Security Considerations

### Encryption
- Files are encrypted using Node's `crypto` module (AES-256-GCM)
- Only file content is encrypted; metadata is stored in plaintext for searchability
- For production, consider encrypting metadata as well

### Authentication & Authorization
- **Demo Mode**: Header-based auth for development (NEVER use in production)
- **Production**: Use Supabase email auth with proper session management
- **Disable Demo Auth**: Set `ALLOW_DEMO_AUTH=false` in production

### Best Practices
1. Change `APP_ENCRYPTION_SECRET` to a strong, unique value in production
2. Use HTTPS/TLS in production
3. Implement proper backup and disaster recovery procedures
4. Regularly audit access logs for suspicious activity
5. Keep all dependencies updated
6. Implement proper rate limiting thresholds for your use case
7. Use environment-specific Supabase projects for dev/staging/production

## Development

### Build for Production
```bash
npm run build
```
Outputs to `dist/` directory.

### Start Production Server
```bash
npm start
```

### Run Database Migrations
```bash
npm run migrate
```
Applies pending migrations from `supabase/migrations/`.

### Project Structure
```
src/
├── routes/          # API route handlers
├── middleware/      # Express middleware
├── services/        # Business logic
├── utils/           # Helper utilities
├── config/          # Configuration files
├── store/           # Data access layer
└── index.ts         # Application entry
```

## Important Notes

- The demo auth fallback is for local/hackathon demos only
- Keep the admin portal simulated unless you have a documented access control policy
- Always encrypt evidence before storage
- Log every access request and maintain audit trails
- Avoid building real integrations with social platforms or government systems for demos

## Troubleshooting

**Port already in use:**
```bash
# Find process using port 3001
lsof -i :3001 | grep LISTEN

# Kill the process
kill -9 <PID>
```

**Demo auth not working:**
- Verify `ALLOW_DEMO_AUTH=true` in `.env`
- Restart the dev server: `npm run dev`

**CORS errors:**
- Check `CORS_ORIGIN` in `.env` matches your frontend URL
- Restart the server after changes

**Database connection issues:**
- Verify Supabase credentials in `.env`
- Check PostgreSQL connection string format

## License

Part of the CivicVault project. See main README for license information.
