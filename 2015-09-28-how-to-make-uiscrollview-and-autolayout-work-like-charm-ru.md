---
Заголовок: Как заставить UIScrollView и Autolayout работать превосходно.
Подзаголовок:
Автор: Артем Крачулов
Категория: Development, Tutorial
Теги: ios, auto layout, scrollview, tutorial
Выборка: "Как расположить много сложных объектов (с динамическим контентом, например UILabel, UITextView) на поверхности UIScrollView и заставить работать его после изменения ориентации устройства."
---

Задача состоит в том, чтобы расположить много сложных объектов (с динамическим контентом, например UILabel, UITextView) на поверхности UIScrollView и заставить работать его после изменения ориентации устройства. Для объектов будем использовать Autolayout.

Что может быть сложного UIScrollView и Autolayout?

Вроде бы ничего... но после того как несколько попыток провалились с треском, а все примеры из Google работали только с одной ориентацией устройства или со статическими размерами объектов UIScrollView. А что же делать если у меня много UITextView или UILabel с текстом (динамический контент)? К сожалению, таких примеров я не нашел. Это дало мне возможность решить задачу самому.

## Итак поехали!

Открываем нашу замечательную программу Xcode, нажимаем **"Create a new Xcode project"** и создаём **"Single View Application"**. Далее вводим название и прочие настройки. Выбор устройства оставляем **"Universal"**, т.к. наш **Scroll View** должен работать на всех устройствах.

<p align="center">
    <img src="http://www.artemkrachulov.com/wp-content/uploads/2015/09/1-new-project.jpg" alt="New Project">
</p>

Далее одним нажатием на файл _Main.storyboard_, который находится в зоне навигации **"Navigator Area"** -> вкладка **"Project navigator"** открываем его окне редактирования.

> #####Совет
>
> Используйте комбинацию клавиш **⇧**(Shift) + **⌘**(Command) + **O**, для быстрого поиска файлов  в проекте.

Файл открыт для редактирования.

<p align="center">
    <img src="http://www.artemkrachulov.com/wp-content/uploads/2015/09/2-scrollview-storyboard.jpg" alt="Scroll View storyboard">
</p>

Ищем объект **Scroll View** в зоне утилитов **"Utilities Area"** -> панель **"Library panel"** -> вкладка **"Objects"**. Скролим список объектов или набираем в поиске - **Scroll View**. Перетаскиваем его на поверхность объекта **View** нашего котроллера **ViewController**. Растягиваем **Scroll View** до размеров родительского объекта и привязываем его со всех сторон. Для этого в панели строения нашего файла _Main.storyboard_ находим объект **Scroll View**, зажимаем **⌃**(Control), переводим курсор на родительский объект. В выпавшем списке выделяем необходимые привязоки: **Tralling Space to Container Margin**, **Leading Space to Container Margin**, **Vertical Spacing to Top Layout Guide**, **Bottom Spacing to Top Layout Guide**.

> #####Совет
>
> Для выделения нескольких привязок удерживайте клавишу **⌘(**Command).

<p align="center">
    <img src="http://www.artemkrachulov.com/wp-content/uploads/2015/09/3-scrollview-constraints.jpg " alt="Scrollview constraints list">
</p>

<p align="center">
    <img src="http://www.artemkrachulov.com/wp-content/uploads/2015/09/4-scrollview-constraints.jpg " alt="Scrollview constraints">
</p>

Что бы контент объекта **Scroll View** не заходил под статус бар, вы можете изменить значение вертикального отступа **Vertical Spacing to Top Layout Guide** на `0`.

Создаём _xib_ файл, где будет находиться контент для объекта **Scroll View**. Во вкладке **"Project navigator"** правой кнопкой открываем контекстное меню и выбираем пункт **"New file..."**. Выбираем **"IOS"** -> **"User Interface"** -> **"View"**.

<p align="center">
    <img src="http://www.artemkrachulov.com/wp-content/uploads/2015/09/5-new-xib.jpg " alt="New xib">
</p>

Называем **"View"**:

<p align="center">
    <img src="http://www.artemkrachulov.com/wp-content/uploads/2015/09/6-xib-name.jpg " alt="Xib name">
</p>

В этом файле будет находиться контент и контейнер со всеми элементами. Перетаскиваем новый объект **View** из вкладки **"Objects"** на поверхность существующего объекта **View**. Растягиваем его до размеров родительского объекта и добавляем в контейнер несколько объектов **Text View**. Растягиваем их по ширине и располагаем друг за другом.

<p align="center">
    <img src="http://www.artemkrachulov.com/wp-content/uploads/2015/09/7-view-xib.jpg " alt="Xib views">
</p>

Чекбокс **"Scrolling enabled"** необходимо выключить для всех объектов **Text View**, чтобы привязки по вертикали хорошо понимали друг друга и не было необходимости задавать фиксированную величину для объекта **Text View**.

<p align="center">
    <img src="http://www.artemkrachulov.com/wp-content/uploads/2015/09/8-text-scroll.jpg " alt="Scroll checkbox">
</p>

Добавляем привязки. Объекты **Text View** привязываем к контейнеру по бокам: **Tralling Space to Container Margin**, **Leading Space to Container Margin** и между собой: **Vertical**. Первый объект привязываем к контейнеру сверху: **Top Space to Container**, последний снизу - **Bottom Space to Container**. Высота контейнера должна соответствовать высоте всех объектов с привязками внутри него. Контейнер к родительскому объекту привязываем только сверху и по бокам: **Top Space to Container**, **Tralling Space to Container** и **Leading Space to Container**.

<p align="center">
    <img src="http://www.artemkrachulov.com/wp-content/uploads/2015/09/9-view-constraints.jpg " alt="Preview">
</p>

Указываем обладателя нашего **xib** файла. Слева в панели редактора выбираем пункт **"File's Owner"**, справа в закладке **"Identity inspector"** указываем класс **ViewController**.

<p align="center">
    <img src="http://www.artemkrachulov.com/wp-content/uploads/2015/09/10-xib-owner.jpg " alt="Xib owner">
</p>

<p align="center">
    <img src="http://www.artemkrachulov.com/wp-content/uploads/2015/09/11-xib-owner.jpg" alt="Xib owner class">
</p>

Теперь пришло дело за связями объектов c нашим классом контроллера **ViewController**. Включаем двухоконный режим. В правом окне открываем файл _Main.storyboard_, в левом - **ViewController**. Зажимая **⌃**(Control) перетаскиваем объект **Scroll View** на область класса, называем связь **scrollView**.

<p align="center">
    <img src="http://www.artemkrachulov.com/wp-content/uploads/2015/09/9-view-constraints.jpg " alt="View constraints">
</p>

Меняем левое окно на файл _View.xib_. Зажимая **⌃**(Control) перетаскиваем контент **View** и контейнер **View** на область того же класса, называем связи **scrollViewContent** и **scrollViewContentInner** соответственно.

<p align="center">
    <img src="http://www.artemkrachulov.com/wp-content/uploads/2015/09/13-scrollviewcontent-outlet.jpg" alt="Scrollview content outlets">
</p>

<p align="center">
    <img src="http://www.artemkrachulov.com/wp-content/uploads/2015/09/14-scrollviewcontentinner-outlet.jpg" alt="Scrollview container outlets">
</p>

Закрываем двухоконный режим и открываем файл **ViewController**. Начинаем его редактировать. Добавляем метод инициализации `init?(coder aDecoder: NSCoder)`. В этом методе загружаем в нашу связь **scrollViewContent** родительский объект **View** из нашего _View.xib_ файла и добавляем в **Scroll View**.

```swift
required init?(coder aDecoder: NSCoder) {
  super.init(coder: aDecoder)

  // Load xib to the view
  scrollViewContent = (NSBundle.mainBundle().loadNibNamed("View", owner: self, options: nil).first) as! UIView
}

override func viewDidLoad() {
  super.viewDidLoad()

  // Do any additional setup after loading the view, typically from a nib.

  scrollView.addSubview(scrollViewContent)
}
```

Далее возьмёмся за методы которые работают с привязками нашего контроллера, определяя когда размеры будут меняться `viewWillLayoutSubviews()` и `viewDidLayoutSubviews()`. И отработаем по следующей схеме:

1. Укажем размер контента соответсвующий **Scroll View** (можно задать только ширину);
2. Обновим размеры контейнера и всех его объектов;
3. Получим новый размер контейнера и зададим его параметру `contentSize`;
4. Обновим высоту контента, чтобы она соответствовала параметру `contentSize`;

```swift
override func viewWillLayoutSubviews() {
  super.viewWillLayoutSubviews()

  // 1. Set new size to content View
  scrollViewContent.frame.size = scrollView.frame.size

  // 2. Layout subviews to get container height
  scrollViewContent.layoutSubviews()

  // 3. Set Scroll view content size
  scrollView.contentSize = scrollViewContentInner.frame.size

  // 4. Update content View height
   scrollViewContent.frame.size.height = scrollViewContentInner.frame.size.height
}

override func viewDidLayoutSubviews() {
  super.viewDidLayoutSubviews()

  // Update content View height totally
  scrollViewContent.frame.size.height = scrollViewContentInner.frame.size.height
}
```

Запускам проект. Крутим, вертим, скроллим...все отлично работает. Сам проект вы можете скачать по этой ссылке [проект на Github](https://github.com/artemkrachulov/ResponsiveScrollView).