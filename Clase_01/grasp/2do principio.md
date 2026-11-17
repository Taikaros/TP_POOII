Diagrama UML antes de aplicar el pírncipio:
@startuml
skinparam classAttributeIconSize 0

class PantallaAsignarJurado {
  + asignarVotosJurados(): void
}

class TribunalEvaluador {
  - codigo: String
  + setVotos(votos: List<VotoJurado>): void
}

class VotoJurado {
  - puntaje: int
  - observaciones: String
}

class Docente {
  - legajo: String
  - nombre: String
}

PantallaAsignarJurado ..> VotoJurado : crea (new)[cite: 2]
PantallaAsignarJurado --> TribunalEvaluador : invoca setVotos()[cite: 2]
TribunalEvaluador "1" *-- "3" VotoJurado : contiene >
VotoJurado "1" --> "1" Docente : emitido por >

@enduml
Despues de aplicarlo:
@startuml
skinparam classAttributeIconSize 0

class PantallaAsignarJurado {
  + registrarJurados(docentes: List<Docente>): void
}

class TribunalEvaluador {
  - codigo: String
  + registrarJurados(docentes: List<Docente>): void
}

class VotoJurado {
  - puntaje: int
  - observaciones: String
  + VotoJurado(docente: Docente)
}

class Docente {
  - legajo: String
  - nombre: String
}

PantallaAsignarJurado --> TribunalEvaluador : solicita registrar jurados >
TribunalEvaluador "1" *-- "3" VotoJurado : crea y contiene >
VotoJurado "1" --> "1" Docente : emitido por >

@enduml