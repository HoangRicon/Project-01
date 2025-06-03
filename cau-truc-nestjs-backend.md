# Cấu Trúc Backend NestJS - Module-Based với Role-Based Access Control

## 📁 Tree View - Cấu Trúc NestJS Backend

```
nestjs-backend/
├── 📄 package.json                          # Dependencies và scripts
├── 📄 package-lock.json                     # Lock file cho npm
├── 📄 tsconfig.json                         # TypeScript configuration
├── 📄 tsconfig.build.json                   # Build-specific TypeScript config
├── 📄 nest-cli.json                         # NestJS CLI configuration
├── 📄 .env                                  # Environment variables
├── 📄 .env.example                          # Environment template
├── 📄 .gitignore                            # Git ignore rules
├── 📄 .eslintrc.js                          # ESLint configuration
├── 📄 .prettierrc                           # Prettier configuration
├── 📄 README.md                             # Project documentation
├── 📄 Dockerfile                            # Docker configuration
├── 📄 docker-compose.yml                    # Docker compose setup
├── 📄 jest.config.js                        # Jest testing configuration
├── 📄 test-setup.ts                         # Test setup file
│
├── 📁 prisma/                               # Prisma ORM (hoặc TypeORM)
│   ├── 📄 schema.prisma                     # Database schema
│   ├── 📁 migrations/                       # Database migrations
│   │   ├── 📄 001_init.sql                  # Initial migration
│   │   ├── 📄 002_add_roles.sql             # Add roles table
│   │   └── 📄 003_add_permissions.sql       # Add permissions table
│   └── 📁 seeds/                            # Database seeders
│       ├── 📄 users.seed.ts                 # User seed data
│       ├── 📄 roles.seed.ts                 # Roles seed data
│       └── 📄 permissions.seed.ts           # Permissions seed data
│
├── 📁 test/                                 # E2E tests
│   ├── 📄 app.e2e-spec.ts                   # App E2E tests
│   ├── 📄 auth.e2e-spec.ts                  # Auth E2E tests
│   ├── 📄 users.e2e-spec.ts                 # Users E2E tests
│   └── 📁 fixtures/                         # Test fixtures
│       ├── 📄 users.fixture.ts              # User test data
│       └── 📄 auth.fixture.ts               # Auth test data
│
├── 📁 docs/                                 # API Documentation
│   ├── 📄 api-spec.yaml                     # OpenAPI/Swagger spec
│   ├── 📄 authentication.md                 # Auth documentation
│   ├── 📄 authorization.md                  # RBAC documentation
│   └── 📄 deployment.md                     # Deployment guide
│
└── 📁 src/                                  # Source code chính
    ├── 📄 main.ts                           # Application entry point
    ├── 📄 app.module.ts                     # Root application module
    ├── 📄 app.controller.ts                 # Root controller
    ├── 📄 app.service.ts                    # Root service
    │
    ├── 📁 config/                           # Configuration modules
    │   ├── 📄 database.config.ts            # Database configuration
    │   ├── 📄 jwt.config.ts                 # JWT configuration
    │   ├── 📄 cors.config.ts                # CORS configuration
    │   ├── 📄 swagger.config.ts             # Swagger configuration
    │   ├── 📄 validation.config.ts          # Validation configuration
    │   └── 📄 index.ts                      # Config exports
    │
    ├── 📁 common/                           # Shared utilities
    │   ├── 📁 decorators/                   # Custom decorators
    │   │   ├── 📄 roles.decorator.ts        # Roles decorator
    │   │   ├── 📄 permissions.decorator.ts  # Permissions decorator
    │   │   ├── 📄 current-user.decorator.ts # Current user decorator
    │   │   ├── 📄 api-response.decorator.ts # API response decorator
    │   │   └── 📄 index.ts                  # Decorator exports
    │   ├── 📁 guards/                       # Authentication & authorization guards
    │   │   ├── 📄 jwt-auth.guard.ts         # JWT authentication guard
    │   │   ├── 📄 roles.guard.ts            # Role-based authorization guard
    │   │   ├── 📄 permissions.guard.ts      # Permission-based authorization guard
    │   │   ├── 📄 resource-owner.guard.ts   # Resource ownership guard
    │   │   └── 📄 index.ts                  # Guard exports
    │   ├── 📁 interceptors/                 # Request/Response interceptors
    │   │   ├── 📄 logging.interceptor.ts    # Request logging
    │   │   ├── 📄 transform.interceptor.ts  # Response transformation
    │   │   ├── 📄 timeout.interceptor.ts    # Request timeout
    │   │   └── 📄 index.ts                  # Interceptor exports
    │   ├── 📁 filters/                      # Exception filters
    │   │   ├── 📄 http-exception.filter.ts  # HTTP exception filter
    │   │   ├── 📄 validation.filter.ts      # Validation exception filter
    │   │   ├── 📄 prisma.filter.ts          # Prisma exception filter
    │   │   └── 📄 index.ts                  # Filter exports
    │   ├── 📁 pipes/                        # Validation pipes
    │   │   ├── 📄 validation.pipe.ts        # Custom validation pipe
    │   │   ├── 📄 parse-int.pipe.ts         # Parse integer pipe
    │   │   ├── 📄 file-validation.pipe.ts   # File validation pipe
    │   │   └── 📄 index.ts                  # Pipe exports
    │   ├── 📁 middleware/                   # Custom middleware
    │   │   ├── 📄 logger.middleware.ts      # Request logger
    │   │   ├── 📄 cors.middleware.ts        # CORS middleware
    │   │   ├── 📄 rate-limit.middleware.ts  # Rate limiting
    │   │   └── 📄 index.ts                  # Middleware exports
    │   ├── 📁 dto/                          # Common DTOs
    │   │   ├── 📄 pagination.dto.ts         # Pagination DTO
    │   │   ├── 📄 response.dto.ts           # Standard response DTO
    │   │   ├── 📄 query.dto.ts              # Query parameters DTO
    │   │   └── 📄 index.ts                  # DTO exports
    │   ├── 📁 types/                        # Common types
    │   │   ├── 📄 user.types.ts             # User-related types
    │   │   ├── 📄 auth.types.ts             # Authentication types
    │   │   ├── 📄 permission.types.ts       # Permission types
    │   │   ├── 📄 response.types.ts         # Response types
    │   │   └── 📄 index.ts                  # Type exports
    │   ├── 📁 utils/                        # Utility functions
    │   │   ├── 📄 password.util.ts          # Password hashing utilities
    │   │   ├── 📄 jwt.util.ts               # JWT utilities
    │   │   ├── 📄 validation.util.ts        # Validation utilities
    │   │   ├── 📄 file.util.ts              # File handling utilities
    │   │   ├── 📄 date.util.ts              # Date utilities
    │   │   ├── 📄 string.util.ts            # String utilities
    │   │   └── 📄 index.ts                  # Utility exports
    │   ├── 📁 constants/                    # Application constants
    │   │   ├── 📄 roles.constant.ts         # Role constants
    │   │   ├── 📄 permissions.constant.ts   # Permission constants
    │   │   ├── 📄 messages.constant.ts      # Message constants
    │   │   ├── 📄 status.constant.ts        # Status constants
    │   │   └── 📄 index.ts                  # Constant exports
    │   └── 📄 index.ts                      # Common exports
    │
    ├── 📁 database/                         # Database module
    │   ├── 📄 database.module.ts            # Database module
    │   ├── 📄 database.service.ts           # Database service
    │   ├── 📄 prisma.service.ts             # Prisma service
    │   ├── 📁 entities/                     # Database entities (TypeORM) hoặc models (Prisma)
    │   │   ├── 📄 user.entity.ts            # User entity
    │   │   ├── 📄 role.entity.ts            # Role entity
    │   │   ├── 📄 permission.entity.ts      # Permission entity
    │   │   ├── 📄 project.entity.ts         # Project entity
    │   │   ├── 📄 audit-log.entity.ts       # Audit log entity
    │   │   └── 📄 index.ts                  # Entity exports
    │   ├── 📁 repositories/                 # Custom repositories
    │   │   ├── 📄 user.repository.ts        # User repository
    │   │   ├── 📄 role.repository.ts        # Role repository
    │   │   ├── 📄 permission.repository.ts  # Permission repository
    │   │   ├── 📄 project.repository.ts     # Project repository
    │   │   └── 📄 index.ts                  # Repository exports
    │   └── 📄 index.ts                      # Database exports
    │
    ├── 📁 modules/                          # Feature modules
    │   ├── 📁 auth/                         # Authentication module
    │   │   ├── 📄 auth.module.ts            # Auth module
    │   │   ├── 📄 auth.controller.ts        # Auth controller
    │   │   ├── 📄 auth.service.ts           # Auth service
    │   │   ├── 📁 dto/                      # Auth DTOs
    │   │   │   ├── 📄 login.dto.ts          # Login DTO
    │   │   │   ├── 📄 register.dto.ts       # Register DTO
    │   │   │   ├── 📄 refresh-token.dto.ts  # Refresh token DTO
    │   │   │   ├── 📄 forgot-password.dto.ts # Forgot password DTO
    │   │   │   ├── 📄 reset-password.dto.ts # Reset password DTO
    │   │   │   └── 📄 index.ts              # DTO exports
    │   │   ├── 📁 strategies/               # Passport strategies
    │   │   │   ├── 📄 jwt.strategy.ts       # JWT strategy
    │   │   │   ├── 📄 local.strategy.ts     # Local strategy
    │   │   │   ├── 📄 google.strategy.ts    # Google OAuth strategy
    │   │   │   └── 📄 index.ts              # Strategy exports
    │   │   ├── 📁 guards/                   # Auth-specific guards
    │   │   │   ├── 📄 local-auth.guard.ts   # Local auth guard
    │   │   │   ├── 📄 google-auth.guard.ts  # Google auth guard
    │   │   │   └── 📄 index.ts              # Guard exports
    │   │   ├── 📄 auth.controller.spec.ts   # Auth controller tests
    │   │   ├── 📄 auth.service.spec.ts      # Auth service tests
    │   │   └── 📄 index.ts                  # Auth module exports
    │   │
    │   ├── 📁 users/                        # User management module
    │   │   ├── 📄 users.module.ts           # Users module
    │   │   ├── 📄 users.controller.ts       # Users controller
    │   │   ├── 📄 users.service.ts          # Users service
    │   │   ├── 📁 dto/                      # User DTOs
    │   │   │   ├── 📄 create-user.dto.ts    # Create user DTO
    │   │   │   ├── 📄 update-user.dto.ts    # Update user DTO
    │   │   │   ├── 📄 user-query.dto.ts     # User query DTO
    │   │   │   ├── 📄 user-response.dto.ts  # User response DTO
    │   │   │   ├── 📄 assign-role.dto.ts    # Assign role DTO
    │   │   │   └── 📄 index.ts              # DTO exports
    │   │   ├── 📄 users.controller.spec.ts  # Users controller tests
    │   │   ├── 📄 users.service.spec.ts     # Users service tests
    │   │   └── 📄 index.ts                  # Users module exports
    │   │
    │   ├── 📁 roles/                        # Role management module
    │   │   ├── 📄 roles.module.ts           # Roles module
    │   │   ├── 📄 roles.controller.ts       # Roles controller
    │   │   ├── 📄 roles.service.ts          # Roles service
    │   │   ├── 📁 dto/                      # Role DTOs
    │   │   │   ├── 📄 create-role.dto.ts    # Create role DTO
    │   │   │   ├── 📄 update-role.dto.ts    # Update role DTO
    │   │   │   ├── 📄 assign-permission.dto.ts # Assign permission DTO
    │   │   │   └── 📄 index.ts              # DTO exports
    │   │   ├── 📄 roles.controller.spec.ts  # Roles controller tests
    │   │   ├── 📄 roles.service.spec.ts     # Roles service tests
    │   │   └── 📄 index.ts                  # Roles module exports
    │   │
    │   ├── 📁 permissions/                  # Permission management module
    │   │   ├── 📄 permissions.module.ts     # Permissions module
    │   │   ├── 📄 permissions.controller.ts # Permissions controller
    │   │   ├── 📄 permissions.service.ts    # Permissions service
    │   │   ├── 📁 dto/                      # Permission DTOs
    │   │   │   ├── 📄 create-permission.dto.ts # Create permission DTO
    │   │   │   ├── 📄 update-permission.dto.ts # Update permission DTO
    │   │   │   └── 📄 index.ts              # DTO exports
    │   │   ├── 📄 permissions.controller.spec.ts # Permissions controller tests
    │   │   ├── 📄 permissions.service.spec.ts # Permissions service tests
    │   │   └── 📄 index.ts                  # Permissions module exports
    │   │
    │   ├── 📁 projects/                     # Project management module
    │   │   ├── 📄 projects.module.ts        # Projects module
    │   │   ├── 📄 projects.controller.ts    # Projects controller
    │   │   ├── 📄 projects.service.ts       # Projects service
    │   │   ├── 📁 dto/                      # Project DTOs
    │   │   │   ├── 📄 create-project.dto.ts # Create project DTO
    │   │   │   ├── 📄 update-project.dto.ts # Update project DTO
    │   │   │   ├── 📄 project-query.dto.ts  # Project query DTO
    │   │   │   ├── 📄 project-response.dto.ts # Project response DTO
    │   │   │   └── 📄 index.ts              # DTO exports
    │   │   ├── 📄 projects.controller.spec.ts # Projects controller tests
    │   │   ├── 📄 projects.service.spec.ts  # Projects service tests
    │   │   └── 📄 index.ts                  # Projects module exports
    │   │
    │   ├── 📁 analytics/                    # Analytics module
    │   │   ├── 📄 analytics.module.ts       # Analytics module
    │   │   ├── 📄 analytics.controller.ts   # Analytics controller
    │   │   ├── 📄 analytics.service.ts      # Analytics service
    │   │   ├── 📁 dto/                      # Analytics DTOs
    │   │   │   ├── 📄 analytics-query.dto.ts # Analytics query DTO
    │   │   │   ├── 📄 analytics-response.dto.ts # Analytics response DTO
    │   │   │   ├── 📄 date-range.dto.ts     # Date range DTO
    │   │   │   └── 📄 index.ts              # DTO exports
    │   │   ├── 📄 analytics.controller.spec.ts # Analytics controller tests
    │   │   ├── 📄 analytics.service.spec.ts # Analytics service tests
    │   │   └── 📄 index.ts                  # Analytics module exports
    │   │
    │   ├── 📁 notifications/                # Notification module
    │   │   ├── 📄 notifications.module.ts   # Notifications module
    │   │   ├── 📄 notifications.controller.ts # Notifications controller
    │   │   ├── 📄 notifications.service.ts  # Notifications service
    │   │   ├── 📁 dto/                      # Notification DTOs
    │   │   │   ├── 📄 create-notification.dto.ts # Create notification DTO
    │   │   │   ├── 📄 notification-query.dto.ts # Notification query DTO
    │   │   │   ├── 📄 mark-read.dto.ts      # Mark as read DTO
    │   │   │   └── 📄 index.ts              # DTO exports
    │   │   ├── 📁 providers/                # Notification providers
    │   │   │   ├── 📄 email.provider.ts     # Email notification provider
    │   │   │   ├── 📄 sms.provider.ts       # SMS notification provider
    │   │   │   ├── 📄 push.provider.ts      # Push notification provider
    │   │   │   └── 📄 index.ts              # Provider exports
    │   │   ├── 📄 notifications.controller.spec.ts # Notifications controller tests
    │   │   ├── 📄 notifications.service.spec.ts # Notifications service tests
    │   │   └── 📄 index.ts                  # Notifications module exports
    │   │
    │   ├── 📁 files/                        # File management module
    │   │   ├── 📄 files.module.ts           # Files module
    │   │   ├── 📄 files.controller.ts       # Files controller
    │   │   ├── 📄 files.service.ts          # Files service
    │   │   ├── 📁 dto/                      # File DTOs
    │   │   │   ├── 📄 upload-file.dto.ts    # Upload file DTO
    │   │   │   ├── 📄 file-query.dto.ts     # File query DTO
    │   │   │   ├── 📄 file-response.dto.ts  # File response DTO
    │   │   │   └── 📄 index.ts              # DTO exports
    │   │   ├── 📁 providers/                # Storage providers
    │   │   │   ├── 📄 local.provider.ts     # Local storage provider
    │   │   │   ├── 📄 s3.provider.ts        # AWS S3 provider
    │   │   │   ├── 📄 cloudinary.provider.ts # Cloudinary provider
    │   │   │   └── 📄 index.ts              # Provider exports
    │   │   ├── 📄 files.controller.spec.ts  # Files controller tests
    │   │   ├── 📄 files.service.spec.ts     # Files service tests
    │   │   └── 📄 index.ts                  # Files module exports
    │   │
    │   ├── 📁 audit/                        # Audit logging module
    │   │   ├── 📄 audit.module.ts           # Audit module
    │   │   ├── 📄 audit.controller.ts       # Audit controller
    │   │   ├── 📄 audit.service.ts          # Audit service
    │   │   ├── 📁 dto/                      # Audit DTOs
    │   │   │   ├── 📄 audit-query.dto.ts    # Audit query DTO
    │   │   │   ├── 📄 audit-response.dto.ts # Audit response DTO
    │   │   │   └── 📄 index.ts              # DTO exports
    │   │   ├── 📁 decorators/               # Audit decorators
    │   │   │   ├── 📄 audit-log.decorator.ts # Audit log decorator
    │   │   │   └── 📄 index.ts              # Decorator exports
    │   │   ├── 📄 audit.controller.spec.ts  # Audit controller tests
    │   │   ├── 📄 audit.service.spec.ts     # Audit service tests
    │   │   └── 📄 index.ts                  # Audit module exports
    │   │
    │   ├── 📁 health/                       # Health check module
    │   │   ├── 📄 health.module.ts          # Health module
    │   │   ├── 📄 health.controller.ts      # Health controller
    │   │   ├── 📄 health.service.ts         # Health service
    │   │   ├── 📄 health.controller.spec.ts # Health controller tests
    │   │   └── 📄 index.ts                  # Health module exports
    │   │
    │   └── 📁 settings/                     # Application settings module
    │       ├── 📄 settings.module.ts        # Settings module
    │       ├── 📄 settings.controller.ts    # Settings controller
    │       ├── 📄 settings.service.ts       # Settings service
    │       ├── 📁 dto/                      # Settings DTOs
    │       │   ├── 📄 update-settings.dto.ts # Update settings DTO
    │       │   ├── 📄 settings-response.dto.ts # Settings response DTO
    │       │   └── 📄 index.ts              # DTO exports
    │       ├── 📄 settings.controller.spec.ts # Settings controller tests
    │       ├── 📄 settings.service.spec.ts  # Settings service tests
    │       └── 📄 index.ts                  # Settings module exports
    │
    ├── 📁 jobs/                             # Background jobs
    │   ├── 📄 jobs.module.ts                # Jobs module
    │   ├── 📁 processors/                   # Job processors
    │   │   ├── 📄 email.processor.ts        # Email job processor
    │   │   ├── 📄 notification.processor.ts # Notification job processor
    │   │   ├── 📄 analytics.processor.ts    # Analytics job processor
    │   │   ├── 📄 cleanup.processor.ts      # Cleanup job processor
    │   │   └── 📄 index.ts                  # Processor exports
    │   ├── 📁 queues/                       # Job queues
    │   │   ├── 📄 email.queue.ts            # Email queue
    │   │   ├── 📄 notification.queue.ts     # Notification queue
    │   │   ├── 📄 analytics.queue.ts        # Analytics queue
    │   │   └── 📄 index.ts                  # Queue exports
    │   └── 📄 index.ts                      # Jobs exports
    │
    └── 📁 shared/                           # Shared services
        ├── 📁 cache/                        # Cache module
        │   ├── 📄 cache.module.ts           # Cache module
        │   ├── 📄 cache.service.ts          # Cache service
        │   ├── 📄 redis.service.ts          # Redis service
        │   └── 📄 index.ts                  # Cache exports
        ├── 📁 logger/                       # Logger module
        │   ├── 📄 logger.module.ts          # Logger module
        │   ├── 📄 logger.service.ts         # Logger service
        │   ├── 📄 winston.config.ts         # Winston configuration
        │   └── 📄 index.ts                  # Logger exports
        ├── 📁 mailer/                       # Email module
        │   ├── 📄 mailer.module.ts          # Mailer module
        │   ├── 📄 mailer.service.ts         # Mailer service
        │   ├── 📁 templates/                # Email templates
        │   │   ├── 📄 welcome.template.ts   # Welcome email template
        │   │   ├── 📄 reset-password.template.ts # Reset password template
        │   │   ├── 📄 notification.template.ts # Notification template
        │   │   └── 📄 index.ts              # Template exports
        │   └── 📄 index.ts                  # Mailer exports
        ├── 📁 storage/                      # Storage module
        │   ├── 📄 storage.module.ts         # Storage module
        │   ├── 📄 storage.service.ts        # Storage service
        │   ├── 📄 s3.service.ts             # S3 service
        │   ├── 📄 cloudinary.service.ts     # Cloudinary service
        │   └── 📄 index.ts                  # Storage exports
        └── 📄 index.ts                      # Shared exports
```

## 📊 **Thống Kê Cấu Trúc NestJS Backend**

### 📁 **Tổng Quan Modules**
- **Core Modules**: 10+ modules (auth, users, roles, permissions, projects, analytics, etc.)
- **Shared Services**: 4 modules (cache, logger, mailer, storage)
- **Background Jobs**: Job processors và queues
- **Database**: Prisma/TypeORM với entities và repositories
- **Common Utilities**: Guards, decorators, interceptors, filters, pipes

### 🎯 **Phân Loại File Theo Loại**

#### **TypeScript Files (.ts)**
- **Controllers**: ~15 files (API endpoints)
- **Services**: ~20 files (business logic)
- **DTOs**: ~40 files (data transfer objects)
- **Entities/Models**: ~10 files (database models)
- **Guards**: ~8 files (authentication & authorization)
- **Decorators**: ~6 files (custom decorators)
- **Utilities**: ~15 files (helper functions)
- **Tests**: ~30 files (unit & integration tests)

#### **Configuration Files**
- **Root Config**: package.json, tsconfig.json, nest-cli.json
- **Database**: schema.prisma, migrations, seeds
- **Testing**: jest.config.js, test-setup.ts
- **Deployment**: Dockerfile, docker-compose.yml

## 🔐 **RBAC Implementation trong NestJS**

### 1. **Permission System**

```typescript
// common/constants/permissions.constant.ts
export enum Permission {
  // User Management
  USER_READ = 'user:read',
  USER_CREATE = 'user:create',
  USER_UPDATE = 'user:update',
  USER_DELETE = 'user:delete',
  USER_MANAGE_ROLES = 'user:manage_roles',

  // Project Management
  PROJECT_READ = 'project:read',
  PROJECT_CREATE = 'project:create',
  PROJECT_UPDATE = 'project:update',
  PROJECT_DELETE = 'project:delete',
  PROJECT_MANAGE = 'project:manage',

  // Analytics
  ANALYTICS_READ = 'analytics:read',
  ANALYTICS_ADVANCED = 'analytics:advanced',
  ANALYTICS_EXPORT = 'analytics:export',

  // System
  SYSTEM_SETTINGS = 'system:settings',
  SYSTEM_LOGS = 'system:logs',
  SYSTEM_BACKUP = 'system:backup',
}

// common/constants/roles.constant.ts
export enum Role {
  ADMIN = 'admin',
  MODERATOR = 'moderator',
  USER = 'user',
}

export const ROLE_PERMISSIONS = {
  [Role.ADMIN]: Object.values(Permission), // Full access
  [Role.MODERATOR]: [
    Permission.USER_READ,
    Permission.PROJECT_READ,
    Permission.PROJECT_UPDATE,
    Permission.ANALYTICS_READ,
  ],
  [Role.USER]: [
    Permission.USER_READ, // Own profile only
    Permission.PROJECT_READ,
    Permission.PROJECT_CREATE,
    Permission.PROJECT_UPDATE, // Own projects only
    Permission.ANALYTICS_READ, // Own data only
  ]
} as const
```

### 2. **Custom Decorators**

```typescript
// common/decorators/roles.decorator.ts
import { SetMetadata } from '@nestjs/common'
import { Role } from '../constants/roles.constant'

export const ROLES_KEY = 'roles'
export const Roles = (...roles: Role[]) => SetMetadata(ROLES_KEY, roles)

// common/decorators/permissions.decorator.ts
import { SetMetadata } from '@nestjs/common'
import { Permission } from '../constants/permissions.constant'

export const PERMISSIONS_KEY = 'permissions'
export const RequirePermissions = (...permissions: Permission[]) =>
  SetMetadata(PERMISSIONS_KEY, permissions)

// common/decorators/current-user.decorator.ts
import { createParamDecorator, ExecutionContext } from '@nestjs/common'

export const CurrentUser = createParamDecorator(
  (data: unknown, ctx: ExecutionContext) => {
    const request = ctx.switchToHttp().getRequest()
    return request.user
  },
)
```

### 3. **Guards Implementation**

```typescript
// common/guards/roles.guard.ts
import { Injectable, CanActivate, ExecutionContext } from '@nestjs/common'
import { Reflector } from '@nestjs/core'
import { Role } from '../constants/roles.constant'
import { ROLES_KEY } from '../decorators/roles.decorator'

@Injectable()
export class RolesGuard implements CanActivate {
  constructor(private reflector: Reflector) {}

  canActivate(context: ExecutionContext): boolean {
    const requiredRoles = this.reflector.getAllAndOverride<Role[]>(ROLES_KEY, [
      context.getHandler(),
      context.getClass(),
    ])

    if (!requiredRoles) {
      return true
    }

    const { user } = context.switchToHttp().getRequest()
    return requiredRoles.some((role) => user.roles?.includes(role))
  }
}

// common/guards/permissions.guard.ts
import { Injectable, CanActivate, ExecutionContext } from '@nestjs/common'
import { Reflector } from '@nestjs/core'
import { Permission, ROLE_PERMISSIONS } from '../constants'
import { PERMISSIONS_KEY } from '../decorators/permissions.decorator'

@Injectable()
export class PermissionsGuard implements CanActivate {
  constructor(private reflector: Reflector) {}

  canActivate(context: ExecutionContext): boolean {
    const requiredPermissions = this.reflector.getAllAndOverride<Permission[]>(
      PERMISSIONS_KEY,
      [context.getHandler(), context.getClass()],
    )

    if (!requiredPermissions) {
      return true
    }

    const { user } = context.switchToHttp().getRequest()
    const userPermissions = this.getUserPermissions(user.roles)

    return requiredPermissions.every((permission) =>
      userPermissions.includes(permission)
    )
  }

  private getUserPermissions(userRoles: string[]): Permission[] {
    const permissions = new Set<Permission>()

    userRoles.forEach((role) => {
      const rolePermissions = ROLE_PERMISSIONS[role as Role] || []
      rolePermissions.forEach((permission) => permissions.add(permission))
    })

    return Array.from(permissions)
  }
}
```

### 4. **Controller Implementation**

```typescript
// modules/users/users.controller.ts
import {
  Controller,
  Get,
  Post,
  Put,
  Delete,
  Body,
  Param,
  Query,
  UseGuards,
} from '@nestjs/common'
import { ApiTags, ApiOperation, ApiBearerAuth } from '@nestjs/swagger'
import { JwtAuthGuard } from '../../common/guards/jwt-auth.guard'
import { RolesGuard } from '../../common/guards/roles.guard'
import { PermissionsGuard } from '../../common/guards/permissions.guard'
import { Roles } from '../../common/decorators/roles.decorator'
import { RequirePermissions } from '../../common/decorators/permissions.decorator'
import { CurrentUser } from '../../common/decorators/current-user.decorator'
import { Role, Permission } from '../../common/constants'
import { UsersService } from './users.service'
import { CreateUserDto, UpdateUserDto, UserQueryDto } from './dto'

@ApiTags('Users')
@ApiBearerAuth()
@Controller('users')
@UseGuards(JwtAuthGuard, RolesGuard, PermissionsGuard)
export class UsersController {
  constructor(private readonly usersService: UsersService) {}

  @Get()
  @ApiOperation({ summary: 'Get all users' })
  @RequirePermissions(Permission.USER_READ)
  async findAll(@Query() query: UserQueryDto) {
    return this.usersService.findAll(query)
  }

  @Get(':id')
  @ApiOperation({ summary: 'Get user by ID' })
  @RequirePermissions(Permission.USER_READ)
  async findOne(@Param('id') id: string, @CurrentUser() currentUser: any) {
    return this.usersService.findOne(id, currentUser)
  }

  @Post()
  @ApiOperation({ summary: 'Create new user' })
  @Roles(Role.ADMIN)
  @RequirePermissions(Permission.USER_CREATE)
  async create(@Body() createUserDto: CreateUserDto) {
    return this.usersService.create(createUserDto)
  }

  @Put(':id')
  @ApiOperation({ summary: 'Update user' })
  @RequirePermissions(Permission.USER_UPDATE)
  async update(
    @Param('id') id: string,
    @Body() updateUserDto: UpdateUserDto,
    @CurrentUser() currentUser: any,
  ) {
    return this.usersService.update(id, updateUserDto, currentUser)
  }

  @Delete(':id')
  @ApiOperation({ summary: 'Delete user' })
  @Roles(Role.ADMIN)
  @RequirePermissions(Permission.USER_DELETE)
  async remove(@Param('id') id: string) {
    return this.usersService.remove(id)
  }

  @Post(':id/assign-role')
  @ApiOperation({ summary: 'Assign role to user' })
  @Roles(Role.ADMIN)
  @RequirePermissions(Permission.USER_MANAGE_ROLES)
  async assignRole(
    @Param('id') id: string,
    @Body() assignRoleDto: { role: Role },
  ) {
    return this.usersService.assignRole(id, assignRoleDto.role)
  }
}
```

### 5. **Service Implementation với RBAC**

```typescript
// modules/users/users.service.ts
import { Injectable, ForbiddenException, NotFoundException } from '@nestjs/common'
import { PrismaService } from '../../database/prisma.service'
import { CreateUserDto, UpdateUserDto, UserQueryDto } from './dto'
import { Role, Permission } from '../../common/constants'
import * as bcrypt from 'bcrypt'

@Injectable()
export class UsersService {
  constructor(private prisma: PrismaService) {}

  async findAll(query: UserQueryDto) {
    const { page = 1, limit = 10, search, role } = query
    const skip = (page - 1) * limit

    const where = {
      ...(search && {
        OR: [
          { name: { contains: search, mode: 'insensitive' } },
          { email: { contains: search, mode: 'insensitive' } },
        ],
      }),
      ...(role && { roles: { some: { name: role } } }),
    }

    const [users, total] = await Promise.all([
      this.prisma.user.findMany({
        where,
        skip,
        take: limit,
        include: {
          roles: {
            include: {
              permissions: true,
            },
          },
        },
        orderBy: { createdAt: 'desc' },
      }),
      this.prisma.user.count({ where }),
    ])

    return {
      data: users.map(user => this.excludePassword(user)),
      meta: {
        page,
        limit,
        total,
        totalPages: Math.ceil(total / limit),
      },
    }
  }

  async findOne(id: string, currentUser: any) {
    const user = await this.prisma.user.findUnique({
      where: { id },
      include: {
        roles: {
          include: {
            permissions: true,
          },
        },
      },
    })

    if (!user) {
      throw new NotFoundException('User not found')
    }

    // Users can only view their own profile unless they have USER_READ permission
    if (currentUser.id !== id && !this.hasPermission(currentUser, Permission.USER_READ)) {
      throw new ForbiddenException('Access denied')
    }

    return this.excludePassword(user)
  }

  async create(createUserDto: CreateUserDto) {
    const { password, roles, ...userData } = createUserDto

    const hashedPassword = await bcrypt.hash(password, 10)

    const user = await this.prisma.user.create({
      data: {
        ...userData,
        password: hashedPassword,
        roles: {
          connect: roles?.map(role => ({ name: role })) || [{ name: Role.USER }],
        },
      },
      include: {
        roles: {
          include: {
            permissions: true,
          },
        },
      },
    })

    return this.excludePassword(user)
  }

  async update(id: string, updateUserDto: UpdateUserDto, currentUser: any) {
    const user = await this.prisma.user.findUnique({ where: { id } })

    if (!user) {
      throw new NotFoundException('User not found')
    }

    // Users can only update their own profile unless they have USER_UPDATE permission
    if (currentUser.id !== id && !this.hasPermission(currentUser, Permission.USER_UPDATE)) {
      throw new ForbiddenException('Access denied')
    }

    const { password, roles, ...userData } = updateUserDto

    const updateData: any = { ...userData }

    if (password) {
      updateData.password = await bcrypt.hash(password, 10)
    }

    if (roles && this.hasPermission(currentUser, Permission.USER_MANAGE_ROLES)) {
      updateData.roles = {
        set: roles.map(role => ({ name: role })),
      }
    }

    const updatedUser = await this.prisma.user.update({
      where: { id },
      data: updateData,
      include: {
        roles: {
          include: {
            permissions: true,
          },
        },
      },
    })

    return this.excludePassword(updatedUser)
  }

  async remove(id: string) {
    const user = await this.prisma.user.findUnique({ where: { id } })

    if (!user) {
      throw new NotFoundException('User not found')
    }

    await this.prisma.user.delete({ where: { id } })

    return { message: 'User deleted successfully' }
  }

  async assignRole(id: string, role: Role) {
    const user = await this.prisma.user.findUnique({ where: { id } })

    if (!user) {
      throw new NotFoundException('User not found')
    }

    const updatedUser = await this.prisma.user.update({
      where: { id },
      data: {
        roles: {
          connect: { name: role },
        },
      },
      include: {
        roles: {
          include: {
            permissions: true,
          },
        },
      },
    })

    return this.excludePassword(updatedUser)
  }

  private excludePassword(user: any) {
    const { password, ...userWithoutPassword } = user
    return userWithoutPassword
  }

  private hasPermission(user: any, permission: Permission): boolean {
    return user.roles?.some((role: any) =>
      role.permissions?.some((perm: any) => perm.name === permission)
    )
  }
}
```

## 🗄️ **Database Schema (Prisma)**

```prisma
// prisma/schema.prisma
generator client {
  provider = "prisma-client-js"
}

datasource db {
  provider = "postgresql"
  url      = env("DATABASE_URL")
}

model User {
  id        String   @id @default(cuid())
  email     String   @unique
  name      String
  password  String
  avatar    String?
  isActive  Boolean  @default(true)
  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt

  // Relationships
  roles     UserRole[]
  projects  ProjectMember[]
  auditLogs AuditLog[]
  notifications Notification[]

  @@map("users")
}

model Role {
  id          String   @id @default(cuid())
  name        String   @unique
  description String?
  createdAt   DateTime @default(now())
  updatedAt   DateTime @updatedAt

  // Relationships
  users       UserRole[]
  permissions RolePermission[]

  @@map("roles")
}

model Permission {
  id          String   @id @default(cuid())
  name        String   @unique
  description String?
  resource    String   // e.g., 'user', 'project', 'analytics'
  action      String   // e.g., 'read', 'create', 'update', 'delete'
  createdAt   DateTime @default(now())
  updatedAt   DateTime @updatedAt

  // Relationships
  roles RolePermission[]

  @@map("permissions")
}

model UserRole {
  id     String @id @default(cuid())
  userId String
  roleId String

  user User @relation(fields: [userId], references: [id], onDelete: Cascade)
  role Role @relation(fields: [roleId], references: [id], onDelete: Cascade)

  @@unique([userId, roleId])
  @@map("user_roles")
}

model RolePermission {
  id           String @id @default(cuid())
  roleId       String
  permissionId String

  role       Role       @relation(fields: [roleId], references: [id], onDelete: Cascade)
  permission Permission @relation(fields: [permissionId], references: [id], onDelete: Cascade)

  @@unique([roleId, permissionId])
  @@map("role_permissions")
}

model Project {
  id          String   @id @default(cuid())
  name        String
  description String?
  status      ProjectStatus @default(ACTIVE)
  createdAt   DateTime @default(now())
  updatedAt   DateTime @updatedAt

  // Relationships
  members ProjectMember[]

  @@map("projects")
}

model ProjectMember {
  id        String      @id @default(cuid())
  projectId String
  userId    String
  role      ProjectRole @default(MEMBER)
  joinedAt  DateTime    @default(now())

  project Project @relation(fields: [projectId], references: [id], onDelete: Cascade)
  user    User    @relation(fields: [userId], references: [id], onDelete: Cascade)

  @@unique([projectId, userId])
  @@map("project_members")
}

model AuditLog {
  id        String   @id @default(cuid())
  userId    String?
  action    String   // e.g., 'CREATE_USER', 'UPDATE_PROJECT'
  resource  String   // e.g., 'User', 'Project'
  resourceId String?
  oldValues Json?
  newValues Json?
  ipAddress String?
  userAgent String?
  createdAt DateTime @default(now())

  user User? @relation(fields: [userId], references: [id], onDelete: SetNull)

  @@map("audit_logs")
}

model Notification {
  id        String             @id @default(cuid())
  userId    String
  title     String
  message   String
  type      NotificationType   @default(INFO)
  isRead    Boolean            @default(false)
  createdAt DateTime           @default(now())

  user User @relation(fields: [userId], references: [id], onDelete: Cascade)

  @@map("notifications")
}

enum ProjectStatus {
  ACTIVE
  INACTIVE
  ARCHIVED
}

enum ProjectRole {
  OWNER
  ADMIN
  MEMBER
  VIEWER
}

enum NotificationType {
  INFO
  SUCCESS
  WARNING
  ERROR
}
```

## 🚀 **Best Practices và Deployment**

### 1. **Environment Configuration**

```bash
# .env
NODE_ENV=development
PORT=3000

# Database
DATABASE_URL="postgresql://username:password@localhost:5432/nestjs_app"

# JWT
JWT_SECRET=your-super-secret-jwt-key
JWT_EXPIRES_IN=7d

# Redis (for caching and sessions)
REDIS_HOST=localhost
REDIS_PORT=6379
REDIS_PASSWORD=

# Email
SMTP_HOST=smtp.gmail.com
SMTP_PORT=587
SMTP_USER=your-email@gmail.com
SMTP_PASS=your-app-password

# File Storage
AWS_ACCESS_KEY_ID=your-aws-access-key
AWS_SECRET_ACCESS_KEY=your-aws-secret-key
AWS_S3_BUCKET=your-s3-bucket
AWS_REGION=us-east-1

# External APIs
GOOGLE_CLIENT_ID=your-google-client-id
GOOGLE_CLIENT_SECRET=your-google-client-secret
```

### 2. **Docker Configuration**

```dockerfile
# Dockerfile
FROM node:18-alpine AS base

# Install dependencies only when needed
FROM base AS deps
RUN apk add --no-cache libc6-compat
WORKDIR /app

COPY package.json package-lock.json* ./
RUN npm ci

# Rebuild the source code only when needed
FROM base AS builder
WORKDIR /app
COPY --from=deps /app/node_modules ./node_modules
COPY . .

# Generate Prisma client
RUN npx prisma generate

# Build the application
RUN npm run build

# Production image, copy all the files and run nest
FROM base AS runner
WORKDIR /app

ENV NODE_ENV production

RUN addgroup --system --gid 1001 nodejs
RUN adduser --system --uid 1001 nestjs

COPY --from=builder /app/dist ./dist
COPY --from=builder /app/node_modules ./node_modules
COPY --from=builder /app/package.json ./package.json
COPY --from=builder /app/prisma ./prisma

USER nestjs

EXPOSE 3000

ENV PORT 3000

CMD ["node", "dist/main"]
```

### 3. **Testing Strategy**

```typescript
// modules/users/users.service.spec.ts
import { Test, TestingModule } from '@nestjs/testing'
import { ForbiddenException, NotFoundException } from '@nestjs/common'
import { UsersService } from './users.service'
import { PrismaService } from '../../database/prisma.service'
import { Role, Permission } from '../../common/constants'

describe('UsersService', () => {
  let service: UsersService
  let prisma: PrismaService

  const mockPrismaService = {
    user: {
      findMany: jest.fn(),
      findUnique: jest.fn(),
      create: jest.fn(),
      update: jest.fn(),
      delete: jest.fn(),
      count: jest.fn(),
    },
  }

  beforeEach(async () => {
    const module: TestingModule = await Test.createTestingModule({
      providers: [
        UsersService,
        {
          provide: PrismaService,
          useValue: mockPrismaService,
        },
      ],
    }).compile()

    service = module.get<UsersService>(UsersService)
    prisma = module.get<PrismaService>(PrismaService)
  })

  describe('findOne', () => {
    it('should return user when user exists and has permission', async () => {
      const mockUser = {
        id: '1',
        name: 'Test User',
        email: 'test@example.com',
        roles: [{ name: Role.USER, permissions: [] }],
      }

      const currentUser = {
        id: '1',
        roles: [{ permissions: [{ name: Permission.USER_READ }] }],
      }

      mockPrismaService.user.findUnique.mockResolvedValue(mockUser)

      const result = await service.findOne('1', currentUser)

      expect(result).toBeDefined()
      expect(result.id).toBe('1')
    })

    it('should throw ForbiddenException when user lacks permission', async () => {
      const mockUser = {
        id: '2',
        name: 'Other User',
        email: 'other@example.com',
        roles: [{ name: Role.USER, permissions: [] }],
      }

      const currentUser = {
        id: '1',
        roles: [{ permissions: [] }], // No permissions
      }

      mockPrismaService.user.findUnique.mockResolvedValue(mockUser)

      await expect(service.findOne('2', currentUser)).rejects.toThrow(
        ForbiddenException,
      )
    })

    it('should throw NotFoundException when user does not exist', async () => {
      const currentUser = {
        id: '1',
        roles: [{ permissions: [{ name: Permission.USER_READ }] }],
      }

      mockPrismaService.user.findUnique.mockResolvedValue(null)

      await expect(service.findOne('999', currentUser)).rejects.toThrow(
        NotFoundException,
      )
    })
  })
})
```

---

**💡 Tip**: Cấu trúc NestJS này cung cấp foundation mạnh mẽ cho enterprise applications với complete RBAC system, scalable architecture, và comprehensive testing strategy!
