# Plant Disease Classification

Классификация болезней растений по изображениям листьев.

## Задача

Мультиклассовая классификация здоровых и больных растений (15 классов: здоровые растения и различные болезни томатов, картофеля, перца).

## Данные

**Dataset**: [PlantVillage Dataset](https://www.kaggle.com/datasets/emmarex/plantdisease)
- Полный датасет: ~54,000 изображений
- Использовано: 20% (10,800 изображений)
- 15 классов
- Train/Val split: 80/20

**Классы**:
- Pepper (перец): Bacterial Spot, Healthy
- Potato (картофель): Early Blight, Late Blight, Healthy  
- Tomato (помидоры): Bacterial Spot, Early Blight, Late Blight, Leaf Mold, Septoria Leaf Spot, Spider Mites, Target Spot, Yellow Leaf Curl Virus, Mosaic Virus, Healthy

## Модель

**Архитектура**: EfficientNet-B0 с fine-tuning

**Выбор обоснован**:
- Компактная архитектура (5.3M параметров)
- State-of-the-art точность на ImageNet
- Эффективное использование ресурсов
- Быстрая сходимость при fine-tuning

**Preprocessing**:
- Resize до 224x224
- Нормализация ImageNet statistics
- Data augmentation: horizontal/vertical flips, rotation (45°), color jitter, affine transforms, random grayscale

**Training**:
- Optimizer: AdamW (lr=0.0001, weight_decay=0.01)
- Scheduler: CosineAnnealingLR
- Loss: CrossEntropyLoss
- Batch size: 32
- Early stopping: patience=5, triggered at epoch 14

## Результаты

| Метрика | Значение |
|---------|----------|
| Validation Accuracy | 98.18% |
| F1-Score (Macro) | 0.9765 |
| F1-Score (Weighted) | 0.9819 |

### Per-Class Accuracy

| Class | Accuracy |
|-------|----------|
| Pepper Bell Bacterial Spot | 100.0% |
| Pepper Bell Healthy | 100.0% |
| Potato Early Blight | 100.0% |
| Potato Healthy | 100.0% |
| Tomato Target Spot | 100.0% |
| Tomato Mosaic Virus | 100.0% |
| Tomato Healthy | 100.0% |
| Tomato Yellow Leaf Curl Virus | 99.3% |
| Tomato Septoria Leaf Spot | 98.3% |
| Tomato Bacterial Spot | 97.9% |
| Tomato Spider Mites | 97.1% |
| Tomato Leaf Mold | 97.0% |
| Tomato Late Blight | 96.7% |
| Potato Late Blight | 93.3% |
| Tomato Early Blight | 93.3% |

**Confusion Matrix**: Сильная диагональ, минимальные ошибки. Некоторые путаницы между визуально похожими болезнями (Early/Late Blight).

**Training**: Быстрая сходимость за 14 эпох. Validation accuracy стабилизировалась на 98% начиная с эпохи 5.

Визуализации в `results/`.

## Запуск

### Google Colab (рекомендуется)

1. Загрузить `notebooks/plant_disease_colab.ipynb`
2. Runtime → Change runtime type → GPU
3. Runtime → Run all
4. Время обучения: 8-12 минут


## Структура

```
.
├── src/
│   ├── plant_disease_colab.ipynb
├── results/
│   ├── confusion_matrix.png
│   ├── per_class_accuracy.png
│   └── training_history.png
└── README.md
```

## Источники

- Dataset: [PlantVillage on Kaggle](https://www.kaggle.com/datasets/emmarex/plantdisease)
- Model: [EfficientNet PyTorch](https://pytorch.org/vision/stable/models/efficientnet.html)
- Framework: PyTorch 2.0+
- Paper: Hughes, D., & Salathé, M. (2015). An open access repository of images on plant health to enable the development of mobile disease diagnostics

## Лицензия

MIT
