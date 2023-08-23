---
title: Win11鼠标右键菜单改回旧版教程
author: Rookie_l
categories: 技术
tags: 随笔
abbrlink: 71255bbb
cover: 'https://pic.ziyuan.wang/2023/08/23/1ba5b0baf3030.jpg'
ai:
  - 这篇文章介绍了如何通过修改注册表来改变 Windows操作系统中鼠标右键菜单的内容。该方法通过编辑注册表来自定义鼠标右键菜单的内容，但需要小心操作，确保不删除或修改不应该被改动的项，以免影响系统稳定性。
date: 2022-09-15 12:58:12
keywords:
description:
---

### 1. Win + R : regedit,回车打开注册表编辑器

![注册表](https://kjimg10.360buyimg.com/ott/jfs/t1/139822/19/29185/15405/631ab505E09e2eade/c389051154367b2b.png)

### 2. 修改注册表文件

1. 找到以下路径：`计算机\HKEY_CURRENT_USER\Software\Classes\CLSID\`
2. 新建一个名为：`86ca1aa0-34aa-4e8b-a509-50c905bae2a2` 的项

![新建项](https://kjimg10.360buyimg.com/ott/jfs/t1/196759/29/27835/87013/631ab582E4eff139d/9c1b2a2c653877b2.png)

3. `{86ca1aa0-34aa-4e8b-a509-50c905bae2a2}`上点击右键，在打开的菜单项中，选择新建名为`InprocServer32`的项

![xinjianxiang](https://kjimg10.360buyimg.com/ott/jfs/t1/147905/38/29644/97959/631ab5e3Ec88a96af/b9bad59fbb8816e9.png)

4. 点击`InproServer32`-->在默认名称上双击-->编辑字符串-->数值数据留空不填写-->直接点击确定保存.

![编辑](https://kjimg10.360buyimg.com/ott/jfs/t1/67854/13/22408/9371/631ab68fE7db4291f/3a46b18af4b5c391.png)

5. 重启电脑即可生效。

![生效](https://kjimg10.360buyimg.com/ott/jfs/t1/163187/26/27770/15784/631ab6bbE969e4be0/4a471c22785fb78d.png)

### 3. 还原

如果想还原为Win11鼠标右键，只需要把 `InproServer32`这一项删除。
