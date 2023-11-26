# Домашнее задание к занятию «Белый хакинг: тестирование на проникновение» - Леонид Хорошев

### Цель задания

Белый хакер должен знать, как правильно проводить тестирование на проникновение в современных веб-технологиях. И он точно сможет анализировать уязвимости, чтобы обезопасить свою технику.

В результате выполнения задания вы сможете обнаружить, эксплуатировать и тестировать веб-технологии на основе методологии OWASP.

------

### Инструкция к заданию

1. Загрузите операционную систему с образом bWAPP для изучения OWASP Top 10.
2. Следуя инструкции из вебинара, установите и подключитесь к виртуальной машине.
3. Выполните задание.

------

### Задание 1: анализ уязвимостей OWASP

1. Для выполнения работы запустите образы операционных систем на базе ОС [Debian](https://www.kali.org/get-kali/#kali-virtual-machines) и [bWAPP](http://www.itsecgames.com/download.htm).

Настроен и запущен "испытательный стенд" из 2 виртуальных машин Kali linux и Bee-Box.
![Alt text](https://github.com/LeonidKhoroshev/sibfree-homeworks/blob/main/3/screenshots/pentest1.png)

2. Настройте сетевой доступ: Kali Linux — `192.168.0.1`, Bee-Box — `192.168.0.2`.

Виртуальные машины взаимодействуют через адаптер существующей виртуальной сети 192.168.56.0, который в свою очередь использует сервер DHCP, поэтому адреса машин могут быть изменены.
![Alt text](https://github.com/LeonidKhoroshev/sibfree-homeworks/blob/main/3/screenshots/pentest2.png)

В нашем случае назначены следующие ip адреса:
- Kali - '192.168.56.102'
- Bee-Box - '192.168.56.103'

Проверяем, что сеть работает и адреса доступны:
![Alt text](https://github.com/LeonidKhoroshev/sibfree-homeworks/blob/main/3/screenshots/pentest3.png)

4. Запустите Burp Suite. Включите проксирование:

Утилита включается через GUI Kali linux:
![Alt text](https://github.com/LeonidKhoroshev/sibfree-homeworks/blob/main/3/screenshots/pentest4.png)

5. Получите доступ к сайту Bee-Box, открыв браузер в системе Kali Linux по адресу `192.168.56.103`. Логин и пароль для доступа: `bee/bug`:
![Alt text](https://github.com/LeonidKhoroshev/sibfree-homeworks/blob/main/3/screenshots/pentest5.png)

6. Выберите атаки типа SQL injection:
![Alt text](https://github.com/LeonidKhoroshev/sibfree-homeworks/blob/main/3/screenshots/pentest6.png)

7. Проведите SQL injection типа `OR ‘1’=’1` и получите полный ответ от сервиса:
![Alt text](https://github.com/LeonidKhoroshev/sibfree-homeworks/blob/main/3/screenshots/pentest7.png)
Нарушен синтаксис SQL, значит, что на этой веб-странице существует данный тип инъекции.
   
Далее вводим поочередно [запросы](https://habr.com/ru/articles/250551/) для получения различной информации, например можем узнать логин.пароль для входа в базу данных:
```
http://192.168.56.103/bWAPP/sqli_1.php?title=hulk%27%20union%20select%201,database%28%29,user%28%29,4,password,6,7%20from%20users%20--%20&action=search
```
![Alt text](https://github.com/LeonidKhoroshev/sibfree-homeworks/blob/main/3/screenshots/pentest8.png)

8. Выберите атаки типа Cross-Site Scripting (XSS).
9. Проведите атаку путём injection HTML-кода типа `alert(‘1’);`.
10. Проанализируете перехваченные Burp Suite пакеты и посмотрите, что передаёт клиент серверу и что отвечает сервер. 
11. Приведите снимок экрана в отчёт и выводы по анализу.

Пункты 8-10 выполняем аналогично 6-7 через GUI программного обеспечения и веб-интерсфейс.
![Alt text](https://github.com/LeonidKhoroshev/sibfree-homeworks/blob/main/3/screenshots/pentest9.png)

В веб интерфейсе введены несуществующие учетные данные пользователя (пример аналогичен лекции). И далее при нажатии кнопки `go` на странице аутентификации получаем введенные нами данные одной строкой. Следовательно, при вводе логина существующего пользователя есть большие шансы получить его пароль ввиде вывода строки на экран.
  
12. Выберите атаки типа Server-Side Includes (SSI).
13. Проведите атаку путём injection `<! –-#exec cmd=“whoami” —>`.
14. Проанализируете перехваченные Burp Suite пакеты и посмотрите, что передаёт клиент серверу и что отвечает сервер.

Пункты 12-13 выполняем аналогично предыдущим.
![Alt text](https://github.com/LeonidKhoroshev/sibfree-homeworks/blob/main/3/screenshots/pentest10.png)

В данном случае получен ip адрес удаленного хоста (не Bee-Box, а Kali), но также возможен и подбор пароля и вывод полной информации о [пользователе](https://blog.hackerassociate.com/how-to-do-server-side-includes-injection-ssi-using-bwapp/)



