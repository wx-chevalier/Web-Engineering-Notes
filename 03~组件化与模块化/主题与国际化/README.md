# 国际化

在谈论到产品国际化的时候，一般都会说到三个术语，全球化、国际化、本地化。在信息技术行业中这三个术语代表了产品出海的三个层面。正确认识和理解这些概念有助于我们正确思考和规划，是沟通和高效协作的基石。下面我们来理一下，当大家在说全球化、国际化、本地化的时候，到底是在说什么。首先了解这几个常见行业术语的缩写：

- 全球化 Globalization = G11n，字母 G 与 n 之间有 11 个字母，以下同理，

- 国际化 Internationalization = I18n，

- 本地化 Localization = L10n。

国际化的要求如下：一套代码、本地语言输入输出、日期时区数字货币的国际化、遵循 Unicode 标准、避免硬编码、分离资源，对位置与文本进行封装。而本地化的要求则更进一步：应用内容与多媒体本地化、多国语言的地道翻译、全局文化脱敏、本地化以及语言学测试。

# 国际化 i18n

先从国际化说起，一句话描述就是：将产品与特定的语言和区域（locale）脱钩，只需一套代码，让产品有适配任何区域的能力。一些具体的国际化技术包括：

- 在技术架构上，开发者只需为每个产品维护一套代码，而不需要为每一个国家的用户单独写一套代码，从而实现当地用户使用本地语言正确输入输出，代码也能处理不同国家的日期、时区和数字格式、汇率转换、货币的转换等，它们都依据(CLDR) 的产品国际化技术规范；
- 还包括源代码编码采用正确的编码，Unicode 标准；
- 代码避免 hard-code 问题；
- 资源分离：本地化的资源与源代码分离，比如界面文案；
- 位置与文本封装：为多语言文案扩展至少 30% 的空间等。

所以，国际化指的是技术层面的实现，是架构和开发要解决的问题。国际化是本地化的前提和基础。

# 本地化 L10n

在国际化的基础上，本地化是指专为特定区域的用户打造，适配当地语言和文化所提供的服务。比如，产品提供多国语言用户界面，让当地用户无障碍使用我们的产品，为其量身定制功能和内容等。本地化是产品国际化最后一公里。

典型的本地化相关的工作包括：

- 跨文化研究与定制；

- 应用界面文案、用户支持、多媒体内容等本地化翻译；

- 确保产品全局文化脱敏，避免不同文化背景下的用户产生情感不适或冲突；

- 产品本地化发版测试等。

本地化是“Do as the Romans do”，入乡随俗。对产品来说，国际化只需做一次，是基础技术能力的实现，但本地化要针对不同的区域各做一次，更多是文化层的裁剪和适配。

# 全球化 G11n

全球化一句话描述的话是指：商业决策者为产品走向全球市场而制定战略安排与规划，包括国际品牌战略、市场推广、支付、法务等一系列全球范围内的商务活动。

![](https://assets.ng-tech.icu/item/20230417212417.png)
