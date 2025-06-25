# ProyectoAtencionHospital

Este proyecto corresponde a la Nota N° 3 de la asignatura **Patrones de Diseño**.  
Implementa un **sistema de turnos para un hospital** con cinco niveles de urgencia médica (I a V) y pantallas dedicadas por nivel, utilizando cuatro patrones GoF.

---

## 🩺 Problema a resolver

En el hospital de **San Vicente de Tagua Tagua** la llamada de turnos se hace todavía “a viva voz” y sin registro digital.  
Esto provoca:

- **Largas esperas** y filas improvisadas.  
- Pacientes que **discuten** porque creen que les saltaron el turno.  
- Personal que pierde tiempo verificando listas en papel.  
- Ambiente tenso y poca claridad sobre los niveles de urgencia.

Este sistema busca digitalizar la atención, mostrar claramente en qué nivel está cada paciente, y ordenar la cola de espera para reducir conflictos y optimizar los recursos.

---

## 🧠 Descripción del sistema

- Se registran pacientes con su **nivel de urgencia** (I a V).  
- Al iniciar, las **cinco pantallas** se muestran vacías.  
- Los pacientes quedan en una cola interna hasta que se les llame.  
- Al llamar (por nombre o automáticamente), su nombre aparece **solo** en la pantalla del nivel correspondiente.  

---

## 🧠 Justificación de los Patrones de Diseño

### 1. 🏭 Factory Method (Creacional)

- **Por qué**:  
  Evita acoplar el código cliente a la creación directa de pantallas. Si mañana se quisiera mostrar la pantalla de otra forma (por ejemplo, en una interfaz gráfica o web), solo se cambia la fábrica.

- **Cómo**:  
  La clase `DisplayFactory` centraliza la creación de pantallas, exponiendo un método estático `createLevelDisplay(int level)` que encapsula el `new LevelDisplay()`.

- **Dónde**:  
  - Clase: `ui/DisplayFactory.java`  
  - Usado en: `App.java`, `HospitalFacade.java`

---

### 2. 🧩 Facade (Estructural)

- **Por qué**:  
  Simplifica el uso del sistema para quien lo quiera integrar. No es necesario saber cómo funcionan las colas, las pantallas ni los observadores, solo se llama a métodos simples como `ingresarPaciente()` o `atenderSiguiente()`.

- **Cómo**:  
  La clase `HospitalFacade` expone métodos de alto nivel y oculta la complejidad de los subsistemas como el manejador de colas, displays y lógica de llamada.

- **Dónde**:  
  - Clase: `facade/HospitalFacade.java`  
  - Métodos: `start()`, `ingresarPaciente()`, `atenderSiguiente()`

---

### 3. 👁️ Observer (Comportamiento)

- **Por qué**:  
  Permite que cada pantalla se actualice automáticamente solo cuando le corresponde, sin necesidad de consultarlas constantemente ni escribir código adicional para cada actualización.

- **Cómo**:  
  `QueueManager` extiende `Observable`, y cada `LevelDisplay` se suscribe como `Observer`. Al llamar a un paciente, el `QueueManager` notifica solo a la pantalla correspondiente.

- **Dónde**:  
  - Sujeto: `queue/QueueManager.java`  
  - Observador: `ui/LevelDisplay.java`  
  - Se vinculan en: `App.java`, `HospitalFacade.java`

---

### 4. 🧠 Strategy (Comportamiento – libre elección)

- **Por qué**:  
  Permite cambiar dinámicamente la forma en que se elige al siguiente paciente a ser atendido. Por ejemplo, se puede usar por prioridad (urgencia), por orden de llegada (FIFO), o con lógica especial (niños, adultos mayores).

- **Cómo**:  
  `QueueManager` utiliza una interfaz `NextPatientStrategy` que define cómo se selecciona el siguiente paciente. Actualmente se usa `HighestPriorityStrategy`, pero se podrían implementar otras sin tocar el resto del sistema.

- **Dónde**:  
  - Interfaz: `queue.strategy.NextPatientStrategy.java`  
  - Implementación: `HighestPriorityStrategy.java`  
  - Usado en: `QueueManager`

---

## 🧪 Instrucciones de compilación y ejecución


