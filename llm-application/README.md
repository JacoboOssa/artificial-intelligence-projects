# Agente de Respuesta de Correos para Recuperación de Seguros

## Contexto del Problema

La recuperación de seguros es un proceso complejo que requiere un seguimiento constante de las comunicaciones entre las compañías de seguros y los bufetes de abogados. Cuando se envía una reclamación para solicitar el recobro de un seguro, es común que las empresas o personas no respondan favorablemente a estas solicitudes de reembolso.

### Problemática Actual

- **Proceso Manual Ineficiente**: Los abogados deben revisar constantemente las respuestas de correos electrónicos para determinar si las compañías de seguros aceptan o rechazan las reclamaciones.
- **Alta Carga de Trabajo**: El volumen de correos y la necesidad de interpretación manual consume tiempo valioso del personal legal.
- **Decisiones Críticas**: Determinar si una respuesta es positiva o negativa es crucial para decidir los siguientes pasos en el proceso legal.

### Solución Propuesta

Este agente automatizado utiliza inteligencia artificial para:

1. **Monitoreo Automático**: Ingresa a las cuentas de correo de los abogados para revisar las respuestas recibidas.
2. **Análisis Inteligente**: Utiliza el modelo DeepSeek-R1 para interpretar si las respuestas son positivas o negativas.
3. **Automatización de Procesos**: En caso de respuestas negativas, inicia automáticamente el proceso de llenado de documentos para envío a otras áreas de la firma.

## Arquitectura del Sistema

### Componentes Principales

- **n8n**: Plataforma de automatización que orquesta todo el flujo de trabajo
- **DeepSeek-R1 14B**: Modelo de lenguaje local que analiza las respuestas de correos
- **ngrok**: Túnel que expone el modelo local para que n8n pueda acceder a él
- **BTL App**: Aplicación backend que maneja los casos y la autenticación

### Flujo de Trabajo

1. **Autenticación**: El workflow se autentica con la aplicación BTL
2. **Obtención de Casos**: Recupera los casos pendientes de revisión
3. **Análisis de Correos**: Para cada caso, analiza las respuestas de correo utilizando DeepSeek-R1
4. **Toma de Decisiones**: Basado en el análisis, determina las acciones a seguir
5. **Automatización de Procesos**: Ejecuta automáticamente los pasos correspondientes

## Integración con Modelo LLM Local

### Exposición del Modelo

El modelo DeepSeek-R1 14B se expone localmente en el puerto 11434 y se hace accesible públicamente usando ngrok:

```bash
ngrok http 11434
```

### Conexión desde n8n

Un nodo HTTP Request en n8n hace una petición al modelo expuesto para analizar los correos de seguros y determinar si las respuestas son positivas o negativas.
```

## Casos de Uso

### Escenario 1: Respuesta Positiva
- **Input**: Correo que acepta la reclamación
- **Análisis**: DeepSeek-R1 identifica respuesta como POSITIVA
- **Acción**: Continuar con el proceso de recobro normal

### Escenario 2: Respuesta Negativa
- **Input**: Correo que rechaza la reclamación
- **Análisis**: DeepSeek-R1 identifica respuesta como NEGATIVA
- **Acción**: Activar proceso de llenado de documentos para derivar a otras áreas