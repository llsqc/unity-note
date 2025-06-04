
### 1. **Commit 信息结构**
通常，一个规范的 commit 信息包括三个部分：**标题**、**正文**和**脚注**。格式如下：

```
<type>(<scope>): <subject>

<body>

<footer>
```

- **标题（Title/Subject）**：简洁地描述本次提交的内容，通常不超过 50 个字符。
- **正文（Body）**：详细描述本次提交的内容，解释为什么进行这些更改，以及如何实现这些更改。每行不超过 72 个字符。
- **脚注（Footer）**：可选部分，通常用于引用相关的 issue 或 bug，或者提供其他相关信息。

### 2. **标题格式**
标题通常采用以下格式：

```
<type>(<scope>): <subject>
```

- **type**：提交的类型，常见的类型包括：
  - `feat`：新功能（feature）
  - `fix`：修复 bug
  - `docs`：文档更新
  - `style`：代码格式调整（不影响代码逻辑）
  - `refactor`：代码重构（既不修复 bug 也不添加新功能）
  - `test`：添加或修改测试用例
  - `chore`：构建过程或辅助工具的变动
  - `perf`：性能优化
  - `ci`：持续集成相关的更改
  - `revert`：回滚之前的提交

- **scope**：可选部分，表示本次提交影响的范围或模块。例如 `(auth)`、`(api)` 等。

- **subject**：简洁的描述本次提交的内容，使用祈使句，首字母小写，不加句号。

**示例**：
```
feat(auth): add user authentication
fix(api): resolve null pointer exception
docs(readme): update installation instructions
```

### 3. **正文（Body）**
正文部分用于详细描述本次提交的内容。可以解释为什么进行这些更改，以及如何实现这些更改。

**示例**：
```
The user authentication feature was added to allow users to log in and access their accounts. 
This includes the implementation of JWT tokens for secure authentication.
```

### 4. **脚注（Footer）**
通常用于引用相关的 issue 或 bug，或者提供其他相关信息。

**示例**：
```
Closes #123
Fixes #456
```

### 5. **示例**
```
feat(auth): add user authentication

This commit introduces user authentication using JWT tokens. 
Users can now log in and access their accounts securely.

Closes #123
```

```
fix(api): resolve null pointer exception

A null pointer exception was occurring in the user profile API endpoint. 
This commit adds a null check to prevent the exception.

Fixes #456
```
