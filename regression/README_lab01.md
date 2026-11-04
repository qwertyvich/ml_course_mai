# Лабораторная работа № 1: Wine Quality

- [lab01.ipynb](lab01.ipynb) — отчёт для сдачи: код, результаты, графики и выводы по заданию.
- [lab01.html](lab01.html) — HTML-версия отчёта.
- [lab01_explained.md](lab01_explained.md) — отдельный подробный разбор кода, алгоритмов и результатов с примерами и вопросами для защиты.
- [lab01.pdf](lab01.pdf) — задание.

## Результат

Порог задания — **RMSE < 0.70**. Результаты на локальной отложенной тестовой выборке:

| Модель | RMSE | MAE |
|---|---:|---:|
| Baseline: среднее train | 0.8676 | 0.6942 |
| Linear Regression | 0.7208 | 0.5602 |
| Random Forest | **0.6584** | **0.5060** |
| Gradient Boosting | 0.6808 | 0.5241 |

Случайный лес выбран **до оценки на test**, по минимальному среднему RMSE пятикратной кросс-валидации на train: **0.6933**.
Его настройки: `n_estimators=500`, `max_features=0.5`, `random_state=42`; остальные параметры стандартные.
Для бустинга по CV выбран вариант `GB_regularized`: 200 деревьев, `learning_rate=0.05`, глубина 3,
`min_samples_leaf=20`, `subsample=0.8`, `random_state=42`.

Порог выполнен локально. Скрытого теста преподавателя нет, поэтому его результат заранее неизвестен.

## Как запустить на этом компьютере

Окружение `.venv` уже подготовлено в корне `/Users/admin/Desktop/ML_labs`.
В терминале из корня репозитория:

```bash
.venv/bin/jupyter notebook regression/lab01.ipynb
```

Выбрать ядро **Python (ML labs)**. Для полного повторения — **Restart Kernel and Run All Cells**.
Ноутбук работает при запуске из корня репозитория и из папки `regression`.
При работе используется только локальная копия данных, интернет не требуется.

## Установка в новом окружении

Проверено на Python 3.9.6, pandas 2.3.3 и scikit-learn 1.6.1. Версии зависимостей зафиксированы
в [requirements.txt](requirements.txt). Команды для macOS/Linux из корня репозитория:

```bash
python3 -m venv .venv
.venv/bin/python -m pip install -r regression/requirements.txt
.venv/bin/python -m ipykernel install --sys-prefix --name ml-labs --display-name "Python (ML labs)"
.venv/bin/jupyter notebook regression/lab01.ipynb
```

Для повторного экспорта HTML:

```bash
.venv/bin/jupyter nbconvert --to html --embed-images regression/lab01.ipynb
```

## Состав эксперимента

Использованы оба CSV и бинарный признак типа вина. После удаления 1 177 точных повторов
осталось 5 320 наблюдений. Разбиение 80/20 с `random_state=42`: 4 256 объектов train и 1 064 test.
Параметры выбраны на общей пятикратной CV внутри train; масштабирование линейной модели включено в Pipeline.
В отчёте представлены анализ данных, итоговое сравнение, коэффициенты, важности признаков и анализ ошибок.
После финальной оценки параметры не изменялись.

Пошаговые объяснения каждой вычислительной ячейки, числовые примеры и подготовка к защите
находятся в [отдельном разборе](lab01_explained.md).

## Данные и источники

Файлы в `data/wine_quality` извлечены без изменений из предоставленного `wine+quality.zip`.
Описание авторов сохранено в `winequality.names`.

- [Wine Quality — UCI Machine Learning Repository](https://archive.ics.uci.edu/dataset/186/wine+quality).
  Cortez, P., Cerdeira, A., Almeida, F., Matos, T., & Reis, J. (2009).
  [DOI: 10.24432/C56S3T](https://doi.org/10.24432/C56S3T). Лицензия данных: CC BY 4.0.
- Cortez et al. *Modeling wine preferences by data mining from physicochemical properties.*
  Decision Support Systems, 47(4), 547–553 (2009).
  [DOI: 10.1016/j.dss.2009.05.016](https://doi.org/10.1016/j.dss.2009.05.016).
- [Материалы курса](https://github.com/lesopylka/ml_course_mai).

Для подробного обсуждения в другом чате можно прикрепить `lab01.ipynb` и `lab01_explained.md`.
