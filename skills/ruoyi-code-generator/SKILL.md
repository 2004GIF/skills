---
name: ruoyi-code-generator
description: |
  基于若依(RuoYi)框架规范生成 CRUD 代码。当用户需要为数据表生成后端Java代码（实体类、Mapper、Service、Controller）、
  前端Vue代码（列表页、API封装）、MyBatis XML映射文件或菜单初始化SQL时使用此技能。
  适用于: 创建新模块、添加业务功能、根据表结构生成代码等场景。
---

# RuoYi 代码生成器技能

## 目标 (Goal)

根据用户提供的数据表信息（表名、字段定义），按照若依框架规范自动生成完整的 CRUD 代码，包括：
- Java 后端代码（Domain、Mapper、Service、ServiceImpl、Controller）
- MyBatis XML 映射文件（MyBatis-Plus 模式下仅子表/自定义 SQL 需要）
- Vue 前端代码（页面组件、API 封装）
- 菜单初始化 SQL

---

## 模板组合选择

生成代码前，根据用户需求选择以下三种组合之一：

| 选项 | 前端框架 | 后端 ORM | 模板目录 |
|------|---------|----------|---------|
| **选项一** | Vue 2 + Element UI | MyBatis | Java: `templates/java/` Vue: `templates/vue/` |
| **选项二** | Vue 3 + Element Plus | MyBatis | Java: `templates/java/` Vue: `templates/vue/v3/` 或 `templates/vue/v3ts/` |
| **选项三** | Vue 3 + Element Plus | MyBatis-Plus | Java: `templates/java/mybatis-plus/` Vue: `templates/vue/v3/` 或 `templates/vue/v3ts/` |

### 选项说明

**选项一**：传统若依技术栈，使用 Vue 2 + Element UI 前端，后端 MyBatis 手动编写 Mapper XML。

**选项二**：升级前端到 Vue 3 + Element Plus，后端保持 MyBatis。Vue 模板有两种：
- `v3/` — JavaScript 版
- `v3ts/` — TypeScript 版

**选项三**：前后端全面升级。后端使用 MyBatis-Plus（BaseMapper / IService / ServiceImpl），Mapper 无需手动声明 CRUD 方法；前端 Vue 3 + Element Plus。

### 模板文件对照表

```
模板目录结构:
templates/
├── java/                              # 选项一、二 的 Java 模板
│   ├── controller.java.vm
│   ├── domain.java.vm
│   ├── mapper.java.vm
│   ├── service.java.vm
│   ├── serviceImpl.java.vm
│   └── mybatis-plus/                  # 选项三 的 Java 模板
│       ├── controller.java.vm
│       ├── domain.java.vm
│       ├── mapper.java.vm
│       ├── service.java.vm
│       └── serviceImpl.java.vm
├── xml/
│   └── mapper.xml.vm                  # MyBatis XML（选项一、二必需；选项三仅子表需要）
├── js/
│   └── api.js.vm                      # 前端 API 封装
├── vue/                               # 选项一 的 Vue 2 模板
│   ├── index.vue.vm
│   ├── index-tree.vue.vm
│   ├── view.vue.vm
│   ├── v3/                            # 选项二、三 的 Vue 3 JS 模板
│   │   ├── index.vue.vm
│   │   ├── index-tree.vue.vm
│   │   └── view.vue.vm
│   └── v3ts/                          # 选项二、三 的 Vue 3 TS 模板
│       ├── index.vue.vm
│       ├── index-tree.vue.vm
│       └── view.vue.vm
└── sql/
    └── sql.vm                         # 菜单 SQL
```

---

## 输入定义 (Input)

用户需要提供以下信息（可以通过对话澄清获取）：

| 参数 | 必填 | 说明 | 示例 |
|------|------|------|------|
| `tableName` | ✅ | 数据库表名 | `sys_product` |
| `tableComment` | ✅ | 表注释/功能名称 | `产品管理` |
| `columns` | ✅ | 字段列表（含类型、注释） | 见下方示例 |
| `packageName` | ❌ | 包路径，默认 `com.ruoyi.system` | `com.ruoyi.business` |
| `moduleName` | ❌ | 模块名，默认取表前缀后的名称 | `product` |
| `businessName` | ❌ | 业务名称，默认取表名去前缀 | `product` |
| `author` | ❌ | 作者名，默认 `ruoyi` | `zhangsan` |
| `tplCategory` | ❌ | 模板类型: `crud`/`tree`/`sub`，默认 `crud` | `crud` |
| `templateOption` | ❌ | 模板组合: `1`/`2`/`3`，默认 `1` | `3` |
| `vueStyle` | ❌ | Vue 3 模板风格: `js`/`ts`，默认 `js`（仅选项二、三） | `ts` |

### 字段定义示例

```json
{
  "tableName": "sys_product",
  "tableComment": "产品管理",
  "columns": [
    {"name": "product_id", "type": "bigint", "comment": "产品ID", "isPk": true, "isIncrement": true},
    {"name": "product_name", "type": "varchar(100)", "comment": "产品名称", "isRequired": true, "isQuery": true},
    {"name": "product_code", "type": "varchar(50)", "comment": "产品编码", "isRequired": true},
    {"name": "category_id", "type": "bigint", "comment": "分类ID", "dictType": "product_category"},
    {"name": "price", "type": "decimal(10,2)", "comment": "价格"},
    {"name": "status", "type": "char(1)", "comment": "状态（0正常 1停用）", "dictType": "sys_normal_disable"},
    {"name": "create_time", "type": "datetime", "comment": "创建时间"}
  ]
}
```

---

## 输出定义 (Output)

生成以下文件结构的代码：

```
输出文件清单:
├── java/
│   ├── domain/{ClassName}.java          # 实体类
│   ├── mapper/{ClassName}Mapper.java    # Mapper接口
│   ├── service/I{ClassName}Service.java # Service接口
│   ├── service/impl/{ClassName}ServiceImpl.java  # Service实现
│   └── controller/{ClassName}Controller.java     # REST控制器
├── xml/                                 # 选项三（MP）无子表时可省略
│   └── {ClassName}Mapper.xml            # MyBatis映射文件
├── vue/
│   ├── api/{businessName}.js            # API封装
│   └── views/{moduleName}/{businessName}/index.vue  # 页面组件
└── sql/
    └── {businessName}Menu.sql           # 菜单初始化SQL
```

---

## 执行流程 (Workflow)

###  第一步：确定生成代码的存放位置 

1. 询问用户需要将代码生成到那个模块下
2. 检查模块模块是否存在，如果模块不存在要先创建模块 

### 第二步：确认模板组合

1. **询问用户选择模板组合** 【如果用户未明确指定，一定要问清楚模版组合】：
   - 选项一：Vue 2 + Element UI + MyBatis
   - 选项二：Vue 3 + Element Plus + MyBatis
   - 选项三：Vue 3 + Element Plus + MyBatis-Plus

### 第三步：信息收集与验证

1. **解析用户请求**：识别表名、字段信息
2. **缺省信息追问**：如果缺少必要信息，主动询问用户
3. **推断默认值**：
   - `className` = 表名转大驼峰（去除表前缀如 `sys_`）
   - `moduleName` = 表前缀后的模块名
   - `businessName` = 表名去前缀后的小写形式
   - 主键字段 = 字段中 `isPk=true` 的字段，默认为 `{tableName}_id`

### 第四步：变量准备

根据输入计算所有模板变量：

```
核心变量:
- ${tableName}         表名
- ${tableComment}      表注释
- ${ClassName}         类名(大驼峰)
- ${className}         类名(小驼峰)
- ${moduleName}        模块名
- ${businessName}      业务名
- ${BusinessName}      业务名(首字母大写)
- ${packageName}       包路径
- ${author}            作者
- ${datetime}          生成日期
- ${pkColumn}          主键字段信息
- ${columns}           所有字段列表
- ${permissionPrefix}  权限前缀 (格式: moduleName:businessName)
```

### 第五步：代码生成

按顺序读取并填充模板：

1. **读取模板文件**：根据选择的模板组合，从对应 `templates/` 目录加载模板
2. **变量替换**：将 `${变量名}` 替换为实际值
3. **条件处理**：根据字段配置处理 `#if/#foreach` 逻辑
4. **输出代码**：生成最终代码文件

#### 各选项的模板加载路径

| 层级 | 选项一 | 选项二 | 选项三 |
|------|--------|--------|--------|
| Java | `templates/java/` | `templates/java/` | `templates/java/mybatis-plus/` |
| Vue | `templates/vue/` | `templates/vue/v3/` 或 `v3ts/` | `templates/vue/v3/` 或 `v3ts/` |
| XML | `templates/xml/` | `templates/xml/` | `templates/xml/`（仅子表/自定义SQL） |
| JS API | `templates/js/` | `templates/js/` | `templates/js/` |
| SQL | `templates/sql/` | `templates/sql/` | `templates/sql/` |

### 第五步：自检与交付

1. **代码审查**：检查生成的代码是否符合规范
2. **依赖提示**：告知用户需要添加的依赖或配置【这里的告知用户即可，不要做修改操作，除非用户要求 】
3. **使用说明**：提供后续操作指引
4. **选项三额外提示**：
   - 需要引入 `mybatis-plus-boot-starter` 依赖
   - 需要将 `PageHelper` 替换为 `PaginationInnerInterceptor`
   - Mapper 接口需添加 `@Mapper` 注解或在启动类配置 `@MapperScan`

---

## 约束条件 (Constraints)

1. **命名规范**：
   - 类名必须使用大驼峰 (PascalCase)
   - 变量名必须使用小驼峰 (camelCase)
   - 包路径必须全小写

2. **编码规范**：
   - Java 文件使用 UTF-8 编码
   - 缩进使用 4 个空格
   - 必须包含完整的 Javadoc 注释

3. **架构分层**：
   - Controller 只调用 Service 层方法，不直接依赖 MyBatis / MyBatis-Plus API
   - 选项三（MyBatis-Plus）：Service 接口声明业务方法，ServiceImpl 调用 `super.save()` / `super.updateById()` / `super.removeByIds()` 实现
   - 选项三：除子表自定义操作外，不在 ServiceImpl 中直接调用 `baseMapper`
   - 选项三：Service 增删改方法返回 `boolean`（选项一、二返回 `int`）
   - 选项三前置要求：项目 `BaseController` 需包含 `toAjax(boolean result)` 重载方法（RuoYi-Vue-Plus 已内置；若使用标准 RuoYi-Vue 需自行添加）

4. **安全规范**：
   - Controller 必须添加 `@PreAuthorize` 权限注解
   - 删除操作必须添加 `@Log` 日志注解
   - 敏感字段（如密码）不出现在列表展示中

5. **禁止事项**：
   - 不生成测试类（如需要请单独请求）
   - 不修改已存在的文件（除非用户明确要求）
   - 不硬编码任何敏感信息

---

## 选项三 MyBatis-Plus 架构速查

### 调用链路

```
Controller → Service接口(业务方法) → ServiceImpl(MP实现)
                                        ├── super.save()         ← 新增
                                        ├── super.updateById()   ← 修改
                                        ├── super.removeById()   ← 单条删除
                                        ├── super.removeByIds()  ← 批量删除
                                        ├── this.getById()       ← 单条查询
                                        └── this.list(wrapper)   ← 列表查询
```

### ServiceImpl 方法对照

| 业务方法 | 实现方式 | 返回值 |
|---------|---------|--------|
| `selectXxxById(id)` | `this.getById(id)` | 实体对象 |
| `selectXxxList(xxx)` | `LambdaQueryWrapper` + `this.list(wrapper)` | `List<T>` |
| `insertXxx(xxx)` | `super.save(xxx)` | `boolean` |
| `updateXxx(xxx)` | `super.updateById(xxx)` | `boolean` |
| `deleteXxxByIds(ids)` | `super.removeByIds(Arrays.asList(ids))` | `boolean` |
| `deleteXxxById(id)` | `super.removeById(id)` | `boolean` |
| 子表批量新增 | `this.baseMapper.batchXxx(list)` | `int`（走 XML） |
| 子表按FK删除 | `this.baseMapper.deleteXxxByXxx(id)` | `int`（走 XML） |

---

## 字段类型映射

| 数据库类型 | Java类型 | 说明 |
|-----------|---------|------|
| `bigint` | `Long` | 长整型 |
| `int/integer` | `Integer` | 整型 |
| `varchar/char/text` | `String` | 字符串 |
| `datetime/timestamp` | `Date` | 日期时间 |
| `date` | `Date` | 日期 |
| `decimal/numeric` | `BigDecimal` | 高精度数值 |
| `float/double` | `Double` | 浮点数 |
| `tinyint(1)/bit` | `Boolean` | 布尔值 |

---

## 模板引用

- **选项一示例**: [references/examples.md](references/examples.md)
- **选项二参考**: Java 部分同选项一（`templates/java/`），Vue 部分使用 `templates/vue/v3/` 或 `v3ts/`
- **选项三示例**: [references/examples-mybatisPlus.md](references/examples-mybatisPlus.md)
- **编码规范**: [references/coding-standards.md](references/coding-standards.md)
- **Java 标准模板**: `templates/java/` 目录下的 `.vm` 文件
- **Java MP 模板**: `templates/java/mybatis-plus/` 目录下的 `.vm` 文件
- **Vue 模板**: `templates/vue/` 目录下的 `.vm` 文件
- **前端 API 模板**: `templates/js/` 目录下的 `.vm` 文件
- **XML 模板**: `templates/xml/` 目录下的 `.vm` 文件
- **SQL 模板**: `templates/sql/` 目录下的 `.vm` 文件

---

## 兼容版本

| 选项 | 前端 | 后端 |
|------|------|------|
| 选项一 | Vue 2 + Element UI | Spring Boot 2.x + MyBatis |
| 选项二 | Vue 3 + Element Plus (JS/TS) | Spring Boot 2.x + MyBatis |
| 选项三 | Vue 3 + Element Plus (JS/TS) | Spring Boot 2.x + MyBatis-Plus 3.5.x |
