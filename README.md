# varo-report-email
This is an html file of how a varo report could look if html can be passed to gmail.

@startuml
'https://plantuml.com/class-diagram

frame "Domain" {
    class DataCollectorInteractor
    class DeviceInteractor
    class EmailInteractor
    class ReportsInteractor
    class HistoryInteractor
}

frame "UI" {
    class FragmentStep
}
FragmentStep --> DataCollectorInteractor
FragmentStep <.. DataCollectorInteractor

@enduml

@startuml
'https://plantuml.com/component-diagram'

frame "Data" {
    component Repo
    database "Database"
}
frame "Domain" {
    component Interactor
}
frame "UI" {
    component Activity
    component Fragment
    component Dialog
}

note right of Fragment {
    UI elements subscribe
    to reactive view model
    changes and data
    changing update calls
}

note top of Interactor {
    Core of the application.
    Provides reactive view
    model subscriptions and
    data update methods.

    Everything the application
    "does" happens here.
}

note top of Repo {
    Manages access to
    storage (database/filesystem
    /web if needed)
    and surfaces subscription
    updates for when data changes
}

Activity -> Interactor
Activity <. Interactor
Fragment -> Interactor
Fragment <. Interactor
Dialog -> Interactor
Dialog <. Interactor

Interactor -> Repo
Interactor <. Repo

Repo -> "Database"
Repo <. "Database"

Fragment -[hidden]-> Activity
Activity -[hidden]-> Dialog

@enduml

@startuml
'https://plantuml.com/use-case-diagram

node "mobile device" {
    rectangle "email app" {
        (edit) as (email)
    }
    rectangle "Varo" {
        (manual report) as (manual)
        (logger report) as (logger)

        (create report) as (report)
        (send report bundle) as (send)
        (View past reports) as (history)
    }
}
node "temperature logger" {
    (connect logger to device over usb) as (connect)
    file "temp data"
}
cloud "email server"
(report) <|-l- (logger)
(report) <|-l- (manual)

(report) <-- User
(send) <-- User
(history) <-- User

User -d-> (connect)

(connect) .. "temp data"
'"30 DTR data" .u.> report
(send) ..> email
(email) -d-> "email server"
"temp data" .u.> logger

(report) -[hidden]d- (send)
(send) -[hidden]d- (history)
(manual) -[hidden]d- (logger)

@enduml
