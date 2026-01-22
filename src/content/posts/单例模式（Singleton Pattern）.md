---
title: "单例模式（Singleton Pattern）"
description: "单例模式能够确保一个类在整个应用的生命周期内只实例化一次"
published: 2026-01-22
pubDate: 2026-01-22
date: 2025-08-12
draft: false
tags: ["设计模式", "Python"]
category: "设计模式"
author: "0x3a0"
sourceLink: "https://github.com/0x3a0/blog/src/posts"
---

# Singleton
**确保一个类在整个应用的生命周期内只实例化一次**，例如数据库链接或日志服务

工作原理如下：假设你创建了一个对象，但过了一段时间后又决定创建一个新的对象，你不会收到一个新的对象，而是会受到你已经创建的对象

## 在Python中实现单例模式
在Python中，单例模式实现方式与”四人帮“最初的设想不同，原始的单例类禁止常规实例化，提供了一个返回单例实例的静态类方法

在Python中，可以使用自定义的__new__方法进行实例化，以获取单例实例

### 基础类实现
```python
class Singleton:
    _instance = None    # 用于存放唯一实例

    def __new__(cls):    # cls 表示类本身，用于访问类变量
        if cls._instance is None:
            cls._instance = super().__new__(cls)    # 调用object来分配内存创建实例对象
        return cls._instance
```

在此示例中重写了__new__方法。如果该实例不存在，则创建一个新的实例，否则返回现有实例

### 元类实现
**元类（MetaClass）** 是Python中一个用来创建类的类。以下代码中SingletonMeta类负责管理实例，确保每一个类只有一个实例，__call__方法负责控制类的实例化，根据需要创建和存储实例。通过SingletonMeta元类应用于Singleton类，可以轻松的执行单例模式，使Singleton类能够更专注业务逻辑

```python
class SingletonMeta(type):    # 类本身是由type创建的
    _instances = {}

    def __call__(cls, *args, **kwargs):
        if cls not in cls._instances:
            instance = super().__call__(*args, **kwargs)
            cls._instances[cls] = instance
        return cls._instances[cls]

class Singleton(metaclass=SingletonMeta):
    pass
```

## 线程安全
单例模式不是线程安全的，在多线程环境中，可能存在多个线程同时创建实例。在多线程环境中请考虑以下用法：

```python
import threading

class ThreadSafeSingleton:
    _instance = None
    _lock = threading.Lock()

    def __new__(cls):
        with cls._lock:
            if cls._instance is None:
                cls._instance = super().__new__(cls)
            return cls._instance
```

这里使用线程锁来确保每次只能有一个线程可以创建实例，从而避免竞争条件

## 单例模式的优势
1. **单个实例**：确保该类在整个生命周期中只创建一个实例，防止多个实例引起冲突或消耗不必要的资源
2. **全局访问**：提供该类实例的全局访问点，能够在不同的模块之间共享数据或功能
3. **资源管理**：更好地管理共享资源，如数据库连接，无需创建多个连接使系统不堪重负
4. **延迟初始化**：仅在需要时创建实例

## 单例模式的缺点
1. **违反单一职责**：该模式同时解决两个问题，资源控制：限制实例的数量，访问控制：提供全局访问点（[单一职责原则（SRP）](https://en.wikipedia.org/wiki/Single-responsibility_principle#cite_note-1)：一个模块应该对一个且仅对一个参与者负责）
2. **全局耦合**：全局可访问的实例可能会加剧应用程序各部分之间的依赖程度，从而使维护和测试变得复杂
3. **测试困难**：
4. **多线程复杂性**：在多线程的环境中需要添加线程锁来避免多个线程同时创建单例

## 单例模式在实际应用中的案例
1. **数据库连接**：统一管理数据库的创建和连接，提高数据库交互效率
2. **日志服务**：在一个单独的日志记录器实例集中管理整个应用的日志
3. **配置管理**：只有一个配置管理器实例负责整个应用程序的设置管理
4. **硬件访问**：通过单个实例控制对硬件资源的访问（如打印机和传感器）

