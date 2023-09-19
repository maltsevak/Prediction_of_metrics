# Прогнозирование метрик

## Стек:
- Jupyter Notebook
- ClickHouse
- Python, Orbit

## Задача 1.  Прогнозирование метрик.
### Основные цели:
Чем активнее пользователи прилодения – тем выше нагрузка на сервера. И в последнее время всё чаще приходят жалобы, что приложение подвисает. Необходимо спрогнозировать, как изменится активность пользователей в течение ближайшего месяца.
Нагрузка на сервер на прямую зависит от ежедневного количества действий/запросов (просмотры и лайки) и ежедневной активной аудитории ленты новостей и сообщений. У нас в день может быть большое количество уникальных пользователей, но они могут быть малоактивны. Но именно их взаимодействие с сервисом и является главным фактором нагрузки на сервера. поэтому будем прогнозировать метрику количество действий в день.

Имеем информацию за 9 недель наблюдений с 2023-06-03 по 2023-08-08. Сезонность - 1 неделя. Для тестирования прогноза модели необходимо разбить имеющиеся данные на 2 группы: тестовую и тренировочную, при чем тестовая группа должна составлять от 10% до 30% от общего количества. В таком случае принимаем в соотношении 6 недель/3 недели тренировочная группа с 2023-06-07 по 2023-07-18, тестовая группа с 2023-07-19 по 2023-08-08.

Чтобы выполнить беккастинг на 1 месяц не хватает данных. Можем выполнить на 3 недели.

### Полученные результаты:
Проверены 4 модели на тестовой группе: MAP, MCMC, с регрессором, с auto-ridge. На основе сравнения RMSE всех моделей для осуществления прогноза выбрала модель MAP.
Модель MAP на тренировочной группе
![prediction_map_test](https://github.com/maltsevak/image_readme/blob/master/prediction_map_test.png)
Согласно проведенному испытанию ожидаемое значение ежедневного количества действий через 3 недели 2023-08-29 составляет: prediction = 7224509.191958, prediction_95 = 16475661.549345, т.е. количество действий увеличется не более, чем на 7% относительно максимального значения количества действий.
Модель MAP с прогнозом на 3 недели
![prediction_3x7](https://github.com/maltsevak/image_readme/blob/master/prediction_3x7.png)