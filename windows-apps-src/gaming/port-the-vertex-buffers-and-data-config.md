---
title: Перенос данных и буферов вершин
description: На этом шаге вы определите буферы вершин, которые будут содержать ваши сетки, и буферы индексов, которые позволят шейдерам обходить вершины в указанном порядке.
ms.assetid: 9a8138a5-0797-8532-6c00-58b907197a25
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, игры, перенос, буферы вершин, данные, direct3d
ms.localizationpriority: medium
ms.openlocfilehash: 8445339d442fb740e9e2aba5e9d1cb0388c746ef
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/29/2019
ms.locfileid: "66368244"
---
# <a name="port-the-vertex-buffers-and-data"></a>Перенос данных и буферов вершин




**Важные API**

-   [**ID3DDevice::CreateBuffer**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createbuffer)
-   [**ID3DDeviceContext::IASetVertexBuffers**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-iasetvertexbuffers)
-   [**ID3D11DeviceContext::IASetIndexBuffer**](https://docs.microsoft.com/windows/desktop/api/d3d10/nf-d3d10-id3d10device-iasetindexbuffer)

На этом шаге вы определите буферы вершин, которые будут содержать ваши сетки, и буферы индексов, которые позволят шейдерам обходить вершины в указанном порядке.

Теперь давайте изучим кодированную модель для используемой сетки куба. Оба представления имеют вершины, упорядоченные в виде списка треугольников (в отличие от полосы или другого, более эффективного, макета из треугольников). Все вершины в обоих представлениях также имеют связанные с ними индексы и значения цвета. Большая часть кода Direct3D в этом разделе содержит ссылки на переменные и объекты, определенные в проекте Direct3D.

Вот куб для обработки в OpenGL ES 2.0. При реализации образца Каждая вершина представляет собой 7 значений с плавающей запятой: 3 координаты позиции, а также 4 значения RGBA цвета.

```cpp
#define CUBE_INDICES 36
#define CUBE_VERTICES 8

GLfloat cubeVertsAndColors[] = 
{
  -0.5f, -0.5f,  0.5f, 0.0f, 0.0f, 1.0f, 1.0f,
  -0.5f, -0.5f, -0.5f, 0.0f, 0.0f, 0.0f, 1.0f,
  -0.5f,  0.5f,  0.5f, 0.0f, 1.0f, 1.0f, 1.0f,
  -0.5f,  0.5f, -0.5f, 0.0f, 1.0f, 0.0f, 1.0f,
  0.5f, -0.5f,  0.5f, 1.0f, 0.0f, 1.0f, 1.0f,
  0.5f, -0.5f, -0.5f, 1.0f, 0.0f, 0.0f, 1.0f,  
  0.5f,  0.5f,  0.5f, 1.0f, 1.0f, 1.0f, 1.0f,
  0.5f,  0.5f, -0.5f, 1.0f, 1.0f, 0.0f, 1.0f
};

GLuint cubeIndices[] = 
{
  0, 1, 2, // -x
  1, 3, 2,

  4, 6, 5, // +x
  6, 7, 5,

  0, 5, 1, // -y
  5, 6, 1,

  2, 6, 3, // +y
  6, 7, 3,

  0, 4, 2, // +z
  4, 6, 2,

  1, 7, 3, // -z
  5, 7, 1
};
```

Вот тот же самый куб для обработки в Direct3D 11.

```cpp
VertexPositionColor cubeVerticesAndColors[] = 
// struct format is position, color
{
  {XMFLOAT3(-0.5f, -0.5f, -0.5f), XMFLOAT3(0.0f, 0.0f, 0.0f)},
  {XMFLOAT3(-0.5f, -0.5f,  0.5f), XMFLOAT3(0.0f, 0.0f, 1.0f)},
  {XMFLOAT3(-0.5f,  0.5f, -0.5f), XMFLOAT3(0.0f, 1.0f, 0.0f)},
  {XMFLOAT3(-0.5f,  0.5f,  0.5f), XMFLOAT3(0.0f, 1.0f, 1.0f)},
  {XMFLOAT3( 0.5f, -0.5f, -0.5f), XMFLOAT3(1.0f, 0.0f, 0.0f)},
  {XMFLOAT3( 0.5f, -0.5f,  0.5f), XMFLOAT3(1.0f, 0.0f, 1.0f)},
  {XMFLOAT3( 0.5f,  0.5f, -0.5f), XMFLOAT3(1.0f, 1.0f, 0.0f)},
  {XMFLOAT3( 0.5f,  0.5f,  0.5f), XMFLOAT3(1.0f, 1.0f, 1.0f)},
};
        
unsigned short cubeIndices[] = 
{
  0, 2, 1, // -x
  1, 2, 3,

  4, 5, 6, // +x
  5, 7, 6,

  0, 1, 5, // -y
  0, 5, 4,

  2, 6, 7, // +y
  2, 7, 3,

  0, 4, 6, // -z
  0, 6, 2,

  1, 3, 7, // +z
  1, 7, 5
};
```

Просматривая этот код, можно заметить, что куб в OpenGL ES 2.0 представлен в правовинтовой системе координат, тогда как куб в коде для Direct3D представлен в левовинтовой системе координат. При импорте ваших данных сетки необходимо обратить координаты по оси Z в вашей модели и соответственно изменить индексы для каждой сетки, чтобы обходить треугольники согласно измененной системе координат.

Предполагая, что мы успешно переместили сетку куба из правовинтовой системы координат OpenGL ES 2.0 в левовинтовую систему координат Direct3D, посмотрим, как загружать данные куба для обработки в обеих моделях.

## <a name="instructions"></a>Инструкция

### <a name="step-1-create-an-input-layout"></a>Шаг 1. Создавать входной макет

В OpenGL ES 2.0 данные вершин поставляются как атрибуты, которые передаются в объекты шейдера и читаются ими. Обычно вы предоставляете в объект программы-шейдера строку, которая содержит имя атрибута, используемое в GLSL шейдера, и получаете в ответ расположение в памяти, которое вы можете передать шейдеру. В этом примере объект буфера вершин содержит список пользовательских структур Vertex, определенных и отформатированных следующим образом:

OpenGL ES 2.0: Задайте для атрибутов, содержащих данные отдельных вершин.

``` syntax
typedef struct 
{
  GLfloat pos[3];        
  GLfloat rgba[4];
} Vertex;
```

В OpenGL ES 2.0 входной макеты являются неявно. воспользоваться общего назначения GL\_элемент\_МАССИВА\_БУФЕРА и предоставить шаг и смещение таким образом, чтобы вершинный построитель текстуры может интерпретировать данные после его отправки. Перед прорисовкой вы сообщаете шейдеру, какие атрибуты сопоставляются с частями каждого блока данных вершин, с помощью **glVertexAttribPointer**.

В Direct3D необходимо предоставить макет входных данных, чтобы описать структуру данных вершин в буфере вершин, перед созданием буфера, а не перед рисованием геометрии. Для этого используется макет входных данных, соответствующий макету данных индивидуальных вершин в памяти. Очень важно делать это аккуратно!

Здесь создаются описание входных данных как массив [ **D3D11\_ВХОДНОЙ\_элемент\_DESC** ](https://docs.microsoft.com/windows/desktop/api/d3d11/ns-d3d11-d3d11_input_element_desc) структуры.

Direct3D: Определите описание входной макет.

``` syntax
struct VertexPositionColor
{
  DirectX::XMFLOAT3 pos;
  DirectX::XMFLOAT3 color;
};

// ...

const D3D11_INPUT_ELEMENT_DESC vertexDesc[] = 
{
  { "POSITION", 0, DXGI_FORMAT_R32G32B32_FLOAT, 0, 0,  D3D11_INPUT_PER_VERTEX_DATA, 0 },
  { "COLOR",    0, DXGI_FORMAT_R32G32B32_FLOAT, 0, 12, D3D11_INPUT_PER_VERTEX_DATA, 0 },
};

```

Это описание входных данных определяет вершину как пару 3-координатных векторов: один трехмерный вектор для сохранения позиции вершины в координатах модели и второй трехмерный вектор для сохранения RGB-значения цвета, соответствующего вершине. В данном случае используется 3x32-битовый формат с плавающей точкой, элементы которого представляются в коде как `XMFLOAT3(X.Xf, X.Xf, X.Xf)`. При любой обработке данных, которые будут использоваться шейдером, необходимо использовать библиотеку [DirectXMath](https://docs.microsoft.com/windows/desktop/dxmath/ovw-xnamath-reference), обеспечивающую правильную упаковку и выравнивание данных. (Например, используйте [**XMFLOAT3**](https://docs.microsoft.com/windows/desktop/api/directxmath/ns-directxmath-xmfloat3) или [**XMFLOAT4**](https://docs.microsoft.com/windows/desktop/api/directxmath/ns-directxmath-xmfloat4) для данных векторов и [**XMFLOAT4X4**](https://docs.microsoft.com/windows/desktop/api/directxmath/ns-directxmath-xmfloat4x4) для матриц.)

Список всех возможных форматов, см. в разделе [ **DXGI\_ФОРМАТ**](https://docs.microsoft.com/windows/desktop/api/dxgiformat/ne-dxgiformat-dxgi_format).

Когда макет входных данных для вершин определен, вы создаете объект макета. В следующем коде, его, чтобы написать **m\_inputLayout**, переменной типа **ComPtr** (который указывает на объект типа [ **ID3D11InputLayout**](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11inputlayout)). **fileData** содержит компилированный объект вершинного шейдера из предыдущего шага, [Перенос шейдеров](port-the-shader-config.md).

Direct3D: Создайте входной макет, используемый буфера вершин.

``` syntax
Microsoft::WRL::ComPtr<ID3D11InputLayout>      m_inputLayout;

// ...

m_d3dDevice->CreateInputLayout(
  vertexDesc,
  ARRAYSIZE(vertexDesc),
  fileData->Data,
  fileShaderData->Length,
  &m_inputLayout
);
```

Мы определили макет входных данных. Теперь создадим буфер, который использует этот макет и загружает его с данными сетки куба.

### <a name="step-2-create-and-load-the-vertex-buffers"></a>Шаг 2. Создание и загрузка буферы вершин

В OpenGL ES 2.0 создается пара буферов, один для данных позиции и второй для данных цвета. (Можно также создать структуру, содержащую оба и один буфер.) Привязать каждый буфер и записи в их положение и данных о цвете. Далее в своей функции обработчика снова выполните привязку буферов и передайте формат данных в буфере в шейдер, чтобы он мог правильно интерпретировать их.

OpenGL ES 2.0: Привязать буферы вершин

``` syntax
// upload the data for the vertex position buffer
glGenBuffers(1, &renderer->vertexBuffer);    
glBindBuffer(GL_ARRAY_BUFFER, renderer->vertexBuffer);
glBufferData(GL_ARRAY_BUFFER, sizeof(VERTEX) * CUBE_VERTICES, renderer->vertices, GL_STATIC_DRAW);   
```

В Direct3D, доступный для шейдера буферы представляются в виде [ **D3D11\_SUBRESOURCE\_данных** ](https://docs.microsoft.com/windows/desktop/api/d3d11/ns-d3d11-d3d11_subresource_data) структуры. Чтобы привязать расположение этого буфера объекта шейдера, необходимо создать CD3D11\_БУФЕРА\_структура DESC для каждого буфера с [ **ID3DDevice::CreateBuffer**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createbuffer)и задайте Буфер контекста устройства Direct3D путем вызова метода set относящимся к типу буфера, такие как [ **ID3DDeviceContext::IASetVertexBuffers**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-iasetvertexbuffers).

При установке буфера необходимо задать шаг по индексу (размер данных элемента для отдельной вершины) и смещение (место, где фактически начинается массив данных вершины) от начала буфера.

Обратите внимание на то, что мы назначим указатель на **vertexIndices** массив **pSysMem** поле [ **D3D11\_SUBRESOURCE\_данных** ](https://docs.microsoft.com/windows/desktop/api/d3d11/ns-d3d11-d3d11_subresource_data) структуры. Если это сделать неправильно, сетка будет поврежденной или пустой!

Direct3D: Создайте и настройте буфера вершин

``` syntax
D3D11_SUBRESOURCE_DATA vertexBufferData = {0};
vertexBufferData.pSysMem = cubeVertices;
vertexBufferData.SysMemPitch = 0;
vertexBufferData.SysMemSlicePitch = 0;
CD3D11_BUFFER_DESC vertexBufferDesc(sizeof(cubeVertices), D3D11_BIND_VERTEX_BUFFER);
        
m_d3dDevice->CreateBuffer(
  &vertexBufferDesc,
  &vertexBufferData,
  &m_vertexBuffer);
        
// ...

UINT stride = sizeof(VertexPositionColor);
UINT offset = 0;
m_d3dContext->IASetVertexBuffers(
  0,
  1,
  m_vertexBuffer.GetAddressOf(),
  &stride,
  &offset);
```

### <a name="step-3-create-and-load-the-index-buffer"></a>Шаг 3. Создание и загрузка буфер индексов

Буферы индексов являются эффективным средством разрешить вершинному шейдеру находить отдельные вершины. Хотя они не являются обязательными, мы используем их в этом образце обработчика. Как и буферы вершин в OpenGL ES 2.0, буфер индексов создается и привязывается как буфер общего назначения, и в него копируются созданные ранее индексы вершин.

Когда вы готовы приступить к рисованию, вы снова выполняете привязку буфера вершин и буфера индексов, а затем вызываете **glDrawElements**.

OpenGL ES 2.0: Отправьте порядок индексов для вызова draw.

``` syntax
GLuint indexBuffer;

// ...

glGenBuffers(1, &renderer->indexBuffer);    
glBindBuffer(GL_ELEMENT_ARRAY_BUFFER, renderer->indexBuffer);   
glBufferData(GL_ELEMENT_ARRAY_BUFFER, 
  sizeof(GLuint) * CUBE_INDICES, 
  renderer->vertexIndices, 
  GL_STATIC_DRAW);

// ...
// Drawing function

// Bind the index buffer
glBindBuffer(GL_ELEMENT_ARRAY_BUFFER, renderer->indexBuffer);
glDrawElements (GL_TRIANGLES, renderer->numIndices, GL_UNSIGNED_INT, 0);
```

Процесс в Direct3D выполняется похожим образом, хотя и несколько более дидактически. Предоставьте буфер индексов как подчиненный ресурс Direct3D в [**ID3D11DeviceContext**](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11devicecontext), созданный при настройке Direct3D. Для этого вызывается [**ID3D11DeviceContext::IASetIndexBuffer**](https://docs.microsoft.com/windows/desktop/api/d3d10/nf-d3d10-id3d10device-iasetindexbuffer) с настроенным подчиненным ресурсом, как в следующем примере. (Опять же, обратите внимание, что необходимо назначить указатель на **cubeIndices** массив **pSysMem** поле [ **D3D11\_SUBRESOURCE\_данных** ](https://docs.microsoft.com/windows/desktop/api/d3d11/ns-d3d11-d3d11_subresource_data) структуры.)

Direct3D: Создание буфера индекса.

``` syntax
m_indexCount = ARRAYSIZE(cubeIndices);

D3D11_SUBRESOURCE_DATA indexBufferData = {0};
indexBufferData.pSysMem = cubeIndices;
indexBufferData.SysMemPitch = 0;
indexBufferData.SysMemSlicePitch = 0;
CD3D11_BUFFER_DESC indexBufferDesc(sizeof(cubeIndices), D3D11_BIND_INDEX_BUFFER);

m_d3dDevice->CreateBuffer(
  &indexBufferDesc,
  &indexBufferData,
  &m_indexBuffer);

// ...

m_d3dContext->IASetIndexBuffer(
  m_indexBuffer.Get(),
  DXGI_FORMAT_R16_UINT,
  0);
```

Далее мы будем рисовать треугольники с помощью вызова [**ID3D11DeviceContext::DrawIndexed**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-drawindexed) (или [**ID3D11DeviceContext::Draw**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-draw) для неиндексированных вершин), как в следующем примере. (Для получения более подробных сведений перейдите к разделу [Рисование на экране](draw-to-the-screen.md).)

Direct3D: Нарисуйте индексированных вершины.

``` syntax
m_d3dContext->IASetPrimitiveTopology(D3D11_PRIMITIVE_TOPOLOGY_TRIANGLELIST);
m_d3dContext->IASetInputLayout(m_inputLayout.Get());

// ...

m_d3dContext->DrawIndexed(
  m_indexCount,
  0,
  0);
```

## <a name="previous-step"></a>Предыдущий шаг


[Перенос объектов шейдеров](port-the-shader-config.md)

## <a name="next-step"></a>Дальнейшие действия

[Перенос GLSL](port-the-glsl.md)

## <a name="remarks"></a>Примечания

При структуризации Direct3D отдельно записывайте код, который вызывает методы для [**ID3D11Device**](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11device), в метод, который вызывается каждый раз, когда требуется заново создавать ресурсы устройства. В шаблоне проекта Direct3D этот код находится в методах **CreateDeviceResource** объекта обработчика. С другой стороны, код, обновляющий контекст устройства ([**ID3D11DeviceContext**](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11devicecontext)), помещается в метод **Render**, поскольку именно здесь вы конструируете стадии шейдера и привязываете данные.

## <a name="related-topics"></a>См. также


* [Практическое: порт простой модуль подготовки отчетов OpenGL ES 2.0 на Direct3D 11](port-a-simple-opengl-es-2-0-renderer-to-directx-11-1.md)
* [Перенос объектов шейдеров](port-the-shader-config.md)
* [Перенос данных и буферов вершин](port-the-vertex-buffers-and-data-config.md)
* [Перенос GLSL](port-the-glsl.md)

 

 




