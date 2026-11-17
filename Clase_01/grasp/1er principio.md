Diagrama UML, antes del principio:
@startuml
skinparam classAttributeIconSize 0

class ValidadorAcademico {
  + validarRequisitosTesis(e: Estudiante): boolean
}

class Estudiante {
  - legajo: String
  - nombre: String
  + getMateriasCursadas(): List<MateriaCursada>
  + getPlanEstudios(): PlanEstudios
}

class MateriaCursada {
  - nota: double
  - estado: String
  + getNota(): double
  + estaAprobada(): boolean
}

class PlanEstudios {
  - codigo: String
  - cantidadTotalMaterias: int
  + getCantidadTotalMaterias(): int
}

ValidadorAcademico ..> Estudiante : consulta >
ValidadorAcademico ..> MateriaCursada : extrae notas >
ValidadorAcademico ..> PlanEstudios : compara >

Estudiante "1" *-- "0..*" MateriaCursada : cursa >
Estudiante "0..*" --> "1" PlanEstudios : sigue >

@enduml

Despues de aplicar el principio:
@startuml
skinparam classAttributeIconSize 0

class Estudiante {
  - legajo: String
  - nombre: String
  + puedePresentarTesis(): boolean
  - calcularMateriasAprobadas(): int
}

class MateriaCursada {
  - nota: double
  - estado: String
  + estaAprobada(): boolean
}

class PlanEstudios {
  - codigo: String
  - cantidadTotalMaterias: int
  + getCantidadTotalMaterias(): int
}

Estudiante "1" *-- "0..*" MateriaCursada : cursa >
Estudiante "0..*" --> "1" PlanEstudios : sigue >

@enduml