# mercur Development Patterns

> Auto-generated skill from repository analysis

## Overview

mercur is a TypeScript-based modular e-commerce framework built with a domain-driven architecture. The codebase follows a clean, modular structure with separate packages for different business domains, workflow orchestration, and notification systems. It emphasizes reusable components, clear separation of concerns, and standardized patterns across modules.

## Coding Conventions

### File Naming
- Use **camelCase** for all TypeScript files
- Examples: `userService.ts`, `orderWorkflow.ts`, `emailTemplate.tsx`

### Import Style
- Use **relative imports** for local modules
- Examples:
```typescript
import { UserModel } from './models/user'
import { validateRequest } from '../middlewares'
import { OrderService } from '../../services/orderService'
```

### Export Style
- Use **named exports** consistently
- Examples:
```typescript
export const UserService = {
  // implementation
}

export interface UserData {
  // interface definition
}

export { UserModel, UserRepository } from './user'
```

### Commit Messages
- Use conventional commit format: `type: description`
- Common prefixes: `fix:`, `feat:`, `chore:`
- Keep messages around 66 characters
- Examples:
  - `feat: add user authentication workflow`
  - `fix: resolve order validation issue`
  - `chore: update package dependencies`

## Workflows

### Release Preparation
**Trigger:** When preparing a new version release  
**Command:** `/release`

1. Update `CHANGELOG.md` with comprehensive release notes
2. Bump version numbers in all `package.json` files across packages
3. Update `README.md` documentation if features changed
4. Create and merge release branch to main
5. Verify all packages are properly versioned

**Files involved:**
- `CHANGELOG.md`
- `package.json` (root and all packages)
- `README.md`

### New API Endpoint
**Trigger:** When adding a new API endpoint  
**Command:** `/api-endpoint`

1. Create `route.ts` file with endpoint implementation:
```typescript
export const GET = async (req: Request) => {
  // endpoint logic
}
```

2. Add authorization/validation middleware in `middlewares.ts`:
```typescript
export const authMiddleware = (req: Request) => {
  // auth logic
}
```

3. Define request/response validators in `validators.ts`:
```typescript
export const createUserValidator = {
  body: z.object({
    name: z.string(),
    email: z.string().email()
  })
}
```

4. Configure query parameters in `query-config.ts`
5. Add OpenAPI specifications for admin endpoints

### New Database Module
**Trigger:** When adding a new database entity/module  
**Command:** `/db-module`

1. Create model classes in `models/` directory:
```typescript
export class UserModel {
  id: string
  name: string
  email: string
}
```

2. Generate migration files with schema changes in `migrations/`
3. Create service class for business logic in `service.ts`:
```typescript
export class UserService {
  async createUser(data: CreateUserData) {
    // business logic
  }
}
```

4. Build workflow steps and orchestration
5. Add module exports in `index.ts`
6. Create links to connect with other modules

### Workflow Implementation
**Trigger:** When implementing complex business processes  
**Command:** `/workflow`

1. Create individual workflow steps in `workflows/*/steps/`:
```typescript
export const validateOrderStep = createStep({
  name: 'validate-order',
  async invoke(input) {
    // step logic
  }
})
```

2. Export steps in `steps/index.ts`:
```typescript
export { validateOrderStep } from './validateOrderStep'
```

3. Build workflow orchestration in `workflows/`:
```typescript
export const orderProcessingWorkflow = createWorkflow({
  name: 'order-processing',
  steps: [validateOrderStep, processPaymentStep]
})
```

4. Add workflow exports in workflow `index.ts`
5. Connect to existing workflows and event handlers

### Notification System
**Trigger:** When adding new email notification functionality  
**Command:** `/notification`

1. Create email template component in `email-templates/`:
```typescript
export const WelcomeEmailTemplate = ({ userName }: Props) => (
  <div>
    <h1>Welcome {userName}!</h1>
  </div>
)
```

2. Add template to `email-templates/index.ts`:
```typescript
export { WelcomeEmailTemplate } from './welcomeEmailTemplate'
```

3. Define template types in `types/templates.ts`:
```typescript
export interface WelcomeEmailData {
  userName: string
  userEmail: string
}
```

4. Create notification subscriber in `subscribers/`:
```typescript
export const userWelcomeSubscriber = async (data) => {
  await sendEmail({
    template: 'welcome',
    data
  })
}
```

5. Hook subscriber to relevant domain events

### Middleware Implementation
**Trigger:** When adding cross-cutting concerns or shared logic  
**Command:** `/middleware`

1. Create middleware function:
```typescript
export const rateLimitMiddleware = (limit: number) => {
  return async (req: Request, next: NextFunction) => {
    // rate limiting logic
    return next()
  }
}
```

2. Add to middleware index exports
3. Apply to relevant API routes in route handlers
4. Add TypeScript types if middleware is complex:
```typescript
export interface MiddlewareConfig {
  enabled: boolean
  options: Record<string, any>
}
```

## Testing Patterns

- Test files follow the pattern `*.test.*`
- Framework: Testing framework not explicitly detected, likely Jest or similar
- Tests should be co-located with source files or in dedicated test directories
- Focus on testing workflow steps, API endpoints, and business logic

## Commands

| Command | Purpose |
|---------|---------|
| `/release` | Prepare and publish a new version release |
| `/api-endpoint` | Create new API endpoint with validation and middleware |
| `/db-module` | Generate new database module with models and migrations |
| `/workflow` | Implement business logic workflows with orchestration |
| `/notification` | Add email notification system with templates |
| `/middleware` | Create reusable middleware for cross-cutting concerns |