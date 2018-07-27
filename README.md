# Ali IoTKit

## 1. 介绍

**ali-iotkit** 是 RT-Thread 移植的用于连接阿里云 IoT 平台的软件包。基础 SDK 是阿里提供的 [**iotkit-embedded C-SDK**](https://github.com/aliyun/iotkit-embedded)。

### 1.1 目录结构

| 名称            | 说明 |
| ----            | ---- |
| docs            | 文档目录 |
| iotkit-embedded | 阿里 iotkit 原文件目录 |
| ports            | 移植文件目录 |
| samples         | 示例文件目录 |

### 1.2 许可证

`ali-iotkit` 软件包延用阿里 `iotkit-embedded` 软件包许可协议，请见 `iotkit-embedded/LICENSE` 文件。

### 1.3 依赖

- [RT_Thread 3.0+](https://github.com/RT-Thread/rt-thread/releases/tag/v3.0.4)
- [MbedTLS 软件包](https://github.com/RT-Thread-packages/mbedtls)

## 2. 如何使用

### 2.1 启用软件包

- 使用 `menuconfig` 使能 iotkit 软件包并填写设备信息

```shell
RT-Thread online packages  --->
    IoT - internet of things  --->
        IoT Cloud  --->
          [*] Ali-iotkit:  Ali Cloud SDK for IoT platform  --->
                Select Aliyun platform (LinkDevelop Platform)  --->
          (a1dSQSGZ77X) Config Product Key
          (RGB-LED-DEV-1) Config Device Name
          (Ghuiyd9nmGowdZzjPqFtxhm3WUHEbIlI) Config Device Secret
          -*-   Enable MQTT
          [*]     Enable MQTT sample
          [*]     Enable MQTT direct connect
          [*]     Enable SSL
          [ ]   Enable COAP
          [*]   Enable OTA
                      Select OTA channel (Use MQTT OTA channel)  --->
                    version (latest)  --->
```

- 增加 `mbedTLS` 帧大小（OTA 的时候**至少需要 8K 大小**）

```shell
RT-Thread online packages  --->
    security packages  --->
      -*- mbedtls:An open source, portable, easy to use, readable and flexible SSL library  --->
      (8192) Maxium fragment length in bytes
```

- 使用 `pkgs --update` 命令下载软件包

### 2.2 执行示例

> 在 MSH 中使用命令执行预置的示例程序

- MQTT Sample 单次发布订阅

该示例程序演示了如何使用 MQTT 发布、订阅 Topic，MSH 命令如下所示：

```shell
msh />ali_mqtt_test
ali_mqtt_test|502 :: iotkit-embedded sdk version: V2.10
[inf] iotx_device_info_init(40): device_info created successfully!
[dbg] iotx_device_info_set(50): start to set device info!
[dbg] iotx_device_info_set(64): device_info set successfully!

...

mqtt_client|324 :: out of sample!
```

- MQTT Sample 监听订阅消息

该示例程序演示了如何使用 MQTT 发布、订阅 Topic，并一直监听订阅 Topic 的消息，MSH 命令如下所示：

```shell
msh />ali_mqtt_test loop
ali_mqtt_test|502 :: iotkit-embedded sdk version: V2.10
[dbg] iotx_device_info_init(32): device_info already created, return!
[dbg] iotx_device_info_set(50): start to set device info!
[dbg] iotx_device_info_set(64): device_info set successfully!

...

[dbg] iotx_mc_cycle(1269): SUBACK
event_handle|111 :: subscribe success, packet-id=0
[dbg] iotx_mc_cycle(1269): SUBACK
event_handle|111 :: subscribe success, packet-id=0
```

- OTA Sample

该示例程序演示了如何使用阿里云 OTA 服务，使用 `ali_ota_test` 命令启动例程，这个时候，设备首先会上报当前版本号到阿里云，然后等待云端下发 OTA 升级命令。

```shell
msh />ali_ota_test
ali_ota_main|325 :: iotkit-embedded sdk version: V2.10
[dbg] iotx_device_info_init(32): device_info already created, return!
[dbg] iotx_device_info_set(50): start to set device info!
[dbg] iotx_device_info_set(64): device_info set successfully!

...

mqtt_client|232 :: wait ota upgrade command....
[dbg] iotx_mc_cycle(1260): PUBACK
event_handle|117 :: publish success, packet-id=2
[dbg] iotx_mc_cycle(1269): SUBACK
event_handle|093 :: subscribe success, packet-id=1
mqtt_client|232 :: wait ota upgrade command....
mqtt_client|232 :: wait ota upgrade command....
mqtt_client|232 :: wait ota upgrade command....
```

## 3、 参考

- 上手使用，请参考[用户指南](docs/user-guide.md)
- 详细的示例介绍，请参考[示例文档](docs/samples.md)
- API 介绍，请参考[API 说明文档](docs/api.md)
- 更多详细文档，请访问 `/docs` 目录查看

## 4、 注意事项

- 使用前请在 `menuconfig` 里配置自己的设备信息（PRODUCT_KEY、DEVICE_NAME 和 DEVICE_SECRET）
- 开启 OTA 功能必须使能加密连接（因为 OTA 升级必须使用 HTTPS 下载固件）

## 5、 常见问题

- MbedTLS 返回 0x7200 错误  
  通常是由于 MbedTLS 帧长度过小，请增加 MbedTLS 帧长度，参考第二章节。

## 6、 联系方式 & 感谢

- 维护： Murphy
- 主页： https://github.com/RT-Thread-packages/ali-iotkit