# PruebaChoreo - WSO2 Choreo Proxy Service

## Descripción General

Este es un proyecto de prueba para la plataforma en la nube **WSO2 Choreo**, que implementa un servicio proxy para la API de Worldtime de API Ninjas. El proyecto está desarrollado usando WSO2 Micro Integrator y está diseñado para ser desplegado en la plataforma Choreo.

## Características del Proyecto

- **Plataforma**: WSO2 Choreo (Cloud)
- **Tecnología**: WSO2 Micro Integrator
- **Tipo**: Servicio Proxy/API Gateway
- **API Externa**: [API Ninjas Worldtime](https://api-ninjas.com/api/worldtime)
- **Método de Desarrollo**: Visual Studio Code con extensión WSO2 MI

## Arquitectura

El proyecto implementa un patrón de proxy service que:

1. **Recibe solicitudes** en el endpoint `/v1/worldtime?city={city}`
2. **Procesa parámetros** de la consulta (nombre de ciudad)
3. **Enruta solicitudes** a la API externa de API Ninjas
4. **Retorna respuestas** en formato JSON con información de zona horaria

### Componentes Principales

#### 1. API Definition (`APIWorldtime.xml`)
- **Contexto**: `/v1`
- **Método**: GET
- **URI Template**: `/worldtime?city={city}`
- **Funcionalidades**:
  - Logging de transacciones
  - Configuración de headers HTTP
  - Manejo de parámetros de consulta
  - Autenticación con API Key

#### 2. Endpoint Configuration (`WorldtimeEndpoint.xml`)
- **Nombre**: WorldtimeEndpoint
- **Método**: GET
- **URI Template**: Dinámico basado en parámetros
- **Características**:
  - Estadísticas habilitadas
  - Trazabilidad habilitada
  - Configuración de suspensión en fallos

#### 3. OpenAPI Specification (`APIWorldtime.yaml`)
- **Versión**: OpenAPI 3.0.1
- **Servidor**: http://localhost:8290/v1
- **Parámetros**: city (query parameter)

## Requisitos Previos

### Herramientas de Desarrollo
- **Visual Studio Code**
- **Extensión WSO2 MI para VS Code**: [Instalar desde aquí](https://mi.docs.wso2.com/en/latest/develop/mi-for-vscode/install-wso2-mi-for-vscode/)
- **Java 11** o superior
- **Maven 3.6** o superior

### Servicios Externos
- **API Key de API Ninjas**: Registro requerido en [api-ninjas.com](https://api-ninjas.com/)

## Estructura del Proyecto

```
PruebaChoreo/
├── src/main/wso2mi/
│   ├── artifacts/
│   │   ├── apis/
│   │   │   └── APIWorldtime.xml          # Definición de la API
│   │   └── endpoints/
│   │       └── WorldtimeEndpoint.xml     # Configuración del endpoint
│   └── resources/
│       ├── api-definitions/
│       │   └── APIWorldtime.yaml         # Especificación OpenAPI
│       └── metadata/
│           └── APIWorldtime_metadata.yaml # Metadatos de la API
├── deployment/
│   ├── deployment.toml                   # Configuración de despliegue
│   └── docker/
│       └── Dockerfile                    # Imagen Docker
├── pom.xml                              # Configuración Maven
└── README.md                            # Documentación del proyecto
```

## Configuración

### 1. Configuración de API Key

Edita el archivo `APIWorldtime.xml` y actualiza el valor de la API Key:

```xml
<property name="X-Api-Key" scope="transport" type="STRING" value="TU_API_KEY_AQUI"/>
```

### 2. Configuración del Servidor

El archivo `deployment/deployment.toml` contiene la configuración del servidor:

- **Hostname**: localhost
- **Keystore**: wso2carbon.jks
- **Truststore**: client-truststore.jks

## Instalación y Ejecución

### 1. Clonar el Repositorio

```bash
git clone <repository-url>
cd PruebaChoreo
```

### 2. Compilar el Proyecto

```bash
mvn clean install
```

### 3. Ejecutar Localmente

Usando VS Code con la extensión WSO2 MI:

1. Abrir el proyecto en VS Code
2. Usar la extensión WSO2 MI para ejecutar el servidor
3. El servidor estará disponible en `http://localhost:8290`

### 4. Desplegar en Choreo

1. Subir el proyecto a un repositorio Git
2. Conectar el repositorio a WSO2 Choreo
3. Configurar las variables de entorno necesarias
4. Desplegar el servicio

## Uso de la API

### Endpoint Principal

```
GET /v1/worldtime?city={city_name}
```

### Ejemplo de Solicitud

```bash
curl -X GET "http://localhost:8290/v1/worldtime?city=London"
```

### Ejemplo de Respuesta

```json
{
  "timezone": "Europe/London",
  "datetime": "2025-06-25T10:30:00.000000+01:00",
  "day_of_week": 3,
  "day_of_year": 176,
  "is_dst": true
}
```

### Parámetros

| Parámetro | Tipo | Descripción | Ejemplo |
|-----------|------|-------------|---------|
| city | string | Nombre de la ciudad | London, Paris, Tokyo |

## Logging y Monitoreo

El proyecto incluye logging comprehensivo:

- **Inicio de transacción**: Log al recibir solicitud
- **Antes de llamada externa**: Log antes de invocar API Ninjas
- **Después de llamada externa**: Log después de recibir respuesta
- **Payloads completos**: Habilitado para debugging

## Seguridad

- **Autenticación**: API Key para servicios externos
- **HTTPS**: Soporte configurado
- **Keystores**: Configuración de certificados incluida

## Desarrollo

### Extensión de VS Code

Este proyecto utiliza la [extensión WSO2 MI para Visual Studio Code](https://mi.docs.wso2.com/en/latest/develop/mi-for-vscode/install-wso2-mi-for-vscode/) que proporciona:

- **Autocompletado** para sintaxis de Synapse
- **Debugging** integrado
- **Despliegue directo** a servidores MI
- **Validación** de configuraciones

### Mejores Prácticas Implementadas

1. **Separación de responsabilidades**: API y Endpoint separados
2. **Configuración externa**: Parámetros configurables
3. **Logging comprehensivo**: Para debugging y monitoreo
4. **Manejo de errores**: Secuencias de fallo configuradas
5. **Documentación**: OpenAPI specification incluida

## Contribución

Para contribuir al proyecto:

1. Fork el repositorio
2. Crear una rama para tu feature
3. Realizar los cambios
4. Crear un Pull Request

## Soporte

Para soporte técnico:

- **WSO2 Documentation**: [docs.wso2.com](https://docs.wso2.com/)
- **API Ninjas**: [api-ninjas.com](https://api-ninjas.com/)
- **Choreo Platform**: [wso2.com/choreo](https://wso2.com/choreo/)

## Licencia

Este proyecto es una prueba de concepto y está disponible para fines educativos y de demostración.

---

*Proyecto desarrollado con WSO2 Micro Integrator para la plataforma Choreo*
