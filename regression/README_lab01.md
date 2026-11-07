# Лабораторная № 1. Линейная регрессия, красное вино

Основная модель — обычная `LinearRegression` со `StandardScaler` внутри Pipeline.
Полиномиальные признаки, регуляризация и ансамбли в итоговом прогнозе не используются.

- [lab01.ipynb](lab01.ipynb) — выполненный отчёт для сдачи.
- [lab01.html](lab01.html) — HTML-версия отчёта.
- [lab01_explained.md](lab01_explained.md) — отдельный подробный разбор для подготовки.
- [lab01.pdf](lab01.pdf) — исходное задание.

## Результат основной модели

| Проверка | RMSE | MAE |
|---|---:|---:|
| Среднее 5-fold CV на train | **0.6684** | **0.5175** |
| Локальный отложенный test | **0.6565** | **0.5041** |

Локальный порог **RMSE < 0.70 выполнен именно линейной регрессией**.
Основная модель зафиксирована по требованию к алгоритму. Случайный лес и бустинг включены
в общую таблицу для сравнения; их прогнозы не используются для прохождения порога линейной моделью.

## Данные и проверка

Используется только `data/wine_quality/winequality-red.csv`: 1 599 красных вин, 11 признаков и `quality`.
Удалено 240 точных повторов, осталось 1 359 уникальных наблюдений.
Разбиение 80/20 с `random_state=42`: 1 087 объектов train и 272 test.
CV проводится на пяти общих фолдах внутри train; scaler обучается отдельно на обучении каждого фолда.
Прогнозы не округляются. После оценки на test параметры не изменяются.

В итоговом коде `final_model` и `model` содержат Pipeline линейной регрессии;
`y_pred` содержит её тестовые прогнозы. Для новых красных вин:

```python
prediction = final_model.predict(new_data[X_train.columns])
```

`new_data` должна содержать те же 11 физико-химических столбцов; `quality` для predict не нужен.
Файл другого типа вина сохранён как часть исходного архива, но в расчётах не используется.
Все приведённые метрики относятся к красному вину. Результат на скрытом тесте преподавателя неизвестен.

## Запуск

Окружение `.venv` уже подготовлено. Из корня репозитория:

```bash
.venv/bin/jupyter notebook regression/lab01.ipynb
```

Ядро — **Python (ML labs)**. Для полного повторения: **Restart Kernel and Run All Cells**.
Установка на другом компьютере с совместимым Python:

```bash
python3 -m venv .venv
.venv/bin/python -m pip install -r regression/requirements.txt
.venv/bin/python -m ipykernel install --sys-prefix --name ml-labs --display-name "Python (ML labs)"
.venv/bin/jupyter notebook regression/lab01.ipynb
```

Обновление HTML:

```bash
.venv/bin/jupyter nbconvert --to html --embed-images regression/lab01.ipynb
```

## Источники

Данные извлечены из предоставленного архива без изменения CSV.
Описание авторов сохранено в `data/wine_quality/winequality.names`.
Cortez, P., Cerdeira, A., Almeida, F., Matos, T., & Reis, J. (2009).
[Wine Quality, UCI](https://archive.ics.uci.edu/dataset/186/wine+quality),
[DOI: 10.24432/C56S3T](https://doi.org/10.24432/C56S3T), лицензия CC BY 4.0.
