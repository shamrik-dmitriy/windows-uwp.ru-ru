---
Description: Используйте элемент управления ParallaxView, чтобы добавить своему приложению глубины и движения.
title: Используйте фокусировки для добавления глубины и перемещения в приложение.
ms.assetid: ''
label: Parallax View
template: detail.hbs
ms.date: 08/09/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: abarlow
design-contact: conrwi
dev-contact: stpete
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: ac195916e76ad7b3f03adc39a293422d0d58f7a4
ms.sourcegitcommit: 8be8ed1ef4e496055193924cd8cea2038d2b1525
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/02/2020
ms.locfileid: "80614082"
---
# <a name="parallax"></a>Параллакс

Параллакс — это визуальный эффект, при котором элементы, расположенные ближе к зрителю, перемещаются быстрее элементов фона. Эффект параллакса создает ощущение глубины, перспективы и движения. В приложении UWP для создания эффекта параллакса можно использовать элемент управления ParallaxView.  

> **API-интерфейсы библиотеки пользовательского интерфейса Windows:** [класс параллаксвиев](/uwp/api/Microsoft.UI.Xaml.Controls.Parallaxview), [свойство вертикалшифт](/uwp/api/Microsoft.UI.Xaml.Controls.Parallaxview.VerticalShift), [свойство хоризонталшифт](/uwp/api/Microsoft.UI.Xaml.Controls.Parallaxview.HorizontalShift)
>
> **API-интерфейсы платформы**: [класс параллаксвиев](/uwp/api/Windows.UI.Xaml.Controls.Parallaxview), [свойство вертикалшифт](/uwp/api/Windows.UI.Xaml.Controls.Parallaxview.VerticalShift), [свойство хоризонталшифт](/uwp/api/Windows.UI.Xaml.Controls.Parallaxview.HorizontalShift)

## <a name="examples"></a>Примеры

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon.png" alt="XAML controls gallery" width="168"></img></td>
<td>
    <p>Если у вас установлено приложение <strong style="font-weight: semi-bold">галереи элементов управления XAML</strong>, щелкните здесь, чтобы <a href="xamlcontrolsgallery:/item/ParallaxView">открыть приложение и увидеть ParallaxView в действии</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Получить приложение XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Получить исходный код (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="parallax-and-the-fluent-design-system"></a>Параллакс и система проектирования Fluent Design

 Система Fluent Design позволяет создавать современные и эффективные пользовательские интерфейсы, которые отличаются яркостью, глубиной, движением, материальностью и масштабированием. Параллакс — это компонент системы проектирования Fluent Design, добавляющий движение, глубину и масштаб вашему приложению. Подробные сведения см. в статье [The Fluent Design System for Windows app creators](/windows/apps/fluent-design-system) (Система проектирования Fluent Design для разработчиков приложений Windows).

## <a name="how-it-works-in-a-user-interface"></a>Как это работает в пользовательском интерфейсе

В пользовательском интерфейсе можно создать эффекта параллакса, перемещая различные объекты с разной скоростью при горизонтальной или вертикальной прокрутке. <!-- Parallax is an important tool in adding depth to applications along with other techniques like transition animations, perspective tilt, and layering. --> Для демонстрации рассмотрим два уровня содержимого: список и фоновое изображение.  Список размещается поверх фонового изображения, создавая ощущение, что список находится ближе к зрителю.  Теперь, чтобы добиться фокусировкиного воздействия, мы хотим, чтобы объект, ближайший к нам, переходился в направлении "быстрее", чем объект, который находится дальше.  Когда пользователь прокручивает интерфейс, список перемещается быстрее относительно фонового изображения, что создает иллюзию глубины.

 ![Пример эффекта параллакса со списком и фоновым изображением](images/_Parallax_v2.gif)

 
## <a name="using-the-parallaxview-control-to-create-a-parallax-effect"></a>Использование элемента управления ParallaxView для создания эффекта параллакса

Для создания эффекта параллакса используется элемент управления [ParallaxView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Parallaxview). Он привязывает положение прокрутки элемента переднего плана, например списка, к фоновому элементу, например изображению. При прокрутке элемента переднего плана он анимирует фоновый элемент, создавая эффект параллакса. 

Чтобы использовать элемент управления ParallaxView, выберите элемент Source, фоновый элемент и установите для свойств [VerticalShift](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Parallaxview.VerticalShift) (для вертикальной прокрутки) и/или [HorizontalShift](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Parallaxview.HorizontalShift) (для горизонтальной прокрутки) значения выше нуля. 
* Свойство Source принимает ссылку на элемент переднего плана. Чтобы добиться эффекта параллакса, объектом переднего плана должен быть объект [ScrollViewer](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ScrollViewer) или элемент, содержащий объект ScrollViewer, например [ListView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listview) или [RichTextBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RichEditBox). 

* Чтобы задать фоновый элемент, добавьте его в качестве дочернего элемента управления ParallaxView. Фоновым элементом может быть любой объект [UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement), например [изображение](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Image) или панель, содержащая дополнительные элементы пользовательского интерфейса. 

Для создания эффекта параллакса объект ParallaxView должен располагаться за элементом переднего плана. Панели [Grid](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.grid) и [Canvas](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.canvas) дают возможность располагать элементы слоями один над другим, поэтому они подходят для использования с элементом управления ParallaxView.  

В этом примере показано, как создать эффект параллакса для списка:
 
```xaml
<Grid>
    <ParallaxView Source="{x:Bind ForegroundElement}" VerticalShift="50"> 
    
        <!-- Background element --> 
        <Image x:Name="BackgroundImage" Source="Assets/turntable.png"
               Stretch="UniformToFill"/>
    </ParallaxView>
    
    <!-- Foreground element -->
    <ListView x:Name="ForegroundElement">
       <x:String>Item 1</x:String> 
       <x:String>Item 2</x:String> 
       <x:String>Item 3</x:String> 
       <x:String>Item 4</x:String> 
       <x:String>Item 5</x:String>     
       <x:String>Item 6</x:String> 
       <x:String>Item 7</x:String> 
       <x:String>Item 8</x:String> 
       <x:String>Item 9</x:String> 
       <x:String>Item 10</x:String>     
       <x:String>Item 11</x:String> 
       <x:String>Item 13</x:String> 
       <x:String>Item 14</x:String> 
       <x:String>Item 15</x:String> 
       <x:String>Item 16</x:String>     
       <x:String>Item 17</x:String> 
       <x:String>Item 18</x:String> 
       <x:String>Item 19</x:String> 
       <x:String>Item 20</x:String> 
       <x:String>Item 21</x:String>        
    </ListView>
</Grid>
```    

Параллаксвиев автоматически корректирует размер образа, чтобы он работал для операции фокусировки, чтобы не беспокоиться о прокрутке изображения.

## <a name="customizing-the-parallax-effect"></a>Настройка эффекта параллакса 

Свойства VerticalShift и HorizontalShift позволяют контролировать степень эффекта параллакса.

* Свойство VerticalShift указывает, насколько фон должен сдвинуться по вертикали за всю операцию параллакса. Значение 0 означает, что фон не будет перемещаться вообще.
* Свойство HorizontalShift указывает, насколько фон должен сдвинуться по горизонтали за всю операцию параллакса. Значение 0 означает, что фон не будет перемещаться вообще.

Чем выше значение, тем сильнее эффект. 

Полный перечень способов настройки эффекта параллакса приведен в разделе "Класс ParallaxView". 

## <a name="dos-and-donts"></a>Что рекомендуется и что не рекомендуется делать

- Используйте параллакс для списков с фоновым изображением.
- Можно использовать эффект параллакса в ListViewItems, если там содержится изображение.
- Не используйте его везде, что чрезмерно сказывается на его влиянии

## <a name="related-articles"></a>Связанные статьи

- [Класс Параллаксвиев](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Parallaxview) 
- [Fluent Design для UWP](/windows/apps/fluent-design-system)
- [Наука в системе: дизайн и глубина Fluent](https://medium.com/microsoft-design/science-in-the-system-fluent-design-and-depth-fb6d0f23a53f)
