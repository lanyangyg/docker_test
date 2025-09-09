FROM jenkins/jenkins:lts

USER root

# 安装 Python 3、pip 和 venv
RUN apt-get update && \
    apt-get install -y python3 python3-pip python3.11-venv && \
    apt-get clean

# 安装 Chrome 浏览器
RUN apt-get install -y wget gnupg && \
    wget -q -O - https://dl.google.com/linux/linux_signing_key.pub | apt-key add - && \
    echo "deb [arch=amd64] http://dl.google.com/linux/chrome/deb/ stable main" >> /etc/apt/sources.list.d/google-chrome.list && \
    apt-get update && \
    apt-get install -y google-chrome-stable

# 安装匹配的 ChromeDriver（使用新方法）
RUN CHROME_MAJOR_VERSION=$(google-chrome --version | awk '{print $3}' | cut -d '.' -f 1) && \
    echo "Chrome major version: $CHROME_MAJOR_VERSION" && \
    # 使用 ChromeDriver 的最新 API 获取匹配版本
    CHROME_DRIVER_VERSION=$(curl -s "https://googlechromelabs.github.io/chrome-for-testing/LATEST_RELEASE_$CHROME_MAJOR_VERSION") && \
    echo "ChromeDriver version: $CHROME_DRIVER_VERSION" && \
    # 下载 ChromeDriver
    wget -O chromedriver.zip "https://storage.googleapis.com/chrome-for-testing-public/$CHROME_DRIVER_VERSION/linux64/chromedriver-linux64.zip" && \
    unzip chromedriver.zip && \
    mv chromedriver-linux64/chromedriver /usr/local/bin/ && \
    rm -r chromedriver.zip chromedriver-linux64 && \
    chmod +x /usr/local/bin/chromedriver

USER jenkins