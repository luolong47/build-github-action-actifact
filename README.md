# SDKMAN构建物生成器

这个项目提供了一个GitHub Action工作流，用于构建包含JDK21和Maven最新版本的SDKMAN构建物，专门针对Linux ARM64架构。

## 功能特性

- ✅ 自动构建Linux ARM64版本的SDKMAN构建物
- ✅ 内置JDK 21最新版本
- ✅ 内置Maven最新版本
- ✅ 使用QEMU模拟器进行跨平台构建
- ✅ 自动生成环境配置脚本
- ✅ 支持7z压缩格式
- ✅ 自动生成校验和文件
- ✅ 包含详细的使用文档

## 项目结构

```
.
├── .github/
│   └── workflows/
│       └── build-sdkman-artifact.yml    # GitHub Action工作流
├── scripts/                             # 构建脚本（自动生成）
├── build-artifact/                      # 构建物输出目录
├── test-build.sh                       # 本地测试脚本
└── README.md                           # 项目说明
```

## 使用方法

### 1. 克隆项目

```bash
git clone <repository-url>
cd build-github-action-actifact
```

### 2. 推送到GitHub

将代码推送到GitHub仓库，工作流会自动触发：

```bash
git add .
git commit -m "初始化SDKMAN构建物生成器"
git push origin main
```

### 3. 手动触发构建

在GitHub Actions页面可以手动触发构建。

### 4. 下载构建物

构建完成后，在Actions页面的artifacts中下载：
- `sdkman-artifact-linux-arm64` - 完整构建物包

## 构建物使用说明

### 解压构建物

```bash
7z x sdkman-java21-maven-linux-arm64-*.7z
cd sdkman-java21-maven-linux-arm64
```

### 配置环境

```bash
source ./setup-env.sh
```

### 验证安装

```bash
java -version
mvn -version
```

## 工作流详情

### 触发条件

- 推送到main/master分支
- 手动触发（workflow_dispatch）

### 构建步骤

1. **环境准备**：设置QEMU模拟器和Docker Buildx
2. **脚本生成**：创建安装和打包脚本
3. **ARM64构建**：在ARM64容器中执行构建
4. **打包压缩**：生成tar.gz和zip格式
5. **校验和生成**：创建SHA256校验和文件
6. **上传构建物**：保存到GitHub Actions artifacts

### 安全特性

- 使用官方的GitHub Actions
- 在隔离的容器环境中构建
- 包含基础安全扫描步骤

## 技术规格

- **目标平台**: Linux ARM64
- **Java版本**: JDK 21 (通过SDKMAN最新版本)
- **Maven版本**: 最新稳定版 (通过SDKMAN)
- **构建环境**: Ubuntu 22.04 + QEMU
- **压缩格式**: 7z
- **校验和**: SHA256

## 自定义配置

### 修改Java版本

编辑 `.github/workflows/build-sdkman-artifact.yml` 中的Java安装部分：

```yaml
sdk install java 21.0.2-tem  # 指定具体版本
```

### 添加其他工具

在安装脚本中添加其他SDKMAN支持的工具：

```bash
sdk install gradle
sdk install groovy
```

## 故障排除

### 构建失败

1. 检查GitHub Actions日志
2. 确认网络连接正常
3. 检查SDKMAN服务状态

### 构建物问题

1. 验证校验和
2. 重新解压构建物（使用7z x命令）
3. 检查文件权限

## 贡献指南

1. Fork项目
2. 创建功能分支
3. 提交更改
4. 创建Pull Request

## 许可证

本项目采用MIT许可证。

## 联系方式

如有问题或建议，请创建GitHub Issue。