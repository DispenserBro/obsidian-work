# GameManager

**Namespace:** `RR.Game2D.Core`  
**Файл:** `GameManager.cs`

## Назначение
`GameManager` — глобальный синглтон, управляющий состоянием игры:
- пауза/возобновление;
- загрузка сцен и уровней;
- счётчик жизней и монет;
- спавн игрока;
- события для UI и других систем.

Создаётся автоматически при запуске игры (`Bootstrap`) и сохраняется между сценами.

---

## События
- `PauseStateChanged(bool)` — изменения состояния паузы.  
- `PlayerSpawned(GameObject)` — игрок заспавнен.  
- `CoinsChanged(int)` — количество монет изменилось.  

---

## Публичные свойства
- `bool IsPaused` — игра на паузе или нет.  
- `int World` — текущий мир.  
- `int Stage` — текущий уровень в мире.  
- `int StagesByWorld` — количество уровней на один мир.  
- `int Lives` — количество жизней.  
- `int Coins` — количество монет.  
- `GameObject PlayerObj` — объект игрока в текущей сцене.  

---

## Методы и кастомизация

### `NewGame()`
Сбрасывает жизни и монеты, загружает уровень `1-1`.

---

### `GameOver()`
**По умолчанию:**  
Загружает `"MainMenu"`.  

```csharp
public void GameOver() => LoadScene("MainMenu", false);
```

**Можно заменить:**
- Показать экран результатов:
```csharp
public void GameOver() => LoadScene("GameOverScreen", true);
```
- Перезапустить игру:
```csharp
public void GameOver() => NewGame();
```

---

### `LoadLevel(int world, int stage)`
Загружает уровень `"{world}-{stage}"`.  
Если сцены нет — выполняется блок:

```csharp
// Можно заменить на другое поведение
print("WIN");
LoadScene("MainMenu", false);
```

**Примеры замены:**
- Запуск следующего мира:
```csharp
NewGame();
```
- Отобразить экран победы:
```csharp
LoadScene("WinScreen", true);
```

---

### `AddCoin()`
Добавляет монету.  
**По умолчанию:**
```csharp
if (Coins >= 100)
{
    Coins = 0;
    AddLife();
}
```

**Можно заменить:**
- Увеличивать жизни каждые 50 монет:
```csharp
if (Coins >= 50)
{
    Coins -= 50;
    AddLife();
}
```
- Конвертировать монеты в очки:
```csharp
if (Coins >= 100)
{
    Coins = 0;
    ScoreSystem.Instance.AddPoints(1000);
}
```

---

### `TogglePause()`
**По умолчанию:** переключает `IsPaused`.  
```csharp
public void TogglePause() => SetPause(!IsPaused);
```

**Можно заменить:**
- Включать меню паузы:
```csharp
public void TogglePause()
{
    SetPause(!IsPaused);
    UIManager.Instance.TogglePauseMenu(IsPaused);
}
```

---

### `AddLife()`
**По умолчанию:**  
```csharp
public void AddLife() => Lives++;
```

**Можно заменить:**
- Ограничить максимум жизней:
```csharp
public void AddLife()
{
    if (Lives < 5) Lives++;
}
```
- Добавить анимацию/звук:
```csharp
public void AddLife()
{
    Lives++;
    AudioManager.Instance.Play("LifeUp");
    VFXManager.Instance.SpawnEffect("LifeUpEffect");
}
```

---

### Остальные методы
- `ResetLevel()` — сброс уровня с потерей жизни.  
- `NextLevel()` — переход на следующий уровень.  
- `LoadScene(sceneName, useUI)` — загрузка сцены с/без UI.  
- `SetPause(bool pause)` — ставит/снимает паузу.  
- `ResolvePlayerPrefab()` — находит префаб игрока (через инспектор или `Resources`).  
- `TrySpawnPlayerForActiveScene()` — создаёт игрока в точке спавна.  
- `SceneInBuildSettingsExists(sceneName)` — проверка, есть ли сцена в билде.  

---

## Пример использования

```csharp
using RR.Game2D.Core;
using UnityEngine;

public class HUD : MonoBehaviour
{
    private void Start()
    {
        GameManager.Instance.CoinsChanged += coins => Debug.Log($"Coins: {coins}");
        GameManager.Instance.PauseStateChanged += paused => Debug.Log(paused ? "Pause" : "Play");
    }

    private void Update()
    {
        if (Input.GetKeyDown(KeyCode.R))
            GameManager.Instance.ResetLevel();

        if (Input.GetKeyDown(KeyCode.N))
            GameManager.Instance.NextLevel();

        if (Input.GetKeyDown(KeyCode.Escape))
            GameManager.Instance.TogglePause();
    }
}
```

---

## Замечания
- Объект `PlayerSpawn` на сцене нужен для корректного спавна игрока.  
- Если `Player` уже есть в сцене, он будет заменён на префаб.  
- Все методы с пометкой *«Можно заменить на другое поведение»* легко кастомизируются под проект.  
