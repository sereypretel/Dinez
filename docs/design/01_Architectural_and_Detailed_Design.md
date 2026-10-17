## 2. Diagrama de Componentes

El siguiente diagrama muestra los componentes principales enfocados en el requerimiento REQ-04, sus límites, actores e interfaces.

```mermaid
graph TD
    %% Actores Externos
    Actor_Enfermera([Personal Médico / Enfermería])
    Actor_Pantalla([Pantallas de Sala de Espera])

    %% Límite del Sistema
    subgraph Sistema_Urgencias [Sistema de Gestión de Urgencias]
        UI[Componente de Interfaz Clínica]
        CompTriage[Componente Gestor de Triage]
        CompTurnos[Componente Gestor de Turnos y Colas]
        CompNotificador[Componente Notificador en Tiempo Real]
        DB[(Repositorio de Datos Clínicos)]
    end

    %% Relaciones e Interfaces
    Actor_Enfermera -->|Registra valoración clínica| UI
    UI -->|Solicita cálculo y registro| CompTriage
    CompTriage -->|Persiste valoración (I-V)| DB
    CompTriage -->|Informa nivel de urgencia (II-V)| CompTurnos
    CompTurnos -->|Aplica BR-TRG-02: Genera turno y ordena| DB
    CompTurnos -->|Emite evento de actualización| CompNotificador
    CompNotificador -->|Empuja actualización visual| Actor_Pantalla
