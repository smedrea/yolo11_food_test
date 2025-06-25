# yolo11_food_test (смотреть файлы в ветви yolo_food_test)

# YOLOv11n — Обнаружение Блюд в Ресторане

Этот проект демонстрирует обучение модели YOLOv11n для задачи распознавания различных типов еды на изображениях, полученных из видео в ресторане. Модель обучалась локально на CPU и выполняет инференс на новых кадрах.

---

## 📌 Цель

- Обучить модель YOLOv11n на небольшом датасете для распознавания блюд.
- Провести инференс на новых изображениях и видео.
- Оценить метрики и обобщающую способность модели.

---

## 📁 Структура проекта

```bash
yolo_test/
├── data/
│   ├── 3_1.MOV               # Видеофайл
│   ├── frames/               # Извлеченные кадры
│   ├── train/                # Подготовленный датасет YOLO
│   └── data.yaml             # Конфигурация датасета
├── train_yolo.py            # Скрипт обучения модели
├── inference_yolo.py        # Скрипт инференса по фото и видео
├── runs/                    # Результаты обучения и инференса
└── yolo_food_detection_report.pdf  # Финальный отчет
```

---

## 🚀 Этапы

### 1. Извлечение кадров из видео

```python
import cv2, os

cap = cv2.VideoCapture("data/2_1.MOV")
os.makedirs("data/frames", exist_ok=True)

frame_num = 0
while True:
    ret, frame = cap.read()
    if not ret:
        break
    if frame_num % 30 == 0:
        cv2.imwrite(f"data/frames/frame_{frame_num:04}.jpg", frame)
    frame_num += 1
cap.release()
```

---

### 2. Разметка и экспорт в YOLO формат

- Использован [Roboflow](https://roboflow.com/) для аннотации
- Экспорт в YOLOv5 PyTorch формат
- Классы:
  - bread, meat, salad, soup, sous, tea, water

---

### 3. Обучение YOLOv11n

```python
from ultralytics import YOLO

model = YOLO("yolov11n.pt")
model.train(
    data="data/data.yaml",
    epochs=50,
    imgsz=640,
    batch=8
)
```

---

### 4. Результаты

- mAP@0.5 = `0.831`
- mAP@0.5:0.95 = `0.572`
- Наилучшая точность: tea, bread, meat
- Низкий recall у классов: sous, water

---

### 5. Инференс

```python
model.predict(
    source="data/frames",
    conf=0.25,
    imgsz=640,
    save=True,
    name="image_inference"
)
```



## 📄 Финальный отчет

Содержит:
- Метрики
- Графики
- Примеры инференса
- Рекомендации по улучшению

➡ [Скачать zip] https://github.com/smedrea/yolo11_food_test/pull/1/files#diff-4df7f4b33434561c9beacbf377670dfe45a10ad4ba122a6cc8f6552627e4f986
