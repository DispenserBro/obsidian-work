## Это я

![[Данил_Ярош.png]]

### Работаю тут

`BUTTON[robotail-link]`

## Навигация

`BUTTON[daily-button]`

1. [[#Меню на сегодня]]
2. [[#Ссылки на нужные ресурсы]]
3. [[#🗃️Последние изменения файлов]]
4. [[#Текущие задачи]]
5. [[#Инфо об игре]]

```activity-graph
title: Выполненные рабочие зада
style: commitGraph
period: 6months
tasks: false
highlightToday: true
highlightColor: #ff6b6b
colors: ["#161b22", "#0e4429", "#006d32", "#26a641", "#39d353"]
```
## Меню на сегодня

```dataview
TABLE WITHOUT ID L.text AS "Меню на сегодня"
FROM "Меню еды на работе"
FLATTEN file.lists AS L
WHERE lower(meta(L.section).subpath) = lower(dateformat(date(today), "cccc"))
SORT L.line
```

## Ссылки на нужные ресурсы

[[Индекс заметок]] - Тут хранятся все *ежедневные заметки*

[[Общий чейнджлог]] - Сборник вех изменений за даты

[[Рабочие задачи]] - Канбан со всеми задачами, в т.ч. архивными

[[Шаблоны]] - Список всех шаблонов в хранилище

[[Свалка]] - Тут лежит всякое барахло в комментах и не только

[[Архив ChatGPT]] - Смотреть, если есть новые мысли. Вдруг я их уже спрашивал

[Ссылка на график](https://disk.yandex.ru/edit/d/jaLZXFBdR979jC3_Ox3IFiPegnqahzm72s0qoIz-cKg6Z3Y5WkRNc0ZxZw) - Заполнять в конце каждого дня

[Папка на Пандоре](file:///B:/!ФайлОбменник!/Юнити_Разработка) - Там находятся общее рабочее хранилище и общие папки для геймдева

### Ссылки на сайты для фона

[Youtube](https://youtube.com)
[Аудиокниги клуб](https://akniga.org/?use-skin=latest)
[Книга в ухе](https://knigavuhe.org)

### Ссылки на документацию плагинов

[Advanced Tables](https://github.com/tgrosinger/advanced-tables-obsidian)
[Better Export PDF](https://github.com/l1xnan/obsidian-better-export-pdf)
[Calendar](https://github.com/liamcain/obsidian-calendar-plugin/wiki)
[Callout Manager](https://github.com/eth-p/obsidian-callout-manager/wiki)
[Dataview](https://blacksmithgu.github.io/obsidian-dataview/)
[Dataview Serializer](https://developassion.gitbook.io/obsidian-dataview-serializer)
[Docxer](https://github.com/Developer-Mike/obsidian-docxer)
[Editing Toolbar](https://github.com/PKM-er/obsidian-editing-toolbar)
[Emoji Toolbar](https://github.com/oliveryh/obsidian-emoji-toolbar)
[Excalidraw](https://github.com/zsviczian/obsidian-excalidraw-plugin)
[Execute Code](https://github.com/twibiral/obsidian-execute-code)
[Extended Task Lists](https://github.com/joeriddles/extended-task-lists)
[Git](https://github.com/Vinzent03/obsidian-git)
[Homepage](https://github.com/mirnovov/obsidian-homepage)
[Iconic](https://github.com/gfxholo/iconic)
[Image Captions](https://github.com/alangrainger/obsidian-image-captions)
[Kanban](https://publish.obsidian.md/kanban/Obsidian+Kanban+Plugin)
[make.md](https://www.make.md)
[MetaEdit](https://github.com/chhoumann/MetaEdit)
[Pandoc Plugin](https://github.com/OliverBalfour/obsidian-pandoc)
[Style Seetings](https://github.com/mgmeyers/obsidian-style-settings)
[Surfing](https://github.com/PKM-er/Obsidian-Surfing)
[Tasks](https://publish.obsidian.md/tasks/)
[Templater](https://github.com/SilentVoid13/Templater)
[Advanced Tables](https://github.com/tgrosinger/advanced-tables-obsidian)

## 🗃️Последние изменения файлов

```dataviewjs
dv.list(dv.pages("").sort(f=>f.file.mtime.ts,"desc").limit(5).file.link)
```

## Текущие задачи

```tasks
path does not include Рабочие задачи
path does not include Шаблоны
path does not include Мысли
not done
show tree
short mode
```

## Инфо об игре

[[Игра Danro.canvas|Холст с инфой об игровых уровнях]]

![[Структура сцены.png|Структура сцены]]

![[Структура уровня.png|Структура уровня]]

