# 功能规格：任务看板

## 1. 数据模型

### 1.1 Task（任务）
```typescript
interface Task {
  id: string;                    // 唯一标识
  title: string;                 // 任务标题
  description?: string;          // 任务描述
  status: TaskStatus;            // 状态
  priority: Priority;            // 优先级
  assignee?: User;               // 指派人
  tags: string[];                // 标签
  dueDate?: string;              // 截止日期
  createdAt: string;             // 创建时间
  updatedAt: string;             // 更新时间
}
```

### 1.2 TaskStatus（任务状态）
```typescript
type TaskStatus =
  | 'backlog'      // 待处理
  | 'todo'         // 待办
  | 'in-progress'  // 进行中
  | 'done';        // 已完成
```

### 1.3 Priority（优先级）
```typescript
type Priority =
  | 'critical'     // 紧急
  | 'high'         // 高
  | 'medium'       // 中
  | 'low';         // 低
```

### 1.4 User（用户）
```typescript
interface User {
  id: string;
  name: string;
  avatar?: string;
}
```

## 2. API 接口

### 2.1 获取任务列表
**优先级：P0**

```
GET /api/tasks
Query Parameters:
  - status?: TaskStatus    // 按状态筛选
  - assignee?: string      // 按指派人筛选
  - search?: string        // 搜索关键词

Response:
{
  tasks: Task[]
}
```

### 2.2 创建任务
**优先级：P0**

```
POST /api/tasks
Body:
{
  title: string;
  description?: string;
  priority: Priority;
  assigneeId?: string;
  tags?: string[];
  dueDate?: string;
}

Response:
{
  task: Task
}
```

### 2.3 更新任务
**优先级：P0**

```
PATCH /api/tasks/:id
Body:
{
  title?: string;
  description?: string;
  status?: TaskStatus;
  priority?: Priority;
  assigneeId?: string;
  tags?: string[];
  dueDate?: string;
}

Response:
{
  task: Task
}
```

### 2.4 移动任务
**优先级：P0**

```
POST /api/tasks/:id/move
Body:
{
  status: TaskStatus;
  position?: number;  // 新位置索引
}

Response:
{
  task: Task
}
```

### 2.5 删除任务
**优先级：P1**

```
DELETE /api/tasks/:id

Response:
{
  success: true
}
```

## 3. 功能需求

### 3.1 基础功能（P0）
- [x] 展示任务看板（4 列：待处理、待办、进行中、已完成）
- [x] 在看板中查看任务卡片
- [x] 通过拖拽移动任务到不同列
- [x] 在列内拖拽调整任务顺序
- [x] 创建新任务
- [x] 查看任务详情
- [x] 编辑任务信息

### 3.2 增强功能（P1）
- [ ] 按指派人筛选任务
- [ ] 按优先级筛选任务
- [ ] 按标签筛选任务
- [ ] 搜索任务
- [ ] 添加自定义列
- [ ] 删除任务
- [ ] 批量操作

### 3.3 体验优化（P1）
- [ ] 空状态提示
- [ ] Loading 状态
- [ ] 拖拽动画
- [ ] 过渡动画
- [ ] 响应式适配（移动端列表视图）
- [ ] 键盘快捷键

## 4. 业务规则

1. **状态流转限制**：
   - 任务可以从任意状态移动到任意状态
   - 移动到 "已完成" 状态时，自动更新 `updatedAt` 时间

2. **优先级排序**：
   - 同一列内，任务按优先级排序（critical > high > medium > low）
   - 相同优先级按创建时间排序

3. **标签限制**：
   - 每个任务最多 5 个标签
   - 标签长度不超过 20 字符

4. **删除限制**：
   - 已完成的任务才能删除（可选规则）
   - 删除操作需要二次确认
