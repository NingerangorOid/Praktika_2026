classDiagram
    %% Определение перечисления
    class FailureType {
        <<enumeration>>
        UnexpectedShutdown = 0
        ShortNonResponding = 1
        HardwareFailures = 2
        ConnectionProblems = 3
    }

    %% Определение классов
    class Device {
        +int Id
        +string Name
        +Device(int id, string name)
    }

    class Failure {
        +int DeviceId
        +FailureType FailureType
        +DateTime Date
        +Failure(int deviceId, FailureType failureType, DateTime date)
        +IsSerious() bool
    }

    class ReportMaker {
        +FindDevicesFailedBeforeDate(DateTime targetDate, List~Failure~ failures, List~Device~ devices)$ List~string~
        +FindDevicesFailedBeforeDateObsolete(int day, int month, int year, int[] failureTypes, int[] deviceId, object[][] times, List~Dictionary~string_object~~ devices)$ List~string~
    }

    %% Определение связей и зависимостей
    Failure --> FailureType : Содержит тип (FailureType)
    ReportMaker ..> Device : Зависит/Использует в методах
    ReportMaker ..> Failure : Зависит/Использует в методах
