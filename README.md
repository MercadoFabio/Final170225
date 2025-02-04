# **Desarrollo del Juego del Ahorcado con Angular**

## **Objetivo**
Desarrollar una aplicación web en Angular 18 que implemente el clásico juego del Ahorcado, utilizando formularios reactivos, validaciones y comunicación con una API REST para gestionar palabras y puntuaciones.

---

## **Requerimientos Técnicos**
1. Angular CLI 18.x
2. Bootstrap 5.x (para estilos y diseño responsive)
3. JSON Server (para simular la API REST localmente)
4. RxJS (para manejo de observables y peticiones HTTP)

---

## **Estructura del Proyecto**

### **1. Juego del Ahorcado**
#### **1.1 Lógica del Juego**
- El juego debe seleccionar una palabra aleatoria desde la API REST.
- El jugador tiene un máximo de 6 intentos para adivinar la palabra.
- Mostrar una interfaz gráfica que incluya:
    - La palabra oculta (con guiones bajos `_ _ _ _`).
    - Letras incorrectas ya seleccionadas.
    - Letras correctas ya seleccionadas.
    - Un dibujo del ahorcado que se completa con cada error.(Al final se da un ejemplo del dibujo con svg)
    - Un teclado virtual para seleccionar las letras.
- Cuando el jugador adivina la palabra, se muestra un mensaje de victoria y se guarda su puntuación.
- Si el jugador no adivina la palabra, se muestra un mensaje de derrota.

#### **1.2 Puntuaciones**
- Guardar la puntuación del jugador en la API REST (nombre del usuario, palabra adivinada, intentos restantes y fecha).
- Mostrar una tabla de puntuaciones en la pantalla de resultados, ordenada por la cantidad de intentos restantes.
- Si es un player se debera mostrar su puntuación si es un usuario administrador debera ver todas las puntuaciones
---

### **2. Formularios Reactivos**
#### **2.1 Formulario de Inicio de Sesión**
- Campos:
    - Nombre del jugador (requerido, mínimo 3 caracteres).
- Validaciones:
    - Verificar que el nombre no esté vacío.

#### **3 Sistema de autenticación**
- Crear un sistema de autenticación para los usuarios.
- Los usuarios podrán iniciar sesión con su nombre y contraseña que coincida con el usuario del json, si no es asi
  deberá informar con un mensaje de error.
- Los usuarios podrán cerrar sesión en cualquier momento.
- Deberán proteger las rutas de la aplicación para que solo los usuarios autenticados puedan acceder a ellas.

---

### **3. Comunicación con la API REST**
#### **Endpoints de la API**
1. **Palabras**
    - **GET** `/words`: Obtener lista de palabras para el juego.
2. **Puntuaciones**
    - **GET** `/scores`: Obtener lista de puntuaciones.
    - **POST** `/scores`: Guardar una nueva puntuación.
3. **Usuarios**
    - **GET** `/users?username={}&password={}`: Login.
---

### **4. Algoritmo de Procesamiento**
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

## **Tabla de Evaluación**

| Criterio                   | Puntos |
|----------------------------|--------|
| Lógica del Juego           | 40     |
| Formularios Reactivos      | 20     |
| Validaciones de Formulario | 10     |
| Login                      | 20     |
| Estilos                    | 10     |
| **Total**                  | **100**|

---

