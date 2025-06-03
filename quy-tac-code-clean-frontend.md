# Quy Tắc Code Clean - Frontend Next.js với Module-Based RBAC

## 📋 **Tổng Quan**

Tài liệu này định nghĩa các quy tắc code clean cho dự án Next.js frontend với module-based architecture và Role-Based Access Control (RBAC).

## 🎯 **Nguyên Tắc Cơ Bản**

### 1. **Consistency (Nhất Quán)**
- Sử dụng naming conventions thống nhất trong toàn bộ dự án
- Áp dụng cùng một pattern cho tất cả modules
- Maintain cùng một code style và formatting

### 2. **Modularity (Tính Modular)**
- Mỗi module phải độc lập và có thể tái sử dụng
- Tránh dependencies chéo giữa các modules
- Sử dụng barrel exports để quản lý exports

### 3. **Readability (Dễ Đọc)**
- Code phải self-documenting
- Sử dụng tên biến và function có ý nghĩa
- Comments bằng tiếng Việt cho business logic

## 📝 **Naming Conventions**

### 1. **Files và Folders**

#### ✅ **Đúng:**
```
// Components
UserProfile.tsx
LoginForm.tsx
AdminDashboard.tsx

// Hooks
useAuth.ts
usePermissions.ts
useUserActions.ts

// Types
user.types.ts
auth.types.ts
permission.types.ts

// Stores
authStore.ts
userStore.ts
permissionStore.ts

// Modules
auth/
user-management/
dashboard/
```

#### ❌ **Sai:**
```
// Không sử dụng
userprofile.tsx
UseAuth.ts
UserStore.ts
user_management/
```

### 2. **Components**

#### ✅ **Đúng:**
```typescript
// PascalCase cho component names
export const UserProfile: React.FC<UserProfileProps> = ({ user }) => {
  return <div>...</div>
}

// camelCase cho props và variables
interface UserProfileProps {
  user: User
  isEditable?: boolean
  onEdit?: (userId: string) => void
}
```

#### ❌ **Sai:**
```typescript
// Không sử dụng
export const userProfile = () => { ... }
export const User_Profile = () => { ... }

interface UserProfileprops { ... } // Thiếu PascalCase
```

### 3. **Hooks**

#### ✅ **Đúng:**
```typescript
// Luôn bắt đầu với "use"
export const useAuth = () => { ... }
export const usePermissions = () => { ... }
export const useUserActions = () => { ... }

// Custom hooks với specific purpose
export const useRoleBasedNavigation = () => { ... }
export const useModulePermissions = (module: string) => { ... }
```

### 4. **Constants và Enums**

#### ✅ **Đúng:**
```typescript
// SCREAMING_SNAKE_CASE cho constants
export const API_BASE_URL = 'https://api.example.com'
export const DEFAULT_PAGE_SIZE = 10
export const STORAGE_KEYS = {
  AUTH_TOKEN: 'auth_token',
  USER_PREFERENCES: 'user_preferences'
} as const

// PascalCase cho enums
export enum Permission {
  USER_READ = 'user:read',
  USER_CREATE = 'user:create',
  PROJECT_MANAGE = 'project:manage'
}

export enum UserRole {
  ADMIN = 'admin',
  MODERATOR = 'moderator',
  USER = 'user'
}
```

## 🏗️ **Module Organization Patterns**

### 1. **Module Structure Standard**

Mỗi module phải tuân theo cấu trúc sau:

```
modules/[module-name]/
├── components/
│   ├── shared/          # Components dùng chung cho tất cả roles
│   ├── admin/           # Components chỉ dành cho admin
│   ├── moderator/       # Components chỉ dành cho moderator
│   ├── user/            # Components chỉ dành cho user
│   └── index.ts         # Barrel exports
├── hooks/
│   ├── use[ModuleName].ts
│   ├── use[ModuleName]Actions.ts
│   ├── use[ModuleName]Permissions.ts
│   └── index.ts
├── types/
│   ├── [module].types.ts
│   └── index.ts
├── store/
│   ├── [module]Store.ts
│   └── index.ts
├── utils/
│   ├── [module]Utils.ts
│   └── index.ts
└── index.ts             # Module exports
```

### 2. **Barrel Exports Pattern**

#### ✅ **Đúng:**
```typescript
// modules/auth/components/index.ts
export { LoginForm } from './LoginForm'
export { RegisterForm } from './RegisterForm'
export { AuthGuard } from './AuthGuard'

// modules/auth/hooks/index.ts
export { useAuth } from './useAuth'
export { useLogin } from './useLogin'
export { usePermissions } from './usePermissions'

// modules/auth/index.ts
export * from './components'
export * from './hooks'
export * from './types'
export * from './store'
```

#### Usage:
```typescript
// Sử dụng barrel exports
import { LoginForm, useAuth, AuthGuard } from '@/modules/auth'

// Thay vì import từng file riêng lẻ
import { LoginForm } from '@/modules/auth/components/LoginForm'
import { useAuth } from '@/modules/auth/hooks/useAuth'
import { AuthGuard } from '@/modules/auth/components/AuthGuard'
```

## ⚛️ **React Components Best Practices**

### 1. **Component Structure**

#### ✅ **Đúng:**
```typescript
// components/UserCard.tsx
import React from 'react'
import { User } from '@/types'
import { usePermissions } from '@/lib/hooks'
import { Permission } from '@/lib/rbac/permissions'
import { PermissionGuard } from '@/components/common'

interface UserCardProps {
  user: User
  onEdit?: (userId: string) => void
  onDelete?: (userId: string) => void
  className?: string
}

/**
 * Component hiển thị thông tin user với role-based actions
 * @param user - Thông tin user cần hiển thị
 * @param onEdit - Callback khi edit user
 * @param onDelete - Callback khi delete user
 */
export const UserCard: React.FC<UserCardProps> = ({
  user,
  onEdit,
  onDelete,
  className
}) => {
  const { hasPermission } = usePermissions()

  const handleEdit = () => {
    onEdit?.(user.id)
  }

  const handleDelete = () => {
    if (window.confirm('Bạn có chắc chắn muốn xóa user này?')) {
      onDelete?.(user.id)
    }
  }

  return (
    <div className={`user-card ${className || ''}`}>
      <div className="user-info">
        <h3>{user.name}</h3>
        <p>{user.email}</p>
        <span className="role-badge">{user.role}</span>
      </div>
      
      <div className="user-actions">
        <PermissionGuard permission={Permission.USER_UPDATE}>
          <button onClick={handleEdit} className="btn-edit">
            Chỉnh sửa
          </button>
        </PermissionGuard>
        
        <PermissionGuard permission={Permission.USER_DELETE}>
          <button onClick={handleDelete} className="btn-delete">
            Xóa
          </button>
        </PermissionGuard>
      </div>
    </div>
  )
}

// Export default nếu là main component của file
export default UserCard
```

### 2. **Props Interface Design**

#### ✅ **Đúng:**
```typescript
// Sử dụng interface thay vì type cho props
interface UserFormProps {
  // Required props trước
  user: User
  onSubmit: (userData: UpdateUserData) => void
  
  // Optional props sau, có default values
  mode?: 'create' | 'edit'
  isLoading?: boolean
  disabled?: boolean
  
  // Event handlers
  onCancel?: () => void
  onValidationError?: (errors: ValidationError[]) => void
  
  // Style props
  className?: string
  variant?: 'default' | 'compact'
}

// Sử dụng default parameters
export const UserForm: React.FC<UserFormProps> = ({
  user,
  onSubmit,
  mode = 'edit',
  isLoading = false,
  disabled = false,
  onCancel,
  onValidationError,
  className,
  variant = 'default'
}) => {
  // Component implementation
}
```

### 3. **Conditional Rendering Patterns**

#### ✅ **Đúng:**
```typescript
// Sử dụng early return cho complex conditions
export const UserDashboard: React.FC = () => {
  const { user, isLoading } = useAuth()
  const { userRole } = usePermissions()

  // Early return cho loading state
  if (isLoading) {
    return <LoadingSpinner />
  }

  // Early return cho unauthorized
  if (!user) {
    return <AccessDenied message="Vui lòng đăng nhập để tiếp tục" />
  }

  // Main render
  return (
    <div className="dashboard">
      <h1>Chào mừng, {user.name}</h1>
      
      {/* Role-based rendering */}
      <RoleBasedRenderer allowedRoles={['admin']}>
        <AdminPanel />
      </RoleBasedRenderer>
      
      <RoleBasedRenderer allowedRoles={['admin', 'moderator']}>
        <ModerationTools />
      </RoleBasedRenderer>
      
      {/* Permission-based rendering */}
      <PermissionGuard permission={Permission.ANALYTICS_READ}>
        <AnalyticsWidget />
      </PermissionGuard>
    </div>
  )
}
```

#### ❌ **Sai:**
```typescript
// Tránh nested ternary operators phức tạp
return (
  <div>
    {isLoading ? (
      <LoadingSpinner />
    ) : user ? (
      userRole === 'admin' ? (
        <AdminDashboard />
      ) : userRole === 'moderator' ? (
        <ModeratorDashboard />
      ) : (
        <UserDashboard />
      )
    ) : (
      <LoginForm />
    )}
  </div>
)
```

## 🎣 **Custom Hooks Best Practices**

### 1. **Hook Structure Pattern**

#### ✅ **Đúng:**
```typescript
// hooks/useUserActions.ts
import { useMutation, useQueryClient } from '@tanstack/react-query'
import { usePermissions } from '@/lib/hooks'
import { apiClient } from '@/lib/api-client'
import { Permission } from '@/lib/rbac/permissions'
import { User, CreateUserData, UpdateUserData } from '@/types'

interface UseUserActionsReturn {
  // Actions
  createUser: (userData: CreateUserData) => Promise<User>
  updateUser: (id: string, userData: UpdateUserData) => Promise<User>
  deleteUser: (id: string) => Promise<void>
  
  // States
  isCreating: boolean
  isUpdating: boolean
  isDeleting: boolean
  
  // Permissions
  canCreateUser: boolean
  canUpdateUser: boolean
  canDeleteUser: boolean
}

/**
 * Hook quản lý các actions liên quan đến user với permission checking
 */
export const useUserActions = (): UseUserActionsReturn => {
  const queryClient = useQueryClient()
  const { hasPermission } = usePermissions()
  
  // Permission checks
  const canCreateUser = hasPermission(Permission.USER_CREATE)
  const canUpdateUser = hasPermission(Permission.USER_UPDATE)
  const canDeleteUser = hasPermission(Permission.USER_DELETE)
  
  // Mutations
  const createUserMutation = useMutation({
    mutationFn: (userData: CreateUserData) => {
      if (!canCreateUser) {
        throw new Error('Không có quyền tạo user')
      }
      return apiClient.users.create(userData)
    },
    onSuccess: () => {
      queryClient.invalidateQueries({ queryKey: ['users'] })
    }
  })
  
  const updateUserMutation = useMutation({
    mutationFn: ({ id, data }: { id: string; data: UpdateUserData }) => {
      if (!canUpdateUser) {
        throw new Error('Không có quyền cập nhật user')
      }
      return apiClient.users.update(id, data)
    },
    onSuccess: (_, { id }) => {
      queryClient.invalidateQueries({ queryKey: ['users'] })
      queryClient.invalidateQueries({ queryKey: ['user', id] })
    }
  })
  
  const deleteUserMutation = useMutation({
    mutationFn: (id: string) => {
      if (!canDeleteUser) {
        throw new Error('Không có quyền xóa user')
      }
      return apiClient.users.delete(id)
    },
    onSuccess: () => {
      queryClient.invalidateQueries({ queryKey: ['users'] })
    }
  })
  
  return {
    // Actions
    createUser: createUserMutation.mutateAsync,
    updateUser: (id: string, data: UpdateUserData) => 
      updateUserMutation.mutateAsync({ id, data }),
    deleteUser: deleteUserMutation.mutateAsync,
    
    // States
    isCreating: createUserMutation.isPending,
    isUpdating: updateUserMutation.isPending,
    isDeleting: deleteUserMutation.isPending,
    
    // Permissions
    canCreateUser,
    canUpdateUser,
    canDeleteUser
  }
}
```

### 2. **Hook Dependencies và Performance**

#### ✅ **Đúng:**
```typescript
// Sử dụng useMemo và useCallback đúng cách
export const useFilteredUsers = (filters: UserFilters) => {
  const { data: users, isLoading } = useUsers()
  
  // Memoize filtered results
  const filteredUsers = useMemo(() => {
    if (!users) return []
    
    return users.filter(user => {
      if (filters.role && user.role !== filters.role) return false
      if (filters.search && !user.name.toLowerCase().includes(filters.search.toLowerCase())) return false
      if (filters.isActive !== undefined && user.isActive !== filters.isActive) return false
      return true
    })
  }, [users, filters.role, filters.search, filters.isActive])
  
  // Memoize sort function
  const sortedUsers = useMemo(() => {
    if (!filteredUsers.length) return []
    
    return [...filteredUsers].sort((a, b) => {
      switch (filters.sortBy) {
        case 'name':
          return a.name.localeCompare(b.name)
        case 'email':
          return a.email.localeCompare(b.email)
        case 'createdAt':
          return new Date(b.createdAt).getTime() - new Date(a.createdAt).getTime()
        default:
          return 0
      }
    })
  }, [filteredUsers, filters.sortBy])
  
  return {
    users: sortedUsers,
    isLoading,
    totalCount: filteredUsers.length
  }
}
```

## 📊 **TypeScript Best Practices**

### 1. **Type Definitions**

#### ✅ **Đúng:**
```typescript
// types/user.types.ts

// Base types
export interface User {
  id: string
  name: string
  email: string
  role: UserRole
  isActive: boolean
  avatar?: string
  createdAt: string
  updatedAt: string
}

// Derived types
export type CreateUserData = Omit<User, 'id' | 'createdAt' | 'updatedAt'>
export type UpdateUserData = Partial<Omit<User, 'id' | 'createdAt' | 'updatedAt'>>
export type UserSummary = Pick<User, 'id' | 'name' | 'email' | 'role'>

// Union types
export type UserRole = 'admin' | 'moderator' | 'user'
export type UserStatus = 'active' | 'inactive' | 'pending' | 'suspended'

// Generic types
export interface ApiResponse<T> {
  data: T
  message: string
  success: boolean
}

export interface PaginatedResponse<T> {
  data: T[]
  meta: {
    page: number
    limit: number
    total: number
    totalPages: number
  }
}

// Utility types
export type UserWithPermissions = User & {
  permissions: Permission[]
}

export type UserFormData = {
  [K in keyof CreateUserData]: CreateUserData[K]
}
```

### 2. **Generic Types và Utility Types**

#### ✅ **Đúng:**
```typescript
// types/common.types.ts

// Generic API response
export interface ApiResponse<TData = any> {
  data: TData
  message: string
  success: boolean
  errors?: string[]
}

// Generic form state
export interface FormState<TData> {
  data: TData
  errors: Partial<Record<keyof TData, string>>
  isSubmitting: boolean
  isDirty: boolean
}

// Generic table props
export interface TableProps<TItem> {
  data: TItem[]
  columns: TableColumn<TItem>[]
  onRowClick?: (item: TItem) => void
  onSort?: (field: keyof TItem, direction: 'asc' | 'desc') => void
  isLoading?: boolean
}

export interface TableColumn<TItem> {
  key: keyof TItem
  title: string
  render?: (value: TItem[keyof TItem], item: TItem) => React.ReactNode
  sortable?: boolean
  width?: string
}

// Conditional types
export type RequiredFields<T, K extends keyof T> = T & Required<Pick<T, K>>
export type OptionalFields<T, K extends keyof T> = Omit<T, K> & Partial<Pick<T, K>>

// Usage examples
type UserWithRequiredEmail = RequiredFields<User, 'email'>
type UserWithOptionalAvatar = OptionalFields<User, 'avatar'>
```

## 🏪 **State Management với Zustand**

### 1. **Store Structure Pattern**

#### ✅ **Đúng:**
```typescript
// store/authStore.ts
import { create } from 'zustand'
import { devtools, persist } from 'zustand/middleware'
import { User, LoginCredentials } from '@/types'
import { apiClient } from '@/lib/api-client'

interface AuthState {
  // State
  user: User | null
  token: string | null
  isLoading: boolean
  error: string | null

  // Actions
  login: (credentials: LoginCredentials) => Promise<void>
  logout: () => void
  refreshToken: () => Promise<void>
  clearError: () => void
  setUser: (user: User) => void
}

export const useAuthStore = create<AuthState>()(
  devtools(
    persist(
      (set, get) => ({
        // Initial state
        user: null,
        token: null,
        isLoading: false,
        error: null,

        // Actions
        login: async (credentials) => {
          set({ isLoading: true, error: null })
          try {
            const response = await apiClient.auth.login(credentials)
            set({
              user: response.user,
              token: response.token,
              isLoading: false
            })
          } catch (error) {
            set({
              error: error instanceof Error ? error.message : 'Đăng nhập thất bại',
              isLoading: false
            })
            throw error
          }
        },

        logout: () => {
          set({ user: null, token: null, error: null })
          // Clear persisted state
          localStorage.removeItem('auth-storage')
        },

        refreshToken: async () => {
          const { token } = get()
          if (!token) return

          try {
            const response = await apiClient.auth.refreshToken()
            set({ token: response.token })
          } catch (error) {
            // Token refresh failed, logout user
            get().logout()
            throw error
          }
        },

        clearError: () => set({ error: null }),
        setUser: (user) => set({ user })
      }),
      {
        name: 'auth-storage',
        partialize: (state) => ({
          user: state.user,
          token: state.token
        })
      }
    ),
    { name: 'auth-store' }
  )
)

// Selectors
export const useAuth = () => useAuthStore((state) => ({
  user: state.user,
  isAuthenticated: !!state.user,
  isLoading: state.isLoading
}))

export const useAuthActions = () => useAuthStore((state) => ({
  login: state.login,
  logout: state.logout,
  refreshToken: state.refreshToken,
  clearError: state.clearError
}))
```

### 2. **Store Composition Pattern**

#### ✅ **Đúng:**
```typescript
// store/index.ts - Store composition
import { useAuthStore } from './authStore'
import { useUserStore } from './userStore'
import { usePermissionStore } from './permissionStore'

// Combined selectors
export const useAppState = () => ({
  auth: useAuthStore((state) => ({
    user: state.user,
    isAuthenticated: !!state.user,
    isLoading: state.isLoading
  })),
  users: useUserStore((state) => ({
    users: state.users,
    isLoading: state.isLoading
  })),
  permissions: usePermissionStore((state) => ({
    permissions: state.permissions,
    roles: state.roles
  }))
})

// Combined actions
export const useAppActions = () => ({
  auth: useAuthStore((state) => ({
    login: state.login,
    logout: state.logout
  })),
  users: useUserStore((state) => ({
    fetchUsers: state.fetchUsers,
    createUser: state.createUser
  })),
  permissions: usePermissionStore((state) => ({
    checkPermission: state.checkPermission,
    hasRole: state.hasRole
  }))
})
```

## 🔐 **RBAC Implementation Patterns**

### 1. **Permission Guard Component**

#### ✅ **Đúng:**
```typescript
// components/common/PermissionGuard.tsx
import React from 'react'
import { usePermissions } from '@/lib/hooks'
import { Permission, UserRole } from '@/lib/rbac/permissions'

interface PermissionGuardProps {
  permission?: Permission
  permissions?: Permission[]
  role?: UserRole
  roles?: UserRole[]
  requireAll?: boolean
  fallback?: React.ReactNode
  children: React.ReactNode
}

/**
 * Component bảo vệ nội dung dựa trên permissions hoặc roles
 * @param permission - Permission đơn lẻ cần kiểm tra
 * @param permissions - Danh sách permissions (kiểm tra ANY hoặc ALL)
 * @param role - Role đơn lẻ cần kiểm tra
 * @param roles - Danh sách roles cần kiểm tra
 * @param requireAll - Yêu cầu tất cả permissions/roles (default: false)
 * @param fallback - Component hiển thị khi không có quyền
 * @param children - Nội dung được bảo vệ
 */
export const PermissionGuard: React.FC<PermissionGuardProps> = ({
  permission,
  permissions = [],
  role,
  roles = [],
  requireAll = false,
  fallback = null,
  children
}) => {
  const { hasPermission, hasAnyPermission, hasAllPermissions, hasRole, hasAnyRole } = usePermissions()

  let hasAccess = true

  // Check single permission
  if (permission) {
    hasAccess = hasPermission(permission)
  }

  // Check multiple permissions
  if (permissions.length > 0) {
    hasAccess = requireAll
      ? hasAllPermissions(permissions)
      : hasAnyPermission(permissions)
  }

  // Check single role
  if (role) {
    hasAccess = hasAccess && hasRole(role)
  }

  // Check multiple roles
  if (roles.length > 0) {
    hasAccess = hasAccess && hasAnyRole(roles)
  }

  if (!hasAccess) {
    return <>{fallback}</>
  }

  return <>{children}</>
}
```

### 2. **Role-Based Routing**

#### ✅ **Đúng:**
```typescript
// lib/guards/RouteGuard.tsx
import React, { useEffect } from 'react'
import { useRouter } from 'next/navigation'
import { useAuth, usePermissions } from '@/lib/hooks'
import { Permission, UserRole } from '@/lib/rbac/permissions'
import { LoadingSpinner } from '@/components/common'

interface RouteGuardProps {
  requiredPermissions?: Permission[]
  allowedRoles?: UserRole[]
  requireAuth?: boolean
  redirectTo?: string
  children: React.ReactNode
}

export const RouteGuard: React.FC<RouteGuardProps> = ({
  requiredPermissions = [],
  allowedRoles = [],
  requireAuth = true,
  redirectTo = '/login',
  children
}) => {
  const router = useRouter()
  const { user, isLoading } = useAuth()
  const { hasAnyPermission, hasAnyRole } = usePermissions()

  useEffect(() => {
    // Chờ auth state load xong
    if (isLoading) return

    // Kiểm tra authentication
    if (requireAuth && !user) {
      router.push(redirectTo)
      return
    }

    // Kiểm tra roles
    if (allowedRoles.length > 0 && !hasAnyRole(allowedRoles)) {
      router.push('/access-denied')
      return
    }

    // Kiểm tra permissions
    if (requiredPermissions.length > 0 && !hasAnyPermission(requiredPermissions)) {
      router.push('/access-denied')
      return
    }
  }, [user, isLoading, hasAnyPermission, hasAnyRole, router, redirectTo, allowedRoles, requiredPermissions, requireAuth])

  // Show loading while checking
  if (isLoading) {
    return <LoadingSpinner />
  }

  // Show nothing while redirecting
  if (requireAuth && !user) {
    return null
  }

  if (allowedRoles.length > 0 && !hasAnyRole(allowedRoles)) {
    return null
  }

  if (requiredPermissions.length > 0 && !hasAnyPermission(requiredPermissions)) {
    return null
  }

  return <>{children}</>
}
```

## 🧪 **Testing Conventions**

### 1. **Component Testing**

#### ✅ **Đúng:**
```typescript
// __tests__/components/UserCard.test.tsx
import { render, screen, fireEvent } from '@testing-library/react'
import userEvent from '@testing-library/user-event'
import { UserCard } from '@/components/UserCard'
import { TestProviders } from '@/test-utils'

const mockUser = {
  id: '1',
  name: 'Nguyễn Văn A',
  email: 'nguyenvana@example.com',
  role: 'user' as const,
  isActive: true,
  createdAt: '2023-01-01T00:00:00Z',
  updatedAt: '2023-01-01T00:00:00Z'
}

describe('UserCard', () => {
  const mockOnEdit = jest.fn()
  const mockOnDelete = jest.fn()

  beforeEach(() => {
    jest.clearAllMocks()
  })

  it('hiển thị thông tin user đúng cách', () => {
    render(
      <TestProviders userRole="admin">
        <UserCard user={mockUser} onEdit={mockOnEdit} onDelete={mockOnDelete} />
      </TestProviders>
    )

    expect(screen.getByText('Nguyễn Văn A')).toBeInTheDocument()
    expect(screen.getByText('nguyenvana@example.com')).toBeInTheDocument()
    expect(screen.getByText('user')).toBeInTheDocument()
  })

  it('hiển thị nút chỉnh sửa cho admin', () => {
    render(
      <TestProviders userRole="admin">
        <UserCard user={mockUser} onEdit={mockOnEdit} onDelete={mockOnDelete} />
      </TestProviders>
    )

    expect(screen.getByText('Chỉnh sửa')).toBeInTheDocument()
  })

  it('ẩn nút chỉnh sửa cho user thường', () => {
    render(
      <TestProviders userRole="user">
        <UserCard user={mockUser} onEdit={mockOnEdit} onDelete={mockOnDelete} />
      </TestProviders>
    )

    expect(screen.queryByText('Chỉnh sửa')).not.toBeInTheDocument()
  })

  it('gọi onEdit khi click nút chỉnh sửa', async () => {
    const user = userEvent.setup()

    render(
      <TestProviders userRole="admin">
        <UserCard user={mockUser} onEdit={mockOnEdit} onDelete={mockOnDelete} />
      </TestProviders>
    )

    await user.click(screen.getByText('Chỉnh sửa'))
    expect(mockOnEdit).toHaveBeenCalledWith('1')
  })
})
```

### 2. **Hook Testing**

#### ✅ **Đúng:**
```typescript
// __tests__/hooks/useUserActions.test.ts
import { renderHook, waitFor } from '@testing-library/react'
import { QueryClient, QueryClientProvider } from '@tanstack/react-query'
import { useUserActions } from '@/hooks/useUserActions'
import { apiClient } from '@/lib/api-client'

// Mock API client
jest.mock('@/lib/api-client')
const mockApiClient = apiClient as jest.Mocked<typeof apiClient>

const createWrapper = () => {
  const queryClient = new QueryClient({
    defaultOptions: {
      queries: { retry: false },
      mutations: { retry: false }
    }
  })

  return ({ children }: { children: React.ReactNode }) => (
    <QueryClientProvider client={queryClient}>
      {children}
    </QueryClientProvider>
  )
}

describe('useUserActions', () => {
  beforeEach(() => {
    jest.clearAllMocks()
  })

  it('tạo user thành công', async () => {
    const mockUser = { id: '1', name: 'Test User', email: 'test@example.com' }
    mockApiClient.users.create.mockResolvedValue(mockUser)

    const { result } = renderHook(() => useUserActions(), {
      wrapper: createWrapper()
    })

    const userData = { name: 'Test User', email: 'test@example.com', role: 'user' }

    await waitFor(async () => {
      const createdUser = await result.current.createUser(userData)
      expect(createdUser).toEqual(mockUser)
    })

    expect(mockApiClient.users.create).toHaveBeenCalledWith(userData)
  })

  it('xử lý lỗi khi tạo user thất bại', async () => {
    const errorMessage = 'Email đã tồn tại'
    mockApiClient.users.create.mockRejectedValue(new Error(errorMessage))

    const { result } = renderHook(() => useUserActions(), {
      wrapper: createWrapper()
    })

    const userData = { name: 'Test User', email: 'test@example.com', role: 'user' }

    await expect(result.current.createUser(userData)).rejects.toThrow(errorMessage)
  })
})
```

## 🎨 **Styling và CSS Conventions**

### 1. **Tailwind CSS Best Practices**

#### ✅ **Đúng:**
```typescript
// Sử dụng Tailwind classes có tổ chức
export const UserCard: React.FC<UserCardProps> = ({ user, className }) => {
  return (
    <div className={cn(
      // Base styles
      "bg-white rounded-lg shadow-md border border-gray-200",
      // Layout
      "p-6 flex items-center justify-between",
      // Hover effects
      "hover:shadow-lg transition-shadow duration-200",
      // Responsive
      "sm:p-4 md:p-6",
      // Custom className
      className
    )}>
      <div className="flex items-center space-x-4">
        <img
          src={user.avatar || '/default-avatar.png'}
          alt={user.name}
          className="w-12 h-12 rounded-full object-cover"
        />
        <div>
          <h3 className="text-lg font-semibold text-gray-900">
            {user.name}
          </h3>
          <p className="text-sm text-gray-600">
            {user.email}
          </p>
        </div>
      </div>
    </div>
  )
}
```

### 2. **CSS Modules (nếu cần)**

#### ✅ **Đúng:**
```css
/* styles/UserCard.module.css */
.userCard {
  @apply bg-white rounded-lg shadow-md border border-gray-200 p-6;
  @apply flex items-center justify-between;
  @apply hover:shadow-lg transition-shadow duration-200;
}

.userInfo {
  @apply flex items-center space-x-4;
}

.avatar {
  @apply w-12 h-12 rounded-full object-cover;
}

.userName {
  @apply text-lg font-semibold text-gray-900;
}

.userEmail {
  @apply text-sm text-gray-600;
}
```

## 📏 **Linting và Formatting**

### 1. **ESLint Configuration**

```json
// .eslintrc.json
{
  "extends": [
    "next/core-web-vitals",
    "@typescript-eslint/recommended",
    "prettier"
  ],
  "parser": "@typescript-eslint/parser",
  "plugins": ["@typescript-eslint", "react-hooks"],
  "rules": {
    // TypeScript rules
    "@typescript-eslint/no-unused-vars": "error",
    "@typescript-eslint/no-explicit-any": "warn",
    "@typescript-eslint/prefer-const": "error",

    // React rules
    "react-hooks/rules-of-hooks": "error",
    "react-hooks/exhaustive-deps": "warn",
    "react/prop-types": "off",
    "react/react-in-jsx-scope": "off",

    // General rules
    "prefer-const": "error",
    "no-var": "error",
    "no-console": "warn",
    "no-debugger": "error"
  },
  "overrides": [
    {
      "files": ["**/*.test.ts", "**/*.test.tsx"],
      "env": {
        "jest": true
      },
      "rules": {
        "@typescript-eslint/no-explicit-any": "off"
      }
    }
  ]
}
```

### 2. **Prettier Configuration**

```json
// .prettierrc
{
  "semi": false,
  "trailingComma": "es5",
  "singleQuote": true,
  "tabWidth": 2,
  "useTabs": false,
  "printWidth": 80,
  "bracketSpacing": true,
  "arrowParens": "avoid",
  "endOfLine": "lf"
}
```

## ✅ **Code Review Checklist**

### 1. **Component Review**
- [ ] Component name sử dụng PascalCase
- [ ] Props interface được định nghĩa rõ ràng
- [ ] Sử dụng TypeScript types đúng cách
- [ ] Có JSDoc comments cho complex components
- [ ] Sử dụng RBAC guards khi cần thiết
- [ ] Performance optimization (memo, useMemo, useCallback)
- [ ] Accessibility attributes (aria-*, role)
- [ ] Error boundaries được implement

### 2. **Hook Review**
- [ ] Hook name bắt đầu với "use"
- [ ] Return type được định nghĩa rõ ràng
- [ ] Dependencies array được optimize
- [ ] Error handling được implement
- [ ] Permission checking được tích hợp

### 3. **Type Review**
- [ ] Sử dụng interface cho object types
- [ ] Sử dụng type cho unions và primitives
- [ ] Generic types được sử dụng đúng cách
- [ ] Utility types được leverage
- [ ] Consistent naming conventions

### 4. **RBAC Review**
- [ ] Permission guards được sử dụng đúng chỗ
- [ ] Route protection được implement
- [ ] Role-based rendering được áp dụng
- [ ] Permission checking trong hooks
- [ ] Fallback UI cho unauthorized access

### 5. **Testing Review**
- [ ] Unit tests cho components
- [ ] Hook testing với proper mocking
- [ ] RBAC scenarios được test
- [ ] Error cases được cover
- [ ] Test descriptions bằng tiếng Việt

---

**💡 Lưu ý**: Tuân thủ các quy tắc này sẽ đảm bảo code quality cao, maintainability tốt, và team collaboration hiệu quả!
