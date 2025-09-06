# 在 Windows 11 环境下 gemini.AI 系统开发规则

> **重要说明**：本项目以简体中文为主要开发语言，所有文档、注释、提交信息等均使用简体中文编写。

## 1. 项目概述

本项目是基于 Laravel 10 采用以下技术栈：

- 后端 API Server：Laravel 10
- 后台管理系统：Filament v3
- 数据库：MySQL 8.0+
- 移动端：uniapp + unibest
- 队列服务：Redis
- 部署环境：Docker / Nginx / PHP-FPM

## 2. 开发工具规范

### 2.1 IDE 与开发环境

- **IDE**：使用 gemini.AI 作为主要开发工具
- **代码版本控制**：Git
- **本地开发环境**：Docker（包含 PHP-FPM、MySQL、Redis 等服务）

### 2.2 gemini.AI 配置与插件

- **PHP 插件**：启用 PHP Intelephense 或 PHP Language Server
- **Laravel 插件**：启用 Laravel Extension Pack
- **Vue/JavaScript 插件**：启用 Vetur/Volar（用于 uniapp 开发）
- **代码格式化**：使用 Prettier 和 PHP CS Fixer
- **代码质量检查**：使用 ESLint 和 PHP_CodeSniffer

### 2.3 开发环境性能优化（Windows 特定）

- **安全软件配置**：将项目目录添加到 Windows Defender 或其他杀毒软件的排除列表中
- **实时文件扫描**：禁用对开发目录的实时文件扫描以提升性能
- **文件系统优化**：使用 SSD 存储开发项目，避免机械硬盘的 I/O 瓶颈
- **内存配置**：确保 PHP 内存限制足够（建议 512M 以上用于开发环境）
- **Composer 优化**：使用 `composer install --optimize-autoloader` 优化自动加载

## 3. 代码规范

### 3.1 通用规范

- 使用 UTF-8 编码
- 使用 LF（\n）作为行结束符
- 文件末尾保留一个空行
- 删除行尾空格
- 最大行长度为 120 个字符
- 使用 4 个空格缩进（不使用 Tab）
- **所有代码注释必须使用简体中文**，确保团队成员能够清晰理解

### 3.2 PHP/Laravel 代码规范

- 遵循 PSR-12 编码规范
- 类名使用 PascalCase（例如：`ProductController`）
- 方法名使用 camelCase（例如：`getProductList`）
- 变量名使用 camelCase（例如：`$productPrice`）
- 常量名使用 UPPER_CASE（例如：`API_VERSION`）
- 注释使用 DocBlock 格式

#### 3.2.1 Laravel 特定规范

**优先使用 Laravel Artisan 指令创建代码：**

- **模型创建**：`php artisan make:model ModelName -mfsc`（同时创建迁移、工厂、种子、控制器）
- **控制器创建**：`php artisan make:controller ControllerName --resource`（创建资源控制器）
- **中间件创建**：`php artisan make:middleware MiddlewareName`
- **请求验证创建**：`php artisan make:request RequestName`
- **资源类创建**：`php artisan make:resource ResourceName`
- **策略创建**：`php artisan make:policy PolicyName --model=ModelName`
- **事件创建**：`php artisan make:event EventName`
- **监听器创建**：`php artisan make:listener ListenerName --event=EventName`
- **任务创建**：`php artisan make:job JobName`
- **通知创建**：`php artisan make:notification NotificationName`
- **邮件创建**：`php artisan make:mail MailName --markdown=emails.name`
- **命令创建**：`php artisan make:command CommandName`
- **服务提供者创建**：`php artisan make:provider ProviderName`

**代码组织规范：**

- 控制器方法遵循 RESTful 命名（index, show, store, update, destroy）
- 模型放在 `app/Models` 目录下
- 资源类放在 `app/Http/Resources` 目录下
- 请求验证类放在 `app/Http/Requests` 目录下
- 使用依赖注入而非直接实例化
- **重要：手动创建文件前，必须先检查是否有对应的 Artisan 指令**

#### 3.2.2 Filament 特定规范

##### 3.2.2.1 目录结构规范

**优先使用 Filament Artisan 指令创建代码：**

- **资源创建**：`php artisan make:filament-resource ResourceName --generate`（自动生成 CRUD）
- **页面创建**：`php artisan make:filament-page PageName`
- **自定义页面创建**：`php artisan make:filament-resource-page PageName ResourceName`
- **小部件创建**：`php artisan make:filament-widget WidgetName`
- **关系管理器创建**：`php artisan make:filament-relation-manager ResourceName relationshipName TitleAttribute`
- **自定义字段创建**：`php artisan make:filament-field FieldName`
- **自定义表格列创建**：`php artisan make:filament-column ColumnName`
- **自定义操作创建**：`php artisan make:filament-action ActionName`
- **面板创建**：`php artisan make:filament-panel PanelName`
- **主题创建**：`php artisan make:filament-theme ThemeName`
- **用户创建**：`php artisan make:filament-user`

**目录结构：**

- 资源类放在 `app/Filament/Resources` 目录下
- 页面类放在 `app/Filament/Pages` 目录下
- 小部件放在 `app/Filament/Widgets` 目录下
- 关系管理器放在 `app/Filament/Resources/{ResourceName}/RelationManagers` 目录下
- 自定义字段组件放在 `app/Filament/Components` 目录下
- 表单架构文件放在 `app/Filament/Resources/{ResourceName}/Schemas` 目录下（保持资源类整洁）

**重要提醒：**
- **在手动创建任何 Filament 相关文件前，必须先检查是否有对应的 Artisan 指令**
- **使用 `--generate` 参数可以自动生成基础的 CRUD 操作**
- **使用 `php artisan filament:list-commands` 查看所有可用的 Filament 指令**

##### 3.2.2.2 代码组织规范

- 使用 Filament 提供的表单和表格构建器
- 复杂的表单架构应提取到独立的 Schema 类中，避免资源类过于庞大
- 使用 `visibleOn()` 和 `hiddenOn()` 方法控制字段在不同页面的显示
- 利用 `dehydrated()` 方法控制字段数据的保存行为
- 自定义字段组件应继承相应的基础组件类

##### 3.2.2.3 性能优化规范

- 生产环境必须运行 `php artisan filament:optimize` 命令
- 大量组件时运行 `php artisan filament:cache-components` 缓存组件
- 配置 PHP OPcache 以提升性能
- 异步加载依赖第三方库的 Alpine.js 组件
- 避免在表格中进行 N+1 查询，使用 `with()` 预加载关联数据

##### 3.2.2.4 安全规范

- 用户模型必须实现 `FilamentUser` 接口
- 实现 `canAccessPanel()` 方法控制面板访问权限
- 使用 Laravel Policy 进行资源级别的授权控制
- 为敏感操作添加额外的中间件验证
- 避免在 JavaScript 中暴露敏感的模型属性（使用 `$hidden` 属性）

##### 3.2.2.5 本地化规范

- **后台管理界面所有文本必须使用简体中文**
- 使用 Filament 的本地化功能配置简体中文界面
- 自定义组件的文本也应支持本地化
- 错误消息和验证提示使用简体中文

##### 3.2.2.6 自定义组件开发规范

- 自定义字段组件应继承 Filament 提供的基础组件类
- 使用 Filament 的字段包装器组件渲染标签和验证错误
- 重度依赖第三方库的组件应异步加载 Alpine.js 组件
- 组件状态通过 Livewire 的公共属性进行管理
- 自定义组件应提供完整的文档和使用示例

示例自定义字段组件结构：
```php
<?php

namespace App\Filament\Components;

use Filament\Forms\Components\Field;

class CustomField extends Field
{
    protected string $view = 'filament.components.custom-field';
    
    public function setUp(): void
    {
        parent::setUp();
        
        $this->afterStateHydrated(function (CustomField $component, $state) {
            // 状态初始化逻辑
        });
    }
}
```

##### 3.2.2.7 嵌套资源开发规范

- 使用 Trait 和自定义页面实现嵌套资源功能
- 确保父记录数据在子记录页面导航时始终可用
- 正确处理面包屑导航和页面重定向
- 支持多租户场景下的嵌套资源访问控制

### 3.3 JavaScript/Vue/uniapp 代码规范

- 使用 ES6+ 语法
- 变量名使用 camelCase（例如：`productList`）
- 组件名使用 PascalCase（例如：`ProductCard.vue`）
- 使用单引号（'）而非双引号（"）
- 每个语句末尾使用分号（;）

#### 3.3.1 uniapp 特定规范

- 页面放在 `pages` 目录下
- 组件放在 `components` 目录下
- 使用 `unibest` 提供的组件和工具
- API 请求封装在 `utils/request.js` 中
- 状态管理使用 Pinia
- **所有用户界面文本必须使用简体中文**
- 界面文本应存放在语言包中，便于后期国际化

## 4. 数据库规范

### 4.0 数据库设计文档

- 所有数据库表结构必须在设计文档中详细说明
- 设计文档必须包含表名、字段名、字段类型、字段长度、是否允许为空、默认值和中文注释
- 设计文档应包含表与表之间的关系图
- 设计文档必须使用简体中文编写

示例表结构文档格式：

**表名：products（商品表）**

| 字段名 | 类型 | 长度 | 允许为空 | 默认值 | 注释 |
| ------ | ---- | ---- | -------- | ------ | ---- |
| id | bigint | 20 | 否 | 无 | 商品ID |
| name | varchar | 255 | 否 | 无 | 商品名称 |
| description | text | - | 是 | null | 商品描述 |
| price | decimal | 10,2 | 否 | 0.00 | 商品价格 |
| category_id | bigint | 20 | 否 | 无 | 分类ID |
| is_active | tinyint | 1 | 否 | 1 | 是否激活 |
| created_at | timestamp | - | 是 | null | 创建时间 |
| updated_at | timestamp | - | 是 | null | 更新时间 |
| deleted_at | timestamp | - | 是 | null | 删除时间 |

### 4.1 命名规范

- 表名使用复数形式，小写，单词间用下划线连接（例如：`product_categories`）
- 主键统一命名为 `id`
- 外键命名格式为 `{单数表名}_id`（例如：`product_id`）
- 时间戳字段使用 `created_at` 和 `updated_at`
- 软删除字段使用 `deleted_at`

### 4.2 字段类型规范

- 使用 `bigIncrements` 作为主键类型
- 使用 `string` 存储短文本
- 使用 `text` 存储长文本
- 使用 `decimal` 存储金额（例如：`decimal('price', 10, 2)`）
- 使用 `boolean` 存储布尔值
- 使用 `json` 存储 JSON 数据

### 4.3 字段注释规范

- **所有数据库表字段必须添加中文注释**，清晰描述字段用途
- 注释应简洁明了，表达字段的实际业务含义
- 使用 Laravel 迁移文件的 `comment` 方法添加注释

示例：

```php
// 在迁移文件中
Schema::create('products', function (Blueprint $table) {
    $table->id()->comment('产品ID');
    $table->string('name')->comment('产品名称');
    $table->text('description')->comment('产品描述');
    $table->decimal('price', 10, 2)->comment('产品价格');
    $table->unsignedBigInteger('category_id')->comment('分类ID');
    $table->boolean('is_active')->default(true)->comment('是否激活');
    $table->timestamps();
    $table->softDeletes()->comment('软删除时间');
});
```

### 4.3 数据库开发与查询规范

#### 4.3.1 数据库开发规范

**优先使用 Laravel Artisan 指令进行数据库操作：**

- **迁移文件创建**：`php artisan make:migration create_table_name_table`
- **模型迁移创建**：`php artisan make:model ModelName -m`（同时创建模型和迁移）
- **种子文件创建**：`php artisan make:seeder TableNameSeeder`
- **工厂文件创建**：`php artisan make:factory ModelNameFactory --model=ModelName`
- **数据库状态查看**：`php artisan migrate:status`
- **迁移执行**：`php artisan migrate`
- **迁移回滚**：`php artisan migrate:rollback --step=1`
- **数据库重置**：`php artisan migrate:fresh --seed`
- **种子执行**：`php artisan db:seed --class=SeederName`

#### 4.3.2 数据库查询规范

- 在 Windows 11 环境下，开发新功能或遇到不确定的数据库逻辑问题时，必须使用 `php artisan tinker --execute="语句"` 命令查询数据库表结构和表数据
- **重要：`php artisan tinker --execute` 仅用于查询数据，不应用于添加数据或修改表结构**
- 使用 tinker 查询表结构的常用命令：
  ```php
  // 查看表结构
  php artisan tinker --execute="Schema::getColumnListing('表名');"
  
  // 查看字段详细信息
  php artisan tinker --execute="DB::select('SHOW FULL COLUMNS FROM 表名');"
  
  // 查看表索引
  php artisan tinker --execute="DB::select('SHOW INDEX FROM 表名');"
  ```
- 使用 tinker 查询表数据的常用命令：
  ```php
  // 查询所有记录
  php artisan tinker --execute="DB::table('表名')->get();"
  
  // 使用模型查询
  php artisan tinker --execute="App\Models\模型名::all();"
  php artisan tinker --execute="App\Models\模型名::find(1);"
  php artisan tinker --execute="App\Models\模型名::where('字段', '值')->get();"
  
  // 查询关联数据
  php artisan tinker --execute="App\Models\模型名::with('关联关系')->find(1);"
  ```
- 在编写复杂查询前，应先使用上述命令验证查询逻辑的正确性
- 遇到数据库相关问题时，应优先使用上述命令进行排查和调试

#### 4.3.3 数据添加与表结构修改规范

- **数据添加和表结构修改必须通过 Seeder 或 Migration 进行，不得使用 tinker 命令**
- **优先使用上述 Artisan 指令创建相关文件**
- 运行所有种子文件：`php artisan db:seed`
- 运行特定种子文件：`php artisan db:seed --class=名称Seeder`
- 运行所有迁移：`php artisan migrate`
- 回滚最后一次迁移：`php artisan migrate:rollback`
- 重置并重新运行所有迁移：`php artisan migrate:fresh`
- 重置并重新运行所有迁移和种子：`php artisan migrate:fresh --seed`

## 5. API 规范

### 5.1 RESTful API 设计

- 使用名词复数形式作为资源标识（例如：`/api/products`）
- 使用 HTTP 方法表示操作（GET, POST, PUT, DELETE）
- 使用嵌套资源表示关系（例如：`/api/products/{id}/reviews`）
- 使用查询参数进行过滤、排序和分页（例如：`/api/products?category=1&sort=price&page=2`）

### 5.2 API 响应格式

```json
{
  "status": "success",  // 或 "error"
  "code": 200,          // HTTP 状态码
  "message": "操作成功",  // 响应消息
  "data": {             // 响应数据
    // 具体数据
  },
  "meta": {             // 元数据，如分页信息
    "current_page": 1,
    "per_page": 15,
    "total": 50
  }
}
```

### 5.3 API 错误处理

- 使用适当的 HTTP 状态码
- 提供详细的错误消息
- 对于验证错误，返回具体的字段错误信息
- **所有响应消息必须使用简体中文**，包括成功提示和错误信息

## 6. 版本控制与分支管理

### 6.1 Git 工作流

采用 Git Flow 工作流：

- `main`：生产环境分支，稳定版本
- `develop`：开发环境分支，最新开发版本
- `feature/*`：功能分支，用于开发新功能
- `bugfix/*`：修复分支，用于修复非生产环境的 bug
- `hotfix/*`：热修复分支，用于修复生产环境的 bug
- `release/*`：发布分支，用于准备新版本发布

### 6.2 提交规范

提交信息格式：

```
<type>(<scope>): <subject>

<body>

<footer>
```

- **type**：提交类型
  - `feat`：新功能
  - `fix`：修复 bug
  - `docs`：文档更新
  - `style`：代码格式（不影响代码运行的变动）
  - `refactor`：重构（既不是新增功能，也不是修改 bug 的代码变动）
  - `perf`：性能优化
  - `test`：增加测试
  - `chore`：构建过程或辅助工具的变动
- **scope**：影响范围（可选）
- **subject**：简短描述（**必须使用简体中文**）
- **body**：详细描述（可选，**必须使用简体中文**）
- **footer**：备注，如关闭 issue（可选）

示例：

```
feat(product): 添加商品搜索功能

实现了按名称、分类和价格区间搜索商品的功能

Closes #123
```

## 7. 测试规范

### 7.1 后端测试

- 使用 PHPUnit 进行单元测试和功能测试
- 控制器测试：测试 API 响应和状态码
- 模型测试：测试关系和作用域
- 中间件测试：测试权限和认证
- 命令测试：测试命令行工具

#### 7.1.1 Filament 测试规范

- 使用 Filament 提供的测试辅助方法进行面板测试
- 测试多面板时需在 `setUp()` 方法中指定测试面板
- 使用 Filament 的表格测试辅助方法测试数据表格功能
- 测试表单验证和数据保存逻辑
- 测试权限控制和访问限制
- 测试自定义组件的渲染和交互

示例测试代码：
```php
// 测试资源访问权限
public function test_user_can_access_product_resource()
{
    $user = User::factory()->create();
    
    $this->actingAs($user)
        ->get(ProductResource::getUrl('index'))
        ->assertSuccessful();
}

// 测试表单提交
public function test_can_create_product()
{
    $user = User::factory()->create();
    
    $this->actingAs($user)
        ->livewire(ProductResource\Pages\CreateProduct::class)
        ->fillForm([
            'name' => '测试产品',
            'price' => 99.99,
        ])
        ->call('create')
        ->assertHasNoFormErrors();
}
```

### 7.2 前端测试

- 使用 Jest 进行单元测试
- 使用 Cypress 进行端到端测试
- 组件测试：测试组件渲染和交互
- 状态测试：测试 Pinia store
- API 测试：测试 API 请求和响应处理

## 8. 文档规范

### 8.1 代码文档

- 使用 DocBlock 注释关键类和方法
- 复杂逻辑需要添加行内注释
- API 端点使用 Swagger 或 Scribe 生成文档

### 8.2 项目文档

- **所有项目文档必须使用简体中文编写**
- `README.md`：项目概述、安装步骤、使用说明
- `CONTRIBUTING.md`：贡献指南
- `CHANGELOG.md`：版本变更记录
- `/docs`：详细文档目录
  - 架构设计文档
  - API 文档
  - 数据库设计文档
  - 部署文档
  - **开发过程.md**：记录每次开发的详细过程

### 8.3 开发过程记录规范

- **每次开发完成后，必须将开发过程记录到 `/docs/开发过程.md` 文档中**
- 记录内容应包括：
  - 开发日期
  - 开发人员
  - 开发内容概述
  - 实现的功能点
  - 遇到的问题及解决方案
  - 待解决的问题（如有）
  - 相关的代码提交ID
- 记录格式应清晰、条理分明，便于其他团队成员理解
- 重要的技术决策和架构变更必须详细说明原因和影响

## 9. 安全规范

### 9.1 通用安全规范

- 不在代码中硬编码敏感信息（密码、API 密钥等）
- 使用环境变量存储敏感配置
- 定期更新依赖包以修复安全漏洞
- 实施 HTTPS

### 9.2 后端安全规范

- 使用 Laravel 提供的安全特性（CSRF 保护、XSS 防护等）
- 实施请求验证和数据清理
- 使用参数化查询防止 SQL 注入
- 实施适当的访问控制和权限检查

#### 9.2.1 Filament 安全最佳实践

- **权限管理插件**：推荐使用 `filament-shield` 插件进行细粒度权限控制
- **多租户支持**：使用 `SyncShieldTenant` 中间件确保租户数据隔离
- **Policy 授权**：为每个资源创建对应的 Policy 类进行授权控制
- **自定义认证模型**：根据需要使用独立的 Admin 模型而非默认 User 模型
- **中间件配置**：为敏感操作添加额外的认证和授权中间件

示例权限配置：
```php
// 在 Panel Provider 中配置
->plugins([
    \BezhanSalleh\FilamentShield\FilamentShieldPlugin::make(),
])
->middleware([
    \BezhanSalleh\FilamentShield\Middleware\SyncShieldTenant::class,
], isPersistent: true)

// Policy 示例
class ProductPolicy
{
    public function viewAny(User $user): bool
    {
        return $user->can('view_any_product');
    }
    
    public function create(User $user): bool
    {
        return $user->can('create_product');
    }
}
```

### 9.3 前端安全规范

- 防止 XSS 攻击：对用户输入进行转义
- 实施 CSP（内容安全策略）
- 不在前端存储敏感信息
- 使用 HTTPS 进行 API 通信

## 10. 部署规范

### 10.1 环境配置

- 开发环境（Development）
- 测试环境（Testing）
- 预生产环境（Staging）
- 生产环境（Production）

### 10.2 部署流程

1. 代码审查和测试
2. 合并到适当的分支
3. 构建和打包
4. 部署到目标环境
5. 运行数据库迁移
6. 清除缓存
7. **Filament 特定部署步骤**：
   - 运行 `php artisan filament:optimize` 优化 Filament 性能
   - 运行 `php artisan filament:cache-components` 缓存组件（如有大量组件）
   - 配置 PHP OPcache 优化设置
   - 验证 Filament 面板访问权限配置
8. 验证部署

### 10.3 监控与日志

- 使用 Laravel Telescope 进行开发环境调试
- 使用 Laravel Horizon 监控队列
- 配置适当的日志级别和轮转策略
- 使用监控工具监控服务器和应用性能

#### 10.3.1 Filament 性能监控

- **开发环境调试**：集成 Laravel Telescope 监控 Filament 页面性能
- **数据库查询优化**：监控并优化 Filament 资源的数据库查询性能
- **组件渲染监控**：跟踪复杂表单和表格的渲染时间
- **内存使用监控**：监控大数据量操作时的内存使用情况
- **缓存命中率**：监控 Filament 组件缓存的命中率和效果

推荐监控指标：
- Filament 页面加载时间
- 数据库查询数量和执行时间
- 内存峰值使用量
- 用户操作响应时间
- 错误率和异常频率

## 11. 持续集成与持续部署 (CI/CD)

### 11.1 CI 流程

1. 代码提交触发构建
2. 运行代码质量检查（Lint）
3. 运行自动化测试
4. 生成测试覆盖率报告
5. 构建应用

### 11.2 CD 流程

1. 手动或自动触发部署
2. 部署到目标环境
3. 运行数据库迁移
4. 清除缓存
5. 运行冒烟测试
6. 通知部署结果

## 12. 性能优化规范

### 12.1 后端性能优化

- 使用缓存减少数据库查询
- 优化数据库查询（索引、N+1 问题等）
- 使用队列处理耗时任务
- 使用 CDN 提供静态资源

### 12.2 前端性能优化

- 代码分割和懒加载
- 图片优化（压缩、响应式图片）
- 减少 HTTP 请求
- 使用缓存策略

## 13. 协作与沟通规范

### 13.1 任务管理

- 使用项目管理工具（如 Jira、Trello）跟踪任务
- 任务描述应清晰、具体，包含验收标准
- 使用标签和优先级分类任务

### 13.2 代码审查

- 所有代码必须经过审查后才能合并
- 审查重点：功能正确性、代码质量、安全性、性能
- 提供建设性的反馈
- 及时响应审查意见

### 13.3 会议规范

- 每日站会：同步进度、讨论问题
- 迭代计划会：规划下一迭代的任务
- 迭代回顾会：总结经验教训，持续改进
- 技术讨论会：讨论技术方案和架构决策

## 14. 项目时间线

根据开发执行计划，项目分为以下阶段：

1. **后端 API 开发完成**：第 4 周末
2. **Filament 后台管理系统开发完成**：第 7 周末
3. **手机端开发完成**：第 11 周末
4. **系统联调与部署完成**：第 13 周末

## 15. 开发效率优化指南

### 15.1 Laravel & Filament 指令优先原则

**核心原则：优先使用官方 Artisan 指令创建代码**

1. **开发前检查**：在手动创建任何文件前，先执行 `php artisan list` 查看可用指令
2. **Filament 指令查看**：使用 `php artisan filament:list-commands` 查看所有 Filament 相关指令
3. **指令帮助**：使用 `php artisan help command-name` 查看指令详细用法
4. **自动生成优势**：
   - 确保代码结构标准化
   - 自动生成必要的样板代码
   - 减少人为错误
   - 提高开发效率
   - 保持项目一致性

### 15.2 常用指令速查表

**Laravel 核心指令：**
```bash
# 模型相关
php artisan make:model ModelName -mfsc  # 创建模型+迁移+工厂+种子+控制器
php artisan make:controller ControllerName --resource
php artisan make:request RequestName
php artisan make:policy PolicyName --model=ModelName

# 数据库相关
php artisan make:migration create_table_name
php artisan make:seeder TableNameSeeder
php artisan migrate:status
php artisan migrate:fresh --seed
```

**Filament 核心指令：**
```bash
# 资源管理
php artisan make:filament-resource ResourceName --generate
php artisan make:filament-relation-manager ResourceName relationName TitleAttribute

# 界面组件
php artisan make:filament-page PageName
php artisan make:filament-widget WidgetName
php artisan make:filament-field FieldName

# 系统管理
php artisan make:filament-user
php artisan filament:optimize
```

### 15.3 开发工作流建议

1. **规划阶段**：确定需要创建的文件类型
2. **指令查询**：查找对应的 Artisan 指令
3. **自动生成**：使用指令创建基础代码
4. **定制开发**：在生成的基础上进行定制
5. **测试验证**：确保功能正常

## 16. 总结

本文档定义了使用AI 工具进行电商系统开发的规则和标准，特别强调了**优先使用 Laravel 和 Filament 官方 Artisan 指令**的重要性。遵循这些规则将有助于：

- 保持代码质量和一致性
- 提高开发效率
- 减少人为错误和 bug
- 确保项目按时高质量地完成
- 利用框架的最佳实践

所有团队成员应熟悉并遵循这些规则，特别是要熟练掌握相关的 Artisan 指令。如有特殊情况需要偏离规则，应事先讨论并获得团队共识。