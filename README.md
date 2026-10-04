# 小恐龙快跑 (Dino Game)

一个仿 Chrome 小恐龙（T-Rex Run）游戏的 Android 应用，体积极小（约 16 KB），支持 Android 6.0+（含 Android 9 及以上）。

## 玩法

- 点击 / 触摸屏幕上半部分 / 按空格键 / 方向键 ↑：跳跃
- 触摸屏幕下半部分 / 方向键 ↓：下蹲
- 躲避仙人掌和飞鸟，跑得越远分数越高
- 撞到障碍物游戏结束，点击可重新开始
- 自动记录最高分（本地存储）

## 特点

- 纯 HTML5 Canvas + 像素风格绘制，无第三方依赖
- 单 Activity + WebView，体积小巧（约 16 KB）
- 全屏沉浸式显示，横屏运行
- 支持触摸与键盘双操作

## 安装

直接安装 `DinoGame.apk` 即可（Android 6.0 及以上）。

## 项目结构

```
├── AndroidManifest.xml          # 应用清单
├── assets/
│   └── index.html               # 游戏主体（HTML5）
├── src/com/dinogame/
│   └── MainActivity.java        # 入口 Activity（全屏 WebView）
├── res/mipmap/ic_launcher.png   # 应用图标
└── DinoGame.apk                 # 构建好的 APK
```

## 自行构建

需要 aapt2、javac（JDK 8+）、d8、apksigner、zipalign 及 Android SDK 的 android.jar：

```bash
# 1. 编译资源
aapt2 compile --dir res -o res.zip
# 2. 链接资源与清单
aapt2 link -o app.apk -I $ANDROID_JAR --manifest AndroidManifest.xml -A assets res.zip
# 3. 编译 Java
javac -source 8 -target 8 -classpath $ANDROID_JAR -d classes src/com/dinogame/MainActivity.java
# 4. 生成 dex
d8 --lib $ANDROID_JAR --release --output . classes/com/dinogame/MainActivity.class
# 5. 打包 dex 并对齐
zip -j app.apk classes.dex && zipalign -f 4 app.apk app-aligned.apk
# 6. 签名
apksigner sign --ks keystore --out DinoGame.apk app-aligned.apk
```

## 许可

MIT License
