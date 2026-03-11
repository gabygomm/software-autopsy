# Análisis de Arquitectura

## Clases Principales del Sistema

### Book
Representa la entidad **Book** dentro del sistema.

Contiene información relacionada con los libros:
- id
- title
- author
- quantity
- availability

Esta clase funciona como un **modelo de datos** que representa cada libro
dentro del sistema de gestión de biblioteca.

---

### BookRepository
Clase encargada de gestionar las operaciones relacionadas con los libros.

Métodos principales:
- addBook()
- upgradeBookQty()
- searchBookById()
- searchBookByTitle()
- searchBookByAuthor()
- displayAllBooks()
- checkoutBook()
- checkinBook()

Esta clase actúa como un **repositorio en memoria**, utilizando
estructuras del Java Collections Framework para almacenar y gestionar
los libros del sistema.

---

### Student
Representa la entidad **Student** dentro del sistema.

Contiene información como:
- id
- name
- booksAssigned

También gestiona acciones relacionadas con el préstamo de libros:
- borrowBook()
- returnBook()

Esta clase modela a los estudiantes registrados en la biblioteca.

---

### StudentRepository
Clase encargada de gestionar las operaciones relacionadas con los estudiantes.

Métodos principales:
- registerStudent()
- searchStudentById()
- displayAllStudents()

Esta clase se encarga de almacenar y gestionar los estudiantes
registrados dentro del sistema.

---

### Library
Clase principal que contiene las funciones principales del sistema
y el menú de interacción con el usuario.

Permite realizar operaciones como:
- add new book
- upgrade book quantity
- search book
- display all books
- register student
- display all students
- checkout book
- checkin book

Esta clase actúa como el **punto central de control de la aplicación**,
coordinando las operaciones entre libros, estudiantes y el usuario.

---

### ErrorHandling
Clase encargada de validar las entradas del usuario y prevenir errores
durante la ejecución del programa.

Incluye validaciones como:
- validación de opciones del menú
- validación de ID
- validación de números enteros
- validación de entradas de texto

Su objetivo es evitar que el sistema falle cuando el usuario introduce
datos inválidos.
