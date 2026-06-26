# Tienda Perritos - Frontend 🐶

**Curso:** INTRODUCCION A HERRAMIENTAS DEVOPS_801D  
**Integrantes:** Lucas Alcaino - Diego Miranda  
**Asignatura:** ISY1101  

## Descripción
Frontend de la Tienda de Alimentos para Perritos. Aplicación web simple que permite gestionar productos mediante un CRUD completo. Desarrollada con HTML, CSS y JavaScript vanilla, servida mediante Nginx.

## Tecnologías
- HTML5 / CSS3 / JavaScript
- Nginx (servidor web y proxy inverso)
- Docker
- Amazon ECS (Fargate)
- Amazon ECR
- GitHub Actions (CI/CD)

## Arquitectura
El frontend es servido por Nginx en el puerto 80. Las llamadas a `/api/` son redirigidas mediante proxy inverso hacia el backend en el puerto 3001.
Usuario → http://3.91.188.1 → Nginx (puerto 80) → Backend (puerto 3001)

## Variables de entorno
No requiere variables de entorno. La URL del backend está configurada en `nginx.conf`.

## nginx.conf
El archivo `nginx.conf` configura el proxy inverso hacia el backend:
```nginx
location /api/ {
    proxy_pass http://18.206.204.46:3001;
}
```

## Pipeline CI/CD
El pipeline está definido en `.github/workflows/deploy.yml` y se ejecuta automáticamente en cada push a `main`.

### Pasos del pipeline:
1. **Checkout** del código fuente
2. **Configuración** de credenciales AWS
3. **Login** a Amazon ECR
4. **Build y Push** de la imagen Docker a ECR
5. **Deploy** automático al servicio ECS `frontend`

## Cómo ejecutar localmente
```bash
docker build -t tienda-perritos-frontend .
docker run -p 80:80 tienda-perritos-frontend
```
Luego abre http://localhost en tu navegador.

## Estructura del proyecto
frontend/

├── index.html          # Página principal

├── app.js              # Lógica CRUD

├── nginx.conf          # Configuración proxy inverso

├── Dockerfile          # Imagen Docker

└── .github/

└── workflows/

└── deploy.yml  # Pipeline CI/CD

## Commits
Cada commit sigue el formato:
- `feat:` para nuevas funcionalidades
- `fix:` para correcciones
- `update:` para actualizaciones