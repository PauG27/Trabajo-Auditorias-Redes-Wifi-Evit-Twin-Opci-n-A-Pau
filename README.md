Opción A: Ataque Evil Twin contra WPA2 PSK 
En este tipo de ataque, se realiza el ataque Evil Twin contra una wifi que usa el sistema de cifrado WPA PSK. El objetivo es conseguir que la víctima se conecte a un punto de acceso malicioso que suplanta la red wifi de la empresa que usa WPA con clave compartida (PSK). 
 



Preparación de la antena WiFi

•	Detección de la interfaz
La antena fue detectada.

•	Activación del modo monitor
La interfaz se colocó en modo monitor, requisito indispensable para:

•	Escanear redes

•	Capturar tráfico

•	Permitir la creación del AP falso

Sin este paso previo no se podría realizar el ataque. En caso de que falle la antena, repetir estos pasos resolvería el problema.

   <img width="856" height="597" alt="image" src="https://github.com/user-attachments/assets/5f672c21-8da4-4d91-bd63-54e293a5dc12" />



Ejecución de Airgeddon
Al iniciar Airgeddon, se verificaron:

•	Permisos root

•	Resolución

•	Compatibilidad con Kali

•	Herramientas esenciales

•	Herramientas opcionales

En mi caso los errores mostrados en las capturas no afectan en la realización del ataque.

  <img width="636" height="205" alt="image" src="https://github.com/user-attachments/assets/60ea23d2-d994-45c8-ad67-d3e997b9269e" />


  <img width="668" height="354" alt="image" src="https://github.com/user-attachments/assets/3d3aba21-17e9-4a37-92e3-184e3a445207" />






Selección de la interfaz y acceso al menú Evil Twin
En el menú principal se seleccionó antes del inicio del ataque la opción 2 WLAN0, dada que es la opción que necesitamos para realizar el ataque mediante nuestra antena.

  <img width="1019" height="200" alt="image" src="https://github.com/user-attachments/assets/06b67699-2520-4b2a-a4e7-e813f54102f8" />


7. Evil Twin attacks menú
Airgeddon mostró la interfaz en modo monitor y permitió acceder al menú específico del ataque, en nuestro caso será el 9.



  <img width="732" height="623" alt="image" src="https://github.com/user-attachments/assets/b5904b24-8c0a-41af-961a-22714514b16f" />





Exploración de objetivos
Se realizó un escaneo de redes WPA/WPA2/WPA3. El sistema detectó múltiples redes y estaciones asociadas.
La red seleccionada para la práctica fue:

•	ESSID: CETI

•	BSSID: 18:D6:C7:51:4E:68

Airgeddon validó que la red era apta para un ataque Evil Twin con portal cautivo.


  <img width="477" height="487" alt="image" src="https://github.com/user-attachments/assets/ff690ceb-a369-455b-b07f-46d828f55d3e" />






Selección del ataque
Se eligió la opción:

9. Evil Twin AP attack with captive portal

Este ataque es el único que no requiere una segunda interfaz con acceso a Inter-net.
 
<img width="939" height="989" alt="image" src="https://github.com/user-attachments/assets/7cebe013-2c5a-47da-b66a-7a414dbc9d97" />




Captura del Handshake / PMKID
Airgeddon preguntó si ya se disponía de un handshake previo. (No seleccioné crear uno nuevo porque entraba en bucle, seguramente porque realicé el ataque varias veces y ya lo tenía, lo he explicado en el video).

El sistema:

•	Abrió dos ventanas (captura + desautenticación)

•	Esperó 20 segundos

•	Detectó correctamente un Handshake

•	Detectó además un PMKID válido

El archivo se guardó en la ruta propuesta.

 
  <img width="939" height="490" alt="image" src="https://github.com/user-attachments/assets/dfd40e9f-0708-4da0-b261-cc9722f69a21" />


Levantamiento del punto de acceso falso (Evil Twin)

Airgeddon inició automáticamente:

•	Hostapd: Creación del AP falso

•	Dnsmasq: servidor DHCP y DNS

•	Lighttpd: servidor web del portal cautivo

•	Mecanismo de autenticación: Para forzar reconexión del cliente

En las ventanas se observaron:
AP
Autenticación y asociación del cliente de pruebas.
DHCP
Asignación de IP privada (192.169.1.33).
DNS
Redirección de todas las peticiones a la página del portal.
Webserver
Inicio del servidor web con el portal TP-Link modificado.
Control
Estado del ataque, tiempo online y clientes conectados.

  <img width="940" height="450" alt="image" src="https://github.com/user-attachments/assets/a13883fd-86c2-44fc-bc4c-be98c6100366" />




Interacción del cliente con el portal cautivo
El dispositivo móvil, al intentar conectarse a la red “CETI”, fue redirigido automáticamente al portal falso.
El portal mostraba:

•	ESSID: CETI

•	Formulario de contraseña

•	Botón “Enviar”
El usuario introdujo una contraseña incorrecta, lo que generó el mensaje:
“Contraseña Wi-Fi incorrecta”
Esto demuestra que el portal estaba funcionando correctamente y que el cliente estaba siendo redirigido por el DNS spoofing.


  <img width="591" height="242" alt="image" src="https://github.com/user-attachments/assets/c37bf361-fa24-4210-82c0-de0514355ce9" />


  <img width="506" height="592" alt="image" src="https://github.com/user-attachments/assets/3fbbaf92-35b3-4e51-b58f-e04640f78ea3" />


 
Una vez insertadas las credenciales en nuestro dispositivo, Airgeddon captará las mismas y las recopilará.


  <img width="940" height="457" alt="image" src="https://github.com/user-attachments/assets/e1d00adf-4baa-4d6e-9c3e-67983f8de5c6" />





Podemos consultar las credenciales capturadas en la ruta seleccionada, demos-trando que el ataque fue un éxito.


  <img width="938" height="222" alt="image" src="https://github.com/user-attachments/assets/46470e4c-b083-4ef4-8d3d-6e18fd8d774c" />




Conclusión

La práctica realizada permitió ejecutar de manera integral un ataque Evil Twin con portal cautivo, cubriendo satisfactoriamente todas las fases críticas: desde la configuración de la antena en modo monitor y el escaneo de redes mediante Airgeddon, hasta la captura del handshake y el despliegue de un punto de acceso falso con servicios DHCP, DNS y HTTP completamente funcionales. El éxito en la redirección del tráfico del cliente y la posterior obtención de credenciales a través del portal falso demostraron que, más allá de la robustez del cifrado, el factor humano sigue siendo un vector de riesgo determinante en redes WPA2-PSK. En conclusión, la experiencia subraya la vulnerabilidad de las redes con claves compartidas y la necesidad imperativa de adoptar estándares más seguros como WPA3, así como sistemas de monitorización activa para detectar este tipo de suplantaciones en tiempo real.



