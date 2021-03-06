---
title: Преобразование проекции
description: Проекционное преобразование управляет внутренними процедурами камеры, такими как выбор объектива для камеры. Это самый сложный из всех трех типов преобразований.
ms.assetid: 378F205D-3800-4477-9820-5EBE6528B14A
keywords:
- Преобразование проекции
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: f0806c0aa7a130a080457f4361d17f64451846f9
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2019
ms.locfileid: "57634549"
---
# <a name="projection-transform"></a>Преобразование проекции


*Проекционное преобразование* управляет внутренними процедурами камеры, такими как выбор объектива для камеры. Это самый сложный из всех трех типов преобразований.

Обычно матрица проекции — это проекция масштаба и перспективы. Проекционное преобразование превращает видимое пространство (усеченную пирамиду) в кубовидную фигуру. Так как ближний конец видимого пространства меньше дальнего, это приводит к развертыванию объектов, которые находятся ближе к камере — так перспектива применяется к сцене.

В [видимом пространстве](viewports-and-clipping.md) расстояние между камерой и началом координат видимого пространства преобразования определяется произвольно как D, поэтому матрица проекции выглядит, как показано на следующем рисунке.

![иллюстрация матрицы проекции](images/projmat1.png)

Матрица просмотра переносит камеру в начало координат, перемещаясь в направлении z на –D. Матрицы преобразования выглядит, как показано на следующем рисунке.

![иллюстрация матрицы преобразования](images/projmat2.png)

Умножении матрицы преобразования матрица проекции (T\*P) предоставляет матрица составного проекции, как показано на следующем рисунке.

![иллюстрация составной матрицы проекции](images/projmat3.png)

Преобразование перспективы превращает видимое пространство в новое пространство координат. Обратите внимание, что усеченная пирамида становится кубоидом, а начало координат перемещается из правого верхнего угла сцены в центр, как показано на следующем рисунке.

![схема изменения усеченной пирамиды обзора в новое пространства координат за счет преобразования перспективы](images/cuboid.png)

При преобразовании перспективы ограничения направлений x и y равны –1 и 1 соответственно. Ограничения по оси z равны 0 для передней плоскости и 1 для задней плоскости.

Эта матрица преобразует и масштабирует объекты на основе определенного расстояния между камерой и ближней плоскостью отсечения, но при этом не учитывается, что поле зрения (fov) и z-значения, создаваемые для объектов, на расстоянии могут быть практически идентичными, что усложняет сравнение глубины. Следующая матрица решает эти проблемы и корректирует вершины для учета пропорций окна просмотра, что хорошо подходит для проекции перспективы.

![иллюстрация матрицы для проекции перспективы](images/prjmatx1.png)

В этой матрице Zₙ — это z-значение ближней плоскости отсечения. Переменные w, h и Q имеют следующие значения. Обратите внимание, что fov<sub>w</sub> и fovₖ представляют горизонтальные и вертикальные поля зрения окна просмотра в радианах.

![формулы значений переменных](images/prjmatx2.png)

В вашем приложении использование углов полей зрения для определения коэффициентов масштабирования осей x и y может быть не так удобно, как применение горизонтального и вертикального измерения окна просмотра (в пространстве камеры). В следующих двух уравнениях для w и h используются измерения окна просмотра, при этом они эквивалентны предыдущим формулам.

![формулы значений переменных w и h](images/prjmatx3.png)

В этих формулах Zₙ представляет положение ближней плоскости отсечения, а переменные V<sub>w</sub> и Vₕ представляют ширину и высоту окна просмотра в пространстве камеры.

Какую бы формулу вы ни выбрали, обязательно установите для Zₙ максимально возможное значение, так как z-значения, очень близкие к камере, не сильно отличаются. Это относительно усложняет сравнение глубины с помощью 16-разрядных z-буферов.

## <a name="span-idawfriendlyprojectionmatrixspanspan-idawfriendlyprojectionmatrixspanspan-idawfriendlyprojectionmatrixspana-w-friendly-projection-matrix"></a><span id="A_W_Friendly_Projection_Matrix"></span><span id="a_w_friendly_projection_matrix"></span><span id="A_W_FRIENDLY_PROJECTION_MATRIX"></span>Матрица проекции w с поддержкой


Direct3D может использовать компонент w вершины, которая была преобразована абсолютной матрицей, а также матрицами представления и проекции для вычислений на основе глубины при использовании буфера глубины или эффекта тумана. Для таких вычислений требуется, чтобы матрица проекции нормализовала переменную w как эквивалентную координате z в абсолютном пространстве. Иными словами, если матрица проекции включает коэффициент (3,4), не равный 1, необходимо масштабировать все коэффициенты, инвертируя коэффициент (3,4) для получения правильной матрицы. Если не предоставить совместимую матрицу, эффекты тумана и буферизация глубины не будут правильно применены.

На следующем рисунке показана несоответствующая требованиям матрица проекции и та же матрица, масштабированная для применения тумана туман относительно зрителя.

![иллюстрации несоответствующей матрицы проекции и матрицы с туманом относительно зрителя](images/eyerlmx.png)

В предыдущих матрицах предполагается, что все переменные не равны нулю. Сведения о буферизации глубины на основе компонента w см. в разделе [Буферы глубины](depth-buffers.md).

Direct3D использует текущую матрицу проекции при расчетах глубины вершин на основе переменной w. В результате приложения должны установить соответствующую матрицу проекции для получения необходимых функций на основе компонента w, даже если они не используют Direct3D для преобразования.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Связанные разделы


[Преобразует](transforms.md)

 

 




