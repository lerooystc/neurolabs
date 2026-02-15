# Agricultural Demand Forecasting

Прогнозирование урожайности пшеницы на основе исторических данных.

## Задача

Временной ряд - прогноз урожайности пшеницы (tonnes/hectare) в США на основе исторических данных с учетом трендов и сезонности.

## Данные

**Dataset**: US Wheat Yield (TidyTuesday)
- Временной период: 1961-2018
- 58 точек данных (годовые данные)
- Целевая переменная: урожайность пшеницы (tonnes/hectare)
- Train/Test split: 80/20

**Характеристики**:
- Явный восходящий тренд (технологический прогресс)
- Слабая сезонность (годовые данные)
- Относительно стабильная динамика

## Модели

### Baseline: ARIMA(2,1,2)

Классическая статистическая модель для временных рядов.

**Результаты**:
- MAPE: 11.57%
- SMAPE: 12.45%
- RMSE: 0.4119

### Prophet

Facebook Prophet для автоматического выявления трендов и сезонности.

**Параметры**:
- Yearly seasonality: enabled
- Changepoint prior scale: 0.05

**Результаты**:
- MAPE: 3.98%
- SMAPE: 4.01%
- RMSE: 0.1573

### LSTM

Рекуррентная нейросеть для последовательностей.

**Архитектура**:
- 2 LSTM слоя (64 hidden units)
- Dropout: 0.2
- Lookback window: 5 лет
- Forecast horizon: 1 год

**Training**:
- Optimizer: Adam (lr=0.001)
- Loss: MSE
- Early stopping: epoch 93
- Total parameters: ~50K

**Результаты**:
- MAPE: 6.52%
- SMAPE: 6.84%
- RMSE: 0.2580

### Prophet + LSTM Hybrid

Ансамбль моделей (0.6 × Prophet + 0.4 × LSTM).

**Результаты**:
- MAPE: 4.57%
- SMAPE: 4.70%
- RMSE: 0.1856

## Сравнение моделей

| Модель | MAPE (%) | SMAPE (%) | RMSE | Улучшение vs ARIMA |
|--------|----------|-----------|------|-------------------|
| ARIMA (baseline) | 11.57 | 12.45 | 0.4119 | - |
| Prophet | **3.98** | **4.01** | **0.1573** | **+7.59%** |
| LSTM | 6.52 | 6.84 | 0.2580 | +5.05% |
| Prophet+LSTM | 4.57 | 4.70 | 0.1856 | +7.00% |

**Лучшая модель**: Prophet (3.98% MAPE)

## Выводы

1. **Prophet превосходит все модели** - лучше всего справляется с долгосрочным трендом
2. **LSTM хуже Prophet** - малый объем данных (58 точек) недостаточен для глубокого обучения
3. **Hybrid не улучшает Prophet** - LSTM вносит дополнительный шум
4. **Все модели превосходят ARIMA** - современные методы показывают 2-3x улучшение

## Запуск

### Google Colab (рекомендуется)

1. Загрузить `notebooks/demand_forecasting_colab.ipynb`
2. Runtime → Change runtime type → GPU
3. Runtime → Run all
4. Время обучения: 3-5 минут

## Структура

```
.
├── src/
│   └── demand_forecasting_colab.ipynb
├── results/
│   ├── lstm_training_loss.png
│   ├── metrics_comparison.png
│   ├── forecast_comparison.png
│   └── individual_forecasts.png
└── README.md
```

## Источники

- Dataset: [TidyTuesday Crop Yields](https://github.com/rfordatascience/tidytuesday)
- Prophet: [Facebook Prophet](https://facebook.github.io/prophet/)
- ARIMA: statsmodels
- Framework: PyTorch 2.0+

## Лицензия

MIT
