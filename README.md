# Info de la materia: ST0263 Tópicos especiales de telemática
#
# Estudiante(s): 
- Sara María Cardona Villada - smcardonav@eafit.edu.co
- Luisa María Polanco Rodríguez - lmpolanco1@eafit.edu.co
                
# Profesor: Alvaro Enrique Ospina Sanjuan - aeospinas@eafit.edu.co

# Reto 2
#
# 1. Breve descripción de la actividad
Desplegar un CMS Wordpress en un clúster de alta disponibilidad en Kubernetes autoescalado en nube, con su propio dominio y certificado SSL.
Utilizar nginx COMO BALANCEADOR DE CARGAS (LB) de la capa de aplicación de wordpress. Además de lo anterior, se utilizarán 2 servidores adicionales, uno
para BASE DE DATOS (DBServer) y otro para ARCHIVOS(FileServer). 
## 1.1. Que aspectos cumplió o desarrolló de la actividad propuesta por el profesor (requerimientos funcionales y no funcionales)
Se cumplió con el despliegue de una aplicación wordpress en un cluster de kubernetes dentro de este mismo cluster se desplegó un statefulset 
  para la base de datos que a través de un service se disponibiliza para poder ser usado en el deployment de la aplicación de Wordpress. 
  Se logró la creación de un EFS en AWS el cual a través de su id y un punto de acceso se asoció a los persistent volumes (pv) y a su vez a los 
  persistent volume claims (pvc) del wordpress. También se hizo uso de NgInx como balanceador de carga que a su vez fue usado para redirigir todo el 
  tráfico del dominio reto2k8s.online. Este dominio se registro en Route 53 y se le emitió un certificado a tavés del Certificate Manager de AWS. 
  
## 1.2. Que aspectos NO cumplió o desarrolló de la actividad propuesta por el profesor (requerimientos funcionales y no funcionales)
Todos los puntos generales fueron cumplidos. 

# 2. Información general de diseño de alto nivel, arquitectura, patrones, mejores prácticas utilizadas.
![reto2_kubernetes drawio](https://github.com/user-attachments/assets/c376e079-cf84-4441-906a-0e4a1ae8323f)


# 3. Descripción del ambiente de desarrollo y técnico: lenguaje de programación, librerias, paquetes, etc, con sus numeros de versiones.
Este proyecto fue desarrollado a través de la creación de un cluster de kubernetes en AWS, a este se le creó un grupo de nodos con instancias de tipo t2.small con un minimo de nodos 1 y un máximo de 3. 

## Detalles del desarrollo.
Para el despliegue de la aplicación de Kubernetes y sus distintos componentes se tomaron los siguientes pasos: 
- Creación de EFS y configuración de punto de acceso
- Registro y configuración de dominio en Route 53
- Emisión de certificado de seguridad a través de Certificate Manager AWS
- Instalación de csi drivers para conexión con EFS en cluster
- Habilitar puertos para NFS en las reglas de entrada y salida de los grupos de seguridad del NFS y del cluster.
- Crear archivos .yaml
- Instalar NGINX Ingress Controller:
    kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/main/deploy/static/provider/cloud/deploy.yaml
- Aplicar cambios de archivos en el siguiente orden:
  - kubectl apply -f mysql-pv-pvc.yaml
  - kubectl apply -f mysql-statefulset.yaml
  - kubectl apply -f mysql-service.yaml
  - kubectl apply -f wordpress-pv-pvc.yaml
  - kubectl apply -f wordpress-deployment.yaml
  - kubectl apply -f wordpress-ingress.yaml
- Verificar que los componentes se estén ejecutando:
![image](https://github.com/user-attachments/assets/c8e696ee-9769-478a-bb7d-7535ac2205d6)
![image](https://github.com/user-attachments/assets/d1c596f9-5b76-4f6b-81cc-513cf63a2b18)
![image](https://github.com/user-attachments/assets/5eaa2c97-f5df-4edd-9306-49aca8f1e6b5)
![image](https://github.com/user-attachments/assets/2cdf56a1-5dfc-4a55-a39c-9d8a453ffdd5)
![image](https://github.com/user-attachments/assets/7db0e09d-f3e6-4b37-a4e5-d1efff3e2024)
![image](https://github.com/user-attachments/assets/057a06da-4771-4088-ab47-abb72749de72)


## Descripción y como se configura los parámetros del proyecto (ej: ip, puertos, conexión a bases de datos, variables de ambiente, parámetros, etc)
Los diferentes parámetros y variables de entorno necesarias se configuraron a través de los archivos yaml que se encuentran alojados en este repositorio. 

## Resultados
![image](https://github.com/user-attachments/assets/d2707bf4-2576-4ad9-9931-b95c0a3b41ba)
![image](https://github.com/user-attachments/assets/e0fb6d66-52d3-439b-a279-70a1fc3630e5)

# Sitio desplegado:
 https://reto2k8s.online

# Referencias:
## https://engr-syedusmanahmad.medium.com/wordpress-on-kubernetes-cluster-step-by-step-guide-749cb53e27c7
## https://aws.amazon.com/es/blogs/storage/running-wordpress-on-amazon-eks-with-amazon-efs-intelligent-tiering/
## https://www.youtube.com/watch?v=DCoBcpOA7W4
