# Virtual Facility Microservices

A NestJS-based microservices architecture for managing virtual facilities, buildings, and workflows.

## 📋 Table of Contents

- [Overview](#overview)
- [Architecture](#architecture)
- [Microservices](#microservices)
- [Project Structure](#project-structure)
- [Getting Started](#getting-started)
- [How It Works - Step by Step](#how-it-works---step-by-step)
- [API Endpoints](#api-endpoints)
- [Development](#development)
- [Testing](#testing)
- [Technologies Used](#technologies-used)

## 🎯 Overview

This project implements a **microservices architecture** using NestJS framework with a **monorepo structure**. It consists of two independent microservices:

1. **Virtual Facility Service** - Manages buildings and facility-related operations (Port 3000)
2. **Workflows Service** - Handles workflow management and orchestration (Port 3001)

## 🏗️ Architecture

```
┌─────────────────────────────────────────────────────────┐
│                    Client Application                   │
└────────────┬───────────────────────┬───────────────────┘
             │                       │
             ▼                       ▼
┌────────────────────────┐  ┌──────────────────────────┐
│  Virtual Facility      │  │   Workflows Service      │
│  Service               │  │                          │
│  Port: 3000            │  │   Port: 3001             │
│                        │  │                          │
│  - Buildings Module    │  │   - Workflows Module     │
│  - App Controller      │  │   - Workflows Controller │
│  - App Service         │  │   - Workflows Service    │
└────────────────────────┘  └──────────────────────────┘
```

### Architecture Principles

- **Independent Deployment**: Each service can be deployed independently
- **Loose Coupling**: Services communicate through well-defined interfaces
- **Single Responsibility**: Each service focuses on a specific domain
- **Scalability**: Services can scale independently based on load

## 🔧 Microservices

### 1. Virtual Facility Service (Port 3000)

**Purpose:** Manages virtual facilities and buildings

**Modules:**

- **BuildingsModule**: CRUD operations for building management
- **AppModule**: Root module that orchestrates the application

**Key Components:**

- `BuildingsController` - Handles HTTP requests for buildings
- `BuildingsService` - Contains business logic for buildings
- DTOs for request/response validation
- Entity definitions for data models

**Endpoints:**

- `POST /buildings` - Create a new building
- `GET /buildings` - Get all buildings
- `GET /buildings/:id` - Get a specific building by ID
- `PATCH /buildings/:id` - Update a building by ID
- `DELETE /buildings/:id` - Delete a building by ID

**Entry Point:** `apps/virtual-facility/src/main.ts`

### 2. Workflows Service (Port 3001)

**Purpose:** Handles workflow management and orchestration

**Modules:**

- **WorkflowsModule**: CRUD operations for workflow management
- **WorkflowsServiceModule**: Root module for the workflows service

**Key Components:**

- `WorkflowsController` - Handles HTTP requests for workflows
- `WorkflowsService` - Contains business logic for workflows
- DTOs for request/response validation
- Entity definitions for workflow models

**Endpoints:**

- `POST /workflows` - Create a new workflow
- `GET /workflows` - Get all workflows
- `GET /workflows/:id` - Get a specific workflow by ID
- `PATCH /workflows/:id` - Update a workflow by ID
- `DELETE /workflows/:id` - Delete a workflow by ID

**Entry Point:** `apps/workflows-service/src/main.ts`

## 📁 Project Structure

```
virtual-facility/
├── apps/
│   ├── virtual-facility/          # Main facility service
│   │   ├── src/
│   │   │   ├── buildings/         # Buildings feature module
│   │   │   │   ├── dto/           # Data Transfer Objects
│   │   │   │   │   ├── create-building.dto.ts
│   │   │   │   │   └── update-building.dto.ts
│   │   │   │   ├── entities/      # Database entities
│   │   │   │   │   └── building.entity.ts
│   │   │   │   ├── buildings.controller.ts      # REST endpoints
│   │   │   │   ├── buildings.controller.spec.ts # Unit tests
│   │   │   │   ├── buildings.service.ts         # Business logic
│   │   │   │   ├── buildings.service.spec.ts    # Service tests
│   │   │   │   └── buildings.module.ts          # Module definition
│   │   │   ├── app.controller.ts
│   │   │   ├── app.service.ts
│   │   │   ├── app.module.ts      # Root module
│   │   │   └── main.ts            # Application entry point
│   │   ├── test/                  # E2E tests
│   │   │   ├── app.e2e-spec.ts
│   │   │   └── jest-e2e.json
│   │   └── tsconfig.app.json
│   │
│   └── workflows-service/         # Workflows microservice
│       ├── src/
│       │   ├── workflows/         # Workflows feature module
│       │   │   ├── dto/           # Data Transfer Objects
│       │   │   │   ├── create-workflow.dto.ts
│       │   │   │   └── update-workflow.dto.ts
│       │   │   ├── entities/      # Database entities
│       │   │   │   └── workflow.entity.ts
│       │   │   ├── workflows.controller.ts      # REST endpoints
│       │   │   ├── workflows.controller.spec.ts # Unit tests
│       │   │   ├── workflows.service.ts         # Business logic
│       │   │   ├── workflows.service.spec.ts    # Service tests
│       │   │   └── workflows.module.ts          # Module definition
│       │   ├── workflows-service.controller.ts
│       │   ├── workflows-service.service.ts
│       │   ├── workflows-service.module.ts      # Root module
│       │   └── main.ts            # Application entry point
│       ├── test/                  # E2E tests
│       │   ├── app.e2e-spec.ts
│       │   └── jest-e2e.json
│       └── tsconfig.app.json
│
├── package.json                   # Dependencies and scripts
├── nest-cli.json                  # NestJS CLI configuration
├── tsconfig.json                  # TypeScript configuration
├── tsconfig.build.json            # Build configuration
├── eslint.config.mjs              # ESLint configuration
└── README.md                      # This file
```

## 🚀 Getting Started

### Prerequisites

- **Node.js** (v16 or higher recommended)
- **npm** or **yarn**
- **NestJS CLI** (optional but recommended)

### Installation

1. **Clone the repository:**

   ```bash
   git clone <repository-url>
   cd virtual-facility
   ```

2. **Install dependencies:**

   ```bash
   npm install
   ```

3. **Build the project:**
   ```bash
   npm run build
   ```

## 🔄 How It Works - Step by Step

### Step 1: Application Bootstrap 🚀

When you start the application, each microservice independently bootstraps itself:

#### Virtual Facility Service (Port 3000):

```typescript
// apps/virtual-facility/src/main.ts
async function bootstrap() {
  // 1. Creates a new NestJS application instance
  const app = await NestFactory.create(AppModule);

  // 2. Starts the HTTP server on port 3000
  await app.listen(process.env.PORT ?? 3000);
}
```

**What happens:**

1. NestJS scans the `AppModule` for metadata
2. Dependency injection container is initialized
3. All modules (BuildingsModule) are loaded
4. Controllers and providers are registered
5. HTTP server starts listening on port 3000

#### Workflows Service (Port 3001):

```typescript
// apps/workflows-service/src/main.ts
async function bootstrap() {
  // 1. Creates a new NestJS application instance
  const app = await NestFactory.create(WorkflowsServiceModule);

  // 2. Starts the HTTP server on port 3001
  await app.listen(process.env.port ?? 3001);
}
```

**What happens:**

1. NestJS scans the `WorkflowsServiceModule` for metadata
2. Dependency injection container is initialized
3. All modules (WorkflowsModule) are loaded
4. Controllers and providers are registered
5. HTTP server starts listening on port 3001

### Step 2: Module Initialization 📦

Each service initializes its respective modules using NestJS dependency injection:

#### Virtual Facility Service:

```
AppModule (Root)
  ↓
  ├── BuildingsModule (Feature Module)
  │     ↓
  │     ├── BuildingsController (Handles HTTP requests)
  │     └── BuildingsService (Business logic)
  │
  ├── AppController (Root controller)
  └── AppService (Root service)
```

#### Workflows Service:

```
WorkflowsServiceModule (Root)
  ↓
  ├── WorkflowsModule (Feature Module)
  │     ↓
  │     ├── WorkflowsController (Handles HTTP requests)
  │     └── WorkflowsService (Business logic)
  │
  ├── WorkflowsServiceController (Root controller)
  └── WorkflowsServiceService (Root service)
```

### Step 3: Request Processing Flow 🔄

When a client makes a request, it follows the **Controller → Service → Response** pattern:

#### Example 1: Creating a Building

**1. Client sends HTTP request:**

```bash
POST http://localhost:3000/buildings
Content-Type: application/json

{
  "name": "Building A",
  "address": "123 Main St",
  "floors": 5
}
```

**2. Request hits the Controller Layer:**

```typescript
// BuildingsController
@Post()
create(@Body() createBuildingDto: CreateBuildingDto) {
  // Decorator @Post() routes POST requests to this method
  // @Body() extracts and validates request body using DTO
  return this.buildingsService.create(createBuildingDto);
}
```

**What happens:**

- NestJS routing matches `/buildings` POST request
- Request body is validated against `CreateBuildingDto`
- If validation passes, request proceeds; otherwise, 400 Bad Request is returned

**3. Service Layer processes the request:**

```typescript
// BuildingsService
create(createBuildingDto: CreateBuildingDto) {
  // Business logic for creating a building
  // This is where you would:
  // - Validate business rules
  // - Save to database
  // - Process data
  return 'This action adds a new building';
}
```

**4. Response is sent back to client:**

```json
{
  "message": "This action adds a new building"
}
```

#### Example 2: Fetching All Buildings

**1. Client request:**

```bash
GET http://localhost:3000/buildings
```

**2. Controller handles request:**

```typescript
@Get()
findAll() {
  return this.buildingsService.findAll();
}
```

**3. Service returns data:**

```typescript
findAll() {
  // Fetch all buildings from database
  return [{id: 1, name: "Building A"}, {id: 2, name: "Building B"}];
}
```

**4. JSON response:**

```json
[
  { "id": 1, "name": "Building A" },
  { "id": 2, "name": "Building B" }
]
```

#### Example 3: Managing Workflows

**1. Client creates a workflow:**

```bash
POST http://localhost:3001/workflows
Content-Type: application/json

{
  "name": "Approval Workflow",
  "description": "Multi-stage approval process",
  "steps": ["Review", "Approve", "Deploy"]
}
```

**2. Workflows Controller processes:**

```typescript
@Post()
create(@Body() createWorkflowDto: CreateWorkflowDto) {
  return this.workflowsService.create(createWorkflowDto);
}
```

**3. Service implements business logic:**

```typescript
create(createWorkflowDto: CreateWorkflowDto) {
  // Validate workflow steps
  // Save workflow to database
  // Return created workflow
  return 'This action adds a new workflow';
}
```

### Step 4: Data Flow Diagram 📊

```
┌─────────┐
│ Client  │
└────┬────┘
     │ HTTP Request (POST /buildings)
     ▼
┌─────────────────────────┐
│  BuildingsController    │ ◄── Validates request using DTOs
└────┬────────────────────┘
     │ Calls service method
     ▼
┌─────────────────────────┐
│  BuildingsService       │ ◄── Executes business logic
└────┬────────────────────┘
     │ Returns result
     ▼
┌─────────────────────────┐
│  BuildingsController    │ ◄── Formats response
└────┬────────────────────┘
     │ HTTP Response (JSON)
     ▼
┌─────────┐
│ Client  │
└─────────┘
```

### Step 5: Inter-Service Communication (Future) 🔗

Currently, both services are **independent** and don't communicate. To enable service-to-service communication:

**Option 1: HTTP Communication**

```typescript
// In WorkflowsService, call Virtual Facility Service
async getBuildingForWorkflow(buildingId: number) {
  const response = await fetch(`http://localhost:3000/buildings/${buildingId}`);
  return response.json();
}
```

**Option 2: NestJS Microservices (Recommended)**

```typescript
// Configure TCP transport
const app = await NestFactory.createMicroservice(AppModule, {
  transport: Transport.TCP,
  options: { port: 3002 }
});

// Use ClientProxy to communicate
@Client({ transport: Transport.TCP, options: { port: 3002 } })
client: ClientProxy;

// Send messages
this.client.send('get_building', { id: 1 });
```

## 📡 API Endpoints

### Virtual Facility Service (`http://localhost:3000`)

| Method | Endpoint         | Description            | Request Body      | Response           |
| ------ | ---------------- | ---------------------- | ----------------- | ------------------ |
| GET    | `/`              | Health check / Welcome | -                 | String message     |
| POST   | `/buildings`     | Create a new building  | CreateBuildingDto | Created building   |
| GET    | `/buildings`     | Get all buildings      | -                 | Array of buildings |
| GET    | `/buildings/:id` | Get building by ID     | -                 | Building object    |
| PATCH  | `/buildings/:id` | Update building        | UpdateBuildingDto | Updated building   |
| DELETE | `/buildings/:id` | Delete building        | -                 | Success message    |

**Example Requests:**

```bash
# Create a building
curl -X POST http://localhost:3000/buildings \
  -H "Content-Type: application/json" \
  -d '{"name": "Building A", "address": "123 Main St"}'

# Get all buildings
curl http://localhost:3000/buildings

# Get specific building
curl http://localhost:3000/buildings/1

# Update building
curl -X PATCH http://localhost:3000/buildings/1 \
  -H "Content-Type: application/json" \
  -d '{"name": "Updated Building A"}'

# Delete building
curl -X DELETE http://localhost:3000/buildings/1
```

### Workflows Service (`http://localhost:3001`)

| Method | Endpoint         | Description            | Request Body      | Response           |
| ------ | ---------------- | ---------------------- | ----------------- | ------------------ |
| GET    | `/`              | Health check / Welcome | -                 | String message     |
| POST   | `/workflows`     | Create a new workflow  | CreateWorkflowDto | Created workflow   |
| GET    | `/workflows`     | Get all workflows      | -                 | Array of workflows |
| GET    | `/workflows/:id` | Get workflow by ID     | -                 | Workflow object    |
| PATCH  | `/workflows/:id` | Update workflow        | UpdateWorkflowDto | Updated workflow   |
| DELETE | `/workflows/:id` | Delete workflow        | -                 | Success message    |

**Example Requests:**

```bash
# Create a workflow
curl -X POST http://localhost:3001/workflows \
  -H "Content-Type: application/json" \
  -d '{"name": "Approval Workflow", "steps": ["Review", "Approve"]}'

# Get all workflows
curl http://localhost:3001/workflows

# Get specific workflow
curl http://localhost:3001/workflows/1

# Update workflow
curl -X PATCH http://localhost:3001/workflows/1 \
  -H "Content-Type: application/json" \
  -d '{"name": "Updated Workflow"}'

# Delete workflow
curl -X DELETE http://localhost:3001/workflows/1
```

## 💻 Development

### Running the Services

**Option 1: Run Virtual Facility Service (Default)**

```bash
npm run start:dev
# Runs on http://localhost:3000
```

**Option 2: Run Workflows Service**

```bash
# Using NestJS CLI
nest start workflows-service --watch
# Runs on http://localhost:3001
```

**Option 3: Run Both Services Simultaneously**

Open two terminal windows:

**Terminal 1:**

```bash
npm run start:dev
# Virtual Facility Service running on port 3000
```

**Terminal 2:**

```bash
nest start workflows-service --watch
# Workflows Service running on port 3001
```

### Development Scripts

```bash
# Start in development mode (with watch mode)
npm run start:dev

# Start in debug mode
npm run start:debug

# Build the project
npm run build

# Format code with Prettier
npm run format

# Lint code with ESLint
npm run lint

# Run unit tests
npm run test

# Run tests in watch mode
npm run test:watch

# Run E2E tests
npm run test:e2e

# Generate test coverage
npm run test:cov
```

### Building for Production

```bash
# Build all services
npm run build

# Start in production mode
npm run start:prod
```

## 🧪 Testing

### Unit Tests

Run unit tests for all services:

```bash
npm run test
```

Run tests in watch mode (auto-rerun on changes):

```bash
npm run test:watch
```

### End-to-End (E2E) Tests

Run E2E tests for Virtual Facility Service:

```bash
npm run test:e2e
```

### Test Coverage

Generate test coverage report:

```bash
npm run test:cov
```

This creates a coverage report showing:

- Line coverage
- Branch coverage
- Function coverage
- Statement coverage

## 🛠️ Technologies Used

- **[NestJS](https://nestjs.com/)** - Progressive Node.js framework for building efficient, scalable server-side applications
- **[TypeScript](https://www.typescriptlang.org/)** - Typed superset of JavaScript for better developer experience
- **[Express](https://expressjs.com/)** - Fast, unopinionated web framework (NestJS default platform)
- **[Jest](https://jestjs.io/)** - Delightful JavaScript testing framework
- **[RxJS](https://rxjs.dev/)** - Reactive extensions for JavaScript
- **[ESLint](https://eslint.org/)** - Pluggable linting utility for JavaScript and TypeScript
- **[Prettier](https://prettier.io/)** - Opinionated code formatter
- **[Supertest](https://github.com/visionmedia/supertest)** - HTTP assertions for E2E testing

## 🔍 Key Features

### 1. **Modular Architecture**

- Clear separation of concerns
- Feature-based module organization
- Reusable components across services

### 2. **RESTful API Design**

- Standard HTTP methods (GET, POST, PATCH, DELETE)
- Resource-based URL structure
- Consistent response formats

### 3. **Type Safety**

- Full TypeScript support
- DTOs for request/response validation
- Strongly-typed entities and interfaces

### 4. **Scalability**

- Independent service deployment
- Horizontal scaling capabilities
- Microservices can scale based on demand

### 5. **Testability**

- Unit tests for business logic
- E2E tests for complete user flows
- Dependency injection for easy mocking

### 6. **Developer Experience**

- Hot reload in development mode
- Auto-formatting with Prettier
- Linting with ESLint
- TypeScript IntelliSense support

## 📝 Development Workflow

### Adding a New Feature

**1. Create DTOs** (`dto/` folder):

```typescript
// create-building.dto.ts
export class CreateBuildingDto {
  name: string;
  address: string;
  floors: number;
}
```

**2. Define Entities** (`entities/` folder):

```typescript
// building.entity.ts
export class Building {
  id: number;
  name: string;
  address: string;
  floors: number;
}
```

**3. Implement Service** (`*.service.ts`):

```typescript
// buildings.service.ts
@Injectable()
export class BuildingsService {
  create(createBuildingDto: CreateBuildingDto) {
    // Business logic
  }
}
```

**4. Create Controller Endpoints** (`*.controller.ts`):

```typescript
// buildings.controller.ts
@Controller('buildings')
export class BuildingsController {
  @Post()
  create(@Body() createBuildingDto: CreateBuildingDto) {
    return this.buildingsService.create(createBuildingDto);
  }
}
```

**5. Write Tests** (`*.spec.ts`):

```typescript
// buildings.service.spec.ts
describe('BuildingsService', () => {
  it('should create a building', () => {
    // Test implementation
  });
});
```

### Adding a New Microservice

Use NestJS CLI to generate a new application:

```bash
# Generate new microservice
nest g app <service-name>

# Example: nest g app orders-service
```

This creates:

- New app directory in `apps/<service-name>`
- Main entry point
- Module, controller, and service files
- Test configuration

## 🚀 Next Steps / Roadmap

### Short Term

- [ ] Add database integration (PostgreSQL/MongoDB)
- [ ] Implement data validation using `class-validator`
- [ ] Add proper error handling and logging
- [ ] Create Swagger/OpenAPI documentation

### Medium Term

- [ ] Implement authentication & authorization (JWT)
- [ ] Add API Gateway for unified entry point
- [ ] Set up service-to-service communication
- [ ] Add Docker support for containerization
- [ ] Implement rate limiting and throttling

### Long Term

- [ ] Set up CI/CD pipeline (GitHub Actions/GitLab CI)
- [ ] Add monitoring and observability (Prometheus, Grafana)
- [ ] Implement event-driven architecture
- [ ] Add message queue (RabbitMQ/Redis)
- [ ] Deploy to cloud (AWS, GCP, Azure)
- [ ] Add API versioning
- [ ] Implement caching strategy (Redis)

## 📄 License

UNLICENSED

---

**Built with [NestJS](https://nestjs.com/)** - A progressive Node.js framework

For more information about NestJS:

- [NestJS Documentation](https://docs.nestjs.com)
- [NestJS Discord](https://discord.gg/G7Qnnhy)
- [NestJS Courses](https://courses.nestjs.com/)
