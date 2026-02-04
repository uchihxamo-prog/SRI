

🛠️ Fase 1: Configuración del Servidor (Ubuntu Server)
1.1 Configuración de Red (Adaptador Puente)



1.2 Verificación de Sonido e Instalación de Icecast2

    Comprobación de sonido: aplay -l o speaker-test.

    Instalación:
    Bash

    sudo apt update && sudo apt install icecast2 -y

    Estado del servicio: systemctl status icecast2

🎧 Fase 2: Configuración del DJ (Ubuntu Desktop)
2.1 Entorno y Mixxx

    Red: Configurada en modo puente para alcanzar al servidor.

    Instalación de Mixxx:
    Bash

    sudo add-apt-repository ppa:mixxx/mixxx
    sudo apt update && sudo apt install mixxx -y

2.2 Creación de la Radio (/tu_nombre)

Configuración de la conexión en Mixxx (Preferencias -> Live Broadcasting):

    Tipo de servidor: Icecast2

  

    Mount: /cris

    Puerto: 8000

    
<img width="1280" height="800" alt="icecast1" src="https://github.com/user-attachments/assets/136e6bc5-5071-44b2-9429-d9d639f9dd00" />

🖥️ Fase 3: Pruebas de Funcionamiento (Anfitrión)

Se ha verificado la emisión desde el equipo anfitrión accediendo a la URL: http://[IP_DEL_SERVIDOR]:8000/manu
Medio de Prueba	Resultado	Observaciones
Navegador Web	✅ Funciona	Probado en Chrome/Firefox
VLC Player	✅ Funciona	Abrir volcado de red

    Nota: Para VLC, se usó la opción "Abrir ubicación de red" con la URL completa del mountpoint.
<img width="1280" height="800" alt="icecast5" src="https://github.com/user-attachments/assets/1c4442b8-1328-45df-921d-e39ed099fcb6" />
🤝 Fase 4: Interconexión con Compañeros



    URL de la radio del compañero: http://[IP_COMPAÑERO]:8000/[MOUNT]
<img width="1280" height="800" alt="icecast6" src="https://github.com/user-attachments/assets/3dcb10a9-60d9-4a2a-aabc-b200671cc181" />

    Resultado: Conexión exitosa y audio fluido.





    Servidor Icecast funcionando: ![Icecast Status]

    Mixxx conectado: ![Mixxx Config]

    Escuchando en VLC: ![VLC Evidence]

    
