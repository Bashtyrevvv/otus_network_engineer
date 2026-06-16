# Настройка Spanning Tree (Rapid PVST+)

## 🎯 Цель
Предотвратить broadcast-штормы и оптимизировать использование линков в каждом офисе.

## 🛡 Используемые технологии

### VLAN (Virtual Local Area Network)
Разделение сети на отдельные VLAN позволяет изолировать broadcast-трафик и предотвратить его распространение на всю сеть.

### STP / RSTP (Spanning Tree Protocol)
Протокол STP предотвращает образование петель в сети, которые могут привести к broadcast штормам. Используется Rapid PVST+ для быстрого обнаружения и устранения петель.

### BPDU Guard
Защита от неправильных BPDU на access-портах.

### PortFast
Ускорение работы портов, подключенных к конечным устройствам.

---

## 🏢 Офис Москва

### SW4 (Root Bridge для VLAN 10, 20, 999)
spanning-tree mode rapid-pvst
spanning-tree vlan 10 root primary
spanning-tree vlan 20 root primary
spanning-tree vlan 999 root primary




### SW5 (Secondary Root)
spanning-tree mode rapid-pvst
spanning-tree vlan 10 root secondary
spanning-tree vlan 20 root secondary
spanning-tree vlan 999 root secondary

### Access Ports (SW2, SW3)
interface Ethernet0/2
switchport access vlan 20
switchport mode access
spanning-tree portfast
spanning-tree bpduguard enable

### Оптимизация линков
| Линк | Роль | Статус | Причина |
|------|------|--------|---------|
| SW4 e1/0 → R12 | Root Port | Forwarding | Основной путь |
| SW5 e1/1 → R12 | Root Port | Forwarding | Основной путь |
| SW4 e1/1 → R13 | Designated | Forwarding | Резерв |
| SW5 e1/0 → R13 | Designated | Forwarding | Резерв |

**Балансировка нагрузки:**
- VLAN 10 → через SW4 и SW5 (оба через R12)
- VLAN 20 → через SW4 и SW5 (оба через R13)

---

## 🏢 Офис Санкт-Петербург

### SW9 (Root Bridge для VLAN 30)
spanning-tree mode rapid-pvst
spanning-tree vlan 30 root primary
### SW10 (Secondary Root)
spanning-tree mode rapid-pvst
spanning-tree vlan 30 root secondary
### Access Ports
nterface Ethernet0/0
switchport access vlan 30
switchport mode access
spanning-tree portfast
spanning-tree bpduguard enable

### Оптимизация линков
| Линк | Роль | Статус |
|------|------|--------|
| SW9 e0/3 → R17 | Root Port | Forwarding |
| SW10 e0/3 → R16 | Root Port | Forwarding |
| SW9 e1/0 → R16 | Designated | Forwarding |
| SW10 e1/0 → R17 | Designated | Forwarding |

---

## 🏢 Офис Чокурдах

### R28 (Root Bridge для всех VLAN)
spanning-tree vlan 100 root primary
spanning-tree vlan 200 root primary
spanning-tree vlan 999 root primary

### Sub-интерфейсы с PortFast
spanning-tree vlan 100 root primary
spanning-tree vlan 200 root primary
spanning-tree vlan 999 root primary

text

### Sub-интерфейсы с PortFast
interface Ethernet0/2.100
encapsulation dot1Q 100
ip address 10.67.1.1 255.255.255.128
spanning-tree portfast
!
interface Ethernet0/2.200
encapsulation dot1Q 200
ip address 10.67.1.129 255.255.255.128
spanning-tree portfast
!
interface Ethernet0/2.999
encapsulation dot1Q 999
ip address 10.67.0.1 255.255.255.248
spanning-tree portfast

---

## 📊 Сводная таблица Root Bridges

| Офис | Root Bridge | Secondary | VLAN |
|------|-------------|-----------|------|
| Москва | SW4 | SW5 | 10, 20, 999 |
| Санкт-Петербург | SW9 | SW10 | 30 |
| Чокурдах | R28 | - | 100, 200, 999 |

---

## ✅ Итог

- ✅ Сеть защищена от broadcast-штормов
- ✅ Линки используются максимально эффективно
- ✅ Балансировка нагрузки настроена
- ✅ BPDU Guard включен на всех access-портах
- ✅ PortFast включен на всех access-портах
