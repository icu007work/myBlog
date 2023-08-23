---
title: Github Action实现Android自动打包并发布apk
author: Rookie_l
categories: 技术
cover: 'https://pic.ziyuan.wang/2023/08/23/bab4140e88a71.jpg'
ai:
  - >-
    这篇文章主要介绍了GitHub Action的基本概念和使用方法，以下是文章的摘要：文章首先介绍了GitHub
    Action是什么，它是GitHub推出的一款持续集成工具，用于自动化构建、测试和部署代码。接着，解释了持续集成的概念，即自动化地执行特定的指令来构建程序，例如在前端项目中自动安装依赖并打包。Yaml文件在GitHub
    Action中的作用被提及，它是用于配置GitHub
    Action的文件，通常存放在存储库的根目录下的.github文件夹中。接下来，文章介绍了GitHub
    Action的一些使用限制，包括job执行时间、API请求限制等。关于Workflow，文章解释了它是由一个或多个job组成的自动化过程，通过创建Yaml文件来配置Workflow。在具体配置Workflow时，文章介绍了如何定义Workflow的名称和触发事件，以及如何定义job、job的名称、依赖关系、运行环境、环境变量等。对于job中的步骤（Step），文章解释了Step的属性，每个job由多个Step构成，每个Step可以运行命令、进行环境配置或执行Action。最后，文章提到了GitHub
    Action的Action，即可重用的命令集合，通过uses关键字引入，可以用于各种任务，比如代码检查、构建等。文章还介绍了如何使用Action库，如何传递参数以及如何运行命令行命令。总之，该文章详细介绍了GitHub
    Action的基本概念、配置方法和使用示例，适合初学者了解和入门GitHub Action的使用。
abbrlink: 40fd71e7
date: 2023-05-12 17:35:25
tags:
keywords:
description:
---

## Github Action介绍

### 一、Github Action 是什么？

是 Github 推出的持续集成工具

### 二、持续集成是什么？

简单说就是自动化的打包程序——如果是前端程序员，这样解释比较顺畅：

每次提交代码到 Github 的仓库后，Github 都会自动创建一个虚拟机（Mac / Windows / Linux 任我们选），来执行一段或多段指令（由我们定），例如：

1. npm install
2. npm run build

### 三、Yaml 是什么？

我们集成 Github Action 的做法，就是在我们仓库的根目录下，创建一个 .github 文件夹，里面放一个 *.yaml 文件——这个 Yaml 文件就是我们配置 Github Action 所用的文件。

### 四、Github Action 的使用限制

- 每个 Workflow 中的 job 最多可以执行 6 个小时
- 每个 Workflow 最多可以执行 72 小时
- 每个 Workflow 中的 job 最多可以排队 24 小时
- 在一个存储库的所有 Action 中，一个小时最多可以执行 1000 个 API 请求
- 并发工作数：Linux：20，Mac：5（专业版可以最多提高到 180 / 50）

### 五、什么是 Workflow？

Workflow 是由一个或多个 job 组成的可配置的自动化过程。我们通过创建 YAML 文件来创建 Workflow 配置。

------

#### 5.1 如何定义 Workflow 的名字？

> name

Workflow 的名称，Github 在存储库的 Action 页面上显示 Workflow 的名称。

如果我们省略 name，则 Github 会将其设置为相对于存储库根目录的工作流文件路径。

```text
name: Greeting from Mona
on: push
```

------

#### 5.2 如何定义 Workflow 的触发器？

> on

触发 Workflow 执行的 event 名称，比如：**每当我提交代码到 Github 上的时候，或者是每当我打 TAG 的时候。**

```text
// 单个事件
on: push

// 多个事件
on: [push,pull_request]
```

### 六、Workflow 的 job 是什么？

答：一个 Workflow 由一个或多个 jobs 构成，含义是一次持续集成的运行，可以完成多个任务。

#### 6.1 如何定义一个 job？

```text
jobs:
  my_first_job:
    name: My first job
  my_second_job:
    name: My second job
```

答：通过 job 的 id 定义。

每个 job 必须具有一个 id 与之关联。

上面的 my_first_job 和 my_second_job 就是 job_id。

#### 6.2 如何定义 job 的名称？

> jobs.<job_id>.name

name 会显示在 Github 上

#### 6.3 如何定义 job 的依赖？job 是否可以依赖于别的 job 的输出结果？

> jobs.<job_id>.needs

答：needs 可以标识 job 是否依赖于别的 job——如果 job 失败，则会跳过所有需要该 job 的 job。

```text
jobs:
  job1:
  job2:
    needs: job1
  job3:
    needs: [job1, job2]
```

> jobs.<jobs_id>.outputs：用于和 need 打配合，outputs 输出=》need 输入

jobs 的输出，用于和 needs 打配合：可以看到 ouput

```text
jobs:
  job1:
    runs-on: ubuntu-latest
    # Map a step output to a job output
    outputs:
      output1: ${{ steps.step1.outputs.test }}
      output2: ${{ steps.step2.outputs.test }}
    steps:
    - id: step1
      run: echo "::set-output name=test::hello"
    - id: step2
      run: echo "::set-output name=test::world"
  job2:
    runs-on: ubuntu-latest
    needs: job1
    steps:
    - run: echo ${{needs.job1.outputs.output1}} ${{needs.job1.outputs.output2}}
```

#### 6.4 如何定义 job 的运行环境？

> jobs.<job_id>.runs-on

指定运行 job 的运行环境，Github 上可用的运行器为：

- windows-2019
- ubuntu-20.04
- ubuntu-18.04
- ubuntu-16.04
- macos-10.15

```text
jobs:
  job1:
    runs-on: macos-10.15
  job2:
    runs-on: windows-2019
```

#### 6.5 如何给 job 定义环境变量？

> jobs.<jobs_id>.env

```text
jobs:
  job1:
    env:
      FIRST_NAME: Mona
```

#### 6.6 如何使用 job 的条件控制语句？

> jobs.<job_id>.if

我们可以使用 if 条件语句来组织 job 运行

------

### 七、Step 属性是什么？

答：每个 job 由多个 step 构成，它会从上至下依次执行。

#### 7.1 step 运行的是什么？

**step 可以运行：**

1. **command**s：命令行命令
2. **setup tasks**：环境配置命令（比如安装个 Node 环境、安装个 Python 环境）
3. **action**（in your repository, in public repository, in Docker registry）：一段 action（Action 是什么我们后面再说）

**每个 step 都在自己的运行器环境中运行，并且可以访问工作空间和文件系统。**

因为每个 step 都在运行器环境中独立运行，所以 **step 之间不会保留对环境变量的更改**。

```text
# 定义 Workflow 的名字
name: Greeting from Mona

# 定义 Workflow 的触发器
on: push

# 定义 Workflow 的 job
jobs:
  # 定义 job 的 id
  my-job:
    # 定义 job 的 name
    name: My Job
    # 定义 job 的运行环境
    runs-on: ubuntu-latest
    # 定义 job 的运行步骤
    steps:
    # 定义 step 的名称
    - name: Print a greeting
      # 定义 step 的环境变量
      env:
        MY_VAR: Hi there! My name is
        FIRST_NAME: Mona
        MIDDLE_NAME: The
        LAST_NAME: Octocat
      # 运行指令：输出环境变量
      run: |
        echo $MY_VAR $FIRST_NAME $MIDDLE_NAME $LAST_NAME.
```

------

### 八、Action 是什么？

我们可以直接打开下面的 Action 市场来看看：

[https://github.com/marketplace?type=actionsgithub.com/marketplace?type=actions](https://link.zhihu.com/?target=https%3A//github.com/marketplace%3Ftype%3Dactions)

Action 其实就是命令，比如 Github 官方给了我们一些默认的命令：

[GitHub Marketplace: actions to improve your workflowgithub.com/marketplace?type=actions&query=actions![img](https://pic4.zhimg.com/v2-d06dfc50fef28a5c76fa9034fe797faf_180x120.jpg)](https://link.zhihu.com/?target=https%3A//github.com/marketplace%3Ftype%3Dactions%26query%3Dactions)

![img](https://pic2.zhimg.com/80/v2-5208c22215435edfa36040d6b217c325_720w.webp)

比如最常用的，check-out 代码到 Workflow 工作区：

[https://github.com/marketplace/actions/checkoutgithub.com/marketplace/actions/checkout](https://link.zhihu.com/?target=https%3A//github.com/marketplace/actions/checkout)

#### 8.1 我们应该如何使用 Action？

> jobs.<job_id>.steps.uses

![img](https://pic3.zhimg.com/80/v2-e6855fd2e091401590baf4febf356cce_720w.webp)

Action 库：Checkout

比如我们可以 check-out 仓库中最新的代码到 Workflow 的工作区：

```text
steps:
  - uses: actions/checkout@v2
```

当然，我们还可以给它添加个名字：

```text
steps:
  - name: Check out Git repository
    uses: actions/checkout@v2
```

![img](https://pic2.zhimg.com/80/v2-8ea878b5f2917be32ba9aa18b3a1cef9_720w.webp)

Action 库：Setup Node

再比如说，我们如果是 node 项目，我们可以安装 Node.js 与 NPM：

```text
steps:
- uses: actions/checkout@v2
- uses: actions/setup-node@v2-beta
  with:
    node-version: '12'
```

#### 8.2 上面我们为什么要用：@v2 和 @v2-beta 呢？

答：首先，正如大家所想，这个 @v2 和 @v2-beta 的意思都是 Action 的版本。

我们如果不带版本号的话，其实就是默认使用最新版本的了。

但是 **Github 官方强烈要求我们带上版本号**——这样子的话，我们就不会出现：**写好一个 Workflow，但是由于某个 Action 的作者一更新，我们的 Workflow 就崩了的问题。**



#### **8.3 上面的 with 参数是什么意思？**

答：**有的 Action 可能会需要我们传入一些特定的值**：比如上面的 node 版本啊之类的，这些**需要我们传入的参数由 with 关键字来引入**。

**具体的 Action 需要传入哪些参数，还请去 Github Action Market 中 Action 的页面中查看。**



具体库的使用和参数，我们可以去官方的 Action 市场查看：

[Github Action 市场github.com/marketplace/actions/](https://link.zhihu.com/?target=https%3A//github.com/marketplace/actions/)

------

### 九、我们如何运行命令行命令？

```text
jobs.<job_id>.steps.run
```

上文说到，**steps 可以运行：action 和 command-line programs**。

我们现在已经知道**可以使用 uses 来运行 action 了**，那么我们该如何运行 command-line programs 呢？

**答案是：run**

**run 命令在默认状态下会启动一个没有登录的 shell 来作为命令输入器。**

#### **9.1 如何运行多行命令？**

**每个 run 命令都会启动一个新的 shell，所以我们执行多行连续命令的时候需要写在同一个 run 下：**

- **单行命令**

```text
- name: Install Dependencies
  run: npm install
```

- **多行命令**

```text
- name: Clean install dependencies and build
  run： |
    npm ci
    npm run build
```



#### 9.2 如何指定 command 运行的位置？

使用 working-directory 关键字，我们可以指定 command 的运行位置：

```text
- name: Clean temp directory
  run: rm -rf *
  working-directory: ./temp
```



#### 9.3 如何指定 shell 的类型？（使用 cmd or powershell or python？？）

使用 shell 关键字，来指定特定的 shell：

```text
steps:
  - name: Display the path
    run: echo $PATH
    shell: bash
```

下面是各个系统支持的 shell 类型：

![img](https://pic2.zhimg.com/80/v2-9283bfe170157cf550cbc86c36fa28c5_720w.webp)

------

### 十、什么是矩阵？

答：就是有时候，我们的代码可能编译环境有多个。比如 electron 的程序，我们需要在 macos 上编译 dmg 压缩包，在 windows 上编译 exe 可执行文件。

**这种时候，我们使用矩阵就可以啦~**

比如下面的代码，我们使用了矩阵指定了：**2 个操作系统，3 个 node 版本**。

这时候**下面这段代码就会执行 6 次**—— 2 x 3 = 6！！！

```text
runs-on: ${{ matrix.os }}
strategy:
  matrix:
    os: [ubuntu-16.04, ubuntu-18.04]
    node: [6, 8, 10]
steps:
  - uses: actions/setup-node@v1
    with:
      node-version: ${{ matrix.node }}
```

## 如何用GitHub Action自动编译并且发布软件到 Release

### 一、配置连接私有秘钥仓库的token

1. 首先点击 `Settings`

![image.png](http://p1.meituan.net/csc/fe431f6c5e071351005b8f960891b1a539243.png)

2. 然后选择`Developer settings`

![image.png](http://p0.meituan.net/csc/43fff60e59380a738053cdd60ed0e1f7279525.png)

3. 依次选择`Tokens` -> `generate new token`生成新的token后复制并保存下来。

![image.png](http://p1.meituan.net/csc/af9948599cade116963039b4f6897988176494.png)

4. 在仓库的设置中依次选择 `Settings` -> `Secrets and variables` -> `Action` -> `new repository secret`新建一个名为TOKEN的密钥，并将第三步复制的token粘贴进去。

![image.png](http://p0.meituan.net/csc/f381faeac8abed51ad36adee738ef4d7336383.png)

### 二、配置 GitHub Action

1. 依次点击 `Action` -> `Android CI` -> `Configure`

![image.png](http://p0.meituan.net/csc/f2d6e561835f7aa369fde4574259892f374070.png)

2. 配置 `android.yml`

![image.png](http://p0.meituan.net/csc/c210d15c2d5069928b0cd2072e8366d3274932.png)

- `android.yml`配置如下

```yaml
name: Build and Release

on:
 push:
   branches:
     - master
     - "feature/*"
   tags:
     - "v*.*.*"
 pull_request:
   branches:
     - master
     - "feature/*"

jobs:
 apk:
   name: Generate APK
   runs-on: ubuntu-latest
   steps:
     - name: Checkout
       uses: actions/checkout@v2.4.0
     - name: Branch name
       run: echo running on branch ${GITHUB_REF##*/}
     - name: Setup JDK
       uses: actions/setup-java@v2.5.0
       with:
         distribution: temurin
         java-version: "11"
     - name: Set execution flag for gradlew
       run: chmod +x gradlew
     - name: Build APK
       run: bash ./gradlew assembleDebug --stacktrace
     - name: Upload APK
       uses: actions/upload-artifact@v1
       with:
         name: apk
         path: app/build/outputs/apk/debug/app-debug.apk

 release:
   name: Release APK
   needs: apk
   runs-on: ubuntu-latest
   steps:
     - name: Get branch name
       id: branch-name
       uses: tj-actions/branch-names@v5.1
     - name: Print branch    
       run: |
         echo "Running on default: ${{ steps.branch-name.outputs.current_branch }}"
       
     - name: Download APK from build
       uses: actions/download-artifact@v1
       with:
         name: apk
     - name: Create Release
       id: create_release
       uses: actions/create-release@v1
       env:
         GITHUB_TOKEN: ${{ secrets.TOKEN }}
       with:
         tag_name: ${{ github.run_number }}
         release_name: ${{ github.event.repository.name }}  ${{ steps.branch-name.outputs.current_branch }} v${{ github.run_number }}.${{ github.run_attempt }}
     - name: Upload Release APK
       id: upload_release_asset
       uses: actions/upload-release-asset@v1.0.1
       env:
         GITHUB_TOKEN: ${{ secrets.TOKEN }}
       with:
         upload_url: ${{ steps.create_release.outputs.upload_url }}
         asset_path: apk/app-debug.apk
         asset_name: ${{ github.event.repository.name }}  ${{ steps.branch-name.outputs.current_branch }} v${{ github.run_number }}.${{ github.run_attempt }}.apk
         asset_content_type: application/zip
```

3. 手动执行一次`Action` 依次点击 `Action` -> `Build and Release` -> `Update android.yml`

![image.png](http://p0.meituan.net/csc/e1dbb65ea6a7f7de21d6c9d0730d19d3205339.png)

4. 如下图所示后，表明Action 已经帮我们完成了编译-发布这两个操作。

![image.png](http://p1.meituan.net/csc/3fe33a7c8b4f494fbab499b98545674f324900.png)
