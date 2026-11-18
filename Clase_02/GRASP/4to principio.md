Diagrama UML antes de aplicar el principio:
@startuml
skinparam classAttributeIconSize 0

class CalculadoraPuntos {
  + asignarPuntaje(docente: Docente, rol: String): int
}

class Docente {
  - legajo: String
  - nombre: String
}

CalculadoraPuntos ..> Docente : consulta el rol >

@enduml

Despues de aplicar el principio:
@startuml
skinparam classAttributeIconSize 0

abstract class RolEnTesis {
  + calcularPuntaje(): int
}

class Tutor {
  + calcularPuntaje(): int
}

class JuradoTitular {
  + calcularPuntaje(): int
}

class JuradoSuplente {
  + calcularPuntaje(): int
}

class Docente {
  - legajo: String
  - nombre: String
  - rolEnTesis: RolEnTesis
  + calcularPuntaje(): int
}

RolEnTesis <|-- Tutor
RolEnTesis <|-- JuradoTitular
RolEnTesis <|-- JuradoSuplente
Docente "0..*" --> "1" RolEnTesis : participa como >

@enduml
