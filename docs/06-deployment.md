# Deployment and CI/CD

## Setting up Continuous Integration with GitLab

AppCertificates
Heroku
Web
Swift Package
App 

App -> run fastlane
  contains SPM
    contains Web
    
Heroku mirrors SPM with Web


Poly Repo
Submodules
Fastlane

```mermaid
flowchart TD
    A[("AppStoreCertificates")] == Contains ==> A1("Provisioning Profiles") & A2("Certificates")
    F("Fastlane (Match)") -. Uses .-> J("Fastlane Configuration") & A
    B[("Web")] -- Contains --> G1("TypeScript") & G2("Vite") & G3("TailwindCSS") & G4("VueJS")
    C[("GBeatKit")] -- Contains --> H("Swift Package")
    C -. Submodule of .-> B
    D[("SwiftApp")] == Contains ==> I("Xcode Project Setup") & J
    D -. Uses .-> F
    D -. Submodule of .-> C
    E[("Heroku")] == Mirrors ==> C & B
    F -- Deploys to --> N["App Store"]
    E -- Deploys to --> O["Heroku Platform"]
     A:::repository
     B:::repository
     C:::repository
     D:::repository
     E:::repository
     N:::deployment
     O:::deployment
    classDef repository fill:#f9f,stroke:#333,stroke-width:2px
    classDef deployment fill:#bfb,stroke:#333,stroke-width:2px
```

```mermaid
sequenceDiagram
    participant Web
    participant GBeatKit
    participant SwiftApp
    participant Heroku Repo
    participant AppStore
    participant Beta Heroku
    participant Prod Heroku

    Web->>Web: Build website
    GBeatKit->>GBeatKit: Build on macOS
    GBeatKit->>GBeatKit: Build on Linux
    alt Deployment triggered for Web
        GBeatKit->>Web: Pull latest code
        GBeatKit->>Heroku: Push Web code
        Heroku->>BetaInstance: Deploy
        alt Production deployment
            Heroku->>ProdInstance: Deploy
        end
    end
    SwiftApp->>SwiftApp: Archive iOS app
    alt Deployment triggered for iOS
        SwiftApp->>AppStore: Push archived app
    end
```

## Setting up Continuous Integration with GitHub



```mermaid
flowchart LR
 subgraph Parallel["In Parallel"]
        A["Build Web FrontEnd"]
        C["Build on Ubuntu"]
  end
    Parallel --> B["Build on macOS"]
    B --> D["Linting on macOS"]
    D --> E["Archiving and/or Deliver"]
    A -.- note1[/"builds the Front End Website<br>contains TypeScript, TailwindCSS,<br>VueJS, and Vite"/]
    C -.- note2[/"builds the Swift Package on Ubuntu<br>to ensure it'll work on a server"/]
    B -.- note3[/"ensures the build works for<br>the iPhone and Watch app"/]
    D -.- note4[/"ensures the code is formatted properly<br>and follows consistent styling"/]
    E -.- note5[/"ensures the app can be archived and<br>if necessary gets uploaded to the App Store"/]
    E -.- note6[/"If everthing passes based on branch, Heroku auto-deploys to instance."/]
     note1:::note
     note2:::note
     note3:::note
     note4:::note
     note5:::note
    classDef note stroke-dasharray: 2 4,stroke-opacity: 0.6, fill-opacity: 0.75
    linkStyle 3,4,5,6,7 opacity: 0.60
```

## Deploying to Heroku

### Setting up Build Packs

### 
    1. Build Packs
      1. Vapor
      2. JS