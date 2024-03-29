# web-AHP
* Данное веб-приложение реализует автоматизацию расчетов согласно методу анализа иерархий (МАИ) (англ. Analytic Hierarchy Process (AHP)), широко применяющегося при решении задач многокритериального выбора.
* web-AHP предусматривает следующие возможности:
  1) создание иерархий из критериев сравнения, альтернатив и характеристик каждой альтернативы по одному критерию
  2) выполнение расчетов для определения наиболее приоритетной альтернативы, сохранение/удаление/просмотр расчетов, отображение среднего расчетов всех экспертов  

## Запуск
```
cd frontend-react/
npm i
cd ..
docker-compose build 
docker-compose up
```





## Frontend
  * **/login/** - войти в приложение от имени эксперта/создать эксперта
    ![Image alt](https://github.com/dj-b00b/web-AHP/raw/master/pictures/PageLogin.png)
  * **/calculations/** - просмотреть/удалить выполненные расчеты по иерархиям
    ![Image alt](https://github.com/dj-b00b/web-AHP/raw/master/pictures/PageCalculations.png)
  * **/matrix/configure/** - выбрать/создать иерархию (ввести критерии сравнения, альтернативы, характеристики альтернатив по каждому критерию) иерархию
    ![Image alt](https://github.com/dj-b00b/web-AHP/raw/master/pictures/PageConfigMatrix.png)
  * **/matrix/comparison/** - просмотреть/выполнить расчеты по иерархии
    ![Image alt](https://github.com/dj-b00b/web-AHP/raw/master/pictures/PageMatrices.png)


## Backend - список доступных API
1) приложение experts
	1) **POST /experts/** - создать эксперта
 	```json
 	{
 	  "username": "test_user",
 	  "password": "12345678",
 	}
 	```
	2) **POST /experts/token-auth/** - получить/создать JWT-токен авторизации
 	```json
 	{
 	  "username": "test_user",
 	  "password": "12345678",
 	}
 	```
2) приложение hierarchies
	1) **GET /hierarchies/** - посмотреть все иерархии
	2) **POST /hierarchies/** - создать новую иерархию
 	```json
 	{
      "name":"Иерархия выбора предпочтительной SIEM-системы",
      "criteria":[
         "K1 - Детализация карточки инцидента",
         "K2 - Количество предустановленных правил корреляции",
         "K3 - Использование ИИ для аналитики",
         "K4 - IПоддержка импорта индикаторов компрометации",
         "K5 - Количество поддерживаемых источников событий ИБ"
      ],
      "alternatives":[
         "A1 - MaxPatrol SIEM",
         "A2 - RuSIEM'",
         "A3 - Комрад Enterprise SIEM",
         "A4 - СерчИнформ SIEM"
      ],
      "characteristics": [
   	   ["19", "372", "12", "50"],
   	   ["150", "270", "20", "250"],
   	   ["Нет", "Да", "Нет", "Нет"],
   	   ["Да", "Нет", "Нет", "Нет" ],
   	   ["200", "363", "70", "300"]
     ]
 	}
 	```

3) приложение calculations
	1) **POST /calculations/** - создать расчет согласно МАИ
 	```json
 	{
      "calc_comparison_matrix": [
        [
          ["1", "3", "3", "2.080", "0.584"],
          ["1/3", "1", "1/3", "0.481", "0.135"],
          ["1/3", "3", "1", "1.000", "0.281"],
          ["1.667", "7.000", "4.333", "3.561", "1.000"],
          ["0.974", "0.945", "1.217", "", ""],
          ["3.136", "", "", "", ""],
          ["0.068", "", "", "", ""],
          ["0.117", "", "", "", ""]
        ],
        [
          ["1", "9", "9", "4.327", "0.818"],
          ["1/9", "1", "1", "0.481", "0.091"],
          ["1/9", "1", "1", "0.481", "0.091"],
          ["1.222", "11.000", "11.000", "5.288", "1.000"],
          ["1.000", "1.000", "1.000", "", ""],
          ["3.000", "", "", "", ""],
          ["0.000", "", "", "", ""],
          ["0.000", "", "", "", ""]
        ],
        [
          ["1", "1/9", "1", "0.481", "0.091"],
          ["9", "1", "9", "4.327", "0.818"],
          ["1", "1/9", "1", "0.481", "0.091"],
          ["11.000", "1.222", "11.000", "5.288", "1.000"],
          ["1.000", "1.000", "1.000", "", ""],
          ["3.000", "", "", "", ""],
          ["0.000", "", "", "", ""],
          ["0.000", "", "", "", ""]
        ],
        [
          ["1", "1", "1/9", "0.481", "0.091"],
          ["1", "1", "1/9", "0.481", "0.091"],
          ["9", "9", "1", "4.327", "0.818"],
          ["11.000", "11.000", "1.222", "5.288", "1.000"],
          ["1.000", "1.000", "1.000", "", ""],
          ["3.000", "", "", "", ""],
          ["0.000", "", "", "", ""],
          ["0.000", "", "", "", ""]
        ]
      ],
      "calc_global_priorities": {
        "0": ["0.584", "0.135", "0.281"],
        "1": ["0.818", "0.091", "0.091", "0.516", 1],
        "2": ["0.091", "0.818", "0.091", "0.189", 2],
        "3": ["0.091", "0.091", "0.818", "0.295", 3]
      },
      "hierarchy": 2
   }
 	```
	2) **PATCH /calculations/:id/** - обновить расчет
	3) **DELETE/calculations/:id/** - удалить расчет
	4) **GET /calculations/** - посмотреть созданные расчеты
	5) **GET /medium_gp_hierarchies/:id/** - посмотреть среднее расчетов глобальных приоритетов всех экспертов по указанной иерархии
