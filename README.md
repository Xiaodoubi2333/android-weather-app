# android-weather-app v1.1

Android 天气应用，课程项目「项目 1」。实现省 / 市 / 县三级地区选择、当前天气与未来预报、空气质量与生活建议、后台定时刷新，数据取自《第一行代码》配套的 `guolin.tech` 天气接口。

工程基于 Android Studio 3.5.2 环境，包名 `com.coolweather.android`，`compileSdk 28 / minSdk 15 / targetSdk 24`。

## 文件结构

```text
app/src/main/java/com/coolweather/android/
├── MainActivity.java               入口，判断本地是否已选城市
├── WeatherActivity.java            天气主页（当前 / 预报 / AQI / 建议）
├── ChooseAreaFragment.java         省 - 市 - 县 三级联动选择
├── db/                             LitPal 实体：Province / City / County
├── gson/                           接口返回体：Basic / Now / Forecast / AQI / Suggestion / Weather
├── service/AutoUpdateService.java  后台定时刷新天气
└── util/                           HttpUtil（OkHttp 封装）、Utility（JSON 解析并落库）
app/src/main/res/layout/            9 个布局文件
```

## 启动

1. 用 Android Studio 3.5.2 及以上版本打开工程根目录，等待 Gradle 同步完成；
2. 直接运行 `app` 模块，无需配置服务器；
3. 首次进入会请求网络权限，选择「省 → 市 → 县」后自动拉取天气。

依赖：`appcompat 1.0.0`、`glide 3.7.0`、`litepal 1.3.2`、`okhttp 3.4.1`、`gson 2.7`；权限仅 `INTERNET`。

## 功能落点表

| # | 功能 | 状态 | 落点 |
|---|---|---|---|
| 1 | 省 / 市 / 县三级选择 | 完成 | `ChooseAreaFragment` + LitPal 三张表 |
| 2 | 选中城市持久化，再次进入直接进天气页 | 完成 | `MainActivity` 读 `Province` 首选记录 |
| 3 | 实时天气（温度 / 天气现象 / 体感） | 完成 | `WeatherActivity.fetchWeather()` |
| 4 | 未来三天预报 | 完成 | `gson/Forecast.java` + `Utility.handleForecastResponse()` |
| 5 | 空气质量 AQI 与等级配色 | 完成 | `gson/AQI.java` |
| 6 | 生活建议（舒适度 / 洗车 / 运动） | 完成 | `gson/Suggestion.java` |
| 7 | 天气背景图随城市切换 | 完成 | Glide 加载 `guolin.tech/api/bing_pic` |
| 8 | 下拉刷新 | 完成 | `WeatherActivity` + `SwipeRefreshLayout` |
| 9 | 后台每 8 小时自动更新 | 完成 | `AutoUpdateService` + `AlarmManager` |

## 版本记录

### v0.1 · 项目初始化

- `feat: 空工程跑通编译（Hello, World）`：模板工程编译通过。

### v0.2 · 工程骨架

- `chore(gradle): 调整 settings.gradle 与工程结构`：Gradle 配置反复失败，先把工程结构固定下来。
- `feat: 统一包名为 com.coolweather.android，布局初稿`：包名与 XML 设计成型。

### v0.3 · 布局补全

- `feat(ui): 补齐 9 个布局，业务逻辑留空`：界面画完，Java 侧仍为空。

### v0.4 · 导入完整教学源码

- `feat: 回退 Android Studio 3.5.2 并导入完整教学源码`：因工具链版本冲突回退。
- `chore: 清空目录以重新导入（Gradle 版本冲突）`：清空后重来，避免残余配置干扰。
- `feat: 重新导入教学源码并修正 Git 导入导出流程`：源码与 `.gitignore` 一并校正。

### v1.0 · 升级到 AndroidX

- `build: 升级 Gradle、AGP 与 SDK 版本`：工具链整体升级。
- `refactor!: 迁移到 AndroidX，回归原功能`：迁移完成，当前 `versionName 1.1`。

## 已知限制

- 接口为 `guolin.tech` 教学接口，长期可用性不保证；请求密钥直接写在 `HttpUtil`，**仅限课程练习，请勿用于生产**。
- `targetSdk 24` 与 `minSdk 15` 低于当前应用商店要求，真机安装需允许「未知来源」。
- 与本仓库同属一套教材代码的还有 [`android-weather-04-fragments`](https://github.com/Xiaodoubi2333/android-weather-04-fragments)、[`android-weather-05-navigation`](https://github.com/Xiaodoubi2333/android-weather-05-navigation)，三者是教材的不同阶段。
