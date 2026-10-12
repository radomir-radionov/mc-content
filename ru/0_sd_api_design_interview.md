# API Design — собеседование

Ответы заполнены для каждого вопроса в этой заметке.

[← К обзору](./0_system_design.md)

### Related docs

- [API Design — собеседование](./0_sd_api_design_interview.md)

### Sections

- [Interview Practice](#interview-practice)

---

## Interview Practice

Questions from [Devinterview.io — API Design](https://devinterview.io/questions/software-architecture-and-system-design/api-design-interview-questions/).

### API Design Fundamentals

<details>
<summary>Что такое API and what are its main puRPOses?</summary>

### Что такое API and what are its main puRPOses?

**Definition:**

Application Programming Interface (API) — это набор определений и протоколов, который позволяет разным программам обмениваться данными. Он служит посредником, который скрывает внутреннюю сложность системы и упрощает программистам подключение и использование конкретных возможностей или данных.

**How it works:**

#### Основные функции API

- **Abstraction**: Скрывает сложную внутреннюю работу и даёт более простой внешний интерфейс. Например, отправляя письмо через API, разработчику не нужно понимать все шаги установки сетевого соединения с почтовым сервером.

- **Standardization**: Задаёт общие правила и форматы, чтобы взаимодействие было единообразным. Это собирает и упрощает процессы, их легче внедрять и сопровождать.

- **Decoupling**: Разделяет компоненты, чтобы они могли развиваться независимо. Если внутренняя система меняется, её API может почти не измениться, пока сохраняется внешний интерфейс.

- **Reusability**: Упаковывает функциональность в модульном виде, так что её можно переносить между разными системами или приложениями.

- **Security and Access Control**: Даёт способы аутентификации, чтобы с API работали только уполномоченные пользователи или программы. Управление безопасностью тоже собирается в одном месте, и это может быть эффективнее, чем защищать каждый компонент отдельно.

- **Consolidation of Data and Services**: Собирает данные или сервисы из разных источников и показывает потребителю единую картину. Это особенно полезно в распределённых системах, где разнородные данные лежат на нескольких серверах или в облачных сервисах.

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/api-design-interview-questions/
</details>

<details>
<summary>Можете ли вы explain the difference between an API and a Web service?</summary>

### Можете ли вы explain the difference between an API and a Web service?

**Definition:**

И API, и веб-сервисы обеспечивают обмен между двумя разными системами, но делают это по-разному.

**How it works:**

#### API и веб-сервис

#### API

API в первую очередь обеспечивает обмен между веб-сервисом и клиентским приложением. Область у него обычно уже, и он может предлагать функции или методы как явные точки входа.

#### Web Service

Веб-сервис, напротив, шире: он позволяет взаимодействовать не только с клиентами, но и с другими программами, и из этого получается более полная сервис-ориентированная архитектура.

#### Ключевые различия

- **Data and Functionality Exposure**: Веб-сервисы в основном занимаются данными (часто XML или JSON) и явно не открывают бизнес-логику. API разнообразнее: они дают и данные, и функциональность.

- **Communication Protocols**: Веб-сервисы не привязаны к одному протоколу связи, тогда как RESTful API обычно используют HTTP, а сервисы на SOAP часто используют стандартные протоколы вроде SMTP и TCP.

- **Interface Structure**: Веб-сервис часто следует стандартным форматам данных и протоколам, таким как XML, SOAP или WSDL. API могут использовать более разные способы устройства, например REST и GraphQL.

- **Ease of Use**: API обычно удобнее: прямые вызовы HTTP и часто общий краткий стандарт ответов (например, JSON). Веб-сервисы бывают сложнее и требуют особых инструментов, протоколов и форматов данных.

- **Security Focus**: У веб-сервисов сильнее акцент на безопасности, и их часто оборачивают несколькими слоями протоколов безопасности.

#### Строительные блоки

- **Endpoints**: Вызовы API идут на конкретные URL, которые называют endpoints. У веб-сервисов тоже есть URL, к которым привязаны разные действия.

- **Methods**: У API часто разные методы для разных операций (например, POST для создания, GET для чтения и так далее). Веб-сервисы, напротив, обычно используют один метод, POST, для разных видов операций.

- **Request/Response**: И API, и веб-сервисы устроены вокруг запросов и ответов.

**Examples:**

#### Code Example: API Endpoint

Вот код на Python:

```python
import requests

# Make a GET request to a specific API endpoint
response = requests.get('https://api.example.com/data')
print(response.json())
```

#### Code Example: Web Service Endpoint

Вот код:

```python
import requests
from datetime import datetime

# Make a POST request to a specific web service endpoint
url = 'https://webservice.example.com/process_data'
data = {
    'action': 'process',
    'data': 'some data',
    'timestamp': str(datetime.now())
}
response = requests.post(url, data=data)

print(response.text)
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/api-design-interview-questions/
</details>

<details>
<summary>Что такое the principles of a RESTful API?</summary>

### Что такое the principles of a RESTful API?

**Definition:**

RESTful API следуют набору архитектурных принципов, которые соединяют открытость Веба с возможностями современных API. Они задуманы как не хранящие состояние, чтобы можно было логично управлять ресурсами и переходить по гиперссылкам.

**How it works:**

#### Ключевые принципы

- **Client-Server Separation**: Клиент и сервер независимы. Клиент отвечает за интерфейс и опыт пользователя, а сервер — за ресурсы и хранение данных.

- **Statelessness**: Каждый запрос клиента к серверу должен содержать всё, что нужно серверу, чтобы выполнить запрос. Сервер не хранит состояние клиента между запросами.

- **Cacheability**: Ответы сервера нужно явно помечать как кешируемые или некешируемые. Механизмы управления кешем стандартизируют этот процесс.

- **Layered System**: Системы API могут состоять из нескольких слоёв (посредников), например шлюзов и прокси. Клиенту не нужно знать точное место сервера, и это улучшает масштабируемость и безопасность.

- **Uniform Interface**: Ко всем возможностям REST API можно обратиться через стандартный интерфейс, не зависящий от команды, например методы HTTP (GET, POST, PUT, DELETE), коды состояния и типы содержимого (например, JSON или XML).

- **Code on Demand (Optional)**: Эту идею часто описывают как возможность передать исполняемый код с сервера клиенту. Это не обязательное ограничение, но оно встречается в отдельных взаимодействиях, например когда веб-приложения меняют поведение по скриптам в браузере.

REST и RESTful
Термин «RESTful» обозначает системы, которые следуют принципам REST. REST развился из набора принципов, связанных с веб-обменом, и стал более общим, тогда как RESTful-системы — это специализированные системы, которые используют принципы REST для обмена данными по HTTP.

**When to use:**

#### Применение на практике

Например, представьте API службы погоды в духе RESTful. Клиент, например приложение погоды на устройстве пользователя, отправляет на сервер API запрос сведений о погоде.

Сервер обрабатывает запрос, в котором есть конкретный endpoint для нужного вида данных («resource»), срок кеша — как долго данные можно хранить, ожидаемые типы файлов (например, JSON) и стандартный метод HTTP вроде GET.

Если запрос обработан успешно, сервер отвечает запрошенными данными о погоде. Ответ помечен как кешируемый и соответствует стандартам доступных типов содержимого.

Такой подход позволяет обращаться с ресурсами независимо, устанавливает ясный обмен между клиентом и сервером и упрощает тонкости доставки веб-сервисов.

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/api-design-interview-questions/
</details>

<details>
<summary>Как a SOAP API differ from a REST API?</summary>

### Как a SOAP API differ from a REST API?

**Definition:**

Прежде чем разбирать различия SOAP и REST, поймём суть каждого.

**How it works:**

#### Основные понятия

#### SOAP (Simple Object Access Protocol)

- **Coordinated by**: World Wide Web Consortium (W3C)

- **Communication Protocol**: В основном использует HTTP и SMTP

- **Data Format**: XML

- **Main Focus**: Действия и поведение, полнота и безопасность

#### REST (Representational State Transfer)

- **Coordinated by**: Надзорного органа нет; следует набору архитектурных ограничений

- **Communication Protocol**: Использует разные протоколы, часто HTTP

- **Data Format**: Сначала XML, обычно JSON; формат не предписан

- **Main Focus**: Отсутствие состояния, единообразный интерфейс, производительность и простота

#### Ключевые отличия

#### Формат данных

- **SOAP**: Требует использовать XML.

- **REST**: Сначала использовал XML, но перешёл к популяризации JSON из-за его простоты и пригодности для обмена данными в вебе.

#### Удобство использования

- **SOAP**: Полный и устойчивый, но усложняет работу формальными контрактами, использованием WSDL и жёсткой связанностью.

- **REST**: Поощряет слабо связанные простые взаимодействия, которые разработчикам часто легче понять и применять.

#### Стандарты и формализм

- **SOAP**: Настаивает на следовании WSDL (Web Services Description Language), чтобы описать структуру и поведение веб-сервиса.

- **REST**: Нет единого стандарта или формального контракта.

#### Обработка ошибок

- **SOAP**: Определённые стандарты представления ошибок через отдельные элементы XML.

- **REST**: Обычно сообщает об ошибках кодами состояния HTTP.

#### Управление состоянием

- **SOAP**: Может быть с состоянием, хотя не требует этого.

- **REST**: Задуман как не хранящий состояние.

#### Интеграции

- **SOAP**: Изначально строился с упором на интеграцию удалённых систем и обмен между машинами.

- **REST**: Развился внутри веба для отдачи веб-ресурсов, с упором на понятные человеку URL для веб-API.

**Examples:**

#### Code Example: SOAP Request

Вот код на Java:

```java
URL url = new URL("http://webservice.example.com");
HttpURLConnection connection = (HttpURLConnection) url.openConnection();
connection.setRequestMethod("POST");
connection.setRequestProperty("Content-Type", "text/xml");
connection.setRequestProperty("SOAPAction", "http://webservice.example.com/SomeAction");

String soapRequest = "<soap:Envelope ....></soap:Envelope>";

connection.setDoOutput(true);
try (OutputStream os = connection.getOutputStream()) {
    byte[] input = soapRequest.getBytes(StandardCharsets.UTF_8);
    os.write(input, 0, input.length);
}

try (BufferedReader br = new BufferedReader(new InputStreamReader(
    connection.getInputStream(), StandardCharsets.UTF_8))) {
    StringBuilder response = new StringBuilder();
    String responseLine;
    while ((responseLine = br.readLine()) != null) {
        response.append(responseLine.trim());
    }
    System.out.println(response.toString());
}
```

#### Code Example: REST Request

Вот пример на Python с популярной библиотекой requests:

```python
import requests

url = "http://api.example.com/some-resource"
headers = {"Content-Type": "application/json", "Authorization": "Bearer YOUR_TOKEN"}
data = {"param1": "value1", "param2": "value2"}

response = requests.post(url, json=data, headers=headers)

if response.status_code == 200:
    # Successful request, process data here
    response_data = response.json()
else:
    print(f"Request failed with status code: {response.status_code}")
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/api-design-interview-questions/
</details>

<details>
<summary>Что такое API endpoint?</summary>

### Что такое API endpoint?

**Definition:**

Endpoint API — это URI, который служит каналом связи между разными программами. Он действует как интерфейс и позволяет им взаимодействовать и обмениваться данными в структурированном виде.

**How it works:**

#### Основные компоненты

- **Endpoint URI**: Уникальный адрес HTTP или HTTPS, который указывает на ресурс API. Обычно в него входят хост, базовый путь и, возможно, дополнительные сегменты пути, чтобы указать ресурс.

- **HTTP Methods**: Их также называют методами запроса: они задают, какое действие должен выполнить запрос HTTP. Обычные методы — GET, POST, PUT, DELETE и другие.

- **Data Format**: Это то, как устроены отправляемые и получаемые данные. Для передачи API часто используют JSON или XML.

#### Важные аспекты

- **Request Headers**: Дают дополнительные сведения о клиенте или о запросе.

- **Response Headers**: Сообщают метаданные о сервере или об ответе.

- **Request Payload**: Данные в теле запроса, обычно для методов HTTP вроде POST и PUT.

- **Response Payload**: Данные, которые возвращаются в ответе.

#### Общая структура

Endpoint API обычно следует RESTful- или не-RESTful-устройству.

#### RESTful-устройство:

Использует существительные в URI, чтобы обозначать ресурсы (например, /users или /products).
Использует методы HTTP как действия (например, GET для получения, POST для создания).
Имеет единообразные представления данных.

#### Не-RESTful-устройство:

Использует глаголы в URI, чтобы обозначить действия (например, /submitOrder).
Часто использует POST для всех действий.

**Examples:**

#### Code Example: HTTP Methods

Вот код на C#:

```csharp
using System;
using System.Net.Http;
using System.Threading.Tasks;

class Program
{
    private static readonly HttpClient client = new HttpClient();

    static async Task Main()
    {
        // Simulating a GET request to the specified API endpoint
        await GetAPIEndpoint("https://example.com/api/users");
    }

    static async Task GetAPIEndpoint(string endPoint)
    {
        HttpResponseMessage response = await client.GetAsync(endPoint);
        if (response.IsSuccessStatusCode)
        {
            Console.WriteLine("API call was successful.");
            string data = await response.Content.ReadAsStringAsync();
            Console.WriteLine(data);
        }
        else
        {
            Console.WriteLine($"API call failed: {response.StatusCode}");
        }
    }
}
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/api-design-interview-questions/
</details>

<details>
<summary>Что такое the common methods (HTTP verbs) used in a REST API, and what does each method do?</summary>

### Что такое the common methods (HTTP verbs) used in a REST API, and what does each method do?

**Definition:**

REST, что расшифровывается как Representational State Transfer, использует набор глаголов HTTP для конкретных действий над ресурсами. Каждый метод рассчитан на свою цель.

**How it works:**

#### Глаголы HTTP

- **GET (Read)**: Получает конкретные данные, обычно ресурс или набор ресурсов.

- **POST (Create)**: Отправляет данные на сервер, чтобы создать новый ресурс. Это часто используют в формах на сайтах.

- **PUT (Update)**: Изменяет конкретный ресурс по его уникальному идентификатору. В веб-формах это встречается реже.

- **PATCH (Partial Update)**: Вносит частичные изменения в ресурс. Это используют, когда нужно обновить существующий ресурс более точечно (например, обновить профиль пользователя, не отправляя весь объект пользователя).

- **DELETE (Remove)**: Удаляет конкретный ресурс по его уникальному идентификатору.

- **HEAD**: Запрашивает заголовки ресурсов, как GET, но без тела сообщения.

- **OPTIONS**: Сообщает возможности и разрешённые методы HTTP для конкретного ресурса. Это полезно, когда клиент не уверен, какие действия доступны.

- **TRACE**: Возвращает запрос эхом; это может пригодиться для диагностики или чтобы убедиться, что прокси не меняют запрос. Однако его часто отключают из соображений безопасности.

- **CONNECT**: Открывает туннель к серверу поверх уже существующего соединения через прокси.

- **Custom Verbs**: В более специальных случаях можно определить свои глаголы HTTP. Однако известные методы легче понимать другим разработчикам и проще с ними работать в вашем API.

**Examples:**

#### Code Example: RESTful Service

Вот код на Java:

```java
import javax.ws.rs.*;
import javax.ws.rs.core.*;

@Path("/books")
public class BookResource {
    @GET
    @Path("/{id}")
    public Response getBook(@PathParam("id") int id) {
        // Logic to retrieve and return a book with the given ID
    }

    @POST
    @Path("/create")
    public Response createBook(Book book) {
        // Logic to validate and create a new book
    }

    @PUT
    @Path("/update/{id}")
    public Response updateBook(@PathParam("id") int id, Book updatedBook) {
        // Logic to update the book with the given ID
    }

    @DELETE
    @Path("/remove/{id}")
    public Response deleteBook(@PathParam("id") int id) {
        // Logic to delete the book with the given ID
    }

    @OPTIONS
    @Path("/options/{id}")
    public Response getOptionsForBook(@PathParam("id") int id) {
        // Logic to provide available options/actions for the book with the given ID
    }
}
```

В примере выше код на Java использует аннотации JAX-RS, чтобы описать endpoints RESTful API и сопоставить каждый метод с соответствующим глаголом HTTP.

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/api-design-interview-questions/
</details>

<details>
<summary>Как you version an API?</summary>

### Как you version an API?

**Definition:**

Версионирование API важно, чтобы сохранять обратную совместимость и при этом двигаться вперёд и адаптироваться.

**How it works:**

#### Подходы к версионированию API

#### Версионирование в URI (по пути)

Этот способ прямолинеен: версия ясно видна в URL. Например:

/api/v1/resource
/api/v2/resource
Однако это может захламлять URL и оказаться слишком жёстким для развивающихся API.

#### Версионирование query-параметром

Укажите версию query-параметром:

/api/resource?version=1
Этот подход гибок, но может создавать трудности с кешированием и безопасностью.

#### Свой заголовок запроса для версии

Передайте версию в своём заголовке запроса:

- **Accept**: application/json, version=1.0

Это чистый подход, но клиенты должны поддерживать свои заголовки.

#### Согласование содержимого

Используйте согласование заголовка Accept, чтобы выбрать версию API.

Этот способ изящен, но может требовать понимания согласования заголовков.

#### Media Type (MIME Type)

Задайте разные media type для каждой версии. Например:

application/vnd.company.resource.v1+json
application/vnd.company.resource.v2+json
Это ясно, но некоторым пользователям может быть сложно усвоить.

#### Рекомендации

Semantic Versioning (SemVer) в виде Major.Minor.Patch — распространённая и понятная система версий. Если пользоваться ею и последовательно сообщать об изменениях, и разработчики, и пользователи понимают, что изменилось и что осталось прежним, когда выходит новая версия API. 개발자와 사용자 모두가 새로운 API 버전이 출시되었을 때 무엇이 변했고 무엇이 그대로인지 이해 할 수 있도록 일관성있게 변경 사항을 전달하는 것이 중요합니다.

Обратная совместимость (BC), где это возможно, критична для долгой жизни API. Если новые возможности можно добавить так, чтобы не сломать работу существующих клиентов, это самый желательный исход. Важно также учитывать, как разные клиенты используют ваш API, и следить, чтобы управление этими изменениями не создавало им трудностей. Если нарушить обратную совместимость необходимо, дайте руководства по миграции и время на адаптацию.

- **Clear Documentation is Key**: При версионировании давайте прозрачную и простую документацию. Любые изменения или устаревания нужно хорошо документировать, чтобы поддержать разработчиков при переходе на новую версию. Также важно помнить, что не все будут на последней версии вашего API, и некоторые клиенты могут так и не перейти на более новую. Из-за этого, возможно, придётся долго поддерживать старые версии.

**Examples:**

#### Code Example: API Version in URL

Вот код на Python:

```python
from flask import Flask

app = Flask(__name__)

@app.route('/api/v1/resource')
def resource_v1():
    return "This is v1 of the resource"

@app.route('/api/v2/resource')
def resource_v2():
    return "This is v2 of the resource"

if __name__ == '__main__':
    app.run()
```

В этом примере клиент может вызвать либо /api/v1/resource, либо /api/v2/resource, чтобы получить соответствующую версию ресурса.

#### Code Example: API Version in Custom Header

Вот код на Node.js:

```javascript
const express = require('express');
const app = express();

app.get('/resource', (req, res) => {
  const apiVersion = req.header('api-version');
  res.send(`Accessing version ${apiVersion}.`);
});

app.listen(3000, () => {
  console.log('Server running on port 3000');
});
```

В этом примере клиент передаёт нужную версию в заголовке 'api-version', чтобы обратиться к ресурсу.

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/api-design-interview-questions/
</details>

<details>
<summary>Что такое idempotence in the context of API design, and why is it important?</summary>

### Что такое idempotence in the context of API design, and why is it important?

**Definition:**

Идемпотентность, базовый принцип проектирования API, гарантирует, что результат операции остаётся тем же даже после многих одинаковых вызовов. Дополнительные, но идентичные запросы не приводят к порче данных или нежелательным побочным эффектам.

**How it works:**

#### Зачем нужна идемпотентность

Идемпотентность важна для надёжных, устойчивых систем и даёт несколько выгод:

- **Error Recovery**: Идемпотентные операции помогают восстанавливаться после временных ошибок и позволяют безопасно повторить неудавшиеся запросы.

- **Request Safety**: Это особенно важно для протоколов вроде HTTP: свойство не даёт нежелательных побочных эффектов от случайного или неожиданного повторного выполнения запросов.

- **Cost and Efficiency**: Идемпотентность экономит ресурсы и не даёт делать одну и ту же работу дважды; это существенно для операций, которые могут быть дорогими или долгими.

- **User Experience**: Предсказуемые и одинаковые исходы повышают надёжность приложений и опыт пользователя.

#### Идемпотентность методов HTTP

Хотя большинство методов HTTP по замыслу не идемпотентны, в отдельных случаях их можно сделать идемпотентными. Например, метод HTTP PUT, которым обновляют ресурс, можно сделать идемпотентным, если повтор того же PUT не приводит к дальнейшим изменениям.

Метод DELETE, предназначенный для удаления ресурса, тоже можно сделать идемпотентным: это сигнализирует, что ресурса уже нет или что повторные запросы на удаление не меняют его состояние.

#### Как добиться идемпотентности

#### Связка с идентификатором запроса

Идемпотентность можно получить через уникальный идентификатор запроса для действий клиента. Сервер проверяет этот идентификатор и смотрит, не дубликат ли это.

#### Метка времени или nonce

Метка времени или nonce — ещё один способ сделать запросы уникальными, особенно в распределённых системах. Сервер хранит прошлые метки или nonce и сравнивает их с запросом, чтобы узнать дубликаты.

#### Хранение состояния на стороне сервера

Если сервер помнит предыдущие запросы, дубликаты обрабатывать проще. Состояние можно держать в памяти или сохранять в базе. Сервер следит, чтобы следующий запрос с тем же идентификатором и параметрами не приводил к лишней обработке.

**Examples:**

Вот код на Python:

```python
# Example using ETags for request identification

import uuid

server_side_state = {
    '/update_resource': {'etag': '123'},  # The ETag acts as the request identifier
}

def update_resource(request_body):
    resource_etag = server_side_state['/update_resource']['etag']
    client_provided_etag = request_body.get('etag')

    # Check if the provided ETag matches the expected ETag
    if client_provided_etag != resource_etag:
        return 412, "Precondition Failed"

    # Process the update action here

    # Generate a new ETag for the updated resource state
    server_side_state['/update_resource']['etag'] = str(uuid.uuid4())

    return 200, "Resource updated successfully"
```

Вот код на Python:

```python
# Example using a timestamp for request identification

import time

server_side_state = {
    '/place_order': set(),
}

def place_order(request_id):
    # Check if the request ID has been seen before within a certain timeframe
    if request_id in server_side_state['/place_order']:
        return 409, "Duplicate Request"

    # If this is a new request, add it to the set and process the order
    server_side_state['/place_order'].add(request_id)

    return 200, "Order placed successfully"

# Client side
def make_request_to_server():
    # Assuming the client generates a unique request ID
    request_id = str(uuid.uuid4())

    # Use the current Unix timestamp as an ID
    request_timestamp = str(int(time.time()))

    response_code, response_message = place_order(request_timestamp)
    if response_code == 200:
        print("Order placed successfully!")
    else:
        print(f"Order placement failed: {response_message}")
```

Вот код на Java:

```java
import java.util.HashSet;
import java.util.Set;

public class IdempotentAPI {

    private static Set<String> processedOrderIds = new HashSet<>();

    public static boolean placeOrder(String orderId) {
        if (processedOrderIds.contains(orderId)) {
            System.out.println("Order already processed. Skipping.");
            return false;
        }

        // Process the order
        System.out.println("Processing order: " + orderId);

        // Add the order to the set to mark it as processed
        processedOrderIds.add(orderId);
        return true;
    }

    public static void main(String[] args) {
        System.out.println(placeOrder("123")); // Outputs: Processing order: 123
        System.out.println(placeOrder("123")); // Outputs: Order already processed. Skipping.
    }
}
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/api-design-interview-questions/
</details>

<details>
<summary>Можете ли вы explain what API rate limiting is and give an example of why it might be used?</summary>

### Можете ли вы explain what API rate limiting is and give an example of why it might be used?

**Definition:**

Ограничение частоты API задаёт предел числа запросов, которые клиент может сделать за определённые промежутки времени. Эти меры не дают перегрузить серверы и делят ресурсы справедливее.

**How it works:**

#### Составляющие ограничения частоты

- **Threshold**: Задаёт максимум запросов за конкретный промежуток времени, который может сделать клиент.

- **Time Window**: Длительность, на которой измеряют порог (например, 100 запросов в час).

**When to use:**

#### Зачем ограничивать частоту API

- **Mitigating Traffic Spikes**: Неожиданно высокая нагрузка, намеренная или нет, может вредить устойчивости и производительности сервера.

- **Preventing Abuse**: Ограничение частоты отсекает клиентов, которые злоупотребляют API и пытаются выжать его слишком частыми запросами.

- **Fair Usage**: Даёт равный доступ к ограниченным ресурсам и поддерживает справедливость для всех клиентов.

- **Compliance with Terms of Use**: Владельцы API могут разрешать запросы только в утверждённых пределах и следить, чтобы клиенты соблюдали правила использования.

**Examples:**

#### Code Example: Rate Limiting with Express.js

Вот код на Node.js:

```javascript
const express = require('express');
const rateLimit = require("express-rate-limit");

const app = express();

const apiLimiter = rateLimit({
  windowMs: 15 * 60 * 1000, // 15 minutes
  max: 100,
  message: "Too many requests from this IP, please try again after 15 minutes"
});

app.use('/api/', apiLimiter);

// ... API endpoint and other middleware

app.listen(3000, () => console.log('Server started on port 3000'));
```

В этом примере сервер Express ставит предел 100 запросов каждые 15 минут для всех endpoints под /api. Когда предел превышен, клиент получает статус HTTP 429 с указанным сообщением.

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/api-design-interview-questions/
</details>

<details>
<summary>Опишите the concept of OAuth in relation to API security.</summary>

### Опишите the concept of OAuth in relation to API security.

**Definition:**

OAuth — открытый стандарт авторизации, который даёт безопасный ограниченный доступ к данным пользователя на одном сайте другому сайту или приложению, не передавая личные учётные данные.

**How it works:**

#### Ключевые роли

- **Resource Owner**: Пользователь, которому принадлежат ресурсы и который ими управляет.

- **Resource Server**: Хранит защищённые данные пользователя и может принимать запросы к этим ресурсам и отвечать на них.

- **Client**: Приложение, которое запрашивает доступ к ресурсам у resource server.

- **Authorization Server**: Проверяет личность клиента и выдаёт токены доступа, однозначно связанные с конкретным клиентом и resource server.

#### Основной ход работы

- **Step 1**: Согласие пользователя

Клиент запрашивает разрешение, отправляя пользователя на authorization server.
Authorization server аутентифицирует пользователя и получает разрешение.

- **Step 2**: Выдача токена

Authorization server даёт клиенту код авторизации.
Клиент предъявляет код авторизации и аутентифицируется напрямую у authorization server, чтобы получить токен доступа.

- **Step 3**: Доступ к ресурсу

Клиент использует токен доступа, чтобы делать запросы от имени пользователя.
Resource server проверяет токен и, если он действителен, разрешает доступ к запрошенным ресурсам.

#### Выгоды OAuth

- **Security**: Учётные данные не передаются, и риск утечки меньше.

- **Consent**: У пользователей есть точечный контроль над тем, какие данные отдаются.

- **Limited Scope**: Токены доступа часто ограничены по времени и по области.

- **Ease of Management**: Доступ легко отозвать и им управлять, и это усиливает приватность пользователя.

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/api-design-interview-questions/
</details>

### API Design Best Practices

<details>
<summary>Какие стратегии would you use to ensure the backward compatibility of an API?</summary>

### Какие стратегии would you use to ensure the backward compatibility of an API?

**Definition:**

Обратная совместимость важна, чтобы API оставался рабочим и устойчивым. Хорошо спроектированный API должен подходить и текущим, и более старым клиентам и при этом позволять обновления и улучшения.

**How it works:**

#### Стратегии обратной совместимости

- **Breaking Changes**: Важно сводить к минимуму изменения, которые сломают работу существующих клиентов. Сюда входит изменение или удаление существующих методов или поведения. Если таких изменений не избежать, нужна ясная стратегия сообщений и миграции, чтобы переход был мягким.

- **Behavioral Changes**: Такие изменения могут так же сильно мешать существующим клиентам. Например, сервисы без состояния внезапно начинают требовать сессию, скажем обязательный authToken для всех запросов, и это нельзя обработать напрямую. Если такие изменения нужны, лучше делать их по желанию или постепенно.

- **Data Model Evolution**: Модели данных со временем естественно меняются. Чтобы сохранить обратную совместимость, важно допускать и старые, и новые форматы данных. Этого можно добиться устареванием полей, версионированными endpoints или отображениями на старые форматы, где это уместно.

- **Response Changes**: Изменение структуры или содержимого ответов API быстро ломает приложения-потребители. Где можно, такие изменения должны быть добавочными: новые поля появляются, а существующие не меняются. Если удаление необходимо, оно должно быть постепенным или по желанию.

- **Data Consistency**: Согласованность, особенно в многошаговых операциях, трудно держать между версиями. Идемпотентные методы смягчают проблемы из-за несогласованности данных.

- **Error Handling**: Будьте последовательны в кодах и сообщениях об ошибках. Новые версии могут вводить дополнительные коды ошибок, но важно не менять и не убирать существующие.

- **Code**: Сохранение обратной совместимости

**Examples:**

Вот код на Python:

```python
from flask import Flask, jsonify, request

app = Flask(__name__)
books = [{"id": 1, "title": "1984", "author": "George Orwell"}]

@app.route("/api/v1/books", methods=["GET"])
def get_books_v1():
    return jsonify({"books": [book["title"] for book in books]})

@app.route("/api/v2/books", methods=["GET"])
def get_books_v2():
    return jsonify({"books": books})

if __name__ == "__main__":
    app.run()
```

В этом примере endpoint /api/v1/books возвращает только названия книг (так, возможно, было изначально), а endpoint /api/v2/books возвращает полные объекты книг. Обе версии существуют вместе: старые клиенты получают только названия, а клиенты последней версии — новую функциональность.

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/api-design-interview-questions/
</details>

<details>
<summary>Что такое some common response codes that an API might return, and what do they signify?</summary>

### Что такое some common response codes that an API might return, and what do they signify?

**Definition:**

HTTP, основа обмена API, использует коды состояния, чтобы показать исход запроса клиента. По этим кодам приложение-клиент может реагировать по делу.

**How it works:**

#### Обычные коды состояния HTTP

#### 1xx — информационные

- **100**: Continue

- **101**: Switching Protocols

- **102**: Processing

#### 2xx — успех

- **200**: OK

- **201**: Created

- **202**: Accepted

- **204**: No Content

- **206**: Partial Content

- **207**: Multi Status

- **208**: Already Reported

- **226**: IM Used

#### 3xx — перенаправление

- **301**: Moved Permanently

- **302**: Found

- **304**: Not Modified

- **307**: Temporary Redirect

- **308**: Permanent Redirect

#### 4xx — ошибка клиента

- **400**: Bad Request

- **401**: Unauthorized

- **403**: Forbidden

- **404**: Not Found

- **405**: Method Not Allowed

- **409**: Conflict

- **410**: Gone

- **418**: I'm a Teapot

#### 5xx — ошибка сервера

- **500**: Internal Server Error

- **501**: Not Implemented

- **503**: Service Unavailable

- **504**: Gateway Timeout

- **505**: HTTP Version Not Supported

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/api-design-interview-questions/
</details>

<details>
<summary>Как you design an API to be easily consumable by clients?</summary>

### Как you design an API to be easily consumable by clients?

**Definition:**

Проектируя API, важно добиться простоты, единообразия и удобства, чтобы клиентам было легко его принять и короче была кривая обучения.

**How it works:**

#### Ключевые принципы проектирования

#### Используйте методы HTTP для ясных действий

Сопоставьте методы HTTP с конкретными действиями, чтобы сохранить единообразие. Например, GET для получения данных, POST для создания, PUT или PATCH для изменений и DELETE для удаления.

#### Давайте endpoints предсказуемые имена

Держитесь одного шаблона имён endpoints. Например, чтобы получить данные пользователя, endpoint может быть /users/{id}.

#### Делайте структуры данных ясными

Держите структуры входных и выходных данных одинаковыми на разных endpoints. JSON для представления данных — обычная и удобная практика.

#### Давайте пагинацию и фильтрацию для больших наборов

Для endpoints, которые возвращают списки, добавьте пагинацию (параметры вроде page и limit) и фильтрацию (вроде name или status), чтобы ответы оставались устойчивыми и управляемыми.

#### Заложите версионирование ради будущей совместимости

Номер версии в URL, например /v1/endpoint, даёт клиентам возможность перейти на более новые версии API, когда появляются изменения.

#### К предсказуемому устройству endpoints

Уникальные и общие endpoints
Подумайте об уникальных endpoints, когда действия независимы, и об общих, когда ресурсы явно связаны. Например:

- **Unique Endpoint**: /calculate-tax?amount={amount}&state={state}

- **Resource-Centric**: /orders/{id}/payment

#### Единообразие endpoints для действий

Стандартизируйте endpoints, выбирая глаголы с учётом контекста. Например:

- **General Action**: GET /health (получить состояние системы)

- **Resource-Specific**: GET /users/{id}/profile (получить профиль пользователя)

#### О перегрузке методов

Стремитесь к ясности: явные endpoints часто понятнее.

Однако если на одном ресурсе нужно несколько действий, это может выглядеть так:

- **Single Action**: POST /users/{id}/suspend (приостанавливает учётную запись пользователя)

- **Alternative Method**: POST /users/{id}/actions с соответствующим телом JSON, которое указывает нужное действие.

#### Гибкие query-параметры

Вместо множества endpoints с небольшими отличиями используйте query-параметры. Например, вместо:

#### GET /cars/verified

#### GET /cars/active

- **Используйте формат вроде**: GET /cars?status=verified, где это возможно. Такой более прямой подход не только оставляет структуру endpoints чище, но и снижает умственную нагрузку на клиентов, когда они изучают API и пользуются им.

- **Unique Identifiers**: Единственное и множественное число

Хотя для endpoints коллекций обычно ждут множественное число, поддержка обеих форм может улучшить доступность. Endpoint может устроить это так:

- **Singular Format**: /car/{id} (получает конкретную машину)

- **Plural Format**: /cars (получает несколько машин)

**Examples:**

#### Code Example: REST API Design

Вот код на Python:

```python
from flask import Flask, request, jsonify

app = Flask(__name__)

# Sample in-memory data
users = {
    1: {'id': 1, 'name': 'Alice', 'email': 'alice@example.com'},
    2: {'id': 2, 'name': 'Bob', 'email': 'bob@example.com'}
}

# Non-RESTful - Not recommended
@app.route('/create-user', methods=['POST'])
def create_user():
    data = request.json
    user_id = max(users.keys(), default=0) + 1
    users[user_id] = data
    return jsonify({'id': user_id}), 201

# RESTful - Preferred
@app.route('/users', methods=['POST'])
def create_user_restful():
    data = request.json
    user_id = max(users.keys(), default=0) + 1
    users[user_id] = data
    return jsonify({'id': user_id}), 201

# Supporting both singular and plural forms
@app.route('/user/<int:user_id>', methods=['GET'])
@app.route('/users', methods=['GET'])
def get_user(user_id=None):
    if user_id is not None:
        return jsonify(users.get(user_id))
    return jsonify(users.values())

if __name__ == '__main__':
    app.run()
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/api-design-interview-questions/
</details>

<details>
<summary>Когда designing an API, how would you document it for end-users?</summary>

### Когда designing an API, how would you document it for end-users?

**Definition:**

Документация API важна для удобства. Она служит подробным руководством, по которому разработчики понимают, как API работает и как с ним взаимодействовать.

**How it works:**

#### Инструменты документации API

Есть несколько удобных инструментов документации API, среди них:

- **Swagger**: Использует декларативный формат JSON/YAML, чтобы описать детали REST API.

- **API Blueprint**: Встраивает markdown в формат блока кода, чтобы набросать требования к API.

- **RAML**: Использует YAML, чтобы получить ясный собранный чертёж API.

- **OAS (Formerly Swagger 2.0)**: Сосредоточен на том, чтобы собрать справочные данные API и породить интерактивную, очень динамичную документацию.

#### Самодокументируемые API

Современные языки вроде Python и Java позволяют API документировать себя. Если включить код, разработчики видят документацию сразу, инструментами вроде:

- **Javadoc**: Ключевой для Java, этот инструмент встраивает сдвинутые HTML-комментарии в исходный код Java.

- **Doxygen**: Подходит для нескольких языков, разбирает структурированные комментарии и порождает разные виды вывода.

Если встроить самодокументирование в кодовую базу API, документация остаётся актуальной и идёт в ногу с кодом.

#### Графическое представление

«Visual Documentation», или диаграммы API, показывают систему ресурсов, их связи, потоки данных и описания функций в наглядном графическом виде.

Среди инструментов для этого:

- **Postman**: Помогает проектировщикам видеть API в отдельном интерфейсе и облегчает интеграцию.

- **API Workbench**: Интерактивная «IDE» для разработки API, которая поддерживает управление жизненным циклом API.

**Examples:**

#### Code Examples: Self-Documenting API

Вот код на Python:

```python
class Student:
    """
    A simple student class.

    Attributes:
        name (str): The name of the student.
        age (int): The age of the student.
        subjects (list of str): The subjects the student is enrolled in.
    """

    def __init__(self, name, age, subjects=None):
        """
        Initializes a Student.

        Args:
            name (str): The name of the student.
            age (int): The age of the student.
            subjects (list of str, optional): The subjects the student is enrolled in.
        """
        self.name = name
        self.age = age
        self.subjects = subjects if subjects else []

    def enroll_in_subject(self, new_subject):
        """
        Enrolls the student in a new subject.

        Args:
            new_subject (str): The name of the new subject to enroll in.
        """
        self.subjects.append(new_subject)
```

В этом классе на Python видны хорошо описанные атрибуты, методы и сам класс: структурированные комментарии, которые инструменты документации могут извлечь.

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/api-design-interview-questions/
</details>

<details>
<summary>Какие соображения might influence how you paginate API responses?</summary>

### Какие соображения might influence how you paginate API responses?

**Definition:**

Посмотрим, какие аспекты стоит учитывать, когда вы разбиваете ответы API на страницы.

**How it works:**

#### Обычные приёмы пагинации

#### Пагинация по offset

Использует offset, чтобы задать начальную точку, и limit, чтобы задать число возвращаемых записей.
Просто реализовать, но может давать проблемы с производительностью, особенно на очень больших наборах данных.

#### Пагинация по ключу

Использует ключевые атрибуты, например уникальный идентификатор записи, чтобы задать начало следующего набора результатов.
Часто эффективна, но атрибуты должны быть монотонными, чтобы покрыть данные и не получить дубли.

#### Пагинация по метке времени

Берёт метку времени записи, чтобы решить, откуда начать и как далеко назад идти.
Прямолинейна и хорошо справляется с постоянно меняющимися наборами данных.

#### Пагинация по курсору

Pavilion — использует курсоры, обычно в виде закодированных токенов, чтобы указать текущую позицию в наборе данных.
Эффективна и не страдает от недостатков вроде «пропусков» у пагинации по offset.

#### Гибридная пагинация

Гибкость баз данных часто позволяет сочетать перечисленные способы под конкретные требования приложения.

#### Как выбрать лучший приём пагинации

#### Свойства набора данных

Размер набора, его изменчивость и распределение влияют на выбор. Например, набор, куда часто вставляют или откуда часто удаляют записи, может выиграть от пагинации по курсору, чтобы сохранить согласованность.

- **The selectivity of the data attributes is another vital consideration**: Пагинация по ключу и по метке времени лучше работает с уникальными и упорядоченными атрибутами, а методы на курсоре могут быть приспособленнее к неуникальным атрибутам.

#### Связи и целостность данных

Если у данных сильные связи, которые нужно сохранять между страницами, часто предпочитают методы, ведомые ключом.

#### Соображения производительности

Прежде чем выбрать метод, проверьте его производительность на наборах разного размера и разной конфигурации данных.

#### Безопасность и кеширование

Каждый метод по-разному влияет на безопасность и кеширование. Важно оценить, как механизмы кеша и безопасности стыкуются с выбранным подходом.

#### Сложность для клиента

Учтите удобство для потребителей API: некоторые методы добавляют клиентскому коду лишнюю сложность.

#### Требования к порядку

Если обязателен конкретный порядок (например, хронология), убедитесь, что выбранный метод надёжно отдаёт результаты в этом порядке.

#### Обработка ошибок

Вероятность ошибок при пагинации и способы их разрешения могут отличаться у разных методов.

#### Версии и эволюция полей

Структуры данных меняются, и ключ, который когда-то был надёжным, может стать непригодным для дальнейшей постраничной выдачи.

#### Ограничения среды

Некоторые платформы или инструменты лучше стыкуются с определёнными приёмами пагинации.

#### Запас на будущее и гибкость

Постоянно оценивайте, насколько выбранный метод удовлетворяет и нынешним, и обозримым будущим критериям.

**Examples:**

#### Реализация пагинации по диапазону

Вот код на Python:

```python
def paginate_using_range(start, end, limit):
    results = get_results_in_range(start, end, limit)
    next_start = results[-1].id + 1  # Assuming results are sorted by id
    next_end = next_start + limit - 1 if len(results) == limit else None

    return {
        "data": results,
        "links": {
            "next": f"/resource?start={next_start}&end={next_end}&limit={limit}" 
        }
    }
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/api-design-interview-questions/
</details>

<details>
<summary>Что такое HATEOAS, and how does it relate to API design?</summary>

### Что такое HATEOAS, and how does it relate to API design?

**Definition:**

HATEOAS (Hypermedia as the Engine of Application State) — ограничение REST. В ответе есть гипермедиа-ссылки, и именно они ведут клиента дальше.

**How it works:**

GET отдела может вернуть ссылки с href, rel и type, например href 10/employees, rel employees, type GET. Та же ссылка может быть заголовком Link: <10/employees>; rel="employees". RFC 5988 задаёт href, rel и необязательные hreflang, media, title и type. HAL использует application/hal+json или application/hal+xml и те же href, rel и type. Клиент начинает с одного URI и идёт по ссылкам сервера, поэтому формы URI не зашивает. Сервер может менять эти URI, не ломая клиента. У самого JSON нет одного стандарта ссылок.

**Examples:**

```json
{
  "departmentId": 10,
  "links": [{ "href": "10/employees", "rel": "employees", "type": "GET" }]
}
```

**Sources:**

- [HATEOAS](https://restfulapi.net/hateoas/)
</details>

<details>
<summary>Как бы вы handle localization and internationalization in APIs?</summary>

### Как бы вы handle localization and internationalization in APIs?

**Definition:**

Ни одна страница не является руководством по локализации. RFC 5988, как его цитирует страница HATEOAS, включает hreflang у ссылки. Страница CORS перечисляет Accept-Language среди заголовков, которые может выставить простой запрос.

**How it works:**

hreflang описывает язык цели ссылки. Accept-Language — один из CORS-safelisted заголовков запроса, вместе с Accept, Content-Language, Content-Type и одним Range. Каталоги сообщений и согласование локали дальше этих полей страницы не описывают.

**Sources:**

- [HATEOAS](https://restfulapi.net/hateoas/)
- [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CORS)
</details>

<details>
<summary>Что такое some best practices for designing error responses in an API?</summary>

### Что такое some best practices for designing error responses in an API?

**Definition:**

Страницы не задают тело ошибки. MDN говорит, что сбой CORS — это ошибка, подробности которой JavaScript прочитать не может.

**How it works:**

Скрипт знает только, что ошибка произошла. Конкретная причина — в консоли браузера. Так задумано. Страница HATEOAS показывает успешный 200 с заголовком Link, а не документ ошибки.

**Sources:**

- [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CORS)
- [HATEOAS](https://restfulapi.net/hateoas/)
</details>

<details>
<summary>Что такое the benefits of using API Gateways?</summary>

### Что такое the benefits of using API Gateways?

**Definition:**

API-шлюз ни на одной из страниц не описан.

**How it works:**

Фронт на странице HATEOAS — начальный URI плюс ссылки, которые возвращает сервер. Фронт на странице CORS — проверка того же источника в браузере и согласие сервера через Access-Control-Allow-Origin. Ни то, ни другое не продукт шлюза.

**Sources:**

- [HATEOAS](https://restfulapi.net/hateoas/)
- [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CORS)
</details>

<details>
<summary>Как бы вы approach handling file uploads in an API design?</summary>

### Как бы вы approach handling file uploads in an API design?

**Definition:**

Страница CORS считает multipart/form-data типом содержимого, при котором POST может остаться простым запросом. Подписка на XMLHttpRequest.upload делает запрос непростым, и браузер сначала делает preflight.

**How it works:**

Простой POST может использовать application/x-www-form-urlencoded, multipart/form-data или text/plain. Если код вызывает xhr.upload.addEventListener или тело — это ReadableStream, запрос не простой. Методы кроме GET, HEAD и POST, и POST с другими типами содержимого, сначала проверяются запросом OPTIONS. Страница HATEOAS загрузки не обсуждает.

**Sources:**

- [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CORS)
</details>

### API Performance and Scalability

<details>
<summary>Как caching be incoRPOrated into API design to improve performance?</summary>

### Как caching be incoRPOrated into API design to improve performance?

**Definition:**

Ни одна страница не описывает кэш и Cache-Control.

**How it works:**

Страница CORS перечисляет Range с одним диапазоном как safelisted-заголовок запроса. Это запрос части ресурса, а не сохранённый ответ. Ответы HATEOAS несут ссылки; что эти ссылки кэшируются, страница не говорит.

**Sources:**

- [HATEOAS](https://restfulapi.net/hateoas/)
- [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CORS)
</details>

<details>
<summary>Что такое some strategies for dealing with high traffic volumes in a Scalable API?</summary>

### Что такое some strategies for dealing with high traffic volumes in a Scalable API?

**Definition:**

Ни одна страница не описывает стратегию трафика вроде шардирования или очереди.

**How it works:**

Что они меняют при нагрузке — это работа, которую добавляет браузер. Непростой вызов сначала шлёт OPTIONS и только потом настоящий запрос, если сервер согласился. HATEOAS позволяет серверу менять URI, не заставляя клиентов зашивать их. Это правило эволюции, а не правило ёмкости.

**Sources:**

- [HATEOAS](https://restfulapi.net/hateoas/)
- [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CORS)
</details>

<details>
<summary>Как connection pooling work and how can it benefit API performance?</summary>

### Как connection pooling work and how can it benefit API performance?

**Definition:**

Пул соединений ни на одной странице не описан.

**How it works:**

Дополнительное соединение, которое задаёт страница CORS, — это preflight. Браузер шлёт OPTIONS с методом и заголовками, которые собирается использовать, и шлёт настоящий запрос только после разрешения сервера. Это второй обмен HTTP, а не пул.

**Sources:**

- [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CORS)
</details>

<details>
<summary>Когда is it appropriate to use synchronous vs asynchronous processing in an API?</summary>

### Когда is it appropriate to use synchronous vs asynchronous processing in an API?

**Definition:**

Страницы не определяют очередь заданий против вызова запрос-ответ. Они показывают вызов, который ждёт предшествующий ответ OPTIONS.

**How it works:**

Для непростого запроса браузер должен закончить preflight, прежде чем послать настоящий метод. GET, HEAD и POST с типом form-urlencoded, multipart или text/plain могут обойтись без этого ожидания, если выполнены остальные условия простого запроса. Клиент HATEOAS тоже ждёт ответ, прежде чем перейти по следующей ссылке. Фоновую обработку ни одна страница не называет.

**Sources:**

- U
- s
- e
-  
- a
-  
- s
- i
- m
- p
- l
- e
-  
- r
- e
- q
- u
- e
- s
- t
-  
- w
- h
- e
- n
-  
- t
- h
- e
-  
- m
- e
- t
- h
- o
- d
-  
- a
- n
- d
-  
- h
- e
- a
- d
- e
- r
- s
-  
- q
- u
- a
- l
- i
- f
- y
-  
- a
- n
- d
-  
- y
- o
- u
-  
- d
- o
-  
- n
- o
- t
-  
- w
- a
- n
- t
-  
- t
- h
- e
-  
- O
- P
- T
- I
- O
- N
- S
-  
- r
- o
- u
- n
- d
-  
- t
- r
- i
- p
- .
-  
- U
- s
- e
-  
- t
- h
- e
-  
- p
- r
- e
- f
- l
- i
- g
- h
- t
-  
- p
- a
- t
- h
-  
- w
- h
- e
- n
-  
- t
- h
- e
-  
- m
- e
- t
- h
- o
- d
-  
- o
- r
-  
- C
- o
- n
- t
- e
- n
- t
- -
- T
- y
- p
- e
-  
- i
- s
-  
- o
- u
- t
- s
- i
- d
- e
-  
- t
- h
- a
- t
-  
- l
- i
- s
- t
- .
</details>

<details>
<summary>Что такое some challenges of maintaining an API at scale?</summary>

### Что такое some challenges of maintaining an API at scale?

**Definition:**

Проблема масштаба для клиентов на странице HATEOAS — зашитые URI. Если каждый клиент запёк пути, смена URI на сервере их ломает.

**How it works:**

Ссылки в теле или в заголовке Link позволяют серверу переносить ресурсы. Клиент хранит первый URI и типы связей. Сторона CORS — эксплуатационная: каждый непростой вызов из браузера платит за запрос OPTIONS, а отсутствующий Access-Control-Allow-Origin роняет вызов так, что скрипт не может это разобрать.

**Sources:**

- [HATEOAS](https://restfulapi.net/hateoas/)
- [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CORS)
</details>

### API Security Considerations

<details>
<summary>Что такое common security concerns when designing an API?</summary>

### Что такое common security concerns when designing an API?

**Definition:**

Браузеры применяют политику того же источника к fetch и XMLHttpRequest. Скрипт может прочитать межсайтовый ответ, только если сервер согласился заголовками CORS.

**How it works:**

Пример — страница на https://domain-a.com вызывает https://domain-b.com/data.json. Запрос несёт Origin. Методы кроме GET и POST с типом содержимого вне трёх простых сначала проходят preflight. Сервер также говорит, можно ли слать учётные данные, такие как cookies и HTTP-аутентификация. Ошибки CORS прячут причину от JavaScript. Страница HATEOAS аутентификацию не обсуждает.

**Sources:**

- [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CORS)
</details>

<details>
<summary>Как you prevent injection attacks in API design?</summary>

### Как you prevent injection attacks in API design?

**Definition:**

Инъекции ни на одной странице не обсуждаются.

**How it works:**

Близкое предупреждение страницы CORS — CSRF. Простые запросы похожи на отправку формы, которую HTML и так мог послать на другой сайт, поэтому сервер уже должен защищаться от межсайтовой подделки запроса. CORS эту защиту не заменяет. Preflight не ищет во внедрённой команде ни строку запроса, ни тело JSON.

**Sources:**

- [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CORS)
</details>

<details>
<summary>Можете ли вы explain what CORS is and why it’s important in API design?</summary>

### Можете ли вы explain what CORS is and why it’s important in API design?

**Definition:**

CORS — механизм на заголовках HTTP. Сервер перечисляет источники, кроме своего, из которых браузер может прочитать ответ. Иначе браузеры блокируют межсайтовое чтение из fetch и XMLHttpRequest.

**How it works:**

Браузер шлёт Origin. Для непростых запросов он сначала шлёт OPTIONS с методом и заголовками. Сервер соглашается через Access-Control-Allow-Origin; без этого заголовка скрипт не прочитает ответ, даже если сервер запрос обработал. Простые запросы — это GET, HEAD или POST, только safelisted-заголовки и Content-Type application/x-www-form-urlencoded, multipart/form-data или text/plain. Учётные данные — это cookies и HTTP-аутентификация, и сервер должен разрешить их отправку. Подробности сбоя остаются в консоли браузера.

**Examples:**

```javascript
fetch("https://bar.other").then((response) => response.json());
```

**Sources:**

- [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CORS)
</details>

<details>
<summary>Что такое the puRPOse of an API key?</summary>

### Что такое the puRPOse of an API key?

**Definition:**

Ключ API ни на одной странице не определён.

**How it works:**

Типы учётных данных, которые называет страница CORS, — cookies и HTTP-аутентификация. Сервер решает, отправляются ли они с межсайтовым запросом. Ключ в заголовке тоже должен быть safelisted-заголовком, иначе браузер сделает preflight. Заголовок ключа API в safelisted-списке страница не приводит.

**Sources:**

- [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CORS)
</details>

<details>
<summary>Как бы вы implement authentication and authorization in APIs?</summary>

### Как бы вы implement authentication and authorization in APIs?

**Definition:**

Страницы не задают схему аутентификации вроде OAuth. CORS говорит, что сервер управляет тем, включаются ли учётные данные.

**How it works:**

Учётные данные здесь — cookies и HTTP-аутентификация. Простой запрос всё равно уходит, но скрипт не прочитает ответ, пока Access-Control-Allow-Origin не разрешит этот источник. Ссылки HATEOAS называют type, например GET; решение об авторизации они не несут.

**Sources:**

- [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CORS)
- [HATEOAS](https://restfulapi.net/hateoas/)
</details>

### Advanced API Design Concepts

<details>
<summary>Что такое the role of an API Gateway in microservices architecture?</summary>

### Что такое the role of an API Gateway in microservices architecture?

**Definition:**

Ни одна страница не ставит API-шлюз перед микросервисами.

**How it works:**

HATEOAS держит клиента на ссылках, которые дал сервер, чтобы раскладка URI могла меняться по ресурсам. CORS не даёт браузеру прочитать ответ другого источника, пока тот источник не согласится. Это два фронта, которые страницы реально описывают.

**Sources:**

- [HATEOAS](https://restfulapi.net/hateoas/)
- [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CORS)
</details>

<details>
<summary>Можете ли вы explain the concept of a headless API?</summary>

### Можете ли вы explain the concept of a headless API?

**Definition:**

Headless — не термин этих страниц. Клиент HATEOAS потребляет ссылки и не зашивает дерево URI сервера.

**How it works:**

Клиент начинает с одного URI, читает href и rel и обнаруживает действия. Страница сравнивает это с человеком, который ходит по сайту. CMS и разделение отрисовки она не описывает.

**Sources:**

- [HATEOAS](https://restfulapi.net/hateoas/)
</details>

<details>
<summary>Как GraphQL APIs differ from traditional RESTful APIs?</summary>

### Как GraphQL APIs differ from traditional RESTful APIs?

**Definition:**

GraphQL ни на одной странице нет. HATEOAS — ограничение REST: ответ перечисляет следующие ресурсы.

**How it works:**

Пример отдела возвращает ссылки, а не ждёт, что клиент заранее знает 10/employees. HAL упаковывает эти ссылки как application/hal+json или application/hal+xml. Язык запросов, в котором клиент называет поля, не описан.

**Sources:**

- [HATEOAS](https://restfulapi.net/hateoas/)
</details>

<details>
<summary>Что такое gRPC and how might it be used in API design?</summary>

### Что такое gRPC and how might it be used in API design?

**Definition:**

gRPC ни на одной странице нет.

**How it works:**

Механизмы HTTP, которые они называют, — GET в примере отдела, заголовок Link и методы CORS GET, HEAD, POST, плюс OPTIONS для preflight. Обрамление RPC не задано.

**Sources:**

- [HATEOAS](https://restfulapi.net/hateoas/)
- [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CORS)
</details>

<details>
<summary>Как WebSockets enhance API functionalities?</summary>

### Как WebSockets enhance API functionalities?

**Definition:**

WebSockets ни на одной странице нет.

**How it works:**

Браузерные API, которые называет страница CORS, — fetch и XMLHttpRequest, плюс шрифты, текстуры WebGL, картинки canvas и CSS-фигуры. Долгоживущий сокет в этот список не входит. Навигация HATEOAS — последовательность запросов по ссылкам.

**Sources:**

- [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CORS)
- [HATEOAS](https://restfulapi.net/hateoas/)
</details>

### API Development and Testing

<details>
<summary>Какие инструменты or frameworks do you use to develop and test APIs?</summary>

### Какие инструменты or frameworks do you use to develop and test APIs?

**Definition:**

Страница HATEOAS говорит, что каждый REST-фреймворк строит ссылки по-своему. Её пример — ссылки на классах модели ресурса, которые отправляют клиенту. Этот приём она называет в учебнике Spring Boot HATEOAS и не показывает аннотации.

**How it works:**

Клиентский инструмент страницы CORS — fetch или XMLHttpRequest. Тестовый прогон она не называет. Инструмент производительности не перечислен.

**Sources:**

- [HATEOAS](https://restfulapi.net/hateoas/)
- [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CORS)
</details>

<details>
<summary>Как бы вы test for API performance and what metrics would you track?</summary>

### Как бы вы test for API performance and what metrics would you track?

**Definition:**

Ни одна страница не задаёт тест производительности и метрику.

**How it works:**

Дополнительная задержка, которую они делают видимой, — preflight CORS: OPTIONS, затем настоящий запрос, для любого непростого вызова. Клиент HATEOAS добавляет запрос на каждую ссылку, по которой идёт. Число пропускной способности не опубликовано.

**Sources:**

- [HATEOAS](https://restfulapi.net/hateoas/)
- [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CORS)
</details>

<details>
<summary>Что такое contract testing in the context of API development?</summary>

### Что такое contract testing in the context of API development?

**Definition:**

Контрактное тестирование ни на одной странице не описано.

**How it works:**

Ближайший опубликованный контракт — тип связи ссылки. И RFC 5988, и HAL используют href и rel, чтобы клиент знал, как текущий ресурс относится к цели. У HAL ещё есть type для ожидаемого типа медиа, application/hal+json или application/hal+xml. Это форма ответа, а не набор тестов.

**Sources:**

- [HATEOAS](https://restfulapi.net/hateoas/)
</details>

<details>
<summary>Опишите the mocking of APIs for development and testing puRPOses.</summary>

### Опишите the mocking of APIs for development and testing puRPOses.

**Definition:**

Подмена API ни на одной странице не описана.

**How it works:**

Страница HATEOAS говорит, что клиенту нужны начальный URI и ссылки в ответе. Клиент, который зашивает дальнейшие URI, — это как раз случай, который, по странице, ломается, когда сервер меняет пути. Клиент страницы CORS — настоящий fetch на другой источник, с отправленным Origin.

**Sources:**

- [HATEOAS](https://restfulapi.net/hateoas/)
- [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CORS)
</details>

<details>
<summary>Как automated API testing improve the software development lifecycle?</summary>

### Как automated API testing improve the software development lifecycle?

**Definition:**

Автоматическое тестирование API ни на одной странице не обсуждается.

**How it works:**

Поведение клиента, которое они задают, всё же можно проверить: ответ HATEOAS содержит href и rel, а межсайтовый fetch читается, только когда Access-Control-Allow-Origin разрешает Origin. Конвейер, который гоняет эти проверки, страницы не описывают.

**Sources:**

- [HATEOAS](https://restfulapi.net/hateoas/)
- [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CORS)
</details>

### API Data Management

<details>
<summary>Как you handle large volumes of data in API responses?</summary>

### Как you handle large volumes of data in API responses?

**Definition:**

Большим ответам ни одна страница не даёт стратегию. Страница CORS разрешает один заголовок Range в простом запросе.

**How it works:**

Примеры этого заголовка — bytes=256- и bytes=127-255. Больше одного диапазона выходит за safelisted-форму. HATEOAS не говорит постранично отдавать коллекцию; пример отдела возвращает один отдел и ссылку на сотрудников.

**Sources:**

- [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CORS)
- [HATEOAS](https://restfulapi.net/hateoas/)
</details>

<details>
<summary>Как an API be designed to support multiple data formats?</summary>

### Как an API be designed to support multiple data formats?

**Definition:**

HAL публикует два типа медиа: application/hal+json и application/hal+xml. Страница HATEOAS говорит выбрать один формат ссылок и держаться его. Ссылки могут быть и заголовком Link, а не телом JSON.

**How it works:**

Со стороны CORS Content-Type входит в safelisted только для application/x-www-form-urlencoded, multipart/form-data и text/plain. application/json и application/hal+json в этом списке нет, поэтому браузерный POST этих типов проходит preflight. Среди атрибутов RFC 5988 есть type.

**When to use:**

HAL — когда тело должно нести href, rel и type в JSON или XML. Заголовок Link — когда ссылки должны остаться вне тела. От браузера ждут preflight для типа содержимого JSON.

**Sources:**

- [HATEOAS](https://restfulapi.net/hateoas/)
- [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CORS)
</details>

<details>
<summary>Что такое best practices for managing sensitive data through an API?</summary>

### Что такое best practices for managing sensitive data through an API?

**Definition:**

Обращение с чувствительными данными за пределами межсайтовых правил браузера не задано. CORS считает cookies и HTTP-аутентификацию учётными данными, которые сервер должен явно разрешить.

**How it works:**

Межсайтовый скрипт не прочитает ответ без Access-Control-Allow-Origin. Сервер отдельно решает, отправляются ли учётные данные. Сбой CORS не сообщает JavaScript, почему. Шифрование полей и хранилище секретов страницы не описывают.

**Sources:**

- [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CORS)
</details>

<details>
<summary>В an API, how do you approach filtering and sorting of data?</summary>

### В an API, how do you approach filtering and sorting of data?

**Definition:**

Фильтрация и сортировка ни на одной странице не описаны.

**How it works:**

Ссылка отдела в HATEOAS — rel employees на 10/employees. Строку запроса для фильтра или сортировки страница не добавляет. От клиента ждут переход по ссылке, которую дал сервер, а не выдуманные параметры.

**Sources:**

- [HATEOAS](https://restfulapi.net/hateoas/)
</details>

<details>
<summary>Можете ли вы discuss strategies for transaction management within API endpoints?</summary>

### Можете ли вы discuss strategies for transaction management within API endpoints?

**Definition:**

Управление транзакциями внутри конечной точки ни на одной странице не описано.

**How it works:**

HATEOAS меняет состояние приложения ссылкой, по которой клиент идёт дальше, например от отдела к его сотрудникам. Это состояние навигации, а не транзакция базы. CORS OPTIONS ничего не фиксирует и не откатывает.

**Sources:**

- [HATEOAS](https://restfulapi.net/hateoas/)
- [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CORS)
</details>

### API Specification and Standards

<details>
<summary>Являются ли you familiar with any API specification formats and what benefits do they provide?</summary>

### Являются ли you familiar with any API specification formats and what benefits do they provide?

**Definition:**

Страница HATEOAS называет два формата ссылок: RFC 5988 и HAL. Она говорит, что единого стандарта JSON для ссылок нет, поэтому формат выбирают и его придерживаются.

**How it works:**

У ссылок RFC 5988 есть целевой IRI (href), отношение (rel) и необязательные hreflang, media, title и type. HAL описан как черновик с типами медиа application/hal+json и application/hal+xml и свойствами ссылки href, rel и type. Названная польза — клиенты обнаруживают действия и переживают смену URI. OpenAPI на странице нет.

**Sources:**

- [HATEOAS](https://restfulapi.net/hateoas/)
</details>

<details>
<summary>Как you approach the design of an API to comply with industry standards?</summary>

### Как you approach the design of an API to comply with industry standards?

**Definition:**

Стандарты на этих страницах — отношения ссылок RFC 5988 и правила запроса CORS, а не перечень отраслевых режимов.

**How it works:**

Ссылка, по которой могут идти другие клиенты, использует href и rel. Браузерному клиенту с другого источника нужен Access-Control-Allow-Origin и, для непростых вызовов, успешный preflight OPTIONS. type в HAL говорит, какой тип медиа ждать. PCI, HIPAA и похожие режимы страницы не обсуждают.

**Sources:**

- [HATEOAS](https://restfulapi.net/hateoas/)
- [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CORS)
</details>

<details>
<summary>Как you foresee OpenAPI/Swagger specifications evolving, and how do they impact API design?</summary>

### Как you foresee OpenAPI/Swagger specifications evolving, and how do they impact API design?

**Definition:**

OpenAPI и Swagger не обсуждаются, и ни одна страница не прогнозирует, как они изменятся.

**How it works:**

Эволюция, которую описывает страница HATEOAS, на стороне сервера: пути URI могут меняться, потому что клиенты идут по href, а не зашивают их. HAL прямо назван черновиком, с application/hal+json и application/hal+xml. Это статус спецификации на странице, а не дорожная карта.

**Sources:**

- [HATEOAS](https://restfulapi.net/hateoas/)
</details>

<details>
<summary>Как APIs be designed with interoperability in mind?</summary>

### Как APIs be designed with interoperability in mind?

**Definition:**

Совместимость в HATEOAS — это общие типы связей. Клиенты, которые понимают href и rel, переходят между ресурсами без частной карты URI.

**How it works:**

RFC 5988 — каркас веб-ссылок, который цитирует страница. HAL — одно соглашение JSON или XML для тех же href, rel и type. Страница говорит, что и тело JSON, и заголовок Link — годный способ передать ссылки, и форматы не стоит мешать случайно: выбирают один. Браузеру с другого источника всё равно нужно согласие CORS, иначе он не прочитает это тело.

**Sources:**

- [HATEOAS](https://restfulapi.net/hateoas/)
- [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CORS)
</details>

<details>
<summary>Как a standard like JSON:API influence the way you structure response payloads?</summary>

### Как a standard like JSON:API influence the way you structure response payloads?

**Definition:**

JSON:API ни на одной странице не задан. Соглашения JSON-ссылок, которые они задают, — HAL и обычный массив links.

**How it works:**

Пример отдела использует массив links из href, rel и type. HAL использует application/hal+json и те же три свойства и описан как черновик. Заголовок Link из RFC 5988 — альтернатива без тела. Страница говорит выбрать один из этих способов и держаться его, потому что у JSON нет универсального формата ссылок.

**Sources:**

- [HATEOAS](https://restfulapi.net/hateoas/)
</details>
