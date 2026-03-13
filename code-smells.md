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

