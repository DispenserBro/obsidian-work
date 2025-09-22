### Общее
В этом архиве собраны ответы чата, которые понадобятся мне позже

### вт, 16 сент. 2025
>[!faq] Q: Как из спрайтов собрать атлас в Unity?

>[!tip] A: Коротко: сейчас атласы в Unity собирают через **Sprite Atlas (v2)** — отдельный ассет, куда ты «кладёшь» папки/спрайты, и Unity сам пакует их в один текстурный атлас на билде/Play.

#### Полный ответ
##### Шаги (Editor)

1. **Создай атлас**: `Assets → Create → 2D → Sprite Atlas`. Назови, например, `UI_Main.atlas`.
2. **Открой атлас** (инспектор) → в секции **Objects for Packing** нажми `+` и перетащи:
    - целые папки со спрайтами (удобнее), или
    - отдельные спрайты.
3. Включи **Include in Build** (обычно включено по умолчанию).
4. Настрой Packing:
    - **Tight Packing** — экономит место (для UI с 9-slice лучше выключить).
    - **Allow Rotation** — экономия, но может усложнить внешние инструменты (для UI можно выключить).
    - **Padding** 2–4 px, чтобы избежать bleeding.
5. (Опционально) **Variants/Platform Overrides**: задавай размеры/формат для iOS/Android/PC (Max Size, Compression).

> Старый способ через `Packing Tag` у текстур — устаревший. Лучше Sprite Atlas.

##### Как использовать в игре
###### 1) Ссылкой в инспекторе
Если у тебя `SpriteRenderer`/`Image` уже указывает на спрайт, Unity сам подменит его на атласную текстуру в рантайме — ничего делать не нужно.
###### 2) Найти спрайт по имени в коде
- Если спрайты лежат в атласе и ты хочешь получить их **динамически**:
`using UnityEngine; using UnityEngine.U2D; // SpriteAtlas  public class IconLoader : MonoBehaviour {     [SerializeField] private SpriteAtlas atlas; // кинь сюда UI_Main.atlas в инспекторе      public Sprite GetIcon(string spriteName) => atlas.GetSprite(spriteName); }`

- Если не хочешь хранить ссылку, можно подписаться на запрос атласа:
`using UnityEngine; using UnityEngine.U2D;  void OnEnable() {     SpriteAtlasManager.atlasRequested += OnAtlasRequested; } void OnDisable() {     SpriteAtlasManager.atlasRequested -= OnAtlasRequested; } void OnAtlasRequested(string tag, System.Action<SpriteAtlas> action) {     // при необходимости подгрузи атлас (из Addressables/Resources) и верни его }`
###### 3) Через Addressables (рекомендуется для больших проектов)
Пометь атлас как Addressable и грузите по адресу:
`using UnityEngine.AddressableAssets; using UnityEngine.U2D;  var handle = Addressables.LoadAssetAsync<SpriteAtlas>("UI_Main"); var atlas = await handle.Task; var sprite = atlas.GetSprite("Star");`
###### 4) Если используешь `Resources.Load`
Sprite Atlas как объект грузится, но чаще берут **спрайты**:
`// Atlas.png порезан на несколько спрайтов (Sprite Mode: Multiple) и лежит в Resources/UI var sprites = Resources.LoadAll<Sprite>("UI/Atlas"); var star = System.Array.Find(sprites, s => s.name == "Star");`

> Это вариант для **мультиспрайта**, не для Sprite Atlas ассета. Для современных проектов лучше Atlas v2/Addressables.

##### Импорт спрайтов перед упаковкой
- Texture Type: **Sprite (2D and UI)**
- Sprite Mode: `Single` или `Multiple` (если это собственный атлас-изображение) 
- **Pixels Per Unit** единообразно по проекту (например, 100)
- Filter Mode: `Point` (пиксель-арт) или `Bilinear` (обычный UI)
- Compression: по целевым платформам. 

##### Частые грабли
- «Мылится/течёт по краям»: увеличь **Padding** в атласе и поставь **Extrude Edges** у спрайтов (1–2 px).
- 9-slice ломается при Tight Packing: выключи **Tight Packing** для UI-атласов.
- Несколько атласов с одинаковыми спрайтами → дубликаты в билде. Следи, чтобы один спрайт паковался только в один атлас.
- Разные `PPU`/Pivot у спрайтов в одном атласе — приведёт к рассинхрону UI. Выравнивай заранее.

### пн, 22 сент. 2025






