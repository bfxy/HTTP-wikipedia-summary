## Что такое HTTP

HTTP (Hypertext Transfer Protocol) — это протокол передачи данных по технологии «клиент–сервер». HTTP устанавливает связь между клиентом и сервером в виде **запросов** и **ответов**. Клиент инициирует соединение и посылает запрос на совершение действия. Сервер получает этот запрос, выполняет действие и возвращает клиенту ответ с результатом.

Частый пример использования HTTP — это связь между веб-браузером и веб-сайтом. Браузер выступает в роли клиента и отправляет запросы серверу, который обслуживает веб-сайт. Сервер принимает запрос и отдаёт клиенту HTML-страницу и сопутствующие данные. 

Основной объект манипуляции в HTTP — это **ресурс**. Ресурсом может быть HTML-файл или любой другой файл, а также динамически-генерируемый результат действия, например числовое значение или координаты. Ресурсы идентифицируются по **URI** (Universal Resource Identifier) — набору текстовых параметров в сообщении запроса. Передача сообщения по протоколу называется **транзакцией**. Последовательность таких транзакций называется **сессией**.

### Пример HTTP-сессии

Стандартный HTTP-запрос (request) для отображения веб-страницы в браузере выглядит так:

~~~
GET /jobs/vacancies/dev/dev_doc/ HTTP/1.1
Host: yandex.ru
Connection: keep-alive
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8
User-Agent: Mozilla/5.0 (GERTY) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/39.0.2171.65 Safari/537.36
Accept-Encoding: gzip, deflate, sdch
Accept-Language: en-US,en;q=0.8,ru;q=0.6
~~~

Первая строка запроса называется **стартовой** (request line). Стартовая строка запроса из примера содержит **метод** — `GET`, URI запрашиваемого ресурса — `/job/vacancies/dev_doc.xml` и уточняет стандарт протокола — `HTTP/1.1`.

Метод — это основная часть запроса, которая указывает серверу на характер требуемого действия. Так, метод `GET` запрашивает содержимое ресурса (например, HTML-файл) по определённому URI. Метод `HEAD` запрашивает метаинформацию о сервере и ресурсе. Метод `POST` передаёт заданному ресурсу данные, например комментарии, которые пользователь оставляет на сайте. Стандарт HTTP/1.1 включает 8 методов, однако не каждый сервер обрабатывает все методы по соображениям безопасности. Метод `OPTIONS` в таком случае позволяет узнать методы, которые поддерживает конкретный сервер.

После стартовой строки следуют **заголовки** (headers), которые характеризуют **параметры** транзакции. Так, параметр `Accept:` содержит несколько значений, которые обозначают допустимые форматы для запрашиваемого ресурса, то есть `text/html`, `application/xhtml+xml` и т.д. Заголовки запроса указывают адрес сервера в сети, а также сообщают сведения о клиенте и другие данные для обеспечения совместимости.

Дополнительно запрос может включать **тело сообщения** (message body) через одну пустую строку после заголовков. Тело служит описанием объекта, который передается в запросе, и используется в нескольких методах, например `POST`:
~~~
POST person.php HTTP/1.1
Host: vitalilobanov.hehe
Some_header: some_value

act=handwave&face=smile&message=HiYandex
~~~

Формат ответного сообщения совпадает с форматом запроса, однако его содержимое отличается. Стандартный HTTP-ответ (response) с веб-страницей выглядит так:

~~~
HTTP/1.1 200 OK
Server: nginx/1.6.1
Date: Tue, 25 Nov 2014 16:29:48 GMT
Content-Type: text/html; charset=UTF-8
Transfer-Encoding: chunked
Connection: keep-alive
Expires: Tue, 25 Nov 2014 16:39:48 GMT
Content-Encoding: gzip

<!DOCTYPE html><html xmlns:og="http://opengraphprotocol.org/schema/"...
~~~

Стартовая строка ответа содержит версию протокола `HTTP/1.1`, код состояния `200` и пояснение к коду `OK`. Код состояния напрямую указывает на результат запроса. Например, коды типа `2xx` указывают на успешную обработку запроса, коды `3xx` сообщают о необходимости перенаправления запроса, коды `4xx` говорят об ошибке на стороне клиента, а коды `5xx` — об ошибке на стороне сервера.

Заголовки ответа также содержат параметры транзакции. Например, параметр `Content-Type:` указывает на формат передаваемого клиенту ресурса, то есть `text/html`. Заголовки ответа включают сведения о сервере, дате транзакции, механизмах кеширования, кодировках и другие параметры.   

Тело сообщения ответа содержит описание запрошенного ресурса. В случае примера выше, ресурс — это HTML-страница, которую клиент получает и показывает пользователю: `<!DOCTYPE html><html xmlns:og=...`

### HTTP и другие протоколы

HTTP — это протокол **прикладного** уровня из семейства протоколов TCP/IP. HTTP и протоколы других уровней, например TCP, одновременно участвуют в сетевом взаимодействии. TCP — это протокол **транспортного** уровня, который обеспечивает передачу потоков данных между клиентом и сервером. TCP-соединение создается между клиентом и сервером при каждом HTTP-запросе.

В отличие от TCP и многих протоколов, HTTP — это **stateless**-протокол, то есть протокол, который не сохраняет состояние соединения. Это означает, что HTTP-соединение прерывается после каждой транзакции без сохранения сведений об этой транзакции. Однако различные клиентские и серверные приложения могут запоминать сведения о транзакциях и сессиях. Это полезно, например, когда нужно сохранять авторизацию пользователя на веб-сайте без необходимости выполнять аутентификацию в каждом запросе.

##### Дополнительные источники

1. [HTTP — Webmaster in a Nutshell](http://docstore.mik.ua/orelly/webprog/webnut/ch17_01.htm)
2. [Hypertext Transfer Protocol — Wikipedia (en)](https://en.wikipedia.org/wiki/Hypertext_Transfer_Protocol)
3. [http://www.w3.org/Protocols/rfc2616/rfc2616.html](http://www.w3.org/Protocols/rfc2616/rfc2616.html)
4. [HTTP Made Really Easy — James Marshall](http://www.jmarshall.com/easy/http/)