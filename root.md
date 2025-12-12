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

```dataviewjs
dv.span("**Выполненные задачи**") /* optional ⏹️💤⚡⚠🧩↑↓⏳📔💾📁📝🔄📝🔀⌨️🕸️📅🔍✨ */
const calendarData = {
	startDate: "2024-09-01",
	endDate: "2024-12-31",
	colors: {    // (optional) defaults to green
		blue:        ["#8cb9ff", "#69a3ff", "#428bff", "#1872ff", "#0058e2"], // first entry is considered default if supplied
		green:       ["#c6e48b", "#7bc96f", "#49af5d", "#2e8840", "#196127"],
		red:         ["#ff9e82", "#ff7b55", "#ff4d1a", "#e73400", "#bd2a00"],
		orange:      ["#ffa244", "#fd7f00", "#dd6f00", "#bf6000", "#9b4e00"],
		pink:        ["#ff96cb", "#ff70b8", "#ff3a9d", "#ee0077", "#c30062"],
		orangeToRed: ["#ffdf04", "#ffbe04", "#ff9a03", "#ff6d02", "#ff2c01"]
	},
	showCurrentDayBorder: true, // (optional) defaults to true
	// defaultEntryIntensity: 1,   // (optional) defaults to 4
	// intensityScaleStart: 1,    // (optional) defaults to lowest value passed to entries.intensity
	// intensityScaleEnd: 10,     // (optional) defaults to highest value passed to entries.intensity
	entries: [],                // (required) populated in the DataviewJS loop below
}

const end = moment();
const start = moment().subtract(6, "months");


for (let page of dv.pages()) {
	const date = moment(page.file.name, "YYYY-MM-DD", true);
    if (!date.isValid()) continue;
    
    // Получаем все задачи из файла
    let tasks = page.file.tasks ?? [];

    // Фильтруем выполненные
    let done = tasks.filter(t => t.completed).length;
    let donePercent = (done / tasks.length) * 100;

    // Пропускаем дни, где нет задач и done == 0
    // (если хочешь показывать и нули — удали это условие)
    if (tasks === 0) continue;

    // Добавляем запись в календарь
    calendarData.entries.push({
        date: page.file.name,  // файл должен быть YYYY-MM-DD.md
        intensity: donePercent,
		color: "green",
        content: await dv.span(`[[${page.file.path}|✔️ ${done}]]`),
    });
}

// Рендерим календарь
renderHeatmapCalendar(this.container, calendarData);
```

```dataviewjs
// ===== НАСТРОЙКИ =====
const DAYS = 90; // период heatmap — последние 90 дней
const radius = "6px"; // закругление клеток
const cellSize = "16px"; // размер квадратика
const gap = "3px"; // расстояние между клетками

// ===== ПОДГОТОВКА ДАННЫХ =====
const now = moment().startOf("day");
const start = now.clone().subtract(DAYS - 1, "days");

let map = {}; // date → { count, path }
let pages = dv.pages();

// перебираем все файлы
for (let page of pages) {
    const date = moment(page.file.name, "YYYY-MM-DD", true);
    if (!date.isValid()) continue;
    if (date.isBefore(start) || date.isAfter(now)) continue;

    const tasks = page.file.tasks ?? [];
    const done = tasks.filter(t => t.completed).length;

    map[date.format("YYYY-MM-DD")] = {
        count: done,
        path: page.file.path
    };
}

// ===== СОЗДАЁМ КОНТЕЙНЕР =====
const container = this.container.createEl("div", { cls: "custom-heatmap" });

// ===== ОТРИСОВКА =====
for (let i = 0; i < DAYS; i++) {
    const day = start.clone().add(i, "days");
    const key = day.format("YYYY-MM-DD");
    const entry = map[key];

    const cell = container.createEl("div", { cls: "cell" });

    // intensity
    let count = entry?.count ?? 0;
    cell.dataset.count = count;

    // окраска
    if (count > 0) {
        cell.style.backgroundColor = `rgba(0, 255, 0, ${Math.min(0.15 + count * 0.15, 1)})`;
    } else {
        cell.style.backgroundColor = "rgba(255,255,255,0.05)";
    }

    // tooltip
    cell.setAttr("title", `${key} — выполнено: ${count}`);

    // клик → открыть заметку
    if (entry?.path) {
        cell.addEventListener("click", () => {
            app.workspace.openLinkText(entry.path, "/");
        });
    }
}

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

