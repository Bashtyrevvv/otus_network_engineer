cat > Lab12/README.md << 'EOF'
# 🌐 Лабораторная работа №12 — Базовый сервис MPLS

## 🎯 Цель работы
Настроить **BGP Free Core** в офисах Москвы и Санкт-Петербурга.

---

## 📋 Текст задания

В этой самостоятельной работе мы ожидаем, что вы:

1. Настроите **BGP Free Core** в офисе Москвы.
2. Настроите **BGP Free Core** в офисе Санкт-Петербурга.
3. Зафиксируете **план работы и изменения** в документации.
4. Оформите документацию на **GitHub**.

---

## 📚 Теоретическая справка

### Что такое BGP Free Core?

**BGP Free Core** — это архитектура MPLS/VPN, при которой:

- **P-роутеры (транзитные)** не знают BGP-маршрутов.
- **PE-роутеры (пограничные)** обмениваются VPNv4-маршрутами через MP-BGP.
- **MPLS LDP** раздаёт метки для форвардинга внутри MPLS-домена.
- Внутри core используется **OSPF/IS-IS** для достижения Loopback PE.

### Роли устройств

| Роль | Устройства | Задача |
|------|------------|--------|
| **PE** (Provider Edge) | R14, R15 (Москва), R18 (СПб) | BGP VPNv4, VRF |
| **P** (Provider) | R23, R24, R25, R26 (Триада) | MPLS LDP, OSPF |
| **P/PE** | R21 (Ламас), R22 (Киторн) | Транзит MPLS |

---

## 📊 Топология сети

[Текстовая схема MPLS](images/mpls_topologi.txt)

---

## 🔧 Пошаговый план выполнения

### Шаг 1. Настройка MPLS на P-роутерах Триады

| Устройство | Loopback | OSPF | LDP |
|------------|----------|------|-----|
| R23 | 100.0.0.251/32 | area 0 | ✅ |
| R24 | 100.0.0.252/32 | area 0 | ✅ |
| R25 | 100.0.0.253/32 | area 0 | ✅ |
| R26 | 100.0.0.254/32 | area 0 | ✅ |

### Шаг 2. Настройка OSPF для достижения PE Loopback

OSPF анонсирует Loopback PE через MPLS-сеть.

### Шаг 3. Настройка LDP на всех MPLS-интерфейсах

```bash
mpls ip
mpls ldp router-id Loopback0 force


Шаг 4. Настройка MP-BGP между PE
PE	AS	Соседи
R14 (Москва)	1001	R15 (iBGP), R18 (eBGP)
R15 (Москва)	1001	R14 (iBGP), R18 (eBGP)
R18 (СПб)	2042	R14, R15 (eBGP)


📁 Конфигурации
Устройство	Файл	Роль
R14	config/routers/R14.cfg	PE Москва
R15	config/routers/R15.cfg	PE Москва
R18	config/routers/R18.cfg	PE СПб
R23	config/routers/R23.cfg	P Триада
R24	config/routers/R24.cfg	P Триада
R25	config/routers/R25.cfg	P Триада
R26	config/routers/R26.cfg	P Триада
R21	config/routers/R21.cfg	P/PE Ламас
R22	config/routers/R22.cfg	P/PE Киторн


 🔧Проверка работоспособности

# Проверка LDP соседей
R14# show mpls ldp neighbor
R23# show mpls ldp neighbor

# Проверка MPLS forwarding
R14# show mpls forwarding-table

# Проверка BGP VPNv4
R14# show bgp vpnv4 unicast all summary
R14# show bgp vpnv4 unicast all

# Проверка связности между PE
R14# ping 10.78.0.254 source 10.77.0.254
R18# ping 10.77.0.254 source 10.78.0.254

# Проверка маршрутов на P-роутере (не должно быть BGP)
R23# show ip route
# Должны быть только OSPF и Connected



📌 Автор: Баштырев В.
📅 Дата: 2026
📚 Лабораторная работа №12 — Базовый сервис MPLS
EOF
