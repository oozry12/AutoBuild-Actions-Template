## Actions for Building OpenWRT

测试通过的设备: `d-team_newifi-d2(bin)`、`x86_64(img、img.gz)`

测试通过的源码: `coolsnowwolf/lede:master`、`immortalwrt/immortalwrt:openwrt-18.06`

## Github Actions 部署指南(STEP 1):

1. 首先需要获取 **Github Token**: [点击这里](https://github.com/settings/tokens/new) 获取,

   `Note`项填写一个名称,`Select scopes`不理解就**全部打勾**,操作完成后点击下方`Generate token`

2. 复制页面中生成的 **Token**,并保存到本地,**Token 只会显示一次!**

3. **Fork** 我的`AutoBuild-Actions`仓库,然后进入你的`AutoBuild-Actions`仓库进行之后的设置

4. 点击上方菜单中的`Settings`,依次点击`Secrets`-`New repository secret`

   其中`Name`项填写`GITHUB_TOKEN`,然后将你的 **Token** 粘贴到`Value`项,完成后点击`Add secert`

## 客制化固件(STEP 2):

1. 进入你的`AutoBuild-Actions`仓库,**下方所有操作都将在你的`AutoBuild-Actions`仓库下进行**

2. 把本地的 '.config' 文件重命名为**设备名称**并上传到`/Configs`

3. 编辑`.github/workflows/*.yml`文件,修改`第 9 和 29 行`为你的**设备名称**

   **更换源码与分支** 修改`第 27 行 REPO_URL:`为源码仓库地址,`第 28 行`为分支

4. 按照需求编辑`Scripts/AutoBuild_DiyScript.sh`文件的 **Firmware-Diy() 函数**

   **软件包列表** 编辑`CustomPackages`下对应**设备名称**的文件,按照现有语法为**特定设备**添加软件包

   **定时编译** 先删除`第 20-21 行 #`注释,然后按需修改相关参数,[使用方法](https://www.runoob.com/w3cnote/linux-crontab-tasks.html)

   **一键编译** 先删除`第 23-24 行 #`注释,单(双)击右上角 **Star** 重新点亮 **Star** 即可一键编译

**AutoBuild_DiyScript.sh Diy_Core() 的赋值解释:**
```
   Author 作者名称,这个名称将在 OpenWrt 首页显示

   Default_Device 设备的官方名称,例如 [d-team_newifi-d2]、[x86_64]

   INCLUDE_AutoUpdate 启用后,将自动添加 Scripts/AutoUpdate.sh 和 luci-app-autoupdate 到固件

   INCLUDE_AutoBuild_Tools 添加 Scripts/AutoBuild_Tools.sh 到固件

   INCLUDE_DRM_I915 添加 Intel Graphics 驱动(仅部分 x86_64 平台可用)

   INCLUDE_Obsolete_PKG_Compatible 优化原生 OpenWrt-19.07、21.02 支持(测试特性)
```

   **其他指令:** 编辑`Scripts/AutoBuild_DiyScript.sh`,参照下方语法:
```
   [使用 git clone 拉取文件]  ExtraPackages git 存放位置 软件包名 仓库地址 分支

   [使用 svn checkout 拉取文件]  ExtraPackages svn 存放位置 软件包名 仓库地址/trunk/目录

   [替换 /CustomFiles 文件到源码] Replace_File 文件名称 目标路径 重命名(可选)
```

## 使用一键更新固件脚本:

   首先需要打开 Openwrt 主页,点击`系统`-`TTYD 终端`或者在浏览器输入`192.168.1.1:7681`,按需输入下方指令:
   
   检查并更新固件(保留配置): `bash /bin/AutoUpdate.sh`

   检查并更新固件(不保留配置): `bash /bin/AutoUpdate.sh -n`
   
   更多使用方法: `bash /bin/AutoUpdate.sh -help`
   
   **注意: 一键更新固件需要在 Diy-Core() 函数中启用`INCLUDE_AutoUpdate`**
   
## 鸣谢

   - [Lean's Openwrt Source code](https://github.com/coolsnowwolf/lede)

   - [P3TERX's Actions-OpenWrt Project](https://github.com/P3TERX/Actions-OpenWrt)

   - [P3TERX's Blog](https://p3terx.com/archives/build-openwrt-with-github-actions.html)

   - [ImmortalWrt](https://github.com/immortalwrt)

   - [eSir's workflow](https://github.com/esirplayground/AutoBuild-OpenWrt/blob/master/.github/workflows/Build_OP_x86_64.yml)

   - 测试与建议: [CurssedCoffin](https://github.com/CurssedCoffin) [Licsber](https://github.com/Licsber) [sirliu](https://github.com/sirliu) [teasiu](https://github.com/teasiu)