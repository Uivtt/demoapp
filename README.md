# DemoApp — Kotlin + Jetpack Compose (Material3)

一个使用 Gradle Kotlin DSL 的标准 Android 工程骨架，构建过程完全在 **GitHub Actions** 上完成（本地无需安装 JDK / Gradle / Android SDK）。

## 结构

```
demoapp/
├── .github/workflows/android-ci.yml   # 云端构建：出 debug APK 产物
├── settings.gradle.kts                # 仓库与模块声明
├── build.gradle.kts                   # 根构建脚本（AGP / Kotlin 插件）
├── gradle.properties
├── gradle/libs.versions.toml          # 版本目录（AGP 8.7.3 / Kotlin 2.0.21 / Compose BOM 2024.10.01）
├── gradle/wrapper/gradle-wrapper.properties
└── app/
    ├── build.gradle.kts               # compileSdk 35 / minSdk 26 / Compose 开启
    └── src/main/
        ├── AndroidManifest.xml
        ├── java/com/example/demoapp/
        │   ├── MainActivity.kt        # 可交互计数器示例
        │   └── ui/theme/              # Color / Theme / Type（支持动态取色）
        └── res/
```

## 用 GitHub 构建（无需本地环境）

1. 在 GitHub 新建空仓库（不要勾选 README），把本目录内容推送上去：

```bash
cd demoapp
git init -b main
git add .
git commit -m "feat: init Compose demoapp skeleton"
git remote add origin https://github.com/<你的账号>/<仓库名>.git
git push -u origin main
```

2. 推送后 `Android CI` 工作流自动运行（也可在 Actions 页手动 `Run workflow`）。
3. 构建结束后在 **Actions → 对应运行 → Artifacts** 下载 `demoapp-debug-apk`，内含 `app-debug.apk`。

## 工作流做了什么

| 步骤 | 说明 |
| --- | --- |
| setup-java 17 | 提供 JDK 17 |
| gradle/actions/setup-gradle | 提供 Gradle 8.9（AGP 8.7.3 要求 ≥ 8.9） |
| Install Android SDK packages | 复用 runner 预装 SDK，补齐 platform 35 与 build-tools 35.0.0，写入 local.properties |
| gradle wrapper | 生成缺失的 `gradle-wrapper.jar` |
| ./gradlew assembleDebug | 产出 debug APK |

## 本地构建（可选，需自备工具链）

JDK 17 + Android SDK（platform 35）+ Gradle 8.9 以上，然后 `./gradlew assembleDebug`。