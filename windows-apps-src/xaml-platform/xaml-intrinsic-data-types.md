---
description: Перечисляются возможности поддержки на уровне языка в XAML для среды выполнения Windows для определенных типов данных в среде CLR и в других языках программирования, таких как C++.
title: Встроенные типы данных в языке XAML
ms.assetid: D50E6127-395D-4E27-BAA2-2FE627F4B711
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 19f297e731225d28f92f63ad9359bba70f0f0b32
ms.sourcegitcommit: a20457776064c95a74804f519993f36b87df911e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/27/2019
ms.locfileid: "71340446"
---
# <a name="xaml-intrinsic-data-types"></a>Встроенные типы данных в языке XAML


XAML для среды выполнения Windows на уровне языка поддерживает несколько типов данных, которые представляют собой часто используемые примитивы в общей среде выполнения языков (CLR) и в других языках программирования, например C++.

Чаще всего встроенные типы данных в языке XAML используются в том случае, когда ресурсы определены в словаре ресурсов XAML. Здесь можно определить константы, например числа, используемые для нескольких значений. Также можно применить раскадрованную анимацию, в которой используется строка или логическое значение. В этом случае вам потребуется элемент объекта XAML, представляющий строку или логическое значение, для заполнения опорного кадра определения [**ObjectAnimationUsingKeyFrames**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.ObjectAnimationUsingKeyFrames). В шаблонах XAML по умолчанию в среде выполнения Windows используются оба эти способа.

XAML для среды выполнения Windows поддерживает на уровне языка следующие типы.

| Примитив XAML | Описание |
|-------|-------------|
| **кс:булеан**  | Для поддержки CLR соответствует [**Boolean**](https://docs.microsoft.com/dotnet/api/system.boolean). XAML при анализе значений **x:Boolean** не учитывает регистр. Обратите внимание, что x:Bool не является допустимой альтернативой. |
| **кс:стринг**   | Для поддержки CLR соответствует [**String**](https://docs.microsoft.com/dotnet/api/system.string). Кодировка для строки по умолчанию соответствует окружающей кодировке XML. |
| **кс:даубле**   | Для поддержки CLR соответствует [**Double**](https://docs.microsoft.com/dotnet/api/system.double). В дополнение к числовым значениям синтаксис текста для **x:Double** допускает токен NaN, представляющий способ сохранения Auto для поведения макета в виде значения ресурса. Маркеры обрабатываются с учетом регистра. Можно использовать экспоненциальное представление, например "1+E06" для `1,000,000`. |
| **x:Int32**    | Для поддержки CLR соответствует [**Int32**](https://docs.microsoft.com/dotnet/api/system.int32). **x:Int32** рассматривается как число со знаком, поэтому можно добавлять знак минуса ("–") для отрицательного целого. В XAML в отсутствие знака "плюс" (+) в синтаксисе текста подразумевается положительное значение со знаком. |

Эти примитивы языка XAML обычно представляют единственные случаи, где необходимо определять элемент объекта, использующий префикс **x:** в XAML. Все прочие элементы языка XAML обычно используются в форме атрибута или как расширение разметки.

**Примечание** .  по соглашению, Языковые примитивы для XAML и все остальные языковые элементы XAML отображаются с префиксом "x:". Так элементы языка XAML обычно используются на практике для создания разметки. Это соглашение соблюдается в документации по языку XAML и в спецификации XAML.

## <a name="other-xaml-primitives"></a>Другие примитивы XAML

В спецификации XAML 2009 отмечены другие примитивы XAML на уровне языка, такие как **x:Uri** и **x:Single**. Если это не указано в таблице в этой теме, другие примитивы языка XAML, определенные в других словарях XAML или спецификации XAML 2009, в настоящее время не поддерживаются в XAML для среды выполнения Windows.

**Обратите внимание** ,  даты и времени (свойства, использующие [**DateTime**](https://docs.microsoft.com/uwp/api/Windows.Foundation.DateTime) или [**DateTimeOffset**](https://docs.microsoft.com/dotnet/api/system.datetimeoffset), [**TimeSpan**](https://docs.microsoft.com/uwp/api/Windows.Foundation.TimeSpan) или [**System. TimeSpan**](https://docs.microsoft.com/dotnet/api/system.timespan)), не задаются с помощью примитива XAML. Обычно эти свойства вообще нельзя задавать в коде XAML, поскольку в средстве синтаксического анализа XAML в среде выполнения Windows нет правила по умолчанию для преобразования строки в значения даты и времени. Для значений инициализации любых свойств даты и времени необходимо использовать код программной части, который выполняется при загрузке страницы или элемента.

## <a name="related-topics"></a>Статьи по теме

* [Обзор языка XAML](xaml-overview.md)
* [Синтаксис XAML](xaml-syntax-guide.md)
* [Раскадровая анимация](https://docs.microsoft.com/windows/uwp/graphics/storyboarded-animations)
 

