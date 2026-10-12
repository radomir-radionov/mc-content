# Core

advices:  
The Only Thing Stopping You, Is You - https://medium.com/@milhoornaert/d28646b2775f

https://stackblitz.com/

`Обучение` - это воспитание культуры мышления  
Не важно на каком языку ты пишешь, важно на каком думаешь! Если ты думаешь на своём языке программирования(js, py), то тебе конец

Почему ии требует ещё больше знаний чем без него, не зная ничего ты получишь ничего фантик и то корявые

Логика - мыслить логично

Программист без продукта

**road** - https://roadmap.sh/

**RO docs** - https://drive.google.com/drive/folders/1tHt1I9HhHeVTmBAY9xFUCHMwuHTeNVXF

**basic be** - https://www.youtube.com/watch?v=cwti7BuWZ0c

**conferences** https://portal.gitnation.org/

**image of code** - https://ray.so/

website vs web apps - https://www.instagram.com/p/CgHUG2srciZ/  
usefull links - https://www.instagram.com/p/CetPdRuPQKJ/

     Понимать как работает система

`script` (сценарий) — это последовательность действий, описанных с помощью скриптового языка программирования.

`Методология программирования` - это система правил, принципов и процессов, которые разработчики используют для создания качественного программного обеспечения. Она определяет шаги, которые должны быть выполнены для разработки программного обеспечения, и обычно включает в себя следующие этапы: анализ требований, проектирование системы, реализацию, тестирование и сопровождение.

`environment` is a program that can run the scripts(js). It is not just limited to standard desktop environments but can extend to specialized devices, including microcontrollers and spacecraft components.

`Компиляция` - вначалае весь код преобразуется в машинный, а потом исполняется. Компиляция быстрее, но может занимать больше времени и памяти.  
`Интерпретиция` - код строчка за строчкой преобразуется и сразу испольняется. Интерпретация более гибкая, но менее эффективная по производительности.

A `statement` is a piece of code that performs an action, such as declaring a variable, assigning a value to a variable, or executing a loop. Statements are usually terminated with a semicolon (;) in JavaScript.  
An `expression`, on the other hand, is a piece of code that produces a value. Expressions can be used as part of a statement or on their own.

A `payload` is the core data or content within a transmission or function, distinct from any control or routing information. It's typically used in the following contexts:

- Network Communications: In data packets, the payload is the actual data being transmitted, like the content of a message or file.
- Malware and Cybersecurity: Payload refers to the harmful code or actions carried out by malicious software, like viruses or Trojans.
- Aerospace: Payload refers to cargo, instruments, or equipment carried by vehicles like rockets or satellites.
- Information Security: In penetration testing, it's the code or data used to exploit vulnerabilities in a system.

Use `payload` when you want to emphasize and refer to the essential data or content of a transmission, making your code or communication more precise and understandable.

`Библиотека` - решает конкретное направление  
`Framework` - набор из 3(примерно) билиотек

The key difference between a library and a framework is "Inversion of Control".
You tell libraries what to do, frameworks tell you what to do.
A library is a collection of prewritten code that can be used to simplify tasks(classes, functions
A framework is a structure in which the application defines the content of the operation by asking the developer to fill in the spaces.

В информатике термин `кластер` обычно относится к группе связанных компьютеров или серверов, которые работают вместе и выступают в качестве единого вычислительного ресурса. Кластеры используются для выполнения задач, которые требуют больших вычислительных мощностей, распределения нагрузки, повышенной отказоустойчивости или параллельной обработки.

<details>
  <summary>absolute vs relative path</summary>

### absolute vs relative path

An `absolute path` specifies the complete path to a file or directory, starting from the root directory of the file system. Here are some examples of absolute paths:

Windows: C:\Users\username\Documents\file.txt  
Linux/Mac: /home/username/Documents/file.txt  
In both cases, the complete path is specified, starting from the root directory of the file system.

A `relative path`, on the other hand, specifies the path to a file or directory relative to the current working directory. Here are some examples of relative paths:

If the current working directory is /home/username/Documents/:  
To specify the file `file.txt`, you can use `file.txt`.  
To specify the directory `images` in the parent directory, you can use `../images`.  
To specify the file `logo.png` in the images directory in the parent directory, you can use `../images/logo.png`.  
Relative paths are more flexible and easier to use, but they may be less specific and require more care to ensure that the current working directory is correct.

</details>

<details>
  <summary>library & tool</summary>

### library & tool

`Library`: A library is a collection of precompiled code modules or functions that provide specific functionality. It is designed to be reused by other programs and can be included in the code of an application to provide ready-made solutions for common tasks. Libraries often focus on a specific domain or functionality, such as handling network communication, parsing JSON data, or performing mathematical calculations. Developers can leverage libraries to save time and effort by utilizing pre-existing code rather than implementing everything from scratch.

`Tool`: A tool, on the other hand, is a software program or utility that assists developers in performing specific tasks or achieving specific goals during the development process. Tools are typically standalone applications that provide functionality beyond what libraries offer. They are used to aid in development, debugging, testing, deployment, or project management. Examples of tools include integrated development environments (IDEs), code editors, version control systems, build systems, testing frameworks, and project management software.

In summary, a library provides reusable code components that developers can include in their applications to leverage existing functionality, while a tool is a standalone program that assists developers in performing various development tasks or managing projects. Libraries focus on providing specific functionality, while tools support the development process as a whole.

</details>

<details>
  <summary>services & utils</summary>

### services & utils

`Services` in Node.js are typically modules that provide specific functionality, such as authentication, database access, or file management, to other parts of the application. Services are often designed to be modular and reusable, so they can be accessed by multiple parts of the application. For example, a user authentication service that handles user login and registration is a service.

`Utilities` in Node.js, on the other hand, are standalone modules or functions that provide specific functionality to help developers perform specific tasks. Utilities are often included in the standard Node.js library, and they can be used for various purposes, such as string manipulation, error handling, or data validation. For example, the "fs" module in Node.js provides utilities for working with the file system, such as reading and writing files.

</details>

jQuery — набор функций для манипуляций с DOM, работы с событиями и модуль для клиент-серверного взаимодействия.
D3 — библиотека для создания визуализаций с использованием SVG.
React — UI library conspect → [4_react.md](4_react.md)
Next.js — full-stack React framework conspect → [4_nextjs.md](4_nextjs.md)
Vue — UI library conspect → [4_vue.md](4_vue.md)
Nuxt — full-stack Vue framework conspect → [4_nuxt.md](4_nuxt.md)

`a magic variable` is a variable that has a special meaning or behavior within a particular context. Magic variables are often defined by the programming language or framework being used, and can be used to simplify code or provide useful functionality.

1bite = 8bit

**_Протокол описыает как фронтенд общается с бэкэндом._**
Example: frontennd sends http request with data which describes what it need from backend and backend sends response.
Protocol describes rules in this case how they will interacts.

`Профилирование (profiling)` - это процесс анализа данных или характеристик, с целью создания профиля или описания определенных объектов, лиц, или событий.

a `literal` refers to a notation for representing a fixed value in source code. It is the way values are expressed directly within the code, as opposed to being represented by variables or expressions.

<details>
  <summary>Design System</summary>

### Design System

src: https://uxdesign.cc/everything-you-need-to-know-about-design-systems-54b109851969

A `Design System` is the single source of truth which groups all the elements that will allow the teams to design, realize and develop a product. Design System is not a deliverable, but a set of deliverables.

The fundamental purpose of a Design System is to facilitate the work of the teams.

#### important moments

- `Purpose and shared values`(it’s essential to align teams around a clear set of shared goals. It will help to build a vision and making sure everyone looks in the same direction.)
- `Design Principles`(Design principles are the guiding sentences that helps the teams to reach the purpose of the product thanks to the design. They will help the teams to make meaningful Design decisions. Design Principles must guide design choices)
- `Brand identity & language`(Colours Fonts Spaces Shapes Icons Illustrations Photographies Animations Voice and tone Sounds. All of this forms the alphabet of the Brand.)
- `Components & patterns`(The `components` are our LEGO blocs. They’re used in Sketch by designers, and directly in the code by developers. Their functional behaviour must be specified. As for the `patterns`, they are the building instructions that will allow us to use these components in a logical and consistent way, across all the products.

#### deliverables

- A Style Guide -as indicated by its name- will focus on graphic styles (colours, fonts, illustrations…) and their usage.
- A Pattern Library will integrate functional components and their usage.

#### What kind of Design System?

To define our own, we have to ask ourselves the right questions:

How many people will use this system?
What is their profile and are they mature enough on the subject?
How many products must we align? On how many platforms? How many different technologies (Angular, React, others…)?
What degree of consistency do we want across our products?

All these answers will help us to define the most appropriate kind of Design System.

</details>

<details>
  <summary>Architectural methodology for frontend projects</summary>

### Architectural methodology for frontend projects

src: https://feature-sliced.design/

</details>
