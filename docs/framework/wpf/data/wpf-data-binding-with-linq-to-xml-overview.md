---
title: Привязка данных с помощью LINQ to XML
ms.date: 10/22/2019
ms.topic: conceptual
ms.openlocfilehash: 3c5567c81d2097a1524f5bbbf9010836ca8c0646
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2020
ms.locfileid: "76733816"
---
# <a name="overview-of-wpf-data-binding-with-linq-to-xml"></a>Общие сведения о привязке данных WPF к LINQ to XML

В этой статье описаны функции динамического связывания данных в пространстве имен <xref:System.Xml.Linq>. Эти средства можно применять в качестве источника данных для элементов пользовательского интерфейса в приложениях Windows Presentation Foundation (WPF). Такая организация работы основана на использовании особых *динамических свойств* классов <xref:System.Xml.Linq.XAttribute?displayProperty=fullName> и <xref:System.Xml.Linq.XElement?displayProperty=fullName>.

## <a name="xaml-and-linq-to-xml"></a>XAML и LINQ to XML

Язык XAML является диалектом XML, созданным в корпорации Майкрософт для поддержки технологий .NET. Он используется в WPF для представления элементов пользовательского интерфейса и связанных с этим возможностей, таких как события и привязка данных. В Windows Workflow Foundation язык XAML используется для представления структуры программ, например в виде средств управления программой (*потоков операций*). XAML позволяет отделить декларативные аспекты технологии от связанного с ними процедурного кода, определяющего более индивидуализированное поведение программы.

Существует два общих способа взаимодействия XAML и LINQ to XML.

- Поскольку файлы XAML - это XML-документы правильного формата, к ним можно выполнять запросы и управлять ими с помощью таких XML-технологий, как LINQ to XML.

- Запросы LINQ to XML представляют источник данных, поэтому могут использоваться как источник данных для привязки данных в элементах пользовательского интерфейса WPF.

В этой документации описан второй сценарий.

## <a name="data-binding-in-the-windows-presentation-foundation"></a>Привязка данных в Windows Presentation Foundation

Привязка данных в WPF позволяет связать элемент пользовательского интерфейса с одним из его свойств в источнике данных. Простым примером этого может служить метка <xref:System.Windows.Controls.Label>, текст которой представляет значение открытого свойства в пользовательском объекте. Привязка данных в WPF зависит от следующих компонентов.

|Компонент|Description|
|---------------|-----------------|
|Цель привязки|Элемент пользовательского интерфейса, который должен быть привязан к источнику данных. Визуальные элементы WPF являются производными класса <xref:System.Windows.UIElement>.|
|Целевое свойство|*Свойство зависимостей* целевого объекта привязки, отражающее значение источника привязки к данным. Свойства зависимости напрямую поддерживаются классом <xref:System.Windows.DependencyObject>, производным которого является <xref:System.Windows.UIElement>.|
|Источник привязки|Исходный объект для одного или нескольких значений, которые отправляются в элемент пользовательского интерфейса. WPF автоматически поддерживает следующие типы источников привязки: объекты среды CLR, объекты данных ADO.NET, XML-данные (полученные с помощью запросов XPath или LINQ to XML) и другие объекты <xref:System.Windows.DependencyObject>.|
|Исходный путь|Свойство источника привязки, разрешение которого приводит к получению значения или набора значений, подлежащих привязке.|

Свойство зависимости является понятием WPF, которое представляет динамически вычисляемое свойство элемента пользовательского интерфейса. Например, свойства зависимостей часто имеют значение или значения по умолчанию, предоставляемые родительским элементом. Эти особые свойства поддерживаются экземплярами класса <xref:System.Windows.DependencyProperty> (но не полями, как в случае стандартных свойств). Дополнительные сведения см. в [обзоре свойств зависимостей](../advanced/dependency-properties-overview.md).

### <a name="dynamic-data-binding-in-wpf"></a>Динамическая привязка данных в WPF

По умолчанию привязка данных происходит, только если инициализирован целевой элемент пользовательского интерфейса. Это называется *однократной* привязкой. Для большинства целей этого недостаточно, поскольку решение с привязкой к данным требует, чтобы изменения динамически передавались во время выполнения с помощью одного из следующих способов.

- *Односторонняя* привязка автоматически передает изменения в одном направлении. В большинстве случаев изменения в источнике отражаются в назначении, но иногда полезно, чтобы происходило обратное.

- При наличии *двусторонней* привязки изменения в источнике автоматически передаются в назначение, а изменения в назначении автоматически отражаются в источнике.

Чтобы односторонняя или двусторонняя привязка могла иметь место, источник должен применять механизм уведомлений об изменениях, например с помощью реализации интерфейса <xref:System.ComponentModel.INotifyPropertyChanged>, либо используя шаблон *PropertyNameChanged* для каждого поддерживаемого свойства.

Дополнительные сведения о привязке данных в WPF см. в статье [Привязка данных (WPF)](/dotnet/framework/wpf/data/data-binding-wpf).

## <a name="dynamic-properties-in-linq-to-xml-classes"></a>Динамические свойства в классах LINQ to XML

Большинство классов LINQ to XML не могут считаться настоящими динамическими источниками данных WPF: некоторые самые полезные данные доступны только через методы, но не через свойства, а свойства не реализуют уведомление об изменениях. Для поддержки привязки данных WPF в LINQ to XML предоставляется набор *динамических свойств*.

Эти динамические свойства являются особыми свойствами времени выполнения, дублирующими функциональность существующих методов и свойств в классах <xref:System.Xml.Linq.XAttribute> и <xref:System.Xml.Linq.XElement>. Динамические свойства были добавлены к этим классам только для того, чтобы позволить им выступать в роли динамических источников данных для WPF. С этой целью все динамические свойства реализуют уведомления об изменениях. Подробные сведения о динамических свойствах представлены в следующем разделе, [Динамические свойства LINQ to XML](linq-to-xml-dynamic-properties.md).

> [!NOTE]
> Многие стандартные общие свойства различных классов в пространстве имен <xref:System.Xml.Linq> могут использоваться в качестве однократной привязки данных. Однако следует помнить, что в этом случае ни источник, ни цель не будут обновляться динамически.

### <a name="access-dynamic-properties"></a>Доступ к динамическим свойствам

Динамические свойства в классах <xref:System.Xml.Linq.XAttribute> и <xref:System.Xml.Linq.XElement> не могут оцениваться как стандартные свойства. Например, в языках, совместимых со средой CLR, например в C#, с ними нельзя выполнять следующие действия.

- Оценивать непосредственно во время компиляции. Динамические свойства невидимы для компилятора и технологии IntelliSense среды Visual Studio.

- Обнаруживать или оценивать во время выполнения с помощью отражения .NET. Даже во время выполнения они не рассматриваются как свойства с точки зрения среды CLR.

В C# динамические свойства можно оценивать только во время выполнения через ряд возможностей пространства имен <xref:System.ComponentModel>.

В отличие от этого, в XML-источнике динамические свойства можно оценивать, используя простую нотацию в следующей форме:

```xml
<object>.<dynamic-property>
```

Динамические свойства для этих двух классов разрешены с получением значения, которое можно использовать напрямую, или индексатора, который необходимо представить вместе с индексом, чтобы получить значение или коллекцию значений. В последнем случае синтаксис принимает следующий вид:

```xml
<object>.<dynamic-property>[<index-value>]
```

Дополнительные сведения см. в разделе [Динамические свойства LINQ to XML](linq-to-xml-dynamic-properties.md).

Для реализации динамической привязки WPF динамические свойства используются со средствами пространства имен <xref:System.Windows.Data>, особенно с классом <xref:System.Windows.Data.Binding>.

## <a name="see-also"></a>См. также раздел

- [Привязка данных WPF с помощью LINQ to XML](wpf-data-binding-with-linq-to-xml-overview.md)
- [Динамические свойства LINQ to XML](linq-to-xml-dynamic-properties.md)
- [XAML в WPF](../advanced/xaml-in-wpf.md)
- [Привязка данных (WPF)](/dotnet/framework/wpf/data/data-binding-wpf)
- [Использование разметки рабочего процесса](https://go.microsoft.com/fwlink/?LinkId=98685)
