# **Plan de Proyecto: App "Recordable" (React Native \+ Expo)**

## **1\. Introducción**

Este documento redefine el plan de desarrollo para la aplicación "Recordable", cambiando el enfoque tecnológico de Kotlin Multiplatform (KMP) a **React Native con el framework Expo**. El objetivo sigue siendo crear una aplicación móvil (iOS y Android) que permita a los usuarios grabar o importar notas de voz, transcribirlas usando IA (Whisper) y generar informes estructurados basados en la transcripción y prompts del usuario usando IA (Gemini).

Se prioriza la **simplicidad**, el **bajo coste operativo**, la **automatización** y la utilización del ecosistema **React Native/Expo**. Este plan servirá como base para generar prompts detallados para guiar el desarrollo.

## **2\. Casos de Uso Principales**

A continuación, se describen las interacciones clave del usuario con la aplicación:

* **CU-01: Grabar Nueva Nota de Voz:**  
  * El usuario inicia la grabación de audio directamente desde la aplicación.  
  * El usuario detiene la grabación.  
  * La aplicación guarda el archivo de audio localmente y lo asocia con metadatos básicos (ej. fecha, hora, título editable).  
* **CU-02: Importar Archivo de Audio:**  
  * El usuario selecciona un archivo de audio existente desde el almacenamiento de su dispositivo.  
  * La aplicación copia o referencia el archivo y lo asocia con metadatos básicos.  
* **CU-03: Ver Lista de Notas de Voz:**  
  * El usuario visualiza una lista de todas las notas de voz grabadas o importadas, mostrando información relevante (título, fecha, estado de transcripción/informe).  
* **CU-04: Ver Detalles de Nota de Voz:**  
  * El usuario selecciona una nota de voz de la lista.  
  * La aplicación muestra los detalles de la nota, incluyendo controles de reproducción de audio.  
* **CU-05: Solicitar Transcripción:**  
  * Desde la pantalla de detalles, el usuario inicia el proceso de transcripción para la nota de voz seleccionada.  
  * La aplicación envía el audio a un servicio backend para su procesamiento con Whisper.  
  * La aplicación indica el estado del proceso (pendiente, procesando, completado, error).  
* **CU-06: Ver Transcripción:**  
  * Una vez completada, el usuario visualiza el texto de la transcripción en la pantalla de detalles.  
* **CU-07: Solicitar Generación de Informe:**  
  * Con una transcripción disponible, el usuario introduce un prompt de texto describiendo el tipo de informe deseado.  
  * El usuario inicia el proceso de generación del informe.  
  * La aplicación envía la transcripción y el prompt a un servicio backend para su procesamiento con Gemini.  
  * La aplicación indica el estado del proceso (pendiente, procesando, completado, error).  
* **CU-08: Ver Informe Generado:**  
  * Una vez completado, el usuario visualiza el informe generado en la pantalla de detalles.  
* **CU-09: Gestionar Notas de Voz:**  
  * El usuario puede editar el título de una nota de voz.  
  * El usuario puede eliminar una nota de voz (incluyendo audio, transcripción e informe asociados).  
* **(Opcional) CU-10: Autenticación de Usuario:**  
  * El usuario se registra o inicia sesión para sincronizar sus notas entre dispositivos (requiere backend con base de datos).

## **3\. Requisitos del Software**

### **3.1. Requisitos Funcionales (RF)**

* **RF-01:** La aplicación debe permitir grabar audio usando el micrófono del dispositivo. (Ref: CU-01)  
* **RF-02:** La aplicación debe permitir importar archivos de audio en formatos comunes (ej. M4A, MP3, WAV) desde el almacenamiento local. (Ref: CU-02)  
* **RF-03:** La aplicación debe almacenar localmente los archivos de audio grabados/importados y sus metadatos. (Ref: CU-01, CU-02)  
* **RF-04:** La aplicación debe mostrar una lista navegable de las notas de voz almacenadas. (Ref: CU-03)  
* **RF-05:** La aplicación debe permitir la reproducción del audio de las notas de voz. (Ref: CU-04)  
* **RF-06:** La aplicación debe permitir iniciar la transcripción de una nota de voz seleccionada. (Ref: CU-05)  
* **RF-07:** La aplicación debe interactuar con un servicio backend seguro para realizar la transcripción utilizando la API de Whisper. (Ref: CU-05)  
* **RF-08:** La aplicación debe mostrar el estado del proceso de transcripción. (Ref: CU-05)  
* **RF-09:** La aplicación debe mostrar el texto de la transcripción una vez completada. (Ref: CU-06)  
* **RF-10:** La aplicación debe permitir al usuario introducir un prompt de texto. (Ref: CU-07)  
* **RF-11:** La aplicación debe permitir iniciar la generación de un informe basado en la transcripción y el prompt. (Ref: CU-07)  
* **RF-12:** La aplicación debe interactuar con un servicio backend seguro para generar el informe utilizando la API de Gemini. (Ref: CU-07)  
* **RF-13:** La aplicación debe mostrar el estado del proceso de generación de informe. (Ref: CU-07)  
* **RF-14:** La aplicación debe mostrar el texto del informe generado. (Ref: CU-08)  
* **RF-15:** La aplicación debe permitir editar el título de las notas de voz. (Ref: CU-09)  
* **RF-16:** La aplicación debe permitir eliminar notas de voz y sus datos asociados. (Ref: CU-09)  
* **(Opcional) RF-17:** La aplicación debe permitir el registro e inicio de sesión de usuarios. (Ref: CU-10)  
* **(Opcional) RF-18:** La aplicación debe sincronizar las notas de voz del usuario autenticado con un backend. (Ref: CU-10)

### **3.2. Requisitos No Funcionales (RNF)**

* **RNF-01: Plataforma:** La aplicación debe funcionar en iOS y Android, desarrollada usando React Native con Expo.  
* **RNF-02: Framework UI:** Se utilizarán componentes nativos de React Native y/o bibliotecas de componentes compatibles con Expo (ej. React Native Elements, React Native Paper, NativeWind para Tailwind).  
* **RNF-03: Gestión de Audio:** Se utilizará la API expo-av para grabación y reproducción de audio.  
* **RNF-04: Selección de Archivos:** Se utilizará la API expo-document-picker para la importación de archivos.  
* **RNF-05: Almacenamiento Local:** Se utilizará expo-file-system para gestionar archivos de audio locales y AsyncStorage (o una alternativa como MMKV) para metadatos/preferencias.  
* **RNF-06: Backend:** Se requiere un servicio backend (ej. Firebase Functions, Supabase Edge Functions, AWS Lambda, Google Cloud Functions) para:  
  * Gestionar de forma segura las claves API de Whisper y Gemini.  
  * Orquestar las llamadas a las APIs de Whisper y Gemini.  
  * (Opcional) Gestionar la autenticación y la base de datos (Firestore, Supabase DB, etc.) si se implementa CU-10.  
* **RNF-07: Comunicación Frontend-Backend:** Se utilizarán llamadas HTTPS (fetch API o librerías como Axios) desde la app Expo al backend.  
* **RNF-08: Gestión de Estado:** Se seleccionará una solución de gestión de estado adecuada para React Native (ej. React Context API, Zustand, Redux Toolkit).  
* **RNF-09: Navegación:** Se utilizará react-navigation.  
* **RNF-10: Seguridad:** Las claves API de servicios externos (Whisper, Gemini) NUNCA deben almacenarse en el código fuente de la aplicación cliente. Deben residir y ser utilizadas únicamente en el backend. La comunicación con el backend debe ser sobre HTTPS.  
* **RNF-11: Coste:** Se priorizarán soluciones de bajo coste tanto en el desarrollo (uso de Expo y librerías open source) como en la operación (elección de servicios backend y APIs con modelos de precios adecuados, preferiblemente pago por uso).  
* **RNF-12: Rendimiento:** La interfaz de usuario debe ser fluida. Las operaciones de larga duración (subida de audio, transcripción, generación de informe) deben ejecutarse en segundo plano sin bloquear la UI y mostrando indicadores de progreso claros.  
* **RNF-13: Mantenibilidad:** El código debe seguir buenas prácticas de React Native/JavaScript/TypeScript, ser modular y estar bien comentado.

## **4\. Plan de Implementación Detallado (React Native \+ Expo)**

Este plan se divide en fases lógicas para un desarrollo incremental.

### **Fase 1: Configuración del Proyecto y Navegación Básica**

* **Tareas:**  
  * Inicializar un nuevo proyecto Expo (npx create-expo-app). Preferiblemente con TypeScript.  
  * Configurar la estructura básica de directorios (screens, components, navigation, services, hooks, store, etc.).  
  * Instalar y configurar react-navigation (Stack Navigator).  
  * Crear pantallas placeholder básicas (ej. NoteListScreen, NoteDetailScreen).  
  * Implementar la navegación inicial entre la lista y la pantalla de detalle.  
  * (Opcional) Configurar una biblioteca de componentes UI (ej. React Native Paper) o Tailwind CSS (NativeWind).  
* **Prompts Ejemplo:**  
  * "Inicializa un proyecto Expo con TypeScript llamado 'RecordableApp'."  
  * "Configura React Navigation con un Stack Navigator que incluya 'NoteListScreen' y 'NoteDetailScreen'."  
  * "Crea un componente funcional básico para 'NoteListScreen' que muestre un título y un botón para navegar a 'NoteDetailScreen'."

### **Fase 2: Grabación e Importación de Audio**

* **Tareas:**  
  * Instalar expo-av y expo-document-picker.  
  * Implementar la lógica de grabación de audio en NoteDetailScreen usando expo-av:  
    * Solicitar permisos de micrófono.  
    * Iniciar/detener grabación.  
    * Guardar el archivo grabado usando expo-file-system en un directorio local de la app.  
    * Gestionar el estado de grabación (grabando/parado).  
  * Implementar la lógica de importación de audio:  
    * Añadir un botón para abrir el selector de documentos (expo-document-picker).  
    * Copiar el archivo seleccionado al directorio local de la app usando expo-file-system.  
  * Implementar la lógica de reproducción del audio almacenado usando expo-av.  
  * Almacenar metadatos básicos de la nota (ruta del archivo, título, fecha) usando AsyncStorage o MMKV.  
  * Mostrar la lista de notas en NoteListScreen leyendo los metadatos almacenados.  
* **Prompts Ejemplo:**  
  * "Implementa la funcionalidad de grabación de audio en 'NoteDetailScreen' usando expo-av. Solicita permisos, permite iniciar/detener, guarda el archivo localmente y actualiza el estado."  
  * "Añade un botón en 'NoteDetailScreen' que use expo-document-picker para seleccionar un archivo de audio y lo copie al almacenamiento local de la app."  
  * "Implementa la reproducción de audio en 'NoteDetailScreen' para un archivo local usando expo-av."  
  * "Usa AsyncStorage para guardar y leer una lista de objetos que representen las notas de voz (id, title, filePath, createdAt)."

### **Fase 3: Configuración del Backend (Ej. Firebase Functions)**

* **Tareas:**  
  * Configurar un proyecto backend (ej. Firebase con Functions, Supabase, etc.).  
  * Definir dos funciones HTTP/endpoints en el backend:  
    * transcribeAudio: Recibe un archivo de audio (o una URL a él si se sube a un storage), llama a la API de Whisper de forma segura y devuelve la transcripción (o un ID de trabajo).  
    * generateReport: Recibe un texto (transcripción) y un prompt, llama a la API de Gemini de forma segura y devuelve el informe generado.  
  * Configurar el almacenamiento seguro de las claves API de Whisper y Gemini en el entorno del backend (variables de entorno).  
  * Implementar la lógica básica de las funciones para recibir datos y devolver respuestas placeholder.  
  * Desplegar el backend inicial.  
* **Prompts Ejemplo (para Firebase Functions):**  
  * "Crea una Firebase Function HTTP (Node.js/TypeScript) llamada 'transcribeAudio' que acepte una solicitud POST con datos de archivo."  
  * "Crea una Firebase Function HTTP llamada 'generateReport' que acepte una solicitud POST con JSON conteniendo 'transcription' y 'prompt'."  
  * "Configura variables de entorno en Firebase Functions para 'WHISPER\_API\_KEY' y 'GEMINI\_API\_KEY'."

### **Fase 4: Integración de Whisper (Frontend \+ Backend)**

* **Tareas:**  
  * **Frontend:**  
    * En NoteDetailScreen, añadir un botón "Transcribir".  
    * Al pulsar, leer el archivo de audio local.  
    * Enviar el archivo de audio a la función backend transcribeAudio (usando fetch o Axios, probablemente como FormData).  
    * Gestionar el estado de carga (mostrando un indicador).  
    * Recibir la transcripción (o ID de trabajo) del backend.  
    * Almacenar la transcripción asociada a la nota (AsyncStorage/MMKV).  
    * Mostrar la transcripción en la UI.  
    * Manejar errores de red o del backend.  
  * **Backend (transcribeAudio):**  
    * Recibir el archivo de audio.  
    * Llamar a la API de Whisper usando la clave API almacenada.  
    * Manejar la respuesta de Whisper.  
    * Devolver la transcripción al frontend.  
    * Implementar manejo de errores.  
* **Prompts Ejemplo:**  
  * "En 'NoteDetailScreen', implementa una función que envíe el archivo de audio local a la URL de la función 'transcribeAudio' usando fetch y FormData."  
  * "Actualiza la función backend 'transcribeAudio' para que reciba el archivo, llame a la API de OpenAI Whisper (usando axios o node-fetch) con la clave API de las variables de entorno, y devuelva la transcripción."

### **Fase 5: Integración de Gemini (Frontend \+ Backend)**

* **Tareas:**  
  * **Frontend:**  
    * En NoteDetailScreen, añadir un TextInput para el prompt y un botón "Generar Informe".  
    * Habilitar el botón solo si hay una transcripción disponible.  
    * Al pulsar, enviar la transcripción y el prompt del usuario a la función backend generateReport (como JSON).  
    * Gestionar el estado de carga.  
    * Recibir el informe generado del backend.  
    * Almacenar el informe asociado a la nota.  
    * Mostrar el informe en la UI.  
    * Manejar errores.  
  * **Backend (generateReport):**  
    * Recibir la transcripción y el prompt.  
    * Llamar a la API de Google Gemini usando la clave API almacenada.  
    * Manejar la respuesta de Gemini.  
    * Devolver el texto del informe al frontend.  
    * Implementar manejo de errores.  
* **Prompts Ejemplo:**  
  * "Añade un TextInput y un botón en 'NoteDetailScreen'. Al pulsar el botón, envía la transcripción almacenada y el texto del TextInput a la función 'generateReport' como JSON."  
  * "Actualiza la función backend 'generateReport' para que reciba la transcripción y el prompt, llame a la API de Google Gemini (usando la librería cliente de Google o fetch) con la clave API, y devuelva el texto generado."

### **Fase 6: Gestión de Estado y Refinamiento UI**

* **Tareas:**  
  * Implementar o refinar la solución de gestión de estado elegida (Context, Zustand, Redux) para manejar de forma centralizada los datos de las notas, transcripciones, informes y estados de carga/error.  
  * Refactorizar los componentes para leer/escribir del estado centralizado en lugar de pasar props excesivamente o usar estado local complejo.  
  * Mejorar la UI/UX:  
    * Añadir indicadores de carga claros para todas las operaciones asíncronas.  
    * Mostrar mensajes de error descriptivos.  
    * Implementar la edición de títulos y la eliminación de notas.  
    * Asegurar un diseño consistente y agradable.  
    * Optimizar el rendimiento de la lista (FlatList).  
* **Prompts Ejemplo:**  
  * "Configura Zustand (o React Context) para gestionar el estado global de la aplicación, incluyendo la lista de notas, la nota seleccionada, y los estados de carga/error para transcripción e informes."  
  * "Refactoriza 'NoteListScreen' y 'NoteDetailScreen' para usar el store de Zustand."  
  * "Implementa la funcionalidad para eliminar una nota de la lista y de AsyncStorage/MMKV en 'NoteListScreen'."

### **Fase 7: Pruebas y Despliegue**

* **Tareas:**  
  * Realizar pruebas manuales exhaustivas en dispositivos iOS y Android (o simuladores/emuladores).  
  * Escribir pruebas unitarias para lógica crítica (si aplica, usando Jest).  
  * Escribir pruebas de componentes/integración (usando React Native Testing Library).  
  * Optimizar el rendimiento y el uso de memoria.  
  * Preparar la aplicación para el despliegue (iconos, splash screen, configuración de build).  
  * Construir la aplicación para producción usando EAS Build (Expo Application Services).  
  * (Opcional) Desplegar en tiendas de aplicaciones (App Store, Google Play) o mediante distribución ad-hoc.  
* **Prompts Ejemplo:**  
  * "Configura Jest y React Native Testing Library en el proyecto Expo."  
  * "Escribe una prueba unitaria para una función de utilidad que formatea fechas."  
  * "Escribe una prueba de componente para 'NoteListScreen' que verifique que la lista se renderiza correctamente con datos mock."  
  * "Configura EAS Build en el proyecto y crea un build de desarrollo para Android."

## **5\. Conclusión y Próximos Pasos**

Este plan proporciona una hoja de ruta detallada para desarrollar la aplicación "Recordable" usando React Native y Expo. Se enfoca en la integración de servicios de IA (Whisper, Gemini) a través de un backend seguro y prioriza la simplicidad y el bajo coste.

Los próximos pasos consisten en utilizar las tareas y ejemplos de prompts de cada fase para guiar el desarrollo, ya sea de forma manual o asistida por herramientas de IA, asegurando seguir las mejores prácticas del ecosistema React Native/Expo.