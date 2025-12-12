[![CI/CD Pipeline](https://github.com/Clement-Szewczyk/2025-devops-tp-final/actions/workflows/ci-cd.yml/badge.svg?branch=main)](https://github.com/Clement-Szewczyk/2025-devops-tp-final/actions/workflows/ci-cd.yml)
[![Frontend CI](https://github.com/Clement-Szewczyk/2025-devops-tp-final/actions/workflows/frontend-ci.yml/badge.svg?branch=main)](https://github.com/Clement-Szewczyk/2025-devops-tp-final/actions/workflows/frontend-ci.yml)
[![Backend CI](https://github.com/Clement-Szewczyk/2025-devops-tp-final/actions/workflows/backend-ci.yml/badge.svg?branch=main)](https://github.com/Clement-Szewczyk/2025-devops-tp-final/actions/workflows/backend-ci.yml)
[![E2E Tests](https://github.com/Clement-Szewczyk/2025-devops-tp-final/actions/workflows/e2e.yml/badge.svg?branch=main)](https://github.com/Clement-Szewczyk/2025-devops-tp-final/actions/workflows/e2e.yml)
[![Release](https://github.com/Clement-Szewczyk/2025-devops-tp-final/actions/workflows/release.yml/badge.svg)](https://github.com/Clement-Szewczyk/2025-devops-tp-final/actions/workflows/release.yml)
[![Latest Release](https://img.shields.io/github/v/release/Clement-Szewczyk/2025-devops-tp-final)](https://github.com/Clement-Szewczyk/2025-devops-tp-final/releases/latest)
[![Docker Frontend](https://img.shields.io/docker/v/clementszewczyk/christmas-frontend?label=frontend)](https://hub.docker.com/r/clementszewczyk/christmas-frontend)
[![Docker Backend](https://img.shields.io/docker/v/clementszewczyk/christmas-backend?label=backend)](https://hub.docker.com/r/clementszewczyk/christmas-backend)

# Christmas Gift List

A full-stack web application for managing Christmas gift ideas with a festive theme. Keep track of people and their gift ideas, and select the perfect gift for everyone on your list!

## About

This project is a demonstration of a modern full-stack application with:

- Frontend built with React 19, TypeScript, TanStack Router/Query, and Tailwind CSS
- Backend API built with Go using Chi router and PostgreSQL
- Component development environment with Storybook
- Unit testing with Vitest (frontend) and Go's testing framework (backend)
- End-to-end testing with Playwright

## Deployment

- [Frontend Deployment](https://christmas-frontend.onrender.com)
- [Backend Deployment](https://christmas-backend-nme8.onrender.com)
- [Storybook Deployment](https://clement-szewczyk.github.io/2025-devops-tp-final/storybookFront/?path=/docs/components-giftcard--docs)

## Docker Images

- Frontend: `clementszewczyk/christmas-frontend`
- Backend: `clementszewczyk/christmas-backend`

## Project Structure

```
2025-devops-tp-final/
├── frontend/           # React SPA with TanStack Router, Query & Form
├── backend/            # Go API with Chi router, pgx driver, and PostgreSQL
├── e2e/                # Playwright E2E tests
└── docs/               # Project documentation
```

### Packages Overview

- **[frontend](./frontend/README.md)**: React-based single-page application with TypeScript, component library (Storybook), and unit tests (Vitest)
- **[backend](./backend/README.md)**: RESTful API server with Go, database migrations, and data seeding utilities
- **[e2e](./e2e/README.md)**: End-to-end test suite using Playwright for user flow validation

Each package has its own detailed README with setup instructions, available commands, and best practices.
- Automated CI/CD pipelines
- Documentation (with storybook), frontend, backend, database deployed (any provider, you can use a single one or multiple)
- Dockerfile for frontend and backend and deployed images on Docker Hub
- docker-compose file to run the full application (frontend, backend, database)
- Source code on GitHub
- Automatic creation of GitHub release for the project
- documentation on what you did in the `docs/` directory of the project
  - if you chose another platform than the ones we saw in class, document why and add clear steps on how you deploed on those platforms
## Getting Started

### Prerequisites

- **Node.js** 18+ and npm
- **Go** 1.23+
- **PostgreSQL** 15+ (or Docker for running PostgreSQL in a container)

### Installation

Install dependencies for all packages:

```bash
# Frontend dependencies
cd frontend && npm install

# Backend dependencies
cd backend && go mod download

# E2E test dependencies
cd e2e && npm install
```

For detailed setup instructions for each package, see:

- [Frontend Setup Guide](./frontend/README.md#getting-started)
- [Backend Setup Guide](./backend/README.md#getting-started)
- [E2E Testing Guide](./e2e/README.md#getting-started)

### Running the Application

1. **Start PostgreSQL database**
   Use docker or existing instance

2. **Start the backend server**

   ```bash
   cd backend
   go run cmd/server/main.go
   ```

   Server runs at http://localhost:8080. Migrations will run automatically on startup.

3. **Seed the database** (optional)

   ```bash
   cd backend
   go run scripts/seed.go
   ```

4. **Start the frontend dev server**

   ```bash
   cd frontend
   npm run dev
   ```

   Frontend runs at http://localhost:5173

## Available Commands

Detailed command documentation is available in each package's README:

- [Frontend Commands](./frontend/README.md#available-commands)
- [Backend Commands](./backend/README.md#development)
- [E2E Commands](./e2e/README.md#available-commands)

### Quick Reference

### Frontend Commands

All frontend commands should be run from the `frontend/` directory:

```bash
cd frontend
```

#### Development

```bash
npm run dev              # Start Vite dev server (http://localhost:5173)
npm run build            # Build for production (outputs to dist/)
npm run preview          # Preview production build locally
```

#### Testing

```bash
npm run test             # Run unit tests with Vitest
npm run test:ui          # Run tests with Vitest UI
npm run test:coverage    # Run tests with coverage report
```

#### Code Quality

```bash
npm run lint             # Run ESLint
npm run lint:fix         # Fix ESLint errors automatically
npm run format           # Format code with Prettier
npm run format:check     # Check code formatting with Prettier
```

#### Storybook

```bash
npm run storybook        # Start Storybook dev server (http://localhost:6006)
npm run build-storybook  # Build Storybook for production
```

### Backend Commands

All backend commands should be run from the `backend/` directory:

```bash
cd backend
```

#### Development

```bash
go run cmd/server/main.go              # Start the API server (http://localhost:8080)
go build -o bin/server cmd/server/main.go  # Build production binary
```

#### Testing

```bash
go test ./...                          # Run all tests
go test ./... -v                       # Run tests with verbose output
go test ./... -cover                   # Run tests with coverage
go test -run <TestName> ./...          # Run specific test
```

#### Code Quality

```bash
go vet ./...                          # Run Go vet (detect suspicious code)
go fmt ./...                          # Format Go code
```

#### Database

```bash
./scripts/migrate.sh                   # Run database migrations
go run scripts/seed.go                 # Seed database with sample data
```

### E2E Testing Commands

All E2E commands should be run from the `e2e/` directory:

```bash
cd e2e
```

```bash
npm test                # Run E2E tests in headless mode
npm run test:headed     # Run tests in headed mode (see browser)
npm run test:ui         # Run tests with Playwright UI
npm run report          # Show HTML test report
```

**Note**: Ensure both frontend and backend servers are running before executing E2E tests.

### Linting & Formatting (All Projects)

Run quality checks across the entire project:

```bash
# Frontend (from frontend/)
cd frontend
npm run lint
npm run format

# Backend (from backend/)
cd backend
go vet ./...
go fmt ./...
```

### Testing (All Projects)

Run all tests:

```bash
# Frontend unit tests
cd frontend && npm test

# Backend tests
cd backend && go test ./...

# E2E tests (requires running servers)
cd e2e && npm test
```

### Build (All Projects)

Build production artifacts:

```bash
# Frontend - outputs to frontend/dist/
cd frontend && npm run build

# Backend - outputs to backend/bin/server
cd backend && go build -o bin/server cmd/server/main.go
```

## Environment Variables

### Backend

Create a `.env` file in the `backend/` directory or set these environment variables:

- `DATABASE_URL` - PostgreSQL connection string
  - Default: `postgres://postgres:postgres@localhost:5432/christmas_gifts?sslmode=disable`
- `PORT` - Server port
  - Default: `8080`

### Frontend

The frontend uses Vite's proxy configuration to forward `/api` requests to the backend at `http://localhost:8080`. You can modify this in `frontend/vite.config.ts` if needed.

## Additional Documentation

- [Frontend README](./frontend/README.md) - Detailed frontend setup, API proxy configuration, and deployment gui- Automated CI/CD pipelines
- Documentation (with storybook), frontend, backend, database deployed (any provider, you can use a single one or multiple)
- Dockerfile for frontend and backend and deployed images on Docker Hub
- docker-compose file to run the full application (frontend, backend, database)
- Source code on GitHub
- Automatic creation of GitHub release for the project
- documentation on what you did in the `docs/` directory of the project
  - if you chose another platform than the ones we saw in class, document why and add clear steps on how you deploed on those platformsde
- [Backend README](./backend/README.md) - Backend setup, API development, and database management
- [E2E Testing README](./e2e/README.md) - End-to-end testing guide with Playwright
- [API Documentation](./docs/api.md) - API endpoints and request/response formats
- [Database Schema](./docs/database.md) - Database structure and migrations
- [Reverse Proxy Architecture](./docs/reverse-proxy.md) - How the reverse proxy works in different deployment scenarios

---

Made with care for Christmas
