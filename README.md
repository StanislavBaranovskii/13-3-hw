# Домашнее задание к занятию 13.3. «`Защита сети`» - `Станислав Барановский`

**Домашнее задание выполните в Google Docs или в md-файле в вашем репозитории GitHub.** 

Для оформления вашего решения в GitHub можете воспользоваться [шаблоном](https://github.com/netology-code/sys-pattern-homework).

Название файла Google Docs должно содержать номер лекции и фамилию студента. Пример названия: «13.2. Защита хоста — Александр Александров».

Перед тем как выслать ссылку, убедитесь, что её содержимое не является приватным, то есть открыто на просмотр всем, у кого есть ссылка. Если необходимо прикрепить дополнительные ссылки, просто добавьте их в свой Google Docs.

Любые вопросы по решению задач задавайте в чате учебной группы.

------

## Подготовка к выполнению заданий

1. Подготовка защищаемой системы:

- установите **Suricata**,
- установите **Fail2Ban**.

```bash
sudo apt update && sudo apt upgrade
sudo apt install software-properties-common
sudo add-apt-repository ppa:oisf/suricata-stable
sudo apt update
sudo apt install -y suricata
sudo suricata-update
systemctl status suricata.service
suricata -V

sudo nano /etc/suricata/suricata.yaml # EXTERNAL_NET: "any"
sudo systemctl restart suricata.service
systemctl status suricata.service


sudo apt install -y fail2ban
systemctl status fail2ban
fail2ban --version
```

2. Подготовка системы злоумышленника: установите **nmap** и **thc-hydra** либо скачайте и установите **Kali linux**.

Обе системы должны находится в одной подсети.

------

## Задание 1

Проведите разведку системы и определите, какие сетевые службы запущены на защищаемой системе:

**sudo nmap -sA < ip-адрес >**

**sudo nmap -sT < ip-адрес >**

**sudo nmap -sS < ip-адрес >**

**sudo nmap -sV < ip-адрес >**

По желанию можете поэкспериментировать с опциями: https://nmap.org/man/ru/man-briefoptions.html.


*В качестве ответа пришлите события, которые попали в логи Suricata и Fail2Ban, прокомментируйте результат.*

```script
sudo suricata -c /etc/suricata/suricata.yaml -i enp0s3
sudo tail -f /var/log/suricata/fast.log
```
Сканирование nmap -sA в логи suricata не попало

**Сканирование nmap -sT**
![Сканирование nmap -sT](https://github.com/StanislavBaranovskii/13-3-hw/blob/main/img/13-3-1-nmap-sT.png "Сканирование nmap -sT")

**Сканирование nmap -sS**
![Сканирование nmap -sS](https://github.com/StanislavBaranovskii/13-3-hw/blob/main/img/13-3-1-nmap-sS.png "Сканирование nmap -sS")

**Сканирование nmap -sV**
![Сканирование nmap -sV](https://github.com/StanislavBaranovskii/13-3-hw/blob/main/img/13-3-1-nmap-sV.png "Сканирование nmap -sV")


------

## Задание 2

Проведите атаку на подбор пароля для службы SSH:

**hydra -L users.txt -P pass.txt < ip-адрес > ssh**

1. Настройка **hydra**: 
 
 - создайте два файла: **users.txt** и **pass.txt**;
 - в каждой строчке первого файла должны быть имена пользователей, второго — пароли. В нашем случае это могут быть случайные строки, но ради эксперимента можете добавить имя и пароль существующего пользователя.

Дополнительная информация по **hydra**: https://kali.tools/?p=1847.

2. Включение защиты SSH для Fail2Ban:

-  открыть файл /etc/fail2ban/jail.conf,
-  найти секцию **ssh**,
-  установить **enabled**  в **true**.

Дополнительная информация по **Fail2Ban**:https://putty.org.ru/articles/fail2ban-ssh.html.

*В качестве ответа пришлите события, которые попали в логи Suricata и Fail2Ban, прокомментируйте результат.*

------
