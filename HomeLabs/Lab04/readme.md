# Лабораторная работа №4 — IS-IS

## Цель
Настроить IS-IS в ISP Триада.

## Задание
1. Настроить IS-IS в ISP Триада
2. R23 и R25 находятся в зоне 2222
3. R24 находится в зоне 24
4. R26 находится в зоне 26
5. Настройка одновременно для IPv4 и IPv6

## Топология
[Текстовая схема IS-IS](images/isis.txt)

## Зоны IS-IS

| Устройство | Зона | Уровень |
|------------|------|---------|
| R23 | 2222 | Level-2 |
| R25 | 2222 | Level-2 |
| R24 | 24 | Level-2 |
| R26 | 26 | Level-2 |

## Конфигурации
- [R23](config/routers/R23.cfg) — зона 2222
- [R25](config/routers/R25.cfg) — зона 2222
- [R24](config/routers/R24.cfg) — зона 24
- [R26](config/routers/R26.cfg) — зона 26

## Проверка
```bash
R23# show isis topology
R23# show isis neighbors
R23# show ip route
R23# show ipv6 route
