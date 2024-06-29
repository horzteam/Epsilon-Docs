# 账户管理系统 API 文档

## 简介

这是一个使用 Node.js 和 Express.js 实现的账户管理系统 API。系统使用 MySQL 作为数据库，并且所有的配置信息存储在 `.env` 文件中。

## 安装与配置

### 安装步骤
1. 克隆

2. 安装依赖

   ```
   bash
   npm install
   ```

3. 配置环境变量

   在项目根目录下创建 `.env` 文件，并添加以下内容：

   ```
   envPORT=5000
   DB_HOST=localhost
   DB_USER=root
   DB_PASSWORD=yourpassword
   DB_NAME=account_management
   JWT_SECRET=your_jwt_secret
   EMAIL_HOST=smtp.your-email-provider.com
   EMAIL_PORT=587
   EMAIL_USER=your-email@example.com
   EMAIL_PASSWORD=your-email-password
   ```

4. 启动服务器

   ```
   bash
   npm start
   ```

## API 端点

### 注册

**URL:** `/api/auth/reg`
**方法:** `POST`
**描述:** 注册新用户
**请求体:**

```
json{
  "username": "exampleUser",
  "email": "example@example.com",
  "password": "password123"
}
```

**响应:**

- 成功:

   

  ```
  201 Created
  ```

  ```
  json{
    "message": "User registered"
  }
  ```

- 失败:

   

  ```
  400 Bad Request
  ```

  ```
  json{
    "message": "Error message"
  }
  ```

### 验证码发送

**URL:** `/api/auth/emailsend`
**方法:** `POST`
**描述:** 发送验证码到指定邮箱
**请求体:**

```
json{
  "email": "example@example.com"
}
```

**响应:**

- 成功:

   

  ```
  200 OK
  ```

  ```
  json{
    "message": "Email sent"
  }
  ```

- 失败:

   

  ```
  500 Internal Server Error
  ```

  ```
  json{
    "message": "Error message"
  }
  ```

### 登录

**URL:** `/api/auth/login`
**方法:** `POST`
**描述:** 用户登录，返回 JWT
**请求体:**

```
json{
  "email": "example@example.com",
  "password": "password123"
}
```

**响应:**

- 成功:

   

  ```
  200 OK
  ```

  ```
  json{
    "token": "JWT token"
  }
  ```

- 失败:

   

  ```
  401 Unauthorized
  ```

  ```
  json{
    "message": "Invalid credentials"
  }
  ```

### 用户信息

**URL:** `/api/auth/userinfo`
**方法:** `GET`
**描述:** 获取用户信息
**请求头:**

```
json{
  "Authorization": "Bearer JWT token"
}
```

**响应:**

- 成功:

   

  ```
  200 OK
  ```

  ```
  json{
    "id": 1,
    "username": "exampleUser",
    "email": "example@example.com",
    "created_at": "2024-01-01T00:00:00.000Z"
  }
  ```

- 失败:

   

  ```
  401 Unauthorized
  ```

  ```
  json{
    "message": "Not authorized"
  }
  ```

### 用户信息更新

**URL:** `/api/auth/userupdateinfo`
**方法:** `PUT`
**描述:** 更新用户信息
**请求头:**

```
json{
  "Authorization": "Bearer JWT token"
}
```

**请求体:**

```
json{
  "username": "newUsername",
  "email": "newemail@example.com"
}
```

**响应:**

- 成功:

   

  ```
  200 OK
  ```

  ```
  json{
    "message": "User info updated"
  }
  ```

- 失败:

   

  ```
  400 Bad Request
  ```

  ```
  json{
    "message": "Error message"
  }
  ```

### 用户忘记密码

**URL:** `/api/auth/userforgot`
**方法:** `POST`
**描述:** 发送重置密码邮件
**请求体:**

```
json{
  "email": "example@example.com"
}
```

**响应:**

- 成功:

   

  ```
  200 OK
  ```

  ```
  json{
    "message": "Password reset email sent"
  }
  ```

- 失败:

   

  ```
  500 Internal Server Error
  ```

  ```
  json{
    "message": "Error message"
  }
  ```

## 数据库初始化

请使用以下 SQL 指令来初始化数据库：

```
sqlCREATE DATABASE account_management;

USE account_management;

CREATE TABLE users (
  id INT AUTO_INCREMENT PRIMARY KEY,
  username VARCHAR(255) NOT NULL,
  email VARCHAR(255) NOT NULL UNIQUE,
  password VARCHAR(255) NOT NULL,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

## 项目结构

```
unknown/account-management
|-- .env
|-- package.json
|-- server.js
|-- /routes
|   |-- auth.js
|-- /controllers
|   |-- authController.js
|-- /models
|   |-- userModel.js
|-- /config
|   |-- db.js
|-- /utils
|   |-- generateToken.js
|   |-- sendEmail.js
|-- /middleware
|   |-- authMiddleware.js
```

### 文件说明

- **server.js**: 服务器入口文件。
- **/routes/auth.js**: 定义所有认证相关的路由。
- **/controllers/authController.js**: 实现各个路由对应的控制器函数。
- **/models/userModel.js**: 用户模型（在本项目中未使用，保留以供扩展）。
- **/config/db.js**: 数据库连接配置。
- **/utils/generateToken.js**: 生成 JWT 的工具函数。
- **/utils/sendEmail.js**: 发送邮件的工具函数。
- **/middleware/authMiddleware.js**: 认证中间件，用于保护需要认证的路由。

## 结语

请确保在部署之前更新 `.env` 文件中的配置信息，并根据需要调整代码。此文档和代码适用于一个简单的账户管理系统，实际使用中可能需要根据具体需求进行扩展和修改。
