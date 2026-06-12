# CivicVault - Secure Civic Document Management

> A comprehensive full-stack application for secure storage, management, and controlled sharing of sensitive civic documents and personal records with advanced privacy protection and audit logging.

## 🎯 Overview

CivicVault is an open-source platform designed to help citizens securely manage and control access to their sensitive civic documents, financial records, and personal information. The system implements emergency privacy lockdown capabilities, evidence collection for safety purposes, and granular access control with complete audit trails.

### Key Principles

- **Privacy First**: Documents are encrypted and users maintain full control over who accesses their data
- **Transparency**: Complete audit logs of every access and action taken on documents
- **Safety**: Emergency mode for users facing threats or persecution
- **Compliance**: Built with security best practices and prepared for regulatory compliance
- **User-Centric**: Simple, intuitive interface for non-technical users

## 📋 Features

### Core Document Management
- **Organized Storage**: Categorize documents (bank statements, government IDs, property deeds, etc.)
- **Secure Upload**: Files encrypted with AES-256-GCM before storage
- **Access Control**: Granular permission management with requestor workflow
- **Version Tracking**: Keep history of document access requests and grants
- **Metadata Management**: Track document details without exposing content

### Safety & Emergency Features
- **Emergency Lockdown**: Activate emergency mode to restrict all access except trusted contacts
- **Threat Assessment**: Evaluate exposure and threat levels based on user situation
- **Evidence Collection**: Secure upload and encryption of threat evidence (screenshots, messages, etc.)
- **Safety Checklist**: Automated safety recommendations based on threat level
- **Admin Workflow**: Request and approve access to user accounts during emergencies (with full audit trail)

### Security & Compliance
- **Encryption**: AES-256-GCM encryption for all sensitive files
- **Authentication**: Supabase-powered email auth with demo mode for development
- **Authorization**: Role-based access control (user, admin)
- **Audit Logging**: Complete log of all actions, access requests, and data access
- **Rate Limiting**: Protection against brute force and DoS attacks
- **CORS Protection**: Strict cross-origin resource sharing policies
- **Security Headers**: Helmet.js for comprehensive HTTP security headers

## 🏗️ Architecture

```
CivicVault/
├── CODORRA/                          # Main application folder
│   ├── src/                          # Backend source code
│   │   ├── routes/                   # API route handlers
│   │   ├── middleware/               # Express middleware
│   │   ├── services/                 # Business logic
│   │   ├── utils/                    # Helper functions
│   │   ├── config/                   # Configuration
│   │   ├── store/                    # Data access layer
│   │   ├── types/                    # TypeScript types
│   │   └── index.ts                  # Application entry point
│   ├── CivicVault-frontend/          # Frontend application
│   │   └── civicvault-app/           # React Vite app
│   │       ├── src/                  # React components and pages
│   │       ├── public/               # Static assets
│   │       └── package.json
│   ├── supabase/                     # Database migrations
│   │   └── migrations/               # SQL migration files
│   ├── tools/                        # Utility scripts
│   ├── scripts/                      # Testing and demo scripts
│   └── package.json                  # Backend dependencies
```

### Technology Stack

#### Backend
- **Runtime**: Node.js
- **Framework**: Express.js 5.x
- **Language**: TypeScript 5.x
- **Database**: PostgreSQL via Supabase
- **Authentication**: Supabase Auth
- **File Storage**: Supabase Storage
- **Encryption**: Node.js crypto (AES-256-GCM)
- **Validation**: Zod for schema validation
- **Security**: Helmet.js, CORS, Rate Limiting, Express compression

#### Frontend
- **Framework**: React 19.x
- **Build Tool**: Vite 8.x
- **Routing**: React Router 7.x
- **Styling**: CSS (custom)
- **Linting**: ESLint 10.x

#### Infrastructure
- **Database**: PostgreSQL (Supabase)
- **File Storage**: Supabase Storage (S3-compatible)
- **Deployment**: Container-ready (can be deployed to Docker, Vercel, Railway, etc.)

## 🚀 Quick Start

### Prerequisites
- Node.js 18+ and npm/yarn
- Git
- Supabase account (optional, demo mode available for development)

### Installation

1. **Clone the repository**
```bash
git clone https://github.com/TheNityant/Civic_Sense-Full-Stack.git
cd Civic_Sense-Full-Stack/CODORRA
```

2. **Install backend dependencies**
```bash
npm install
```

3. **Set up environment variables**
```bash
cp .env.example .env
```

Edit `.env` and configure:
- `PORT`: Backend port (default: 3001)
- `CORS_ORIGIN`: Frontend URL for CORS (default: http://localhost:3000)
- `APP_ENCRYPTION_SECRET`: Secret key for encryption (change in production!)
- `ALLOW_DEMO_AUTH`: Set to `true` for development without Supabase
- Supabase credentials (optional for demo mode):
  - `SUPABASE_URL`
  - `SUPABASE_SERVICE_ROLE_KEY`
  - `SUPABASE_ANON_KEY`

4. **Start the backend development server**
```bash
npm run dev
```

Backend will be available at `http://localhost:3001`

Health check:
```bash
curl http://localhost:3001/health
```

5. **Install and start frontend (in another terminal)**
```bash
cd CivicVault-frontend/civicvault-app
npm install
npm run dev
```

Frontend will be available at `http://localhost:5173`

## 📡 API Reference

All API endpoints require authentication. For development, use demo headers:

```
x-demo-user-id: <user_id>
x-demo-user-email: <user_email>
x-demo-user-name: <display_name>
x-demo-role: user|admin
```

### Authentication
- `POST /api/auth/login` - User login
- `POST /api/auth/logout` - User logout
- `GET /api/auth/profile` - Get current user profile

### Documents & Assessments
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
- `GET /api/evidence/:id` - Get specific evidence
- `DELETE /api/evidence/:id` - Delete evidence

### Admin & Access Control
- `POST /api/admin/requests` - Submit admin access request
- `GET /api/admin/requests` - List access requests
- `PATCH /api/admin/requests/:id` - Approve/deny request
- `GET /api/audit` - View audit logs

### Complete API Documentation

For detailed API documentation with request/response examples, see [CODORRA/README.md](./CODORRA/README.md)

## 💻 Development

### Build
```bash
npm run build
```

Compiles TypeScript and outputs to `dist/`

### Start Production Server
```bash
npm start
```

### Database Migrations
```bash
npm run migrate
```

Runs pending migrations from `supabase/migrations/`

### Testing with curl

Backend provides convenient testing scripts:

**macOS/Linux:**
```bash
./scripts/test.sh
```

**Windows (PowerShell):**
```bash
./scripts/test.ps1
```

Example manual test - exposure assessment:
```bash
curl -X POST http://localhost:3001/api/assessments/exposure \
  -H "Content-Type: application/json" \
  -H "x-demo-user-id: alice" \
  -d '{"publicInstagram":true,"locationSharing":false}'
```

Example - upload evidence:
```bash
curl -X POST http://localhost:3001/api/evidence \
  -H "x-demo-user-id: alice" \
  -F "file=@/path/to/evidence.png" \
  -F "label=Screenshot of threat"
```

### Frontend Development

```bash
cd CivicVault-frontend/civicvault-app
npm run dev      # Start dev server (port 5173)
npm run build    # Build for production
npm run lint     # Run ESLint
npm run preview  # Preview production build
```

## 🔒 Security

### Encryption
- **Algorithm**: AES-256-GCM (NIST approved)
- **Key Derivation**: 32-byte keys from environment
- **Evidence Files**: Fully encrypted before storage
- **Metadata**: Stored in plaintext for searchability (can be encrypted in production)

### Authentication & Authorization
- **Auth Method**: Supabase Auth (production) or demo headers (development)
- **Roles**: `user`, `admin`
- **Token Storage**: HTTP-only cookies (Supabase) or custom headers (demo)
- **Session Management**: Token expiration and refresh

### API Security
- **HTTPS**: Recommended for production
- **CORS**: Strict origin checking
- **Rate Limiting**: Per-IP request throttling
- **Input Validation**: Zod schema validation on all endpoints
- **Error Handling**: Generic error responses to prevent information leakage
- **Helmet**: Comprehensive HTTP security headers

### Best Practices (for deployment)
1. Change `APP_ENCRYPTION_SECRET` to a strong, unique value
2. Disable `ALLOW_DEMO_AUTH` in production
3. Use environment-specific Supabase projects
4. Enable HTTPS/TLS
5. Implement proper backup and disaster recovery
6. Regularly audit access logs
7. Keep dependencies updated

## 📚 Project Structure

### Backend (`CODORRA/src/`)

- **routes/** - API route definitions
  - `auth.routes.ts` - Authentication endpoints
  - `assessments.routes.ts` - Risk assessment endpoints
  - `emergency.routes.ts` - Emergency mode endpoints
  - `evidence.routes.ts` - Evidence management
  - `admin.routes.ts` - Admin operations
  - `audit.routes.ts` - Audit log retrieval

- **middleware/** - Express middleware
  - `auth.ts` - Authentication verification
  - `require-role.ts` - Role-based authorization
  - `error-handler.ts` - Global error handling

- **services/** - Business logic
  - Risk assessment algorithms
  - Evidence processing
  - Access request workflows

- **utils/** - Helper functions
  - `crypto.ts` - Encryption/decryption utilities
  - `risk.ts` - Risk calculation algorithms
  - `http.ts` - HTTP utilities

- **store/** - Data access layer
  - `supabase.ts` - Database queries
  - `memory.ts` - In-memory storage (demo mode)
  - `types.ts` - Store interface definitions

- **config/** - Configuration
  - `env.ts` - Environment variable parsing
  - `supabase.ts` - Supabase client initialization

### Frontend (`CivicVault-frontend/civicvault-app/src/`)

- **components/** - React UI components
- **pages/** - Page-level components
- **hooks/** - Custom React hooks
- **utils/** - Frontend utilities
- **styles/** - CSS stylesheets

## 🤝 Contributing

We welcome contributions! Please follow these guidelines:

1. **Fork** the repository
2. **Create** a feature branch (`git checkout -b feature/amazing-feature`)
3. **Make** your changes with clear, descriptive commits
4. **Write** or update tests if applicable
5. **Ensure** code passes linting (`npm run lint`)
6. **Submit** a pull request with a clear description

### Code Style

- Use TypeScript for all backend code
- Follow ESLint configuration for frontend
- Write comments for complex logic
- Keep components small and focused
- Use meaningful variable and function names

### Testing

Before submitting a PR, test your changes:

```bash
# Backend tests (run in CODORRA/)
npm run dev

# Frontend tests
cd CivicVault-frontend/civicvault-app
npm run dev

# Manual API testing
./scripts/test.sh  # macOS/Linux
./scripts/test.ps1 # Windows
```

## 🐛 Troubleshooting

### Backend won't start

**Issue**: `Error: EACCES: permission denied, open '.env'`

**Solution**: Ensure `.env` file exists and is readable:
```bash
cp .env.example .env
chmod 644 .env
```

**Issue**: `Port 3001 already in use`

**Solution**: Change port in `.env` or kill process using the port:
```bash
# Linux/macOS
lsof -i :3001 | grep LISTEN | awk '{print $2}' | xargs kill -9

# Windows
netstat -ano | findstr :3001
taskkill /PID <PID> /F
```

### Frontend build errors

**Issue**: `Module not found` errors

**Solution**: Reinstall dependencies:
```bash
cd CivicVault-frontend/civicvault-app
rm -rf node_modules package-lock.json
npm install
```

### Database connection fails

**Issue**: `Connection refused` or `FATAL: password authentication failed`

**Solution**: Verify Supabase credentials in `.env`:
```bash
# Test connection with psql
psql "******db.example.com/postgres"
```

### Demo auth not working

**Issue**: `401 Unauthorized` even with demo headers

**Solution**: Ensure `ALLOW_DEMO_AUTH=true` in `.env` and restart the server:
```bash
cat .env | grep ALLOW_DEMO_AUTH
npm run dev  # Restart
```

### CORS errors

**Issue**: `CORS blocked for origin 'http://localhost:3000'`

**Solution**: Verify `CORS_ORIGIN` in `.env` matches your frontend URL and restart:
```bash
# .env should have:
CORS_ORIGIN=http://localhost:3000
npm run dev  # Restart
```

## 📝 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🙋 Support

Have questions or issues? Please:

1. Check [Troubleshooting](#-troubleshooting) section
2. Review existing [GitHub Issues](https://github.com/TheNityant/Civic_Sense-Full-Stack/issues)
3. Create a new issue with detailed information
4. Contact the maintainers

## 🔗 Related Resources

- [Supabase Documentation](https://supabase.com/docs)
- [Express.js Guide](https://expressjs.com/)
- [React Documentation](https://react.dev)
- [Node.js Crypto Module](https://nodejs.org/api/crypto.html)
- [OWASP Security Guidelines](https://owasp.org/)

## ✨ Acknowledgments

This project was built with security, privacy, and user empowerment in mind. Special thanks to all contributors and the open-source community.

---

**Made with ❤️ for civic empowerment and digital privacy**
