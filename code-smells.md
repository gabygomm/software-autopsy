# Code Smell Analysis: Book.java
## 1. Data Class
### Código afectado:
```
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
```
import librarymanagementsystem.BookRepository;
import librarymanagementsystem.Student;
import librarymanagementsystem.StudentRepository;
import librarymanagementsystem.Library;
```
### Definición: 
Se incluyen importaciones de clases que no se utilizan en ninguna parte del cuerpo del archivo, lo que añade acoplamiento innecesario.

## 3. Redundant Conditional
### Código afectado:
```
public boolean isAvailable() {
    if (getAvailable() > 0) return true;
    return false;
}
```
### Definición: 
El uso de un if para retornar true o false basándose en una comparación es redundante, ya que la comparación > 0 ya es un valor booleano.

## 4. Dead Code
### Código afectado:
```
//  System.out.println("Book is out of stock!");
```
### Definición: 
Fragmentos de código comentados que permanecen en el archivo fuente. Esto genera ruido visual y debe ser eliminado en favor del control de versiones (Git).

---
