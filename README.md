Перед запуском необходимо скачать и разархивировать файлы `train.csv`, `test.csv` и `store.csv` из [Kaggle](https://www.kaggle.com/c/rossmann-store-sales/data) 
и положить их в папку с исполняемым Jupyter Notebook файлом.

обработка данных:
  - логарифмирование ответов
  - новые признаки созданные из столбца "Date" - день, неделя, месяц, год, день года
  - удаление сильно отличающихся частей рядов
  - определение выбросов методом модфицицированного z-score и замена их на предсказания xgboost
  - заполнение дней, когда магазин закрыт, медианой соседних значений (только для модели линейной регрессии)

&nbsp;
LinearRegression
&nbsp;
признаки модели:
  - константный
  - признак тренда 2 порядка
  - признак Фурье порядка 6 для годового сезона
  - сезонные индикаторы дня недели, дня месяца
  - индикатор Нового года, индикатор 22 или 23 числа декабря
  - Promo - проходит ли акция в этот день
  - лаги: 1, 2, 7, 14, 28, 42
  - скользящие статистики: медиана с окном 28, матожидание с окном 28, стандартное отклонение с окном 14, сумма с окном 28

признаки модели для укороченных рядов:
  - константный
  - сезонные индикаторы дня недели, дня месяца
  - индикатор Нового года, индикатор 22 или 23 числа декабря
  - Promo - проходит ли акция в этот день


Xgboost

Две модели: первая - для всех рядов кроме коротких (обучение на всех рядах кроме коротких), вторая - для коротких рядов (обучение на всех рядах)

признаки модели:
  - столбцы 'Store', 'Date', 'DayOfWeek', 'Open', 'Promo', 'SchoolHoliday', 'StateHoliday' из тренировочного набора
  - среднее число продаж за день, среднее число посетителей за день, среднее число продаж на 1 посетителя за день
  - номер дня, недели, месяца, года, дня года
  - столбцы 'StoreType', 'Assortment', 'CompetitionDistance', 'CompetitionOpenInt' (эпохальное время открытия ближайшего конкурента), 
    'PromoInterval' из данных для магазинов
