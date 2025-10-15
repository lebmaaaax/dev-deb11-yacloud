# Dev Debian 11 Image for Yandex Cloud

Автоматизированное создание кастомного образа **Debian 11** для **Yandex Cloud** с помощью **Packer** и **Ansible**.
Образ включает готовую среду для разработки с установленными инструментами: `git`, `docker`, `python3`, `vim`, `htop` и другими.
Чтобы добавить дополнить образ, корректируйте роль devtools или добавьте новую.

Что представлено в репозитории:

- Автоматическая сборка кастомного образа Debian 11  
- Установка и настройка базовых инструментов  
- Создание пользователя `dev` с доступом по SSH  
- Предустановленный `Docker` и `Python` окружение  
- Конфигурация через `Ansible` роли

 **Требования**
 
- `Packer` ≥ 1.9  
- `Ansible`≥ 2.15  
- Аккаунт в `Yandex Cloud`
- Созданный сервисный аккаунт с ролью `editor` или `compute.admin`  
- Ключ `key.json` для аутентификации
- В примере используется алгоритм шифрования `id_ed25519`

**Настройка**

1. Скопируйте примеры конфигураций:
   ```bash
   cp packer/variables.auto.pkrvars.json.example packer/variables.auto.pkrvars.json
   cp packer/key.json.example packer/key.json
   Необходимо добавить публичный ключ, с которого будет идти подключение к вм в роль по пути packer/ansible/roles/devtools/files/id_ed25519.pub.
2. Заполните реальные значения в variables.auto.pkvars.json и в key.json укажите реальные данные сервисного аккаунта.

**Сборка образа**

Из директории packer/ выполните:

packer init .

packer validate image.json

packer build -var-file=variables.auto.pkrvars.json image.json


После успешного выполнения в Yandex Cloud → Compute Cloud → Images появится новый образ:

debian-11-(timestamp)

После развертывания из образа можно подключиться по SSH:

ssh -i ~/.ssh/id_ed25519 dev@<VM_IP>

Пользователь dev создается автоматически, необходимо добавить публичный ключ в роль devtools/files/id_ed25519.pub.

**Установленные пакеты**
Категория	Пакеты
Базовые	curl, vim, unzip, git, htop, net-tools, jq
Python	python3, python3-pip, virtualenv, ansible, requests
Docker	docker.io
Система	sudo с NOPASSWD для пользователя dev
