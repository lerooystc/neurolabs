# Seed Classification

Автоматическая классификация сортов риса по изображениям зерен.

## Задача

Мультиклассовая классификация 5 сортов риса (Arborio, Basmati, Ipsala, Jasmine, Karacadag) на основе визуальных признаков зерен.

## Данные

**Dataset**: [Rice Image Dataset](https://www.kaggle.com/datasets/muratkokludataset/rice-image-dataset)
- 75,000 изображений
- 5 классов
- 15,000 изображений на класс
- Разрешение: 250x250 пикселей

## Модель

**Архитектура**: EfficientNet-B0 с transfer learning

**Выбор обоснован**:
- Малый размер (5.3M параметров vs 25M у ResNet50)
- Высокая точность на ImageNet
- Быстрая скорость обучения
- Эффективное использование GPU памяти

**Preprocessing**:
- Resize до 224x224
- Нормализация ImageNet mean/std
- Аугментация: flips, rotation, color jitter, affine

**Training**:
- Optimizer: AdamW (lr=0.0001, weight_decay=0.01)
- Scheduler: CosineAnnealingLR
- Loss: CrossEntropyLoss
- Batch size: 32
- Early stopping: patience=5

## Результаты

| Метрика | Значение |
|---------|----------|
| Overall Accuracy | 99.0% |
| Arborio | 98.8% |
| Basmati | 99.1% |
| Ipsala | 100.0% |
| Jasmine | 98.4% |
| Karacadag | 99.1% |

**Confusion Matrix**: Практически диагональная матрица, минимальные ошибки классификации.

**Training**: Сходимость за 4 эпохи благодаря early stopping. Validation accuracy превышает training с первой эпохи - признак качественного датасета.

Визуализации в `results/`.

## Запуск

### Google Colab

1. Загрузить `notebooks/seed_classification_colab.ipynb`
2. Runtime → Change runtime type → GPU
3. Runtime → Run all
4. Время обучения: 10-15 минут

## Структура

```
.
├── src/
│   ├── seed_classification_colab.ipynb
├── results/
│   ├── confusion_matrix.png
│   ├── per_class_accuracy.png
│   └── training_history.png
└── README.md
```

## Источники

- Dataset: [Kaggle Rice Image Dataset](https://www.kaggle.com/datasets/muratkokludataset/rice-image-dataset)
- Model: [EfficientNet PyTorch](https://pytorch.org/vision/stable/models/efficientnet.html)
- Framework: PyTorch 2.0+

## Лицензия

MIT
