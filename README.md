1)Заменить /etc/apt/sources.list

2)Установить и настроить sudo
	apt-get install sudo
	nano /etc/sudoers
	добавить строчку имя_пользователя ALL=(ALL) ALL
	
3)Установка Google Chrome
	Для установки Google Chrome вам нужно будет загрузить ключ репозитория, добавить сам репозиторий и обновить список пакетов:
	wget -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | sudo apt-key add -
	sudo sh -c 'echo "deb [arch=amd64] http://dl.google.com/linux/chrome/deb/ stable main" >> /etc/apt/sources.list.d/google-chrome.list'
	sudo apt-get update
	
	Существует три версии Chrome:
	google-chrome-stable - стабильная версия
	google-chrome-beta - бета-версия
	google-chrome-unstable - версия для разработчиков
	
	Для установки рекомендуется стабильная версия. Для того, чтобы установить ее выполните:
	sudo apt-get install google-chrome-stable	

4)Настройка Gnome
	Ставим Gnome Tweak Tool
	sudo apt-get install gnome-tweak-tool
	
5)Установка Git
	sudo apt-get install git
	
	Скачать репозиторий
	git clone --depth=1 <ссылка на репозиторий>
	
6)Установка Docker
	sudo apt-get update
	
	sudo apt-get install \
  apt-transport-https \
  ca-certificates \
  curl \
  gnupg2 \
  software-properties-common
	 
	curl -fsSL https://download.docker.com/linux/$(. /etc/os-release; echo "$ID")/gpg | sudo apt-key add -
	
	sudo add-apt-repository \
	"deb [arch=amd64] https://download.docker.com/linux/$(. /etc/os-release; echo "$ID") \
	$(lsb_release -cs) \
	stable"
	
	sudo apt-get update

	sudo apt-get install docker-ce
	
7)Получение Serial Raspberry Pi 3
    В консоли набрать cat /proc/cpuinfo
    Строчка "Serial" нули в начале не писать 
    (или просто использовать последние 8 цифр MAC Raspberry Pi 3)
    P.s. логин от Raspberrian: pi пароль: raspberry

8)Установка Putty
	sudo apt-get install putty
9)Работа с Raspberry Pi 3 через com порт
	Подключить uart
	Ввести в консоль sudo dmesg | grep ttyUSB
	Запустить Putty с правами 
	Потавить следущие настройки в putty:
		Connection type: Serial
		Serial line /dev/ttyUSB0
		Speed 115200
	Нажать кнопку "Open"

9)Сборка и настройка Docker
	Cкачиваем https://github.com/kswt/docker-openbsd_arm64-tftpboot
	
	Меняем имя у папки srv/tftp/a7c3571d на свой Serial Raspberry Pi 3
	Поменять mac на свой в файле etc/dhcp/dhcpd.conf и в файле etc/dnsmasq.conf на свой
	
	Сборка Docker
	Открываем папку в консоли и вводим sudo docker build --network=host .
	
	Для запуска вводим sudo docker run -it --net=host $( sudo docker images| awk 'NR==2 {print $(3)}')
	
	Подключаем Raspberry Pi 3 к той же сети 
	Вводим в Docker service tftpd-hpa start && service isc-dhcp-server start; tail -f /var/log/daemon.log 
	P.s. Команды есть в истории, до них можно добраться клавишами вверх и вниз.
	Подаем питание на Raspberry Pi 3

