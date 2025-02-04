# **Desarrollo del Juego del Ahorcado con Angular**

## **Objetivo**
Desarrollar una aplicación web en Angular 18 que implemente el clásico juego del Ahorcado, utilizando formularios reactivos, validaciones, autenticación de usuarios y comunicación con una API REST para gestionar palabras y puntuaciones. La aplicación deberá ser intuitiva, responsive y seguir buenas prácticas de desarrollo.

---

## **Requerimientos Técnicos**
1. **Angular CLI 18.x**: Para la creación y gestión del proyecto.
2. **Bootstrap 5.x**: Para estilos y diseño responsive.
3. **JSON Server**: Para simular la API REST localmente.
4. **RxJS**: Para manejo de observables y peticiones HTTP.

---

## **Estructura del Proyecto**

### **1. Juego del Ahorcado**
#### **1.1 Lógica del Juego**
- El juego seleccionará una palabra aleatoria desde la API REST.
- El jugador tendrá un máximo de 6 intentos para adivinar la palabra.
- La interfaz gráfica deberá incluir:
    - La palabra oculta representada con guiones bajos (`_ _ _ _`).
    - Letras incorrectas ya seleccionadas.
    - Letras correctas ya seleccionadas.
    - Un dibujo del ahorcado que se completa progresivamente con cada error (ver ejemplo SVG al final).
    - Un teclado virtual para seleccionar las letras.
- Cuando el jugador adivine la palabra, se mostrará un mensaje de victoria y se guardará su puntuación.
- Si el jugador no adivina la palabra, se mostrará un mensaje de derrota.

#### **1.2 Puntuaciones**
- Guardar la puntuación del jugador en la API REST (nombre del usuario, palabra adivinada, intentos restantes y fecha).
- Mostrar una tabla de puntuaciones en la pantalla de resultados, ordenada por la cantidad de intentos restantes.
- **Roles de Usuario**:
    - **player**: Solo podrá ver sus propias puntuaciones.
    - **admin**: Podrá ver todas las puntuaciones.

---

### **2. Formularios Reactivos**
#### **2.1 Formulario de Inicio de Sesión**
- Campos:
    - Nombre de usuario (requerido, mínimo 3 caracteres).
    - Contraseña (requerida, mínimo 6 caracteres).
- Validaciones:
    - Verificar que los campos no estén vacíos.
    - Mostrar mensajes de error claros para cada validación.

---

### **3. Sistema de Autenticación**
- Crear un sistema de autenticación para los usuarios.
- Los usuarios podrán iniciar sesión con su nombre de usuario y contraseña. Si las credenciales no coinciden, se mostrará un mensaje de error.
- Los usuarios podrán cerrar sesión en cualquier momento.
- Proteger las rutas de la aplicación para que solo los usuarios autenticados puedan acceder a ellas.
- **Roles**:
    - **player**: Acceso limitado a las funcionalidades del juego y sus puntuaciones.
    - **admin**: Acceso completo a todas las puntuaciones y gestión de usuarios.

---

### **4. Comunicación con la API REST**
#### **Endpoints de la API**
1. **Palabras**
    - **GET** `/words`: Obtener lista de palabras para el juego.
2. **Puntuaciones**
    - **GET** `/scores`: Obtener lista de puntuaciones.
    - **POST** `/scores`: Guardar una nueva puntuación.
3. **Usuarios**
    - **GET** `/users?username={}&password={}`: Login.
---

### **5. Algoritmo de Procesamiento**
- Generar un identificador único para cada partida.
- Calcular la puntuación basada en la cantidad de intentos restantes:
    - 6 intentos restantes: 100 puntos.
    - 5 intentos restantes: 80 puntos.
    - 4 intentos restantes: 60 puntos.
    - 3 intentos restantes: 40 puntos.
    - 2 intentos restantes: 20 puntos.
    - 1 intento restante: 10 puntos.
    - 0 intentos restantes: 0 puntos.

---

## **Documentación de la API**

### **1. Endpoint de Palabras**
**URL**: `http://localhost:3000/words`  
**Descripción**: Obtiene la lista de palabras para el juego.

#### Formato de Respuesta (GET):
```json
[
  {
    "id": "1",
    "word": "angular",
    "category": "tecnología"
  }
]
```

### **2. Endpoint de Puntuaciones**
**URL**: `http://localhost:3000/scores`  
**Descripción**: Gestiona las puntuaciones de los jugadores.

#### Formato de Respuesta (GET):
```json
[
  {
    "id": "1",
    "playerName": "Juan Pérez",
    "word": "angular",
    "attemptsLeft": 4,
    "score": 60,
    "date": "2024-10-28"
  }
]
```

#### Formato de Solicitud (POST):
```json
{
  "id": "p1JP",
  "playerName": "Juan Pérez",
  "word": "angular",
  "attemptsLeft": 4,
  "score": 60,
  "date": "2024-10-28"
}
```
### **2. Endpoint de Puntuaciones**
**URL**: `http://localhost:3000/users`  
**Descripción**: Gestiona las puntuaciones de los jugadores.

#### Formato de Respuesta (GET):
```json
 [
    {
      "id": 1,
      "username": "admin",
      "password": "admin123",
      "role": "admin"
    }
  ]
```
---

## **Configuración de JSON Server**
1. Instalar JSON Server globalmente:
   ```bash
   npm install -g json-server
   ```


2. Iniciar JSON Server:
   ```bash
   json-server --watch db.json
   ```

---

## **Diseño Gráfico (SVG del Ahorcado)**
```html
<svg width="200" height="300">
    <!-- Base y soporte -->
    <line x1="50" y1="250" x2="150" y2="250" stroke="black" stroke-width="5" /> <!--base-->
    <line x1="100" y1="250" x2="100" y2="50" stroke="black" stroke-width="5" /> <!--soporte-->
    <line x1="100" y1="50" x2="150" y2="50" stroke="black" stroke-width="5" /> <!--travesaño-->
    <line x1="150" y1="50" x2="150" y2="100" stroke="black" stroke-width="5" /> <!--cuerda-->

    <!-- Dibujo del personaje -->
    <circle cx="150" cy="120" r="20" fill="black" /> <!--cabeza--->
    <line x1="150" y1="140" x2="150" y2="200" stroke="black" /> <!--cuerpo-->
    <line x1="150" y1="150" x2="130" y2="180" stroke="black"/> <!--brazo izquierdo-->
    <line x1="150" y1="150" x2="170" y2="180" stroke="black"  /> <!--brazo derecho-->
    <line x1="150" y1="200" x2="130" y2="240" stroke="black"  /> <!--pierna izquierda-->
    <line x1="150" y1="200" x2="170" y2="240" stroke="black"  /> <!--pierna derecha-->
</svg>
```

---

## **Mockup**
En la carpeta `assets` encontrarán las imágenes con los estilos de la aplicación.

---

## **Tabla de Evaluación**

| Criterio                   | Puntos |
|----------------------------|--------|
| Lógica del Juego           | 40     |
| Formularios Reactivos      | 20     |
| Validaciones de Formulario | 10     |
| Sistema de Autenticación   | 20     |
| Estilos y Diseño Responsive| 10     |
| **Total**                  | **100**|

---
