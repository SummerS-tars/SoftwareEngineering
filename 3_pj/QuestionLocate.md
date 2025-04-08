# 软件工程荣誉课前沿探索进度汇报：问题定位

---

- [1. 项目背景](#1-项目背景)
    - [1.1. 目标项目](#11-目标项目)
    - [1.2. 测试用LLM](#12-测试用llm)
    - [1.3. 测试目标](#13-测试目标)
- [2. LLM测试流程介绍](#2-llm测试流程介绍)
    - [2.1. 目标用例](#21-目标用例)
    - [2.2. 测试方式](#22-测试方式)
    - [2.3. 返回结果定义](#23-返回结果定义)
    - [2.4. 第二种方式使用prompt](#24-第二种方式使用prompt)
- [3. 测试结果列举](#3-测试结果列举)
    - [3.1. 单文件更改定位测试](#31-单文件更改定位测试)
    - [多文件更改定位测试](#多文件更改定位测试)
    - [新功能更改定位测试](#新功能更改定位测试)

---

## 1. 项目背景

### 1.1. 目标项目

[AutoDrive](https://github.com/tangjiewei0336/autodrive)  

### 1.2. 测试用LLM

[GLM-4-Plus](https://open.bigmodel.cn/dev/api/normal-model/glm-4)  

### 1.3. 测试目标

基于已经存在的项目([AutoDrive](https://github.com/tangjiewei0336/autodrive))，提供不同详细程度的相关信息，根据选出的特定的git commit相关的commit message，测试LLM定位此commit相关的代码（当前具体至相关函数的及其变更方式）  

## 2. LLM测试流程介绍

### 2.1. 目标用例

**测试用例git commits:**  

- 单文件
    - 【feat: 新增停止训练的功能】  
        eb4d870893e9e6f56db7a75b4ef3b77894d69d84
    - 【feat: 更加频繁地更新用户活跃信息】  
        e6937c19a4a71f349423c72423af66d3b62b617f
    - 【fix: 导入了错误的HttpServletRequest和HttpServletResponse】  
        205f9f8f1cc8385c26bd30e63ffc2602d604fd31
    - 【feat: /train/submitOffline 接口：提交训练完毕的文件】  
        b8da36816369c7f6afc6348baf79d4b43d10de24
    - 【fix: 调整导出数据库时候的文件名为exported_data.zip】  
        473b3fcf7d2a21c026b65957d01a45e6e632dd36
    - 【feat: 增加editModel鉴权】  
        46e6333f01b9191147406de30481bcd8944e63ad

- 多个文件
    - 【feat: 修复了getDataset时的老师权限问题】  
        effafa25c65ce93ef90b8922392dfdb9c194bfac
    - 【fix: 修复了对图片进行操作时，缩略图不会更新的问题】  
        52acc656958ce42421cda544b4da988f3d4d0726
    - 【fix: 修改密码时验证旧密码】  
        c770120479533b77032f518ca09823d954adbe1e
    - 【fix： 修复不能查询role的bug】  
        e8a288ab10b30d430ff414784a12c9b07b78695e

- 新功能
    - 【更新配置：jwt认证已启用，接下来会对请求中的用户id字段进行弃用】  
        47dfb300637ce6a5763eb928984cdced999c54d6  

### 2.2. 测试方式

1. commit message + 全部代码  
2. commit message + 总结信息 多轮对话  

### 2.3. 返回结果定义

以json格式返回相关测试结果，具体格式如下  

```json
{
    "file": "the_file_name",
    "function": "the_function_name",
    "operation": "delete" or "create" or "update",
}
```

### 2.4. 第二种方式使用prompt

```txt
Prompt:
Strictly answer in following json format. Do not add any words. Do not add any comments. Do not make up parts. Do not prepend or append any words to the json.
[
    {
        "file": "the_file_name",
        "function": "the_function_name",
        "operation": "delete" or "create" or "update",
    },
    {
        ( the second or more file with same format as above if needed )
    }
]
Do not explain the reason. Do not make up your answer. Don't miss anything because of newlines and whitespaces.
I will give you a code summary of a Spring Boot project.
Your task is to analyze the code summary and then try to complement "git commit message".
You need to decide which function is needed to be created, deleted, or updated, and then reply with the format defined above.  
```

## 3. 测试结果列举

根据commit相关的文件更改的不同，对结果进行列举：  

1. 单文件更改  
2. 多文件更改  
3. 新功能更改

### 3.1. 单文件更改定位测试  

1. 【feat: 新增停止训练的功能】  
    1. Way1:  

        ```json
        [
            {
                "file": "VerifyService.java",
                "function": "stopJob",
                "operation": "update"
            },
            {
                "file": "TrainController.java",
                "function": "stopTrain",
                "operation": "create"
            }
        ]
        ```

    2. Way2:  

        attempt1:  

        ```json
        [
            {
                "file": "D:\\2025春\\软件工程\\前沿探索\\autodrive\\src\\main\\java\\com\\adas\\flask\\parameters\\StartVerify.java",
                "function": "timeout",
                "operation": "update"
            }
        ]
        ```

        attempt2:  

        ```json
        [
            {
                "file": "D:\\2025春\\软件工程\\前沿探索\\autodrive\\src\\main\\java\\com\\adas\\flask\\parameters\\StopVerify.java",
                "function": "StopVerify",
                "operation": "create"
            }
        ]
        ```

        attempt3:  

        ```json
        [
            {
                "file": "D:\\2025春\\软件工程\\前沿探索\\autodrive\\src\\main\\java\\com\\adas\\flask\\train\\TrainService.java",
                "function": "stopTrain",
                "operation": "update"
            }
        ]
        ```

2. 【fix: 调整导出数据库时候的文件名为exported_data.zip】  
    1. Way1:  

        ```json
        [
            {
                "file": "TransferController.java",
                "function": "exportAllDbAndFs",
                "operation": "update"
            },
            {
                "file": "TransferController.java",
                "function": "exportAllDbAndFs",
                "operation": "update"
            }
        ]
        ```

    2. Way2:  

        attempt1:  

        ```json
        [
            {
                "file": "D:\\2025春\\软件工程\\前沿探索\\autodrive\\src\\main\\java\\com\\adas\\controller\\TransferController.java",
                "function": "exportAllDbAndFs",
                "operation": "update"
            }
        ]
        ```

        attempt2:  

        ```json
        [
            {
                "file": "D:\\2025春\\软件工程\\前沿探索\\autodrive\\src\\main\\java\\com\\adas\\controller\\TransferController.java",
                "function": "exportAllDbAndFs",
                "operation": "update"
            }
        ]
        ```

        attempt3:  

        ```json
        [
            {
                "file": "D:\\2025春\\软件工程\\前沿探索\\autodrive\\src\\main\\java\\com\\adas\\controller\\DatasetController.java",
                "function": "exportDataset",
                "operation": "update"
            }
        ]
        ```

### 多文件更改定位测试

【fix: 修复了对图片进行操作时，缩略图不会更新的问题】  

1. Way1:  

    ```json
    [
        {
            "file": "DatasetController.java",
            "function": "processNewDataset",
            "operation": "update"
        },
        {
            "file": "TagProcessingService.java",
            "function": "processJsonFiles",
            "operation": "update"
        },
        {
            "file": "TrainController.java",
            "function": "updateTrainStatus",
            "operation": "update"
        },
        {
            "file": "TrainController.java",
            "function": "queryTrainById",
            "operation": "update"
        },
        {
            "file": "DatasetController.java",
            "function": "syncCarStatus",
            "operation": "update"
        },
        {
            "file": "VerifyController.java",
            "function": "startValidation",
            "operation": "update"
        }
    ]
    ```

2. Way2:  

    attempt1:  

    ```json
    [
        {
            "file": "D:\\2025春\\软件工程\\前沿探索\\autodrive\\src\\main\\java\\com\\adas\\controller\\DatasetController.java",
            "function": "processNewDataset",
            "operation": "update"
        }
    ]
    ```

    attempt2:  

    ```json
    [
        {
            "file": "D:\\2025春\\软件工程\\前沿探索\\autodrive\\src\\main\\java\\com\\adas\\entity\\File.java",
            "function": "update",
            "operation": "update"
        }
    ]
    ```

    attempt3:  

    ```json
    [
        {
            "file": "D:\\2025春\\软件工程\\前沿探索\\autodrive\\src\\main\\java\\com\\adas\\flask\\parameters\\PicOps.java",
            "function": "PicOps",
            "operation": "update"
        }
    ]
    ```

### 新功能更改定位测试

【更新配置：jwt认证已启用，接下来会对请求中的用户id字段进行弃用】  

1. Way1:  

    ```json
    [
        {
            "file": "TrainController.java",
            "function": "createNewTrain",
            "operation": "update"
        },
        {
            "file": "VerifyController.java",
            "function": "startValidation",
            "operation": "update"
        }
    ]
    ```

2. Way2:  

    attempt1:  

    ```json
    [
        {
            "file": "D:\\2025春\\软件工程\\前沿探索\\autodrive\\src\\main\\java\\com\\adas\\configuration\\CorsConfig.java",
            "function": "corsConfig",
            "operation": "update"
        },
        {
            "file": "D:\\2025春\\软件工程\\前沿探索\\autodrive\\src\\main\\java\\com\\adas\\configuration\\CorsConfig.java",
            "function": "corsFilter",
            "operation": "update"
        }
    ]
    ```

    attempt2:  

    ```json
    [
        {
            "file": "D:\\2025春\\软件工程\\前沿探索\\autodrive\\src\\main\\java\\com\\adas\\entity\\User.java",
            "function": "userId",
            "operation": "update"
        },
        {
            "file": "D:\\2025春\\软件工程\\前沿探索\\autodrive\\src\\main\\java\\com\\adas\\entity\\CarStatus.java",
            "function": "userId",
            "operation": "delete"
        }
    ]
    ```

    attempt3:  

    ```json
    [
        {
            "file": "D:\\2025春\\软件工程\\前沿探索\\autodrive\\src\\main\\java\\com\\adas\\entity\\Config.java",
            "function": "updateConfig",
            "operation": "update"
        }
    ]
    ```
