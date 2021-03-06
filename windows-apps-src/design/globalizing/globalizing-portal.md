---
Description: Узнайте о преимуществах глобализации и локализации приложения, а также о том, что означают эти термины.
Search.SourceType: Video
title: Глобализация и локализация
ms.assetid: c0791eec-5bb8-4a13-8977-61d7d98e35ce
label: Intro
template: detail.hbs
ms.date: 12/07/2018
ms.topic: article
keywords: windows 10, uwp, глобализация, локализуемость, локализация
ms.localizationpriority: medium
ms.openlocfilehash: d180621736e79daec91a11a6932e80633962d6c7
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/06/2020
ms.locfileid: "75684524"
---
# <a name="globalization-and-localization"></a>Глобализация и локализация

Windows используется во всем мире пользователями с разной культурой, живущими в разных регионах и говорящими на разных языках. Пользователи говорят на множестве различных языков, в различных странах и регионах. Некоторые пользователи говорят на нескольких языках. Таким образом, ваше приложение используется в конфигурациях, которые включают много вариантов системных параметров для языка и региона. Вы можете расширить потенциальный рынок для вашего приложения, используя *глобализацию* и *локализацию* для быстрой адаптации к любому региону.

Просмотрите это видео, чтобы узнать, как подготовить приложение для использования в любой стране мира: [Введение в глобализацию и локализацию](https://channel9.msdn.com/Blogs/One-Dev-Minute/Introduction-to-globalization-and-localization).

**Глобализация** — это процесс проектирования и разработки приложения таким образом, чтобы оно правильно работало на разных мировых рынках (на системах с различными конфигурациями языка и региона) без каких-либо изменений или настроек.

- При работе со строками следует учитывать культурные особенности региона. Например, не изменяйте регистр строк перед их сравнением.
- Используйте календари, которые подходят для текущего языка и региональных параметров.
- Используйте API глобализации, чтобы отображать данные, которые форматируются по-разному в различных регионах или странах, например числа, даты, время и валюты.
- Следует учитывать, что разные языки и региональные параметры имеют различные правила сортировки текста и других данных.

Ваш код должен работать одинаково хорошо в любой конфигурации языков и региональных параметров, которые будет поддерживать ваше приложение. В оптимальном варианте ваш код будет работать одинаково хорошо в контексте *любого* языка и региональных параметров. Наиболее эффективным способом глобализации функций вашего приложения является использование концепции языков/региональных параметров/регионов. Языки/региональные параметры/регионы — это набор правил и данных, характерных для конкретного языка и географического региона. Эти правила и данные включают следующие сведения.

- Классификации символов
- Система письма
- Форматы даты и времени
- Условные обозначения чисел, валюты, веса и систем измерений
- Правила сортировки

>[!NOTE]
> Список поддерживаемых имен языковых стандартов по версии операционной системы Windows см. в столбце тег языка таблицы в [приложении а. поведение продукта](https://docs.microsoft.com/openspecs/windows_protocols/ms-lcid/a9eac961-e77d-41a6-90a5-ce1a8b0cdb9c) в [справочнике по идентификатору кода языка Windows (LCID)](https://docs.microsoft.com/openspecs/windows_protocols/ms-lcid/70feba9f-294e-491e-b6eb-56532684c37f).

**Возможность локализации** — это процесс подготовки глобализованного приложения к локализации и/или проверка приложения на предмет готовности к локализации. Правильная подготовка приложения к локализации обеспечивает отсутствие выявления каких-либо функциональных неисправностей в приложении во время локализации. Самой важной особенностью локализуемого приложения является то, что его исполняемый код четко отделен от локализуемых ресурсов приложения.

- Длина строк, переведенных на разные языки, может сильно различаться. Таким образом, следует проектировать пользовательский интерфейс с учетом текста разной длины и размеров шрифтов для меток и элементов управления вводом текста.
- Старайтесь не использовать в изображениях текст или материал, восприятие которых зависит от языка и региональных стандартов.
- Не следует жестко задавать строки и зависимые от региона изображения в коде приложения и разметке. Вместо этого сохраните их в виде ресурсов строк и изображений, чтобы их можно было адаптировать для различных местных рынков независимо от двоичных файлов в вашем приложении.
- Псевдолокализация приложения выполняется для выявления проблем локализации.

**Локализация** — это процесс адаптации или перевода локализуемых ресурсов вашего приложения для обеспечения соответствия языковым, культурным и политическим требованиям определенных местных рынков, на которых это предложение будет представлено. К тому времени, как вы достигнете этапа локализации вашего приложения, и если ваше приложение может быть локализовано, вам не потребуется изменять какую-либо логику во время этого процесса.

- Переведите строковые ресурсы и другие ресурсы приложения для нового рынка.
- При необходимости измените какие-либо изображения, зависящие от языка и региональных параметров.
- Файлы на компьютере пользователя могут отличаться не только по языку, но и в зависимости от региона нахождения пользователя. Например, карта может иметь разные границы в зависимости от местоположения пользователя, но надписи должны быть на предпочтительном для пользователя языке.

Большинство групп локализации используют специальные средства для облегчения процесса. Например, это может быть повторное использование текста, перевод которого повторяется.

| Статья | Описание |
|---------|-------------|
| [Рекомендации по глобализации](guidelines-and-checklist-for-globalizing-your-app.md) | Проектирование и разработка приложения таким образом, чтобы оно правильно работало на системах с различными конфигурациями языка и региона. |
| [Общие сведения о языках профилей пользователей и языках манифестов приложений](manage-language-and-region.md) | В этом разделе приводятся определения терминов "список языков профиля пользователя", "список языков манифеста приложения" и "список языков среды выполнения приложения". Мы будем использовать эти термины в этом разделе и других разделах этой области функций, поэтому важно знать, что они означают. |
| [Глобализация форматов даты, времени и чисел](use-global-ready-formats.md) | Проектируйте приложение так, чтобы оно соответствовало международному стандарту, выбирая при разработке правильные форматы дат, времени, чисел, номеров телефона и валют. Это позволит позже адаптировать приложение для дополнительных языков и региональных параметров на международном рынке. |
| [Использование шаблонов и шаблонов для форматирования даты и времени](use-patterns-to-format-dates-and-times.md) | Для отображения даты и времени в желаемом формате используйте классы в пространстве имен [**Windows.Globalization.DateTimeFormatting**](/uwp/api/windows.globalization.datetimeformatting?branch=live) с пользовательскими шаблонами. |
| [Настройка макета и шрифтов, добавление поддержки написания справа налево](adjust-layout-and-fonts--and-support-rtl.md) | Проектируйте свое приложение таким образом, чтобы оно поддерживало макеты и шрифты различных языков, в том числе с написанием справа налево. |
| [Значения Нумералсистем](glob-numeralsystem-values.md) | В этом разделе перечислены значения, доступные для свойства **NumeralSystem** различных классов в пространстве имен [**Windows.Globalization**](/uwp/api/windows.globalization?branch=live). |
| [Сделайте приложение локализованным](prepare-your-app-for-localization.md) | Локализованное приложение — это приложение, которое можно локализовать для других рынков, регионов или на другие языки, не раскрыв какие-либо функциональные неисправности приложения. Самой важной особенностью локализуемого приложения является то, что его исполняемый код четко отделен от локализуемых ресурсов приложения. |
| [Международные шрифты](loc-international-fonts.md) | В этом разделе перечислены шрифты, доступные для приложений UWP, которые локализованы для языков, отличных от английского (США). |
| [Разработка приложения для двунаправленного текста](design-for-bidi-text.md) | Проектируйте приложения с поддержкой двунаправленного текста (BiDi), чтобы можно было комбинировать сценарии из систем письменности слева направо и справа налево. |
| [Использование набора многоязычных приложений 4,0](use-mat.md) | Набор многоязычных приложений (Toolkit) 4,0 интегрируется с Microsoft Visual Studio 2017 и более поздних версий для предоставления приложений UWP с поддержкой перевода, управления файлами перевода и инструментами для редактора. |
| [Вопросы и ответы по многоязычному инструментарию приложений 4,0 & устранение неполадок](mat-faq-troubleshooting.md) | В этом разделе содержатся ответы на часто задаваемые вопросы и инструкции по устранению проблем, связанных с работой набора средств для многоязычных приложений (MAT) версии 4.0. |
| [Использование кодовой страницы UTF-8](use-utf8-code-page.md) | UTF-8 — это универсальная кодовая страница для интернационализации. |
| [Подготовка приложения к изменению японской эры](japanese-era-change.md) | Узнайте об изменениях японской эры в мае 2019 года и о том, как подготовить свое приложение. |
