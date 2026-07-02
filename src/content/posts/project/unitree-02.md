---
title: Unitree 机器狗（二）：ROS导航
published: 2026-03-19
updated: 2026-07-02
description: 宇树机器狗 Go2-Air相关的二次开发
image: /assets/bolg_cover/unitree-02.webp
tags: [机器狗, ROS, 自动导航]
category: 项目
draft: false
author: larry
password: ""
passwordHint: ""
---

---

# 前言

**[Unitree 机器狗（一）：运动控制](/posts/project/unitree-01/)**

>本文档用于记录摩方M6s N100平台下，宇树机器狗Go2 Air版本相关的ROS控制开发、功能测试、联调调试等相关工作，作为项目过程追溯、参数参考、问题排查的基础依据，后续内容可根据实际项目需求补充完善。

- **基础环境**
  
  | **主机设备**   | 摩方M6s N100                                                 |
  | -------------- | ------------------------------------------------------------ |
  | **操作系统**   | Ubuntu 20.04 LTS                                             |
  | **传感器设备** | 思岚 A1M8                                                    |
  | **ROS版本**    | ROS Noetic Ninjemys                                          |
  | **补充说明**   | 本环境为项目/测试的基础运行环境，后续所有操作均基于此环境开展，若环境有变更，需在文档中同步更新。 |
  
- **参考链接**
  
  | **[GitHub - abizovnuralem/go2_ros2_sdk](https://github.com/abizovnuralem/go2_ros2_sdk)** |
  | ------------------------------------------------------------ |
  | **[GitHub - legion1581/unitree_webrtc_connect](https://github.com/legion1581/unitree_webrtc_connect)** |
  | **[GitHub - phospho-app/go2_webrtc_connect](https://github.com/phospho-app/go2_webrtc_connect)** |
  | **[宇树科技 文档中心](https://support.unitree.com/home/zh/developer/Obtain%20SDK)** |
  | **[timer/GO2GO](https://gitee.com/simon_133/go2-go)**        |
  | **[GitHub - legion1581/go2_firmware_tools](https://github.com/legion1581/go2_firmware_tools)** |
  | **[unitree-go2-slam-nav2](https://relatedrepos.com/gh/h-naderi/unitree-go2-slam-nav2)** |
  | **[DarrenPig (darrenpig) - Gitee.com](https://gitee.com/darrenpig)** |

本文件将在上述基础环境的框架下，详细记录项目开发、测试验证等相关工作的进展。各具体模块可根据实际需求灵活规划与补充。鉴于篇幅及内容聚焦考量，有关系统安装及部分常用配置的具体细节将不做赘述。
