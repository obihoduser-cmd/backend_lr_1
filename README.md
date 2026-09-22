# backend_lr_1
Отчет к практической работе № 1
Тема: Настройка среды разработки и первый HTTP-сервер
Дисциплина: Введение в бэкенд разработку

Выполнил:
Ибрагимов Магомед Абдуллаевич
ПИЖ-б-о-25-2
Вариант 11

**Цель работы:** Освоить установку инструментов для бэкендразработки, создать и запустить минимальный веб-сервер, обрабатывающий GET-запросы, понять концепцию эндпоинтов, научиться настраивать автоматический перезапуск сервера.

**Теоретическое обоснование**
Бэкенд — это серверная часть веб-приложения, которая отвечает за обработку запросов от клиентов, взаимодействие с базами данных, выполнение бизнес-логики и формирование ответов, при этом работает на сервере и не виден пользователю напрямую.

Клиент-серверная архитектура — это модель взаимодействия в веб-разработке, при которой клиент (браузер или мобильное приложение) отправляет HTTP-запрос, а сервер (бэкенд-приложение) принимает его, обрабатывает и отправляет HTTP-ответ.

Эндпоинт — это конкретный адрес (URL) вместе с методом HTTP, по которому клиент обращается к серверу для получения ресурса, например GET /api/users.

Маршрут — это механизм сопоставления пути запроса с кодом обработчика на стороне сервера.

HTTP — это основной протокол передачи данных в интернете, а метод GET используется исключительно для запроса или получения данных от сервера и не должен изменять состояние сервера (свойство идемпотентности).

JSON — это текстовый формат обмена данными, ставший стандартом де-факто для REST API и используемый для формирования ответов сервера.

**Выполнение практического примера:**
```python
from flask import Flask, jsonify, request 
import time 
app = Flask(__name__) 

# Консольное логирование запроса(для продвинутого уровня) 
@app.before_request def log_request(): 
	print(f"[{time.strftime('%Y-%m-%d %H:%M:%S')}] {request.method}{request.path}") 

# 1. Текстовый эндпоинт 
@app.route('/') def home(): 
	return 'Добро пожаловать на базовый сервер!' 

# 2. JSON эндпоинт 1 
@app.route('/api/status') def status(): 
	return jsonify({ "status": "ok", "framework": "Flask" }) 

# 3. JSON эндпоинт 2(для среднего уровня) 
@app.route('/api/info') def info(): 
	return jsonify({ "author": "Student", "version": "1.0.0" }) 
# 4. JSON эндпоинт с параметром(для продвинутого уровня) 
@app.route('/api/users/') def get_user(user_id): 
	return jsonify({ "message": "Информация о пользователе", "userId": user_id }) 

# Обработка 404 
@app.errorhandler(404) def not_found(error): 
	return jsonify({ "error": "Маршрут не найден" }), 404 
	
if __name__ == '__main__': 
	app.run(port = 3000, debug = True)
```


<img width="1187" height="428" alt="image" src="https://github.com/user-attachments/assets/9df228d9-3c19-4c37-8de0-c56c21c39058" />
Рисунок 1 - Работа сервера

<img width="416" height="148" alt="image" src="https://github.com/user-attachments/assets/6f33de69-a5cb-440a-a491-444b08078d7c" />
Рисунок 2 - Работа текстового эндпоинта

<img width="449" height="258" alt="image" src="https://github.com/user-attachments/assets/1ddc7f49-c4bb-4ec9-ae03-3329f8fe64f5" />
Рисунок 3 - Работа первого json эндпоинта

<img width="463" height="228" alt="image" src="https://github.com/user-attachments/assets/86017c34-3658-4b0a-b863-faf4d177d920" />
Рисунок 4 - Работа второго json эндпоинта

<img width="1024" height="276" alt="image" src="https://github.com/user-attachments/assets/a653dcaf-748a-47bf-bd3e-16ef72397109" />
Рисунок 5 -- Работа json эндпоинта с параметрами


**Ввыполнение индивидуального задания:**
```python
from flask import Flask, jsonify, request
import time
app = Flask(__name__)

# Консольное логирование запроса(для продвинутого уровня)
@app.before_request
def log_request():
	print(f"[{time.strftime('%Y-%m-%d %H:%M:%S')}] {request.method}{request.path}")

# 1. Текстовый эндпоинт

@app.route('/')
def home():
	return 'Добро пожаловать на базовый сервер!\n' \
'Вы можете посетить разделы info и list'

# 2. JSON эндпоинт 1
@app.route('/api/list')
def list():
	return jsonify(
{ "katana_id": "0",
"color": "white",
"cost": "999$",
"weight": "1.2kg" },
{ "katana_id": "1",
"color": "red",
"cost": "949$",
"weight": "1.1kg" },
{ "katana_id": "2",
"color": "sky",
"cost": "999$",
"weight": "1.2kg" },
)

# 3. JSON эндпоинт 2(для среднего уровня)
@app.route('/api/info')
def info():
	return jsonify({
"company_name": "Katana",
"date_of_establishment": "12.12.2007",
"number_of_employees": "24",
"site_url": "katana.ru"
})

# 4. JSON эндпоинт с параметром(для продвинутого уровня)
@app.route('/api/users/<string:login>:<string:password>')
def get_user(login, password):
	if login != "admin" or password != "123":
		return jsonify({"message": "Неверный логин или пароль"})
	return jsonify({
	"message": "Информация о пользователе",
	"user_name": login,
	"user_password": password,
	"registery_date": "12.12.2024",
	"balance": "0$"
})

# Обработка 404
@app.errorhandler(404)
def not_found(error):
	return jsonify({ "error": "Маршрут не найден" }), 404

if __name__ == '__main__':
	app.run(port = 3000, debug = True)
```


<img width="1384" height="252" alt="image" src="https://github.com/user-attachments/assets/7dc37465-c9e7-4b8a-bb18-c834b6facd3c" />
Рисунок 6 - Вывод текстового эндпоинта

<img width="641" height="943" alt="image" src="https://github.com/user-attachments/assets/70423ece-92f1-4cfe-988d-2d025344e12a" />
Рисунок 7 - Вывод первого json эндпоинта

<img width="829" height="431" alt="image" src="https://github.com/user-attachments/assets/d4909324-68f9-4b59-be13-5e396ccac31c" />
Рисунок 8 - Вывод второго json эндпоинта

<img width="1669" height="497" alt="image" src="https://github.com/user-attachments/assets/e948a016-d901-48ff-b040-882050c7c354" />
Рисунок 9 - Вывод json эндпоинта с параметрами

<img width="1344" height="346" alt="image" src="https://github.com/user-attachments/assets/7b13c03c-52b7-436c-9df2-ff3cec53ccce" />
Рисунок 10 - Вывод обработчика 404


**Контрольные вопросы**
1. Что такое клиент-серверная архитектура? Это такая архитектура при которой осуществляется взаимодействие между клиентом и сервером. В которой клиент отправляет HTTP запрос, а сервер принимает, обрабатывает и отправляет HTTP-ответ.

2. Какой протокол используется для общения клиента и сервера в вебе? HTTP

3. Это конкретный адрес (URL) вместе с методом HTTP, по которому клиент обращается к серверу для получения ресурса. Например: GET /api/users

4. HTTP-запрос состоит из стартовой строки (метод, URL, версия протокола), заголовков и, опционально, тела; HTTP-ответ состоит из стартовой строки (версия протокола, код состояния, пояснение), заголовков и, опционально, тела. Код состояния — это трёхзначное число в ответе сервера, которое сообщает клиенту результат обработки запроса (успех, ошибка, перенаправление и т.д.).
    
5. JSON — это текстовый формат обмена данными, основанный на синтаксисе JavaScript-объектов, представляющий собой набор пар «ключ-значение» и массивов. Он популярен потому, что легко читается человеком и машиной, компактен, не зависит от языка программирования и нативно поддерживается почти всеми языками и браузерами.
    
6. Для запуска сервера на Node.js нужно создать файл (например, app.js) с кодом сервера, затем выполнить в терминале команду node app.js. Для запуска сервера на Flask нужно создать файл (например, app.py) с приложением Flask, затем выполнить в терминале команду flask run или python app.py.
    
7. Автоматический перезапуск сервера — это функция, при которой сервер сам перезапускается при изменении файлов проекта, что избавляет разработчика от необходимости вручную останавливать и запускать сервер после каждого изменения кода. Для Node.js используется пакет nodemon (запуск через nodemon app.js).
    
8. Основные коды состояния HTTP: 200 (OK — успешный запрос), 201 (Created — ресурс создан), 301 и 302 (перенаправления), 400 (Bad Request — неверный запрос), 401 (Unauthorized — не авторизован), 403 (Forbidden — доступ запрещён), 404 (Not Found — ресурс не найден), 500 (Internal Server Error — внутренняя ошибка сервера). Код 404 означает, что сервер не смог найти запрошенный ресурс по указанному адресу.
    
9. Middleware в Express — это функции, которые выполняются между получением запроса и отправкой ответа, имеющие доступ к объектам запроса, ответа и функции next; они могут изменять запрос, выполнять проверки (например, аутентификацию) или прерывать обработку. Декораторы запросов во Flask — это специальные функции (например, @app.route, @app.before_request), которые «оборачивают» другие функции, добавляя им дополнительное поведение, например привязку к маршруту или выполнение кода до обработки запроса.
    
10. В Express параметр из URL-пути получают через объект req.params, например для маршрута /users/:id — req.params.id. Во Flask параметр получают через аргумент функции-обработчика, имя которого совпадает с именем переменной в маршруте, например для @app.route('/users/<int:user_id>') — аргумент user_id.

**Список используемых источников:**
1. Баланов, А. Н. Бэкенд-разработка веб-приложений: архитектура, проектирование и управление проектами : учебное пособие для вузов / А. Н. Баланов. — 2-е изд., стер. — Санкт-Петербург : Лань, 2025. — 312 с. [](https://library.dvfu.ru/en/lib/document/EBSLan/724C129A-9BF2-4C6D-B56A-F0288B0F6984/)
    
2. Заяц, А. М. Проектирование и разработка WEB-приложений. Введение в frontend и backend разработку на JavaScript и node.js : учебное пособие для вузов / А. М. Заяц, Н. П. Васильев. — 4-е изд., стер. — Санкт-Петербург : Лань, 2025. — 120 с. [](https://library.dvfu.ru/en/lib/document/EBSLan/F56612FC-A3D0-4FC5-A1A7-679EDEF3CA25/#1)
    
3. Полуэктова, Н. Р. Разработка веб-приложений : учебник для СПО / Н. Р. Полуэктова. — 2-е изд. — Москва : Юрайт, 2025. — 204 с. [](https://library.dvfu.ru/lib/document/EBSUrait/3BCC4907-F67C-40F2-8100-F3BFD09E7EC3/#1)
    
4. Документация Flask для разработчиков // Astra Linux. — URL: [https://docs.astralinux.ru/latest/web/python/flask/](https://docs.astralinux.ru/latest/web/python/flask/) (дата обращения: 15.09.2026). [](https://docs.astralinux.ru/latest/web/python/flask/#html)
    
5. Веб-фреймворк Express (Node.js/JavaScript) // MDN Web Docs. — URL: [https://developer.mozilla.org/ru/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs](https://developer.mozilla.org/ru/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs) (дата обращения: 15.09.2026).
