# AN7581 四机型独立云端编译

工作流：`.github/workflows/build-an7581-four-devices.yml`

## 设备与 Profile

| 选择项 | OpenWrt Profile | 硬件定义 |
|---|---|---|
| `040g-md` | `nokia_xg-040g-md-ubi` | Nokia XG-040G-MD UBI |
| `5585-ct` | `fiberhome_hg5585f-ct` | FiberHome HG5585F CT |
| `5585-cu` | `fiberhome_hg5585f-cu` | FiberHome HG5585F CU |
| `zn515` | `znxt_zn515xg-d` | ZNXT ZN515XG-D |

每个矩阵任务重新合并 `configs/an7581.config` 和 `configs/release.config`，然后将设备专属 rootfs profile 限定为单一机型。四个任务独立编译、独立缓存、独立上传 Artifact，并生成 SHA256 校验文件。

## 启动方式

1. 打开仓库 GitHub Actions。
2. 选择 **Build PonWrt AN7581 - Four Device Profiles**。
3. 点击 **Run workflow**。
4. `device=all` 并行编译四款；也可以只选其中一款。
5. 编译完成后，在对应 Job 的 Artifacts 下载固件与 `SHA256SUMS`。

## 注意事项

- 本工作流复用当前仓库的完整功能配置，包括 LuCI、Momo/Sing-box、IPTV、DDNS、NFQUEUE、WireGuard 内核支持、网络诊断与无线驱动等。
- HG5585F CU 使用独立 CU DTS 和内部交换 PHY 映射；CT 的 GPY211 外置 PHY 配置不会套用到 CU。
- 040G-MD、HG5585F CT/CU、ZN515 的 DTS、分区与设备校准数据不同，严禁跨机型刷写。
- 首次刷写前，务必备份本机完整原厂分区，尤其 factory、PON/LOID、MAC、Wi-Fi EEPROM/校准数据。不要把某台设备的 factory 分区恢复到另一台设备。
- GitHub Actions 成功只代表镜像完成构建，不代表已在真实硬件上验证启动、PON 注册、无线校准或运营商兼容性。
