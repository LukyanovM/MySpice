## Установка
#### Ubuntu и другие debian-like системы
Установка проверялась на Ubuntu 19.04. PySpice по умолчанию требует ngspice как разделяемую библиотеку. Ngspice в свою очередь в репозитории такую библиотеку не поставляет, нужно собирать самому. Ставим тулчейн и необходимые dev пакеты:

`sudo apt install -y libreadline-dev make build-essential wget python3-pip`

Собираем ngspice:

```
wget http://sourceforge.net/projects/ngspice/files/ng-spice-rework/30/ngspice-30.tar.gz
tar -xvzf ./ngspice-30.tar.gz
cd ./ngspice-30/
./configure --prefix=/usr/local --enable-xspice --disable-debug --enable-cider --with-readline=yes --enable-openmp --with-ngshared
make -j4
sudo make install
sudo ldconfig
``` 
Устанавливаем MySpice:

`pip3 install git+git://github.com/LukyanovM/MySpice.git`

#### Windows
Скачиваем ngspice для windows https://sourceforge.net/projects/ngspice/files/ng-spice-rework/30/ngspice-30_dll_64.zip/download. Распаковываем, папку Spice64_dll помещаем в C:\Program Files .

Устанавливаем miniconda https://repo.anaconda.com/miniconda/Miniconda3-latest-Windows-x86_64.exe . Запускаем **Anaconda Prompt (Miniconda3)** и ставим git:

`conda install git`

И далее ставим уже MySpice:

`pip install git+git://github.com/LukyanovM/MySpice.git`


## Интерфейс
1. Функция LoadFile() принимает путь к файлу в формате spice (.cir). Файлы для тестов генерировались в qucs-s версии 0.21. Реализован базовый функционал, секции .include, .subckt, .control игнорируются.
2. Функция SaveFile() сохраняет данные в формате csv с разделителем ";". Первая строка - напряжение, вторая - сила тока.
3. Функция CreateCVC() проводит анализ переходного процесса на первом цикле длительностью определяемой частотой в струкутре InitData. Возвращает экземпляр класса PySpice.CircuitSimulation, к которому можно обратиться напрямую для получения напряжения - **analysis.input_dummy**, для получения силы тока - **analysis.VCurrent**.

В папке examples лежат примеры.
