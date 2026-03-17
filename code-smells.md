# Code Smell Analysis: Book.java
## 1. Data Class
### Código afectado:
``` java
public class Book {
    private int id;
    private String title;
    // ... (campos y getters/setters para cada atributo)
    public void setId(int id) { this.id = id; }
    public void setTitle(String title) { this.title = title; }
    public void setAuthor(String author) { this.author = author; }
    // ...
}
```
### Definición: 
La clase se comporta como un simple almacén de datos. Al exponer todos sus campos mediante setters públicos, pierde el control sobre su propio estado interno.

## 2. Speculative Generality
### Código afectado:
``` java
import librarymanagementsystem.BookRepository;
import librarymanagementsystem.Student;
import librarymanagementsystem.StudentRepository;
import librarymanagementsystem.Library;
```
### Definición: 
Se incluyen importaciones de clases que no se utilizan en ninguna parte del cuerpo del archivo, lo que añade acoplamiento innecesario.

## 3. Redundant Conditional
### Código afectado:
``` java
public boolean isAvailable() {
    if (getAvailable() > 0) return true;
    return false;
}
```
### Definición: 
El uso de un if para retornar true o false basándose en una comparación es redundante, ya que la comparación > 0 ya es un valor booleano.

## 4. Dead Code
### Código afectado:
``` java
//  System.out.println("Book is out of stock!");
```
### Definición: 
Fragmentos de código comentados que permanecen en el archivo fuente. Esto genera ruido visual y debe ser eliminado en favor del control de versiones (Git).

## 5. Primitive Obession
### Código afectado:
``` java
    private int id;
    private String title;
    private String author;
    private int totalQuantity;
    private int available;
```
### Definición:
Se usan tipos primitivos como int y String para representar conceptos del dominio como el identificador del libro, autor y cantidades, en lugar de tipos más específicos.

---

# Code Smell Analysis: BookRepository.java
## 1. Duplicate Code
### Código afectado:
``` java
public List<Book> searchByTitle(String q) {
    List<Book> bookList = new ArrayList<Book>();
    for (Book b : books.values()) {
        if (b.getTitle().contains(q)) {
            bookList.add(b);
        }
    }
    ...
}

public List<Book> searchByAuthor(String q) {
    List<Book> bookList = new ArrayList<Book>();
    for (Book b : books.values()) {
        if (b.getAuthor().contains(q)) {
            bookList.add(b);
        }
    }
    ...
}
```
### Definición:
Este mal olor ocurre cuando hay código repetido en varios lugares del programa. En este caso, los métodos searchByTitle y searchByAuthor tienen prácticamente la misma lógica; solo cambia el atributo que se consulta. Esto dificulta el mantenimiento, porque si se quiere cambiar la lógica de búsqueda, habría que modificar varios métodos en lugar de uno solo.

## 2. Global State
### Código afectado: 
``` java
private static Map<Integer, Book> books = new HashMap<Integer, Book>();
...
books.put(b.getId(), b);
```
### Definición:
El uso de variables static para almacenar datos compartidos crea un estado global accesible desde cualquier parte del programa. Esto puede provocar problemas de mantenimiento, pruebas y control de datos, ya que todas las instancias de la clase comparten la misma colección de libros.

## 3. Message Chains
### Código afectado:
``` java
public boolean issueBook(int bookId) {
    findById(bookId).decrementAvailable();
    ...
}
```
### Definición:
Este mal olor aparece cuando un método llama a otro y luego inmediatamente invoca otro método sobre el resultado. Esto genera dependencias fuertes entre objetos y aumenta el riesgo de errores como NullPointerException si el objeto retornado es null.

## 4. Inappropriate Intimacy
### Código afectado:
``` java
b.setTotalQuantity(b.getTotalQuantity() + extra);
b.setAvailable(b.getAvailable() + extra);
...
b.decrementAvailable();
```
### Definición:
Sucede cuando una clase conoce demasiado sobre los detalles internos de otra clase. Aquí BookRepository manipula directamente el estado interno de Book. Esto rompe el principio de encapsulación y aumenta el acoplamiento entre clases.

## 5.Divergent Change
### Código afectado:
``` java
public boolean addBook(Book b) { ... }

public boolean issueBook(int bookId) { ... }

public boolean returnBook(int bookId) { ... }
``` 
### Definición:
La clase está realizando diferentes responsabilidades: almacenar libros, gestionar préstamos y gestionar devoluciones. Cuando una clase tiene múltiples responsabilidades, cualquier cambio en una de ellas obliga a modificar la misma clase, lo que la hace más difícil de mantener.

## 6. Null Handling Problem
### Código afectado: 
``` java
public Book findById(int bookId) {
    if (books.containsKey(bookId)) {
        return books.get(bookId);
    }
    ...
    return null;
}
```
### Definición:
El método devuelve null cuando no encuentra un libro. Esto puede provocar errores si otro método usa el resultado sin verificarlo. Este tipo de diseño puede generar fallos en tiempo de ejecución y hace el código menos seguro.

## 7. Excessive Console Output
### Código afectado:
``` java
System.out.println("Book added successfully!");
System.out.println(b);

System.out.println("Book returned successfully!");
```
### Definición:
La clase BookRepository se encarga de imprimir mensajes en consola. Esto es un mal olor porque mezcla lógica de negocio con presentación. Lo ideal sería que la clase solo gestione datos y que la interfaz de usuario sea la encargada de mostrar mensajes.

## 8. Primitive Obsession
### Código afectado:
``` java
public boolean issueBook(int bookId) { ... }

public Book findById(int bookId) { ... }
```
### Definición:
Se utilizan tipos primitivos (int) para representar conceptos importantes del dominio, como el identificador de un libro. En sistemas más robustos se suelen usar objetos específicos (por ejemplo BookId) para representar estos valores y evitar errores.

## 9. Shotgun Surgery
### Código afectado:
``` java
public boolean issueBook(int bookId) { ... }

public boolean returnBook(int bookId) { ... }

public boolean upgradeQuantity(int bookId, int extra) { ... }
```
### Definición:
Este mal olor ocurre cuando un cambio pequeño en el sistema obliga a modificar varios métodos o partes del código. Por ejemplo, si cambia la lógica de manejo de cantidades o disponibilidad de libros, habría que modificar varios métodos en la misma clase.

## 10. Long Method
### Código afectado:
``` java
public boolean addBook(Book b) {
    if (books.containsKey(b.getId())) {
        System.out.println("Book already exists!");
        return false;
    }
    ...
}
```
### Definición:
Un método hace demasiadas tareas (validar, guardar y mostrar mensajes). Esto dificulta leer y mantener el código. Cada método debería tener una sola responsabilidad.

--- 

# Code Smell Analysis: ErrorHandling.java
## 1. Long Method
### Código afectado: 
``` java
public int validateMenuChoice(int val, String msg, Scanner sc) { ... }
public int validateID(int val, String msg, Scanner sc) { ... }
public int validateIntegerInput(int val, String msg, Scanner sc) { ... }
public String validateStringInput(String val, String msg, Scanner sc) { ... }
```
### Definición: 
El método realiza muchas acciones: lectura, conversión de tipos, validación de rango y mensajes de error. Esto dificulta la lectura y mantenimiento.

## 2. Duplicated Code
### Código afectado:
``` java
// Estructura idéntica en validateMenuChoice, validateID y validateIntegerInput
while (true) {
    try {
        System.out.print(msg + " ");
        String input = sc.nextLine();
        val = Integer.parseInt(input);
        if (val < min || val > max) { // ... lógica de rango ... }
        break;
    } catch (InputMismatchException e) { // ... }
}
```
### Definición: 
Se repite la misma estructura de control, manejo de excepciones y lectura de teclado en tres métodos diferentes. La única variación son los límites numéricos, lo que indica que la lógica debería ser parametrizada en un único método genérico.

## 3. Magic Numbers
### Código afectado:
``` java
if (val < 0 || val > 9)      // validateMenuChoice
if (val < 100 || val > 1000) // validateID
if (val < 0 || val > 100)    // validateIntegerInput
```
### Definición: 
El uso de valores numéricos literales directamente en las comparaciones sin asignarles un nombre claro. Esto dificulta la comprensión de por qué esos números son los límites y complica el mantenimiento si las reglas de validación cambian.

## 4. Primitive Obsession
### Código afectado:
``` java
public int validateMenuChoice(int val, String msg, Scanner sc) { ... }
public int validateID(int val, String msg, Scanner sc) { ... }
public int validateIntegerInput(int val, String msg, Scanner sc) { ... }
public String validateStringInput(String val, String msg, Scanner sc) { ... }
```
### Definición: 
Se usan tipos primitivos (int, String) para representar datos que podrían tener objetos especializados que encapsulen reglas de validación.

## 5. Inconsistent Exception Handling
### Código afectado:
``` java
catch (InputMismatchException e) {
    System.out.println("Invalid input. Please enter a valid number.");
    sc.nextLine();
}
``` 
### Definición: 
Se intenta capturar InputMismatchException, pero Integer.parseInt() lanza NumberFormatException. Por eso el manejo de excepciones no es efectivo.

---

# Code Smell Analysis: Library.java
## 1. Long Method
### Código afectado:
``` java
public void searchBook(Scanner sc, BookRepository bookRepo, ErrorHandling err) {
    String query = err.validateStringInput("", "Search for the Book here:", sc);

    List<Book> bookListByName = bookRepo.searchByTitle(query);
    List<Book> bookListByAuthor = bookRepo.searchByAuthor(query);

    ...

    for (Book b : bookListByName) {
        System.out.println(b.toString());
    }

    ...

    for (Book b : bookListByAuthor) {
        System.out.println(b.toString());
    }

    ...
}
``` 
### Definición:
El método searchBook está haciendo varias cosas: pedir datos, buscar libros por título y autor, verificar si se encontraron resultados y mostrar los libros en pantalla.

## 2. Long Parameter List
### Código afectado:
``` java
public void checkOutBook(Scanner sc, BookRepository bookRepo,
                         StudentRepository studRepo, ErrorHandling err)

public void checkInBook(Scanner sc, BookRepository bookRepo,
                        StudentRepository studRepo, ErrorHandling err)
``` 
### Definición:
Los métodos necesitan varios objetos externos (Scanner, BookRepository, StudentRepository, ErrorHandling) para poder funcionar.

## 3. Feature Envy
### Código afectado:
```  java
Book b = bookRepo.findById(bookId);
Student s = studRepo.findById(studId);

if (s == null || b == null || !b.isAvailable() || !s.canIssueMore()) {
    System.out.println("Book can't be issued!");
    return;
}
...
bookRepo.issueBook(bookId);
s.addIssuedBook(bookId, bookRepo);
``` 
### Definición:
El método checkOutBook usa constantemente métodos y datos de Book, Student y BookRepository para tomar decisiones y realizar acciones.

## 4. Primitive Obsession
### Código afectado:
``` java
int bookId = err.validateID(-1, "Enter Book Id:", sc);
int studId = err.validateID(-1, "Enter Student Id:", sc);
```
### Definición:
Los IDs de libros y estudiantes se están manejando directamente como int dentro de los métodos.

## 5. Large Class
### Código afectado: 
``` java
public class Library {

    public void addBook(...) { ... }

    public void upgradeQuantity(...) { ... }

    public void searchBook(...) { ... }

    public void registerStudent(...) { ... }

    public void checkOutBook(...) { ... }

    public void checkInBook(...) { ... }

    public static void main(...) { ... }
}
```
### Definición:
La clase Library contiene métodos para gestionar libros, estudiantes, préstamos, devoluciones y el menú del sistema. Si algo cambia en la interfaz o en la base de datos, esta clase se rompe.

## 6. Duplicate Code
### Código afectado:
``` java
System.out.println("Books by same Name:");
for (Book b : bookListByName) {
    System.out.println(b.toString());
}
...
System.out.println("Books by same Author Name:");
for (Book b : bookListByAuthor) {
    System.out.println(b.toString());
}
...
if (!isBookFound) System.out.println("None");
else System.out.println();
Definición
```
### Definición: 
Dentro del método searchBook se repite la misma lógica para mostrar resultados de búsqueda y recorrer listas de libros, además de repetirse la verificación para mostrar "None" cuando no se encuentran resultados. Estas estructuras aparecen varias veces dentro del mismo método.

## 7. Switch Statements
### Código afectado:
``` java
while (true) {
    int option = err.validateMenuChoice(-1, library.showMenu(), sc);

    switch (option) {
        case 0:
            System.out.println("Exiting Application...");
            System.exit(0);
        case 1:
            library.addBook(sc, bookRepo, err);
            break;
        case 2:
        ...
        default:
            System.out.println("Invalid input");
    }
}
```
### Definición
El flujo del programa depende de un switch con muchas opciones del menú, donde cada caso ejecuta una acción diferente de la clase Library.
Toda la lógica de decisiones del sistema está concentrada en este switch.

## 8. Data Clump (Grupo de Datos)
### Código afectado:
``` java
// En addBook:
public void addBook(Scanner sc, BookRepository bookRepo, ErrorHandling err)

// En upgradeQuantity:
public void upgradeQuantity(Scanner sc, BookRepository bookRepo, ErrorHandling err)

// En searchBook:
public void searchBook(Scanner sc, BookRepository bookRepo, ErrorHandling err)
```
### Definición: 
Cuando ves que un grupo de variables siempre viajan juntas como mejores amigas. Scanner, BookRepository y ErrorHandling aparecen en casi todas las firmas de los métodos. Deberían ser atributos globales de la clase.

## 9. Inappropriate Intimacy
### Código afectado:
``` java
Book b = bookRepo.findById(bookId);
Student s = studRepo.findById(studId);

if (!s.getIssuedBookIds().contains(bookId)) {
    System.out.println(s.getName() + " didn't issue this book!");
    return;
}

bookRepo.returnBook(bookId);
s.removeIssuedBook(bookId, bookRepo);
```
### Definición:
El método checkInBook está accediendo directamente a la estructura interna de Student y Book, manipulando sus datos internos en lugar de usar métodos de alto nivel que encapsulen la lógica.
Hay una relación demasiado estrecha entre Library y Student/Book, rompiendo la independencia de las clases.

---
