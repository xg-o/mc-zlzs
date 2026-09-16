# mc-zlzs
这是一个支持拼接、预设、查询ID、悬浮窗等的我的世界指令助手，内置1000+可直接复制的成品命令+命令ID和26个命令面板。代码及软件打包由豆包工作任务模式完成。
详细信息（我的世界指令助手源码与文档v1.2.8.zip/我的世界指令助手-源码与文档-v1.2.8/我的世界指令助手-开发文档.md）

# 代码及软件打包由豆包工作任务模式完成。

# 我的世界指令助手
> 当前版本：**v1.2.8（versionCode 11）**

---

## 一、项目是什么

一个**纯本地离线**的 Android 原生应用，帮助《我的世界》玩家快速查找、拼接、复制游戏指令（命令），并提供：

- **命令拼接器**：如 `/give <目标> <物品ID> [数量] [数据值]` 填参数生成完整命令
- **预设命令一键复制**：内置 1641 条命令（含获取命令方块、设置晴天/白天/清掉落物等常用命令）
- **ID 查询**：物品/方块/实体/生物群系/药水/附魔/粒子/音效等 12 类约 1467 项 ID
- **AI 生成命令**：接入 OpenAI 兼容 API（可自定义接口与模型，含本地模型），多会话、会话分类、自定义提示词（如："我的我的世界是移动端基岩版 1.26.45.1，给我命令方块可以使用的指令"）
- **本地 API 联动**：应用内置 HTTP 服务（默认端口 8756，可自定义），供第三方（如自制键盘 App）通过局域网/本地访问指令数据；纯本地，无数据上传
- **悬浮窗**：全局悬浮球 + 可展开功能面板，面板内直接操作（复制/拼接/ID/AI），无需跳转，复制后可直接粘贴进游戏
- **收藏 / 导出导入**：命令、ID、AI 会话可收藏；数据支持全部/部分导出导入（JSON，可压缩）
- **命令源管理**：拼接源 / 预设源 / ID源 三类，支持添加链接自动抓取、开关、删除、内置源可删除可恢复
- **多语言**：简体中文 / 繁体中文 / 英语 / 法语 / 日语（全局生效）
- **主题**：默认紫色（Material 3 风格），支持绿色等预设配色 + 自定义主色（全局生效）
- **隐私引导**：首次启动隐私协议 + 游戏版本向导，根据玩家版本推荐可用指令
- **运行日志**：记录复制/开启/异常，崩溃详情可在「运行日志」页查看（用于排查悬浮窗、API 等闪退）

**硬性约束（用户多次强调，不可违背）**：
1. 纯本地离线、无任何数据上传、开源免费
2. 悬浮窗面板内直接操作，不跳转 Activity
3. 每次升级必须同步更新「更新日志」（关于页 + 5 语言 strings）
4. UI 为 Google Material Design 3 紫色调（默认主色 #6750A4 / #7E57C2 系）
5. 悬浮窗、API 等服务场景禁用 Material 组件（会崩溃），一律用系统组件（Button/EditText/ImageView/LinearLayout）
6. 应用内命令/ID 数量必须与悬浮窗显示数量同步

---

## 二、版本历史（务必延续此记录习惯）

| 版本 | versionCode | 内容 |
|---|---|---|
| v1.0.0 | 1 | 首个 APK：命令查询/复制、ID 查询、拼接、AI（需 API）、收藏、主题紫色 Material3、悬浮球 |
| v1.1.0 | 2 | 多语言（简中/繁中/英/法/日）、主题自定义配色、AI 多会话+分类、模型服务商预设+自定义、数据导出导入、关于页改独立页面（含更新日志/开发者文档/使用教程）、本地 API（局域网/127.0.0.1/localhost） |
| v1.2.0 | 3 | 修 API 启动闪退（Manifest 补 FOREGROUND_SERVICE / FOREGROUND_SERVICE_SPECIAL_USE）、悬浮窗点击闪退、全局崩溃捕获 RunLog + 运行日志页、首次启动隐私协议+版本向导、命令源管理、语言/主题全局重启生效 |
| v1.2.1 | 4 | 修 `InflateException: view_floating_window line #34`（Service 环境 Material 组件崩溃 → 面板全换系统组件）、悬浮窗图标多文件组合（紫渐变圆+图标+绿点）、命令源加内置命令库（内置标签/可开关/禁删）、模型服务商下拉修复（AutoCompleteTextView setThreshold(1)+setOnClickListener(showDropDown)） |
| v1.2.2 | 5 | 悬浮窗面板四入口（拼接/预览/ID/AI → 当时是跳转）、命令源三类自动检测（SourceFetcher.Result 判 template/preset/id + 确认弹窗） |
| v1.2.3 | 6 | 悬浮窗改面板内直接操作（5 视图：recent/builder/preset/ids/ai，点按钮切视图不跳转）、命令扩充到 1087 条（程序化生成 give/summon/gamerule/locate/enchant/effect + 手写 99 条常用）、首页热门命令改「常用命令」精选 8 条、补 16 条漏写 `/` 前缀的命令、源开关联动（内置开关控制预设/拼接模板/ID/悬浮窗各视图） |
| v1.2.4 | 7 | 修 ID 页/拼接页 onResume 重建分类 |
| v1.2.5 | 8 | 修 `IllegalStateException: child already has a parent`（listHost 双挂）+ `NullPointerException`（showView 空防护）、源管理页按三类分组展示（拼接/预设/ID + 内置） |
| v1.2.6 | 9 | 修 renderBuilder 里 Material ChipGroup 在 Service 崩溃 → 换系统 Button 横向滚动 + 高亮（tplBtns）、命令扩到 **1641 条**（+粒子 148 `/particle xxx ~ ~ ~`、+音效 406 `/playsound xxx @s`）、更新日志补 v1.2.5、源三组空组常显「该类型暂无来源」 |
| v1.2.7 | 10 | 添加源弹窗改三个输入框（拼接/预设/ID 各一行：标签+输入框+添加按钮，不再自动检测）、内置源可删除 + 工具栏「+」恢复、悬浮窗最近可删除、悬浮窗数量与应用内同步（预览/ID 顶部「共 N 条」、列表 10→50 条）、更新日志补 v1.2.6 |
| v1.2.8 | 11 | 修内置源删除后 ID 页仍显示内置数据（IdsFragment 内置开关控制分类/搜索）、悬浮窗最近删除按钮明显化（灰紫底圆 + 深色 X）、悬浮窗面板位置自动适配屏幕边界（防面板出屏致关闭按钮不可达）、关闭按钮加大 32dp + 深灰图标 + 浅紫圆底（原白色图标在白色面板不可见）、更新日志补 v1.2.7 |

**升级固定动作（每次都要做）**：
1. `app/build.gradle`：`versionCode +1`、`versionName` 升版
2. `App.java`：启动日志文案 `应用启动 vX.Y.Z` 同步
3. `res/layout/activity_about.xml`：在最新条目（log_v12x）上方插入新版标题 + 条目
4. `res/values*/strings.xml`（5 语言）：加 `log_v12x` 标题与 `log_v12x_items` 内容
5. 全部 strings 改完后跑整体正则转义（裸 `&`→`&amp;`、`'`→`\'`，见「已知坑」），否则 aapt Failed to flatten

---

## 三、目录结构与源码地图

```
我的世界指令助手-APK/
├── settings.gradle / gradle.properties / build.gradle(根,无内容则删)
├── mc-key.jks                 # 正式签名文件（重要，升级必须用它）
└── app/
    ├── build.gradle           # versionCode/versionName/签名配置
    └── src/main/
        ├── AndroidManifest.xml
        ├── assets/data.json   # 全部内置数据（1641 命令/1467 ID/26 模板/9 提供商/默认提示词）
        ├── java/com/mc/cmd/assistant/
        │   ├── App.java                  # 全局 Application：崩溃捕获→RunLog、启动日志文案
        │   ├── BaseActivity.java         # 主题/语言应用基类
        │   ├── MainActivity.java         # 主界面（底部导航：首页/命令/拼接/ID/我的）
        │   ├── ai/                       # AI 对话
        │   │   ├── AiActivity.java       # AI 页（多会话、分类、自定义提示词）
        │   │   ├── AiClient.java         # OpenAI 兼容 API 客户端（流式/非流式）
        │   │   ├── AiAdapter.java / ConvAdapter.java
        │   ├── api/HttpApiService.java   # 本地 HTTP 服务（端口可配，供键盘等第三方取数）
        │   ├── data/
        │   │   ├── DataStore.java        # 解析 data.json → presets/templates/ids/providers
        │   │   ├── Models.java           # Preset/Template/IdEntry/Provider 等数据模型
        │   │   ├── Prefs.java            # 全部 SharedPreferences 封装（关键，见下文）
        │   │   ├── Clip.java             # 剪贴板封装
        │   │   ├── RunLog.java           # 运行日志（复制/开启/异常，环形存储）
        │   │   ├── SourceFetcher.java    # 命令源抓取/类型检测（template/preset/id）
        │   │   └── ExportUtil.java       # 数据导出/导入（全部/部分，JSON，zip）
        │   ├── floating/FloatingService.java  # 悬浮球+功能面板（核心，580 行）
        │   ├── ui/                       # 各页面
        │   │   ├── HomeFragment.java     # 首页（常用命令 8 条 + 搜索入口）
        │   │   ├── PresetFragment.java   # 命令页（分类浏览/搜索/复制/收藏）
        │   │   ├── BuilderFragment.java  # 拼接页（模板填参）
        │   │   ├── IdsFragment.java      # ID 页（12 类 + 来源分类 + 搜索）
        │   │   ├── SourcesActivity.java  # 命令源管理（三输入框添加/分组/内置可删可恢复）
        │   │   ├── SettingsActivity.java # 设置（语言/主题/版本向导/API 端口/模型服务商…）
        │   │   ├── AppearanceActivity.java # 主题自定义配色
        │   │   ├── ApiActivity.java      # 开发者文档/API 接口页（端口、URL 说明）
        │   │   ├── AboutActivity.java    # 关于页（版本/UI 信息/更新日志/开发者文档/使用教程）
        │   │   ├── LogActivity.java      # 运行日志页
        │   │   ├── SearchActivity.java   # 全局搜索（命令/作用）
        │   │   ├── DataActivity.java     # 导出导入
        │   │   ├── MyFragment.java       # 「我的」页（收藏/设置/关于等入口）
        │   │   ├── VersionDialog.java    # 首次启动版本向导
        │   │   └── 各 Adapter.java
        │   └── util/
        │       ├── LangUtil.java         # 语言切换（全局重启生效）
        │       └── ThemeUtil.java        # 主题切换（预设色+自定义色，全局重启生效）
        └── res/
            ├── layout/    # activity_*.xml、fragment_*.xml、view_floating_window.xml（悬浮窗面板布局）
            ├── drawable/  # 悬浮窗图标（紫渐变圆+ic_floating+绿点）、按钮背景等
            ├── menu/      # 工具栏菜单（含 menu_sources.xml 的「+」恢复内置）
            ├── values/    # 默认（中文）strings + colors + themes
            ├── values-zh-rTW/ values-en/ values-fr/ values-ja/   # 5 语言
            ├── values-night/  # 夜间主题
            └── xml/       # 悬浮窗/后台服务相关配置
```

### 关键类要点

**Prefs.java（数据层核心，SharedPreferences 封装）**——后续加功能大概率要动它：
- 内置命令库开关：`getBuiltinEnabled()/setBuiltinEnabled(KEY_BUILTIN_ENABLED)`（返回值含 `&& !getBuiltinDeleted()`，即删除即关闭）
- 内置源删除/恢复：`getBuiltinDeleted()/setBuiltinDeleted()/restoreBuiltin()`（KEY_BUILTIN_DELETED）
- 命令源：`sourcePresets()/sourceTemplates()/sourceIds()/srcCount(type)/sourceCount()/getSources()/saveSources()`
- 最近复制：`getLastCmds()/pushLastCmd()`（上限 20）、`removeLastCmd(String)`
- 其它：`getSettings()/getMcInfo()/isOnboarded`、API 端口 `KEY_API_PORT(8756)`、主题色/语言、收藏、AI 会话/API 配置

**FloatingService.java（悬浮窗，580 行）**：
- 悬浮球可拖动（WindowManager 参数更新）
- 展开面板 `view_floating_window.xml`：标题行（title + floatClose 关闭按钮 32dp 深灰图标浅紫圆底）+ 底部 5 个视图切换按钮（最近/拼接/预设/ID/AI）
- `showView(v)`：按视图名切换 body 内容，`listHost(parent)` 返回 ScrollView 容器（防 child already has a parent）
- 视图渲染：`renderRecent`（delCmdRow 带删除按钮）/ `renderBuilder`（tplBtns 系统 Button 横向滚动+高亮）/ `renderPreset` / `renderIds`（顶部「共 N 条」+ 搜索过滤 + 最多 50 条）/ `renderAi`（面板内 AI 提问）
- `copyCmd(cmd)`：写剪贴板 + RunLog T_COPY + `prefs.pushLastCmd()`
- `expandInner()`：面板位置自适应屏幕边界（x/y 钳制，球在屏幕下方时面板弹到球上方）
- **硬规则：此文件及 Service 环境一律用系统组件，禁止 Material 组件**

**DataStore.java**：`DataStore.get()` 单例，解析 `assets/data.json` → `presets/templates/ids(idcats 12 类)/flatIds()/providers/defaultPrompt`。

**data.json 结构**：
```json
{
  "presets": [ {"c":"分类","cmd":"/give @s command_block","d":"简介，含兼容信息","t":"测试标签(未测试/已测试/提示，UI 已不再显示)"} , ... ]  // 键：c=分类、cmd=命令、d=简介、t=标签(UI隐藏)
  "templates": [ {"id":"give","n":"给予物品 /give","s":"/give <目标> <物品ID> [数量] [数据值] [组件]","args":[{"k":"target","l":"目标","t":"select","o":["@s","@p","@a","@r"],"d":"@s"},...]}, ... ],  // 26 条，s 为模板串；args.t: select/id/number/text/boolean/fix
  "ids": { "blocks":[...], "items":[...], ... },        // 12 类
  "idcats": [[key,名称],...],
  "icatHints": {key:简介},
  "providers": [ {名称, base, key, model, chat} ... ],  // 9 个（含 custom）
  "defaultPrompt": "……基岩版指令助手规则……"
}
```
- presets 分类由字段 c 决定（特殊方块/获取物品/召唤实体/游戏规则/定位/附魔/效果/粒子/音效/常用等）。
- **注意：新增命令时 data.json 键为 c/cmd/d/t，不要与 Models.Preset 字段混淆。**
- **兼容信息**：Models.Preset 内置基岩版兼容范围 `VER_MIN=1.26.43.1 ~ VER_MAX=1.26.45.1`（`inRange(v)` 判断用户版本是否兼容）；首页常用命令与搜索结果的简介行下方显示兼容行（`compat_line`/`compat_ok`/`compat_bad`）。用户设备基岩版 1.26.45.1。

---

## 四、构建与签名（完整命令）

**环境**（本机已验证）：
```bash
export JAVA_HOME=$HOME/jdks/jdk17
export ANDROID_HOME=$HOME/android-sdk
export PATH=$JAVA_HOME/bin:$PATH
# SDK：platforms;android-34 + build-tools;34.0.0；Gradle 8.9（无 wrapper，用独立发行版）
$HOME/gradle-dist/gradle-8.9/bin/gradle assembleRelease --no-daemon
```

**签名**（写死在 app/build.gradle）：
- storeFile: `../mc-key.jks`（随源码打包）
- storePassword / keyAlias / keyPassword：`mc2026assistant` / `mcassistant` / `mc2026assistant`
- 证书：CN=MC Command Assistant, O=Doubao；SHA-256 `206e28bfe2c67151646a4d70075ef9b6b6ad21322b5c5b851a98e628cea3fae1`
- **必须用同一签名升级**，否则用户无法覆盖安装

**产物与验证**：
```bash
APK=app/build/outputs/apk/release/app-release.apk
$ANDROID_HOME/build-tools/34.0.0/aapt dump badging $APK | grep versionName   # 核对 versionName
$ANDROID_HOME/build-tools/34.0.0/apksigner verify --print-certs $APK          # 核对签名
cp $APK "/home/user/Doubao/chats/38441676894628866/我的世界指令助手-<版本>.apk"
```

**每次交付**：用 `present_files` 交付 APK（一次一个）。构建失败先看 `/tmp/gradle_buildN.log`。

---

## 五、已知坑（务必先读，避免重蹈覆辙）

1. **Service 环境禁用 Material 组件**：悬浮窗面板里 inflate 或 new 任何 Material 组件（Chip/ChipGroup/MaterialButton/MaterialCard）会在 Service 环境崩溃（`InflateException` / `IllegalStateException`）。已反复 3 轮踩坑。悬浮窗一切用系统组件。Activity 里可用 Material。
2. **strings 转义**：改完 5 语言 strings.xml 后必须整体跑：裸 `&`→`&amp;`、`'`→`\'`（按整个 string body），再用 minidom 校验，否则 aapt `Failed to flatten XML`（values-fr 多次踩坑）。
3. **listHost 双挂**：悬浮窗把列表 addView 到 `listHost(body)` 返回的 ScrollView 内层 LinearLayout，不要重复 addView 到父级，否则 `child already has a parent`。
4. **`split("\\.")`**：Java 源里写正则要 `split("\\\\.")`（字符串里转义）。
5. **悬浮窗图标**：面板内 `ic_close` 是白色矢量，在白色面板不可见——任何放白底上的图标要固定 tint（如 `android:tint="#5F5F5F"`）或加底色 drawable（`bg_float_close` 浅紫 oval）。
6. **面板位置**：展开面板要钳制在屏幕边界内（wm.getDefaultDisplay().getSize 计算 x/y/width），否则球在屏幕下方/右侧时面板出屏、按钮不可达。
7. **AndroidManifest**：后台服务需 `FOREGROUND_SERVICE` + `FOREGROUND_SERVICE_SPECIAL_USE`，否则 API 服务/悬浮窗启动闪退。
8. **模型服务商下拉**：AutoCompleteTextView 要 `setThreshold(1)` + `setOnClickListener(v -> showDropDown())`，否则点不开（用户反馈过 2 次）。
9. **Toolbar 动态取色**：`android.R.attr` 不能直接 setImageTintList？此前动态取色用 `Resources.getIdentifier` 运行时解析。
10. **悬浮窗/面板视图数量同步**：应用内命令数与悬浮窗显示数要保持一致（顶部显示「共 N 条」）。
11. **崩溃复现**：用户反馈闪退时先看用户上传的 run-log.json（App 内 RunLog + LogActivity 也会记录），定位后再改。
12. **版本相关命令**：基岩版兼容信息显示在命令简介下方（如「基岩版 1.20+」），不做未测试标签。

---

## 六、用户强偏好契约（接续开发必须遵守）

- UI 保持 Google Material Design 3 紫色调（默认主色 #6750A4 / #7E57C2 系，白色卡片、圆角、柔和阴影）
- 纯本地离线、无数据上传、开源免费；开发者文档以本地 API 供第三方（如键盘）取数
- 悬浮窗面板内直接操作、不跳转；复制后不退出游戏即可粘贴
- 每次更新必须同步更新日志（5 语言 + 关于页）
- 命令/ID 数量与悬浮窗同步；内置源可删可恢复；添加源分拼接/预设/ID 三类输入框
- 语言、主题全局生效（重启后全应用生效）
- 用户设备为移动端基岩版；命令兼容信息要准确，不要标错版本
- 用中文回复用户；交付 APK 一次一个

## 七、后续可升级方向（建议）

1. 命令数继续扩充（红石/记分板/数据包/函数等），保持程序化生成 + 手写校验
2. 悬浮窗面板支持拖动面板本身、宽度记忆
3. AI 支持流式输出、更多提供商模板、密钥本地加密存储
4. 命令源支持更多格式（JSON Schema 自动识别、GitHub 源一键导入）
5. 键盘 App 联动（用户已约定：MC 项目完成后写一个键盘软件，通过本应用本地 API 推送指令）
6. 深色主题适配完整化、动态取色（Material You）
7. 桌面小部件、快捷方式直达常用命令
8. 数据加密导出 / 多设备同步（需用户同意联网，当前硬约束离线）

---

## 八、本次打包内容

- `我的世界指令助手-APK/`：完整源码工程（含签名 mc-key.jks、assets/data.json、5 语言、全部 Java）
- `我的世界指令助手-开发文档.md`：本文档
- `我的世界指令助手-1.2.8.apk`：最新交付 APK
- `HTML原型/`：早期 HTML 版原型（仅参考，勿作为交付物）

> 解压后在已配置 JDK17 + Android SDK 34 的环境执行第四节构建命令即可产出同签名 APK。
