---
tags:
  - "#browser-technology"
  - "#perfomance"
---
# Проблемы с рендерингом

Это важно знать, так как это вполне могут спросить на собеседовании

---

## Проблема с Force Reflow

Эта проблема продемонстрирована в этом примере: https://googlechrome.github.io/devtools-samples/jank/. Если мы будем кучу раз нажимать на кнопку "Add 10", то у нас создастся куча синих квадратиков и все начнет лагать. Однако, если мы нажмем на кнопку "Optimize", то все начнет летать!

Сначала нужно понять как работает пайплайн отрисовки в браузере. Наш пайплайн выглядит так:

DOM -> CSS OM -> Layout -> Paint -> Composite

- DOM - это этап парсинга html файла в Document Oblect Model
- CSS OM - это этап парсинга css стилей в дерево стилей CSS Object Model
- Layout - Процесс сопоставления DOM элементов со стилями из CSS OM в соответствии с селекторами и создание макета страницы. На этом этапе берутся все width, paddings, margins и так далее для создания макета страницы
- Paint - Браузер берет все box-shadow, backgraund, backgraund-color, color и так далее для раскраски всех элементов ранее построенного шаблона
- Composite - Браузер собирает все слои в один документ (Браузер умный и сам умеет делить документ на слои, но эта возможность доступна и программисту).

Force reflow - это проблема, когда у нас много раз вызывается стадия Layout. Эта стадия очень трудозатратна и она вызывается каждый раз когда мы вносим изменения в шаблон (меняем ширину, высоту, паддинги элементов). Плюс, дергая Layout у нас может дергаться стадия Paint и Composite.
Эта стадия вызывается не только когда мы меняем эти свойства, но и когда мы пытаемся получить ширину, отступ и т.д. через методы offsetTop, scrollTop  и т. д. Это происходит потому что когда мы дергаем offsetTop, то браузеру нужно пересчитать Layout чтобы вернуть тебе этот самый Top. Поэтому менять размеры и позиции элементов лучше через transform, так как свойство transform не затрагивает Layout (transform просто никак не изменяет шаблон документа).

Если мы посмотрим на код наших синих квадратиков, то увидим это:

![[optimization-1.png]]

Так выглядит перфоманс в devtools։

![[optimization-3.png]]

В неоптимизированном варианте мы каждый тик анимации пытаемся получить offsetTop квадратика и пересчитать его. В оптимизированном варианте мы не дергаем этот метод, а достаем свойство top из стилей։

![[optimization-2.png]]

==В примере от google разработчики сильно ускории перфоманс за счет переходна от .offsetTop к .style.top, но так не всегда можно сделать. Проблема в том, что в стилях то же свойство top может быть задано как auto и тогда вы не сможете получить нормальное абсолютное значение. Не забывайте что у нас есть transform, который не дергает Layout и старайтесь использовать его тоже==

Так выглядит перфоманс в devtools։

![[optimization-4.png]]

Можем увидеть, что количество времени, затраченного на отрисовку уменьшилось в два раза с 2458 мс до 1242 мс. Наш браузер даже бездействовал целых 1081 мс.

---
## Разделение страницы на слои

Браузер автоматически выделяет элементы в отдельные слои в некоторых случаях. Например, при использовании transform, opacity, z-index, display со значением absolute и relative и т.д. Слои можно просматривать в devtools:

![[optimization-5.png]]

Для того чтобы ускорить стадию перерисовки, мы можем самостоятельно выделить любой блок в отдельный слой. Это можно сделать задав в стилях блока свойство will-change. Иногда это свойство задают динамически через js (желательный способ) чтобы предупредить браузер что элемент будет в скоре перерисован.

==Нужно помнить что создание слишком большого количества слоев вредно. Браузер тратит большие ресурсы видеокарты для того чтобы следить за каждым слоем, а потом компановать их воедино. В частности рекомендуется применять will-change только в крайних случаях, так как браузер в большинстве случаев сам знает что и как оптимизировать. Также рекомендуется использовать will-change экономно и не писать его непосредственно в таблице стилей.==

Синтаксис will-change выглядит так:

```css
will-change: auto;//Применение свойства без применения какой-либо специальной эвристики
will-change: scroll-position;//Говорит браузеру что вероятно будет изменена позиция скролла
will-change: contents;// Говорит, что планируется анимировать или изменить что-то в содержимом элемента в ближайшем будущем.
will-change: custom-ident; //Тут может быть любое свойство, например: transform
```

---
# Производительность в v8

В этом разделе статьи будет много спичечных оптимизаций, поэтому можно особо не вникать, но если интересно понять как работает язык js, то можете почитать :)

---
## Идентификаторы

Все переменные, все параметры функции, свойства объектов и даже строки - это идентификаторы. Js пытается их оптимизировать и создать как можно меньше идентификаторов:

```js
let hello = "hello" - создан один идентификатор hello
let hello = "world" - создано два идентификатора: hello и world
```

---
## Скрытые классы (hidden classes)

Тут тот же принцип как и с идентификаторами. Если у нас несколько объектов, которые имеют одну и ту же структуру, то js под капотом создаст для них один hidden class (если что-то крякает как утка, плавает как утка и что-то там еще как утка - то это утка:) ). Если мы добавим какое-то свойство в объект, то js придется создать уже новый скрытый класс. Утиная типизация - это необходимость, так как свойства в объектах могут добавляться и удаляться (в Java, C++ и так далее так, естественно, нельзя). Плюс в js динамическая типизация и переменная может менять тип данных, следовательно нельзя привязаться к размеру переменной и счетать смещения в памяти относительно них.

---
## Дырявые массивы (hole arrays)

Если вы удалите элемент массива при помощи delete оператора, то вы увидите что теперь на месте удаленного элемента у нас стоит empty. Это не очень хорошо, так как с этой минуты v8 больше никогда не станет его оптимизировать. Дело в том, что с обычными массивами v8 работает как с обычными динамическими массивами. Как только в нем появляется дырка, v8 уже не может полагаться на то что все индексы имеются в массиве и конвертирует его под капотом в хэш-таблицу.

---
## Следить за постоянством типов данных

Это делает код предсказуемым для оптимизатора. Если мы будем использовать нормальные массивы исключительно, например, из целых чисел, то этот код будет работать со скоростью C++ (так как JIT в Turbofan скомпилирует его в машинный код для платформы нашего железа, что очень сильно ускорит программу). Если функция в качестве параметра всегда принимает целое число, то оптимизатор оптимизирует функцию, а если до этого вы передавали цифры, а потом вдруг внезапно передатите в нее строку, то оптимизатору придется все деоптимизировать обратно.

---
## Особенности let и const

Интерпритация кода в js разделена на три этапа (в соответствии со спецификацией). Первая стадия - статический анализ. Одним из этапов статического анализа - проход по коду и поиск дубликатов каждой переменной, объявленной через let и const. Это нужно чтобы убедиться что переменная не объявлена более одного раза. В случае с var такой проблемы нет так как такая переменная может быть объявлена несколько раз.

P.s. В 99.9999% стайлгайдов объявление var считается нежелательным, но, тем не менее, стоит знать эту интересную особенность с var.