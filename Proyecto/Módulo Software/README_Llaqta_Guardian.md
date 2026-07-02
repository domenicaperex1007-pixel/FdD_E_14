# Llaqta Guardian

**Llaqta Guardian** es un prototipo de monitoreo y alerta temprana de inundaciones pensado para zonas vulnerables cercanas a ríos, como Requena, Loreto. El sistema combina un módulo físico con sensores y una plataforma web local para registrar datos, evaluar el riesgo y apoyar la toma de decisiones de dirigentes o autoridades locales.

## Funcionalidades principales

- Lectura de distancia del agua mediante sensor ultrasónico impermeable.
- Conversión de distancia a nivel de agua usando la referencia física del prototipo.
- Lectura de temperatura y humedad con DHT22.
- Detección de lluvia mediante sensor FC-37.
- Envío de datos desde ESP32 hacia una API Flask por WiFi.
- Almacenamiento histórico en SQLite.
- Dashboard web con nivel de agua, lluvia, riesgo, confianza, predicción y gráficas.
- Clasificación del estado en: `NORMAL`, `PREVENTIVO`, `ALERTA` y `CRITICO`.
- Alertas locales mediante LED y módulo MP3.

## Medidas consideradas del prototipo

- Altura del sensor ultrasónico: **45 cm**.
- Distancia mínima confiable del sensor: **20 cm**.
- Nivel máximo confiable: **25 cm**.
- Umbral preventivo: **10 cm**.
- Umbral de alerta: **18 cm**.
- Umbral crítico: **23 cm**.

La fórmula usada es:

```txt
nivel_agua_cm = altura_sensor_cm - distancia_sensor_agua_cm
```

Si la distancia medida es menor o igual a 20 cm, el sistema considera que el agua llegó a la zona muerta del sensor y marca riesgo crítico por seguridad.

## Estructura del proyecto

```txt
llaqta_software/
├── app2.py
├── llaqta_guardian.db
└── templates/
    └── index2.html

sketch_jun25a/
└── sketch_jun25a.ino
```

## Tecnologías usadas

### Hardware / Arduino IDE

- ESP32.
- Sensor ultrasónico impermeable.
- DHT22.
- Sensor de lluvia FC-37.
- LED de alerta.
- Módulo MP3 para mensajes de alerta.
- WiFi para comunicación con el servidor local.

### Software

- Python.
- Flask.
- Flask-CORS.
- SQLite.
- HTML, CSS y JavaScript.
- ArduinoJson para el envío de datos desde ESP32.

## Instalación del software

Instalar dependencias:

```bash
pip install flask flask-cors
```

Ejecutar el servidor:

```bash
python app2.py
```

Abrir el dashboard en el navegador:

```txt
http://127.0.0.1:5000
```

## Configuración del ESP32

En el archivo `sketch_jun25a.ino`, configurar el WiFi y la IP de la laptop donde corre Flask:

```cpp
const char* WIFI_SSID = "NOMBRE_DEL_WIFI";
const char* WIFI_PASSWORD = "CONTRASEÑA_DEL_WIFI";
const char* SERVER_URL = "http://IP_DE_LA_LAPTOP:5000/api/medicion";
```

Ejemplo:

```cpp
const char* SERVER_URL = "http://10.255.118.127:5000/api/medicion";
```

La laptop y el ESP32 deben estar conectados a la misma red WiFi.

## Endpoints principales

```txt
GET  /                 Dashboard web
POST /api/medicion     Recibe datos del ESP32
GET  /api/ultimas      Devuelve las últimas mediciones
POST /api/reiniciar    Borra el historial de mediciones
```

## Nota de seguridad

No subir contraseñas reales de WiFi ni datos sensibles al repositorio. Se recomienda usar valores de ejemplo en el código público.

## Estado del proyecto

Prototipo funcional en etapa de pruebas. El sistema ya permite recibir datos del ESP32, almacenarlos, visualizarlos en un dashboard local y calcular el nivel de riesgo de inundación.
