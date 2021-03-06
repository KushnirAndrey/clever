Использование rviz
===

![](assets/Снимок экрана 2017-11-28 в 23.50.36.png)

Инструмент [rviz](http://wiki.ros.org/rviz) позволяет в реальном времени визуализировать на 3D-сцене все компоненты роботехнической системы — системы координат, движущиеся части, показания датчиков, изображения с камер.

Для использования rviz необходим компьютер с ОС Ubuntu Linux (либо виртуальная машина, например [Parallels Desktop Lite](https://itunes.apple.com/ru/app/parallels-desktop-lite/id1085114709?mt=12) или [VirtualBox](https://www.virtualbox.org)).

На него необходимо установить пакет `ros-kinetic-desktop-full` или `ros-kinetic-desktop`, используя [документацию по установке](http://wiki.ros.org/kinetic/Installation/Ubuntu).

Запуск rviz
---

Для запуска визуализация состояния Клевера в реальном времени, необходимо подключиться к нему по Wi-Fi (`CLEVER-xxx`) и запустить rviz, указав соответствующий ROS_MASTER_URI:

```bash
ROS_MASTER_URI=http://192.168.11.1:11311 rviz
```

Если соединение не устанавливается, необходимо убедиться, что в `.bashrc` Клевера присутствует строка:

```bash
export ROS_IP=192.168.11.1
```

Использование
---

В качестве reference frame рекомендуется установить фрейм `local_origin`. Для визуализации коптера можно добавить визуализационные маркеры из топика `/vehicle_markers`. Можно просмотреть картинку с дополненной реальностью из топика основной камеры `/main_camera/image_raw`.

Axis или Grid настроенный на фрейм `aruco_map` будут визуализировать расположение [карты ArUco-меток](aruco.md).

Рекомендуется также установка набора дополнительных полезных плагинов [jsk_rviz_plugins](https://jsk-docs.readthedocs.io/en/latest/jsk_visualization/doc/jsk_rviz_plugins/index.html). Это набор позволяет визуализировать топики типа `TwistStamped` (скорость), `CameraInfo`, `PolygonArray` и многое другое. Для установки используйте команду:

```bash
sudo apt-get install ros-kinetic-jsk-visualization
```
