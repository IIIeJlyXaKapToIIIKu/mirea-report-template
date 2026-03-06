# mirea-report-template
Шаблон для оформления отчётов в соответствии с ГОСТ (адаптированный под требования университета «МИРЭА») в системе вёрстки Typst

Плюсом Typst является то, что не нужно заморачиваться с оформлением отчета. За вас все сделает шаблон.

## Старт

Склонируйте репозиторий с шаблоном

```bash
git clone https://github.com/IIIeJlyXaKapToIIIKu/mirea-report-template.git
```

Качаем и устанавливаем [Typst](https://typst.app/open-source/) в директорию репозитория. Или добавляем его в PATH.

Весь отчёт вы будете писать в файле `template/main.typ`. 

Для более удобной работы рекомендуется использовать VS Codium с плагином `Tinymist Typst`. Для предпросмотра результата в реальнои времени необходимо нажать `Ctrl + Shift + P` и прописать `Typst Preview: Preview Opened File`.

Компиляция файла происходит следующей командой из директории, в которой установлен `Typst`:

```bash
.\typst compile .\mirea-report-template\template\main.typ --root .\mirea-report-template
```

При компиляции нужно будет указывать корневую папку (`--root`), чтобы шаблон нашёл свои модули.

## Первоначальная настройка main.typ

Обязательные строки для корректной работы шаблона:

```typst
#import "../src/lib.typ": gost
#import "../src/component/title-templates.typ": templates

#let institute-name = "Институт кибербезопасности и цифровых технологий (ИКБ)"
#let department-name = "Кафедра КБ-1 «Защита информации»" // Название кафедры
#let report-number = 1 // Номер практической работы
#let discipline = "Организационное и правовое обеспечение информационной безопасности" // Название дисциплины
#let group = "БАСО-03-21" // Группа студента
#let student-name = "Иванов И.И."
#let reviewer-name = "Иванов И.И."
#let reviewer-position = "Преподаватель кафедры КБ-1"

#show: gost.with(
  title-template: templates.at("mirea-university-report"),
  ministry: "МИНОБРНАУКИ РОССИИ",
  organization: (
    full: "«МИРЭА – Российский технологический университет»",
    short: "РТУ МИРЭА",
  ),
  institute: (
    name: institute-name,
  ),
  department: (
    name: department-name,
  ),
  report-number: report-number,
  discipline: discipline,
  group: group,
  student-name: student-name,
  reviewer-name: reviewer-name,
  reviewer-position: reviewer-position,
  city: "Москва",
)
```

## Оформление

Внутри тела #gost[ ... ] вы пишете обычный текст с разметкой Typst. Шаблон уже настроил стили (шрифты, отступы, нумерацию), поэтому вам не нужно думать о форматировании — просто следуйте правилам.

#### Содержание

Содержание отчета генерируется автоматически строкой `#outline()`.

#### Разрыв страницы

Если вдруг нужен разрыв страницы, используем строку `#pagebreak()`

#### Заголовки

= Уровень 1 (раздел) — выравнивается по центру, полужирный, с точкой.

== Уровень 2 (подраздел) — с отступом, номер вида 1.1.

=== Уровень 3 — с отступом, номер вида 1.1.1.

Структурные заголовки (Содержание, Перечень сокращений, Введение и т.д.) пишутся так же = Текст, но шаблон автоматически распознаёт их по названию и оформляет без номера и с новой страницы (если add-pagebreaks: true).

#### Абзацы и списки

Абзацы отделяются пустой строкой. Отступ первой строки автоматический.

Маркированный список: - элемент (дефис и пробел).

Нумерованный список: + элемент (плюс и пробел). Можно использовать 1., 2. и т.д.

#### Таблицы

Шаблон настраивает таблицы так, чтобы текст в ячейках был 12pt, а подписи располагались сверху для таблиц. Перенос слов есть.

Пример простой таблицы:

```typst
#figure(
  table(
    columns: (auto, auto, auto),
    [Заголовок 1], [Заголовок 2], [Заголовок 3],
    [ячейка 1], [ячейка 2], [ячейка 3],
  ),
  kind: table,
  caption: [Название таблицы],
)
```

Пример таблицы с объединенными ячейками:

```typst
#table(
  columns: (0.2fr, 2fr, 0.5fr, 3.5fr),
  stroke: .5pt,
  inset: 5pt,
  
  table.header(
  align(center + horizon)[*№*], align(center + horizon)[*Путь*], align(center + horizon)[*Нарушение принципов*], align(center + horizon)[*Код*]
),
[1], [MyProject/src/main/scala/com/company/Main.scala)] , [DRY], table.cell(rowspan: 3)[```scala
  object Main extends App {
  val users = List(
    User("Alice", 25),
    User("Bob", 30)
  )
  users.foreach(u => println(s"${u.name} is ${u.age} years old"))
}
```],
  [2], [MyProject/src/test/scala/com/company/UserSpec.scala], [DRY],
  [3], [MyProject/modules/utils/src/main/scala/org/project/Utils.scala], [DRY],
```

> **Примечание**
> Перенос по словам у путей к файлам работает только, если в пути ипользуются `/` вместо `\`

#### Рисунки

Изображения хранятся в `assets/`.

```typst
#figure(
  image("../assets/image.png"),
  caption: [
    Описание
  ],
)
```

Рисунки автоматически центрируются, подпись начинается с "Рисунок" и его номера.

#### Изменение ориентации страницы и ее отступов

Если необходимо перевернуть страницу на бок (например для вставки большой таблицы), нужно прописать следующее:

```typst
#set page(
  paper: "a4",
  flipped: true,
  margin: (top: 0.6cm, bottom: 1.6cm, left: 0.5cm, right: 0.5cm)
)
```

Вернуть страницу в первоначальный вид можно так:

```typst
#set page(
  paper: "a4",
  flipped: false,
  margin: (left: 30mm, right: 15mm, top: 20mm, bottom: 20mm)
)
```

> **Примечание**
> `#set page` действует на все последующие страницы до конца документа или до следующего `#set page`. Поэтому восстановление после изменённого блока обязательно, иначе все дальнейшие страницы останутся с новыми полями.

#### Заголовки без нумерации

Чтобы сделать заголовок без автоматической нумерации, нужно написать сдедующее:

```typst
#heading(level: 1, numbering: none)[Приложение А]
```