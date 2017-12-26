---
title: "Списки линий"
description: "Список линий — это список изолированных прямых отрезков. Списки линий полезны для таких задач, как добавление дождя или ливня в трехмерную сцену. Приложения создают списки линий, заполняя массив вершин."
ms.assetid: 42BF32A1-3535-42A3-82C5-3945CB309F2C
keywords: "Списки линий"
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.openlocfilehash: 8ebe6c67bd6b68023f59599cdcbd1f8f44f80e84
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="line-lists"></a>Списки линий


Список линий — это список изолированных прямых отрезков. Списки линий полезны для таких задач, как добавление дождя или ливня в трехмерную сцену. Приложения создают списки линий, заполняя массив вершин. Обратите внимание, что число вершин в списке линий должно быть четным числом больше двух или равным двум.

-   [Пример](#example)
-   [Статьи по теме](#related-topics)

## <a name="span-idexamplespanspan-idexamplespanspan-idexamplespanexample"></a><span id="Example"></span><span id="example"></span><span id="EXAMPLE"></span>Пример.


На следующем рисунке показан список отображаемых линий.

![иллюстрация списка линий](images/linelst.png)

К списку линий можно применять материалы и текстуры. Цвета в материале или текстуре отображаются только на прорисованных линиях и отсутствуют во всех других точках, не относящихся к этим линиям.

Следующий код показывает, как создать вершины для такого списка линий.

```
struct CUSTOMVERTEX
{
    float x,y,z;
};

CUSTOMVERTEX Vertices[] = 
{
    {-5.0, -5.0, 0.0},
    { 0.0,  5.0, 0.0},
    { 5.0, -5.0, 0.0},
    {10.0,  5.0, 0.0},
    {15.0, -5.0, 0.0},
    {20.0,  5.0, 0.0}
};
```

В примере кода ниже показано, как отрисовать список линий в Direct3D.

```
//
// It is assumed that d3dDevice is a valid
// pointer to an IDirect3DDevice interface.
//
d3dDevice->DrawPrimitive( D3DPT_LINELIST, 0, 3 );
```

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Статьи по теме


[Примитивы](primitives.md)

 

 



