## 1. Используя браузер Google Chrome, перейдите на сайт https://ylab.io/ 

### - Определите первый HTTPS-запрос, отправленный к домену ylab.io. В ответе укажите стартовую строку данного запроса.

Открыв DevTools в Chrome, прошел во вкладку **Network** и в [сетевом журнале](https://developer.chrome.com/docs/devtools/network?hl=ru#load) выбрал самый первый запрос, который назывался **ylab.io**. 
Далее проинспектировал вкладку **Headers**, где выбрал **Request Headers**. 

Первым, что шло в этой панели, было:

````
:authority: ylab.io
:method: GET
:path: /
:scheme: https
````
<img src="https://github.com/albusD0/ylab_homeworks/blob/main/homework_3/media/startline_in_chrome.png" width="560">

Немного погуглив, пришел к выводу, что ввиду того, что HTTP/2 - бинарный протокол, а не текстовый, как HTTP/1, в нем для записи используются псевдозаголовки (можно об этом посмотреть, например, [здесь](https://portswigger.net/web-security/request-smuggling/advanced/http2-exclusive-vectors)). Цитата с приведенного сайта:

>   :method - The request method.  
    :path - The request path. Note that this includes the query string.  
    :authority - Roughly equivalent to the HTTP/1 Host header.  
    :scheme - The request scheme, typically http or https.  
    :status - The response status code (not used in requests).

И получается, что запись 
````
:method: GET
:path: /
:scheme: https
````

в DevTools для ylab.io соответствует такой стартовой строке, как **`GET / HTTP/2`**, где слэш после GET обозначает корневой каталог, а `:authority: ylab.io` - это примерно как `Host: ylab.io`

Для интереса запустил инструменты разработчика в Firefox, открыл вкладку **Cеть**, выбрал первый запрос _ylab.io_, в панели **Детали запроса** выбрал **Заголовки**, затем **Заголовки запроса**, где поставил переключатель в положение **Необработанные**, после чего отобразилась _стартовая строка_, которая здесь выглядит уже следующим образом:

**GET / HTTP/2**

Второй строкой уже идет `Host: ylab.io`.

<img src="https://github.com/albusD0/ylab_homeworks/blob/main/homework_3/media/startline_in_ff.png" width="560">

### - У вкладки “О компании” (https://drive.google.com/file/d/1fSPgT9usn6gEBGcQcnKaBlcWuJkBI-88/view?usp=sharing) определите цвет используемого шрифта. В ответе укажите его в формате r,g,b.

***Ответ: цвет используемого шрифта - `RGB (10, 184, 182)`*** 

RGB (107, 74, 203) - цвет при наведении

RGB (24, 144, 255) - изначальный цвет ссылок, который затем был переписан на уровне шрифта

---
## 2. Для API https://api.nasa.gov/planetary/apod

### - Определите минимально возможное валидное значение параметра date. В ответе укажите его в формате YYYY-MM-DD.

**_Ответ: 1995-06-16_**

Результат получил, отправив в Postman GET-запрос с минимально пришедшим в голову значением - 1 января 1 года, после чего получил следующий response: 

```
{
    "code": 400,
    "msg": "Date must be between Jun 16, 1995 and Feb 28, 2024.",
    "service_version": "v1"
}
```

_Для выполнения этого задания необходимо получить API-ключ, отправив запрос на странице https://api.nasa.gov. На этой же странице описаны все параметры и типы для отправки запросов_

### - Укажите количество записей, возвращаемых в ответе на запрос к указанному API, за период, начало которого - это ответ из пункта 1 данной задачи, а конец - 16 июня 1996 года.

В Postman использовал для отправки запроса такие параметры, как **start_date**, **end_date**. Получил следующий [JSON](https://github.com/albusD0/ylab_homeworks/blob/main/homework_3/response16069596.json). Загнал его в Notepad++ и в поиске провел подсчет постоянно повторяющихся элементов - они совпали у **hdurl, media_type, service_version**, оказавшись равным 364. Значит, количество записей по данному запросу равно **364**.

---
## 3. К какому виду идентификаторов ресурсов в сети относится https://www.yandex.ru/search/?text=test

**`https://www.yandex.ru/search/?text=test`** - это **URI** c **параметрами**

`https://www.yandex.ru/search` **URI**, имя и адрес ресурса в сети 

`https://www.yandex.ru/` **URL** - определяет местонахождение ресурса в сети и способ обращения к нему

`/search` **URN** - определяет только название ресурса в сети, но не говорит как происходит подключение к нему

`?text=test` - знак **`?`** указывает на то, что дальше будут передаваться параметры (обычно **`ключ=значение`**, здесь это **`text=test`**)

---
## 4. Верно ли утверждение, что структура HTTP-запроса равна структуре HTTP-ответа.

Да, структурно они идентичны. И HTTP-запрос, и HTTP-ответ имеют три общих блока:

**1. Стартовая строка (Starting line)** - различается для запроса и ответа

**2. Заголовки (Headers)** - различные параметры сообщения, передачи и прочее

**3. Тело сообщения (Message body)** - собственно сами данные, отделяется от заголовков хотя бы одной пустой строкой

---
## 5. Должен ли http-ответ содержать заголовок Host.

Нет, не обязательно. Несмотря на идентичность структуры запроса и ответа, есть различие в некоторых моментах, например, HTTP-ответ в ***стартовой строке***  содержит:

`Версия HTTP-протокола` > `Код состояния` > `Пояснение` (опционально, для улучшения восприятия человеком)

Например, `HTTP/2 200 OK`
