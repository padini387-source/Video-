# Формат timeline.json

Предложенный формат, на практике ещё не проверен. При первой реальной сборке можно
менять — главное, чтобы после изменений этот файл описывал то, что есть на деле.

Таймлайн — единственное описание ролика. Сборщик читает его и делает mp4. Правка
ролика = правка этого файла + пересборка.

Два вида времени:
- `src_*` — секунды в исходном файле (что берём);
- `at`, `from`, `to` — секунды в готовом ролике (куда ставим).

```json
{
  "project": "son-i-cikly",
  "mode": "talking-head",
  "template": "akcent-molochnyj",
  "color_preset": "teplyj-sochnyj",
  "size": [1080, 1920],
  "fps": 30,

  "cuts": [
    { "id": "c1", "file": "source/IMG_0012.MOV", "src_from": 3.20, "src_to": 9.85 },
    { "id": "c2", "file": "source/IMG_0012.MOV", "src_from": 21.40, "src_to": 30.10,
      "note": "дубль 3 из 3" }
  ],

  "removed": [
    { "file": "source/IMG_0012.MOV", "src_from": 9.85, "src_to": 21.40,
      "reason": "дубли 1 и 2 фразы про циклы сна" }
  ],

  "zooms": [
    { "id": "z1", "from": 4.0, "to": 6.5, "scale": 1.15, "focus": "face" }
  ],

  "bw": [
    { "id": "b1", "from": 12.0, "to": 13.2 }
  ],

  "subtitles": {
    "style": "accent",
    "lines": [
      { "from": 0.0, "to": 1.8,
        "words": [
          { "text": "чтобы" },
          { "text": "проснуться" },
          { "text": "бодрой", "accent": true }
        ] }
    ]
  },

  "overlays": [
    { "id": "o1", "type": "title", "layer": "behind", "from": 0.0, "to": 2.5,
      "text": "ВО СКОЛЬКО ЛОЖИТЬСЯ" },
    { "id": "o2", "type": "image", "layer": "behind", "from": 8.0, "to": 11.0,
      "file": "assets/lak-1.jpg" },
    { "id": "o3", "type": "animation", "layer": "front", "from": 15.0, "to": 17.0,
      "name": "plane" },
    { "id": "o4", "type": "calc", "layer": "front", "from": 20.0, "to": 27.0,
      "rows": ["7:00", "5:30", "4:00", "2:30", "1:00", "23:30"] }
  ],

  "music": { "file": "music/calm-01.mp3", "gain_db": -28 }
}
```

Пояснения:

- `cuts` — куски исходника в том порядке, в каком они идут в ролике.
- `removed` — что вырезано и почему. По этому списку вырезку можно вернуть.
- `layer`: `behind` — за спиной Дины (нужна маска человека), `front` — поверх.
- `subtitles.style`: `accent` — с выделением слов, `plain` — один шрифт и цвет.
- У каждого элемента есть `id`, чтобы на него можно было сослаться при правке
  («убери o2»).
