---
title: Анализатор Xbox Live трассировки
description: Узнайте, как использовать анализатор трассировки Xbox Live для просмотра вызовов службы, сделанных название.
ms.assetid: b4490fae-d554-403d-bbbc-601af38af0ef
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, игры, универсальной платформы Windows, windows 10 для настольных ПК, xbox, один, вызовы служб, тестирование, анализатор
ms.localizationpriority: medium
ms.openlocfilehash: fa8ca37842edfbeaab0063cd953f3a34358a82da
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2019
ms.locfileid: "57623469"
---
# <a name="xbox-live-trace-analyzer"></a>Анализатор Xbox Live трассировки

API службы Xbox Live позволяет разработчикам title записать все вызовы служб, а затем проанализировать их в автономный режим для любых нарушений в вызове шаблонов. Вызов службы трассировки можно активировать с помощью новых функций, доступных в средстве xbtrace командной строки или с помощью протокола активации для более сложных сценариев. Также поддерживается активация отслеживания непосредственно из кода заголовок вызовов службы. Автономный анализ утилиту, Xbox Live трассировки анализаторе (XBLTraceAnalyzer.exe) можно найти в рамках пакета Xbox Live средств из [ https://aka.ms/xboxliveuwptools ](https://aka.ms/xboxliveuwptools).


## <a name="gather-logs-and-analyze-the-service-calls"></a>Сбор журналов и анализ вызовов служб

Следующие действия необходимы для сбора журналов, содержащие записи из вызовов службы и анализировать их с помощью анализатора-трассировки Xbox Live.

1.  Создание заголовка с помощью версии Xbox Live Services API, включенный в июле 2015 г или более новые версии пакета средств разработчика Xbox (XDK).
2.  Измените название, чтобы включить трассировку, как описано ниже.
3.  Разверните заголовок вашей.
4.  Запустите заголовка и хотя бы один вызов Xbox Live Services для инициализации служб API Xbox Live.
5.  Начать трассировку в том месте названия, что вы хотите проанализировать.
6.  Остановите трассировку.
7.  Запустите на Компьютере разработки Xbox Live трассировки анализатор и просмотреть выходные данные.

## <a name="starting-and-stopping-tracing"></a>Запуск и остановка трассировки

Существует три способа запуска и остановки трассировки:

1.  Набор API-службы Xbox Live можно вызвать непосредственно из заголовка.
2.  Можно использовать *xbtrace* средство командной строки.
3.  Вы можете воспользоваться активацией протокола через *управление приложениями (xbapp.exe)* средство командной строки.


### <a name="starting-and-stopping-tracing-directly-from-your-title"></a>Запуск и остановка трассировки непосредственно из заголовка

Чтобы начать трассировку непосредственно из названия, выполните следующие действия.

1.  В `Microsoft::Xbox::Services::Experimental` пространством имен, `EnableServiceCallTracking` свойство `ServiceCallTrackerSettings` класса значение true.
2.  Вызовите `StartServiceCallTracking()` Чтобы запустить трассировку вызовов службы.
3.  Вызовите `StopServiceCallTracking()` Чтобы остановить трассировку вызовов службы.
4.  После остановки трассировки скопировать результирующий файл трассировки от разработчика временный диск на консоли обратно на компьютер либо при помощи *копирования файлов (xbcp.exe)* или *одной окрестности Xbox* в анализ с помощью анализатора-трассировки Xbox Live.

### <a name="starting-and-stopping-tracing-by-using-the-xbtrace-command-line-tool"></a>Запуск и остановка трассировки с помощью средства командной строки xbtrace

Использовать средство командной строки xbtrace с типом трассировки xboxliveservices является наиболее удобный и простой способ запуска трассировки. При использовании xbTrace, результирующий файл трассировки копируется обратно на ваш компьютер для вас.

Запуск и остановка трассировок с помощью xbtrace зависит от протокола активации. Прежде чем использовать xbtrace для запуска и остановки трассировки, необходимо инициализировать активации протокола, вызвав `RegisterForProtcolActivation` метод `ServiceCallTrackerSettings` класса.

В следующем примере показано, как запуск и остановка трассировки приложения Xbox Live Services с помощью xbTrace:

    xbtrace start xboxliveservices
    xbtrace stop


Помните, что название должна быть запущена и активации протокола должен быть инициализирован, прежде чем можно запустить и остановить трассировку с xbtrace. После остановки трассировки xbtrace копирует файл трассировки на Компьютере разработки и помещает его в каталоге, имя которого содержит «xbtrace» и отметку времени. Имя этого каталога могут быть переопределены с помощью \[etlfile\] возможность xbtrace.

<a name="starting-and-stopping-tracing-by-using-protocol-activation"></a>Запуск и остановка трассировки с помощью протокола активации
----------------------------------------------------------
Трассировки можно также управлять с помощью протокола активации функций «xbApp выпуск». Необходимо знать заголовок вашей titleid для запуска и остановки трассировки через протокол. В файле манифеста заголовок вашей можно найти свой идентификатор заголовка. Трассировка осуществляется с помощью URI, который содержит параметр «serviceCallTracking». Ниже приведены примеры, как запустить и остановить трассировку для заголовка, идентификатор которого title — 12345678.

    xbapp launch "ms-xbl-12345678://serviceCallTracking?state=start"
    xbapp launch "ms-xbl-12345678://serviceCallTracking?state=stop"

При использовании протокола активации, результирующий файл трассировки хранится на временный диск разработчика на консоли. Вам потребуется скопировать файл обратно на вашем ПК, с помощью xbcp или одной окрестности Xbox. Файл не копируется автоматически к ПК, как при использовании xbtrace.

Протокол активации можно задать параметры дополнительные трассировки, такие как уровень детализации. Поддерживаются четыре уровня детализации: quiet, диагностики, подробные и минимальным. В следующем примере показано, как задать уровень детализации:

    xbapp launch "ms-xbl-12345678://serviceCallTracking?verbosity=diagnostic"

## <a name="analyze-the-trace-file"></a>Анализ файла трассировки

После копирования файла трассировки обратно на вашем ПК, Xbox Live трассировки анализатор на GNDP можно использовать для анализа использования заголовок вашей Xbox Live служб. См. в документации, включенные в Xbox Live трассировки анализатор сети разработчиков игр описание того, как для вызова инструмента и интерпретации выходных данных. Также вы можете XBLTraceAnalyzer.exe с параметром командной строки? или -h, чтобы просмотреть справку командной строки.