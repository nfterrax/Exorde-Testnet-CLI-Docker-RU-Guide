# РУКОВОДСТВО ПО EXORDE TESTNET ДЛЯ CLI + DOCKER

> ⚠️ Помните, что вам необходимо постоянно следить за обновлениями, которые публикуются в нашем [Discord](https://discord.gg/ExordeLabs) на канале #testnet. Если вышло обновление, но вы его проигнорируете, вы можете просто впустую потратить свои ресурсы (время; вычислительные мощности; средства, если используете арендованный vps, и т.п.).
> 
> Данный гайд будет обновляться и дополняться.


## ⭕ СИСТЕМНЫЕ ТРЕБОВАНИЯ

**CPU:** 2 cores

**RAM:** 4 Gb

**SSD:** 1 Gb


## ⭕ АКТУАЛЬНАЯ ВЕРСИЯ МОДУЛЯ: 1.3.4a


## ⭕ Краткое и быстрое руководство по установке. Последовательно выполняйте команды одну за другой:

1
```
apt update
```
2
```
apt install git
```
3
```
apt install apt-transport-https ca-certificates software-properties-common curl
```
4
```
curl -f -s -S -L https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```
5
```
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable"
```
6
```
apt update
```
7
```
cd /root
```
8
```
sudo apt install docker-ce docker-ce-cli containerd.io docker-compose-plugin -y
```
9
```
sudo systemctl status docker
```
10
```
docker run -d --restart unless-stopped --pull always --name <ИМЯ_КОНТЕЙНЕРА> rg.fr-par.scw.cloud/exorde-labs/exorde-cli -m <ВАШ_ОСНОВНОЙ_ETH_КОШЕЛЕК> -l <УРОВЕНЬ ЛОГОВ>
```
> **Переменные:**
> - <ИМЯ_КОНТЕЙНЕРА> - придумайте любое название вашего контейнера, например: **exorde-cli_1**
> - <ВАШ_ОСНОВНОЙ_ETH_КОШЕЛЕК> - Небиржевой адрес Ethereum (ETH) Mainnet кошелька. (Например, из MetaMask).
> - <УРОВЕНЬ ЛОГОВ> -  прописывается одной из пяти цифр от 0 до 4 и означает:
>      
>      = 0 - нет логов
>  
>      = 1 - общие логи
>  
>      = 2 - логи валидации
>  
>      = 3 - логи валидации + скраппинг
>  
>      = 4 - подробные логи валидации + скраппинг (например, для устранения неполадок)
>  
> Оптимально использовать: 2

Таким образом, пример готовой команды для запуска одного модуля (контейнера) выглядит следующим образом:
```
docker run -d --restart unless-stopped --pull always --name exorde-cli_1 rg.fr-par.scw.cloud/exorde-labs/exorde-cli -m 0x16f177263988fF6fc8999013BD9bCB70F39b42d3 -l 2
```

## ⭕ ПРИМЕЧАНИЯ

Готово! Ваш модуль запущен в контейнере в фоновом режиме. Теперь вы можете оставить все как есть, закрыть терминал CLI, и модуль продолжит работать. Но помните, что нужно следить за обновлениями в Discord и за работоспособностью каждого модуля по отдельности!

Чтобы запустить дополнительную копию модуля, просто повторите ту же команду, но с другим ИМЕНЕМ_КОНТЕЙНЕРА:
```
docker run -d --restart unless-stopped --pull always --name <ИМЯ_КОНТЕЙНЕРА_2> rg.fr-par.scw.cloud/exorde-labs/exorde-cli -m <ВАШ_ОСНОВНОЙ_ETH_КОШЕЛЕК> -l <УРОВЕНЬ ЛОГОВ>
```
Например:
```
docker run -d --restart unless-stopped --pull always --name exorde-cli_2 rg.fr-par.scw.cloud/exorde-labs/exorde-cli -m 0x16f177263988fF6fc8999013BD9bCB70F39b42d3 -l 2
```
Сколько раз вы ввeдете эту команду с разными именами, столько модулей вы запустите.


> Не забывайте, что контейнеры = модули иногда могут останавливаться.

Проверить количество запущенных модулей и их статус (+ узнать <container_id> каждого модуля):

```
docker ps -a
```

Если статус контейнера "Exited", значит он сейчас не работает. Нужно сделать рестарт:

```
docker restart <container_id>
```
или
```
docker restart <ИМЯ_КОНТЕЙНЕРА>
```
Например:
```
docker restart 1f77bd5b1111
```
или
```
docker restart exorde-cli
```
Для рестарта сразу всех контейнеров = модулей используйте команду:
```
docker restart $(docker ps -a -q)
```

Для нормальной работы модулей необходимо, чтобы они не перегружали вашу систему. Поэтому запускайте такое количество модулей, которое будет работать оптимально для ваших технических характеристик. Соответственно, в случае обнаружения работы системы на износ, остановите и удалите лишние модули.

Просмотр общей статистики вашего сервера (словно Диспетчер Задач для Windows):
```
top
```

Для удаления контейнера/(ов) нужно сначала его/их остановить (кроме варианта использования команды "грубого" удаления).  

Остановить контейнер = модуль:
```
docker stop <container_id>
```
или
```
docker stop <ИМЯ_КОНТЕЙНЕРА>
```
Остановить все контейнеры = модули:
```
docker stop $(docker ps -a -q)
```
Удалить контейнер = модуль:
```
docker rm <container_id>
```
или
```
docker rm <ИМЯ_КОНТЕЙНЕРА>
```
Удалить все контейнеры:
```
docker rm $(docker ps -a -q)
```
"Грубое" удаление контейнера = модуля (без предварительной остановки):
```
docker rm <container_id> --force
```
или
```
docker rm <ИМЯ_КОНТЕЙНЕРА> --force
```
"Грубое" удаление всех контейнеров = модулей (без предварительной остановки):
```
docker rm $(docker ps -a -q) --force
```

## ⭕ Чтобы отобразить процессы = логи = журналы, происходящие в контейнере, открытом в фоновом режиме, необходимо ввести команду:
```
docker logs --follow  <container_id>
```
или
```
docker logs --follow  <ИМЯ_КОНТЕЙНЕРА>
```
Например:
```
docker logs --follow  1f77bd5b1111
```
или
```
docker logs --follow  exorde-cli
```
Процессы = логи = журналы можно смотреть только для каждого контейнера = модуля отдельно.

В них будут отображаться:
- текущая версия
- происходящие процессы (например, валидация, голосование и т.п.)
- репутация (REP) и реварды (системное сообщение с вашей статистикой автоматически появляется каждые 10 минут)
![image](https://user-images.githubusercontent.com/97410466/203428442-d0c91be6-957d-42ea-af9a-8b68b1f6bb11.png)
- ошибки, если что-то пошло не так

В случае, если текущая версия актуальна, процессы происходят, REP с течением времени увеличивается, - ваш модуль работает исправно.


## ⭕ В СЛУЧАЕ ВЫХОДА НОВОЙ ВЕРСИИ МОДУЛЯ (О КОТОРОЙ ВЫ МОЖЕТЕ УЗНАТЬ ИЗ КАНАЛА #testnet В НАШЕМ ДИСКОРД) НАИБОЛЕЕ НАДЕЖНЫМ СПОСОБОМ ОБНОВЛЕНИЯ ЯВЛЯЕТСЯ СПОСОБ ПОЛНОГО УДАЛЕНИЯ И ПОВТОРНОЙ УСТАНОВКИ. Для этого выполните поочередно следующие команды:

```
docker rm $(docker ps -a -q) --force
```

```
docker rmi $(docker images -a -q) --force
```

```
docker run -d --restart unless-stopped --pull always --name <ИМЯ_КОНТЕЙНЕРА> rg.fr-par.scw.cloud/exorde-labs/exorde-cli -m <ВАШ_ОСНОВНОЙ_ETH_КОШЕЛЕК> -l 2
```

Таким образом вы заново запустите один модуль. Если вы снова пожелаете запустить несколько модулей, то просто повторите последнюю команду, изменив <ИМЯ_КОНТЕЙНЕРА>.



⭕ [ОФИЦИАЛЬНАЯ ДОКУМЕНТАЦИЯ ОТ КОМАНДЫ EXORDE LABS](https://docs.exorde.network/)
⭕ [ОФИЦИАЛЬНОЕ РУКОВОДСТВО В GITHUB](https://github.com/exorde-labs/ExordeModuleCLI)
