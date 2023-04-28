@OneToOne 
Se debe poner sobre el atributo que tenga la relación
@JoinColumn(name = "<name>") Nombre de la columna que las une, el atributo tiene que estar en la clase dueña de la relación
@OneToOne(cascade = {cascadeType.ALL}) me persiste la información, aún sin estar creada, o me remueve registros en cascada de ser eliminados.

Relación inversa
@OneToOne(mappedBy = "<name>") nombre del otro campo que se relaciona
@OneToOne(mappedBy = "<name>", fetch = FetchType.Lazy) Lazy para no generar el empleado hasta que no se requiera al empleado

---

@OneToMany Se pone en la entidad que sea dueña de muchas otras
@ManyToOne Se localiza en la entidad que es de una sola otra entidad

Tambien pueden utilizar cacade, fetch como parametros

@JoinColumn(name="<name>") para indicar la tabla que liga con la otra entidad, este se anota en el @ManyToOne7
Parámetro mappedBy para indicar la entidad del otro extremo de la relación



---

@ManyToMany