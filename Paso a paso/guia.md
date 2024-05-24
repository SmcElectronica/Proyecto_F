<img src="https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgV4mhigbSMzzAbN3XyYDbUg1xCvX20QHRJruRaO90HErE-qXFSl_RKp9kNsEpnYoi_MeK0DG8rfFES48OY9WJIcUCmeQLUD_gwxSeoIFOAytfvG2tAu0UoBJa4-YTvbnmIRmuUv5s4y5pw/s1600/nagios_core.png" width="700" height="200">

## Integrantes

  - Juan Esteban Pulido
  - Juan Sebastian Valencia Charry
  - Geraldine Filigrana Sanchez
  - Kevin José Cadena Hurtado
  - Sergio Andres Cardenas Muñoz

# Monitoreo de infraestructura con Nagios

## 1. Preparación de las máquinas virtuales

Primeramente creamos el VagrantFile con el servidor y los clientes

### En el servidor
  - Iniciamos dandole permisos de superusuario
```
  sudo -i
```
  - Instalamos el vim y net-tools, con el siguiene comando
  ```
  yum install -y vim net-tools
  ```
  -  Se deshabilita SeLinux en el directorio: <code>/etc/selinux/</code>
  ```
  cd /etc/selinux/
  vim config
  ```
  - Luego habilitamos el firewall
  ```
  systemctl start firewalld
  ```
  - Agregamos los puertos 80 y 443
  ```
  firewall-cmd --permanet --add-port=80/tcp
  firewall-cmd --permanet --add-port=443/tcp
  ```
  - Hacemos un reboot
  ```
  systemctl reboot
  ```
  - Nos volvemos a loguear al servidor y procedemos con las dependcias
## 2. Instalación de las dependencias necesarias para instalar Nagios
  
  - Con el siguiente comando instalamos todas las dependecias que se necesitaran para hacer la instalacion de nagios en el servidor
  ```
  yum install -y gettext wget net-snmp-utils openssl-devel glibc-common unzip perl epel-release gcc php gd automake autoconf httpd make glibc gd-devel net-snmp
  ```
  - Posterior a este instalamos el Perl-Net
  ```
    yum install perl-Net-SNMP
  ```
1. Crearmos un usuario un Nagios y agregarlo como grupo secundario con:
```
  useradd nagios
  usermod -a -G nagios apache
```
2. Obtenermos el paquete de nagios del repositorio:

```
https://github.com/NagiosEnterprises/nagioscore/releases
```

Copiamos y pegamos la direccion del paquete segun la version en la maquina, nos quedaria asi:

```
wget https://github.com/NagiosEnterprises/nagioscore/releases/download/nagios-4.5.2/nagios-4.5.2.tar.gz
```

3. Descomprimir el archivo con:
```
tar -xzvf nagios-4.5.2.tar.gz
```
4. Ingresamos al directorio de los archivos descomprimidos anteriormente e ingresar el siguiente comando:
```
./configure
```
5. Compilar Nagios con el comando:
```
make all
```
6. Hacer la instalación con:
```
make install
```
7. Continuar con los siguentes comandos: 
```
make install-init
make install-commandmode
make install-config
make install-webconf
```
8. Habilitar los servicios de Nagios y HTTPD con: 
```
systemctl enable nagios
systemctl enable httpd
```
9. Crear un usuario y contraseña para acceder a Nagios con: 
```
htpasswd -c /usr/local/nagios/etc/htpasswd.users nagiosadmin
```
10. Activar los servicios con:
```
systemctl start nagios
systemctl start httpd
```
Se puede verificar la correcta activación con:
```
systemctl status nagios
systemctl status httpd
```
```
systemctl is-enabled nagios
```
En esta instancia ya se puede revisar la pagina de nagios para revisar que quedo correcamente instalado se puede ingresar en el
navegador con: <code>http://"tu_direccion_ip_servidor"/nagios</code>

En nuestro caso es:
```
http://192.168.50.3/nagios
```
Logueamos las credenciales y ya podremos ver a nagios funcionando

### 2.1 Instalación de plugins en el servidor

1. Obtener los plugins de:

```
https://github.com/nagios-plugins/nagios-plugins/releases
```

En la máquina virtual:

```
wget https://github.com/nagios-plugins/nagios-plugins/releases/download/release-2.4.10/nagios-plugins-2.4.10.tar.gz
```

2. Descomprimir los archivos con:
```
tar -xzvf nagios-plugins-2.4.10.tar.gz</code>
```
3. Dentro del directorio del archivo descomprimido se ingresa el comando:
```
./configure
```
4. Hacer la instalación con:
```
make install
```
5. Reiniciar el servicio de Nagios.


💡 **Los plugins se instalan en el directorio** <code>/usr/local/nagios/libexec/</code> **y dentro del directorio se puede ejecutar el comando**
<code>./plugin_name --help</code> **para ver como se usa dicho plugin.**
  - 
  ```
