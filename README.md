## 背景

伴随着敏捷开发和持续交付，软件开发项目中的业务的频繁更新迭代，导致测试任务工作剧增，在此背景下测试人员急需从繁琐重复的工作中抽离出来。最初的目的就是解放人力，提高测试效率。
相较UI，接口一旦研发完成，通常变更或重构的频率和幅度相对较小，且UI虽然能模拟用户的真实行为，但是受外部的原因，如电脑卡顿，浏览器卡顿，网速，需求变动等，从而容易造成脚本执行失败，投入较高等问题。做接口自动化的性价比更高，通常运用于迭代版本上线前的回归测试中。
目前有开源的基于工具类的接口自动化测试工具框架，但也存在不足，如
  1. 测试工具适用于单一临时的API场景测试，对于复杂场景测试数据驱动配置繁琐，不易操作。
  2. 开源的接口测试工具无法与CI/CD工具结合，做到二次开发，扩展功能难以实现。

## 技术选型

基于代码类的技术选型：**Python+Requests+Pytest+YAML+Allure** 
选型理由：整个选型基于python，其内的大部分框架均为python的相关插件，与python的融合性，兼容性较好。相较于java的厚重，python更加轻量，易于读写且兼容众多平台，新手更易学习。pytest脱胎于unittest但更优，包含很多扩展的插件，语法也更简单且易上手。

扩展技术选型：

关联报告发送 - JIRA

持续集成 - Jenkins

## 项目说明

通过Python+Requests统一发送和处理请求接口， 使用Pytest作为测试执行器， 使用Allure生成测试报告，使用YAML管理测试数据，使用Logging管理日志。




## 项目部署

首先，下载项目源码后，在根目录下找到 `requirements.txt` 文件，然后通过 pip 工具安装 requirements.txt 依赖，执行命令：

```
pip3 install -r requirements.txt
```

接着，修改 `config/setting.ini` 配置文件，在 Windows 环境下，安装相应依赖之后，在命令行窗口执行命令：

```
pytest
```

## 实现功能

- 采用统一请求封装，session自动关联
- 接口测试数据（接口测试的配置、路径、参数、调用方法等）与代码进行分离，
- 支持单接口批量多组测试数据，进行正反例参数校验
- 支持多接口业务场景的数据依赖
- 项目运行自动生成Log日志文件、自动输出报告
- 支持配置多个环境执行


## 项目结构

- service ====>> 放置基础功能和数据
  - common ====>> 公共方法：存放统一发送请求模块，日志模块，读取分析文件模块，请求断言模块
  - api_interface ====>> 关键字驱动，将每个请求封装为关键字，供测试用例调用
  - data ====>> 存放基础配置文件和测试用例所需的测试数据
- testcases ====>> 存放测试用例，包括单接口用例和业务场景用例
- log ====>> 存放运行生成的日志文件
- report ====>> 存放测试报告
- pytest.ini ====>> pytest 配置文件
- requirements.txt ====>> 相关依赖包文件
- run.py ====>> 项目运行入口（执行测试集的主程序）


## yaml编写测试用例常规格式及可用关键字

  feature: 模块名
  story: 接口名（必填）
  title: 用例标题（必填）
  request: 请求
    method: 请求方式（必填）
    url: 请求地址（必填）
    headers: 请求头
    params: (显性参数)
    data: (请求表单)
    json: 
    file: (文件上传)
  vilidate: 断言（必填）
    equals
    contains

```
# 生成allure报告需要提前配置allure环境

  官网下载安装包，配置变量环境path

  注：配置好变量环境后需重启编辑器
```

