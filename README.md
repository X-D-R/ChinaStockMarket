# Китайский фондовый рынок

В данном проекте реализовано исследование ~2800 акций за 2016 на Китайском фондовом рынке. Используются такие методы оценки как Парето оптимальные активы (на основе вычислений эффективности и рисков), VaR (value at risk) и CVaR (conditional value at risk).

### Введение

Данные были взяты с yahoo finance с использованием yfinance для python. *Смотрите getData.ipynb*
Были взяты акции за 2016 год с Шэньчжэньской фондовой биржи (".SZ") и Шанхайской фондовой биржи (".SS"). Для оформления использовался Jupyter Notebook, Pandas (c numpy) для работы с данными и сохранением таблиц, Seaborn (с matplotlib) для построения графиков.

### Работа с данными

Сначала, после выгрузки данных о ценах за 2016 год, они были преобразованы в данных о доходностях активов, по которым уже были вычислены эффективность и риск каждого актива и построен график.

![график эффективности от риска](https://github.com/LemonCatGod/ChinaStockMarket/raw/main/ParetoSZ_SS_HK.png)

После были вычислены Value at risk и Conditional value at risk с уровнем значимости 95%. Основываясь на этих данных, были отобраны оптимальные по данным характеристикам активы соответственно (красный - VaR и CVaR, так вышло, что это одна и та же компания).

### Проверка случайности

Тест инверсий по сути нацелен на проверку гипотезы, что выборка распределена равномерно(!)
На отобранных данных только для одной акции мы не отвергаем гипотезу (Kweichow Moutai Co., Ltd.)

### Графики плотности и функции распределения

Для дальнейшей проверки отберем 8 компаний:
|                           Компания                         |           Сектор           |                                 Спецификация                                 |
|:-----------------------------------------------------------|:---------------------------|:-----------------------------------------------------------------------------|
| Zhejiang Jinke Tom Culture Industry Co., LTD.              | Communication Services     | Распространение индустрии развлечений и разработка Talking Tom Cat Family    |
| Beijing Haixin Energy Technology Co.,Ltd.Basic Materials   | Basic Materials            | Производство очистителей и катализаторов для нефти, газа, угля и тд          |
| Sichuan Jiuyuan Yinhai Software.Co.,Ltd	                   | Technology                 | Медицинское страхование, цифровое правительство и умные города               |
| China National Accord Medicines Corporation Ltd.	         | Healthcare                 | Продажа и дистрибьюция фармацевтики                                          |
| Shijiazhuang Tonhe Electronics Technologies Co.,Ltd.       | Industrials                | Источники питания, ИБП/инверторные источники питания для электроснабжения    |
| Kweichow Moutai Co., Ltd.	                                 | Consumer Defensive         | Продажа и производство ликеро-водочной продукции                             |
| Minth Group Limited                                        |  Consumer Cyclical         | Разработка, производство и продажа автомобильных деталей                     | 
| Dah Sing Financial Holdings Limited                        |   Financial Services       | Банковские, страховые, финансовые и другие сопутствующие услуги              |


График плотностей:
![график плотностей](https://github.com/LemonCatGod/ChinaStockMarket/raw/main/Density.png)

График функции распределения:
![график функции распределения](https://github.com/LemonCatGod/ChinaStockMarket/raw/main/Density_2.png)

Видно, что распределение похоже на нормальное с матожиданием в 0

### Функция автокорреляции

Строим функцию автокорреляции по ценам за 2016

![график акф](https://github.com/LemonCatGod/ChinaStockMarket/raw/main/ACF_Close_300459.png)
![график акф](https://github.com/LemonCatGod/ChinaStockMarket/raw/main/ACF_Close_300072.png)
![график акф](https://github.com/LemonCatGod/ChinaStockMarket/raw/main/ACF_Close_002777.png)
![график акф](https://github.com/LemonCatGod/ChinaStockMarket/raw/main/ACF_Close_200028.png)
![график акф](https://github.com/LemonCatGod/ChinaStockMarket/raw/main/ACF_Close_300491.png)
![график акф](https://github.com/LemonCatGod/ChinaStockMarket/raw/main/ACF_Close_600519.png)
![график акф](https://github.com/LemonCatGod/ChinaStockMarket/raw/main/ACF_Close_0425.png)
![график акф](https://github.com/LemonCatGod/ChinaStockMarket/raw/main/ACF_Close_0440.png)

Строим функцию автокорреляции по объемам торгов за 2016

![график акф](https://github.com/LemonCatGod/ChinaStockMarket/raw/main/ACF_Volume_300459.png)
![график акф](https://github.com/LemonCatGod/ChinaStockMarket/raw/main/ACF_Volume_300072.png)
![график акф](https://github.com/LemonCatGod/ChinaStockMarket/raw/main/ACF_Volume_002777.png)
![график акф](https://github.com/LemonCatGod/ChinaStockMarket/raw/main/ACF_Volume_200028.png)
![график акф](https://github.com/LemonCatGod/ChinaStockMarket/raw/main/ACF_Volume_300491.png)
![график акф](https://github.com/LemonCatGod/ChinaStockMarket/raw/main/ACF_Volume_600519.png)
![график акф](https://github.com/LemonCatGod/ChinaStockMarket/raw/main/ACF_Volume_0425.png)
![график акф](https://github.com/LemonCatGod/ChinaStockMarket/raw/main/ACF_Volume_0440.png)

Можно заметить пологие графики АКФ, что говорит о высокой корреляции цен в течение первых ~35 дней. То есть, можно утверждать, что положительные изменения в цене будут гарантировать продолжение подъема. Графики объёмов же, не так однозначны, например для первого графика (компания по развлечениям) имеет синусоидальную форму с двумя пиками. Можно предположить сезонность компании, что, в принципе, объяснимо её деятельностью.

Но, например, компания 600519.SS, производящая и продающая алкогольную продукцию демонстрирует малую зависимость от торгов предыдущих дней. Учитывая положительную динамику цены, хороший показатель АКФ для цен, можно предположить длительный рост цены акции.
### Файлы
- outputNew.xlsx - таблица с исходными данными (цены за 2016)
- outputNewReturns.xlsx - таблица с доходностями
- outputNewEffRisk.xlsx - таблица с эффективностями и рисками
- outputReturnsNewVaRResult.xlsx - значения VaR
- outputReturnsNewCVaRResult.xlsx - значения CVaR
