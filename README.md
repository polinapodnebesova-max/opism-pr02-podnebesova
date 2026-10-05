# Практична робота № 2

**Дисципліна:** Основи побудови інформаційних систем та мереж (ОК-13)

**Тема:** Структура повідомлень прикладного протоколу HTTP. Формування запиту вручну

| Поле | Значення |
|---|---|
| Студент (прізвище, ім'я, по батькові) |Поднебесова Поліна Андріївна |
| Група |2.02 |
| Номер варіанта |20 |
| Індивідуальний домен |w3.org |
| «Чужий» домен для завдання A.3.1 (варіант ± 20) |postgresql.org |
| Середовище виконання |Windows |
| Дата виконання |05.10.2026 |

> Бланк заповнюють, не змінюючи структури розділів. Порожні заготовки блоків коду замінюють власними виводами. Позначки-підказки в кутових дужках вилучають.

---

## Частина A. Збір експериментальних даних

### Завдання A.1. Формування запиту вручну

**Команда:**

```
$d = "w3.org"
$c = New-Object System.Net.Sockets.TcpClient($d, 80)
$s = $c.GetStream()
$w = New-Object System.IO.StreamWriter($s)
$w.Write("GET / HTTP/1.1`r`nHost: $d`r`nConnection: close`r`n`r`n")
$w.Flush()
(New-Object System.IO.StreamReader($s)).ReadToEnd()
$c.Close()
```

**Набраний запит:**

```
GET / HTTP/1.1
Host: w3.org
Connection: close

```

**Відповідь:**

```
HTTP/1.1 301 Moved Permanently
Date: Mon, 05 Oct 2026 11:10:43 GMT
Content-Type: text/html; charset=UTF-8
Transfer-Encoding: chunked
Connection: close
Location: https://www.w3.org/
set-cookie: __cf_bm=L.p4AHI.akK1RnBV5vCnU4zODt_a.NGKYTghdv5lH1o-1791198643.6780946-1.0.1.1-hnJnbqDFxjCji4DQOxt2K7IOwiVsHKj7Ev3vQjXm88ziMj6m0b2qGc_nOMetpOEj2QTVbX1P7Wf5eVwcq9LnwCWz6PMNOgq7HbyYXwm8T9bkyMxLS0HcEg9zfLLttx29; HttpOnly; Path=/; Domain=w3.org; Expires=Mon, 05 Oct 2026 11:40:43 GMT
X-Content-Type-Options: nosniff
Server: cloudflare
CF-RAY: a45bfb42fbd423b5-VIE
alt-svc: h3=":443"; ma=86400

a7
<html>
<head><title>301 Moved Permanently</title></head>
<body>
<center><h1>301 Moved Permanently</h1></center>
<hr><center>cloudflare</center>
</body>
</html>

0
```

---

### Завдання A.2. Запит без поля `Host` у версії 1.1

**Команда:**

```
$d = "w3.org"
$c = New-Object System.Net.Sockets.TcpClient($d, 80)
$s = $c.GetStream()
$w = New-Object System.IO.StreamWriter($s)
$w.Write("GET / HTTP/1.1`r`nConnection: close`r`n`r`n")
$w.Flush()
(New-Object System.IO.StreamReader($s)).ReadToEnd()
$c.Close()
```

**Вивід:**

```
HTTP/1.1 400 Bad Request
Server: cloudflare
Date: Mon, 05 Oct 2026 11:28:17 GMT
Content-Type: text/html
Content-Length: 155
Connection: close
CF-RAY: -

<html>
<head><title>400 Bad Request</title></head>
<body>
<center><h1>400 Bad Request</h1></center>
<hr><center>cloudflare</center>
</body>
</html>
```

---

### Завдання A.3. Вплив поля `Host` на відповідь сервера

#### A.3.1. Чуже доменне ім'я в полі `Host`

**Команда:**

```
$d = "w3.org"
$c = New-Object System.Net.Sockets.TcpClient($d, 80)
$s = $c.GetStream()
$w = New-Object System.IO.StreamWriter($s)
$w.Write("GET / HTTP/1.1`r`nHost: postgresql.org`r`nConnection: close`r`n`r`n")
$w.Flush()
(New-Object System.IO.StreamReader($s)).ReadToEnd()
$c.Close()
```

**Вивід:**

```
HTTP/1.1 409 Conflict
Date: Mon, 05 Oct 2026 11:47:40 GMT
Content-Type: text/plain; charset=UTF-8
Content-Length: 16
Connection: close
X-Frame-Options: SAMEORIGIN
Referrer-Policy: same-origin
Cache-Control: private, max-age=0, no-store, no-cache, must-revalidate, post-check=0, pre-check=0
Expires: Thu, 01 Jan 1970 00:00:01 GMT
Server: cloudflare
CF-RAY: a45c3162be165bb3-VIE

error code: 1001
```

#### A.3.2. Неіснуюче ім'я в полі `Host`

**Команда:**

```
$d = "w3.org"
$c = New-Object System.Net.Sockets.TcpClient($d, 80)
$s = $c.GetStream()
$w = New-Object System.IO.StreamWriter($s)
$w.Write("GET / HTTP/1.1`r`nHost: opism-pr02.invalid`r`nConnection: close`r`n`r`n")
$w.Flush()
(New-Object System.IO.StreamReader($s)).ReadToEnd()
$c.Close()
```

**Вивід:**

```
HTTP/1.1 409 Conflict
Date: Mon, 05 Oct 2026 11:53:48 GMT
Content-Type: text/plain; charset=UTF-8
Content-Length: 16
Connection: close
X-Frame-Options: SAMEORIGIN
Referrer-Policy: same-origin
Cache-Control: private, max-age=0, no-store, no-cache, must-revalidate, post-check=0, pre-check=0
Expires: Thu, 01 Jan 1970 00:00:01 GMT
Server: cloudflare
CF-RAY: a45c3a5b18ffcd5a-VIE

error code: 1001
```

#### A.3.3. Запит без поля `Host` у версії 1.0

**Команда:**

```
$d = "w3.org"
$c = New-Object System.Net.Sockets.TcpClient($d, 80)
$s = $c.GetStream()
$w = New-Object System.IO.StreamWriter($s)
$w.Write("GET / HTTP/1.0`r`n`r`n")
$w.Flush()
(New-Object System.IO.StreamReader($s)).ReadToEnd()
$c.Close()
```

**Вивід:**

```
HTTP/1.1 403 Forbidden
Date: Mon, 05 Oct 2026 11:58:24 GMT
Content-Length: 57
Connection: close
Cache-Control: private, max-age=0, no-store, no-cache, must-revalidate, post-check=0, pre-check=0
Referrer-Policy: same-origin
Expires: Thu, 01 Jan 1970 00:00:01 GMT
CF-RAY: a45c411dd8325aa5-VIE

Cloudflare encountered an error processing this request:
```

Зведення результатів наведено в **Додатку Д**.

---

### Завдання A.4. Два запити в одному з'єднанні

**Команда:**

```
$d = "w3.org"
$c = New-Object System.Net.Sockets.TcpClient($d, 80)
$s = $c.GetStream()
$w = New-Object System.IO.StreamWriter($s)
$w.Write("GET /opism-pr02-12345 HTTP/1.1`r`nHost: $d`r`n`r`nGET / HTTP/1.1`r`nHost: $d`r`nConnection: close`r`n`r`n")
$w.Flush()
(New-Object System.IO.StreamReader($s)).ReadToEnd()
$c.Close()
```

**Вивід:**

```
HTTP/1.1 301 Moved Permanently
Date: Mon, 05 Oct 2026 12:06:43 GMT
Content-Type: text/html; charset=UTF-8
Transfer-Encoding: chunked
Connection: keep-alive
Location: https://www.w3.org/opism-pr02-12345
set-cookie: __cf_bm=puMeAsQm5u0jHR8obtd1iPSKFKXhCVgOYX5X_SsjgLo-1791202003.0701811-1.0.1.1-gXXR2kfuaOwsMmxhpiGD2NjpcyCJOp0VdHVQdWCHkjVBINCjTjIRrFgNR8IEYJ24UNaMpxYHN5fGOByTR6Wca98zHdxGAaByVR2iDe4EptMI4y9Vr8wTfr2yYcKWK3d4; HttpOnly; Path=/; Domain=w3.org; Expires=Mon, 05 Oct 2026 12:36:43 GMT
X-Content-Type-Options: nosniff
Server: cloudflare
CF-RAY: a45c4d472f602693-VIE
alt-svc: h3=":443"; ma=86400

a7
<html>
<head><title>301 Moved Permanently</title></head>
<body>
<center><h1>301 Moved Permanently</h1></center>
<hr><center>cloudflare</center>
</body>
</html>

0

HTTP/1.1 301 Moved Permanently
Date: Mon, 05 Oct 2026 12:06:43 GMT
Content-Type: text/html; charset=UTF-8
Transfer-Encoding: chunked
Connection: close
Location: https://www.w3.org/
set-cookie: __cf_bm=trE3kGJxqjZaGgrYv7IvyPJonKeVqVRd8Ik4mta0eSw-1791202003.0762234-1.0.1.1-SEULdHvtw5_xKUvUwUHvki9f2oFLI6PJdmxJaTif0vVB_ukuFOIW7HXLyCqDTIF1O4UObaiLp4uST3EctitXbC8_Q1IAmjCkQS8Bb39pfOlx_2ijGCpZrJFoKLsLxxQA; HttpOnly; Path=/; Domain=w3.org; Expires=Mon, 05 Oct 2026 12:36:43 GMT
X-Content-Type-Options: nosniff
Server: cloudflare
CF-RAY: a45c4d473f7c2693-VIE
alt-svc: h3=":443"; ma=86400

a7
<html>
<head><title>301 Moved Permanently</title></head>
<body>
<center><h1>301 Moved Permanently</h1></center>
<hr><center>cloudflare</center>
</body>
</html>

```

**Кількість отриманих відповідей:** 2

**Коди стану отриманих відповідей:**  ```301 Moved Permanently``` для обох

---

### Завдання A.5. Запит за допомогою клієнтської програми

**Команда:**

```
curl.exe -v --http1.1 http://w3.org/ -o /dev/null
```

**Вивід:**

```
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0* Host w3.org:80 was resolved.
* IPv6: (none)
* IPv4: 104.18.22.19, 104.18.23.19
*   Trying 104.18.22.19:80...
* Connected to w3.org (104.18.22.19) port 80
* using HTTP/1.x
> GET / HTTP/1.1
> Host: w3.org
> User-Agent: curl/8.13.0
> Accept: */*
>
* Request completely sent off
< HTTP/1.1 301 Moved Permanently
< Date: Mon, 05 Oct 2026 12:17:41 GMT
< Content-Type: text/html; charset=UTF-8
< Transfer-Encoding: chunked
< Connection: keep-alive
< Location: https://www.w3.org/
< set-cookie: __cf_bm=stVheJRxxShM7c8IV5J7g8G2_tnGpmPDVQg4GYw.ZFs-1791202661.4462788-1.0.1.1-up64trGcZW.kS3q5Yac0lN5dLPaZsPPOdFYhkZg3VLdlPKifLI3w9yH7WIrEuHVTQW2MG2KcvHKF7r6NRBMcvEfZaSvwwU.knREvVwpWkiWcYGz6awqpM2G19FLPvA9d; HttpOnly; Path=/; Domain=w3.org; Expires=Mon, 05 Oct 2026 12:47:41 GMT
< X-Content-Type-Options: nosniff
< Server: cloudflare
< CF-RAY: a45c5d5a0d7d7817-VIE
< alt-svc: h3=":443"; ma=86400
<
{ [178 bytes data]
Warning: Failed to open the file /dev/null: No such file or directory
* client returned ERROR on write of 167 bytes
* Failed reading the chunked-encoded stream
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0
* closing connection #0
curl: (23) client returned ERROR on write of 167 bytes
```

---

### Завдання A.6. Запит через захищене з'єднання

**Ресурс, на якому виконано завдання:** власний домен w3.org

**Підстава для використання резервного ресурсу (заповнюють за потреби):**

**Команда:**

```
$d = "w3.org"
$c = New-Object System.Net.Sockets.TcpClient($d, 443)
$s = New-Object System.Net.Security.SslStream($c.GetStream(), $false)
$s.AuthenticateAsClient($d)
$w = New-Object System.IO.StreamWriter($s)
$w.Write("GET / HTTP/1.1`r`nHost: $d`r`nConnection: close`r`n`r`n")
$w.Flush()
(New-Object System.IO.StreamReader($s)).ReadToEnd()
$c.Close()
```

**Набраний запит:**

```
GET / HTTP/1.1
Host: w3.org
Connection: close

```

**Вивід:**

```
HTTP/1.1 301 Moved Permanently
Date: Mon, 05 Oct 2026 14:26:49 GMT
Content-Type: text/html; charset=UTF-8
Transfer-Encoding: chunked
Connection: close
Location: https://www.w3.org/
set-cookie: __cf_bm=9HfhgiXF.AUVJFi5Sr6kDICqU2RRJwhb82Aikd2SqvY-1791210409.0621412-1.0.1.1-tBv7W646NeZuykTChfvdQlZ5TMBg0KlwlWJq6k1MwgpFSmv.oo60VYfzne.p4ULXB5aelekTGby2TOJHxpQHtAOc4YTz5CcAHsNQnflSyvaqJEKdgZ08cSffuBMy6dcp; HttpOnly; Secure; Path=/; Domain=w3.org; Expires=Mon, 05 Oct 2026 14:56:49 GMT
X-Content-Type-Options: nosniff
Server: cloudflare
CF-RAY: a45d1a809fb7c259-VIE
alt-svc: h3=":443"; ma=86400

a7
<html>
<head><title>301 Moved Permanently</title></head>
<body>
<center><h1>301 Moved Permanently</h1></center>
<hr><center>cloudflare</center>
</body>
</html>

0
```

---

## Частина B. Розбір полів заголовка

Розбирається відповідь, отримана в завданні A.1.

**Загальна кількість полів заголовка у відповіді:**

| № | Поле заголовка | Значення | Призначення (власне формулювання) | Походження: сервер / проміжний вузол / не визначено | Обґрунтування |
|---|---|---|---|---|---|
| 1 |```Date``` |Mon, 05 Oct 2026 11:23:29 GMT |Вказує точний час і дату генерації відповіді сервером. |сервер |Формується сервером під час обробки HTTP-запиту. |
| 2 |```Content-Type``` |text/html; charset=UTF-8 |Визначає медіа-тип переданого тіла повідомлення та кодування символів |сервер |Задається вебсервером залежно від типу ресурсу, що повертається. |
| 3 |```Transfer-Encoding``` |chunked |Вказує, що тіло повідомлення передається блоками |сервер |Використовується сервером для динамічного передавання даних невідомого заздалегідь розміру. |
| 4 |```Connection``` |close |Сигналізує про те, що поточне мережеве з'єднання буде закрите одразу після завершення передачі відповіді. |сервер |Задається сервером у відповідь на політику з'єднання. |
| 5 |```Location``` | https://www.w3.org/ |Вказує URL-адресу, на яку клієнт повинен перейти |сервер |Генерується сервером при перенаправленні запиту з HTTP на HTTPS. |
| 6 |```set-cookie``` |__cf_bm=xp8fJNWEo2MryvdM.v3PJ35LPEuhhmb_Ii.HHKNU3sA-1791199409.3378747-1.0.1.1-ShkkD0gRwRBZMR8kWje2NMNA46IhL0Oa0qj28w65k_hFqkZkUIojUqRR_wTtJy_s2HZWKxSS12gXwmVpzxrJaBWIP4wVW0yK7VKbih13TT7XBPoBBEuL3qAH2uilEOt9; HttpOnly; Path=/; Domain=w3.org; Expires=Mon, 05 Oct 2026 11:53:29 GMT |Передає браузеру значення файлу cookie для збереження у сховищі клієнта. |вузол |Додається захисною системою (CDN) проміжного вузла для перевірки ботів та керування сесією. |
| 7 |```X-Content-Type-Options``` |nosniff |Забороняє браузеру намагатися самостійно визначити тип файлу, якщо він не відповідає заявленому. |сервер |Заголовок безпеки, що додається для запобігання атакам типу MIME-confusion. |
| 8 |```Server``` |cloudflare |Інформує про програмне забезпечення або проксі-сервер, який обробив запит. |вузол |Формується на рівні шлюзу Cloudflare, який виступає проміжним проксі-сервером перед оригінальним сервером. |
| 9 |```CF-RAY``` |a45c0df45faaa5ef-VIE |Унікальний ідентифікатор запиту в мережі Cloudflare, що використовується для діагностики |вузол |Автоматично генерується балансувальником навантаження / проксі-сервером Cloudflare. |
| 10 |```alt-svc``` |h3=":443"; ma=86400 |Рекламує підтримку протоколу HTTP/3 на вказаному порті для наступних з'єднань. |вузол/сервер |Передається для оптимізації та переведення клієнта на швидший протокол. |

> Рядок наводять на кожне поле, яке реально надійшло. Зайві рядки вилучають, за потреби додають нові. Поле, походження якого встановити не вдалося, зазначають із позначкою «не визначено» та поясненням утруднення.

---

## Частина D. Висновки

Обсяг — 150–300 слів. Висновки спираються на власні спостереження.

**D.1.** Що з поведінки сервера виявилося неочевидним або несподіваним. Конкретно, з посиланням на рядок виводу.

Під час виконання завдання неочікуваною виявилася поведінка сервера ```w3.org```, який у відповідь на запит повертає статусний рядок ```HTTP/1.1 301 Moved Permanently``` та заголовок ```Location: [https://www.w3.org/](https://www.w3.org/)```. Це свідчить про примусове перенаправлення всього трафіку на захищену канонічну адресу.

**D.2.** Яке з полів заголовка викликало найбільше утруднення при визначенні походження (частина B) та з якої причини.

Найважче було розібратися із заголовком ```alt-svc``` - взагалі незрозуміло, чи це сам сервер його генерує, чи проксі-сервер, бо він відповідає за перехід на нові протоколи на кшталт HTTP/3.

**D.3.** Яке питання залишилося без відповіді після виконання роботи.

Після виконання роботи залишилося питання: чому після введення команди ```openssl.exe s_client -connect w3.org:443 -servername w3.org -crlf -quiet```у PowerShell постійно вибивало помилку, що такої команди немає.

---

## Контрольні питання

**1.** У завданні A.1 сервер не надсилав відповіді, доки не було введено порожній рядок. Чим це зумовлено?

Згідно зі стандартами протоколу HTTP (зокрема RFC), заголовки HTTP-запиту повинні відділятися від тіла запиту (або завершуватися, якщо тіла немає) за допомогою порожнього рядка ```(\r\n)```. Сервер очікує на цей порожній рядок, щоб зрозуміти, що передача заголовків завершена і можна починати обробку запиту. Доки користувач не введе цей рядок, сервер вважає запит неповним і продовжує чекати на його завершення.

**2.** Порівняйте результати завдань A.1, A.2 та A.3.1–A.3.3 (таблиця Додатка Д). За яких значень поля `Host` і за якої версії протоколу сервер обслуговує запит, а за яких — ні? Яку задачу розв'язує поле `Host`? Відповідь має посилатися на конкретні рядки ваших виводів.

Сервер обслуговує запит успішно тільки у випадку А.1, де використовувався правильний власний домен (w3.org) та версія протоколу HTTP/1.1 (код стану 301 Moved Permanently).
У всіх інших пробах, де поле Host було відсутнє або містило сторонній домен:У пробі А.2 (поле відсутнє, HTTP/1.1) сервер повернув помилку 400 Bad Request.   У пробах А.3.1 (postgresql.org) та А.3.2 (opism-pr02.invalid) сервер повернув код 409 Conflict.   У пробі А.3.3 (поле відсутнє, HTTP/1.0) сервер повернув код 403 Forbidden. 

Поле Host у протоколі HTTP/1.1 є обов'язковим. Воно вказує доменне ім'я цільового сервера, до якого надсилається запит.

**3.** Скільки відповідей надійшло у завданні A.4 і з якими кодами стану? Чи залежить відповідь сервера на порту 80 від запитаного шляху — і що це говорить про роль цього сервера? Якщо надійшла одна відповідь, знайдіть у ній поле заголовка, яке це пояснює, або зазначте, що такого поля немає. Якщо надійшло дві — що це означає для клієнтської програми, яка завантажує сторінку з великою кількістю вкладених ресурсів?

Надійшло дві відповіді, обидві з кодом стану 301 Moved Permanently. Не залежить - сервер перенаправляє всі запити, виконуючи роль редирект-шлюзу на HTTPS. Заголовок Connection: keep-alive дозволяє завантажувати велику кількість вкладених ресурсів через одне відкрите TCP-з'єднання.

**4.** Які поля заголовка програма `curl` додала самостійно (завдання A.5)? Ці поля не є обов'язковими — сервер відповів і без них у завданні A.1. З якою метою їх додано?

Програма curl самостійно додала заголовки User-Agent: curl/8.13.0 та Accept: */*. Мета їх додавання User-Agent ідентифікує тип клієнтської програми для сервера, а Accept: */* вказує, які медіа-типи даних клієнт здатний прийняти. 

**5.** За якими ознаками у вашому виводі виявляється присутність проміжного вузла? Якщо таких ознак не виявлено, поясніть, що з цього випливає.

Ознаки проміжного вузла це наявність заголовків Server: cloudflare, CF-RAY: a45c5d5a0d7d7817-VIE та файлу cookie __cf_bm.

**6.** Три рядки, про які не йшлося на лекції, наведено в **Додатку В**.

---

## Додаток В. Відповіді на питання 6

| № | Рядок виводу | Джерело (номер завдання) |
|---|---|---|
| 1 |X-Content-Type-Options: nosniff |А1 |
| 2 |Referrer-Policy: same-origin |А3.1 |
| 3 |alt-svc: h3=":443"; ma=86400 |А1 |

---

## Додаток Д. Зведення результатів завдання A.3

**Вузол, з яким установлювалося з'єднання (у всіх пробах однаковий):**

| Проба | Значення поля `Host` | Версія | Код стану | Обсяг тіла відповіді | Збігається з A.1 (так / ні) |
|---|---|---|---|---|---|
| A.1 (вихідна) |w3.org | 1.1 |301 |chunked | — |
| A.2 | поле відсутнє | 1.1 |400 |155 бай|ні |
| A.3.1 |postgresql.org | 1.1 |409 |16 байт |ні |
| A.3.2 | `opism-pr02.invalid` | 1.1 |409 |16 байт |ні |
| A.3.3 | поле відсутнє | 1.0 |403 |57 байт |ні |

**Висновок за таблицею (2–4 речення):** що саме змінювалося у запиті від проби до проби і як на це реагував сервер.

У запитах від проби до проби змінювалося значення заголовка Host (або воно виключалося взагалі) та версія протоколу HTTP. Сервер реагував на ці зміни по-різному, за відсутності поля Host або при передачі сторонніх доменів видавав помилки (коди 400, 409, 403), тоді як правильний вихідний запит успішно перенаправляв на адресу зі статусом 301.

---

## Декларування використання технологій штучного інтелекту

Для цієї роботи встановлено **рівень Р3 — ШІ як співвиконавець**.

Виводи команд частини A не можуть бути згенеровані та мають бути отримані внаслідок фактичного виконання команд.

**Чи використовувалися технології ШІ під час виконання роботи:** <так

Якщо так, заповнюють таблицю. Якщо ні, таблицю вилучають.

| № | Інструмент (назва, версія) | Етап роботи | Дослівний текст запиту (промпту) | Як використано результат |
|---|---|---|---|---|
| 1 |Gemini |Вирішення технічних проблем виконання завдань (частина А) |давай вернемся до а6, це проблема саме з повершел, чи щось не так я роблю |Використано для розуміння особливостей роботи утиліти openssl |
| 2 |Gemini |Формування результатів та висновку |поможи заповнити таблицю і написати висновоу відповівши на ці питання  |Використано для написання висновків і оформлення звітних даних на основі виконаних завдань. |


**Підтвердження:** усі виводи команд, наведені в частині A, отримано внаслідок фактичного виконання команд на зазначеному індивідуальному домені.

---

*ОПІСМ (ОК-13) · Практична робота № 2 · бланк звіту*
