---
title: "工厂方法模式（Factory Method）"
description: "单例模式能够确保一个类在整个应用的生命周期内只实例化一次"
published: 2026-01-23
pubDate: 2026-01-23
date: 2025-08-13
draft: false
tags: ["设计模式", "Python"]
category: "设计模式"
author: "0x3a0"
sourceLink: "https://github.com/0x3a0/blog/src/posts/工厂方法模式（Factory Method）.md"
---

# Factory Method Pattern
工厂方法模式的核心是**委托**。客户端代码不再直接创建对象，而是将对象的创建委托给工厂方法，这种委托机制促进了客户端代码与其内部所使用的对象之间的松耦合，从而增强了灵活性和可维护性。

## 工厂方法模式的关键组件
工厂方法模式可以分为一下基本组成部分

1. **Creator(创造者)**：是一个抽象类或接口，声明了工厂方法，本质是一种创建对象的方法，提供了一个用于创建**Product(产品)** 的接口，但不指定具体的产品类
2. **Concrete Creator(具体创造者)**：Concrete Creator是Creator的子类，负责实现工厂方法，每个具体创造者专门创造特定类型的单一产品的具体类(Concrete Product)
3. **Product(产品)**：是一个抽象类或接口，定义工厂方法创建的对象类型，规定产品共有的行为
4. **Concrete Product(具体产品)**：Concrete Product是Product的子类，提供产品的具体实现，每个具体产品都对应一种由工厂方法创建的对象

## 在Python中实现工厂方法
以下是使用工厂方法模式实现不同语言的翻译案例

### Step 1：定义Product
首先定义Product，它代表翻译的抽象类，规定所有翻译类都有一个共有的接口localize

```python
from abc import ABC, abstructmethod

# Step 1: Defining Product
class Localizer(ABC):
    """Abstruct Product: """
    @abstructmethod
    def localize(self, msg):
        psss
```

### Step 2：创建Concrete Product
Concrete Product负责提供每个翻译产品的具体实现，我们将创建三个具体的翻译产品，分别对应英语、法语、西班牙语。

```python
# Step 2: Creating Concrete Products
class EnglishLocalizer(Localizer):
    def __init__(self):
        self.translations = {
            "汽车": "car",
            "自行车": "bike",
            "摩托车": "crycle"
        }

    def localize(self, msg):
        return self.translations.get(msg)

class FrenchLocalizer(Localizer):
    def __init__(self):
        self.translations = {
            "汽车": "voiture",
            "自行车": "bicyclette",
            "摩托车": "cyclette"
        }

    def localize(self, msg):
        return self.translations.get(msg)

class SpanishLocalizer(Localizer):
    def __init__(self):
        self.translations = {
            "汽车": "coche",
            "自行车": "bicicleta",
            "摩托车": "ciclo"
        }

    def localize(self, msg):
        return self.translations.get(msg)
```

### Step 3：定义Creator
现在我们开始定义Creator抽象类，该类声明了工厂方法，是一种创建对象的方法，提供一个用于创建具体产品的接口create_localizer

```python
# Step 3: Defining Creator
class LocalizerFactory(ABC):
    @abstructmethod
    def create_localizer(self):
        pass
```

### Step 4：实现Concrete Creator
Concrete Creator负责实现工厂方法，每个具体创造者专门创造特定语言翻译的具体产品

```python
class EnglishLocalizerFactory(LocalizerFactory):
    def create_localizer(self):
        return EnglishLocalizer()

class FrenchLocalizerFactory(LocalizerFactory):
    def create_localizer(self):
        return FrenchLocalizer()

class SpanishLocalizerFactory(LocalizerFactory):
    def create_localizer(self):
        return SpanishLocalizer()
```

### Step 5：在客户端代码中利用Factory
最后，我们在客户端代码中利用工厂方法模式来创建翻译实例

```python
# Step 5：Utilizing Factory
if __name__ == "__main__":
    english_translate = EnglishLocalizerFactory()
    french_translate = FrenchLocalizerFactory()
    spanish_translate = SpanishLocalizerFactory()

    messages = ["汽车", "自行车", "摩托车"]
    for msg in messages:
        print(english_translate.create_localizer().localize(msg))
        print(french_translate.create_localizer().localize(msg))
        print(spanish_translate.create_localizer().localize(msg))
```

通过使用Factory Method的方法去实现不同语言的翻译，能够让我们避免在业务代码中写一堆 if to_language = "english"来判断需要翻译成什么语言

## 根据Python的灵活性对Factory Method方法进行重构
Python的灵活性能够让我们以更简洁、更清晰的方法实现Step 3和Step 4，如下所示：

```python
def create_localizer(language="English"):
    """
    Factory Method

    Args:
        language (str): 

    Returns:
        Localizer: 
    """
    localizers = {
        "English": EnglishLocalizer,
        "French": FrenchLocalizer,
        "Spanish": SpanishLocalizer,
    }
    return localizers[languge]()

if __name__ == "__main__":
    english_translate = create_localizer("English")
    french_translate = create_localizer("French")
    spanish_translate = create_localizer("Spanish")

    messages = ["汽车", "自行车", "摩托车"]
    for msg in messages:
        print(english_localizer.localize(msg))
        print(french_localizer.localize(msg))    
        print(spanish_localizer.localize(msg))
```

## 整体代码
```python
from abc import ABC, abstructmethod

# Step 1: Defining Product
class Localizer(ABC):
    """Abstruct Product: """
    @abstructmethod
    def localize(self, msg):
        psss
        
# Step 2: Creating Concrete Products
class EnglishLocalizer(Localizer):
    def __init__(self):
        self.translations = {
            "汽车": "car",
            "自行车": "bike",
            "摩托车": "crycle"
        }

    def localize(self, msg):
        return self.translations.get(msg)

class FrenchLocalizer(Localizer):
    def __init__(self):
        self.translations = {
            "汽车": "voiture",
            "自行车": "bicyclette",
            "摩托车": "cyclette"
        }

    def localize(self, msg):
        return self.translations.get(msg)

class SpanishLocalizer(Localizer):
    def __init__(self):
        self.translations = {
            "汽车": "coche",
            "自行车": "bicicleta",
            "摩托车": "ciclo"
        }

    def localize(self, msg):
        return self.translations.get(msg)

class ConcreteFactor():
    def create_localizer(language="English"):
        """
        Factory Method
    
        Args:
            language (str): 
    
        Returns:
            Localizer: 
        """
        localizers = {
            "English": EnglishLocalizer,
            "French": FrenchLocalizer,
            "Spanish": SpanishLocalizer,
        }
        return localizers[languge]()

if __name__ == "__main__":
    concrete_factor = ConcreteFactor()
    english_translate = concrete_factor.create_localizer("English")
    french_translate = concrete_factor.create_localizer("French")
    spanish_translate = concrete_factor.create_localizer("Spanish")

    messages = ["汽车", "自行车", "摩托车"]
    for msg in messages:
        print(english_localizer.localize(msg))
        print(french_localizer.localize(msg))    
        print(spanish_localizer.localize(msg))
```

## 工厂方法模式的优势
1. **可拓展性**：将来会新增很多具体产品类时，无需修改现有代码即可添加新的产品类。
2. **解耦**：将具体类的实现和客户端代码解耦，减少依赖性并增加代码稳定性

## 工厂方法模式的缺点
1. **复杂性**：引入多个工厂方法和相关类可能会增加复杂性
2. **抽象开销**：创建大量的抽象类和接口，增加了代码库的开销
3. **过度设计(过早抽象)**：在简单的情况下，使用工厂方法模式会显得繁琐且不必要，增加复杂度而无明显好处

## 工厂方法模式在实际用的案例
1. **库框架**：工厂方法模式通常用于库框架，允许开发人员扩展和定制库的行为
2. **插件架构**：在构架具有可拓展插件架构的项目时，工厂方法模式简化了新插件的添加，而无需修改现有的代码
3. **测试**：可用于单元测试的模拟对象

