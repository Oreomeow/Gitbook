---
title: Alpine Selenium
author: Leafney
date: 2021-10-22
category: Python
layout: post
source: https://www.itfanr.cc/2016/10/19/configuration-the-selenium-running-environment-in-alpine/
---

## 配置环境

* Alpine Linux 3.4

### 一键搭建脚本

    apk add bash && bash -c $(curl -fsSL https://ghproxy.com/https://raw.githubusercontent.com/Oreomeow/checkinpanel/master/ins_selenium.sh)

## 安装依赖

### 设置软件安装源

    echo "http://dl-4.alpinelinux.org/alpine/v3.4/main" >> /etc/apk/repositories

### 安装依赖包

执行以下命令：

    apk update
    apk add python py-pip curl unzip
    pip install selenium

## 支持的浏览器及 WebDriver 驱动

这里我以常用的 Chrome 和 Firefox 为例，来配置运行环境。

## Selenium 调用 Chrome 浏览器

Alpine 系统下面使用 Chrome 推荐安装开源的 Chromium 浏览器

### 安装 Chromium

因为在软件库中存在 `Chromium` 的包，所以可以直接通过 `apk` 来安装：

    apk add chromium

然后还要安装 chromium 的依赖包：

    apk add libexif udev

如果没有安装 `libexif`  `udev` 这两个依赖包，会报如下错误，Chromium 浏览器会无法启动：

> selenium.common.exceptions. WebDriverException: Message: unknown error:
> Chrome failed to start: crashed

上面命令执行完成后，chromium 浏览器就安装好了。可以通过命令 `chromium-browser` 来测试：

    chromium-browser
    [54:54:1019/081743:ERROR:browser_main_loop.cc(261)] Gtk: cannot open display:

因为在 Server 系统下没有显示窗口，提示上面的信息说明 chromium 程序可以调用的到，只是无法显示。

### 安装 ChromeDriver

ChromeDriver 是一个实现了 WebDriver 与 Chromium 联接协议的独立服务。

#### 通过 apk 安装

我们可以直接通过如下命令来安装 `chromedriver` 程序：

    apk add chromium-chromedriver

#### 测试 ChromeDriver

执行 `chromedriver` 查看是否能正常运行。

    chromedriver
    Starting ChromeDriver 2.22 (5e2d5494d735a71aa5c2e7ef9bf5ce96945e92e9) on port 9515
    Only local connections are allowed.

当提示 `Starting ChromeDriver xxx on port 9515` 信息时，说明 `ChromeDriver` 设置成功。

---

## Selenium 调用 Firefox 浏览器

### 安装 Firefox 浏览器

可以通过 `apk` 直接安装 Firefox 浏览器：

    apk add firefox-esr

然后还要安装 firefox 的依赖包：

    apk add dbus-x11 ttf-freefont

其中 `dbus-x11` 中 `x11` 中是数字 1， `D-Bus` 是一个消息总线，用于在应用程序间发送消息。 如果不安装会报如下错误信息：

> selenium.common.exceptions. WebDriverException: Message: connection refused

在 geckodriver.log 文件中查看到如下信息:

> process 116: D-Bus library appears to be incorrectly set up; failed to read
> machine uuid: Failed to open “/etc/machine-id”: No such file or directory  
> See the manual page for dbus-uuidgen to correct this issue.  
> D-Bus not compiled with backtrace support so unable to print a backtrace  
> Redirecting call to abort() to mozalloc_abort

---

其中 `ttf-freefont` 是一个字体相关的依赖包，如果不安装会报如下错：

> selenium.common.exceptions. WebDriverException: Message: Failed to decode
> response from marionette

在 geckodriver.log 文件中查看到如下信息:

> Crash Annotation GraphicsCriticalError: |[0][gfx1]: no fonts - init: 1
> fonts: 0 loader: 0[GFX1]: no fonts - init: 1 fonts: 0 loader: 0  
> ^G[162] ###!!! ABORT: unable to find a usable font (serif): file
> /home/buildozer/aports/community/firefox-
> esr/src/firefox-45.4.0esr/gfx/thebes/gfxTextRun.cpp  
> [162] ###!!! ABORT: unable to find a usable font (serif): file
> /home/buildozer/aports/community/firefox-
> esr/src/firefox-45.4.0esr/gfx/thebes/gfxTextRun.cpp,

---

可通过命令 `firefox` 测试 firefox 浏览器是否安装成功：

    firefox
    Error: no display specified

同样的，由于没有显示窗口，也会提示 `no display` 的错误。

### 安装 geckodriver

#### 下载 geckodriver

访问站点 [geckodriver](https://github.com/mozilla/geckodriver/releases) 下载当前系统对应
geckodriver 程序。

执行如下命令：

    curl https://github.com/mozilla/geckodriver/releases/download/v0.11.1/geckodriver-v0.11.1-linux64.tar.gz -O

    tar -zxvf geckodriver-v0.11.1-linux64.tar.gz

    ls
    geckodriver

解压后我们得到了一个 `geckodriver` 执行程序。

该 `geckodriver` 压缩包可能由于网络原因下载失败，可通过迅雷等软件下载后拷贝到 Linux 系统中。

#### 将 geckodriver 放到系统 PATH 目录下

我们可以在程序中指定具体的 geckodriver 所在的目录，不指定的话会默认去系统 PATH 目录下找。为了编程方便，我们将其放到系统 PATH 目录下。

查看系统目录：

    echo $PATH

这里我将其放到 `/usr/local/bin/` 目录下，并添加可执行权限：

    mv ./geckodriver /usr/local/bin/

    chmod a+x /usr/local/bin/geckodriver

#### 测试 geckodriver

执行 `geckodriver` 查看是否能正常运行。

    geckodriver
    1476443497207 geckodriver INFO Listening on 127.0.0.1:4444

当提示 `Listening on 127.0.0.1:4444` 信息时，说明 `geckodriver` 设置成功。

如果提示如下错误信息，则是在系统 PATH 下找不到 `geckodriver` :

> selenium.common.exceptions. WebDriverException: Message: ‘geckodriver’
> executable needs to be in PATH.

---

## 安装虚拟显示器 xvfb

### 为什么要用 xvfb??

xvfb 这个工具相当于一个 wrapper，给应用程序提供虚拟的 X server

### 执行如下命令安装

    apk add xvfb
    pip install pyvirtualdisplay

---

## 测试 selenium 调用浏览器获取网页

### Chrome 版本

    #coding:utf-8

    import time
    from selenium import webdriver
    from pyvirtualdisplay import Display

    display=Display(visible=0,size=(800,800))
    display.start()

    driver=webdriver.Chrome()
    driver.get('http://www.cnblogs.com/')
    time.sleep(5)
    title=driver.title
    print(title.encode('utf-8'))
    driver.close()

    display.stop()

将以上代码保存为 `chrome.py` ，执行：

    python chrome.py

### Firefox 版本

    #coding:utf-8

    import time
    from selenium import webdriver
    from pyvirtualdisplay import Display

    display=Display(visible=0,size=(800,800))
    display.start()

    driver=webdriver.Firefox()
    driver.get('http://www.cnblogs.com/')
    time.sleep(5)
    title=driver.title
    print(title.encode('utf-8'))
    driver.close()

    display.stop()

将以上代码保存为 `firefox.py` ，执行：

    python firefox.py

---

## Docker 实现

详情请参考开源项目：

* [Leafney/alpine-selenium-chrome](https://github.com/Leafney/alpine-selenium-chrome)
* [leafney/alpine-selenium-chrome](https://hub.docker.com/r/leafney/alpine-selenium-chrome/)
* [Leafney/alpine-selenium-firefox](https://github.com/Leafney/alpine-selenium-firefox)
* [leafney/alpine-selenium-firefox](https://hub.docker.com/r/leafney/alpine-selenium-firefox/)

---
