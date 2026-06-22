# 🌐 Лабораторная работа №13 — IPSec over DMVPN

## 🎯 Цель работы
Настроить **GRE поверх IPSec** между офисами Москва и С.-Петербург.  
Настроить **DMVPN поверх IPSec** между Москва и Чокурдах, Лабытнанги.

---

## 📋 Текст задания

В этой самостоятельной работе мы ожидаем, что вы самостоятельно:

1. Настроите **GRE поверх IPSec** между офисами Москва и С.-Петербург.
2. Настроите **DMVPN поверх IPSec** между Москва и Чокурдах, Лабытнанги.
3. Обеспечите **IP связность** между всеми офисами.
4. Для IPSec используете **CA и сертификаты**.
5. Зафиксируете **план работы и изменения** в документации.

---

## 📚 Теоретическая справка

### Что такое IPSec?

**IPSec (IP Security)** — набор протоколов для защиты IP-трафика:

- **AH (Authentication Header)** — аутентификация
- **ESP (Encapsulating Security Payload)** — шифрование + аутентификация
- **IKE (Internet Key Exchange)** — обмен ключами и установка SA

### Что такое CA и сертификаты?

**CA (Certificate Authority)** — доверенный центр сертификации:
- Выпускает цифровые сертификаты для устройств
- Используется вместо предварительных ключей (pre-shared keys)
- Обеспечивает более высокий уровень безопасности

---

## 📊 Топология сети

[Текстовая схема IPSec over DMVPN](images/ipsec_topologi.txt)

### Роли устройств

| Устройство | Роль | IPSec | Сертификат |
|------------|------|-------|------------|
| **R15** (Москва) | HUB (DMVPN) + GRE | IPSec Profile + CA Server | CA + сертификат |
| **R18** (СПб) | GRE Spoke | IPSec Profile | Клиентский сертификат |
| **R28** (Чокурдах) | DMVPN Spoke | IPSec Profile | Клиентский сертификат |
| **R27** (Лабытнанги) | DMVPN Spoke | IPSec Profile | Клиентский сертификат |

---

## 🔧 Пошаговый план выполнения

### Шаг 1. Настройка PKI (CA) на R15

```bash
crypto pki server CA
 database url flash:
 grant auto
 no shutdown

Шаг 2. Настройка Trustpoints на всех устройствах

crypto pki trustpoint CA
 enrollment url http://10.77.0.253:80
 serial-number none
 subject-name CN=R15, OU=OTUS, O=VPN, C=RU
 revocation-check none
 rsakeypair CA

Шаг 3. Настройка ISAKMP политики

crypto isakmp policy 10
 encr aes 256
 hash sha
 authentication pre-share
 group 14
 lifetime 86400

Шаг 4. Настройка IPSec Transform-set и Profile

crypto ipsec transform-set TRANSFORM-SET esp-aes 256 esp-sha-hmac
 mode tunnel
!
crypto ipsec profile IPSEC-PROF
 set transform-set TRANSFORM-SET
 set pfs group14
 grant auto
 no shutdown

Шаг 5. Применение IPSec к туннелям

interface Tunnel100
 tunnel protection ipsec profile IPSEC-PROF

---

## 📁 Конфигурации

| **Устройство** | **Файл** | **Роль** |
|----------------|----------|----------|
| **R15** | [**config/routers/R15.cfg**](config/routers/R15.cfg) | **HUB** + CA + DMVPN + GRE + IPSec |
| **R18** | [**config/routers/R18.cfg**](config/routers/R18.cfg) | **GRE** + IPSec |
| **R28** | [**config/routers/R28.cfg**](config/routers/R28.cfg) | **DMVPN** + IPSec |
| **R27** | [**config/routers/R27.cfg**](config/routers/R27.cfg) | **DMVPN** + IPSec |

---
🔧 Проверка работоспособности

# Проверка PKI и сертификатов
R15# show crypto pki certificates
R15# show crypto pki server CA status

# Проверка IPSec SA
R15# show crypto ipsec sa
R15# show crypto isakmp sa

# Проверка туннелей
R15# show ip interface brief | include Tunnel
R15# show dmvpn

# Проверка связности
R15# ping 172.16.0.2
R15# ping 172.16.1.1
R15# ping 172.16.1.2
R28# ping 172.16.1.2   # Spoke-to-Spoke

📌 Автор: Баштырев В.
📅 Дата: 2026
📚 Лабораторная работа №13 — IPSec over DMVPN

