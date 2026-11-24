# Escenario 3: Registro Centralizado de Auditoría Hospitalaria

## 1. Identificación del Patrón
- **Patrón:** Singleton
- **Categoría:** Creacional

## 2. Fundamentación Teórica
- **Problema detectado:** Duplicación de recursos compartidos y ausencia de control centralizado de acceso. Al instanciar cada módulo (`Turnos`, `Guardia`, `Farmacia`, `Facturación`) su propio `GestorAuditoria`, múltiples objetos compiten simultáneamente por el mismo archivo `auditoria.log`, provocando condiciones de carrera, escrituras entrelazadas o fuera de orden y consumo redundante de memoria y descriptores de archivo.
- **Solución con Singleton:** Garantiza que `GestorAuditoria` posea una única instancia en toda la aplicación y provee un punto de acceso global estático (`obtenerInstancia()`). Al declarar el constructor como privado y almacenar la única instancia en un atributo estático, todos los módulos hospitalarios comparten el mismo gestor, el cual sincroniza la escritura secuencial sobre `auditoria.log`.

## 3. Diagrama de Clases UML

```plantuml
@startuml
skinparam classAttributeIconSize 0

class GestorAuditoria {
  - {static} instancia: GestorAuditoria
  - rutaArchivo: String
  - escritorLog: FileWriter
  - GestorAuditoria()
  + {static} obtenerInstancia(): GestorAuditoria
  + registrarEvento(modulo: String, usuario: String, operacion: String): void
}

class ModuloTurnos {
  + confirmarTurno(idTurno: int): void
}

class ModuloGuardia {
  + registrarIngreso(dniPaciente: int): void
}

class ModuloFarmacia {
  + dispensarMedicamento(idReceta: int): void
}

class ModuloFacturacion {
  + emitirComprobante(idFactura: int): void
}

' Asociación reflexiva (única instancia)
GestorAuditoria "1" --> "1" GestorAuditoria : - instancia

' Dependencia (los módulos usan la instancia única)
ModuloTurnos ..> GestorAuditoria : usa >
ModuloGuardia ..> GestorAuditoria : usa >
ModuloFarmacia ..> GestorAuditoria : usa >
ModuloFacturacion ..> GestorAuditoria : usa >

@enduml
```
