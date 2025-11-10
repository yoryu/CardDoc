#AAA

flowchart TB
  %% Top row: parallel Web clients
  subgraph Webs
    direction LR
    web_gen["Web (generic)"]
    web_xxx["Web - XXX"]
    web_yyy["Web - YYY"]
  end

  %% Second row: parallel API services
  subgraph APIs
    direction LR
    api_gen["API (generic)"]
    api_xxx["API - XXX"]
    api_yyy["API - YYY"]
  end

  %% CardNet container with internal layers and internal label
  subgraph CardNetBox["CardNet"]
    direction TB

    %% COM row (common + business-specific impls)
    subgraph COMrow["CardNet.COM"]
      direction LR
      COM_common["COM - common"]
      COM_xxx["COM - XXX impl"]
      COM_yyy["COM - YYY impl"]
    end

    %% SYS row (only SQL Adapter now)
    subgraph SYSrow["CardNet.SYS"]
      direction LR
      SYS_sql["SQL Adapter"]
    end

    %% CORE (wider, foundational) and BASE
    CORE["CardNet.Core"]
    BASE["CardNet.Base"]
  end

  %% Keep external flows visually present but minimal: Web -> API
  web_gen --> api_gen
  web_xxx --> api_xxx
  web_yyy --> api_yyy

  %% Map APIs to corresponding COM elements (keeps intent clear)
  api_gen --> COM_common
  api_xxx --> COM_xxx
  api_yyy --> COM_yyy

  %% Only internal CardNet reference lines (one-directional)
  COM_common --> SYS_sql
  COM_xxx --> SYS_sql
  COM_yyy --> SYS_sql

  SYS_sql --> CORE
  CORE --> BASE
