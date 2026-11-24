# Escenario 1: Servicios Adicionales en Correo Privado

## 1. Identificación del Patrón
- **Patrón:** Decorator
- **Categoría:** Estructural

## 2. Fundamentación Teórica
- **Problema detectado:** Explosión combinatoria de subclases mediante herencia estática. Al intentar modelar cada combinación posible de servicios opcionales (`EnvioConSeguro`, `EnvioConSeguroYFragil`, `EnvioPrioritarioConAcuse`, etc.) sobre un envío estándar, la jerarquía crece de forma exponencial ($2^n$ combinaciones) y el mantenimiento se vuelve inviable ante cualquier nuevo servicio adicional.
- **Solución con Decorator:** El patrón envuelve dinámicamente un objeto `Envio` dentro de objetos decoradores que implementan la misma interfaz (`ServicioEnvio`) y mantienen una referencia por agregación al componente envuelto. Cada decorador delega el cálculo base al envío que envuelve y suma su propio costo y descripción. Así se pueden apilar múltiples extras en tiempo de ejecución en cualquier orden o combinación, respetando el **Principio Abierto/Cerrado (OCP)** y favoreciendo la **composición sobre la herencia**.

## 3. Diagrama de Clases UML

```plantuml
@startuml
skinparam classAttributeIconSize 0

interface ServicioEnvio {
  + calcularCosto(): double
  + obtenerDescripcion(): String
}

class EnvioEstandar {
  - pesoKg: double
  - costoBase: double
  + EnvioEstandar(pesoKg: double, costoBase: double)
  + calcularCosto(): double
  + obtenerDescripcion(): String
}

abstract class DecoradorEnvio {
  # envioEnvuelto: ServicioEnvio
  + DecoradorEnvio(envio: ServicioEnvio)
  + calcularCosto(): double
  + obtenerDescripcion(): String
}

class SeguroContraRobo {
  - porcentajePrima: double
  + SeguroContraRobo(envio: ServicioEnvio, porcentajePrima: double)
  + calcularCosto(): double
  + obtenerDescripcion(): String
}

class EmbalajeFragil {
  - recargoEmbalaje: double
  + EmbalajeFragil(envio: ServicioEnvio, recargoEmbalaje: double)
  + calcularCosto(): double
  + obtenerDescripcion(): String
}

class EntregaPrioritaria {
  - tarifaUrgencia: double
  + EntregaPrioritaria(envio: ServicioEnvio, tarifaUrgencia: double)
  + calcularCosto(): double
  + obtenerDescripcion(): String
}

class AcuseRecibo {
  - costoCertificacion: double
  + AcuseRecibo(envio: ServicioEnvio, costoCertificacion: double)
  + calcularCosto(): double
  + obtenerDescripcion(): String
}

' Realización / Implementación
EnvioEstandar ..|> ServicioEnvio : implements
DecoradorEnvio ..|> ServicioEnvio : implements

' Agregación (envuelve)
DecoradorEnvio "1" o--> "1" ServicioEnvio : envuelve >

' Herencia / Generalización
SeguroContraRobo --|> DecoradorEnvio : extends
EmbalajeFragil --|> DecoradorEnvio : extends
EntregaPrioritaria --|> DecoradorEnvio : extends
AcuseRecibo --|> DecoradorEnvio : extends

@enduml
```
