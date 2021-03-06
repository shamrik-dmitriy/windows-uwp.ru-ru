---
title: Перенос простого отрисовщика OpenGL ES 2.0 в Direct3D 11
description: В качестве первого упражнения в переносе начнем с основ - с переноса простого обработчика для вращающегося куба с затенением по вершинам из OpenGL ES 2.0 в Direct3D, чтобы он соответствовал шаблону «Приложение DirectX 11 (универсальные приложения для Windows)» из Visual Studio 2015.
ms.assetid: e7f6fa41-ab05-8a1e-a154-704834e72e6d
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, игры, opengl, direct3d 11, перенос
ms.localizationpriority: medium
ms.openlocfilehash: 3c17e0b8ceb5938b7ca224f4a67198929a37a7f4
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/29/2019
ms.locfileid: "66368363"
---
# <a name="port-a-simple-opengl-es-20-renderer-to-direct3d-11"></a>Перенос простого отрисовщика OpenGL ES 2.0 в Direct3D 11



В качестве упражнения в переносе начнем с основ — с переноса простого обработчика для вращающегося куба с затенением по вершинам из OpenGL ES 2.0 в Direct3D, чтобы он соответствовал шаблону "Приложение DirectX 11 (универсальные приложения для Windows)" из Visual Studio 2015. Из разбора этого процесса переноса вы узнаете следующее:

-   как переносить простой набор буферов вершин в буферы ввода Direct3D;
-   как переносить однородности и атрибуты в буферы констант;
-   как настраивать объекты шейдеров Direct3D;
-   как базовая семантика HLSL используется в разработке шейдеров Direct3D;
-   как переносить очень простой GLSL в HLSL.

Этот раздел начинается сразу после создания нового проекта DirectX 11. Чтобы узнать, как создавать новые проекты DirectX 11, ознакомьтесь с разделом [Создание нового проекта DirectX 11 для универсальной платформы Windows (UWP)](user-interface.md).

В проекте, созданном на основании любой из этих ссылок, уже подготовлен весь нужный код для инфраструктуры [Direct3D](https://docs.microsoft.com/windows/desktop/direct3d11/dx-graphics-overviews), что позволяет немедленно начать процесс переноса обработчика из Open GL ES 2.0 в Direct3D 11.

В этом разделе подробно разобраны две ветви кода, выполняющие одну и ту же базовую задачу обработки графики: отображение вращающегося куба с затененными вершинами. В обоих случаях код включает следующий процесс.

1.  Создание сетки куба из встроенных данных. Эта сетка представлена как список вершин, каждая из которых обладает положением, нормальным вектором и вектором цвета. Эта сетка помещается в буфер вершин для обработки конвейера затенения.
2.  Создание объектов шейдера для обработки сетки куба. Есть два шейдера: вершинный шейдер, который обрабатывает вершины для растеризации, и шейдер фрагментов, который окрашивает отдельные пиксели куба после растеризации. Эти пиксели записываются в однобуферную прорисовку для отображения.
3.  Формирование языка затенения, который используется для обработки вершин и пикселей в вершинных шейдерах и шейдерах фрагментов соответственно.
4.  Отображение отрисованного куба на экране.

![Простой куб OpenGL](images/simple-opengl-cube.png)

По завершении этого пошагового руководства вы должны знать о следующих фундаментальных различиях между Open GL ES 2.0 и Direct3D 11:

-   отображение буферов и данных вершин;
-   процесс создания и настройки шейдеров;
-   языки затенения, а также входные и выходные данные объектов шейдеров;
-   поведение при прорисовке экрана.

В этом руководстве мы будем говорить о простой и стандартной структуре обработчика OpenGL, которая определена следующим образом.

``` syntax
typedef struct 
{
    GLfloat pos[3];        
    GLfloat rgba[4];
} Vertex;

typedef struct
{
  // Integer handle to the shader program object.
  GLuint programObject;

  // The vertex and index buffers
  GLuint vertexBuffer;
  GLuint indexBuffer;

  // Handle to the location of model-view-projection matrix uniform
  GLint  mvpLoc; 
   
  // Vertex and index data
  Vertex  *vertices;
  GLuint   *vertexIndices;
  int       numIndices;

  // Rotation angle used for animation
  GLfloat   angle;

  GLfloat  mvpMatrix[4][4]; // the model-view-projection matrix itself
} Renderer;
```

У этой структуры имеется один экземпляр, и он содержит все необходимые компоненты для прорисовки очень простой сетки с затенением по вершинам.

> **Примечание**  Any OpenGL ES 2.0 кода в этом разделе основан на реализации Windows API, предоставляемые группе Khronos и использует C Windows, программирования синтаксис.

 

## <a name="what-you-need-to-know"></a>Что необходимо знать


### <a name="technologies"></a>Технологии

-   [Microsoft Visual C++](https://docs.microsoft.com/previous-versions/60k1461a(v=vs.140))
-   OpenGL ES 2.0

### <a name="prerequisites"></a>Предварительные требования

-   Необязательно. Просмотрите раздел [Перенос кода EGL в DXGI и Direct3D](moving-from-egl-to-dxgi.md). Прочитайте этот раздел, чтобы лучше понять графический интерфейс, предоставляемый DirectX.

## <a name="span-idkeylinksstepsheadingspansteps"></a><span id="keylinks_steps_heading"></span>Действия


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Раздел</th>
<th align="left">Описание</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="port-the-shader-config.md">Перенос объектов шейдеров</a></p></td>
<td align="left"><p>При переносе простого обработчика из OpenGL ES 2.0 прежде всего следует задать эквивалентные объекты вершинного шейдера и шейдера фрагментов в Direct3D 11 и убедиться, что основная программа может связываться с объектами шейдера после их компиляции.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="port-the-vertex-buffers-and-data-config.md">Перенос данных и буферов вершин</a></p></td>
<td align="left"><p>На этом шаге вы определите буферы вершин, которые будут содержать ваши сетки, и буферы индексов, которые позволят шейдерам обходить вершины в указанном порядке.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="port-the-glsl.md">Перенос GLSL</a></p></td>
<td align="left"><p>После переноса кода, который создает и настраивает буферы и объекты шейдеров, следует перенести внутренний код шейдеров из версии GLSL (GL Shader Language) для OpenGL ES 2.0 в HLSL (High-level Shader Language) для Direct3D 11.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="draw-to-the-screen.md">Рисование объектов на экране</a></p></td>
<td align="left"><p>Наконец, мы переносим код, который отрисовывает вращающийся куб на экране.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idadditionalresourcesspanadditional-resources"></a><span id="additional_resources"></span>Дополнительные ресурсы


-   [Подготовка среды разработки для разработки игр DirectX для универсальной платформы Windows](prepare-your-dev-environment-for-windows-store-directx-game-development.md)
-   [Создание нового проекта DirectX 11 для универсальной платформы Windows](user-interface.md)
-   [Основные понятия OpenGL ES 2.0 и инфраструктуры сопоставляются Direct3D 11](map-concepts-and-infrastructure.md)

 

 




