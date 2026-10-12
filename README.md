[ОПІСМ_практична_02_бланк.md](https://github.com/user-attachments/files/33259109/_._02_.md)
# Практична робота № 2

**Дисципліна:** Основи побудови інформаційних систем та мереж (ОК-13)

**Тема:** Структура повідомлень прикладного протоколу HTTP. Формування запиту вручну

| Поле | Значення |
|---|---|
| Студент (прізвище, ім'я, по батькові) | Єльцов Олег Дмитрович|
| Група |2.01 |
| Номер варіанта |6 |
| Індивідуальний домен |postgresql.org |
| «Чужий» домен для завдання A.3.1 (варіант ± 20) | |
| Середовище виконання | Власний комп’ютер (Windows) |
| Дата виконання | 09.10.2026|

> Бланк заповнюють, не змінюючи структури розділів. Порожні заготовки блоків коду замінюють власними виводами. Позначки-підказки в кутових дужках вилучають.

---

## Частина A. Збір експериментальних даних

### Завдання A.1. Формування запиту вручну

**Команда:**

```
<$d = "postgresql.org"
>> $c = New-Object System.Net.Sockets.TcpClient($d, 80)
>> $s = $c.GetStream()
>> $w = New-Object System.IO.StreamWriter($s)
>> $w.Write("GET / HTTP/1.1`r`nHost: $d`r`nConnection: close`r`n`r`n")
>> $w.Flush()
>> (New-Object System.IO.StreamReader($s)).ReadToEnd()
>> $c.Close()>
```

**Набраний запит:**

```
<GET / HTTP/1.1
Host: postgresql.org
Connection: close>
```

**Відповідь:**

```
<PS C:\Users\CyberPc> $d = "postgresql.org"
>> $c = New-Object System.Net.Sockets.TcpClient($d, 80)
>> $s = $c.GetStream()
>> $w = New-Object System.IO.StreamWriter($s)
>> $w.Write("GET / HTTP/1.1`r`nHost: $d`r`nConnection: close`r`n`r`n")
>> $w.Flush()
>> (New-Object System.IO.StreamReader($s)).ReadToEnd()
>> $c.Close()
HTTP/1.1 301 Moved Permanently
Date: Mon, 12 Oct 2026 05:36:42 GMT
Server: Varnish
X-Varnish: 428901065
Location: https://www.postgresql.org/
Content-Length: 0
Connection: close>
```

---

### Завдання A.2. Запит без поля `Host` у версії 1.1

**Команда:**

```
<$d = "postgresql.org"
>> $c = New-Object System.Net.Sockets.TcpClient($d, 80)
>> $s = $c.GetStream()
>> $w = New-Object System.IO.StreamWriter($s)
>> $w.Write("GET / HTTP/1.1`r`nConnection: close`r`n`r`n")
>> $w.Flush()
>> (New-Object System.IO.StreamReader($s)).ReadToEnd()
>> $c.Close()>
```

**Вивід:**

```
<PS C:\Users\CyberPc> $d = "postgresql.org"
>> $c = New-Object System.Net.Sockets.TcpClient($d, 80)
>> $s = $c.GetStream()
>> $w = New-Object System.IO.StreamWriter($s)
>> $w.Write("GET / HTTP/1.1`r`nConnection: close`r`n`r`n")
>> $w.Flush()
>> (New-Object System.IO.StreamReader($s)).ReadToEnd()
>> $c.Close()
HTTP/1.1 301 Moved Permanently
Date: Mon, 12 Oct 2026 05:43:05 GMT
Server: Varnish
X-Varnish: 428967837
Location: https://www.postgresql.org/
Content-Length: 0
Connection: close
>
```

---

### Завдання A.3. Вплив поля `Host` на відповідь сервера

#### A.3.1. Чуже доменне ім'я в полі `Host`

**Команда:**

```
<PS C:\Users\CyberPc> $d = "postgresql.org"
PS C:\Users\CyberPc> $foreign = "w3.org"
>> $c = New-Object System.Net.Sockets.TcpClient($d, 80)
>> $s = $c.GetStream()
>> $w = New-Object System.IO.StreamWriter($s);$w.Write("GET / HTTP/1.1`r`nHost: $foreign`r`nConnection: close`r`n`r`n")
>> $w.Flush()
>> (New-Object System.IO.StreamReader($s)).ReadToEnd()  eadToEnd();
>> $c.Close()>
```

**Вивід:**

```
<PS C:\Users\CyberPc> $d = "postgresql.org"
PS C:\Users\CyberPc> $foreign = "w3.org"
>> $c = New-Object System.Net.Sockets.TcpClient($d, 80)
>> $s = $c.GetStream()
>> $w = New-Object System.IO.StreamWriter($s);$w.Write("GET / HTTP/1.1`r`nHost: $foreign`r`nConnection: close`r`n`r`n")
>> $w.Flush()
>> (New-Object System.IO.StreamReader($s)).ReadToEnd()  eadToEnd();
>> $c.Close()
HTTP/1.1 301 Moved Permanently
Date: Mon, 12 Oct 2026 06:52:33 GMT
Server: Varnish
X-Varnish: 448856610
Location: https://www.postgresql.org/
Content-Length: 0
Connection: close


PS C:\Users\CyberPc>>
```

#### A.3.2. Неіснуюче ім'я в полі `Host`

**Команда:**

```
<PS C:\Users\CyberPc> $d = "postgresql.org"
>> $c = New-Object System.Net.Sockets.TcpClient($d, 80)
>> $s = $c.GetStream()
>> $req = "GET / HTTP/1.1`r`nHost: opism-pr02.invalid`r`nConnection: close`r`n`r`n"
>> $b = [System.Text.Encoding]::ASCII.GetBytes($req);$s.Write($b, 0,$b.Length)
>> (New-Object System.IO.StreamReader($s)).ReadToEnd();$c.Close()>
```

**Вивід:**

```
<PS C:\Users\CyberPc> $d = "postgresql.org"
>> $c = New-Object System.Net.Sockets.TcpClient($d, 80)
>> $s = $c.GetStream()
>> $req = "GET / HTTP/1.1`r`nHost: opism-pr02.invalid`r`nConnection: close`r`n`r`n"
>> $b = [System.Text.Encoding]::ASCII.GetBytes($req);$s.Write($b, 0,$b.Length)
>> (New-Object System.IO.StreamReader($s)).ReadToEnd();$c.Close()
HTTP/1.1 301 Moved Permanently
Date: Mon, 12 Oct 2026 07:01:58 GMT
Server: Varnish
X-Varnish: 424843062
Location: https://www.postgresql.org/
Content-Length: 0
Connection: close>
```

#### A.3.3. Запит без поля `Host` у версії 1.0

**Команда:**

```
<PS C:\Users\CyberPc> $d = "postgresql.org"
>> $c = New-Object System.Net.Sockets.TcpClient($d, 80)
>> $s = $c.GetStream()
>> $req = "GET / HTTP/1.0`r`n`r`n"
>> $b = [System.Text.Encoding]::ASCII.GetBytes($req)
>> $s.Write($b, 0,$b.Length)
>> (New-Object System.IO.StreamReader($s)).ReadToEnd();$c.Close()>
```

**Вивід:**

```
<PS C:\Users\CyberPc> $d = "postgresql.org"
>> $c = New-Object System.Net.Sockets.TcpClient($d, 80)
>> $s = $c.GetStream()
>> $req = "GET / HTTP/1.0`r`n`r`n"
>> $b = [System.Text.Encoding]::ASCII.GetBytes($req)
>> $s.Write($b, 0,$b.Length)
>> (New-Object System.IO.StreamReader($s)).ReadToEnd();$c.Close()
HTTP/1.1 301 Moved Permanently
Date: Mon, 12 Oct 2026 07:03:44 GMT
Server: Varnish
X-Varnish: 418652609
Location: https://www.postgresql.org/
Content-Length: 0
Connection: close
>
```

Зведення результатів наведено в **Додатку Д**.

---

### Завдання A.4. Два запити в одному з'єднанні

**Команда:**

```
<$d = "postgresql.org"
>> $c = New-Object System.Net.Sockets.TcpClient($d, 80)
>> $s = $c.GetStream()
>> $req = "GET /opism-pr02-12345 HTTP/1.1`r`nHost: $d`r`n`r`nGET / HTTP/1.1`r`nHost: $d`r`nConnection: close`r`n`r`n"
>> $b = [System.Text.Encoding]::ASCII.GetBytes($req)
>> $s.Write($b, 0,$b.Length)
>> (New-Object System.IO.StreamReader($s)).ReadToEnd()
>> $c.Close()>
```

**Вивід:**

```
<PS C:\Users\CyberPc> $d = "postgresql.org"
>> $c = New-Object System.Net.Sockets.TcpClient($d, 80)
>> $s = $c.GetStream()
>> $req = "GET /opism-pr02-12345 HTTP/1.1`r`nHost: $d`r`n`r`nGET / HTTP/1.1`r`nHost: $d`r`nConnection: close`r`n`r`n"
>> $b = [System.Text.Encoding]::ASCII.GetBytes($req)
>> $s.Write($b, 0,$b.Length)
>> (New-Object System.IO.StreamReader($s)).ReadToEnd()
>> $c.Close()
HTTP/1.1 301 Moved Permanently
Date: Mon, 12 Oct 2026 07:27:16 GMT
Server: Varnish
X-Varnish: 453674176
Location: https://www.postgresql.org/opism-pr02-12345
Content-Length: 0
Connection: keep-alive

HTTP/1.1 301 Moved Permanently
Date: Mon, 12 Oct 2026 07:27:16 GMT
Server: Varnish
X-Varnish: 453674177
Location: https://www.postgresql.org/
Content-Length: 0
Connection: close


PS C:\Users\CyberPc>>
```

**Кількість отриманих відповідей: 2**

**Коди стану отриманих відповідей:301;301**

---

### Завдання A.5. Запит за допомогою клієнтської програми

**Команда:**

```
<curl.exe -v --http1.1 http://postgresql.org/ -o /dev/null>
```

**Вивід:**

```
<PS C:\Users\CyberPc>  curl.exe -v --http1.1 http://postgresql.org/ -o /dev/null
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0*   Trying 217.196.149.50:80...
* Connected to postgresql.org (217.196.149.50) port 80 (#0)
> GET / HTTP/1.1
> Host: postgresql.org
> User-Agent: curl/7.79.1
> Accept: */*
>
* Mark bundle as not supporting multiuse
< HTTP/1.1 301 Moved Permanently
< Date: Mon, 12 Oct 2026 07:29:25 GMT
< Server: Varnish
< X-Varnish: 451807489
< Location: https://www.postgresql.org/
< Content-Length: 0
< Connection: keep-alive
<
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0
* Connection #0 to host postgresql.org left intact
Warning: Failed to create the file /dev/null: No such file or directory
PS C:\Users\CyberPc>>
```

---

### Завдання A.6. Запит через захищене з'єднання

**Ресурс, на якому виконано завдання:** <власний домен / `iana.org`>

**Підстава для використання резервного ресурсу (заповнюють за потреби):**

**Команда:**

```
<curl.exe -v --http1.1 https://iana.org/>
```

**Набраний запит:**

```
<GET / HTTP/1.1
Host: iana.org
Connection: close>
```

**Вивід:**

```
<PS C:\Users\CyberPc> curl.exe -v --http1.1 https://iana.org/
*   Trying 192.0.43.8:443...
* Connected to iana.org (192.0.43.8) port 443 (#0)
* schannel: disabled automatic use of client certificate
* schannel: ALPN, offering http/1.1
* ALPN, server did not agree to a protocol
> GET / HTTP/1.1
> Host: iana.org
> User-Agent: curl/7.79.1
> Accept: */*
>
* Mark bundle as not supporting multiuse
< HTTP/1.1 301 Moved Permanently
< Date: Mon, 12 Oct 2026 07:33:20 GMT
< Server: Apache
< Location: https://www.iana.org/
< Cache-Control: max-age=345600
< Expires: Fri, 16 Oct 2026 07:33:20 GMT
< Content-Length: 229
< Content-Type: text/html; charset=iso-8859-1
<
<!DOCTYPE HTML PUBLIC "-//IETF//DTD HTML 2.0//EN">
<html><head>
<title>301 Moved Permanently</title>
</head><body>
<h1>Moved Permanently</h1>
<p>The document has moved <a href="https://www.iana.org/">here</a>.</p>
</body></html>
* Connection #0 to host iana.org left intact
PS C:\Users\CyberPc>
```

---

## Частина B. Розбір полів заголовка

Розбирається відповідь, отримана в завданні A.1.

**Загальна кількість полів заголовка у відповіді:**

| № | Поле заголовка | Значення | Призначення (власне формулювання) | Походження: сервер / проміжний вузол / не визначено | Обґрунтування |
|---|---|---|---|---|---|
| 1 | | | | | |
| 2 | | | | | |
| 3 | | | | | |
| 4 | | | | | |
| 5 | | | | | |
| 6 | | | | | |
| 7 | | | | | |
| 8 | | | | | |

> Рядок наводять на кожне поле, яке реально надійшло. Зайві рядки вилучають, за потреби додають нові. Поле, походження якого встановити не вдалося, зазначають із позначкою «не визначено» та поясненням утруднення.

---

## Частина D. Висновки

Обсяг — 150–300 слів. Висновки спираються на власні спостереження.

**D.1.** Що з поведінки сервера виявилося неочевидним або несподіваним. Конкретно, з посиланням на рядок виводу.

<текст>

**D.2.** Яке з полів заголовка викликало найбільше утруднення при визначенні походження (частина B) та з якої причини.

<текст>

**D.3.** Яке питання залишилося без відповіді після виконання роботи.

<текст>

---

## Контрольні питання

**1.** У завданні A.1 сервер не надсилав відповіді, доки не було введено порожній рядок. Чим це зумовлено?

<відповідь>

**2.** Порівняйте результати завдань A.1, A.2 та A.3.1–A.3.3 (таблиця Додатка Д). За яких значень поля `Host` і за якої версії протоколу сервер обслуговує запит, а за яких — ні? Яку задачу розв'язує поле `Host`? Відповідь має посилатися на конкретні рядки ваших виводів.

<відповідь>

**3.** Скільки відповідей надійшло у завданні A.4 і з якими кодами стану? Чи залежить відповідь сервера на порту 80 від запитаного шляху — і що це говорить про роль цього сервера? Якщо надійшла одна відповідь, знайдіть у ній поле заголовка, яке це пояснює, або зазначте, що такого поля немає. Якщо надійшло дві — що це означає для клієнтської програми, яка завантажує сторінку з великою кількістю вкладених ресурсів?

<відповідь>

**4.** Які поля заголовка програма `curl` додала самостійно (завдання A.5)? Ці поля не є обов'язковими — сервер відповів і без них у завданні A.1. З якою метою їх додано?

<відповідь>

**5.** За якими ознаками у вашому виводі виявляється присутність проміжного вузла? Якщо таких ознак не виявлено, поясніть, що з цього випливає.

<відповідь>

**6.** Три рядки, про які не йшлося на лекції, наведено в **Додатку В**.

---

## Додаток В. Відповіді на питання 6

| № | Рядок виводу | Джерело (номер завдання) |
|---|---|---|
| 1 | | |
| 2 | | |
| 3 | | |

---

## Додаток Д. Зведення результатів завдання A.3

**Вузол, з яким установлювалося з'єднання (у всіх пробах однаковий):**

| Проба | Значення поля `Host` | Версія | Код стану | Обсяг тіла відповіді | Збігається з A.1 (так / ні) |
|---|---|---|---|---|---|
| A.1 (вихідна) | | 1.1 | | | — |
| A.2 | поле відсутнє | 1.1 | | | |
| A.3.1 | | 1.1 | | | |
| A.3.2 | `opism-pr02.invalid` | 1.1 | | | |
| A.3.3 | поле відсутнє | 1.0 | | | |

**Висновок за таблицею (2–4 речення):** що саме змінювалося у запиті від проби до проби і як на це реагував сервер.

<текст>

---

## Декларування використання технологій штучного інтелекту

Для цієї роботи встановлено **рівень Р3 — ШІ як співвиконавець**.

Виводи команд частини A не можуть бути згенеровані та мають бути отримані внаслідок фактичного виконання команд.

**Чи використовувалися технології ШІ під час виконання роботи:** <так / ні>

Якщо так, заповнюють таблицю. Якщо ні, таблицю вилучають.

| № | Інструмент (назва, версія) | Етап роботи | Дослівний текст запиту (промпту) | Як використано результат |
|---|---|---|---|---|
| 1 | | | | |
| 2 | | | | |

**Підтвердження:** усі виводи команд, наведені в частині A, отримано внаслідок фактичного виконання команд на зазначеному індивідуальному домені.

---

*ОПІСМ (ОК-13) · Практична робота № 2 · бланк звіту*
