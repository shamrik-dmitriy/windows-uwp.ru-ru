---
title: "Списки точек"
description: "Список точек — это коллекция вершин, которые отображаются в виде изолированных точек. Ваше приложение может использовать списки точки в трехмерных сценах для получения звездных полей или пунктирных линий на поверхности многоугольника."
ms.assetid: 332954AE-019F-4A91-B773-E3A7C92F3297
keywords: "Списки точек"
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.openlocfilehash: d5f2ee1c9a42a0d2f380f4f9a98c204088b1379a
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="point-lists"></a>Списки точек


Список точек — это коллекция вершин, которые отображаются в виде изолированных точек. Ваше приложение может использовать списки точки в трехмерных сценах для получения звездных полей или пунктирных линий на поверхности многоугольника.

## <a name="span-idexamplespanspan-idexamplespanspan-idexamplespanexample"></a><span id="Example"></span><span id="example"></span><span id="EXAMPLE"></span>Пример


На следующем рисунке показан отрисованный список точек.

![иллюстрация списка точек](images/pointlst.png)

Приложение может применять к списку точек материалы и текстуры. Цвета материала или текстуры отображаются только в нарисованных точках, но не между ними.

Следующий код показывает, как создать вершины для такого списка точек.

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

В примере кода ниже показано, как отрисовать этот список точек в Direct3D.

```
//
// It is assumed that d3dDevice is a valid
// pointer to an IDirect3DDevice interface.
//
d3dDevice->DrawPrimitive( D3DPT_POINTLIST, 0, 6 );
```

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Статьи по теме


[Примитивы](primitives.md)

 

 



