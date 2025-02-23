+++
date = '2024-12-17T21:10:09+08:00'
draft = false
title = 'Apache Gravitino用户界面'
categories = ["数据治理"]
tags = ["Next.js", "Java"]
+++

本文档主要概述了用户如何使用 Web UI 在 Apache Gravitino 中管理元数据，图形界面可通过 Web 浏览器访问，作为编写代码或使用 REST 接口的替代方案。
目前，您可以集成 [OAuth 设置](https://gravitino.apache.org/docs/0.8.0-incubating/security/security)来查看、添加、修改和删除 Metalake，创建目录，以及查看目录、Schema 和 Table 等功能。
[构建](https://gravitino.apache.org/docs/0.8.0-incubating/how-to-build#quick-start)和[部署](https://gravitino.apache.org/docs/0.8.0-incubating/getting-started#getting-started-locally) Gravitino Web UI，并在浏览器中 `http://<gravitino-host>:<gravitino-port>` 打开它，默认情况下为 [http://localhost:8090](http://localhost:8090/)。
## 初始页面
Gravitino 中显示的 Web UI 主页取决于 OAuth 模式的配置参数，请参阅[安全性](https://gravitino.apache.org/docs/0.8.0-incubating/security/security)中的详细信息。
为 `gravitino.authenticators`、[`simple`](https://gravitino.apache.org/docs/0.8.0-incubating/webui#simple-mode) 或 [`oauth`](https://gravitino.apache.org/docs/0.8.0-incubating/webui#oauth-mode) 设置参数。简单模式是默认的身份验证选项。如果设置了多个验证器，则默认使用第一个验证器。
### 简单模式
```
gravitino.authenticators = simple
```
将配置参数 `gravitino.authenticators` 设置为 `simple，Web` UI 将显示主页 （Metalakes）。
![](image1.png)
在右上角，UI 显示当前的 Gravitino 版本。
主要内容显示已有的 Metalake 列表。
### Oauth 模式
```
gravitino.authenticators = oauth
```
将配置参数 `gravitino.authenticators` 设置为 `oauth，Web` UI 将显示登录页面。
![](image2.png)
1. 输入与您的特定配置对应的值。有关详细说明，请参阅[安全性](https://gravitino.apache.org/docs/0.8.0-incubating/security/security)。
2. 单击`“登录`”按钮将带您进入主页。
![](image3.png)
在右上角，有一个图标按钮，单击该按钮后会将您带到登录页面。
## 管理元数据
### 元湖
#### 创建元湖
在主页上，单击 `CREATE METALAKE` 按钮会显示一个用于创建 metalake 的对话框。
![](image4.png)
创建 Metalake 需要以下字段：
1. **Name**（**_必选_**）：Metalake 的名称。
2. **Comment**（_可选_）：Metalake 的 Comment。
3. **属性**（_可选_）：单击 `ADD PROPERTY` 按钮以添加自定义属性。
![](image5.png)
您可以对 Metalake 执行 3 项作。
![](image6.png)
#### 显示 metalake 详细信息
单击表单元格中的作图标。
你可以在右侧的 drawer 组件中看到这个 metalake 的详细信息。
![](image7.png)
#### 编辑 metalake
单击表单元格中的作图标。
显示用于修改所选 metalake 的字段的对话框。
![](image8.png)
#### 禁用 metalake
Metalake 创建成功后默认为 in-use。
将鼠标悬停在 metalake 名称旁边的开关上，可以看到“正在使用”提示。
![](image9.png)
单击开关将禁用 metalake，将鼠标悬停在 metalake 名称旁边的开关上可以看到“未使用”提示。
![](image10.png)
#### 丢弃 Metalake
单击表单元格中的作图标。
显示确认对话框，单击 `DROP` 按钮将删除此 metalake。
![](image11.png)
### 目录
单击 metalake 中的 views catalogs 表中的 metalake 名称。
如果这是第一次，则在创建目录之前不会显示任何数据。
单击向左箭头图标按钮将带您进入 metalake 页面。
![](image12.png)
单击 Tab - `DETAILS` 可在 metalake catalogs 页面上查看 metalake 的详细信息。
![](image13.png)
页面左侧是一个树列表，目录的图标对应于它们的类型和提供程序。
- Catalog 
- Schema  
- Table 
![](image14.png)
将鼠标悬停在相应的图标上，数据将更改为 reload 图标 。单击此图标可重新加载当前选定的数据。
![[](image15.png)
#### 创建目录
单击 `CREATE CATALOG` 按钮将显示用于创建目录的对话框。
![](image16.png)
### 模式
单击左侧边栏上的目录树节点或表单元格中的目录名称链接。
显示目录的列表模式。
![](image17.png)
### 表
单击左侧边栏上的 hive 模式树节点或表单元格中的模式名称链接。
显示模式中表的明细。
![](image18.png)
### 文件集
单击左侧边栏上的文件集-模式节点或表单元格中的模式名称链接。
显示模式的文件集列表。
![](image19.png)
### 主题
单击左侧边栏上的 kafka 模式树节点或表单元格中的模式名称链接。
显示模式的主题明细。
![](image20.png)
### 模型
单击左侧边栏上的模型模式树节点或表单元格中的模式名称链接。
显示模式的模型明细。
![](image21.png)
### 版本
单击左侧边栏上的模型树节点或表单元格中的模型名称链接。
显示模型的列表版本。
![](image22.png)