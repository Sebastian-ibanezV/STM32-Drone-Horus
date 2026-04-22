# DesControl / Horus Link 🛸
**Sistema de Estabilización de Vuelo en Cascada (Angle + Rate Control)**

Este proyecto consiste en el diseño, desarrollo e implementación de un controlador de vuelo para un dron cuadricóptero, utilizando una arquitectura distribuida entre un microcontrolador de tiempo real (**STM32**) y una unidad de cómputo de alto nivel (**Raspberry Pi 5**).

## 🚀 Descripción del Proyecto
El objetivo principal es lograr la estabilización autónoma del eje de Roll (alabeo) mediante un control en cascada. El sistema utiliza un filtro complementario para la fusión de sensores y un enlace de radiofrecuencia de baja latencia para telemetría y ajuste de parámetros en tiempo real.

### Características Principales
- **Control en Cascada:** Lazo externo para control de Ángulo (Jefe) y lazo interno para control de Tasa/Velocidad Angular (Obrero).
- **Arquitectura Robusta:** Procesamiento de PID y PWM a 200 Hz en la STM32.
- **GCS (Ground Control Station):** Interfaz en Python sobre Raspberry Pi 5 para monitoreo, graficación en tiempo real y ajuste dinámico de ganancias PID.
- **Protocolo Custom:** Comunicación bidireccional optimizada mediante ACK Payloads sobre NRF24L01.

---

## Hardware & Arquitectura

| Componente | Detalle |
| :--- | :--- |
| **Controlador de Vuelo** | STM32F411 (Blackpill) |
| **Estación de Tierra** | Raspberry Pi 5 |
| **IMU (Sensor)** | MPU9250 (Giroscopio + Acelerómetro) |
| **Enlace RF** | NRF24L01+ (SPI) |
| **Motores** | 4 Motores Brushless con ESCs (Protocolo PWM) |

### Diagrama de Conexión (Vista Plana)
![Vista Plana del Dron](./path/a/tu/imagen_plana.jpg)
*(Placeholder: Aquí se detalla la disposición del hardware y el orden de los motores M1-M4 en configuración X)*

---

## Estructura del Software

### 1. STM32 (Firmware C)
Ubicado en `/stm32_core`. Se encarga de:
- Lectura de IMU vía I2C a alta velocidad.
- Filtro Complementario: `0.98 * Gyro + 0.02 * Accel`.
- Generación de señales PWM (1000us - 2000us).
- Lógica de Failsafe ante pérdida de señal.

### 2. Raspberry Pi (GCS Python)
Ubicado en `/pi_gcs`. Ofrece:
- **CLI Interactiva:** Comandos para armar, desarmar, calibrar y setear el acelerador.
- **Live Tuning:** Ajuste de $K_p, K_i, K_d$ y $Kp_{angle}$ sin reiniciar el dron.
- **Logging:** Exportación de telemetría a CSV para análisis de datos posterior.

---

## Control en Cascada (PID)
El sistema opera bajo la siguiente jerarquía:
1. **Lazo de Ángulo:** Recibe el `Setpoint` en grados y genera una orden de velocidad angular.
2. **Lazo de Rate:** Recibe la orden de velocidad y ajusta los motores para contrarrestar la inercia y perturbaciones.

---

## Demostración en Video
Puedes ver el funcionamiento del sistema, la respuesta del PID y la telemetría en vivo en el siguiente enlace:

[![Video del Proyecto](https://img.youtube.com/vi/TU_VIDEO_ID/0.jpg)](https://www.youtube.com/watch?v=TU_VIDEO_ID)

---

## Instalación y Uso

### En la Raspberry Pi 5:
1. Instalar dependencias:
   ```bash
   pip install pyrf24 matplotlib
