@OneToOne 
Se debe poner sobre el atributo que tenga la relación
@JoinColumn(name = "<name>") Nombre de la columna que las une, el atributo tiene que estar en la clase dueña de la relación
@OneToOne(cascade = {cascadeType.ALL}) me persiste la información, aún sin estar creada, o me remueve registros en cascada de ser eliminados.

Relaci
@OneToOne(mappedBy = "<name>") nombre del otro campo que se relaciona

@OneToMany

@ManyToOne

@ManyToMany