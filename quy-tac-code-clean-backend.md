# Quy Tắc Code Clean - Backend NestJS với RBAC System

## 📋 **Tổng Quan**

Tài liệu này định nghĩa các quy tắc code clean cho dự án NestJS backend với Role-Based Access Control (RBAC) system và module-based architecture.

## 🎯 **Nguyên Tắc Cơ Bản**

### 1. **Single Responsibility Principle**
- Mỗi class chỉ có một lý do để thay đổi
- Controllers chỉ xử lý HTTP requests/responses
- Services chứa business logic
- Repositories xử lý data access

### 2. **Dependency Injection**
- Sử dụng NestJS DI container
- Inject dependencies qua constructor
- Sử dụng interfaces cho loose coupling

### 3. **Error Handling**
- Sử dụng NestJS exception filters
- Throw appropriate HTTP exceptions
- Log errors với context đầy đủ

## 📝 **Naming Conventions**

### 1. **Files và Classes**

#### ✅ **Đúng:**
```typescript
// Controllers
UsersController
AuthController
ProjectsController

// Services
UsersService
AuthService
EmailService

// DTOs
CreateUserDto
UpdateUserDto
LoginDto

// Entities
User
Role
Permission

// Guards
JwtAuthGuard
RolesGuard
PermissionsGuard

// Decorators
@Roles()
@RequirePermissions()
@CurrentUser()
```

#### ❌ **Sai:**
```typescript
// Không sử dụng
userController
Users_Service
createUserDTO
user_entity
```

### 2. **Methods và Variables**

#### ✅ **Đúng:**
```typescript
// camelCase cho methods và variables
async findAllUsers(): Promise<User[]>
async createUser(createUserDto: CreateUserDto): Promise<User>
const isUserActive = user.isActive
const hasPermission = this.checkPermission(user, permission)

// SCREAMING_SNAKE_CASE cho constants
const JWT_SECRET = process.env.JWT_SECRET
const DEFAULT_PAGE_SIZE = 10
const MAX_FILE_SIZE = 5 * 1024 * 1024 // 5MB
```

### 3. **Database Conventions**

#### ✅ **Đúng:**
```sql
-- Table names: snake_case, plural
users
user_roles
role_permissions
audit_logs

-- Column names: snake_case
user_id
created_at
updated_at
is_active

-- Index names: descriptive
idx_users_email
idx_users_role_id
idx_audit_logs_created_at
```

## 🏗️ **Module Organization Patterns**

### 1. **Module Structure Standard**

```typescript
// modules/users/users.module.ts
import { Module } from '@nestjs/common'
import { TypeOrmModule } from '@nestjs/typeorm'
import { UsersController } from './users.controller'
import { UsersService } from './users.service'
import { User } from './entities/user.entity'
import { UsersRepository } from './repositories/users.repository'

@Module({
  imports: [TypeOrmModule.forFeature([User])],
  controllers: [UsersController],
  providers: [UsersService, UsersRepository],
  exports: [UsersService] // Export services cần thiết
})
export class UsersModule {}
```

### 2. **Barrel Exports Pattern**

#### ✅ **Đúng:**
```typescript
// modules/users/dto/index.ts
export { CreateUserDto } from './create-user.dto'
export { UpdateUserDto } from './update-user.dto'
export { UserResponseDto } from './user-response.dto'
export { UserQueryDto } from './user-query.dto'

// modules/users/index.ts
export * from './dto'
export * from './entities'
export { UsersService } from './users.service'
export { UsersModule } from './users.module'
```

## 🎮 **Controllers Best Practices**

### 1. **Controller Structure**

#### ✅ **Đúng:**
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
  HttpStatus,
  ParseUUIDPipe
} from '@nestjs/common'
import {
  ApiTags,
  ApiOperation,
  ApiResponse,
  ApiBearerAuth,
  ApiParam,
  ApiQuery
} from '@nestjs/swagger'
import { JwtAuthGuard } from '@/common/guards'
import { RolesGuard } from '@/common/guards'
import { PermissionsGuard } from '@/common/guards'
import { Roles } from '@/common/decorators'
import { RequirePermissions } from '@/common/decorators'
import { CurrentUser } from '@/common/decorators'
import { Role, Permission } from '@/common/constants'
import { UsersService } from './users.service'
import { CreateUserDto, UpdateUserDto, UserQueryDto, UserResponseDto } from './dto'

@ApiTags('Users')
@ApiBearerAuth()
@Controller('users')
@UseGuards(JwtAuthGuard, RolesGuard, PermissionsGuard)
export class UsersController {
  constructor(private readonly usersService: UsersService) {}

  @Get()
  @ApiOperation({ 
    summary: 'Lấy danh sách users',
    description: 'Trả về danh sách users với phân trang và filter'
  })
  @ApiQuery({ name: 'page', required: false, type: Number })
  @ApiQuery({ name: 'limit', required: false, type: Number })
  @ApiResponse({ 
    status: HttpStatus.OK, 
    description: 'Danh sách users được trả về thành công',
    type: [UserResponseDto]
  })
  @RequirePermissions(Permission.USER_READ)
  async findAll(@Query() query: UserQueryDto): Promise<UserResponseDto[]> {
    return this.usersService.findAll(query)
  }

  @Get(':id')
  @ApiOperation({ summary: 'Lấy thông tin user theo ID' })
  @ApiParam({ name: 'id', description: 'User ID', type: 'string' })
  @ApiResponse({ 
    status: HttpStatus.OK, 
    description: 'Thông tin user',
    type: UserResponseDto
  })
  @ApiResponse({ 
    status: HttpStatus.NOT_FOUND, 
    description: 'User không tồn tại'
  })
  @RequirePermissions(Permission.USER_READ)
  async findOne(
    @Param('id', ParseUUIDPipe) id: string,
    @CurrentUser() currentUser: any
  ): Promise<UserResponseDto> {
    return this.usersService.findOne(id, currentUser)
  }

  @Post()
  @ApiOperation({ summary: 'Tạo user mới' })
  @ApiResponse({ 
    status: HttpStatus.CREATED, 
    description: 'User được tạo thành công',
    type: UserResponseDto
  })
  @ApiResponse({ 
    status: HttpStatus.BAD_REQUEST, 
    description: 'Dữ liệu đầu vào không hợp lệ'
  })
  @Roles(Role.ADMIN)
  @RequirePermissions(Permission.USER_CREATE)
  async create(@Body() createUserDto: CreateUserDto): Promise<UserResponseDto> {
    return this.usersService.create(createUserDto)
  }

  @Put(':id')
  @ApiOperation({ summary: 'Cập nhật thông tin user' })
  @ApiParam({ name: 'id', description: 'User ID', type: 'string' })
  @ApiResponse({ 
    status: HttpStatus.OK, 
    description: 'User được cập nhật thành công',
    type: UserResponseDto
  })
  @RequirePermissions(Permission.USER_UPDATE)
  async update(
    @Param('id', ParseUUIDPipe) id: string,
    @Body() updateUserDto: UpdateUserDto,
    @CurrentUser() currentUser: any
  ): Promise<UserResponseDto> {
    return this.usersService.update(id, updateUserDto, currentUser)
  }

  @Delete(':id')
  @ApiOperation({ summary: 'Xóa user' })
  @ApiParam({ name: 'id', description: 'User ID', type: 'string' })
  @ApiResponse({ 
    status: HttpStatus.NO_CONTENT, 
    description: 'User được xóa thành công'
  })
  @Roles(Role.ADMIN)
  @RequirePermissions(Permission.USER_DELETE)
  async remove(@Param('id', ParseUUIDPipe) id: string): Promise<void> {
    await this.usersService.remove(id)
  }
}
```

### 2. **Error Handling trong Controllers**

#### ✅ **Đúng:**
```typescript
// Controllers không nên handle business logic errors
// Để services throw exceptions, controllers chỉ catch và transform nếu cần

@Post()
async create(@Body() createUserDto: CreateUserDto): Promise<UserResponseDto> {
  // Service sẽ throw appropriate exceptions
  return this.usersService.create(createUserDto)
}

// Nếu cần custom error handling
@Post()
async create(@Body() createUserDto: CreateUserDto): Promise<UserResponseDto> {
  try {
    return await this.usersService.create(createUserDto)
  } catch (error) {
    // Log error với context
    this.logger.error(`Failed to create user: ${error.message}`, {
      dto: createUserDto,
      error: error.stack
    })
    
    // Re-throw hoặc transform error
    throw error
  }
}
```

## 🔧 **Services Best Practices**

### 1. **Service Structure**

#### ✅ **Đúng:**
```typescript
// modules/users/users.service.ts
import { 
  Injectable, 
  NotFoundException, 
  ForbiddenException,
  ConflictException,
  Logger
} from '@nestjs/common'
import { InjectRepository } from '@nestjs/typeorm'
import { Repository } from 'typeorm'
import * as bcrypt from 'bcrypt'
import { User } from './entities/user.entity'
import { CreateUserDto, UpdateUserDto, UserQueryDto, UserResponseDto } from './dto'
import { Role, Permission } from '@/common/constants'
import { PaginationDto } from '@/common/dto'

@Injectable()
export class UsersService {
  private readonly logger = new Logger(UsersService.name)

  constructor(
    @InjectRepository(User)
    private readonly userRepository: Repository<User>
  ) {}

  /**
   * Lấy danh sách users với phân trang và filter
   * @param query - Query parameters cho filter và pagination
   * @returns Danh sách users với metadata
   */
  async findAll(query: UserQueryDto): Promise<{
    data: UserResponseDto[]
    meta: PaginationDto
  }> {
    const { page = 1, limit = 10, search, role, isActive } = query
    const skip = (page - 1) * limit

    // Build query với QueryBuilder để tối ưu performance
    const queryBuilder = this.userRepository
      .createQueryBuilder('user')
      .leftJoinAndSelect('user.roles', 'role')
      .leftJoinAndSelect('role.permissions', 'permission')

    // Apply filters
    if (search) {
      queryBuilder.andWhere(
        '(user.name ILIKE :search OR user.email ILIKE :search)',
        { search: `%${search}%` }
      )
    }

    if (role) {
      queryBuilder.andWhere('role.name = :role', { role })
    }

    if (isActive !== undefined) {
      queryBuilder.andWhere('user.isActive = :isActive', { isActive })
    }

    // Apply pagination
    queryBuilder.skip(skip).take(limit)

    // Execute query
    const [users, total] = await queryBuilder.getManyAndCount()

    // Transform to response DTOs
    const data = users.map(user => this.toResponseDto(user))

    return {
      data,
      meta: {
        page,
        limit,
        total,
        totalPages: Math.ceil(total / limit)
      }
    }
  }

  /**
   * Lấy thông tin user theo ID với permission checking
   * @param id - User ID
   * @param currentUser - User hiện tại đang request
   * @returns Thông tin user
   */
  async findOne(id: string, currentUser: any): Promise<UserResponseDto> {
    const user = await this.userRepository.findOne({
      where: { id },
      relations: ['roles', 'roles.permissions']
    })

    if (!user) {
      throw new NotFoundException('User không tồn tại')
    }

    // Permission checking: users chỉ có thể xem profile của mình
    // trừ khi có permission USER_READ
    if (currentUser.id !== id && !this.hasPermission(currentUser, Permission.USER_READ)) {
      throw new ForbiddenException('Không có quyền truy cập thông tin user này')
    }

    return this.toResponseDto(user)
  }

  /**
   * Tạo user mới
   * @param createUserDto - Dữ liệu tạo user
   * @returns User được tạo
   */
  async create(createUserDto: CreateUserDto): Promise<UserResponseDto> {
    const { email, password, ...userData } = createUserDto

    // Kiểm tra email đã tồn tại
    const existingUser = await this.userRepository.findOne({ 
      where: { email } 
    })
    
    if (existingUser) {
      throw new ConflictException('Email đã được sử dụng')
    }

    // Hash password
    const hashedPassword = await bcrypt.hash(password, 10)

    // Tạo user entity
    const user = this.userRepository.create({
      ...userData,
      email,
      password: hashedPassword
    })

    // Save user
    const savedUser = await this.userRepository.save(user)

    this.logger.log(`User created successfully: ${savedUser.id}`)

    return this.toResponseDto(savedUser)
  }

  /**
   * Cập nhật thông tin user
   * @param id - User ID
   * @param updateUserDto - Dữ liệu cập nhật
   * @param currentUser - User hiện tại
   * @returns User được cập nhật
   */
  async update(
    id: string, 
    updateUserDto: UpdateUserDto, 
    currentUser: any
  ): Promise<UserResponseDto> {
    const user = await this.findUserById(id)

    // Permission checking
    if (currentUser.id !== id && !this.hasPermission(currentUser, Permission.USER_UPDATE)) {
      throw new ForbiddenException('Không có quyền cập nhật user này')
    }

    const { password, ...updateData } = updateUserDto

    // Hash password nếu có
    if (password) {
      updateData.password = await bcrypt.hash(password, 10)
    }

    // Update user
    Object.assign(user, updateData)
    const updatedUser = await this.userRepository.save(user)

    this.logger.log(`User updated successfully: ${updatedUser.id}`)

    return this.toResponseDto(updatedUser)
  }

  /**
   * Xóa user
   * @param id - User ID
   */
  async remove(id: string): Promise<void> {
    const user = await this.findUserById(id)

    await this.userRepository.remove(user)

    this.logger.log(`User deleted successfully: ${id}`)
  }

  /**
   * Helper method: Tìm user theo ID
   * @param id - User ID
   * @returns User entity
   * @private
   */
  private async findUserById(id: string): Promise<User> {
    const user = await this.userRepository.findOne({
      where: { id },
      relations: ['roles', 'roles.permissions']
    })

    if (!user) {
      throw new NotFoundException('User không tồn tại')
    }

    return user
  }

  /**
   * Helper method: Kiểm tra permission
   * @param user - User object
   * @param permission - Permission cần kiểm tra
   * @returns Boolean
   * @private
   */
  private hasPermission(user: any, permission: Permission): boolean {
    return user.roles?.some((role: any) =>
      role.permissions?.some((perm: any) => perm.name === permission)
    )
  }

  /**
   * Helper method: Transform entity to response DTO
   * @param user - User entity
   * @returns UserResponseDto
   * @private
   */
  private toResponseDto(user: User): UserResponseDto {
    const { password, ...userWithoutPassword } = user
    return userWithoutPassword as UserResponseDto
  }
}
```

## 📊 **DTOs Best Practices**

### 1. **DTO Structure**

#### ✅ **Đúng:**
```typescript
// modules/users/dto/create-user.dto.ts
import {
  IsEmail,
  IsString,
  IsOptional,
  IsBoolean,
  MinLength,
  MaxLength,
  IsEnum,
  IsArray,
  ArrayNotEmpty
} from 'class-validator'
import { ApiProperty, ApiPropertyOptional } from '@nestjs/swagger'
import { Transform } from 'class-transformer'
import { Role } from '@/common/constants'

export class CreateUserDto {
  @ApiProperty({
    description: 'Tên đầy đủ của user',
    example: 'Nguyễn Văn A',
    minLength: 2,
    maxLength: 100
  })
  @IsString({ message: 'Tên phải là chuỗi ký tự' })
  @MinLength(2, { message: 'Tên phải có ít nhất 2 ký tự' })
  @MaxLength(100, { message: 'Tên không được vượt quá 100 ký tự' })
  @Transform(({ value }) => value?.trim())
  name: string

  @ApiProperty({
    description: 'Email của user',
    example: 'nguyenvana@example.com',
    format: 'email'
  })
  @IsEmail({}, { message: 'Email không hợp lệ' })
  @Transform(({ value }) => value?.toLowerCase().trim())
  email: string

  @ApiProperty({
    description: 'Mật khẩu của user',
    example: 'SecurePassword123!',
    minLength: 8
  })
  @IsString({ message: 'Mật khẩu phải là chuỗi ký tự' })
  @MinLength(8, { message: 'Mật khẩu phải có ít nhất 8 ký tự' })
  password: string

  @ApiPropertyOptional({
    description: 'URL avatar của user',
    example: 'https://example.com/avatar.jpg'
  })
  @IsOptional()
  @IsString({ message: 'Avatar phải là chuỗi ký tự' })
  avatar?: string

  @ApiPropertyOptional({
    description: 'Trạng thái hoạt động của user',
    example: true,
    default: true
  })
  @IsOptional()
  @IsBoolean({ message: 'isActive phải là boolean' })
  isActive?: boolean = true

  @ApiPropertyOptional({
    description: 'Danh sách roles của user',
    example: [Role.USER],
    enum: Role,
    isArray: true
  })
  @IsOptional()
  @IsArray({ message: 'Roles phải là array' })
  @IsEnum(Role, { each: true, message: 'Role không hợp lệ' })
  roles?: Role[]
}

// modules/users/dto/update-user.dto.ts
import { PartialType, OmitType } from '@nestjs/swagger'
import { CreateUserDto } from './create-user.dto'

export class UpdateUserDto extends PartialType(
  OmitType(CreateUserDto, ['email'] as const)
) {
  // Email không được update sau khi tạo
  // Tất cả fields khác đều optional
}

// modules/users/dto/user-query.dto.ts
import { IsOptional, IsString, IsBoolean, IsEnum, IsInt, Min, Max } from 'class-validator'
import { Transform, Type } from 'class-transformer'
import { ApiPropertyOptional } from '@nestjs/swagger'
import { Role } from '@/common/constants'

export class UserQueryDto {
  @ApiPropertyOptional({
    description: 'Số trang',
    example: 1,
    minimum: 1
  })
  @IsOptional()
  @Type(() => Number)
  @IsInt({ message: 'Page phải là số nguyên' })
  @Min(1, { message: 'Page phải lớn hơn 0' })
  page?: number = 1

  @ApiPropertyOptional({
    description: 'Số lượng items per page',
    example: 10,
    minimum: 1,
    maximum: 100
  })
  @IsOptional()
  @Type(() => Number)
  @IsInt({ message: 'Limit phải là số nguyên' })
  @Min(1, { message: 'Limit phải lớn hơn 0' })
  @Max(100, { message: 'Limit không được vượt quá 100' })
  limit?: number = 10

  @ApiPropertyOptional({
    description: 'Từ khóa tìm kiếm (tên hoặc email)',
    example: 'nguyen'
  })
  @IsOptional()
  @IsString({ message: 'Search phải là chuỗi ký tự' })
  @Transform(({ value }) => value?.trim())
  search?: string

  @ApiPropertyOptional({
    description: 'Filter theo role',
    example: Role.USER,
    enum: Role
  })
  @IsOptional()
  @IsEnum(Role, { message: 'Role không hợp lệ' })
  role?: Role

  @ApiPropertyOptional({
    description: 'Filter theo trạng thái hoạt động',
    example: true
  })
  @IsOptional()
  @Transform(({ value }) => {
    if (value === 'true') return true
    if (value === 'false') return false
    return value
  })
  @IsBoolean({ message: 'isActive phải là boolean' })
  isActive?: boolean
}

// modules/users/dto/user-response.dto.ts
import { ApiProperty, ApiPropertyOptional } from '@nestjs/swagger'
import { Exclude, Expose, Type } from 'class-transformer'
import { Role } from '@/common/constants'

export class UserResponseDto {
  @ApiProperty({
    description: 'User ID',
    example: '123e4567-e89b-12d3-a456-426614174000'
  })
  @Expose()
  id: string

  @ApiProperty({
    description: 'Tên đầy đủ của user',
    example: 'Nguyễn Văn A'
  })
  @Expose()
  name: string

  @ApiProperty({
    description: 'Email của user',
    example: 'nguyenvana@example.com'
  })
  @Expose()
  email: string

  @ApiPropertyOptional({
    description: 'URL avatar của user',
    example: 'https://example.com/avatar.jpg'
  })
  @Expose()
  avatar?: string

  @ApiProperty({
    description: 'Trạng thái hoạt động của user',
    example: true
  })
  @Expose()
  isActive: boolean

  @ApiProperty({
    description: 'Danh sách roles của user',
    example: [Role.USER],
    enum: Role,
    isArray: true
  })
  @Expose()
  @Type(() => String)
  roles: Role[]

  @ApiProperty({
    description: 'Thời gian tạo',
    example: '2023-01-01T00:00:00.000Z'
  })
  @Expose()
  createdAt: Date

  @ApiProperty({
    description: 'Thời gian cập nhật cuối',
    example: '2023-01-01T00:00:00.000Z'
  })
  @Expose()
  updatedAt: Date

  // Exclude sensitive fields
  @Exclude()
  password: string
}
```

### 2. **Custom Validation Decorators**

#### ✅ **Đúng:**
```typescript
// common/decorators/validation.decorator.ts
import {
  registerDecorator,
  ValidationOptions,
  ValidationArguments,
  ValidatorConstraint,
  ValidatorConstraintInterface
} from 'class-validator'

// Custom validator cho strong password
@ValidatorConstraint({ name: 'isStrongPassword', async: false })
export class IsStrongPasswordConstraint implements ValidatorConstraintInterface {
  validate(password: string, args: ValidationArguments) {
    if (!password) return false

    // Ít nhất 8 ký tự, có chữ hoa, chữ thường, số và ký tự đặc biệt
    const strongPasswordRegex = /^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*[@$!%*?&])[A-Za-z\d@$!%*?&]{8,}$/
    return strongPasswordRegex.test(password)
  }

  defaultMessage(args: ValidationArguments) {
    return 'Mật khẩu phải có ít nhất 8 ký tự, bao gồm chữ hoa, chữ thường, số và ký tự đặc biệt'
  }
}

export function IsStrongPassword(validationOptions?: ValidationOptions) {
  return function (object: Object, propertyName: string) {
    registerDecorator({
      target: object.constructor,
      propertyName: propertyName,
      options: validationOptions,
      constraints: [],
      validator: IsStrongPasswordConstraint,
    })
  }
}

// Usage trong DTO
export class CreateUserDto {
  @IsStrongPassword()
  password: string
}
```

## 🗄️ **Database và Entities Best Practices**

### 1. **Entity Definition**

#### ✅ **Đúng:**
```typescript
// database/entities/user.entity.ts
import {
  Entity,
  PrimaryGeneratedColumn,
  Column,
  CreateDateColumn,
  UpdateDateColumn,
  ManyToMany,
  JoinTable,
  OneToMany,
  Index
} from 'typeorm'
import { Exclude } from 'class-transformer'
import { Role } from './role.entity'
import { AuditLog } from './audit-log.entity'

@Entity('users')
@Index(['email'], { unique: true })
@Index(['isActive'])
@Index(['createdAt'])
export class User {
  @PrimaryGeneratedColumn('uuid')
  id: string

  @Column({ type: 'varchar', length: 100 })
  name: string

  @Column({ type: 'varchar', length: 255, unique: true })
  email: string

  @Column({ type: 'varchar', length: 255 })
  @Exclude() // Exclude từ serialization
  password: string

  @Column({ type: 'varchar', length: 500, nullable: true })
  avatar?: string

  @Column({ type: 'boolean', default: true })
  isActive: boolean

  @CreateDateColumn({ type: 'timestamp with time zone' })
  createdAt: Date

  @UpdateDateColumn({ type: 'timestamp with time zone' })
  updatedAt: Date

  // Relationships
  @ManyToMany(() => Role, role => role.users, { eager: false })
  @JoinTable({
    name: 'user_roles',
    joinColumn: { name: 'user_id', referencedColumnName: 'id' },
    inverseJoinColumn: { name: 'role_id', referencedColumnName: 'id' }
  })
  roles: Role[]

  @OneToMany(() => AuditLog, auditLog => auditLog.user)
  auditLogs: AuditLog[]

  // Virtual properties
  get fullName(): string {
    return this.name
  }

  // Helper methods
  hasRole(roleName: string): boolean {
    return this.roles?.some(role => role.name === roleName) || false
  }

  hasPermission(permissionName: string): boolean {
    return this.roles?.some(role =>
      role.permissions?.some(permission => permission.name === permissionName)
    ) || false
  }
}
```

### 2. **Repository Pattern**

#### ✅ **Đúng:**
```typescript
// modules/users/repositories/users.repository.ts
import { Injectable } from '@nestjs/common'
import { InjectRepository } from '@nestjs/typeorm'
import { Repository, SelectQueryBuilder } from 'typeorm'
import { User } from '@/database/entities/user.entity'
import { UserQueryDto } from '../dto'

@Injectable()
export class UsersRepository {
  constructor(
    @InjectRepository(User)
    private readonly repository: Repository<User>
  ) {}

  /**
   * Tìm user theo email
   * @param email - Email của user
   * @returns User entity hoặc null
   */
  async findByEmail(email: string): Promise<User | null> {
    return this.repository.findOne({
      where: { email },
      relations: ['roles', 'roles.permissions']
    })
  }

  /**
   * Tìm user theo ID với relations
   * @param id - User ID
   * @returns User entity hoặc null
   */
  async findByIdWithRelations(id: string): Promise<User | null> {
    return this.repository.findOne({
      where: { id },
      relations: ['roles', 'roles.permissions']
    })
  }

  /**
   * Tìm users với query builder để tối ưu performance
   * @param query - Query parameters
   * @returns Query builder
   */
  createFindAllQueryBuilder(query: UserQueryDto): SelectQueryBuilder<User> {
    const { search, role, isActive } = query

    const queryBuilder = this.repository
      .createQueryBuilder('user')
      .leftJoinAndSelect('user.roles', 'role')
      .leftJoinAndSelect('role.permissions', 'permission')

    if (search) {
      queryBuilder.andWhere(
        '(user.name ILIKE :search OR user.email ILIKE :search)',
        { search: `%${search}%` }
      )
    }

    if (role) {
      queryBuilder.andWhere('role.name = :role', { role })
    }

    if (isActive !== undefined) {
      queryBuilder.andWhere('user.isActive = :isActive', { isActive })
    }

    return queryBuilder
  }

  /**
   * Đếm số lượng users active
   * @returns Số lượng users active
   */
  async countActiveUsers(): Promise<number> {
    return this.repository.count({
      where: { isActive: true }
    })
  }

  /**
   * Tìm users được tạo trong khoảng thời gian
   * @param startDate - Ngày bắt đầu
   * @param endDate - Ngày kết thúc
   * @returns Danh sách users
   */
  async findUsersCreatedBetween(startDate: Date, endDate: Date): Promise<User[]> {
    return this.repository
      .createQueryBuilder('user')
      .where('user.createdAt BETWEEN :startDate AND :endDate', {
        startDate,
        endDate
      })
      .orderBy('user.createdAt', 'DESC')
      .getMany()
  }
}
```

## 🛡️ **Guards và Decorators Best Practices**

### 1. **Custom Guards Implementation**

#### ✅ **Đúng:**
```typescript
// common/guards/permissions.guard.ts
import {
  Injectable,
  CanActivate,
  ExecutionContext,
  ForbiddenException,
  Logger
} from '@nestjs/common'
import { Reflector } from '@nestjs/core'
import { Permission, ROLE_PERMISSIONS } from '@/common/constants'
import { PERMISSIONS_KEY } from '@/common/decorators'

@Injectable()
export class PermissionsGuard implements CanActivate {
  private readonly logger = new Logger(PermissionsGuard.name)

  constructor(private reflector: Reflector) {}

  canActivate(context: ExecutionContext): boolean {
    const requiredPermissions = this.reflector.getAllAndOverride<Permission[]>(
      PERMISSIONS_KEY,
      [context.getHandler(), context.getClass()]
    )

    if (!requiredPermissions || requiredPermissions.length === 0) {
      return true
    }

    const request = context.switchToHttp().getRequest()
    const user = request.user

    if (!user) {
      this.logger.warn('User not found in request')
      throw new ForbiddenException('User not authenticated')
    }

    const userPermissions = this.getUserPermissions(user.roles || [])
    const hasPermission = requiredPermissions.every(permission =>
      userPermissions.includes(permission)
    )

    if (!hasPermission) {
      this.logger.warn(`User ${user.id} lacks required permissions: ${requiredPermissions.join(', ')}`)
      throw new ForbiddenException('Insufficient permissions')
    }

    return true
  }

  /**
   * Lấy danh sách permissions của user từ roles
   * @param userRoles - Danh sách roles của user
   * @returns Danh sách permissions
   * @private
   */
  private getUserPermissions(userRoles: string[]): Permission[] {
    const permissions = new Set<Permission>()

    userRoles.forEach(roleName => {
      const rolePermissions = ROLE_PERMISSIONS[roleName] || []
      rolePermissions.forEach(permission => permissions.add(permission))
    })

    return Array.from(permissions)
  }
}

// common/guards/resource-owner.guard.ts
import {
  Injectable,
  CanActivate,
  ExecutionContext,
  ForbiddenException,
  NotFoundException
} from '@nestjs/common'
import { InjectRepository } from '@nestjs/typeorm'
import { Repository } from 'typeorm'
import { User } from '@/database/entities/user.entity'

@Injectable()
export class ResourceOwnerGuard implements CanActivate {
  constructor(
    @InjectRepository(User)
    private readonly userRepository: Repository<User>
  ) {}

  async canActivate(context: ExecutionContext): Promise<boolean> {
    const request = context.switchToHttp().getRequest()
    const user = request.user
    const resourceId = request.params.id

    if (!user) {
      throw new ForbiddenException('User not authenticated')
    }

    // Admin có thể truy cập tất cả resources
    if (user.roles?.includes('admin')) {
      return true
    }

    // Kiểm tra ownership
    const resource = await this.userRepository.findOne({
      where: { id: resourceId }
    })

    if (!resource) {
      throw new NotFoundException('Resource not found')
    }

    // User chỉ có thể truy cập resource của mình
    if (resource.id !== user.id) {
      throw new ForbiddenException('Access denied: not resource owner')
    }

    return true
  }
}
```

### 2. **Custom Decorators**

#### ✅ **Đúng:**
```typescript
// common/decorators/audit-log.decorator.ts
import { SetMetadata } from '@nestjs/common'

export const AUDIT_LOG_KEY = 'audit_log'

export interface AuditLogOptions {
  action: string
  resource: string
  description?: string
}

/**
 * Decorator để tự động log audit cho actions
 * @param options - Audit log options
 */
export const AuditLog = (options: AuditLogOptions) =>
  SetMetadata(AUDIT_LOG_KEY, options)

// Usage
@Post()
@AuditLog({
  action: 'CREATE_USER',
  resource: 'User',
  description: 'Tạo user mới'
})
async create(@Body() createUserDto: CreateUserDto) {
  return this.usersService.create(createUserDto)
}

// common/decorators/api-response.decorator.ts
import { applyDecorators, Type } from '@nestjs/common'
import { ApiResponse, ApiResponseOptions } from '@nestjs/swagger'

/**
 * Decorator kết hợp cho API responses
 * @param options - Response options
 */
export const ApiResponseDto = <TModel extends Type<any>>(
  options: {
    status: number
    description: string
    type?: TModel
    isArray?: boolean
  }
) => {
  const responseOptions: ApiResponseOptions = {
    status: options.status,
    description: options.description
  }

  if (options.type) {
    responseOptions.type = options.type
    if (options.isArray) {
      responseOptions.schema = {
        type: 'array',
        items: { $ref: `#/components/schemas/${options.type.name}` }
      }
    }
  }

  return applyDecorators(ApiResponse(responseOptions))
}

// Usage
@Get()
@ApiResponseDto({
  status: 200,
  description: 'Danh sách users',
  type: UserResponseDto,
  isArray: true
})
async findAll() {
  return this.usersService.findAll()
}
```

## 🧪 **Testing Best Practices**

### 1. **Service Testing**

#### ✅ **Đúng:**
```typescript
// modules/users/users.service.spec.ts
import { Test, TestingModule } from '@nestjs/testing'
import { getRepositoryToken } from '@nestjs/typeorm'
import { Repository } from 'typeorm'
import { NotFoundException, ForbiddenException, ConflictException } from '@nestjs/common'
import { UsersService } from './users.service'
import { User } from '@/database/entities/user.entity'
import { CreateUserDto, UpdateUserDto } from './dto'
import { Role, Permission } from '@/common/constants'

describe('UsersService', () => {
  let service: UsersService
  let repository: Repository<User>

  const mockRepository = {
    findOne: jest.fn(),
    find: jest.fn(),
    create: jest.fn(),
    save: jest.fn(),
    remove: jest.fn(),
    createQueryBuilder: jest.fn(),
    count: jest.fn()
  }

  const mockUser: User = {
    id: '1',
    name: 'Test User',
    email: 'test@example.com',
    password: 'hashedPassword',
    isActive: true,
    roles: [{ name: Role.USER, permissions: [] }],
    createdAt: new Date(),
    updatedAt: new Date()
  } as User

  beforeEach(async () => {
    const module: TestingModule = await Test.createTestingModule({
      providers: [
        UsersService,
        {
          provide: getRepositoryToken(User),
          useValue: mockRepository
        }
      ]
    }).compile()

    service = module.get<UsersService>(UsersService)
    repository = module.get<Repository<User>>(getRepositoryToken(User))
  })

  afterEach(() => {
    jest.clearAllMocks()
  })

  describe('findOne', () => {
    it('nên trả về user khi tìm thấy và có quyền truy cập', async () => {
      const currentUser = {
        id: '1',
        roles: [{ permissions: [{ name: Permission.USER_READ }] }]
      }

      mockRepository.findOne.mockResolvedValue(mockUser)

      const result = await service.findOne('1', currentUser)

      expect(result).toBeDefined()
      expect(result.id).toBe('1')
      expect(result.password).toBeUndefined() // Password should be excluded
    })

    it('nên throw NotFoundException khi user không tồn tại', async () => {
      const currentUser = { id: '1', roles: [] }
      mockRepository.findOne.mockResolvedValue(null)

      await expect(service.findOne('999', currentUser))
        .rejects.toThrow(NotFoundException)
    })

    it('nên throw ForbiddenException khi user không có quyền', async () => {
      const currentUser = {
        id: '2', // Different user
        roles: [] // No permissions
      }

      mockRepository.findOne.mockResolvedValue(mockUser)

      await expect(service.findOne('1', currentUser))
        .rejects.toThrow(ForbiddenException)
    })
  })

  describe('create', () => {
    const createUserDto: CreateUserDto = {
      name: 'New User',
      email: 'newuser@example.com',
      password: 'password123'
    }

    it('nên tạo user thành công', async () => {
      mockRepository.findOne.mockResolvedValue(null) // Email chưa tồn tại
      mockRepository.create.mockReturnValue(mockUser)
      mockRepository.save.mockResolvedValue(mockUser)

      const result = await service.create(createUserDto)

      expect(result).toBeDefined()
      expect(result.email).toBe(mockUser.email)
      expect(mockRepository.save).toHaveBeenCalled()
    })

    it('nên throw ConflictException khi email đã tồn tại', async () => {
      mockRepository.findOne.mockResolvedValue(mockUser) // Email đã tồn tại

      await expect(service.create(createUserDto))
        .rejects.toThrow(ConflictException)
    })
  })

  describe('update', () => {
    const updateUserDto: UpdateUserDto = {
      name: 'Updated Name'
    }

    it('nên cập nhật user thành công', async () => {
      const currentUser = {
        id: '1',
        roles: [{ permissions: [{ name: Permission.USER_UPDATE }] }]
      }

      mockRepository.findOne.mockResolvedValue(mockUser)
      mockRepository.save.mockResolvedValue({ ...mockUser, ...updateUserDto })

      const result = await service.update('1', updateUserDto, currentUser)

      expect(result.name).toBe(updateUserDto.name)
      expect(mockRepository.save).toHaveBeenCalled()
    })

    it('nên throw ForbiddenException khi không có quyền update', async () => {
      const currentUser = {
        id: '2', // Different user
        roles: [] // No permissions
      }

      mockRepository.findOne.mockResolvedValue(mockUser)

      await expect(service.update('1', updateUserDto, currentUser))
        .rejects.toThrow(ForbiddenException)
    })
  })
})
```

### 2. **Controller Testing**

#### ✅ **Đúng:**
```typescript
// modules/users/users.controller.spec.ts
import { Test, TestingModule } from '@nestjs/testing'
import { UsersController } from './users.controller'
import { UsersService } from './users.service'
import { CreateUserDto, UserResponseDto } from './dto'
import { JwtAuthGuard, RolesGuard, PermissionsGuard } from '@/common/guards'

describe('UsersController', () => {
  let controller: UsersController
  let service: UsersService

  const mockUsersService = {
    findAll: jest.fn(),
    findOne: jest.fn(),
    create: jest.fn(),
    update: jest.fn(),
    remove: jest.fn()
  }

  const mockUserResponse: UserResponseDto = {
    id: '1',
    name: 'Test User',
    email: 'test@example.com',
    isActive: true,
    roles: ['user'],
    createdAt: new Date(),
    updatedAt: new Date()
  } as UserResponseDto

  beforeEach(async () => {
    const module: TestingModule = await Test.createTestingModule({
      controllers: [UsersController],
      providers: [
        {
          provide: UsersService,
          useValue: mockUsersService
        }
      ]
    })
    .overrideGuard(JwtAuthGuard)
    .useValue({ canActivate: () => true })
    .overrideGuard(RolesGuard)
    .useValue({ canActivate: () => true })
    .overrideGuard(PermissionsGuard)
    .useValue({ canActivate: () => true })
    .compile()

    controller = module.get<UsersController>(UsersController)
    service = module.get<UsersService>(UsersService)
  })

  afterEach(() => {
    jest.clearAllMocks()
  })

  describe('findAll', () => {
    it('nên trả về danh sách users', async () => {
      const query = { page: 1, limit: 10 }
      const expectedResult = {
        data: [mockUserResponse],
        meta: { page: 1, limit: 10, total: 1, totalPages: 1 }
      }

      mockUsersService.findAll.mockResolvedValue(expectedResult)

      const result = await controller.findAll(query)

      expect(result).toEqual(expectedResult)
      expect(service.findAll).toHaveBeenCalledWith(query)
    })
  })

  describe('create', () => {
    it('nên tạo user mới thành công', async () => {
      const createUserDto: CreateUserDto = {
        name: 'New User',
        email: 'newuser@example.com',
        password: 'password123'
      }

      mockUsersService.create.mockResolvedValue(mockUserResponse)

      const result = await controller.create(createUserDto)

      expect(result).toEqual(mockUserResponse)
      expect(service.create).toHaveBeenCalledWith(createUserDto)
    })
  })
})
```

## 📏 **Error Handling và Logging**

### 1. **Global Exception Filter**

#### ✅ **Đúng:**
```typescript
// common/filters/http-exception.filter.ts
import {
  ExceptionFilter,
  Catch,
  ArgumentsHost,
  HttpException,
  HttpStatus,
  Logger
} from '@nestjs/common'
import { Request, Response } from 'express'

@Catch(HttpException)
export class HttpExceptionFilter implements ExceptionFilter {
  private readonly logger = new Logger(HttpExceptionFilter.name)

  catch(exception: HttpException, host: ArgumentsHost) {
    const ctx = host.switchToHttp()
    const response = ctx.getResponse<Response>()
    const request = ctx.getRequest<Request>()
    const status = exception.getStatus()

    const errorResponse = {
      statusCode: status,
      timestamp: new Date().toISOString(),
      path: request.url,
      method: request.method,
      message: exception.message || 'Internal server error',
      error: exception.getResponse()
    }

    // Log error với context
    this.logger.error(
      `HTTP ${status} Error: ${exception.message}`,
      {
        ...errorResponse,
        stack: exception.stack,
        user: request.user?.id,
        ip: request.ip,
        userAgent: request.get('User-Agent')
      }
    )

    response.status(status).json(errorResponse)
  }
}

// common/filters/validation.filter.ts
import {
  ExceptionFilter,
  Catch,
  ArgumentsHost,
  BadRequestException
} from '@nestjs/common'
import { Response } from 'express'

@Catch(BadRequestException)
export class ValidationExceptionFilter implements ExceptionFilter {
  catch(exception: BadRequestException, host: ArgumentsHost) {
    const ctx = host.switchToHttp()
    const response = ctx.getResponse<Response>()
    const status = exception.getStatus()
    const exceptionResponse = exception.getResponse() as any

    const errorResponse = {
      statusCode: status,
      timestamp: new Date().toISOString(),
      message: 'Validation failed',
      errors: exceptionResponse.message || []
    }

    response.status(status).json(errorResponse)
  }
}
```

### 2. **Logging Best Practices**

#### ✅ **Đúng:**
```typescript
// common/interceptors/logging.interceptor.ts
import {
  Injectable,
  NestInterceptor,
  ExecutionContext,
  CallHandler,
  Logger
} from '@nestjs/common'
import { Observable } from 'rxjs'
import { tap } from 'rxjs/operators'

@Injectable()
export class LoggingInterceptor implements NestInterceptor {
  private readonly logger = new Logger(LoggingInterceptor.name)

  intercept(context: ExecutionContext, next: CallHandler): Observable<any> {
    const request = context.switchToHttp().getRequest()
    const { method, url, body, user } = request
    const startTime = Date.now()

    this.logger.log(
      `Incoming Request: ${method} ${url}`,
      {
        method,
        url,
        userId: user?.id,
        body: this.sanitizeBody(body),
        ip: request.ip,
        userAgent: request.get('User-Agent')
      }
    )

    return next.handle().pipe(
      tap({
        next: (data) => {
          const duration = Date.now() - startTime
          this.logger.log(
            `Request Completed: ${method} ${url} - ${duration}ms`,
            {
              method,
              url,
              duration,
              userId: user?.id,
              responseSize: JSON.stringify(data).length
            }
          )
        },
        error: (error) => {
          const duration = Date.now() - startTime
          this.logger.error(
            `Request Failed: ${method} ${url} - ${duration}ms`,
            {
              method,
              url,
              duration,
              userId: user?.id,
              error: error.message,
              stack: error.stack
            }
          )
        }
      })
    )
  }

  /**
   * Sanitize request body để loại bỏ sensitive data
   * @param body - Request body
   * @returns Sanitized body
   * @private
   */
  private sanitizeBody(body: any): any {
    if (!body) return body

    const sensitiveFields = ['password', 'token', 'secret', 'key']
    const sanitized = { ...body }

    sensitiveFields.forEach(field => {
      if (sanitized[field]) {
        sanitized[field] = '***REDACTED***'
      }
    })

    return sanitized
  }
}
```

## ✅ **Code Review Checklist**

### 1. **Controller Review**
- [ ] Sử dụng appropriate HTTP status codes
- [ ] Swagger documentation đầy đủ
- [ ] Guards và decorators được áp dụng đúng
- [ ] Validation pipes được sử dụng
- [ ] Error handling không có business logic
- [ ] Method names mô tả rõ action

### 2. **Service Review**
- [ ] Business logic được implement đúng
- [ ] Error handling với appropriate exceptions
- [ ] Logging với context đầy đủ
- [ ] Permission checking được tích hợp
- [ ] Database transactions khi cần thiết
- [ ] Helper methods được extract properly

### 3. **DTO Review**
- [ ] Validation decorators đầy đủ
- [ ] Swagger documentation
- [ ] Transform decorators khi cần
- [ ] Sensitive fields được exclude
- [ ] Proper inheritance (PartialType, OmitType)

### 4. **Database Review**
- [ ] Entity relationships được định nghĩa đúng
- [ ] Indexes được tạo cho performance
- [ ] Migrations được viết đúng
- [ ] Query optimization với QueryBuilder
- [ ] Proper error handling cho database operations

### 5. **Security Review**
- [ ] Authentication guards được áp dụng
- [ ] Authorization checks đầy đủ
- [ ] Input validation và sanitization
- [ ] Sensitive data không được log
- [ ] SQL injection prevention
- [ ] Rate limiting được implement

### 6. **Testing Review**
- [ ] Unit tests cho services
- [ ] Controller tests với mocked dependencies
- [ ] Integration tests cho critical flows
- [ ] Error scenarios được test
- [ ] RBAC scenarios được cover
- [ ] Test descriptions bằng tiếng Việt

---

**💡 Lưu ý**: Tuân thủ các quy tắc này sẽ đảm bảo backend NestJS có chất lượng cao, bảo mật tốt, và dễ maintain!
