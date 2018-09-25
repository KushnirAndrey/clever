## Подключение Pixhawk/Pixracer к Raspberry Pi  
В RPi необходимо вставить microSD с залитым [образом системы](https://github.com/CopterExpress/clever/releases)  
RPi подключается к монитору с помощью HDMI кабеля, питание подается либо от аккумулятора clever либо по microUSB  
Чтобы войти в терминал RPi будет запрошен логин и пароль  
**login: pi  
пароль: raspberry**  
Для программирования автономных полетов, работы с Pixhawk по Wi-Fi, использования веб-пульта и других функций необходимо подсоединить Raspberry Pi к Pixhawk.  
Убедиться в работоспособности подключения, выполнив на Raspberry Pi:  
#### rostopic echo /mavros/state  
Поле **connected** должно содержать значение **True**.  
#### Подключение по USB  
Соедините Pixhawk/Pixracer и Raspberry Pi micro-USB to USB кабелем.  
Необходимо убедиться, что в launch-файле Клевера **(~/catkin_ws/src/clever/clever/launch/clever.launch)** тип подключения установлен на USB:  
**_\<arg name="fcu_conn" default="usb"\>_**  
При изменении launch-файла необходимо перезапустить пакет clever:  
**sudo systemctl restart clever**  

### Подключение к Клеверу по Wi-Fi  
На образе для RPi преднастроена раздача Wi-Fi с SSID CLEVER-xxxx, где xxxx – 4 случайных цифры, назначаемых при первом включении Raspberry Pi.  
Пароль: **cleverwifi**.  
Для изменения настроек Wi-Fi или получения более детальной информации о устройстве сети на Raspberri Pi прочитайте [эту статью](https://clever.copterexpress.com/network.html).  
Затем из браузера по ip = **192.168.11.1** можно просматривать видео стрим с камеры  
## Настройка ArUco  
![ArUco Маркеры](https://clever.copterexpress.com/assets/markers.jpg)  
Для генерации маркеров можно использовать [это](http://chev.me/arucogen/) или [это](https://tn1ck.github.io/aruco-print/)  
### Включение  
Необходимо убедиться, что в launch-файле Клевера **(~/catkin_ws/src/clever/clever/launch/clever.launch)** включен запуск aruco_pose и камеры для компьютерного зрения:  
**\<arg name="main_camera" default="true"/>**  
**\<arg name="aruco" default="true"/>**  
При изменении launch-файла необходимо перезапустить пакет clever:  
**sudo systemctl restart clever**  
### Настройка карты ArUco-меток  
Настройка карты меток производится с помощью файла **~/catkin_ws/src/clever/clever/aruco.launch**. Для использования AruCo-board введите его параметры: 
```javascript
<node pkg="nodelet" type="nodelet" name="aruco_pose" args="load aruco_pose/aruco_pose nodelet_manager">
    <param name="frame_id" value="aruco_map_raw"/>
    <!-- тип маркерного поля -->
    <param name="type" value="gridboard"/>

    <!-- количество маркеров по x -->
    <param name="markers_x" value="1"/>

    <!-- количество маркеров по y -->
    <param name="markers_y" value="6"/>

    <!-- ID маркера первого маркера (левого верхнего) -->
    <param name="first_marker" value="240"/>

    <!-- длина стороны маркера в метрах -->
    <param name="markers_side" value="0.3362"/>

    <!-- растояние между маркерами -->
    <param name="markers_sep" value="0.46"/>
</node>
```  
Если используется карта с нестандартным порядком ID меток, то можно использовать параметр marker_ids:  
**<rosparam param="marker_ids">[5, 7, 9, 11, 13, 15]</rosparam>**  
Нумерация маркеров ведется с левого верхнего угла поля.  
Для контроля карты, по которой в данный момент коптер осуществляет навигацию, можно просмотреть содержимое топика aruco_pose/map_image. Через браузер его можно просмотреть при помощи web_video_server по [ссылке](http://192.168.11.1:8080/snapshot?topic=/aruco_pose/map_image)  
При полетах необходимо убедиться, что наклеенные на пол метки соответствуют карте.  
В топике [aruco_pose/debug](http://192.168.11.1:8080/snapshot?topic=/aruco_pose/debug) доступен текущий результат распознования меток  
### Настройка полетного контролера
Для простоты настройки можно воспользоваться готовым файлом настроек для [Clever 2](https://github.com/CopterExpress/clever/blob/master/docs/assets/Clever2LPE_160118.params) или для [Clever 3](https://github.com/CopterExpress/clever/blob/master/docs/assets/Clever3_LPE_020218.params) и вгрузить его в контроллер с помощью меню **Tools - Load from file из раздела Parameters в QGroundControl**.  
![tools](https://clever.copterexpress.com/assets/Screenshot%20from%202018-02-27%2022-30-50.png)  
## Видео стрим с помощью COEX HD Link  
Сначала включается clever потом включаем HD Link  
HD Link подключается HDMI кабелем к монитору, и на него транслируется видео с основной камеры  
Чтобы сохранить снятое видео (с момента включение коптера) нужно в HD Link встваить flash карту и начнется автоматическое сохранение видео на Flash в папку video, также произойдет сохранение телеметрии.  


