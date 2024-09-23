# ARPscanner
**ARPscanner** es un programa de red desarrollado en Python que utiliza la biblioteca `scapy` para realizar escaneos ARP en un host o rango de IPs objetivo. El programa permite identificar dispositivos activos en la red a través del protocolo ARP (Address Resolution Protocol).

## Características
- Escaneo de dispositivos en una red local utilizando ARP.
- Capacidad para especificar un host o rango de IPs objetivo.
- Muestra una lista de dispositivos que han respondido al paquete ARP.
- Manejo de señales SIGINT (Ctrl+C) para salir del programa de manera segura.

## Requisitos

- Python 3.x
- Biblioteca `scapy` (puede instalarse con `pip install scapy`)
- Biblioteca `termcolor` (puede instalarse con `pip install termcolor`)

## Instalación

1. Clona o descarga el repositorio.
2. Instala las dependencias necesarias:
    ```bash
    pip install scapy termcolor
    ```

## Uso

1. Abre una terminal y ejecuta el programa con privilegios de administrador (ya que las operaciones ARP suelen requerir permisos elevados).
2. Proporciona el parámetro `-t` o `--target` seguido del host o rango de IPs que deseas escanear. Por ejemplo:
    ```bash
    sudo python3 ARPscanner.py -t 192.168.1.0/24
    ```

   - El argumento `-t` (o `--target`) es obligatorio y define el host o rango de IPs a escanear. Ejemplo: `192.168.1.0/24` para escanear toda la subred.

3. Si deseas detener el programa durante el escaneo, presiona `Ctrl+C`. El programa manejará la interrupción de manera segura y mostrará un mensaje de salida.

## Explicación del Código

1. **Manejo de Señales (Ctrl+C):**
    - El programa utiliza la función `signal.signal()` para capturar la señal `SIGINT` (interrupción generada al presionar `Ctrl+C`) y ejecutar la función `def_handler`, que imprime un mensaje de salida y termina el programa limpiamente.

2. **Obteniendo Argumentos:**
    - La función `get_arguments()` utiliza la biblioteca `argparse` para manejar los argumentos del programa. En este caso, requiere que el usuario especifique el objetivo a escanear usando el argumento `-t` o `--target`.

3. **Proceso de Escaneo ARP:**
    - El método `scan(ip)` crea un paquete ARP utilizando `scapy.ARP()` dirigido al rango IP proporcionado.
    - Luego, crea un paquete Ethernet con dirección de broadcast (`ff:ff:ff:ff:ff:ff`) utilizando `scapy.Ether()`.
    - Ambos paquetes se combinan y se envían a la red mediante `scapy.srp()` que envía y recibe paquetes personalizados a nivel de enlace.
    - Si hay dispositivos que responden al paquete ARP, el programa muestra la respuesta resumida.

4. **Ejecución Principal:**
    - La función `main()` maneja la obtención del objetivo de escaneo y llama a la función `scan()` para realizar el escaneo ARP.

## Ejemplo de Salida

Al ejecutar el programa correctamente, se verá una salida similar a la siguiente:

```bash
Ether / ARP who has 192.168.1.1 says 192.168.1.10
Ether / ARP who has 192.168.1.2 says 192.168.1.20
