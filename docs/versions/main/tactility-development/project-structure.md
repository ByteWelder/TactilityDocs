# Project Structure

- `Boards`: Contains board configuration projects with drivers (and simulator)
- `Firmware`: The application/firmware example project
- `Libraries`: Contains a mix of regular libraries and ESP modules
- `Tactility`: The main application platform code
- `TactilityC`: C wrappers for making side-loaded apps (C++ is not supported yet)
- `TactilityCore`: Core functionality regarding threads, stdlib, etc.

```plantuml
@startuml
skinparam componentStyle uml1

[InternalApp]
note top of InternalApp : An app that's built into the main firmware
[ExternalApp]
note right of ExternalApp : An app that was installed later
[TactilityC]
note right of TactilityC : optional wrapper to expose Tactility a C code\nC++ is not supported here. 
[Tactility]
note right of Tactility : facilitate apps, services and HAL
[TactilityCore]
note right of TactilityCore : IO, data types, logging, async, etc.

[InternalApp] ..> [Tactility]
[ExternalApp] ..> [TactilityC]
[TactilityC] ..> [Tactility]
[Tactility] ..> [TactilityCore]

@enduml
```
