# 如何构建mc服务器

确定客户端整合包的版本，本人只接触了forge，所以只介绍forge的搭建

确定forge的的版本，例如游戏版本1.20.1和forge47.3.22

就去下载对应版本的forge

https://files.minecraftforge.net/net/minecraftforge/forge/

注意下载installer

长这样：

![image-20251120104725010](https://cdn.jsdelivr.net/gh/wudiloveyh/my-image-bucket@main/images/20251120104725085.png)

然后上传到你的linux服务器，例如/home/mc/luomuqu 

这里我的服务器系统是ubuntu，需要安装java17

```bash
apt install openjdk-17-jre-headless
```

java和上面的jar文件都处理好后，在/home/mc/luomuqu目录下面运行

```bash
java -jar forge-1.20.1-47.3.22-installer.jar --installServer
```

等待安装完成即可：

![image-20251120104936167](https://cdn.jsdelivr.net/gh/wudiloveyh/my-image-bucket@main/images/20251120104936217.png)

运行后会生成如下文件：

![image-20251120105029149](https://cdn.jsdelivr.net/gh/wudiloveyh/my-image-bucket@main/images/20251120105029205.png)

然后迁移你的整合包的内容

可以把下面的内容复制到服务器：

```bash
config/
defaultconfigs/
kubejs/
patchouli_books/
tacz/
tlm_custom_pack/
```

不要复制：

```bash
resourcepacks/
shaderpacks/
PCL/
xaero/
XaeroWaypoints_BACKUP240807/
fancymenu_data/
.local/
logs/
saves/
keybinding presets/
options.txt
BTLib.dll
```

而mod部分需要修剪掉服务端不需要的mod

我没有什么比较好的办法，只能问ai哪些不需要

至少落幕曲可以用下面的bash脚本：

```bash
#!/bin/bash

MOD_DIR="./mods"

# 客户端专用 Mod 列表（你发给我的）
CLIENT_MODS=(
  "itemview.*"
  "appleskin.*"
  "bettercombat.*"
  "betterdays.*"
  "camera.*"
  "catalogue.*"
  "craftpresence.*"
  "customhud.*"
  "debugify.*"
  "embeddium.*"
  "embeddiumplus.*"
  "entity_model_features.*"
  "entity_texture_features.*"
  "fancymenu.*"
  "fastquit.*"
  "fpsreducer.*"
  "glowcase.*"
  "holdthatchunk.*"
  "immediatelyfast.*"
  "inventory_profiles_next.*"
  "iris.*"
  "journeymap.*"
  "lithium.*"
  "modernfix.*"
  "oculus.*"
  "patchouli.*"
  "paxi.*"
  "preview_animations.*"
  "replaymod.*"
  "screenshot_viewer.*"
  "sound_physics_remastered.*"
  "zoomify.*"
)

echo "🧹 开始清理客户端专用 mod..."

for pattern in "${CLIENT_MODS[@]}"; do
  matches=$(ls "$MOD_DIR" | grep -Ei "$pattern")
  
  if [[ ! -z "$matches" ]]; then
    echo "❌ 删除：$matches"
    rm -f "$MOD_DIR"/$pattern
  fi
done

echo "✅ 清理完成！"
```

