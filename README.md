# РУКОВОДСТВО ПО EXORDE TESTNET ДЛЯ CLI + DOCKER

⭕Помните, что вам необходимо постоянно следить за обновлениями, которые публикуются в [Discord Exorde](https://discord.gg/ExordeLabs) в канале #testnet.
Данный гайд будет обновляться в соответствии с новостями из Discord.

## ⭕ АКТУАЛЬНАЯ ВЕРСИЯ: 1.3.3b

## ⭕ ТРЕБОВАНИЯ

**CPU:** 2 cores

**RAM:** 4Gb

**SSD:** 2Gb

## ⭕ Краткое и быстрое руководство по установке. Последовательно выполняйте команды. Одну за другой:

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
docker run -d --restart unless-stopped --pull always --name ИМЯ_КОНТЕЙНЕРА rg.fr-par.scw.cloud/exorde-labs/exorde-cli -m ВАШ_ОСНОВНОЙ_ETH_КОШЕЛЕК -l 2
```
Например:
```
docker run -d --restart unless-stopped --pull always --name exorde-cli1 rg.fr-par.scw.cloud/exorde-labs/exorde-cli -m 0x16f177263988fF6fc8999013BD9bCB70F39b42d3 -l 2
```

## ⭕ ПРИМЕЧАНИЯ

Готово! Ваш модуль запущен в контейнере в фоновом режиме. Теперь вы можете закрыть терминал CLI, и модуль продолжит работать.

Чтобы запустить другую копию модуля, просто повторите ту же команду но с другим ИМЕНЕМ_КОНТЕЙНЕРА:
```
docker run -d --restart unless-stopped --pull always --name ИМЯ_КОНТЕЙНЕРА2 rg.fr-par.scw.cloud/exorde-labs/exorde-cli -m ВАШ_ОСНОВНОЙ_ETH_КОШЕЛЕК -l 2
```
Например:
```
docker run -d --restart unless-stopped --pull always --name exorde-cli2 rg.fr-par.scw.cloud/exorde-labs/exorde-cli -m 0x16f177263988fF6fc8999013BD9bCB70F39b42d3 -l 2
```
Сколько раз вы ввeдете эту команду с разными именами, столько модулей вы запустите.

Не забывайте, что контейнеры иногда могут останавливаться. И их необходимо перезапускать. Сначала нам нужно узнать <container_id>.
Для этого введите команду, которая отобразит все наши контейнеры:
```
docker ps -a
```

Скопируйте свой <container_id> и введите команду для перезапуска контейнера. Также с помощью этой же команды нужно перезапустить модуль при выпуске новой версии:
```
docker restart <container_id>
```
Например:
```
docker restart 1f77bd5b1111
```

Чтобы отобразить процессы, происходящие в контейнере, открытом в фоновом режиме, необходимо ввести команду:
```
docker logs --follow  <container_id>
```
Например:
```
docker logs --follow  1f77bd5b1111
```

Другие команды, которые могут быть вам полезны:

Остановить модуль:
```
docker stop <container_id>
```

Удалить модуль:
```
docker rm <container_id>
```

Для нормальной работы модулей необходимо, чтобы они не перегружали вашу систему. Поэтому открывайте такое количество модулей, которое будет работать оптимально для ваших технических характеристик.

Просмотр общей статистики сервера (например, для проверки загрузки системы):
```
top
```

⭕ [ОФИЦИАЛЬНОЕ РУКОВОДСТВО ОТ КОМАНДЫ EXORDE LABS](https://docs.exorde.network/)
