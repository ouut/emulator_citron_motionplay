# Citron Neo — DSU 协议完整手柄支持

Citron 是一个 Nintendo Switch 模拟器，基于 Yuzu 开发。本分支增加了 DSU（Cemuhook）协议完整手柄支持。

## 背景

DSU（DualShock UDP / Cemuhook）协议通过 UDP 传输控制器数据，数据包包含完整的按键、摇杆、体感信息。

上游代码已实现 DSU 数据包的完整解析（按键 + 摇杆 + 体感），但被 `enable_udp_controller` 设置项关闭，默认仅允许体感数据通过。

本分支将 `enable_udp_controller` 默认开启，使 DSU Server 作为完整的输入设备出现在控制器设置中。

## 工作方式

```
手机 DSU Server (UDP 26760)
         │
         ▼
     模拟器收包
         │
         ├── 体感 → 控制器 motion   ✅
         └── 按键 → 控制器 buttons  ✅
```

1. 控制器设置中选择 "UDP Controller" 作为输入设备
2. 填入 DSU Server IP 和端口（默认 26760）
3. 按键、摇杆、体感同时生效

## 多玩家支持

DSU 协议支持 4 个槽位（slot 0-3），同一 DSU Server 可同时服务 4 个 Player。

## 构建

See [CI Workflow](.github/workflows/) for build instructions.

## 协议参考

- [cemuhook-protocol](https://github.com/v1993/cemuhook-protocol)
