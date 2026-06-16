# Лабораторная работа №7 — iBGP

## Цель
Настроить iBGP в офисе Москва. Настроить iBGP в сети провайдера Триада.
Организовать полную IP связанность всех сетей.

## Задание
1. iBGP в Москве между R14 и R15 (AS 1001)
2. iBGP в Триаде с Route Reflector (AS 520)
3. Москва: приоритетный провайдер — Ламас
4. С.-Петербург: балансировка по 2 линкам
5. Полная IP связность всех сетей

## Топология
[Текстовая схема iBGP](images/ibgp.txt)

## Конфигурации
- [R14](config/routers/R14.cfg) — iBGP Москва
- [R15](config/routers/R15.cfg) — iBGP Москва + Local Pref
- [R23](config/routers/R23.cfg) — RR Триада
- [R24](config/routers/R24.cfg) — RR Триада
- [R25](config/routers/R25.cfg) — RR клиент
- [R26](config/routers/R26.cfg) — RR клиент
- [R18](config/routers/R18.cfg) — балансировка СПб

## Проверка
```bash
R14# show ip bgp summary
R14# show ip bgp
R18# show ip bgp
R23# show ip bgp summary
