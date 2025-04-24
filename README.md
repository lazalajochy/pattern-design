# pattern-design

---

### **1. DRY (Don't Repeat Yourself)**

El principio **DRY** sugiere evitar la duplicación de código. Si encuentras que estás repitiendo la misma lógica, abstrae esa lógica en una función reutilizable.

#### **Ejemplo sin DRY (código repetido):**
```javascript
function calculateAreaOfSquare(side) {
  return side * side;
}

function calculateAreaOfRectangle(length, width) {
  return length * width;
}

function calculateAreaOfCircle(radius) {
  return Math.PI * radius * radius;
}
```

#### **Ejemplo con DRY (código reutilizable):**
```javascript
function calculateArea(shape, ...dimensions) {
  switch (shape) {
    case "square":
      return dimensions[0] * dimensions[0];
    case "rectangle":
      return dimensions[0] * dimensions[1];
    case "circle":
      return Math.PI * dimensions[0] * dimensions[0];
    default:
      throw new Error("Shape not supported");
  }
}

console.log(calculateArea("square", 4)); // 16
console.log(calculateArea("rectangle", 4, 5)); // 20
console.log(calculateArea("circle", 3)); // 28.27
```

---

### **2. KISS (Keep It Simple, Stupid)**

El principio **KISS** sugiere que el código debe ser lo más simple posible. Evita complejidad innecesaria y escribe código fácil de entender.

#### **Ejemplo sin KISS (código innecesariamente complejo):**
```javascript
function isEven(number) {
  if (number % 2 === 0) {
    return true;
  } else {
    return false;
  }
}
```

#### **Ejemplo con KISS (código simple y claro):**
```javascript
function isEven(number) {
  return number % 2 === 0;
}

console.log(isEven(4)); // true
console.log(isEven(5)); // false
```

---

### **3. YAGNI (You Aren't Gonna Need It)**

El principio **YAGNI** sugiere que no debes implementar funcionalidades que no necesitas en este momento. Solo escribe el código necesario para cumplir con los requisitos actuales.

#### **Ejemplo sin YAGNI (funcionalidad innecesaria):**
```javascript
function fetchData(url, options = {}) {
  // Código para manejar múltiples tipos de autenticación, aunque no se necesita ahora.
  if (options.authType === "OAuth") {
    console.log("Handling OAuth...");
  } else if (options.authType === "APIKey") {
    console.log("Handling API Key...");
  }

  // Código para manejar múltiples formatos de respuesta, aunque no se necesita ahora.
  if (options.responseType === "XML") {
    console.log("Handling XML response...");
  }

  console.log(`Fetching data from ${url}`);
}
```

#### **Ejemplo con YAGNI (solo lo necesario):**
```javascript
function fetchData(url) {
  console.log(`Fetching data from ${url}`);
}

fetchData("https://api.example.com/data");
// Salida: Fetching data from https://api.example.com/data
```

---

### **Resumen**
- **DRY**: Evita duplicar código. Usa funciones reutilizables.
- **KISS**: Mantén el código simple y fácil de entender.
- **YAGNI**: No implementes funcionalidades innecesarias hasta que realmente las necesites.

Estos principios ayudan a mantener el código limpio, eficiente y fácil de mantener.


---
Aquí tienes una explicación más detallada de los patrones de diseño, con ejemplos reescritos para mayor claridad y contexto. Los patrones de diseño son soluciones probadas para problemas comunes en el diseño de software. Ayudan a estructurar el código de manera eficiente, reutilizable y mantenible.

---

### **1. Factory Pattern**
El **Factory Pattern** es un patrón creacional que proporciona una interfaz para crear objetos en una superclase, pero permite a las subclases alterar el tipo de objetos que se crean. Es útil cuando no sabemos de antemano qué tipo de objeto necesitamos o cuando queremos centralizar la lógica de creación.

#### **Ventajas:**
- Centraliza la creación de objetos.
- Facilita la extensión del código.
- Reduce la duplicación de lógica de creación.

#### **Ejemplo en JavaScript:**
```javascript
class Car {
  constructor(model, color) {
    this.model = model;
    this.color = color;
  }
}

class CarFactory {
  createCar(model, color) {
    return new Car(model, color);
  }
}

// Uso del Factory
const factory = new CarFactory();
const car1 = factory.createCar("Sedan", "Red");
const car2 = factory.createCar("SUV", "Blue");

console.log(car1); // Car { model: 'Sedan', color: 'Red' }
console.log(car2); // Car { model: 'SUV', color: 'Blue' }
```

---

### **2. Singleton Pattern**
El **Singleton Pattern** es un patrón creacional que asegura que una clase tenga una única instancia y proporciona un punto global de acceso a ella. Es útil para manejar recursos compartidos, como conexiones a bases de datos o configuraciones globales.

#### **Ventajas:**
- Controla el acceso a una única instancia.
- Reduce el uso innecesario de memoria.

#### **Ejemplo en JavaScript:**
```javascript
class Singleton {
  constructor() {
    if (Singleton.instance) {
      return Singleton.instance; // Devuelve la instancia existente si ya fue creada.
    }
    Singleton.instance = this;
    this.data = "Soy una instancia única";
  }

  getData() {
    return this.data;
  }
}

// Uso del Singleton
const instance1 = new Singleton();
const instance2 = new Singleton();

console.log(instance1 === instance2); // true
console.log(instance1.getData()); // "Soy una instancia única"
```

---

### **3. Observer Pattern**
El **Observer Pattern** es un patrón de comportamiento que define una relación uno-a-muchos entre objetos. Cuando un objeto cambia de estado (el sujeto), notifica automáticamente a todos sus observadores.

#### **Ventajas:**
- Promueve el desacoplamiento entre el sujeto y los observadores.
- Facilita la comunicación entre objetos.

#### **Ejemplo en JavaScript:**
```javascript
class Subject {
  constructor() {
    this.observers = [];
  }

  subscribe(observer) {
    this.observers.push(observer); // Agrega un observador.
  }

  unsubscribe(observer) {
    this.observers = this.observers.filter(obs => obs !== observer); // Elimina un observador.
  }

  notify(data) {
    this.observers.forEach(observer => observer.update(data)); // Notifica a todos los observadores.
  }
}

class Observer {
  update(data) {
    console.log(`Observer recibió: ${data}`);
  }
}

// Uso del Observer
const subject = new Subject();
const observer1 = new Observer();
const observer2 = new Observer();

subject.subscribe(observer1);
subject.subscribe(observer2);

subject.notify("Hola Observadores!"); // Ambos observadores reciben el mensaje.
```

---

### **4. Strategy Pattern**
El **Strategy Pattern** es un patrón de comportamiento que permite definir una familia de algoritmos, encapsularlos y hacerlos intercambiables. Es útil cuando necesitas cambiar dinámicamente el comportamiento de un objeto.

#### **Ventajas:**
- Facilita la extensión de nuevos algoritmos.
- Reduce el uso de condicionales en el código.

#### **Ejemplo en JavaScript:**
```javascript
class PaymentStrategy {
  pay(amount) {
    throw new Error("Este método debe ser implementado por una subclase");
  }
}

class CreditCardPayment extends PaymentStrategy {
  pay(amount) {
    console.log(`Pagado ${amount} con tarjeta de crédito.`);
  }
}

class PayPalPayment extends PaymentStrategy {
  pay(amount) {
    console.log(`Pagado ${amount} con PayPal.`);
  }
}

class PaymentContext {
  setStrategy(strategy) {
    this.strategy = strategy; // Define la estrategia de pago.
  }

  executePayment(amount) {
    this.strategy.pay(amount); // Ejecuta la estrategia seleccionada.
  }
}

// Uso del Strategy
const context = new PaymentContext();
context.setStrategy(new CreditCardPayment());
context.executePayment(100); // Pagado 100 con tarjeta de crédito.

context.setStrategy(new PayPalPayment());
context.executePayment(200); // Pagado 200 con PayPal.
```

---

### **5. Module Pattern**
El **Module Pattern** es un patrón estructural que permite encapsular código en un ámbito privado y exponer solo las partes necesarias como una API pública. Es útil para organizar el código y evitar la contaminación del espacio global.

#### **Ventajas:**
- Encapsula datos y lógica.
- Evita conflictos en el espacio global.

#### **Ejemplo en JavaScript:**
```javascript
const CounterModule = (function () {
  let count = 0; // Variable privada.

  return {
    increment() {
      count++;
      console.log(`Count: ${count}`);
    },
    reset() {
      count = 0;
      console.log("Counter reiniciado.");
    }
  };
})();

// Uso del Module
CounterModule.increment(); // Count: 1
CounterModule.increment(); // Count: 2
CounterModule.reset();     // Counter reiniciado.
```

---

### **6. Decorator Pattern**
El **Decorator Pattern** es un patrón estructural que permite añadir funcionalidad a un objeto de manera dinámica sin modificar su estructura original. Es útil para extender el comportamiento de objetos existentes.

#### **Ventajas:**
- Añade funcionalidad sin modificar el código original.
- Promueve la reutilización de código.

#### **Ejemplo en JavaScript:**
```javascript
function addTimestamp(originalFunction) {
  return function (...args) {
    console.log(`Timestamp: ${new Date().toISOString()}`);
    return originalFunction(...args);
  };
}

function greet(name) {
  console.log(`Hola, ${name}!`);
}

// Uso del Decorator
const decoratedGreet = addTimestamp(greet);
decoratedGreet("Alice");
// Timestamp: 2025-04-23T12:00:00.000Z
// Hola, Alice!
```

---

### **Resumen**
- **Factory**: Centraliza la creación de objetos.
- **Singleton**: Garantiza una única instancia de una clase.
- **Observer**: Notifica a múltiples objetos sobre cambios de estado.
- **Strategy**: Permite intercambiar algoritmos dinámicamente.
- **Module**: Encapsula datos y lógica en un ámbito privado.
- **Decorator**: Añade funcionalidad a objetos de forma dinámica.

Estos patrones son fundamentales para escribir código más limpio, reutilizable y fácil de mantener.
