# American Flights API - DEX 401 Training

A Mule 4 application that provides RESTful API endpoints for managing American Airlines flight information.

## Features

- **GET /api/flights** - Retrieve all available flights
- **GET /api/flights/{ID}** - Get specific flight by ID
- **POST /api/flights** - Create a new flight
- **DELETE /api/flights/{ID}** - Delete a flight by ID

## API Endpoints

Base URL: `http://localhost:8081/api`

### Get All Flights
```http
GET /api/flights
```

### Get Flight by ID
```http
GET /api/flights/1
```

### Create Flight
```http
POST /api/flights
Content-Type: application/json

{
  "code": "ER12ab",
  "price": 450.00,
  "departureDate": "2016/12/15",
  "origin": "MUA",
  "destination": "SFO",
  "emptySeats": 25,
  "plane": {
    "type": "Boeing 737",
    "totalSeats": 150
  }
}
```

### Delete Flight
```http
DELETE /api/flights/1
```

## Technology Stack

- **Mule Runtime**: 4.10.0
- **Java**: 17
- **API Specification**: RAML 1.0
- **Build Tool**: Maven

## Project Structure

```
.
├── .github/
│   └── workflows/          # GitHub Actions CI/CD pipelines
├── src/
│   ├── main/
│   │   ├── mule/          # Mule flows and configurations
│   │   └── resources/     # Application resources
│   └── test/
│       ├── munit/         # MUnit test suites
│       └── resources/     # Test resources
├── pom.xml                # Maven project configuration
└── mule-artifact.json     # Mule application descriptor
```

## Local Development

### Prerequisites

- Java 17
- Maven 3.8+
- Anypoint Studio (optional)

### Running Locally

1. **Using VS Code (Recommended)**:
   - Press `F5` or click Run & Debug
   - Select "Debug Mule Application"

2. **Using Maven**:
   ```bash
   mvn clean package
   # Then run through Anypoint Code Builder
   ```

3. **Access the API**:
   - API Console: http://localhost:8081/console/
   - API Endpoint: http://localhost:8081/api/flights

### Testing

The application connects to a mock American Airlines API:
```
https://c5e5b4fd-2f7e-4802-afe9-c46738d735e3.mock.pstmn.io/american-flights
```

## CI/CD Pipeline

This project uses GitHub Actions for automated deployment to CloudHub 2.0.

### Workflows

1. **CI Build** (`ci-build.yml`)
   - Triggers on: PRs to `main`, pushes to `feature/**` or `bugfix/**`
   - Runs: Build, test, package, code quality checks

2. **Deploy** (`deploy-cloudhub2.yml`)
   - Triggers on: Merge to `main` branch
   - Deploys to: **Sandbox** environment on CloudHub 2.0

### Deployment Configuration

- **Environment**: Sandbox (FPA Business Group)
- **Region**: Cloudhub-US-East-2 (Ohio)
- **App Name**: `dex-401-training-mule`
- **Resources**: 1 replica, 0.1 vCores
- **URL**: https://dex-401-training-mule.us-e2.cloudhub.io

### Required Secrets

Add these to GitHub repository settings (Settings → Secrets and variables → Actions):

| Secret | Description |
|--------|-------------|
| `ANYPOINT_CLIENT_ID` | Connected App Client ID |
| `ANYPOINT_CLIENT_SECRET` | Connected App Client Secret |

See [`.github/PIPELINE_SETUP.md`](.github/PIPELINE_SETUP.md) for detailed setup instructions.

## API Documentation

Access the interactive API console when running locally:
- Console: http://localhost:8081/console/

## Configuration

### Business Group
- **Name**: FPA
- **ID**: `bdc363af-79af-48f0-9e26-e6f5fd1bee9c`

### External API
The application integrates with American Airlines mock API for flight data.

## Development Workflow

1. Create a feature branch: `git checkout -b feature/my-feature`
2. Make changes and commit
3. Push to GitHub: `git push origin feature/my-feature`
4. Create Pull Request to `main`
5. CI pipeline runs automatically
6. After merge to `main`, automatic deployment to Sandbox

## Version Management

Versions are auto-incremented in CI/CD:
- Format: `{major}.{minor}.{commitCount}`
- Example: `1.0.42` (based on git commit count)
