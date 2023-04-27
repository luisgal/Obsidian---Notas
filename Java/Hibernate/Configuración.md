Persistence.xml 

Spring aplicaction.properties

javax o jakarta libreria persistence, la libreria de hibernate estan depreciadas

Clase
@Entity sobre la clase
@Table(name="<name>") para indicar el nombre de la tabla, especialmente si la tabla no tiene el mismo nombre a la clase

Atributos
@Column sobre los atributo 
@Column(name="<name>") Si el nombre de la columna no es el mismo al del atributo
@Id -> el tipo de datos debe ser un long



GESTOR DE PERSISTENCIA
---
EntityManager <<interface>>
@PersistenceContext(unitName="<name>") Nombre de la entidad declarada en properties
EntityManagerFactory <<interface>>
@PersistenceUnit(unitName="<name>") Nombre de la entidad

O tambien se puede crear por código con

emf = Persistence.createEntityManagerFactory("<name>") Nombre de la unidad de persistencia
manager = emf.createEntityManager();
---

Con el manager se pueden hacer consultas jql

Se puden crear querys hql arriba de la clase con una anotación


manager.getTransaction().begin() // Para iniciar una transacción
manager.persist(<persist>) // La clase que se desea persistir
manager.getTransaction().commit(9)