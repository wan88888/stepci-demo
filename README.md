# Step CI Demo

这是一个使用 Step CI 进行 API 测试的演示项目。

## 项目简介

本项目展示了如何使用 Step CI 框架对 JSONPlaceholder API 进行自动化测试，并集成 GitHub Actions 实现持续集成。

## 项目结构

```
stepci-demo/
├── .github/
│   └── workflows/
│       └── api-tests.yml    # GitHub Actions 工作流配置
├── workflow.yml             # Step CI 测试配置文件
└── README.md               # 项目说明文档
```

## 功能特性

- ✅ 使用 Step CI 进行 API 测试
- ✅ JSONPath 验证响应数据
- ✅ GitHub Actions 自动化测试
- ✅ 支持多分支触发
- ✅ 测试结果自动上传

## 快速开始

### 前置要求

- Node.js 18+ 
- npm 或 yarn

### 安装 Step CI

```bash
# 全局安装
npm install -g stepci

# 或者使用 yarn
yarn global add stepci
```

### 运行测试

```bash
# 执行 API 测试
stepci run workflow.yml
```

## 测试配置说明

### workflow.yml

当前测试配置验证 JSONPlaceholder API 的以下内容：

- **端点**: `GET https://jsonplaceholder.typicode.com/posts/1`
- **状态码**: 200
- **响应数据**:
  - `userId`: 1
  - `id`: 1

### 自定义测试

你可以修改 `workflow.yml` 来测试其他 API：

```yaml
version: "1.1"
name: 自定义 API 测试
tests:
  customTest:
    steps:
      - name: 测试自定义端点
        http:
          url: https://your-api.com/endpoint
          method: GET
          check:
            status: 200
            jsonpath:
              $.field: "expected_value"
```

## CI/CD 集成

### GitHub Actions

项目配置了自动化测试流水线：

- **触发条件**:
  - 推送到 `main` 或 `develop` 分支
  - 创建针对 `main` 分支的 Pull Request

- **执行步骤**:
  1. 检出代码
  2. 设置 Node.js 环境
  3. 安装 Step CI
  4. 运行 API 测试
  5. 上传测试结果

### 本地开发

```bash
# 克隆项目
git clone <repository-url>
cd stepci-demo

# 运行测试
stepci run workflow.yml
```

## 最佳实践

### 1. 环境管理

```yaml
env:
  host: api.example.com
  version: v1
```

### 2. 数据验证

```yaml
check:
  status: 200
  jsonpath:
    $.data[*].id:
      - isNumber: true
    $.data[*].name:
      - isString: true
```

### 3. 请求链

```yaml
steps:
  - name: 创建资源
    http:
      url: https://api.example.com/posts
      method: POST
      json:
        title: "测试标题"
      captures:
        postId:
          jsonpath: $.id
  
  - name: 获取创建的资源
    http:
      url: https://api.example.com/posts/${{captures.postId}}
      method: GET
```

## 故障排除

### 常见问题

1. **测试失败**: 检查 API 端点是否可访问
2. **YAML 语法错误**: 使用 YAML 验证工具检查语法
3. **GitHub Actions 失败**: 查看 Actions 日志获取详细错误信息

### 调试技巧

```bash
# 详细输出模式
stepci run workflow.yml --verbose

# 检查配置文件语法
stepci validate workflow.yml
```

## 贡献指南

1. Fork 本项目
2. 创建功能分支 (`git checkout -b feature/amazing-feature`)
3. 提交更改 (`git commit -m 'Add some amazing feature'`)
4. 推送到分支 (`git push origin feature/amazing-feature`)
5. 创建 Pull Request

## 许可证

本项目采用 MIT 许可证 - 查看 [LICENSE](LICENSE) 文件了解详情。

## 相关链接

- [Step CI 官方文档](https://docs.stepci.com/)
- [JSONPlaceholder API](https://jsonplaceholder.typicode.com/)
- [GitHub Actions 文档](https://docs.github.com/en/actions)

## 更新日志

### v1.0.0
- ✅ 初始项目设置
- ✅ 基础 API 测试配置
- ✅ GitHub Actions 集成
- ✅ 项目文档