flowchart TB
    addPlop --> initPlop
    addPlopNow -.-> preRerunValidation -.-> initPlop
    addPlopNow --> initPlop
    initPlop --> triggerPlop
    triggerPlop -.-> startPlop
    startPlop --> callInit --> callPrePlops --> callAction --> plopAction --> actionDone
    callInit -.-> onInit -.-> callPrePlops
    callPrePlops -.-> prePlopRunner -.-> callAction
    actionDone --> softRetry --> onResolved --> resolved
    softRetry -.-> startPlop
    plopAction --> errorRetry -.-> startPlop
    errorRetry --> |limit| onRejected --> rejected
    prePlopRunner -.-> errorRetry
    onInit -.-> errorRetry
    preRerunValidation -.-> errorRetry


    subgraph startState ["Start Plop"]
        addPlop("Run")
        addPlopNow("Re-run")
    end

    subgraph initState ["Init Plop"]
        initPlop["Initialize"]
        triggerPlop["🕑Trigger"]
    end

    subgraph onBoxRetry ["&nbsp;"]
        softRetry{"🗘Retry?"}
        errorRetry{"🗘Error Retry?"}
    end

    subgraph runState ["Run Plop"]
        startPlop["Start"]
        callInit["call Init"]
        callPrePlops["call PrePlops"]
        callAction["call Action"]
        actionDone["Action done"]
        onBoxRetry
    end

    subgraph doneState ["Plop Done"]
       resolved["Resolved"]
       rejected["Rejected"]
    end

    subgraph onBoxSolve ["&nbsp;"]
      onResolved(["onResolved"])
        onRejected(["onRejected"])
    end

    subgraph plopFunc ["Plop instance"]
        preRerunValidation(["preRerunValidation"])
        onInit(["onInit"])
        prePlopRunner(["pre-plops[]"])
        plopAction(["Action"])
        onBoxSolve
    end

    subgraph plopRunner ["Plop Runner"]
        startState
        initState
        runState
        doneState
    end

    style preRerunValidation stroke-dasharray: 5 5
    style prePlopRunner stroke-dasharray: 5 5
    style onInit stroke-dasharray: 5 5
    style softRetry stroke-dasharray: 5 5
    style onResolved stroke-dasharray: 5 5
    style onRejected stroke-dasharray: 5 5
    style rejected stroke:#f00,stroke-width:2px
    style resolved stroke:#0f0,stroke-width:2px
    style addPlop stroke:#0ff,stroke-width:2px
    style addPlopNow stroke:#0ff,stroke-width:2px
    style plopRunner fill:#222
    style plopFunc fill:#232
    style onBoxSolve fill:#232,stroke-width:0
    style onBoxRetry stroke-width:0
    linkStyle 0,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18 stroke:#0f0,stroke-width:2px;
    linkStyle 19,20,21,22,23,24,25 stroke:#f00,stroke-width:2px;

