# Análisis de Arquitectura

## Clases Principales del Sistema

### Book
Representa la entidad **Book** dentro del sistema.

Contiene información relacionada con los libros:
- id
- title
- author
- totalQuantity
- available

Esta clase funciona como un **modelo de datos** que representa cada libro
dentro del sistema de gestión de biblioteca.

---

### BookRepository
Clase encargada de gestionar las operaciones relacionadas con los libros.

Métodos principales:
- addBook()
- upgradeQuantity()
- findById()
- searchByTitle()
- searchByAuthor()
- getAllBooks()
- issueBook()
- returnBook()

Esta clase actúa como un **repositorio en memoria**, utilizando
estructuras del Java Collections Framework para almacenar y gestionar
los libros del sistema.

---

### Student
Representa la entidad **Student** dentro del sistema.

Contiene información como:
- id
- name
- issueBookIds

También gestiona acciones relacionadas con el préstamo de libros:
- canIssueMore()
- addIssuedBook()
- removeIssuedBook()

Esta clase modela a los estudiantes registrados en la biblioteca.

---

### StudentRepository
Clase encargada de gestionar las operaciones relacionadas con los estudiantes.

Métodos principales:
- registerStudent()
- findById()
- getAllStudents()

Esta clase se encarga de almacenar y gestionar los estudiantes
registrados dentro del sistema.

---

### Library
Clase principal que contiene las funciones principales del sistema
y el menú de interacción con el usuario.

Permite realizar operaciones como:
- addBook
- upgradeQuantity
- searchBook
- showAllBooks
- registerStudent
- showAllStudents
- checkOutBook
- checkInBook

Esta clase actúa como el **punto central de control de la aplicación**,
coordinando las operaciones entre libros, estudiantes y el usuario.

---

### ErrorHandling
Clase encargada de validar las entradas del usuario y prevenir errores
durante la ejecución del programa.

Incluye validaciones como:
- validateMenuChoice
- validateID
- validateIntegerInput
- validateStringInput

Su objetivo es evitar que el sistema falle cuando el usuario introduce
datos inválidos.
