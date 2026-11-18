Diagrama UML antes de aplicar el principio:
@startuml
skinparam classAttributeIconSize 0

class VentanaDefensaUI {
  + onAprobarTesis(): void
}

class ProyectoTesis {
  - codigo: String
  - estado: String
  + actualizarEstado(estado: String): void
}

class ApiTituloNacional {
  + iniciarTramite(legajo: String): void
}

VentanaDefensaUI ..> ProyectoTesis : actualiza estado en la BD >
VentanaDefensaUI ..> ApiTituloNacional : llama a la API >

@enduml

Despues de aplicar el principio:
@startuml
skinparam classAttributeIconSize 0

class VentanaDefensaUI {
  + onAprobarTesis(): void
}

class ControladorDefensa {
  - proyecto: ProyectoTesis
  + aprobarTesis(): void
}

class ProyectoTesis {
  - codigo: String
  - estado: EstadoTesis
  + aprobar(): void
}

class ServicioTramiteTitulo {
  + iniciarTramite(proyecto: ProyectoTesis): void
}

class ApiTituloNacional {
  + iniciarTramite(legajo: String): void
}

VentanaDefensaUI --> ControladorDefensa : delega el evento >
ControladorDefensa --> ProyectoTesis : aprueba >
ControladorDefensa --> ServicioTramiteTitulo : solicita el trámite >
ServicioTramiteTitulo ..> ApiTituloNacional : adapta la llamada >

@enduml
