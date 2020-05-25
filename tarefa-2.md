Instalacion MariaDB (LINUX)

Vamos a instalar MariaDB desde la ventana de comando de Linux, con los siguientes comandos:
~~~~
sudo apt install mariadb-server mariadb-client
~~~~
![imagen](https://raw.githubusercontent.com/AlbertoMartinezPose/Apuntes-base-de-datos_2-dql-dml/master/1.PNG)
Podemos ver a versión no seguinte comando:
~~~~
mysql --version
~~~~
![imagen](https://raw.githubusercontent.com/AlbertoMartinezPose/Apuntes-base-de-datos_2-dql-dml/master/2.PNG)
Para verificar que funciona podemos verlo con el siguiente comando:
~~~~
systemctl status mysql.service
~~~~
![imagen](https://raw.githubusercontent.com/AlbertoMartinezPose/Apuntes-base-de-datos_2-dql-dml/master/3.PNG)

Y ya estaria todo listo.
