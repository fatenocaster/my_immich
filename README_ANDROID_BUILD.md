# Android 构建说明

## 概述
此仓库已经简化为只保留 Android 应用构建功能。所有其他不必要的 workflow 已被删除。

## 构建流程

### 自动构建
- 当推送到 `main` 分支时自动触发构建
- 当创建 Pull Request 时自动触发构建
- 可以手动触发构建（在 GitHub Actions 页面点击 "Run workflow"）

### 构建类型
根据是否配置了签名密钥，会构建不同类型的 APK：

#### 1. 调试版本 (无签名密钥时)
- 构建 debug APK
- 无需任何 secrets 配置
- 适合开发和测试

#### 2. 发布版本 (有签名密钥时)
- 构建 release APK（已签名）
- 需要配置以下 secrets：
  - `KEY_JKS`: 密钥库文件的 base64 编码
  - `ALIAS`: 密钥别名
  - `ANDROID_KEY_PASSWORD`: 密钥密码
  - `ANDROID_STORE_PASSWORD`: 密钥库密码

## 如何配置签名密钥（可选）

如果你想构建签名的 release 版本，需要在 GitHub repository 的 Settings > Secrets and variables > Actions 中添加以下 secrets：

1. **KEY_JKS**: 你的 Android 签名密钥库文件的 base64 编码
   ```bash
   base64 -i your-key.jks | tr -d '\n'
   ```

2. **ALIAS**: 密钥别名

3. **ANDROID_KEY_PASSWORD**: 密钥密码

4. **ANDROID_STORE_PASSWORD**: 密钥库密码

## 下载构建结果

构建完成后，可以在 GitHub Actions 的 run 页面下载生成的 APK 文件：
- Artifact 名称: `android-apk`
- 包含多个 APK 文件（适用于不同的 CPU 架构）

## 注意事项

1. 构建在 macOS 环境中进行，以确保最佳的 Flutter 构建性能
2. 如果没有配置签名密钥，会自动构建调试版本
3. 支持多架构构建（ARM, ARM64, x64）

## 原始项目
此项目基于 [Immich](https://github.com/immich-app/immich) 项目，仅保留了移动端构建功能。 