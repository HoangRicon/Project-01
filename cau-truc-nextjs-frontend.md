# Cấu Trúc Thư Mục Chi Tiết - Dự Án Frontend Next.js (Module-Based RBAC)

## 📁 Tree View - Cấu Trúc Module-Based với Role-Based Access Control

```
project-root/
├── 📄 package.json                          # Quản lý dependencies và scripts
├── 📄 package-lock.json                     # Lock file cho npm
├── 📄 tsconfig.json                         # Cấu hình TypeScript
├── 📄 next.config.js                        # Cấu hình Next.js
├── 📄 tailwind.config.js                    # Cấu hình Tailwind CSS
├── 📄 .eslintrc.json                        # Cấu hình ESLint
├── 📄 .prettierrc                           # Cấu hình Prettier
├── 📄 .gitignore                            # Git ignore rules
├── 📄 .env.local                            # Environment variables (local)
├── 📄 .env.example                          # Template cho environment variables
├── 📄 README.md                             # Tài liệu dự án
├── 📄 Dockerfile                            # Docker configuration
├── 📄 docker-compose.yml                    # Docker compose setup
├── 📄 vercel.json                           # Vercel deployment config
├── 📄 playwright.config.ts                  # Playwright E2E testing config
├── 📄 jest.config.js                        # Jest unit testing config
├── 📄 jest.setup.js                         # Jest setup file
├── 📄 middleware.ts                         # Next.js middleware
├── 📄 next-env.d.ts                         # Next.js TypeScript declarations
│
├── 📁 public/                               # Static assets
│   ├── 📄 favicon.ico                       # Website icon
│   ├── 📄 logo.svg                          # Logo chính của ứng dụng
│   ├── 📄 manifest.json                     # PWA manifest
│   └── 📁 images/                           # Hình ảnh static
│       ├── 📄 hero-banner.jpg               # Banner trang chủ
│       ├── 📄 placeholder-avatar.png        # Avatar mặc định
│       └── 📄 default-thumbnail.jpg         # Thumbnail mặc định
│
├── 📁 docs/                                 # Tài liệu dự án
│   ├── 📄 api-documentation.md              # Tài liệu API
│   ├── 📄 deployment-guide.md               # Hướng dẫn deploy
│   └── 📄 contributing.md                   # Hướng dẫn contribute
│
├── 📁 scripts/                              # Build và deployment scripts
│   ├── 📄 build.sh                          # Script build production
│   ├── 📄 deploy.sh                         # Script deploy
│   └── 📄 setup.sh                          # Script setup môi trường dev
│
├── 📁 __tests__/                            # Global test files
│   ├── 📄 setup.ts                          # Test setup configuration
│   ├── 📁 __mocks__/                        # Mock files cho testing
│   │   ├── 📄 next-router.ts                # Mock Next.js router
│   │   └── 📄 api-client.ts                 # Mock API client
│   └── 📁 utils/                            # Test utilities
│       ├── 📄 test-utils.tsx                # Custom render functions
│       └── 📄 mock-data.ts                  # Mock data cho tests
│
├── 📁 e2e/                                  # End-to-end tests
│   ├── 📄 auth.spec.ts                      # Test authentication flow
│   ├── 📄 dashboard.spec.ts                 # Test dashboard functionality
│   ├── 📄 user-management.spec.ts           # Test user management
│   └── 📁 fixtures/                         # Test fixtures
│       ├── 📄 users.json                    # Sample user data
│       └── 📄 auth-tokens.json              # Sample auth tokens
│
└── 📁 src/                                  # Source code chính
    ├── 📁 app/                              # Next.js App Router với Role-Based Routes
    │   ├── 📄 globals.css                   # Global styles
    │   ├── 📄 layout.tsx                    # Root layout component
    │   ├── 📄 page.tsx                      # Landing page
    │   ├── 📄 loading.tsx                   # Global loading UI
    │   ├── 📄 error.tsx                     # Global error UI
    │   ├── 📄 not-found.tsx                 # 404 page
    │   │
    │   ├── 📁 (auth)/                       # Authentication routes
    │   │   ├── 📄 layout.tsx                # Auth layout (centered form)
    │   │   ├── 📁 login/                    # Login page
    │   │   │   ├── 📄 page.tsx              # Login page component
    │   │   │   └── 📄 loading.tsx           # Login loading state
    │   │   ├── 📁 register/                 # Register page
    │   │   │   ├── 📄 page.tsx              # Register page component
    │   │   │   └── 📄 loading.tsx           # Register loading state
    │   │   ├── 📁 forgot-password/          # Forgot password page
    │   │   │   └── 📄 page.tsx              # Forgot password component
    │   │   └── 📁 reset-password/           # Reset password page
    │   │       └── 📄 page.tsx              # Reset password component
    │   │
    │   ├── 📁 (admin)/                      # Admin-only routes
    │   │   ├── 📄 layout.tsx                # Admin layout với RouteGuard
    │   │   ├── 📄 page.tsx                  # Admin dashboard
    │   │   ├── 📄 loading.tsx               # Admin loading state
    │   │   │
    │   │   ├── 📁 users/                    # User management (Admin)
    │   │   │   ├── 📄 page.tsx              # All users management
    │   │   │   ├── 📁 create/               # Create user
    │   │   │   │   └── 📄 page.tsx          # Create user form
    │   │   │   └── 📁 [id]/                 # User management
    │   │   │       ├── 📄 page.tsx          # User details
    │   │   │       ├── 📁 edit/             # Edit user
    │   │   │       │   └── 📄 page.tsx      # Edit user form
    │   │   │       └── 📁 roles/            # Manage user roles
    │   │   │           └── 📄 page.tsx      # Role assignment
    │   │   │
    │   │   ├── � system/                   # System management
    │   │   │   ├── 📁 settings/             # System settings
    │   │   │   │   └── 📄 page.tsx          # System configuration
    │   │   │   ├── 📁 logs/                 # System logs
    │   │   │   │   └── 📄 page.tsx          # Audit logs
    │   │   │   └── 📁 backup/               # Backup management
    │   │   │       └── 📄 page.tsx          # Backup & restore
    │   │   │
    │   │   ├── 📁 analytics/                # System analytics
    │   │   │   ├── 📄 page.tsx              # System analytics overview
    │   │   │   ├── 📁 users/                # User analytics
    │   │   │   │   └── 📄 page.tsx          # User behavior analytics
    │   │   │   ├── 📁 performance/          # Performance analytics
    │   │   │   │   └── 📄 page.tsx          # System performance
    │   │   │   └── 📁 reports/              # Advanced reports
    │   │   │       ├── 📄 page.tsx          # Reports listing
    │   │   │       └── 📁 [id]/             # Individual reports
    │   │   │           └── 📄 page.tsx      # Report details
    │   │   │
    │   │   └── 📁 projects/                 # Project management (Admin)
    │   │       ├── 📄 page.tsx              # All projects overview
    │   │       ├── 📁 [id]/                 # Project management
    │   │       │   ├── 📄 page.tsx          # Project admin view
    │   │       │   └── � settings/         # Project settings
    │   │       │       └── �📄 page.tsx      # Project configuration
    │   │       └── 📁 bulk/                 # Bulk operations
    │   │           └── 📄 page.tsx          # Bulk project actions
    │   │
    │   ├── 📁 (moderator)/                  # Moderator routes
    │   │   ├── 📄 layout.tsx                # Moderator layout với RouteGuard
    │   │   ├── 📄 page.tsx                  # Moderator dashboard
    │   │   ├── 📄 loading.tsx               # Moderator loading state
    │   │   │
    │   │   ├── 📁 moderation/               # Content moderation
    │   │   │   ├── 📁 queue/                # Moderation queue
    │   │   │   │   └── 📄 page.tsx          # Pending items
    │   │   │   ├── 📁 reports/              # User reports
    │   │   │   │   ├── 📄 page.tsx          # Reports listing
    │   │   │   │   └── 📁 [id]/             # Individual reports
    │   │   │   │       └── 📄 page.tsx      # Report details
    │   │   │   └── 📁 actions/              # Moderation actions
    │   │   │       └── 📄 page.tsx          # Action history
    │   │   │
    │   │   ├── 📁 content/                  # Content management
    │   │   │   ├── 📁 review/               # Content review
    │   │   │   │   └── 📄 page.tsx          # Review queue
    │   │   │   ├── 📁 approve/              # Content approval
    │   │   │   │   └── 📄 page.tsx          # Approval queue
    │   │   │   └── 📁 flagged/              # Flagged content
    │   │   │       └── � page.tsx          # Flagged items
    │   │   │
    │   │   └── 📁 analytics/                # Moderation analytics
    │   │       ├── 📁 moderation/           # Moderation metrics
    │   │       │   └── 📄 page.tsx          # Moderation statistics
    │   │       └── 📁 content/              # Content metrics
    │   │           └── 📄 page.tsx          # Content statistics
    │   │
    │   ├── 📁 (user)/                       # User routes
    │   │   ├── 📄 layout.tsx                # User layout với RouteGuard
    │   │   ├── 📄 page.tsx                  # User dashboard
    │   │   ├── 📄 loading.tsx               # User loading state
    │   │   │
    │   │   ├── 📁 profile/                  # User profile
    │   │   │   ├── 📄 page.tsx              # View profile
    │   │   │   ├── 📁 edit/                 # Edit profile
    │   │   │   │   └── 📄 page.tsx          # Edit profile form
    │   │   │   └── 📁 settings/             # Profile settings
    │   │   │       ├── 📄 page.tsx          # Settings overview
    │   │   │       ├── 📁 security/         # Security settings
    │   │   │       │   └── 📄 page.tsx      # Password & security
    │   │   │       ├── 📁 privacy/          # Privacy settings
    │   │   │       │   └── 📄 page.tsx      # Privacy preferences
    │   │   │       └── 📁 notifications/    # Notification settings
    │   │   │           └── 📄 page.tsx      # Notification preferences
    │   │   │
    │   │   ├── 📁 projects/                 # User projects
    │   │   │   ├── 📄 page.tsx              # My projects listing
    │   │   │   ├── � create/               # Create project
    │   │   │   │   └── 📄 page.tsx          # Create project form
    │   │   │   └── 📁 [id]/                 # Project management
    │   │   │       ├── � page.tsx          # Project details
    │   │   │       ├── 📁 edit/             # Edit project
    │   │   │       │   └── 📄 page.tsx      # Edit project form
    │   │   │       └── 📁 collaborate/      # Project collaboration
    │   │   │           └── 📄 page.tsx      # Collaboration settings
    │   │   │
    │   │   └── 📁 analytics/                # Personal analytics
    │   │       └── 📁 personal/             # Personal metrics
    │   │           ├── 📄 page.tsx          # Personal dashboard
    │   │           ├── � activity/         # Activity metrics
    │   │           │   └── �📄 page.tsx      # Activity overview
    │   │           └── 📁 projects/         # Project metrics
    │   │               └── 📄 page.tsx      # Project analytics
    │   │
    │   ├── 📁 (shared)/                     # Shared routes (all roles)
    │   │   ├── 📁 notifications/            # Notifications
    │   │   │   ├── 📄 page.tsx              # Notifications list
    │   │   │   └── 📁 settings/             # Notification settings
    │   │   │       └── 📄 page.tsx          # Notification preferences
    │   │   ├── 📁 help/                     # Help & documentation
    │   │   │   ├── 📄 page.tsx              # Help center
    │   │   │   ├── 📁 guides/               # User guides
    │   │   │   │   └── 📄 page.tsx          # Guides listing
    │   │   │   └── 📁 faq/                  # FAQ
    │   │   │       └── 📄 page.tsx          # FAQ page
    │   │   └── 📁 support/                  # Support
    │   │       ├── 📄 page.tsx              # Support center
    │   │       ├── 📁 tickets/              # Support tickets
    │   │       │   ├── 📄 page.tsx          # Tickets listing
    │   │       │   ├── 📁 create/           # Create ticket
    │   │       │   │   └── 📄 page.tsx      # Create ticket form
    │   │       │   └── 📁 [id]/             # Ticket details
    │   │       │       └── 📄 page.tsx      # Ticket view
    │   │       └── 📁 contact/              # Contact support
    │   │           └── 📄 page.tsx          # Contact form
    │   │
    │   └── 📁 access-denied/                # Access denied page
    │       └── 📄 page.tsx                  # Access denied component
    │
    ├── 📁 components/                       # Shared components across modules
    │   ├── 📁 ui/                           # shadcn/ui base components
    │   │   ├── 📄 button.tsx                # Button component
    │   │   ├── 📄 input.tsx                 # Input component
    │   │   ├── 📄 dialog.tsx                # Dialog/Modal component
    │   │   ├── 📄 dropdown-menu.tsx         # Dropdown menu component
    │   │   ├── 📄 toast.tsx                 # Toast notification component
    │   │   ├── 📄 card.tsx                  # Card component
    │   │   ├── 📄 table.tsx                 # Table component
    │   │   ├── 📄 form.tsx                  # Form components
    │   │   ├── 📄 badge.tsx                 # Badge component
    │   │   ├── 📄 avatar.tsx                # Avatar component
    │   │   ├── 📄 skeleton.tsx              # Loading skeleton
    │   │   ├── 📄 tabs.tsx                  # Tabs component
    │   │   ├── 📄 select.tsx                # Select dropdown
    │   │   ├── 📄 checkbox.tsx              # Checkbox component
    │   │   ├── 📄 radio-group.tsx           # Radio group component
    │   │   ├── 📄 switch.tsx                # Toggle switch
    │   │   ├── 📄 textarea.tsx              # Textarea component
    │   │   ├── 📄 label.tsx                 # Label component
    │   │   ├── 📄 separator.tsx             # Divider/Separator
    │   │   ├── 📄 progress.tsx              # Progress bar
    │   │   ├── 📄 alert.tsx                 # Alert component
    │   │   ├── 📄 popover.tsx               # Popover component
    │   │   ├── 📄 tooltip.tsx               # Tooltip component
    │   │   └── 📄 index.ts                  # Barrel exports cho ui components
    │   │
    │   ├── 📁 layout/                       # Layout components với RBAC
    │   │   ├── � shared/                   # Shared layout components
    │   │   │   ├── 📄 Header.tsx            # Base header component
    │   │   │   ├── 📄 Footer.tsx            # Footer component
    │   │   │   ├── 📄 Navigation.tsx        # Base navigation
    │   │   │   ├── 📄 Breadcrumb.tsx        # Breadcrumb navigation
    │   │   │   ├── 📄 UserMenu.tsx          # User dropdown menu
    │   │   │   ├── 📄 NotificationBell.tsx  # Notification icon + dropdown
    │   │   │   ├── 📄 ThemeToggle.tsx       # Dark/Light mode toggle
    │   │   │   └── 📄 MobileMenu.tsx        # Mobile hamburger menu
    │   │   ├── � admin/                    # Admin-specific layouts
    │   │   │   ├── � AdminLayout.tsx       # Admin main layout
    │   │   │   ├── 📄 AdminSidebar.tsx      # Admin sidebar navigation
    │   │   │   ├── 📄 AdminHeader.tsx       # Admin header với admin tools
    │   │   │   └── 📄 AdminBreadcrumb.tsx   # Admin breadcrumb
    │   │   ├── � moderator/                # Moderator-specific layouts
    │   │   │   ├── 📄 ModeratorLayout.tsx   # Moderator main layout
    │   │   │   ├── 📄 ModeratorSidebar.tsx  # Moderator sidebar
    │   │   │   ├── 📄 ModeratorHeader.tsx   # Moderator header
    │   │   │   └── 📄 ModerationQueue.tsx   # Quick moderation access
    │   │   ├── � user/                     # User-specific layouts
    │   │   │   ├── 📄 UserLayout.tsx        # User main layout
    │   │   │   ├── 📄 UserSidebar.tsx       # User sidebar navigation
    │   │   │   ├── 📄 UserHeader.tsx        # User header
    │   │   │   └── 📄 PersonalMenu.tsx      # Personal menu items
    │   │   └── 📄 index.ts                  # Barrel exports cho layout
    │   │
    │   ├── 📁 common/                       # Common shared components
    │   │   ├── 📄 LoadingSpinner.tsx        # Loading spinner component
    │   │   ├── 📄 ErrorBoundary.tsx         # Error boundary wrapper
    │   │   ├── 📄 ConfirmDialog.tsx         # Confirmation dialog
    │   │   ├── 📄 EmptyState.tsx            # Empty state placeholder
    │   │   ├── 📄 DataTable.tsx             # Reusable data table
    │   │   ├── 📄 Pagination.tsx            # Pagination component
    │   │   ├── 📄 SearchInput.tsx           # Search input với debounce
    │   │   ├── 📄 FileUpload.tsx            # File upload component
    │   │   ├── 📄 ImageUpload.tsx           # Image upload với preview
    │   │   ├── 📄 DatePicker.tsx            # Date picker component
    │   │   ├── 📄 RichTextEditor.tsx        # Rich text editor
    │   │   ├── 📄 CodeBlock.tsx             # Code syntax highlighting
    │   │   ├── 📄 CopyToClipboard.tsx       # Copy to clipboard button
    │   │   ├── 📄 ShareButton.tsx           # Social sharing button
    │   │   ├── 📄 PermissionGuard.tsx       # Permission-based component guard
    │   │   ├── 📄 RoleBasedRenderer.tsx     # Role-based conditional rendering
    │   │   ├── 📄 AccessDenied.tsx          # Access denied component
    │   │   └── 📄 index.ts                  # Barrel exports cho common
    │   │
    │   └── 📁 forms/                        # Shared form components
    │       ├── 📄 BaseForm.tsx              # Base form component
    │       ├── 📄 FormField.tsx             # Reusable form field
    │       ├── 📄 FormValidation.tsx        # Form validation component
    │       ├── 📄 SearchForm.tsx            # Search form component
    │       ├── 📄 FilterForm.tsx            # Filter form component
    │       └── 📄 index.ts                  # Barrel exports cho forms
    │
    ├── 📁 modules/                          # Module-based organization
    │   ├── 📁 auth/                         # Authentication module
    │   │   ├── 📁 components/               # Auth-specific components
    │   │   │   ├── 📄 LoginForm.tsx         # Login form
    │   │   │   ├── 📄 RegisterForm.tsx      # Register form
    │   │   │   ├── 📄 LoginCard.tsx         # Login card wrapper
    │   │   │   ├── 📄 RegisterCard.tsx      # Register card wrapper
    │   │   │   ├── 📄 SocialLogin.tsx       # Social login buttons
    │   │   │   ├── 📄 PasswordStrength.tsx  # Password strength indicator
    │   │   │   ├── 📄 RoleSelector.tsx      # Role selection component
    │   │   │   ├── 📄 AuthGuard.tsx         # Route protection component
    │   │   │   └── 📄 index.ts              # Barrel exports
    │   │   ├── 📁 hooks/                    # Auth-specific hooks
    │   │   │   ├── 📄 useAuth.ts            # Authentication hook
    │   │   │   ├── 📄 useLogin.ts           # Login hook
    │   │   │   ├── 📄 useRegister.ts        # Register hook
    │   │   │   ├── 📄 usePermissions.ts     # Permissions hook
    │   │   │   ├── 📄 useRoleCheck.ts       # Role checking hook
    │   │   │   └── 📄 index.ts              # Barrel exports
    │   │   ├── 📁 types/                    # Auth-specific types
    │   │   │   ├── 📄 auth.types.ts         # Authentication types
    │   │   │   ├── 📄 role.types.ts         # Role types
    │   │   │   ├── 📄 permission.types.ts   # Permission types
    │   │   │   └── 📄 index.ts              # Type exports
    │   │   ├── 📁 store/                    # Auth-specific stores
    │   │   │   ├── 📄 authStore.ts          # Authentication store
    │   │   │   ├── 📄 permissionStore.ts    # Permission store
    │   │   │   └── 📄 index.ts              # Store exports
    │   │   ├── 📁 utils/                    # Auth utilities
    │   │   │   ├── 📄 roleUtils.ts          # Role utility functions
    │   │   │   ├── 📄 permissionUtils.ts    # Permission utilities
    │   │   │   ├── 📄 tokenUtils.ts         # Token management
    │   │   │   └── 📄 index.ts              # Utility exports
    │   │   └── 📄 index.ts                  # Module exports
    │   │
    │   ├── 📁 admin/                        # Admin module
    │   │   ├── 📁 components/               # Admin-specific components
    │   │   │   ├── 📄 AdminDashboard.tsx    # Admin dashboard
    │   │   │   ├── 📄 SystemSettings.tsx    # System settings
    │   │   │   ├── 📄 UserManagement.tsx    # User management
    │   │   │   ├── 📄 RoleManagement.tsx    # Role management
    │   │   │   ├── 📄 AuditLogs.tsx         # Audit logs viewer
    │   │   │   ├── 📄 SystemMetrics.tsx     # System metrics
    │   │   │   ├── 📄 BackupManager.tsx     # Backup management
    │   │   │   └── 📄 index.ts              # Barrel exports
    │   │   ├── 📁 hooks/                    # Admin-specific hooks
    │   │   │   ├── 📄 useAdminData.ts       # Admin data hook
    │   │   │   ├── 📄 useSystemStats.ts     # System statistics hook
    │   │   │   ├── 📄 useAuditLogs.ts       # Audit logs hook
    │   │   │   ├── 📄 useUserManagement.ts  # User management hook
    │   │   │   └── 📄 index.ts              # Barrel exports
    │   │   ├── 📁 types/                    # Admin-specific types
    │   │   │   ├── 📄 admin.types.ts        # Admin types
    │   │   │   ├── 📄 system.types.ts       # System types
    │   │   │   └── 📄 index.ts              # Type exports
    │   │   ├── 📁 store/                    # Admin-specific stores
    │   │   │   ├── 📄 adminStore.ts         # Admin store
    │   │   │   └── 📄 index.ts              # Store exports
    │   │   └── 📄 index.ts                  # Module exports
    │   │
    │   ├── 📁 user-management/              # User management module
    │   │   ├── 📁 components/               # User management components
    │   │   │   ├── 📁 shared/               # Shared across roles
    │   │   │   │   ├── 📄 UserCard.tsx      # User card component
    │   │   │   │   ├── 📄 UserAvatar.tsx    # User avatar component
    │   │   │   │   ├── 📄 UserStatus.tsx    # User status badge
    │   │   │   │   ├── 📄 UserProfile.tsx   # User profile view
    │   │   │   │   └── 📄 index.ts          # Shared exports
    │   │   │   ├── 📁 admin/                # Admin-specific components
    │   │   │   │   ├── 📄 UserTable.tsx     # Admin user table
    │   │   │   │   ├── 📄 UserActions.tsx   # Admin user actions
    │   │   │   │   ├── 📄 BulkActions.tsx   # Bulk user operations
    │   │   │   │   ├── 📄 UserRoleEditor.tsx # Role assignment
    │   │   │   │   └── 📄 index.ts          # Admin exports
    │   │   │   ├── 📁 moderator/            # Moderator-specific components
    │   │   │   │   ├── 📄 UserModeration.tsx # User moderation tools
    │   │   │   │   ├── 📄 ReportActions.tsx # Report handling
    │   │   │   │   ├── 📄 UserReports.tsx   # User reports view
    │   │   │   │   └── 📄 index.ts          # Moderator exports
    │   │   │   ├── 📁 user/                 # User-specific components
    │   │   │   │   ├── 📄 ProfileView.tsx   # Own profile view
    │   │   │   │   ├── 📄 ProfileEdit.tsx   # Edit own profile
    │   │   │   │   ├── 📄 PasswordChange.tsx # Change password
    │   │   │   │   └── 📄 index.ts          # User exports
    │   │   │   └── 📄 index.ts              # All components export
    │   │   ├── 📁 hooks/                    # User management hooks
    │   │   │   ├── 📄 useUsers.ts           # Users data hook
    │   │   │   ├── 📄 useUserActions.ts     # User actions hook
    │   │   │   ├── 📄 useUserPermissions.ts # User permissions hook
    │   │   │   ├── 📄 useUserProfile.ts     # User profile hook
    │   │   │   └── 📄 index.ts              # Hook exports
    │   │   ├── 📁 types/                    # User management types
    │   │   │   ├── 📄 user.types.ts         # User types
    │   │   │   ├── 📄 profile.types.ts      # Profile types
    │   │   │   └── 📄 index.ts              # Type exports
    │   │   ├── 📁 store/                    # User management stores
    │   │   │   ├── 📄 userStore.ts          # User store
    │   │   │   ├── 📄 profileStore.ts       # Profile store
    │   │   │   └── 📄 index.ts              # Store exports
    │   │   └── 📄 index.ts                  # Module exports
    │   │
    │   ├── 📁 dashboard/                    # Dashboard module
    │   │   ├── 📁 components/               # Dashboard components
    │   │   │   ├── 📁 shared/               # Shared dashboard components
    │   │   │   │   ├── 📄 StatsCard.tsx     # Statistics card
    │   │   │   │   ├── 📄 ChartWidget.tsx   # Chart widget
    │   │   │   │   ├── 📄 RecentActivity.tsx # Recent activity list
    │   │   │   │   ├── 📄 QuickActions.tsx  # Quick action buttons
    │   │   │   │   └── 📄 index.ts          # Shared exports
    │   │   │   ├── 📁 admin/                # Admin dashboard components
    │   │   │   │   ├── 📄 AdminDashboard.tsx # Admin dashboard
    │   │   │   │   ├── 📄 SystemOverview.tsx # System overview
    │   │   │   │   ├── 📄 AdminMetrics.tsx  # Admin metrics
    │   │   │   │   ├── 📄 UserMetrics.tsx   # User statistics
    │   │   │   │   └── 📄 index.ts          # Admin exports
    │   │   │   ├── 📁 moderator/            # Moderator dashboard components
    │   │   │   │   ├── 📄 ModeratorDashboard.tsx # Moderator dashboard
    │   │   │   │   ├── 📄 ModerationQueue.tsx # Moderation queue
    │   │   │   │   ├── 📄 ModeratorMetrics.tsx # Moderation metrics
    │   │   │   │   └── 📄 index.ts          # Moderator exports
    │   │   │   ├── 📁 user/                 # User dashboard components
    │   │   │   │   ├── 📄 UserDashboard.tsx # User dashboard
    │   │   │   │   ├── 📄 PersonalStats.tsx # Personal statistics
    │   │   │   │   ├── 📄 WelcomeBanner.tsx # Welcome banner
    │   │   │   │   └── 📄 index.ts          # User exports
    │   │   │   └── 📄 index.ts              # All components export
    │   │   ├── 📁 hooks/                    # Dashboard hooks
    │   │   │   ├── 📄 useDashboardData.ts   # Dashboard data hook
    │   │   │   ├── 📄 useRoleBasedMetrics.ts # Role-based metrics
    │   │   │   ├── 📄 usePersonalization.ts # Dashboard personalization
    │   │   │   └── 📄 index.ts              # Hook exports
    │   │   ├── 📁 types/                    # Dashboard types
    │   │   │   ├── 📄 dashboard.types.ts    # Dashboard types
    │   │   │   ├── 📄 metrics.types.ts      # Metrics types
    │   │   │   └── 📄 index.ts              # Type exports
    │   │   ├── 📁 store/                    # Dashboard stores
    │   │   │   ├── 📄 dashboardStore.ts     # Dashboard store
    │   │   │   └── 📄 index.ts              # Store exports
    │   │   └── 📄 index.ts                  # Module exports
    │   │
    │   ├── 📁 analytics/                    # Analytics module
    │   │   ├── 📁 components/               # Analytics components
    │   │   │   ├── 📁 shared/               # Shared analytics components
    │   │   │   │   ├── 📄 AnalyticsChart.tsx # Analytics chart
    │   │   │   │   ├── 📄 MetricsCard.tsx   # Metrics display card
    │   │   │   │   ├── 📄 DateRangePicker.tsx # Date range selector
    │   │   │   │   ├── 📄 ExportButton.tsx  # Data export button
    │   │   │   │   ├── 📄 FilterPanel.tsx   # Analytics filter panel
    │   │   │   │   └── 📄 index.ts          # Shared exports
    │   │   │   ├── 📁 admin/                # Admin analytics components
    │   │   │   │   ├── 📄 SystemAnalytics.tsx # System analytics
    │   │   │   │   ├── 📄 UserAnalytics.tsx # User analytics
    │   │   │   │   ├── 📄 PerformanceMetrics.tsx # Performance metrics
    │   │   │   │   ├── 📄 AdvancedReports.tsx # Advanced reports
    │   │   │   │   └── 📄 index.ts          # Admin exports
    │   │   │   ├── 📁 moderator/            # Moderator analytics components
    │   │   │   │   ├── 📄 ModerationAnalytics.tsx # Moderation analytics
    │   │   │   │   ├── 📄 ContentMetrics.tsx # Content metrics
    │   │   │   │   └── 📄 index.ts          # Moderator exports
    │   │   │   ├── 📁 user/                 # User analytics components
    │   │   │   │   ├── 📄 PersonalAnalytics.tsx # Personal analytics
    │   │   │   │   ├── 📄 ActivityMetrics.tsx # Activity metrics
    │   │   │   │   └── 📄 index.ts          # User exports
    │   │   │   └── 📄 index.ts              # All components export
    │   │   ├── 📁 hooks/                    # Analytics hooks
    │   │   │   ├── 📄 useAnalytics.ts       # Analytics data hook
    │   │   │   ├── 📄 useRoleBasedAnalytics.ts # Role-based analytics
    │   │   │   ├── 📄 useAnalyticsPermissions.ts # Analytics permissions
    │   │   │   ├── 📄 useExportData.ts      # Data export hook
    │   │   │   └── 📄 index.ts              # Hook exports
    │   │   ├── 📁 types/                    # Analytics types
    │   │   │   ├── 📄 analytics.types.ts    # Analytics types
    │   │   │   ├── 📄 chart.types.ts        # Chart types
    │   │   │   └── 📄 index.ts              # Type exports
    │   │   ├── 📁 store/                    # Analytics stores
    │   │   │   ├── 📄 analyticsStore.ts     # Analytics store
    │   │   │   └── 📄 index.ts              # Store exports
    │   │   └── 📄 index.ts                  # Module exports
    │   │
    │   ├── 📁 projects/                     # Projects module
    │   │   ├── 📁 components/               # Project components
    │   │   │   ├── 📁 shared/               # Shared project components
    │   │   │   │   ├── 📄 ProjectCard.tsx   # Project card
    │   │   │   │   ├── 📄 ProjectStatus.tsx # Project status
    │   │   │   │   ├── 📄 ProjectProgress.tsx # Project progress
    │   │   │   │   ├── 📄 ProjectDetails.tsx # Project details
    │   │   │   │   └── 📄 index.ts          # Shared exports
    │   │   │   ├── 📁 admin/                # Admin project components
    │   │   │   │   ├── 📄 ProjectManagement.tsx # Project management
    │   │   │   │   ├── 📄 ProjectSettings.tsx # Project settings
    │   │   │   │   ├── 📄 ProjectAnalytics.tsx # Project analytics
    │   │   │   │   └── 📄 index.ts          # Admin exports
    │   │   │   ├── 📁 moderator/            # Moderator project components
    │   │   │   │   ├── 📄 ProjectModeration.tsx # Project moderation
    │   │   │   │   ├── 📄 ProjectReview.tsx # Project review
    │   │   │   │   └── 📄 index.ts          # Moderator exports
    │   │   │   ├── 📁 user/                 # User project components
    │   │   │   │   ├── 📄 MyProjects.tsx    # My projects
    │   │   │   │   ├── 📄 ProjectCreate.tsx # Create project
    │   │   │   │   ├── 📄 ProjectEdit.tsx   # Edit project
    │   │   │   │   ├── 📄 ProjectForm.tsx   # Project form
    │   │   │   │   └── 📄 index.ts          # User exports
    │   │   │   └── 📄 index.ts              # All components export
    │   │   ├── 📁 hooks/                    # Project hooks
    │   │   │   ├── 📄 useProjects.ts        # Projects data hook
    │   │   │   ├── 📄 useProjectActions.ts  # Project actions hook
    │   │   │   ├── 📄 useProjectPermissions.ts # Project permissions
    │   │   │   └── 📄 index.ts              # Hook exports
    │   │   ├── 📁 types/                    # Project types
    │   │   │   ├── 📄 project.types.ts      # Project types
    │   │   │   └── 📄 index.ts              # Type exports
    │   │   ├── 📁 store/                    # Project stores
    │   │   │   ├── 📄 projectStore.ts       # Project store
    │   │   │   └── 📄 index.ts              # Store exports
    │   │   └── 📄 index.ts                  # Module exports
    │   │
    │   └── 📁 settings/                     # Settings module
    │       ├── 📁 components/               # Settings components
    │       │   ├── 📁 shared/               # Shared settings components
    │       │   │   ├── 📄 ProfileSettings.tsx # Profile settings
    │       │   │   ├── 📄 SecuritySettings.tsx # Security settings
    │       │   │   ├── 📄 NotificationSettings.tsx # Notification settings
    │       │   │   ├── 📄 PrivacySettings.tsx # Privacy settings
    │       │   │   └── 📄 index.ts          # Shared exports
    │       │   ├── 📁 admin/                # Admin settings components
    │       │   │   ├── 📄 SystemSettings.tsx # System settings
    │       │   │   ├── 📄 GlobalSettings.tsx # Global settings
    │       │   │   ├── 📄 SecurityPolicies.tsx # Security policies
    │       │   │   ├── 📄 BackupSettings.tsx # Backup settings
    │       │   │   └── 📄 index.ts          # Admin exports
    │       │   ├── 📁 moderator/            # Moderator settings components
    │       │   │   ├── 📄 ModerationSettings.tsx # Moderation settings
    │       │   │   ├── 📄 ContentPolicies.tsx # Content policies
    │       │   │   └── 📄 index.ts          # Moderator exports
    │       │   ├── 📁 user/                 # User settings components
    │       │   │   ├── 📄 PersonalSettings.tsx # Personal settings
    │       │   │   ├── 📄 AccountSettings.tsx # Account settings
    │       │   │   └── 📄 index.ts          # User exports
    │       │   └── 📄 index.ts              # All components export
    │       ├── 📁 hooks/                    # Settings hooks
    │       │   ├── 📄 useSettings.ts        # Settings hook
    │       │   ├── 📄 useRoleBasedSettings.ts # Role-based settings
    │       │   ├── 📄 useSettingsPermissions.ts # Settings permissions
    │       │   └── 📄 index.ts              # Hook exports
    │       ├── 📁 types/                    # Settings types
    │       │   ├── 📄 settings.types.ts     # Settings types
    │       │   └── 📄 index.ts              # Type exports
    │       ├── 📁 store/                    # Settings stores
    │       │   ├── 📄 settingsStore.ts      # Settings store
    │       │   └── 📄 index.ts              # Store exports
    │       └── 📄 index.ts                  # Module exports
    │
    ├── 📁 hooks/                            # Global custom React hooks
    │   ├── 📄 useAuth.ts                    # Authentication hook
    │   ├── 📄 useLocalStorage.ts            # Local storage hook
    │   ├── 📄 useSessionStorage.ts          # Session storage hook
    │   ├── 📄 useDebounce.ts                # Debounce hook
    │   ├── 📄 useThrottle.ts                # Throttle hook
    │   ├── 📄 useToggle.ts                  # Toggle state hook
    │   ├── 📄 useClickOutside.ts            # Click outside detection
    │   ├── 📄 useKeyPress.ts                # Keyboard event hook
    │   ├── 📄 useWindowSize.ts              # Window size hook
    │   ├── 📄 useMediaQuery.ts              # Media query hook
    │   ├── 📄 useCopyToClipboard.ts         # Copy to clipboard hook
    │   ├── 📄 useIntersectionObserver.ts    # Intersection observer hook
    │   ├── 📄 usePagination.ts              # Pagination logic hook
    │   ├── 📄 useForm.ts                    # Form validation hook
    │   ├── 📄 useApi.ts                     # API calling hook
    │   ├── 📄 useUsers.ts                   # User management hooks
    │   ├── 📄 useProjects.ts                # Project management hooks
    │   ├── 📄 useAnalytics.ts               # Analytics data hooks
    │   ├── 📄 useNotifications.ts           # Notifications hook
    │   └── 📄 index.ts                      # Barrel exports cho hooks
    │
    ├── 📁 lib/                              # Utility libraries và configurations
    │   ├── 📁 rbac/                         # Role-Based Access Control system
    │   │   ├── 📄 permissions.ts            # Permission definitions và constants
    │   │   ├── 📄 roles.ts                  # Role definitions và mappings
    │   │   ├── 📄 rbacUtils.ts              # RBAC utility functions
    │   │   ├── 📄 permissionChecker.ts      # Permission checking logic
    │   │   ├── 📄 roleManager.ts            # Role management utilities
    │   │   └── 📄 index.ts                  # RBAC exports
    │   ├── 📁 guards/                       # Route và component guards
    │   │   ├── 📄 RouteGuard.tsx            # Route protection component
    │   │   ├── 📄 PermissionGuard.tsx       # Component permission guard
    │   │   ├── 📄 RoleGuard.tsx             # Role-based guard
    │   │   ├── 📄 AdminGuard.tsx            # Admin-only guard
    │   │   ├── 📄 ModeratorGuard.tsx        # Moderator guard
    │   │   └── 📄 index.ts                  # Guards exports
    │   ├── 📁 hooks/                        # Global permission hooks
    │   │   ├── 📄 usePermissions.ts         # Permission checking hook
    │   │   ├── 📄 useRoles.ts               # Role management hook
    │   │   ├── 📄 useRoleCheck.ts           # Role checking hook
    │   │   ├── 📄 useAccessControl.ts       # Access control hook
    │   │   └── 📄 index.ts                  # Hooks exports
    │   ├── 📄 api-client.ts                 # External API client configuration
    │   ├── 📄 auth.ts                       # Client-side authentication utilities
    │   ├── 📄 utils.ts                      # General utility functions
    │   ├── 📄 validations.ts                # Zod validation schemas (client-side)
    │   ├── 📄 constants.ts                  # Application constants
    │   ├── 📄 config.ts                     # App configuration
    │   ├── 📄 navigation.ts                 # Navigation utilities với RBAC
    │   ├── 📄 errorHandler.ts               # Client-side error handling
    │   ├── 📄 formatters.ts                 # Data formatting functions
    │   ├── 📄 validators.ts                 # Custom client-side validators
    │   ├── 📄 storage.ts                    # LocalStorage/SessionStorage utilities
    │   ├── 📄 analytics.ts                  # Client-side analytics tracking
    │   ├── 📄 seo.ts                        # SEO utilities
    │   ├── 📄 upload.ts                     # File upload utilities (to external API)
    │   ├── 📄 pdf.ts                        # PDF generation utilities (client-side)
    │   ├── 📄 excel.ts                      # Excel export utilities (client-side)
    │   ├── 📄 date.ts                       # Date manipulation utilities
    │   ├── 📄 string.ts                     # String manipulation utilities
    │   ├── 📄 number.ts                     # Number formatting utilities
    │   ├── 📄 array.ts                      # Array manipulation utilities
    │   └── 📄 object.ts                     # Object manipulation utilities
    │
    ├── 📁 store/                            # State management
    │   ├── 📄 authStore.ts                  # Authentication store
    │   ├── 📄 userStore.ts                  # User management store
    │   ├── 📄 projectStore.ts               # Project management store
    │   ├── 📄 uiStore.ts                    # UI state store (modals, themes, etc.)
    │   ├── 📄 notificationStore.ts          # Notifications store
    │   ├── 📄 settingsStore.ts              # App settings store
    │   ├── 📄 analyticsStore.ts             # Analytics data store
    │   ├── 📄 searchStore.ts                # Search state store
    │   ├── 📄 filterStore.ts                # Filter state store
    │   ├── 📄 cartStore.ts                  # Shopping cart store (nếu cần)
    │   ├── 📄 chatStore.ts                  # Chat/messaging store (nếu cần)
    │   └── 📄 index.ts                      # Store configuration và exports
    │
    ├── 📁 types/                            # TypeScript type definitions
    │   ├── 📄 auth.ts                       # Authentication types
    │   ├── 📄 user.ts                       # User-related types
    │   ├── 📄 project.ts                    # Project-related types
    │   ├── 📄 api.ts                        # API response types
    │   ├── 📄 common.ts                     # Common/shared types
    │   ├── 📄 ui.ts                         # UI component types
    │   ├── 📄 form.ts                       # Form-related types
    │   ├── 📄 navigation.ts                 # Navigation types
    │   ├── 📄 analytics.ts                  # Analytics types
    │   ├── 📄 notification.ts               # Notification types
    │   ├── 📄 settings.ts                   # Settings types
    │   ├── 📄 upload.ts                     # File upload types
    │   ├── 📄 database.ts                   # Database model types
    │   ├── 📄 external.ts                   # External API types
    │   ├── 📄 environment.ts                # Environment variable types
    │   └── 📄 index.ts                      # Type exports và re-exports
    │
    ├── 📁 styles/                           # Additional styles
    │   ├── 📄 components.css                # Component-specific styles
    │   ├── 📄 utilities.css                 # Custom utility classes
    │   ├── 📄 animations.css                # Custom animations
    │   ├── 📄 themes.css                    # Theme variables
    │   ├── 📄 print.css                     # Print styles
    │   ├── 📄 responsive.css                # Responsive utilities
    │   └── 📄 vendor.css                    # Third-party library overrides
    │
    ├── 📁 providers/                        # React context providers
    │   ├── 📄 AuthProvider.tsx              # Authentication context
    │   ├── 📄 ThemeProvider.tsx             # Theme context
    │   ├── 📄 QueryProvider.tsx             # React Query provider
    │   ├── 📄 ToastProvider.tsx             # Toast notification provider
    │   ├── 📄 ModalProvider.tsx             # Modal context provider
    │   ├── 📄 SocketProvider.tsx            # WebSocket provider (nếu cần)
    │   ├── 📄 AnalyticsProvider.tsx         # Analytics tracking provider
    │   └── 📄 index.tsx                     # Combined providers wrapper
    │
    ├── 📁 data/                             # Static data và mock data
    │   ├── 📄 navigation.ts                 # Navigation menu data
    │   ├── 📄 countries.ts                  # Countries list
    │   ├── 📄 currencies.ts                 # Currencies data
    │   ├── 📄 timezones.ts                  # Timezones data
    │   ├── 📄 languages.ts                  # Supported languages
    │   ├── 📄 roles.ts                      # User roles data
    │   ├── 📄 permissions.ts                # Permissions data
    │   ├── 📄 categories.ts                 # Categories data
    │   ├── 📄 tags.ts                       # Tags data
    │   ├── 📄 mockUsers.ts                  # Mock user data cho development
    │   ├── 📄 mockProjects.ts               # Mock project data
    │   └── 📄 mockAnalytics.ts              # Mock analytics data
    │
    ├── 📁 assets/                           # Asset files
    │   ├── 📁 icons/                        # Custom icon files
    │   │   ├── 📄 logo.svg                  # Logo icon
    │   │   ├── 📄 user.svg                  # User icon
    │   │   ├── 📄 dashboard.svg             # Dashboard icon
    │   │   ├── 📄 settings.svg              # Settings icon
    │   │   └── 📄 analytics.svg             # Analytics icon
    │   ├── 📁 images/                       # Image assets
    │   │   ├── 📄 hero-bg.jpg               # Hero background
    │   │   ├── 📄 placeholder.png           # Placeholder image
    │   │   └── 📄 error-illustration.svg    # Error page illustration
    │   └── 📁 fonts/                        # Custom fonts (nếu cần)
    │       ├── 📄 custom-font.woff2         # Custom font file
    │       └── 📄 font-face.css             # Font face declarations
    │
    └── 📁 workers/                          # Web Workers (nếu cần)
        ├── 📄 dataProcessor.ts              # Data processing worker
        ├── 📄 imageOptimizer.ts             # Image optimization worker
        └── 📄 backgroundSync.ts             # Background sync worker
```

## 📊 Thống Kê Cấu Trúc

### 📁 Tổng Quan Thư Mục Chính (Module-Based RBAC)
- **Root Level**: 15+ file cấu hình
- **src/app**: 40+ role-based route pages với route protection
- **src/modules**: 6 modules chính (auth, admin, user-management, dashboard, analytics, projects, settings)
- **src/components**: 50+ shared components với RBAC support
- **src/lib/rbac**: Complete RBAC system với permissions, roles, guards
- **src/hooks**: 25+ custom hooks bao gồm permission hooks
- **src/lib**: 30+ utility functions với RBAC integration
- **src/store**: 15+ state stores bao gồm permission stores
- **src/types**: 20+ type definitions bao gồm RBAC types
- **Testing**: 15+ test files và utilities

### 🎯 Phân Loại File Theo Loại

#### TypeScript/TSX Files (.ts/.tsx)
- **Module Components**: ~150 files (across 6 modules với role-based organization)
- **Shared Components**: ~50 files (UI, layout, common, forms)
- **Pages**: ~40 files (role-based routes với protection)
- **Hooks**: ~35 files (module hooks + global hooks + permission hooks)
- **RBAC System**: ~15 files (permissions, roles, guards, utilities)
- **Utilities**: ~30 files (client-side với RBAC integration)
- **Types**: ~25 files (module types + RBAC types)
- **Stores**: ~15 files (module stores + permission stores)
- **Providers**: ~8 files

#### Configuration Files
- **Root Config**: package.json, tsconfig.json, next.config.js, tailwind.config.js
- **Linting**: .eslintrc.json, .prettierrc
- **Testing**: jest.config.js, playwright.config.ts
- **Deployment**: Dockerfile, vercel.json, docker-compose.yml

#### Style Files (.css)
- **Global**: globals.css
- **Modular**: components.css, utilities.css, animations.css
- **Theme**: themes.css
- **Responsive**: responsive.css

## 🚀 Hướng Dẫn Sử Dụng Cấu Trúc

### 1. Khởi Tạo Dự Án
```bash
# Tạo dự án Next.js mới
npx create-next-app@latest my-project --typescript --tailwind --eslint --app

# Di chuyển vào thư mục dự án
cd my-project

# Tạo cấu trúc thư mục theo template (Module-Based RBAC)
mkdir -p src/{app/{(auth),(admin),(moderator),(user),(shared),access-denied},modules/{auth,admin,user-management,dashboard,analytics,projects,settings}/{components/{shared,admin,moderator,user},hooks,types,store,utils},components/{ui,layout/{shared,admin,moderator,user},common,forms},lib/{rbac,guards,hooks},hooks,store,types,styles,providers,data,assets,workers}
```

### 2. Quy Tắc Tổ Chức File

#### 📁 Components
- **ui/**: Chỉ chứa base components từ shadcn/ui
- **forms/**: Form components có logic validation
- **layout/**: Components liên quan đến layout (Header, Sidebar, Footer)
- **common/**: Shared components dùng chung toàn app
- **features/**: Components specific cho từng feature

#### 📁 Hooks
- Mỗi hook phải có prefix `use`
- Tên file theo camelCase: `useAuth.ts`
- Export default function với tên giống file

#### 📁 Lib
- **API Client**: Configuration để connect với external backend
- **Utilities**: Pure functions không có side effects (client-side)
- **Configurations**: Setup cho external libraries
- **Constants**: Application-wide constants

#### 📁 Store
- Mỗi store quản lý một domain cụ thể
- Sử dụng Zustand với TypeScript
- Export typed hooks cho components

#### 📁 Types
- Tổ chức theo domain/feature
- Sử dụng interface cho object types
- Sử dụng type cho unions và primitives

### 3. Naming Conventions

#### File Names
```
✅ Đúng:
- UserProfile.tsx (Component)
- useAuth.ts (Hook)
- userStore.ts (Store)
- auth.types.ts (Types)

❌ Sai:
- userprofile.tsx
- UseAuth.ts
- UserStore.ts
- authTypes.ts
```

#### Component Names
```typescript
// ✅ Đúng
export const UserProfile: React.FC<UserProfileProps> = () => {
  return <div>...</div>
}

// ❌ Sai
export const userProfile = () => {
  return <div>...</div>
}
```

#### Hook Names
```typescript
// ✅ Đúng
export const useUserData = () => {
  // hook logic
}

// ❌ Sai
export const getUserData = () => {
  // not a hook
}
```

### 4. Import/Export Patterns

#### Barrel Exports
```typescript
// components/ui/index.ts
export { Button } from './button'
export { Input } from './input'
export { Dialog } from './dialog'

// Usage
import { Button, Input, Dialog } from '@/components/ui'
```

#### Absolute Imports
```typescript
// tsconfig.json paths configuration
{
  "paths": {
    "@/*": ["./src/*"],
    "@/components/*": ["./src/components/*"],
    "@/hooks/*": ["./src/hooks/*"],
    "@/lib/*": ["./src/lib/*"]
  }
}

// Usage
import { Button } from '@/components/ui'
import { useAuth } from '@/hooks'
import { apiClient } from '@/lib/api'
```

### 5. Feature-Based Organization

#### Cấu Trúc Feature
```
features/
├── auth/
│   ├── components/
│   ├── hooks/
│   ├── types/
│   ├── store/
│   └── index.ts
```

#### Export Pattern
```typescript
// features/auth/index.ts
export * from './components'
export * from './hooks'
export * from './types'
export * from './store'

// Usage
import { LoginForm, useAuth, AuthProvider } from '@/features/auth'
```

### 6. Testing Organization

#### Test File Placement
```
src/
├── components/
│   ├── UserProfile.tsx
│   └── __tests__/
│       └── UserProfile.test.tsx
├── hooks/
│   ├── useAuth.ts
│   └── __tests__/
│       └── useAuth.test.ts
```

#### Test Naming
```
✅ Đúng:
- UserProfile.test.tsx
- useAuth.test.ts
- auth.spec.ts (E2E)

❌ Sai:
- UserProfile.spec.tsx (Unit test)
- useAuth.spec.ts (Unit test)
- auth.test.ts (E2E)
```

## 🔧 Scripts Hữu Ích

### Tạo Component Mới
```bash
# Script tạo component với template
#!/bin/bash
COMPONENT_NAME=$1
COMPONENT_DIR="src/components/common"

mkdir -p "$COMPONENT_DIR"
cat > "$COMPONENT_DIR/$COMPONENT_NAME.tsx" << EOF
import React from 'react'

interface ${COMPONENT_NAME}Props {
  // Define props here
}

export const $COMPONENT_NAME: React.FC<${COMPONENT_NAME}Props> = ({
  // Destructure props here
}) => {
  return (
    <div>
      {/* Component content */}
    </div>
  )
}
EOF
```

### Tạo Hook Mới
```bash
# Script tạo hook với template
#!/bin/bash
HOOK_NAME=$1
HOOK_DIR="src/hooks"

mkdir -p "$HOOK_DIR"
cat > "$HOOK_DIR/$HOOK_NAME.ts" << EOF
import { useState, useEffect } from 'react'

export const $HOOK_NAME = () => {
  // Hook logic here

  return {
    // Return values
  }
}
EOF
```

## 📝 Lưu Ý Quan Trọng

### 1. Scalability
- Cấu trúc này được thiết kế để scale từ small đến enterprise projects
- Có thể bỏ bớt các thư mục không cần thiết cho dự án nhỏ
- Thêm thư mục mới khi cần thiết

### 2. Maintainability
- Mỗi file có responsibility rõ ràng
- Dễ dàng tìm kiếm và refactor
- Code organization tutân thủ best practices

### 3. Team Collaboration
- Naming conventions nhất quán
- Clear separation of concerns
- Easy onboarding cho team members mới

### 4. Performance
- Code splitting tự động với Next.js App Router
- Lazy loading components khi cần
- Optimized bundle size với tree shaking

## 🔗 **Tích hợp với External Backend**

### API Client Configuration
```typescript
// lib/api-client.ts
class ApiClient {
  private baseURL: string
  private token: string | null = null

  constructor() {
    this.baseURL = process.env.NEXT_PUBLIC_API_URL || 'http://localhost:5000'
    this.token = this.getStoredToken()
  }

  private getStoredToken(): string | null {
    if (typeof window !== 'undefined') {
      return localStorage.getItem('auth_token')
    }
    return null
  }

  private async request<T>(endpoint: string, options?: RequestInit): Promise<T> {
    const url = `${this.baseURL}${endpoint}`

    const response = await fetch(url, {
      headers: {
        'Content-Type': 'application/json',
        ...(this.token && { Authorization: `Bearer ${this.token}` }),
        ...options?.headers,
      },
      ...options,
    })

    if (!response.ok) {
      const error = await response.json().catch(() => ({}))
      throw new Error(error.message || `HTTP ${response.status}`)
    }

    return response.json()
  }

  // Authentication methods
  auth = {
    login: (credentials: LoginCredentials) =>
      this.request<AuthResponse>('/auth/login', {
        method: 'POST',
        body: JSON.stringify(credentials)
      }),

    register: (userData: RegisterData) =>
      this.request<AuthResponse>('/auth/register', {
        method: 'POST',
        body: JSON.stringify(userData)
      }),

    logout: () =>
      this.request('/auth/logout', { method: 'POST' })
  }

  // Users methods
  users = {
    getAll: (params?: UserFilters) => {
      const searchParams = new URLSearchParams(params as any)
      return this.request<UsersResponse>(`/users?${searchParams}`)
    },

    getById: (id: string) =>
      this.request<User>(`/users/${id}`),

    create: (userData: CreateUserData) =>
      this.request<User>('/users', {
        method: 'POST',
        body: JSON.stringify(userData)
      }),

    update: (id: string, userData: UpdateUserData) =>
      this.request<User>(`/users/${id}`, {
        method: 'PUT',
        body: JSON.stringify(userData)
      }),

    delete: (id: string) =>
      this.request(`/users/${id}`, { method: 'DELETE' })
  }

  // Projects methods
  projects = {
    getAll: () => this.request<Project[]>('/projects'),
    getById: (id: string) => this.request<Project>(`/projects/${id}`),
    create: (data: CreateProjectData) =>
      this.request<Project>('/projects', {
        method: 'POST',
        body: JSON.stringify(data)
      }),
    update: (id: string, data: UpdateProjectData) =>
      this.request<Project>(`/projects/${id}`, {
        method: 'PUT',
        body: JSON.stringify(data)
      }),
    delete: (id: string) =>
      this.request(`/projects/${id}`, { method: 'DELETE' })
  }

  setToken(token: string) {
    this.token = token
    if (typeof window !== 'undefined') {
      localStorage.setItem('auth_token', token)
    }
  }

  clearToken() {
    this.token = null
    if (typeof window !== 'undefined') {
      localStorage.removeItem('auth_token')
    }
  }
}

export const apiClient = new ApiClient()
```

### Environment Variables
```bash
# .env.local
NEXT_PUBLIC_API_URL=https://api.yourbackend.com
NEXT_PUBLIC_APP_URL=https://yourapp.com

# Development
# NEXT_PUBLIC_API_URL=http://localhost:5000
```

### React Query Hooks
```typescript
// hooks/useUsers.ts
import { useQuery, useMutation, useQueryClient } from '@tanstack/react-query'
import { apiClient } from '@/lib/api-client'

export const useUsers = (filters?: UserFilters) => {
  return useQuery({
    queryKey: ['users', filters],
    queryFn: () => apiClient.users.getAll(filters),
    staleTime: 5 * 60 * 1000, // 5 minutes
  })
}

export const useUser = (id: string) => {
  return useQuery({
    queryKey: ['user', id],
    queryFn: () => apiClient.users.getById(id),
    enabled: !!id,
  })
}

export const useCreateUser = () => {
  const queryClient = useQueryClient()

  return useMutation({
    mutationFn: (userData: CreateUserData) => apiClient.users.create(userData),
    onSuccess: () => {
      queryClient.invalidateQueries({ queryKey: ['users'] })
    }
  })
}

export const useUpdateUser = () => {
  const queryClient = useQueryClient()

  return useMutation({
    mutationFn: ({ id, data }: { id: string; data: UpdateUserData }) =>
      apiClient.users.update(id, data),
    onSuccess: (_, { id }) => {
      queryClient.invalidateQueries({ queryKey: ['users'] })
      queryClient.invalidateQueries({ queryKey: ['user', id] })
    }
  })
}

export const useDeleteUser = () => {
  const queryClient = useQueryClient()

  return useMutation({
    mutationFn: (id: string) => apiClient.users.delete(id),
    onSuccess: () => {
      queryClient.invalidateQueries({ queryKey: ['users'] })
    }
  })
}
```

### Authentication Integration
```typescript
// lib/auth.ts
import { apiClient } from './api-client'

export const authService = {
  async login(credentials: LoginCredentials) {
    try {
      const response = await apiClient.auth.login(credentials)

      // Store token and user info
      apiClient.setToken(response.token)
      localStorage.setItem('user', JSON.stringify(response.user))

      return response
    } catch (error) {
      throw new Error('Đăng nhập thất bại')
    }
  },

  async register(userData: RegisterData) {
    try {
      const response = await apiClient.auth.register(userData)

      // Auto login after registration
      apiClient.setToken(response.token)
      localStorage.setItem('user', JSON.stringify(response.user))

      return response
    } catch (error) {
      throw new Error('Đăng ký thất bại')
    }
  },

  async logout() {
    try {
      await apiClient.auth.logout()
    } catch (error) {
      // Continue with logout even if API call fails
      console.error('Logout API call failed:', error)
    } finally {
      // Clear local storage
      apiClient.clearToken()
      localStorage.removeItem('user')

      // Redirect to login
      window.location.href = '/login'
    }
  },

  getCurrentUser() {
    if (typeof window !== 'undefined') {
      const userStr = localStorage.getItem('user')
      return userStr ? JSON.parse(userStr) : null
    }
    return null
  },

  isAuthenticated() {
    return !!this.getCurrentUser()
  }
}
```

---

## 🔐 **Module-Based RBAC Implementation Guide**

### 1. **Permission System Setup**

```typescript
// lib/rbac/permissions.ts
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

  // Moderation
  CONTENT_MODERATE = 'content:moderate',
  CONTENT_DELETE = 'content:delete',
  CONTENT_APPROVE = 'content:approve',
}

export const ROLE_PERMISSIONS = {
  admin: Object.values(Permission), // Full access
  moderator: [
    Permission.USER_READ,
    Permission.PROJECT_READ,
    Permission.PROJECT_UPDATE,
    Permission.ANALYTICS_READ,
    Permission.CONTENT_MODERATE,
    Permission.CONTENT_DELETE,
    Permission.CONTENT_APPROVE,
  ],
  user: [
    Permission.USER_READ, // Own profile only
    Permission.PROJECT_READ,
    Permission.PROJECT_CREATE,
    Permission.PROJECT_UPDATE, // Own projects only
    Permission.ANALYTICS_READ, // Own data only
  ]
} as const

export type UserRole = keyof typeof ROLE_PERMISSIONS
```

### 2. **Module Structure Implementation**

```typescript
// modules/auth/index.ts - Module barrel export
export * from './components'
export * from './hooks'
export * from './types'
export * from './store'
export * from './utils'

// modules/auth/components/index.ts
export { LoginForm } from './LoginForm'
export { RegisterForm } from './RegisterForm'
export { AuthGuard } from './AuthGuard'
export { RoleSelector } from './RoleSelector'

// modules/user-management/components/admin/index.ts
export { UserTable } from './UserTable'
export { UserActions } from './UserActions'
export { BulkActions } from './BulkActions'
export { UserRoleEditor } from './UserRoleEditor'
```

### 3. **Route Protection Implementation**

```typescript
// app/(admin)/layout.tsx
import { RouteGuard } from '@/lib/guards'
import { AdminLayout } from '@/components/layout/admin'

export default function AdminLayoutPage({
  children,
}: {
  children: React.ReactNode
}) {
  return (
    <RouteGuard
      allowedRoles={['admin']}
      redirectTo="/access-denied"
    >
      <AdminLayout>
        {children}
      </AdminLayout>
    </RouteGuard>
  )
}

// app/(moderator)/layout.tsx
import { RouteGuard } from '@/lib/guards'
import { ModeratorLayout } from '@/components/layout/moderator'

export default function ModeratorLayoutPage({
  children,
}: {
  children: React.ReactNode
}) {
  return (
    <RouteGuard
      allowedRoles={['admin', 'moderator']}
      redirectTo="/access-denied"
    >
      <ModeratorLayout>
        {children}
      </ModeratorLayout>
    </RouteGuard>
  )
}
```

### 4. **Component-Level Access Control**

```typescript
// modules/user-management/components/shared/UserCard.tsx
import { PermissionGuard, RoleBasedRenderer } from '@/components/common'
import { Permission } from '@/lib/rbac/permissions'

export const UserCard: React.FC<UserCardProps> = ({ user }) => {
  return (
    <div className="user-card">
      <h3>{user.name}</h3>
      <p>{user.email}</p>

      {/* Role-based rendering */}
      <RoleBasedRenderer allowedRoles={['admin', 'moderator']}>
        <div className="admin-actions">
          <PermissionGuard permission={Permission.USER_UPDATE}>
            <button>Edit User</button>
          </PermissionGuard>

          <PermissionGuard permission={Permission.USER_DELETE}>
            <button>Delete User</button>
          </PermissionGuard>
        </div>
      </RoleBasedRenderer>

      {/* User can only edit their own profile */}
      <RoleBasedRenderer allowedRoles={['user']}>
        {user.isCurrentUser && (
          <button>Edit My Profile</button>
        )}
      </RoleBasedRenderer>
    </div>
  )
}
```

### 5. **Module Hook Implementation**

```typescript
// modules/user-management/hooks/useUserActions.ts
import { usePermissions } from '@/lib/hooks/usePermissions'
import { Permission } from '@/lib/rbac/permissions'
import { apiClient } from '@/lib/api-client'

export const useUserActions = () => {
  const { hasPermission } = usePermissions()

  const canCreateUser = hasPermission(Permission.USER_CREATE)
  const canUpdateUser = hasPermission(Permission.USER_UPDATE)
  const canDeleteUser = hasPermission(Permission.USER_DELETE)
  const canManageRoles = hasPermission(Permission.USER_MANAGE_ROLES)

  const createUser = async (userData: CreateUserData) => {
    if (!canCreateUser) {
      throw new Error('Không có quyền tạo user')
    }
    return apiClient.users.create(userData)
  }

  const updateUser = async (id: string, userData: UpdateUserData) => {
    if (!canUpdateUser) {
      throw new Error('Không có quyền cập nhật user')
    }
    return apiClient.users.update(id, userData)
  }

  const deleteUser = async (id: string) => {
    if (!canDeleteUser) {
      throw new Error('Không có quyền xóa user')
    }
    return apiClient.users.delete(id)
  }

  return {
    createUser,
    updateUser,
    deleteUser,
    canCreateUser,
    canUpdateUser,
    canDeleteUser,
    canManageRoles
  }
}
```

### 6. **Navigation với RBAC**

```typescript
// lib/navigation.ts
import { Permission, UserRole } from './rbac/permissions'
import { PermissionChecker } from './rbac/permissionChecker'

interface NavigationItem {
  label: string
  href: string
  icon?: string
  requiredPermissions?: Permission[]
  allowedRoles?: UserRole[]
  children?: NavigationItem[]
}

export const getNavigationItems = (userRole: UserRole): NavigationItem[] => {
  const allItems: NavigationItem[] = [
    {
      label: 'Dashboard',
      href: '/dashboard',
      icon: 'dashboard',
    },
    {
      label: 'User Management',
      href: '/users',
      icon: 'users',
      requiredPermissions: [Permission.USER_READ],
      children: [
        {
          label: 'All Users',
          href: '/admin/users',
          allowedRoles: ['admin'],
        },
        {
          label: 'User Reports',
          href: '/moderator/users',
          allowedRoles: ['admin', 'moderator'],
        },
        {
          label: 'My Profile',
          href: '/user/profile',
          allowedRoles: ['user'],
        }
      ]
    },
    {
      label: 'Analytics',
      href: '/analytics',
      icon: 'analytics',
      requiredPermissions: [Permission.ANALYTICS_READ],
      children: [
        {
          label: 'System Analytics',
          href: '/admin/analytics',
          allowedRoles: ['admin'],
        },
        {
          label: 'Content Analytics',
          href: '/moderator/analytics',
          allowedRoles: ['admin', 'moderator'],
        },
        {
          label: 'Personal Analytics',
          href: '/user/analytics',
          allowedRoles: ['user'],
        }
      ]
    },
    {
      label: 'System Settings',
      href: '/admin/system',
      icon: 'settings',
      allowedRoles: ['admin'],
      requiredPermissions: [Permission.SYSTEM_SETTINGS],
    }
  ]

  return filterNavigationByRole(allItems, userRole)
}

const filterNavigationByRole = (
  items: NavigationItem[],
  userRole: UserRole
): NavigationItem[] => {
  return items.filter(item => {
    // Check role-based access
    if (item.allowedRoles && !item.allowedRoles.includes(userRole)) {
      return false
    }

    // Check permission-based access
    if (item.requiredPermissions) {
      const hasPermission = item.requiredPermissions.some(permission =>
        PermissionChecker.hasPermission(userRole, permission)
      )
      if (!hasPermission) return false
    }

    // Filter children recursively
    if (item.children) {
      item.children = filterNavigationByRole(item.children, userRole)
    }

    return true
  })
}
```

### 7. **Testing RBAC Components**

```typescript
// __tests__/components/UserCard.test.tsx
import { render, screen } from '@testing-library/react'
import { UserCard } from '@/modules/user-management/components/shared/UserCard'
import { TestProviders } from '@/test-utils'

const mockUser = {
  id: '1',
  name: 'Test User',
  email: 'test@example.com',
  role: 'user'
}

describe('UserCard RBAC', () => {
  it('hiển thị admin actions cho admin role', () => {
    render(
      <TestProviders userRole="admin">
        <UserCard user={mockUser} />
      </TestProviders>
    )

    expect(screen.getByText('Edit User')).toBeInTheDocument()
    expect(screen.getByText('Delete User')).toBeInTheDocument()
  })

  it('ẩn admin actions cho user role', () => {
    render(
      <TestProviders userRole="user">
        <UserCard user={mockUser} />
      </TestProviders>
    )

    expect(screen.queryByText('Edit User')).not.toBeInTheDocument()
    expect(screen.queryByText('Delete User')).not.toBeInTheDocument()
  })

  it('hiển thị edit profile cho current user', () => {
    render(
      <TestProviders userRole="user">
        <UserCard user={{ ...mockUser, isCurrentUser: true }} />
      </TestProviders>
    )

    expect(screen.getByText('Edit My Profile')).toBeInTheDocument()
  })
})
```

---

**💡 Tip**: Cấu trúc module-based RBAC này cung cấp scalability cao và maintainability tốt. Bắt đầu với các modules cơ bản và mở rộng dần theo nhu cầu dự án!
