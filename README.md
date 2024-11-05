<h1> <b> Лабораторна робота №4. Розробка додатку для візуалізації вимірювань радару </b> </h1>

<b>Мета:</b> Розробити додаток, який зчитує дані з емульованої вимірювальної частини радару, наданої у вигляді Docker image, та відображає задетектовані цілі на графіку в полярних координатах.

<p><b>Завдання</b></p>
<ol>
    <li><b>Розробити додаток для відображення цілей:</b>
        <ul>
            <li>Розробити веб-додаток, який підключається до WebSocket сервера та зчитує дані про задетектовані цілі.</li>
            <li>Відобразити отримані дані на графіку в полярних координатах. Як варіант, можна використати бібліотеку Plotly або іншу бібліотеку для роботи з графіками.</li>
        </ul>
    </li>
    <li><b>Обробка та візуалізація даних:</b>
        <ul>
            <li>Обробити дані, отримані через WebSocket, і відобразити цілі на графіку.</li>
            <li>Кожна ціль повинна бути представлена як точка на графіку з координатами (кут, відстань).</li>
            <li>Додати можливість зміни параметрів вимірювальної частини радару за допомогою API запитів.</li>
        </ul>
    </li>
    <li><b>Налаштування графіка:</b>
        <ul>
            <li>Відобразити відстань у кілометрах на радіальній осі.</li>
            <li>Використати різні кольори або стилі точок для відображення різних рівнів потужності сигналів, що повертаються від цілей.</li>
        </ul>
    </li>
</ol>
<p><b>Завантаження та запуск емулятора вимірювальної частини радару:</b> Емулятор вимірювальної частини радару надається у вигляді Docker image під назвою <b>radar-emulation-service</b>. Для запуску емулятора завантажуємо Docker image з Docker Hub командою:</p>
<pre><code>docker pull iperekrestov/university:radar-emulation-service</code></pre>
<p>Після цього запускаємо Docker контейнер командою:</p>
<pre><code>docker run --name radar-emulator -p 4000:4000 iperekrestov/university:radar-emulation-service</code></pre>
<p>Ця команда запускає контейнер з ім'ям <b>radar-emulator</b> і відкриває порт <b>4000</b> для з'єднання з емульованою вимірювальною частиною радару.</p>

<p><b>Створення початкових налаштувань радара:</b> Початкові налаштування радара, такі як частота оновлення і потужність сигналу, визначаються в панелі керування на веб-сторінці додатка. Ці налаштування надсилаються на сервер, що дозволяє налаштувати емулятор для отримання відповідних даних.</p>

<p><b>WebSocket-з'єднання:</b> WebSocket-з'єднання встановлюється для підключення до сервера радара за адресою <b>ws://localhost:4000</b>. У разі успішного з'єднання відображається статус "Підключено".</p>
<p>Якщо з'єднання обривається, додаток автоматично намагається перепідключитися через 5 секунд. Вхідні дані від радара надходять у форматі сигналів ехо, які обробляються програмою.</p>

<p><b>Обробка радарних даних:</b> Дані кожного сигналу ехо містять інформацію про час проходження, який використовується для обчислення відстані до цілі. Відстань обчислюється за формулою на основі часу та швидкості світла:</p>
<p align="center">R = (c ⋅ t) / 2</p>
<p>Кожна ціль візуалізується на основі потужності сигналу, що дозволяє виділити більш потужні цілі зеленим кольором, а слабші — червоним.</p>

<p><b>Полярна діаграма:</b> Відображення цілей на полярній діаграмі здійснюється з використанням бібліотеки <b>Plotly</b>. Радіальна координата <b>r</b> представляє відстань до цілі, а кутова координата <b>theta</b> — напрямок. Графік автоматично оновлюється при надходженні нових даних від радара.</p>

<p><b>Оновлення конфігурації:</b> У веб-додатку користувач може оновлювати три параметри радара: <b>кількість вимірювань за оберт</b>, <b>швидкість обертання (RPM)</b> та <b>швидкість цілей (км/ч)</b>. Ці параметри відображаються у відповідних полях вводу, і після внесення змін користувач натискає кнопку <b>«Оновити конфігурацію»</b>. Це дозволяє користувачу відправити оновлені налаштування на сервер через HTTP-запит до порту <b>4000</b>, що дає можливість оновити параметри вимірювань у реальному часі.</p>

<p><b>Інформація про цілі:</b> Текстова інформація про цілі, яка включає відстань, кут і потужність сигналу, відображається під графіком. Це дозволяє користувачам швидко переглянути дані про нещодавно виявлені цілі, які надходили протягом останніх 5 секунд.</p>





