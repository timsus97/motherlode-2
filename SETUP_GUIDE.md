# Пошаговое руководство по настройке Greedisgood Bot

## 1. Подготовка окружения

### Установка Python
Убедитесь, что у вас установлен Python 3.8 или выше:
```bash
python3 --version
```

### Установка зависимостей
```bash
pip install -r requirements.txt
```

## 2. Создание Telegram бота

1. Откройте [@BotFather](https://t.me/BotFather) в Telegram
2. Отправьте команду `/newbot`
3. Введите имя бота (например: "Greedisgood Investment Bot")
4. Введите username бота (например: "greedisgood_bot")
5. Скопируйте полученный токен

### Настройка команд бота
Отправьте BotFather команду `/setcommands` и выберите вашего бота, затем отправьте:
```
start - 🚀 Запустить бота
```

## 3. Получение Telegram ID

1. Откройте [@userinfobot](https://t.me/userinfobot)
2. Отправьте любое сообщение
3. Скопируйте ваш ID (число)

## 4. Создание BSC кошелька

⚠️ **ВАЖНО**: Создайте НОВЫЙ кошелёк специально для бота!

### Способ 1: MetaMask
1. Установите MetaMask
2. Создайте новый аккаунт
3. Добавьте BSC сеть:
   - Network Name: BSC Mainnet
   - RPC URL: https://bsc-dataseed.binance.org/
   - Chain ID: 56
   - Symbol: BNB
   - Block Explorer: https://bscscan.com

### Способ 2: Программно
```bash
python utils.py generate-wallet
```

### Пополнение кошелька
1. Отправьте минимум **0.5 BNB** на кошелёк (для оплаты газа)
   - Бот автоматически отправляет 0.0001 BNB на каждый прокси-кошелёк
   - При недостатке BNB приём инвестиций автоматически блокируется
   - Администратор получает уведомления о критически низком балансе
2. При необходимости добавьте USDT для тестирования

## 5. Настройка конфигурации

1. Скопируйте файл конфигурации:
```bash
cp .env.example .env
```

2. Отредактируйте `.env` файл:
```bash
nano .env
```

3. Заполните обязательные поля:
```env
# Токен бота от BotFather
BOT_TOKEN=1234567890:ABCdefGHIjklMNOpqrsTUVwxyz

# Ваш Telegram ID
ADMIN_ID=123456789

# Пароль для админ-панели
ADMIN_PASSWORD=your_secure_password

# Приватный ключ кошелька (БЕЗ 0x)
MASTER_WALLET_PRIVATE_KEY=abcdef1234567890...

# Адрес кошелька
MASTER_WALLET_ADDRESS=0xYourWalletAddress...
```

## 6. Тестирование настройки

```bash
python utils.py diagnostics
```

Эта команда проверит:
- ✅ Конфигурацию
- ✅ Подключение к базе данных
- ✅ Подключение к BSC
- ✅ Токен бота
- ✅ Адрес кошелька

## 7. Запуск бота

### Простой запуск
```bash
python run.py
```

### Или напрямую
```bash
python main.py
```

## 8. Тестирование функций

### Тест пользователя
1. Найдите вашего бота в Telegram
2. Отправьте `/start`
3. Выберите язык
4. Попробуйте создать инвестицию

### Тест администратора
1. Отправьте `/whosyourdaddy your_password`
2. Проверьте админ-панель
3. Посмотрите отчёты

## 9. Производственное развёртывание

### Использование systemd (Linux)

1. Создайте service файл:
```bash
sudo nano /etc/systemd/system/greedisgood-bot.service
```

2. Добавьте содержимое:
```ini
[Unit]
Description=Greedisgood Telegram Bot
After=network.target

[Service]
Type=simple
User=your_user
WorkingDirectory=/path/to/motherlode
ExecStart=/usr/bin/python3 main.py
Restart=always
RestartSec=10

[Install]
WantedBy=multi-user.target
```

3. Запустите сервис:
```bash
sudo systemctl daemon-reload
sudo systemctl enable greedisgood-bot
sudo systemctl start greedisgood-bot
```

### Использование Docker

1. Создайте Dockerfile:
```dockerfile
FROM python:3.9-slim

WORKDIR /app
COPY requirements.txt .
RUN pip install -r requirements.txt

COPY . .

CMD ["python", "main.py"]
```

2. Соберите и запустите:
```bash
docker build -t greedisgood-bot .
docker run -d --name greedisgood-bot --restart unless-stopped greedisgood-bot
```

## 10. Мониторинг и обслуживание

### Просмотр логов
```bash
# Systemd
sudo journalctl -u greedisgood-bot -f

# Docker
docker logs -f greedisgood-bot
```

### Резервное копирование
Регулярно создавайте резервные копии:
- База данных: `greedisgood.db`
- Конфигурация: `.env`

### Обновление
1. Остановите бота
2. Обновите код
3. Запустите бота

## Устранение неполадок

### Бот не отвечает
- Проверьте токен бота
- Убедитесь, что бот запущен
- Проверьте интернет-соединение

### Ошибки блокчейна
- Проверьте баланс BNB на кошельке
- Убедитесь в правильности RPC URL
- Проверьте приватный ключ

### Ошибки базы данных
- Проверьте права доступа к файлу БД
- Убедитесь в наличии свободного места

### Проблемы с выплатами
- Проверьте баланс USDT на основном кошельке
- Убедитесь, что выплаты включены в админ-панели
- Проверьте логи на ошибки

## Безопасность

⚠️ **КРИТИЧЕСКИ ВАЖНО**:

1. **Никогда не публикуйте .env файл**
2. **Используйте отдельный кошелёк только для бота**
3. **Регулярно меняйте пароль админ-панели**
4. **Мониторьте баланс кошелька**
5. **Делайте резервные копии**

## Поддержка

При возникновении проблем:
1. Проверьте логи
2. Запустите диагностику: `python utils.py diagnostics`
3. Убедитесь в правильности конфигурации
4. Проверьте баланс кошелька