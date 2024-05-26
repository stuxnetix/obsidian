```
# Используем официальный Python образ
FROM python:3.8-slim-buster

# Устанавливаем рабочую директорию
WORKDIR /app

# Устанавливаем необходимые пакеты
RUN apt-get update && apt-get install -y \
    wget unzip \
    curl xvfb \
    gnupg --no-install-recommends && \
    rm -rf /var/lib/apt/lists/*

# Добавляем репозиторий Google Chrome и устанавливаем Chrome
RUN curl -sSL https://dl-ssl.google.com/linux/linux_signing_key.pub | apt-key add - && \
    echo "deb http://dl.google.com/linux/chrome/deb/ stable main" > /etc/apt/sources.list.d/google.list && \
    apt-get update && apt-get install -y google-chrome-stable --no-install-recommends && \
    rm -rf /var/lib/apt/lists/*

# Скачиваем и устанавливаем ChromeDriver
RUN DRIVER_VERSION=$(curl -sS https://chromedriver.storage.googleapis.com/LATEST_RELEASE) && \
    wget -q "https://chromedriver.storage.googleapis.com/${DRIVER_VERSION}/chromedriver_linux64.zip" && \
    unzip chromedriver_linux64.zip -d /usr/local/bin && \
    rm chromedriver_linux64.zip

# Устанавливаем необходимые Python пакеты
RUN pip install --no-cache-dir flask waitress undetected-chromedriver selenium

# Копируем скрипты и задаем права на выполнение
COPY start.py start.py
COPY start.sh start.sh
RUN chmod 555 start.py start.sh

# Задаем команду для запуска контейнера
CMD ["bash", "start.sh"]
```