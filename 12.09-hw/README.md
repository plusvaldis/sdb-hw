# Домашнее задание к занятию «Базы данных в облаке» - Черепанов Владислав  

### Задание 1  


#### Создание кластера
1. Перейдите на главную страницу сервиса Managed Service for PostgreSQL.
1. Создайте кластер PostgreSQL со следующими параметрами:
- класс хоста: s2.micro, диск network-ssd любого размера;
- хосты: нужно создать два хоста в двух разных зонах доступности и указать необходимость публичного доступа, то есть публичного IP адреса, для них;
- установите учётную запись для пользователя и базы.

Остальные параметры оставьте по умолчанию либо измените по своему усмотрению.

* Нажмите кнопку «Создать кластер» и дождитесь окончания процесса создания, статус кластера = RUNNING. Кластер создаётся от 5 до 10 минут.

#### Подключение к мастеру и реплике 

* Используйте инструкцию по подключению к кластеру, доступную на вкладке «Обзор»: cкачайте SSL-сертификат и подключитесь к кластеру с помощью утилиты psql, указав hostname всех узлов и атрибут ```target_session_attrs=read-write```.

* Проверьте, что подключение прошло к master-узлу.
```
select case when pg_is_in_recovery() then 'REPLICA' else 'MASTER' end;
```
* Посмотрите количество подключенных реплик:
```
select count(*) from pg_stat_replication;
```

### Проверьте работоспособность репликации в кластере

* Создайте таблицу и вставьте одну-две строки.
```
CREATE TABLE test_table(text varchar);
```
```
insert into test_table values('Строка 1');
```

* Выйдите из psql командой ```\q```.

* Теперь подключитесь к узлу-реплике. Для этого из команды подключения удалите атрибут ```target_session_attrs```  и в параметре атрибут ```host``` передайте только имя хоста-реплики. Роли хостов можно посмотреть на соответствующей вкладке UI консоли.

* Проверьте, что подключение прошло к узлу-реплике.
```
select case when pg_is_in_recovery() then 'REPLICA' else 'MASTER' end;
```
* Проверьте состояние репликации
```
select status from pg_stat_wal_receiver;
```

* Для проверки, что механизм репликации данных работает между зонами доступности облака, выполните запрос к таблице, созданной на предыдущем шаге:
```
select * from test_table;
```

*В качестве результата вашей работы пришлите скриншоты:*

*1) Созданной базы данных;*
*2) Результата вывода команды на реплике ```select * from test_table;```.*  

![Скриншот-1](https://github.com/plusvaldis/sdb-hw/blob/main/12.09-hw/img/Screenshot_3.png)  
![Скриншот-2](https://github.com/plusvaldis/sdb-hw/blob/main/12.09-hw/img/Screenshot_2.png)  
![Скриншот-3](https://github.com/plusvaldis/sdb-hw/blob/main/12.09-hw/img/Screenshot_1.png)  


### Задание 2*

Создайте кластер, как в задании 1 с помощью Terraform.


*В качестве результата вашей работы пришлите скришоты:*

*1) Скриншот созданной базы данных.*
*2) Код Terraform, создающий базу данных.*  

![Скриншот-5](https://github.com/plusvaldis/sdb-hw/blob/main/12.09-hw/img/Screenshot_5.png)  
![Скриншот-6](https://github.com/plusvaldis/sdb-hw/blob/main/12.09-hw/img/Screenshot_6.png)  
![Скриншот-7](https://github.com/plusvaldis/sdb-hw/blob/main/12.09-hw/img/Screenshot_7.png)  
![Скриншот-8](https://github.com/plusvaldis/sdb-hw/blob/main/12.09-hw/img/Screenshot_8.png)  
![Скриншот-9](https://github.com/plusvaldis/sdb-hw/blob/main/12.09-hw/img/Screenshot_4.png)  


```terraform
terraform {
  required_providers {
    yandex = {
      source = "yandex-cloud/yandex"
    }
  }
}

resource "yandex_mdb_postgresql_cluster" "test" {
  name        = "test"
  environment = "PRODUCTION"
  network_id  = "enp5u6r5ok457noi05l1"
  folder_id   = "enp5u6r5ok457noi05l1"

  config {
    version = 15
    resources {
      resource_preset_id = "m2.micro"
      disk_type_id       = "network-ssd"
      disk_size          = 16
    }

  }



  host {
    zone      = "ru-central1-a"
    subnet_id = "e9baq8ug66jvt0eeln90"
    assign_public_ip = "true"
  }
  host {
    zone      = "ru-central1-b"
    subnet_id = "e2lon7oad3njmhlmqann"
    assign_public_ip = "true"
  }
}

resource "yandex_vpc_network" "default" {
  name = "default"
}

resource "yandex_vpc_subnet" "ru-central1-a" {
  zone           = "ru-central1-a"
  network_id     = "enp5u6r5ok457noi05l1"
  v4_cidr_blocks = ["10.128.0.0/24"]
}
resource "yandex_vpc_subnet" "ru-central1-b" {
  zone           = "ru-central1-b"
  network_id     = "enp5u6r5ok457noi05l1"
  v4_cidr_blocks = ["10.129.0.0/24"]
}
```

---

Задания, помеченные звёздочкой, — дополнительные, то есть не обязательные к выполнению, и никак не повлияют на получение вами зачёта по этому домашнему заданию. Вы можете их выполнить, если хотите глубже шире разобраться в материале.