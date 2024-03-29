<a name="readme-top"></a>
# MySQL-IoT-
base de datos de un proyecto IoT para invernaderos
<br>
<div align="center" >
 <img src="BD tablas/Logo ITQ.png" alt="Logo" width="612" height="200">
</div>
 <h3 align="center">Proyecto IoT</h3>
 <h2>Integrantes
 </h2>
 <li>Joel Caza</li>
  <li>Mathias Cruz</li>
   <li>Kevin Chiguano</li>
<!-- PROJECT LOGO -->
<div align="center" >
   <img src="https://cdn-icons-png.flaticon.com/512/6228/6228463.png" alt="Logo" width="280" height="280">
</div>
<!-- TABLE OF CONTENTS -->
<details>
  <summary>Tabla De Contenidos</summary>
  <ol>
    <li>
      <a href="#introdducion">Introduccion</a>
    </li>
    <li>
      <a href="#escenario">Escenario</a>
    </li>
    <li><a href="#herramientas">Herramientas</a></li>
    <li><a href="#consultas">Consultas</a></li>
  </ol>
</details>
</div>


<!-- ABOUT THE PROJECT -->
## Introduccion <a name="introdducion"></a>

En un mundo donde la seguridad alimentaria y la eficiencia agrícola son prioridades globales, la tecnología de Internet de las Cosas (IoT) ha emergido como un catalizador fundamental para la transformación del sector agrícola. Los invernaderos, en particular, representan un espacio crucial donde la integración de dispositivos IoT puede revolucionar la forma en que se cultiva y se gestiona la producción de alimentos.

Este proyecto tiene como objetivo explorar y desarrollar un sistema de IoT para invernaderos, diseñado para optimizar el control ambiental, monitorear el crecimiento de las plantas y mejorar la eficiencia en el uso de recursos como agua y energía. Al emplear sensores inteligentes, actuadores y una plataforma de gestión de datos en la nube, se busca crear un entorno de cultivo inteligente y autónomo que permita a los agricultores supervisar y gestionar sus cultivos de manera remota y en tiempo real.


<p align="right">(<a href="#readme-top">Regresar al Inicio</a>)</p>

<!-- GETTING STARTED -->
## Escenario <a name="escenario"></a>
Somos una empresa de sensores para invernaderos, los sensores se 
encargarán de recoger la información relacionada con: temperatura, humedad, 
presión atmosférica, luminosidad.
<p align="right">(<a href="#readme-top">Regresar al Inicio</a>)</p>

## Herramientas <a name="herramientas"></a>
Para la realización de un proyecto IoT para un invernadero, es necesario contar con una variedad de herramientas y tecnologías que permitan la adquisición, procesamiento y gestión de datos, así como el control de dispositivos. A continuación, se detallan algunas de las herramientas esenciales:


  
  <li>Sensores y Actuadores:
            <ul>
                <li>Sensores de temperatura </li>
                <li>Sensores de luz</li>
                <li>Sensores de humedad </li>
            </ul>
        </li>
        <li>Plataforma de Gestión de Datos:
            <ul>
                <li>Base de Datos (MySQL, PostgreSQL, MongoDB)</li>
                <li>Servicios en la Nube (AWS IoT Core, Google Cloud IoT Core, Azure IoT Hub)</li>
            </ul>
        </li>
        <li>Software de Desarrollo:
            <ul>
                <li>Lenguajes de Programación (Python, JavaScript, C++)</li>
                <li>Frameworks de Desarrollo IoT (MQTT, CoAP, HTTP)</li>
            </ul>
        </li>
        <li>Interfaz de Usuario:
            <ul>
                <li>Aplicación Web o Móvil</li>
                <li>Frameworks de Desarrollo Web o Móvil (Flask, Django, React, Angular, Flutter)</li>
            </ul>
        </li>
        <li>Seguridad:
            <ul>
                <li>Protocolos de Seguridad IoT (TLS/SSL)</li>
                <li>Autenticación y Autorización</li>
            </ul>
        </li>
<p align="right">(<a href="#readme-top">Regresar al Inicio</a>)</p>

<br>
 <h1> Modelo de la Base de Datos: </h1>
 <div align="center" >
 <img src="BD tablas/Modelo Bd.jpg">
 </div>


<!-- USAGE EXAMPLES -->
## Consultas <a name="consultas"></a>
 1. Obtener todos los dispositivos junto con la cantidad de datos de sensores registrados
para cada uno.
 ```sh
   SELECT dev_id, dev_name, COUNT(sre_id) AS Cantidad_sensores_registrados
   FROM Devices 
   LEFT JOIN Sensors  ON dev_id = sen_dev_id
   LEFT JOIN SensorsReading  ON sen_id = sre_sen_id
   GROUP BY dev_id, dev_name;
   ```
<div align="center" >
 <img src="BD tablas/Constulta1.jpg">
 </div>

2. Encontrar la temperatura máxima registrada por cada dispositivo.
```sh
   SELECT dev_id, dev_name, MAX(sre_value) AS max_temperature
FROM devices 
LEFT JOIN sensors ON dev_id = sen_dev_id
LEFT JOIN sensorsreading ON sen_id = sre_sen_id
WHERE sen_name = 'termometro'
GROUP BY dev_id, dev_name;
   ```

<div align="center" >
 <img src="BD tablas/Constula2.jpg">
 </div>
 
3. Mostrar la cantidad total de datos de sensores registrados para cada tipo de sensor.
```sh
  SELECT sen_type, COUNT(sre_id) AS total_sensor_data
FROM sensors 
LEFT JOIN sensorsreading  ON sen_id = sre_sen_id
GROUP BY sen_type;
   ```

<div align="center" >
 <img src="BD tablas/Constula3.jpg">
 </div>
 
4. Obtener la lista de dispositivos que no han enviado datos en las últimas 24 horas.
```sh
   SELECT dev_id, dev_name
FROM devices 
LEFT JOIN sensors  ON dev_id = sen_dev_id
LEFT JOIN sensorsreading ON sen_id = sre_sen_id
GROUP BY dev_id, dev_name
HAVING MAX(sre_date) < NOW() - INTERVAL 24 HOUR OR MAX(sre_date) IS NULL;
   ```

<div align="center" >
 <img src="BD tablas/Constula4.jpg">
 </div>
 
5. Calcular el promedio de temperatura por hora para todos los dispositivos.
```sh
   SELECT dev_id, dev_name, HOUR(sre_date) AS HORA, AVG(sre_value) AS PromedioTemperatura
FROM devices 
LEFT JOIN sensors  ON dev_id = sen_dev_id
LEFT JOIN sensorsreading ON sen_id = sre_sen_id
WHERE sen_name = 'termometro'
GROUP BY dev_id, dev_name, HOUR(sre_date);
   ```
<div align="center" >
 <img src="BD tablas/Constula5.jpg">
 </div>

6. Encontrar el dispositivo con la mayor cantidad de datos de sensores registrados.
```sh

   SELECT dev_id, dev_name, COUNT(sre_id) AS sensor_data_count
FROM devices 
LEFT JOIN sensors ON dev_id = sen_dev_id
LEFT JOIN sensorsreading ON sen_id = sre_sen_id
GROUP BY dev_id, dev_name
ORDER BY sensor_data_count DESC
LIMIT 1;

   ```

<div align="center" >
 <img src="BD tablas/Constula6.jpg">
 </div>
 
7. Mostrar los dispositivos cuya temperatura promedio sea superior a cierto umbral.
```sh
   SELECT dev_id,dev_name, AVG(sre_value) AS average_temperature
FROM devices
LEFT JOIN sensors  ON dev_id = sen_dev_id
LEFT JOIN sensorsreading ON sen_id = sre_sen_id
WHERE sen_name = 'termometro'
GROUP BY dev_id, dev_name
HAVING AVG(sre_value) > 40.00;
   ```

<div align="center" >
 <img src="BD tablas/Constula7.jpg">
 </div>
 
8. Obtener la cantidad total de datos de sensores registrados para cada dispositivo en el
último mes.
```sh
  SELECT dev_id,dev_name, COUNT(sre_id) AS sensor_data_count
FROM devices
LEFT JOIN sensors ON dev_id = sen_dev_id
LEFT JOIN sensorsreading ON sen_id = sre_sen_id
WHERE sre_date >= DATE_SUB(NOW(), INTERVAL 1 MONTH)
GROUP BY dev_id,dev_name;
   ```

<div align="center" >
 <img src="BD tablas/Constula8.jpg">
 </div>

9. Encontrar los dispositivos que tengan al menos dos tipos diferentes de sensores.
```sh
SELECT dev_id, dev_name
FROM devices 
INNER JOIN (
    SELECT sen_dev_id, COUNT(DISTINCT sen_type) AS num_sensor_types
    FROM sensors
    GROUP BY sen_dev_id
) AS sensor_counts ON dev_id = sensor_counts.sen_dev_id
WHERE sensor_counts.num_sensor_types >= 2;
   ```

<div align="center" >
 <img src="BD tablas/Constula9.jpg">
 </div>
 
10. Mostrar los sensores cuyo tipo  actual sea "aluminio" y que hayan enviado datos en
las últimas 12 horas.
```sh
   SELECT sen_id, sen_name, sen_type
FROM Sensors
WHERE sen_type = 'aluminio'
AND sen_id IN (
    SELECT DISTINCT sen_id
    FROM SensorsReading
    WHERE sre_date >= NOW() - INTERVAL 12 HOUR
);
   ```

<div align="center" >
 <img src="BD tablas/10.jpg">
 </div>
 
11. Calcular la suma total de datos de sensores para cada tipo de sensor en los últimos siete
días.
```sh
   SELECT sre_type, SUM(sre_value) AS total_data
FROM SensorsReading
WHERE sre_date >= DATE_SUB(NOW(), INTERVAL 7 DAY)
GROUP BY sre_type;
   ```

<div align="center" >
 <img src="BD tablas/11.jpg">
 </div>
 
12. Encontrar los dispositivos que han experimentado un aumento del 20% en la
temperatura en la última hora.
```sh
   SELECT sen_id, sen_name
FROM Sensors
WHERE sen_type = 'temperatura'
AND sen_id IN (
    SELECT sre_sen_id
    FROM SensorsReading
    WHERE sre_date >= DATE_SUB(NOW(), INTERVAL 1 HOUR)
)
AND sen_id IN (
    SELECT sre_sen_id
    FROM SensorsReading
    WHERE sre_date >= DATE_SUB(NOW(), INTERVAL 2 HOUR)
)
AND ((SELECT MAX(sre_value) FROM SensorsReading 
WHERE sre_sen_id = Sensors.sen_id AND sre_date >= DATE_SUB(NOW(), INTERVAL 1 HOUR)) - 
(SELECT MAX(sre_value) FROM SensorsReading 
WHERE sre_sen_id = Sensors.sen_id AND sre_date >= DATE_SUB(NOW(), INTERVAL 2 HOUR))) / 
(SELECT MAX(sre_value) FROM SensorsReading WHERE sre_sen_id = Sensors.sen_id AND sre_date >= DATE_SUB(NOW(), INTERVAL 2 HOUR)) * 100 > 20;
   ```

<div align="center" >
 <img src="BD tablas/12.jpg">
 </div>
 
13. Mostrar los dispositivos cuya humedad promedio sea inferior al 30% y que estén
actualmente inactivos.
```sh
   SELECT sen_id, sen_name
FROM Sensors
WHERE sen_id NOT IN (
    SELECT sre_sen_id
    FROM SensorsReading
    WHERE sre_description LIKE '%humedad%'  AND sre_value < 50.00
);
   ```

<div align="center" >
 <img src="BD tablas/13.jpg">
 </div>
 
14. Obtener la cantidad de datos de sensores registrados para cada dispositivo, agrupados
por día.
```sh
   SELECT sre_id, DATE(sre_date) AS fecha, COUNT(*) AS cantidad_datos
FROM SensorsReading
GROUP BY sre_id, DATE(sre_date);
   ```

<div align="center" >
 <img src="BD tablas/14.jpg">
 </div>
 
15. Calcular la temperatura promedio por cada hora del día para todos los dispositivos.
```sh
   SELECT HOUR(sre_date) AS hora_del_dia, AVG(sre_value) AS average_temperature
FROM SensorsReading
WHERE sre_type = 'c' 
GROUP BY HOUR(sre_date);
   ```

<div align="center" >
 <img src="BD tablas/15.jpg">
 </div>
 
16. Encontrar los dispositivos que tengan tanto sensores de temperatura como sensores de
humedad.
```sh
   SELECT dev_id, dev_name
FROM Devices
WHERE dev_id IN (
    SELECT DISTINCT sen_dev_id
    FROM Sensors
    WHERE sen_name IN ('termometro', 'higrometro')
);
   ```

<div align="center" >
 <img src="BD tablas/16.jpg">
 </div>
 
17. Mostrar los dispositivos cuya temperatura máxima haya superado un cierto umbral en
los últimos siete días.
```sh
   SELECT dev_id, dev_name
FROM Devices
WHERE dev_id IN (
    SELECT sen_dev_id
    FROM Sensors
    WHERE sen_name = 'termometro'
) AND dev_id IN (
    SELECT sre_sen_id
    FROM sensorsreading
    WHERE sre_description = 'Lectura de temperatura'
    AND sre_value > 30
    AND sre_date >= DATE_SUB(NOW(), INTERVAL 7 DAY)
);
   ```

<div align="center" >
 <img src="BD tablas/17.jpg">
 </div>
 
18. Obtener la lista de dispositivos junto con la cantidad de datos de sensores registrados,
ordenados por fecha de registro.
```sh
   SELECT dev_id,dev_name,dev_type,
    COUNT(sre_id) AS cantidad_datos_sensores,
    MAX(sre_date) AS fecha_registro_mas_reciente
FROM devices 
LEFT JOIN sensors  ON dev_id = sen_dev_id
LEFT JOIN sensorsreading  ON sen_id = sre_sen_id
GROUP BY dev_id
ORDER BY fecha_registro_mas_reciente DESC;
   ```

<div align="center" >
 <img src="BD tablas/18.jpg">
 </div>
 
19. Calcular la diferencia de temperatura máxima y mínima registrada para cada dispositivo.
```sh
   
SELECT MAX(sre_value) AS temperatura_maxima,
		MIN(sre_value) AS temperatura_minima
FROM sensorsreading
WHERE sre_type = 'c';
   ```

<div align="center" >
 <img src="BD tablas/19.jpg">
 </div>
 
20. Encontrar los dispositivos que han enviado datos constantemente durante las últimas 48
horas.
```sh
   SELECT dev_id, dev_name, dev_type, MAX(sr.sre_date) AS ultima_fecha_envio
FROM devices 
LEFT JOIN sensors ON dev_id = sen_dev_id
LEFT JOIN sensorsreading sr ON sen_id = sre_sen_id
WHERE sre_date >= NOW() - INTERVAL 48 HOUR
GROUP BY dev_id
HAVING ultima_fecha_envio IS NOT NULL
ORDER BY ultima_fecha_envio DESC;
   ```

<div align="center" >
 <img src="BD tablas/20.jpg">
 </div>
 
21. Mostrar los dispositivos cuya humedad promedio sea superior al 60% y que estén
actualmente activos.
```sh
   SELECT dev_id,dev_name,dev_type,AVG(sre_value) AS humedad_promedio
FROM devices 
JOIN sensors  ON dev_id = sen_dev_id
JOIN sensorsreading  ON sen_id = sre_sen_id
WHERE sre_type = '%'
GROUP BY dev_id
HAVING humedad_promedio > 60
ORDER BY humedad_promedio DESC;
   ```

<div align="center" >
 <img src="BD tablas/21.jpg">
 </div>
 
22. Obtener la suma total de datos de sensores para cada dispositivo, excluyendo aquellos
cuyo estado sea "inactivo".
```sh
   SELECT d.dev_id,d.dev_name,d.dev_type,SUM(sr.sre_value) AS suma_total_datos_sensores
FROM devices d
JOIN sensors s ON d.dev_id = s.sen_dev_id
JOIN sensorsreading sr ON s.sen_id = sr.sre_sen_id
GROUP BY d.dev_id
ORDER BY suma_total_datos_sensores DESC;
   ```

<div align="center" >
 <img src="BD tablas/22.jpg">
 </div>
 
23. Calcular la temperatura promedio por cada día de la semana para todos los dispositivos.
```sh
   SELECT DAYNAME(sre_date) AS dia_semana,AVG(sre_value) AS temperatura_promedio
FROM sensorsreading 
JOIN sensors  ON sre_sen_id = sen_id
JOIN devices ON sen_dev_id = dev_id
WHERE sre_type = 'c'
GROUP BY dia_semana
ORDER BY FIELD(dia_semana, 'Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday', 'Sunday');
   ```

<div align="center" >
 <img src="BD tablas/23.jpg">
 </div>
 
24. Encontrar los dispositivos cuyo consumo total de energía supere cierto umbral.
```sh
   SELECT dev_id,dev_name,dev_type,MAX(sre_value) AS maxima_lectura
FROM devices 
JOIN sensors ON dev_id = sen_dev_id
JOIN sensorsreading ON sen_id = sre_sen_id
GROUP BY dev_id, dev_name, dev_type
ORDER BY maxima_lectura DESC
LIMIT 3;
   ```

<div align="center" >
 <img src="BD tablas/24.jpg">
 </div>
 
25. Mostrar los dispositivos que hayan experimentado una variación de temperatura superior al 15% en la última hora.
```sh
   SELECT dev_id,dev_name,dev_type,sre_value AS humedad_lectura
FROM devices 
JOIN sensors ON dev_id = sen_dev_id
JOIN sensorsreading ON sen_id = sre_sen_id
WHERE sre_type = '%'
AND sre_value > 15
ORDER BY sre_value DESC;
   ```

<div align="center" >
 <img src="BD tablas/25.jpg">
 </div>
 

<p align="right">(<a href="#readme-top">Regresar al Inicio</a>)</p>

