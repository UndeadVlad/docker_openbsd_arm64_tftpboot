
## 1. Установить и настроить sudo
  *  `apt-get install sudo`
  *  `nano /etc/sudoers`
  * Добавиить строчку имя_пользователя `ALL=(ALL) ALL`
	
## 2. Установка и использование Git
  * `sudo apt-get install git`
  * Скачать репозиторий `git clone --depth=1 <ссылка на репозиторий>`
	
## 3. Установка Docker
  * `sudo apt-get update`
  * `sudo apt-get install apt-transport-https ca-certificates curl gnupg2 software-properties-common`
  * `curl -fsSL https://download.docker.com/linux/$(. /etc/os-release; echo "$ID")/gpg | sudo apt-key add -`
  * `sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/$(. /etc/os-release; echo "$ID") $(lsb_release -cs) stable" `
  * `sudo apt-get update`
  * `sudo apt-get install docker-ce`
	
## 4. Получение Serial Raspberry Pi 3
  * В консоли набрать `cat /proc/cpuinfo`
  * Строчка "Serial" нули в начале не писать (или просто использовать последние 8 цифр MAC Raspberry Pi 3)
    
  P.s. логин от Raspberrian: pi пароль: raspberry

## 5. Установка Putty 
  `sudo apt-get install putty`
	
## 6. Работа с Raspberry Pi 3 через com порт с помощью putty
  * Подключить uart
  * Ввести в консоль `sudo dmesg | grep ttyUSB`
  * Запустить putty с правами 
  * Потавить следущие настройки в putty:
      ```
      Connection type: Serial
      Serial line /dev/ttyUSB0
      Speed 115200
      ```
  * Нажать кнопку "Open"

## 7. Сборка и настройка Docker
  * Cкачиваем https://github.com/UndeadVlad/docker_openbsd_arm64_tftpboot
  * Меняем имя у папки `srv/tftp/a7c3571d` на свой Serial Raspberry Pi 3
  * Поменять mac на свой в файле `etc/dhcp/dhcpd.conf` и в файле `etc/dnsmasq.conf` на свой
  
  * Для Сборки Docker открываем папку в консоли и вводим `sudo docker build --network=host .`
  * Для запуска вводим `sudo docker run -it --net=host $( sudo docker images| awk 'NR==2 {print $(3)}')`
	
## 8. Запуск Raspberry Pi 3 через сеть
  * Подключаем Raspberry Pi 3 к той же сети 
  * Назначаем статичный ip 192.168.1.110 для Raspberry Pi 3 (если малина привязанна к другому адресу - надо поменять настройки в файлах: `etc/dhcp/dhcpd.conf` и `etc/dnsmasq.conf`
  * Вводим в Docker `service tftpd-hpa start && service isc-dhcp-server start; tail -f /var/log/daemon.log`
    P.s. Команды есть в истории, до них можно добраться клавишами вверх и вниз.
  * Подаем питание на Raspberry Pi 3
	
## 9. Установка и настройка Wireshark
  * **Установка:** 
    `sudo apt-get install wireshark-gtk`

  * **Настройка:**
    * `sudo dpkg-reconfigure wireshark-common`
    * Согласиться с тем, что не только root должен иметь возможность снифить пакеты.
    * Добавить себя к группе wireshark: `sudo adduser $USER wireshark`
