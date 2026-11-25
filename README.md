# Chopin 4.14.186 + SukiSU-Nova 云编译仓库

面向 **Redmi Note 10 Pro 5G (`chopin`, MT6891/MT6893)** 运行 **MIUI 14 `V14.0.8.0.TKPCNXM` (Android 13)** 的非 GKI **Linux 4.14.186** 内核云编译配置，通过 GitHub Actions 自动集成 **SukiSU-Nova (`builtin-new`)**。

## 技术方案要点

- **内核源码**：`ChopinKernels/android_kernel_chopin_android_T`（`T` 分支，Linux `4.14.186`，`chopin_user_defconfig`）
- **Root 分支**：`SukiSU-Nova/SukiSU-Nova`（`builtin-new` 分支，管理器包名 `com.sukisu.nova`）
- **Hook 方案**：`CONFIG_KSU_TAMPER_SYSCALL_TABLE=y` + `CONFIG_KSU_LSM_SECURITY_HOOKS=y`
  - 官方验证并推荐用于 **Linux 3.0 ~ 4.14** 非 GKI 内核，不依赖 Kprobes、无需脆弱的手动修改 C 文件；
  - 内置 `mount_hide`、`selinux_hide`、`avc_spoof`、`uts_spoof`、`kernel_umount`、`CONFIG_KSU_HOSTSREDIRECT=y`、`CONFIG_KALLSYMS_ALL=y` 以及开机 3 次音量上/下键安全模式。
- **说明**：SukiSU-Nova 的 `KPM` 仅支持 5.10+ GKI 内核（4.14 编译时自动忽略），其余全部核心 Root、模块挂载与内核级隐藏特性均原生支持。
- **版本伪装**：默认将内核版本号、主机名、编译时间与编译器字符串伪装为原厂 `V14.0.8.0.TKPCNXM`（`4.14.186-gaa24d707761c`）。

## 构建产物

每次 `push` 到 `main` 或在 **Actions → Build Chopin 4.14 SukiSU-Nova Kernel → Run workflow** 手动触发后，会自动产出并发布到 Releases / Artifacts：
- `Image.gz`：压缩内核镜像（推荐用原厂纯净 `boot.img` 解包后仅替换 kernel 重打包，最安全）
- `AnyKernel3-chopin-4.14.186-SukiSU-Nova-*.zip`：AnyKernel3 卡刷包
- `Image`、`kernel_built.config`、`SHA256SUMS.txt`
