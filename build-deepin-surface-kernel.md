# 内核编译

获取 deepin 的开源内核代码和 linux-surface 的补丁:
内核配置文件位于: linux-surface 的 config 目录下,因为选用了 deepin 对外开放的 5.18 分支, 所以这里 linux-deepin 的配置文件也选择 5.18
内核 patch 位于 linux-surface 目录的 patches 文件夹下, deepin 内核版本选取 5.18 分支
deepin 的原始 config 可以在 deepin 的仓库中获取

```
mkdir ~/deepin-surface && cd ~/deepin-surface
git clone git@github.com:linux-surface/linux-surface.git
git clone git@github.com:deepin-community/kernel.git -b hwe-5.18 kernel-hwe-5.18
```

# 安装依赖:

```
sudo apt-get install git fakeroot build-essential ncurses-dev xz-utils libssl-dev bc flex libelf-dev bison 
```

## 编译过程:
### 应用补丁

```
cd ~/deepin-surface/kernel
for i in ~/deepin-surface/linux-surface/patches/[VERSION]/*.patch; do patch -p1 < $i; done
```

### 合并内核配置

```
./scripts/kconfig/merge_config.sh <base-config> ~/deepin-surface/linux-surface/configs/surface-5.18.config
```

# 编译:
# 简单编译
make -j `getconf _NPROCESSORS_ONLN` dpkg-pkg LOCALVERSION=-linux-surface
# 修改后继续编译
make -j `getconf _NPROCESSORS_ONLN` bindeb-pkg LOCALVERSION=-linux-surface

