---
name: laravel-standards
description: "Use when working on any Laravel project — enforces layered architecture, TDD, API design, error handling, caching, and coding discipline. Activates on keywords: controller, service, model, request, route, api, endpoint, migration, eloquent, laravel."
user-invocable: true
---

# Laravel 工程规范

提炼自生产级 Laravel 项目，聚焦可复用工程纪律。

---

## 1. 分层职责

| 层 | 路径 | 允许 | 禁止 |
|---|---|---|---|
| Controller | `Http/Controllers/` | 调用 Service，返回 Resource | 业务逻辑、直接查 DB |
| FormRequest | `Http/Requests/` | 验证输入 | Controller 里写 `$request->validate()` |
| Service | `Services/` | 业务逻辑、事务、事件 | 返回 HTTP 响应 |
| Resource | `Http/Resources/` | 数据转换 | 业务计算 |
| Model | `Models/` | 关系、Scope、Accessor | 复杂业务逻辑 |

标准写法：

```php
// Controller — 薄
final class UserController extends Controller
{
    public function __construct(private readonly UserService $userService) {}

    public function store(StoreUserRequest $request): JsonResponse
    {
        $user = $this->userService->create($request->validated());
        return ApiResponse::success(new UserResource($user), '创建成功', 201);
    }
}

// Service — 厚
final class UserService extends Service
{
    public function create(array $data): User
    {
        return DB::transaction(function () use ($data) {
            $user = User::create($data);
            event(new UserCreated($user));
            return $user;
        });
    }
}
```

---

## 2. API 设计

### RESTful 路由

```
GET    /api/v{N}/{资源}          列表
POST   /api/v{N}/{资源}          创建
GET    /api/v{N}/{资源}/{id}     详情
PUT    /api/v{N}/{资源}/{id}     全量更新
PATCH  /api/v{N}/{资源}/{id}     部分更新
DELETE /api/v{N}/{资源}/{id}     删除
```

### 统一响应

所有响应通过 `ApiResponse` 封装：

```php
ApiResponse::success($data, $message);                    // code: 0
ApiResponse::error($message, $code);
ApiResponse::notFound('资源不存在');
ApiResponse::unauthorized('未授权');
ApiResponse::pagination($items, $total, $page, $size);
ApiResponse::badRequest('参数错误');
ApiResponse::validationError($message, $errors);
```

### HTTP 状态码

`200` 查询/更新 · `201` 创建 · `204` 删除 · `400` 参数错误 · `401` 未认证 · `403` 无权限 · `404` 不存在 · `422` 验证失败 · `429` 限流 · `500` 内部错误

---

## 3. 错误处理

异常分级处理，生产返回友好消息：

| 异常类型 | HTTP | 业务码 | 日志级别 |
|---|---|---|---|
| `ValidationException` | 422 | ERR_PARAMS_NOT_VALID | info |
| `AuthenticationException` | 401 | UNAUTHORIZED | info |
| `ModelNotFoundException` | 404 | RESOURCE_NOT_FOUND | warning |
| `ThrottleRequestsException` | 429 | TOO_MANY_REQUESTS | warning |
| `AuthorizationException` | 403 | FORBIDDEN | info |
| `ServiceException` | 500 | 自定义 | error |
| `QueryException` | 500 | DATABASE_ERROR | error |

---

## 4. 编码纪律

### YAGNI / DRY

- **YAGNI** — 只实现当前需求，不为假设未来设计。三行相似代码优于过早抽象。
- **DRY** — 同一逻辑出现 3 次以上才提取。偶然相同 → 保持独立；本质相同 → 提取。

### 代码标准

- `declare(strict_types=1)` 所有 PHP 文件
- Controller/Service 声明为 `final`
- 所有方法必须有类型声明（参数 + 返回值），禁止裸 `mixed`
- 类 PascalCase · 方法/变量 camelCase · 表 snake_case · 路由 kebab-case

### 禁止事项

- ❌ `DB::select()` 原始查询（复杂报表除外）
- ❌ `dd()` / `dump()` 提交到代码库
- ❌ 硬编码配置（密钥、URL、域名走 `.env`）
- ❌ N+1 查询（必须 `with()` 预加载）
- ❌ 无测试的业务逻辑变更
- ❌ 迁移提交后修改（需修改则新建迁移）

---

## 5. 缓存规范

**非必要不缓存**。缓存带来复杂度（失效、一致性），只在以下场景使用：
- 首页聚合数据（单次请求聚合多数据源）
- 全量低频数据（地理区域等）
- AI 嵌入向量（外部 API 有成本）

```php
// 通过 CacheManager 调用，禁止直接 Cache::remember()
$cacheManager->remember($key, 'read', fn() => $this->fetchData());
```

- ❌ 禁止 per-user 缓存键
- ❌ 禁止 Controller 中直接使用 `Cache::remember()`
- ❌ 禁止硬编码 TTL
- ❌ 禁止缓存闭包中捕获请求级变量

---

## 6. 性能底线

- 列表接口默认分页，`per_page` 最大 100
- 耗时操作放入队列
- 复杂查询加 `EXPLAIN` 分析

---

## 7. 任务生命周期

非平凡任务按 5 阶段推进：

| 阶段 | 内容 |
|---|---|
| Spec | 复述意图，确认范围，明确验收标准 |
| Design | 读代码，列方案（≥2 种），选最简，识风险 |
| Plan | 拆解为有序步骤，每步标注文件/改动/验证 |
| Execute | 严格 TDD，逐步实现，不顺手优化 |
| Verify | 全量测试 + 格式化 + 无调试代码 + 逐项确认验收 |

每步自检：改动在计划内？无新依赖？无禁止事项？需迁移？无安全风险？测试通过？

---

## 适用判断

本 skill 适用于 Laravel 项目。非 Laravel 项目仅 API 设计（§2）和编码纪律（§4 通用部分）可参考。
