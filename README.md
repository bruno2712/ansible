# Ansible para empresas

Este repositorio proporciona una base sólida y segura para la implementación de un servidor Ansible en entornos corporativos. El objetivo es establecer una configuración inicial que cumpla buenas prácticas de seguridad y simplifiquen la administración del servidor.
    
En la carpeta playbooks se incluyen ejemplos de playbooks en formato YAML, listos para ser adaptados y utilizados en escenarios empresariales comunes, como la gestión de configuraciones, el despliegue de aplicaciones o la automatización de tareas de mantenimiento.

## Preparación servidores

### Servidores destino

En los servidores que queramos automatizar con ansible vamos a crear un usuario de servicio el cual va a ser el encargado de ejecutar los playbooks, este será un usuario de servicio y sin loguin.  

```bash
sudo useradd -r -s /sbin/nologin ansible
```
Agregamos un archivo en la carpeta sudoers.d para darle permisos de sudo y sin contraseña a nuestro usuario (esto porque  usara una key ssh)
```bash
sudo vim /etc/sudoers.d/ansible

# Agregamos el siguiente contenido
ansible ALL=(ALL) NOPASSWD: ALL
```

Preparamos la conexión SSH

``` bash
sudo mkdir -p /home/ansible/.ssh
sudo touch /home/ansible/.ssh/authorized_keys

# Asignamos permisos
sudo chown -R ansible:ansible /home/ansible/.ssh
sudo chmod 700 /home/ansible/.ssh
sudo chmod 600 /home/ansible/.ssh/authorized_keys
```
---
### Servidor ansible

Instalamos los paquetes de ansible, git y descargamos el repositorio

```bash
sudo dnf install ansible-core git -y
  
git clone https://github.com/bruno2712/ansible.git
```  

Copiamos nuestra calve publica ssh al servidor, reemplazando [ usuario ] por un usuario existente en el servidor destino y con permisos sudo. En [ servidor_destino ] reemplazamos por la ip de nuestro servidor destino

```bash
ssh-copy-id [ usuario ]@[ servidor_destino ]
```

Por ultimo debemos copiar esta clave al archivo que creamos anteriormente, por lo que nos conectamos por ssh o de manera local al servidor y movemos la clave

```bash
ssh [ usuario ]@[ servidor_destino ]

sudo cp /[ usuario ]/.ssh/authorized_keys /home/ansible/.ssh/authorized_keys

# Nos aseguramos que el dueño del archivo sea ansible
sudo chown ansible:ansible /home/ansible/.ssh/authorized_keys
```

Una vez tenemos el repositorio clonado y la clave ssh configurada debemos editar el archvio de inventario y adaptarlo a nuestra infraestructura para poder utilizar los playbooks

```bash
vim ansible/inventarios/inventario.ini
```


## License

[MIT](https://choosealicense.com/licenses/mit/)
