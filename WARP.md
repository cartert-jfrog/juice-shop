# WARP.md

This file provides guidance to WARP (warp.dev) when working with code in this repository.

## Project Overview

OWASP Juice Shop is an intentionally insecure web application used for security training, awareness demos, CTFs, and testing security tools. It's built with Node.js/Express backend and Angular frontend, containing vulnerabilities from the OWASP Top Ten and many other security flaws found in real-world applications.

## Development Commands

### Setup and Installation
```bash
npm install                    # Install all dependencies (backend + frontend)
```

### Build Commands
```bash
npm run build:frontend        # Build Angular frontend only
npm run build:server          # Compile TypeScript backend to build/
npm run postinstall            # Full build (frontend + server) - runs automatically after npm install
```

### Running the Application
```bash
npm start                      # Run production build (requires build first)
npm run serve                  # Run in development mode (backend + frontend concurrently)
npm run serve:dev              # Development mode with hot reload using ts-node-dev
```

### Testing
```bash
npm test                       # Run all tests (frontend unit tests + backend server tests)
npm run test:server            # Run backend Mocha tests only
npm run test:api               # Run Frisby API integration tests
npm run test:chromium          # Run frontend tests in headless Chromium
npm run frisby                 # Run API tests with coverage reporting
```

### End-to-End Testing
```bash
npm run cypress:open           # Open Cypress test runner GUI
npm run cypress:run            # Run Cypress e2e tests headlessly
```

### Code Quality
```bash
npm run lint                   # Lint all TypeScript files (backend + frontend)
npm run lint:fix               # Auto-fix linting issues
npm run lint:config            # Validate YAML configuration files against schema
```

### Docker
```bash
docker pull bkimminich/juice-shop                    # Get official image
docker run --rm -p 3000:3000 bkimminich/juice-shop   # Run in container
```

## Architecture Overview

### Backend Structure
- **Entry Point**: `app.ts` → `server.ts` - Main server setup and middleware configuration
- **Models**: `models/` - Sequelize ORM models for SQLite database (User, Product, Challenge, etc.)
- **Routes**: `routes/` - Express route handlers organized by feature (login, products, challenges, etc.)
- **Libraries**: `lib/` - Core business logic, utilities, security functions, challenge validation
- **Data**: `data/` - Database initialization, static data, and data creation utilities
- **Configuration**: `config/` - YAML configuration files for different deployment scenarios

### Frontend Structure
- **Framework**: Angular with TypeScript
- **Location**: `frontend/` directory with standard Angular CLI structure
- **Build**: Webpack-based build process, outputs to `frontend/dist/`

### Key Components

#### Challenge System
- Challenges are defined in `data/static/challenges.yml` and managed through `models/challenge.ts`
- Challenge validation logic in `lib/challengeUtils.ts`
- Progress tracking and scoring system built into the application

#### Security Features (Intentionally Vulnerable)
- Authentication via JWT (with vulnerabilities)
- SQL injection opportunities in various endpoints
- XSS vulnerabilities in multiple locations
- File upload vulnerabilities
- Business logic flaws

#### Configuration System
- YAML-based configuration in `config/`
- `default.yml` contains base configuration
- Environment-specific overrides (e.g., `ctf.yml`, `bodgeit.yml`)
- Schema validation via `config.schema.yml`

## Development Guidelines

### Code Standards
- Uses ESLint with JavaScript Standard Style
- All backend code in TypeScript
- Frontend uses Angular style guidelines
- All commits must be signed off (DCO compliance)

### Testing Requirements
- New features require corresponding unit/integration tests
- New challenges must have e2e tests in Cypress
- API changes need Frisby test coverage

### Key Development Paths
- **Challenge Development**: Add to `data/static/challenges.yml`, implement validation in `lib/challengeUtils.ts`
- **Route Development**: Create in `routes/`, register in `server.ts`
- **Model Changes**: Update Sequelize models in `models/`, may require database migration
- **Frontend Features**: Standard Angular development in `frontend/src/`

### Important Files
- `server.ts` - Main server configuration and middleware setup
- `config/default.yml` - Application configuration
- `data/datacreator.ts` - Database initialization and seeding
- `lib/insecurity.ts` - Intentional security vulnerabilities
- `models/index.ts` - Database connection and model registration

### Multi-Language Support
- Internationalization via `i18n/` directory
- Crowdin integration for community translations
- Frontend translation system using ngx-translate

## Configuration Customization

The application can be customized through YAML configuration files:
- Products, users, and challenges can be modified
- Branding and theming options available
- CTF mode configuration for competitive scenarios
- Custom welcome messages and social media links

## Docker Development

For containerized development:
- Dockerfile uses multi-stage build
- Development can use docker-compose for database services
- Volume mounting supported for live development
