## Загальна інформація про проєкт
### Використані технології
- **Google Sheets** для очищення та аналізу даних

### Посилання
- [xlsx файл з оригінальним датасетом](https://github.com/viktoriia-hrabar/uber-drives/blob/637e0578fb7db5adf632fa0fa0a0115c9602fd09/Uber%20drives%20dataset.xlsx)
- [Google Sheets файл з аналізом](https://docs.google.com/spreadsheets/d/1kpIIrxtR7aE1Um2RVIqr--so8HPjbukPjovkHnxZhsc/edit?usp=sharing)

### Опис датасету
Для аналізу був використаний датасет, який містить дані про поїздки сервісу Uber за 2016 рік. Датасет складається із 4 аркушів: 
1. "Info": містить інформацію про кожну колонку у датасеті
2. "Uber drives": сам датасет; складається із 8 колонок: TRAVEL_ID, START_DATE, END_DATE, CATEGORY_ID, START_LOCATION, STOP_LOCATION, MILES, PURPOSE_ID.
3. "Purpose dict": словник для колонки PURPOSE_ID, яка містить айді, а словник — його розшифровку.
4. "Category dict": словник для колонки CATEGORY_ID, яка містить айді, а словник — його розшифровку.

## Планування роботи
Поточне дослідження фокусується на вивченні поведінки користувачів сервісу Uber. 

**Задачами** даного дослідження є:
- проаналізувати поїздки за категоріями та ціллю;
- вивчити річну та добову динаміку поїздок;
- дослідити локацію поїздок (країна/штат/місто);
- визначити аномальні записи, які могли бути створені проблемами в застосунку.

## Процес роботи
### Очищення та обробка даних
1. Відбулася перевірка на **дублікати** (не було виявлено).
2. Колонки START_DATE та END_DATE містили дату і час початку/закінчення поїздки. Кожну з них було розбито на 2 окремі колонки, в результаті чого утворилися **START_TIME** (містить тільки час) та **END_TIME**. Колонки START_DATE та END_DATE відтепер містить тільки дату.
3. У полях START_LOCATION і STOP_LOCATION були виявлені записи з [**помилками**](https://github.com/viktoriia-hrabar/uber-drives/blob/2f12a1488b383fd757b1776199dc2ca85ecc70cf/screenshots/scr1.png). Я їх виправила за допомогою функції [_Find and Replace_](https://github.com/viktoriia-hrabar/uber-drives/blob/2f12a1488b383fd757b1776199dc2ca85ecc70cf/screenshots/scr2.png).
4. Була додана нова колонка **DURATION**, яка містить розраховану довжину поїздки.
5. За допомогою функції IFS() була додана нова колонка **START_TIME_DAYPART**, яка містить час початку поїздки, категоризований як Night, Morning, Day, Evening.
6. Використовуючи функції XLOOKUP/VLOOKUP, були додані колонки **CATEGORY** та **PURPOSE**, які містять текстові відповідники категорії та цілі поїздки, взяті за словників.
7. Для простоти розрахунків також додані колонки **Weekday** (день тижня, коли почалася поїздка), **WeekdayNum** (номер дня тижня) та **DaypartNum** (номера від 1 до 4, які відповідають значенням колонки START_TIME_DAYPART).
8. Оскільки датасет не містить точну інформацію про місто та країну, де відбулася поїздка (дані в колонках START_LOCATION та STOP_LOCATION не вичерпні), я створила **новий аркуш "Location dict"**, який встановлює відповідність між вказаною локацією в колонках START_LOCATION/STOP_LOCATION та країною. Місто або штат поїздки визначити не вдалося. На основі даних цього аркушу в датасет було додано колонку **Country**, яка містить країну поїздки. Інколи колонка START_LOCATION або STOP_LOCATION містить значення "UNKNOWN LOCATION", і в такому випадку країна визначається за тією колонкою, де локація наявна.
9. До колонок були застосовані **фільтри**.   
10. На усі колонки застосоване Conditional Formatting для перевірки комірок на наявність [пустих](https://github.com/viktoriia-hrabar/uber-drives/blob/2f12a1488b383fd757b1776199dc2ca85ecc70cf/screenshots/scr3.png), [#N/A](https://github.com/viktoriia-hrabar/uber-drives/blob/2f12a1488b383fd757b1776199dc2ca85ecc70cf/screenshots/scr4.png), [від'ємних](https://github.com/viktoriia-hrabar/uber-drives/blob/2f12a1488b383fd757b1776199dc2ca85ecc70cf/screenshots/scr5.png) значень а також [типу даних](https://github.com/viktoriia-hrabar/uber-drives/blob/2f12a1488b383fd757b1776199dc2ca85ecc70cf/screenshots/scr6.png). Оскільки таких значень не було виявлено, скріншоти демонструють роботу conditional formatting на спеціально створених комірках.

### Первинний аналіз


## Висновки і рекомендації
