# Document Status
The Document Status section is used to display the current status of the design, the options are: Draft, Ready for Review, Approved)


|Document Status|`Approved`|
|--|--|


# Revision Requests
The revisions requests section is used to track requests changes to the design.

| Date | Section | Requested Change |  Authors Comments|
|--|--|--|--|
|  |  |  |  |
|  |  |  |  |
|  |  |  |  |

# Version Control

The version control section is used to capture design modifications.


|Date|Version| Author |Section Changed | Reason for Change|
|--|--|--|--|--|
| 30/06/2022 |1.0  |Dermot Reynolds  |Approval  |Approved by TDA - under name "Policy Archive"  |
| 13/07/2022 |1.1  |Shivraj Chari  |TO-BE Details  |Corrected subnet and suSRCription names  |
| 18/07/2022 |1.2  |Shivraj Chari  |TO-BE Details  |Added Subnet details  |
| 22/08/2022 |1.3  |Shivraj Chari  |TO-BE Details  |Updated Subnet details  |
| 11/09/2022 |1.4  |Sathish Balasubramani |TO-BE Details  |Updated User Access details  |




# Scope
The scope section defines the scope of the design.

- FinCo Archive Application Only.

# Dependencies
The dependencies will call out any known upstream or downstream dependencies.

- Core Infrastructure

# Methodology

Given the volume of data - 17TB - and the different services we should take a different approach to how we deploy the infrastructure for this application.

Below is the proposed approach.

``` mermaid

graph LR

    Device0([TDA Approve UAT]) --> Device1([Build UAT]) --> Device2([Test Migration Methods*]) --> Device3([Refine Architecture]) --> Device4([Present To TDA]) --> Device5([Build PROD]) --> Device6([Test/Migrate])

    style Device0 fill:#90EE90,stroke:black,stroke-width:1px , stroke-dasharray: 2
    style Device1 fill:#90EE90,stroke:black,stroke-width:1px , stroke-dasharray: 2
    style Device2 fill:#90EE90,stroke:black,stroke-width:1px , stroke-dasharray: 2
    style Device3 fill:#90EE90,stroke:black,stroke-width:1px , stroke-dasharray: 2
    style Device4 fill:#90EE90,stroke:black,stroke-width:1px , stroke-dasharray: 2
    style Device5 fill:#90EE90,stroke:black,stroke-width:1px , stroke-dasharray: 2        
    style Device6 fill:#90EE90,stroke:black,stroke-width:1px , stroke-dasharray: 2   
    
```

*Including mechanisms to reduce cost/complexity e.g. removal of unwanted containers, consolidation etc.

# Solution Overview
The solution overview section will introduce the design, this is a single paragraph  describing the Why (requirement) and the How (design) 

## 1. Background

|Element|Value|
|-------|-----|
|Business|Group Functions|
|Application Name| Group Archive|
|Business Process| Management Information|
|Description|Stores archive data from a variety of FinCo Policy Administration Systems|
|Application Short Code| arcv|
|Business Short Code| gfn|
|Environment|Production|
|Support|3rd Party, 3rdParty|
|Business Owner| TBC|
|FinCo SME| sme@3rdParty.com |
|Support Contact| sme@3rdParty.com |
|NIP Trajectory| `Move` |




## 2. Production AS-IS

### Key Features
1. Flat network
2. No transit firewall
3. No perimiter firewall on PaaS - which is the majority of the services within the application
4. Perimiter firewall on IaaS
5. Confusing naming conventions - e.g. production services in UAT resource group
6. Comingling of production and non-production services

``` mermaid
graph TD
    subgraph Device0000[Key]
        Device1000((A)) --> |Network Connection|Device1001((B))
        Device1002((C)) -.-> |Application Connection|Device1003((D))
        USR1000((Users))-->Device1004[Device]-->VNET1000((vNet))
    end

    subgraph Group2[Consumers]
        USR0([Users])
        USR1([Users])
        USR2([Scanner Users])
        USR3([Developer])
        %%USR4([Various])
    end


    USR0 -.->Device7
    USR1 -.->Device11
    USR2 -.->Device5
    USR3 -.->Device12

    subgraph Group6[FinCo Archive]

        subgraph Group0[Great Plains & Complaints - AZ-RG-UAT-SRC001 ]
            
            Device7[ServicePlan6755327f-8868/web-uat-SRC001]
        end
            BadCallout0([No Firewall]) --> Device7
            BadCallout0([No Firewall]) --> Device11
            BadCallout0([No Firewall]) --> Device9

            Device5[S-UAT-APP300 - APG]
            


            Device7 -.-> Device9
            Device7 -.-> Device8
            Device7 -.-> Device10

        subgraph Group1[Advisory Archive & FISH - AZ-RG-TG-SRC002]
            
            Device11[ServicePlan6755327f-8869/web-prod-SRC001]
        end
            Device9[(archivalprod-cosmos)]
            Device8[cosmosdbsearchnew]
            Device10[azsaprodSRC001]
            Device11 -.-> Device9
            Device11 -.-> Device8
            Device7 -.-> Device10
            Device11 -.-> Device10

        
            Device12[W-UAT-W1060]
        

        %%Device12 --> Device1
        %%Device12 --> Device2
        %%Device12 --> Device3
        %%Device12 --> Device4

        Device12 -.-> Device9
        Device12 -.-> Device8
        Device12 -.-> Device10      
        Device12 -.-> |release|Device7
        Device12 -.-> |release|Device11          


        subgraph Group5[Wider FinCo]
            
        end

        %%Device14 --> |scan to pdf|Device5
        Device5 -.-> |smb share|Device12
        
    end

        Device12 -.-> |adhoc pull|Device14
    
    subgraph Group3[Wider FinCo]
        Device14[Various]
    end


    %%Device0-->|Runs On|Device2
    %%Device1-->|Runs On|Device2
    %%Device3-->|Connected To|Device2
    %%Device3-->|Connected To|VNET1
    %%Device5-->VNET1
    %%Device6-->Device7


    %%Device6["prd-foundations.yaml"]


    class External1 ExternalClass

    classDef ExternalClass fill:black,stroke:grey, stroke-width:2px , font-size: 100%, color: white

    %%linkStyle 6 stroke-width:2px,fill:none,stroke:red;


    %%style Label0 fill:black,color:white, stroke:black

    style Group0 fill:#FFFFFF,stroke:#333,stroke-width:1px, stroke-dasharray: 2
    style Group1 fill:#FFFFFF,stroke:#333,stroke-width:1px, stroke-dasharray: 2
    style Group1 fill:#FFFFFF,stroke:#333,stroke-width:1px, stroke-dasharray: 2
    style Group2 fill:#FFFFFF,stroke:#333,stroke-width:1px, stroke-dasharray: 2    
    style Group3 fill:#FFFFFF,stroke:#333,stroke-width:1px, stroke-dasharray: 2    
    %%style Group4 fill:#FFFFFF,stroke:#333,stroke-width:1px, stroke-dasharray: 2
    style Group5 fill:#FFFFFF,stroke:#333,stroke-width:1px, stroke-dasharray: 2               
    style Group6 fill:#FFFFFF,stroke:#333,stroke-width:1px, stroke-dasharray: 2




    %%style Device0 fill:white,stroke:#333,colour: white, stroke-width:1px , stroke-dasharray: 2
    %%style Device1 fill:white,stroke:#333,colour: white, stroke-width:1px , stroke-dasharray: 2
    %%style Device2 fill:white,stroke:#333,colour: white, stroke-width:1px , stroke-dasharray: 2
    %%style Device3 fill:white,stroke:#333,colour: white, stroke-width:1px , stroke-dasharray: 2
    %%style Device4 fill:white,stroke:#333,colour: white, stroke-width:1px , stroke-dasharray: 2
    style Device5 fill:white,stroke:#333,colour: white, stroke-width:1px , stroke-dasharray: 2
    %%style Device6 fill:white,stroke:#333,colour: white, stroke-width:1px , stroke-dasharray: 2
    style Device7 fill:white,stroke:#333,colour: white, stroke-width:1px , stroke-dasharray: 2
    style Device8 fill:white,stroke:#333,colour: white, stroke-width:1px , stroke-dasharray: 2
    style Device9 fill:white,stroke:#333,colour: white, stroke-width:1px , stroke-dasharray: 2
    style Device10 fill:white,stroke:#333,colour: white, stroke-width:1px , stroke-dasharray: 2
    style Device11 fill:white,stroke:#333,colour: white, stroke-width:1px , stroke-dasharray: 2
    style Device12 fill:white,stroke:#333,colour: white, stroke-width:1px , stroke-dasharray: 2
    %%style Device13 fill:white,stroke:#333,colour: white, stroke-width:1px , stroke-dasharray: 2
    style Device14 fill:white,stroke:#333,colour: white, stroke-width:1px , stroke-dasharray: 2
    %%style Device15 fill:white,stroke:#333,colour: white, stroke-width:1px , stroke-dasharray: 2             
    %%style InfoCallout0 fill:yellow	,stroke:#333,stroke-width:1px , stroke-dasharray: 2
    %%style InfoCallout1 fill:black	, color: white, stroke:#333,stroke-width:1px , stroke-dasharray: 2
    %%style GoodCallout0 fill:#90EE90	,stroke:#333,stroke-width:1px , stroke-dasharray: 2   

    %%style GoodCallout1 fill:#90EE90	,stroke:#333,stroke-width:1px , stroke-dasharray: 2   

    %%style GoodCallout3 fill:#90EE90	,stroke:#333,stroke-width:1px , stroke-dasharray: 2   
    

    %%style Location0 fill:#F6F7F7,stroke:#0352FC,stroke-width:2px

    %%style Device0 fill:white,stroke:#333,colour: white, stroke-width:1px , stroke-dasharray: 2
    %%style Group0 fill:#FFFFFF,stroke:#333,stroke-width:1px, stroke-dasharray: 2
    %%style Location0 fill:#F6F7F7,stroke:#0352FC,stroke-width:2px
    %%style WAN0 fill:#F6F7F7,stroke:#0352FC,stroke-width:2px
    %%style SUBS0 fill:#F6F7F7,stroke:#0352FC,stroke-width:2px
        
    %%style VNET0 fill:#CDD2D6	,stroke:#333,stroke-width:1px , stroke-dasharray: 2
    %%style VNET1 fill:#CDD2D6	,stroke:#333,stroke-width:1px , stroke-dasharray: 2
    %%style VNET2 fill:#CDD2D6	,stroke:#333,stroke-width:1px , stroke-dasharray: 2    
    %%style VNET3 fill:#CDD2D6	,stroke:#333,stroke-width:1px , stroke-dasharray: 2 
    %%style VNET4 fill:#CDD2D6	,stroke:#333,stroke-width:1px , stroke-dasharray: 2        

    %%style GoodCallout0 fill:#90EE90	,stroke:#333,stroke-width:1px , stroke-dasharray: 2
    style BadCallout0 fill:#FF5733	,stroke:#333,stroke-width:1px , stroke-dasharray: 2
    %% style BadCallout1 fill:#FF5733	,stroke:#333,stroke-width:1px , stroke-dasharray: 2
    style USR0 fill:#F6F7F7,stroke:#0352FC,stroke-width:2px
    style USR1 fill:#F6F7F7,stroke:#0352FC,stroke-width:2px
    style USR2 fill:#F6F7F7,stroke:#0352FC,stroke-width:2px 
    style USR3 fill:#F6F7F7,stroke:#0352FC,stroke-width:2px   

    %%style UnsureCallout0 fill:#FFBB33	,stroke:#333,stroke-width:1px , stroke-dasharray: 2
    %%style InfoCallout0 fill:yellow	,stroke:#333,stroke-width:1px , stroke-dasharray: 2
  
    %%style VNET1 fill:#CDD2D6	,stroke:#333,stroke-width:1px , stroke-dasharray: 2  

    %%Index
    style Device1000 fill:#FFFFFF,stroke:#333,stroke-width:1px, stroke-dasharray: 2
    style Device1001 fill:#FFFFFF,stroke:#333,stroke-width:1px, stroke-dasharray: 2
    style Device1002 fill:#FFFFFF,stroke:#333,stroke-width:1px, stroke-dasharray: 2
    style Device1003 fill:#FFFFFF,stroke:#333,stroke-width:1px, stroke-dasharray: 2
    style Device1004 fill:#FFFFFF,stroke:#333,stroke-width:1px, stroke-dasharray: 2
    style Device0000 fill:#FFFFFF,stroke:#333,stroke-width:1px, stroke-dasharray: 2
    %% style KeyDevice1000 fill:white,stroke:#90EE90,colour: white, stroke-width:1px , stroke-dasharray: 0
    style USR1000 fill:#F6F7F7,stroke:#0352FC,stroke-width:2px
    style VNET1000 fill:#CDD2D6	,stroke:#333,stroke-width:1px , stroke-dasharray: 2  


```

## 3. Non Production AS-IS

``` mermaid
graph LR
    subgraph Group3[AZ-RG-UAT-SRC001 - Development]
        Device0[(cosmosdb-cosmos)]
        Device1[cosmosdbsearch]
        Device2[azsauatSRC001 - unused]
        Device3[azsauatSRC002]
        Device4[azsauatSRC003]
        Device6[W-UAT-WIN - decom]
    end
```

## 4. Production TO-BE - Segmented Network

### Key Features

1. Segmented network
2. Transit firewall
3. Perimiter firewall on PaaS - which is the majority of the services within the application
4. Perimiter firewall on IaaS 
5. Communication over private network leveraging private endpoints
6. Alignment to naming conventions
7. Split of production and non-production into different suSRCriptions



``` mermaid
graph LR

    subgraph Group2[Consumers]
        USR0([Users])
        USR2([Scanner Users])
        USR3([Developers])
        
    end


    %%USR0 -.->Device7
    %%USR1 -.->Device11
    %%USR2 -.->  Device5
    %%USR3 -.-> Device12

    USR0 -->  Device1
    %%USR1 -->  Device0
    USR2 -->  Device1
    USR3 -->  Device2
    

    %%Device0 --> Device5
    %%Device0 --> Device12

    subgraph Group8[Transit]
        subgraph VNET0[VNET-TRANSIT]
            GoodCallout0([All traffic flows through the transit firewall]) -->Device0
            Device14((Peer))
            Device0[Azure Fw]
            Device1[SDWAN/Meraki/Zscaler]
            %%Device1-->Device0
        end
    end
    Device14 -->Device13
    Device15 --> Device14
    Device16 --> Device14
    Device20 --> Device14

    subgraph Group9[GINF]
        subgraph VNET1[VNET-GINF]
            Device16((Peer))
            Device2[CyberArk]
        end
    end
        %%Device2-->Device0

    subgraph Group10[Business SuSRCriptions]
        subgraph VNET2[VNET-BUSINESS]
            Device15((Peer))
            Device3[Legacy Services]
        end
    end
    %%Device3-->Device0

    subgraph Group6[Prod Subscription]

        subgraph VNET3[Prod vNet]
            Device13((Peer))
            subgraph Group4[Web Subnet]
                    Device7[Prod App Service]

            end

            subgraph Group7[App Subnet]
                Device5[Prod Ingestion Server]
            end


                %%Device7 -.-> Device9
                %%Device7 -.-> Device8
                %%Device7 -.-> Device10

            subgraph Group5[Data Subnet]
                Device9[(Prod Cosmos Instance)]
                Device8[Prod Search Instance]
                Device10[Prod Storage Instance]
            end
        end

        
    end

    subgraph Group11[UAT Subscription]

        subgraph VNET4[UAT vNet]
            Device20((Peer))
            subgraph Group12[UAT Web Subnet]
                Device21[UAT App Service]
            end

            subgraph Group13[UAT App Subnet]
                Device22[UAT Ingestion Server]
            end

            subgraph Group14[UAT Data Subnet]
                Device23[(UAT Cosmos Instance)]
                Device24[UAT Search Instance]
                Device25[UAT Storage Instance]
            end
        end

        
    end
        %%Device12 -.-> |adhoc pull|Device3
    
    %%subgraph Device0000[Key]
    %%    Device1000((A)) --> |Network Connection|Device1001((B))
    %%    Device1002((C)) -.-> |Application Connection|Device1003((D))
    %%    USR1000((Users))-->Device1004[Device]-->VNET1000((vNet))
    %%end


    %%Device0-->|Runs On|Device2
    %%Device1-->|Runs On|Device2
    %%Device3-->|Connected To|Device2
    %%Device3-->|Connected To|VNET1
    %%Device5-->VNET1
    %%Device6-->Device7


    %%Device6["prd-foundations.yaml"]


    class External1 ExternalClass

    classDef ExternalClass fill:black,stroke:grey, stroke-width:2px , font-size: 100%, color: white

    %%linkStyle 6 stroke-width:2px,fill:none,stroke:red;


    %%style Label0 fill:black,color:white, stroke:black

    %%style Group0 fill:#FFFFFF,stroke:#333,stroke-width:1px, stroke-dasharray: 2
    %%style Group1 fill:#FFFFFF,stroke:#333,stroke-width:1px, stroke-dasharray: 2
    %%style Group1 fill:#FFFFFF,stroke:#333,stroke-width:1px, stroke-dasharray: 2
    style Group2 fill:#FFFFFF,stroke:#333,stroke-width:1px, stroke-dasharray: 2    
    %%style Group3 fill:#FFFFFF,stroke:#333,stroke-width:1px, stroke-dasharray: 2    
    style Group4 fill:#FFFFFF,stroke:#333,stroke-width:1px, stroke-dasharray: 2
    style Group5 fill:#FFFFFF,stroke:#333,stroke-width:1px, stroke-dasharray: 2               
    style Group6 fill:#FFFFFF,stroke:#333,stroke-width:1px, stroke-dasharray: 2
    style Group7 fill:#FFFFFF,stroke:#333,stroke-width:1px, stroke-dasharray: 2
    style Group8 fill:#FFFFFF,stroke:#333,stroke-width:1px, stroke-dasharray: 2
    style Group9 fill:#FFFFFF,stroke:#333,stroke-width:1px, stroke-dasharray: 2
    style Group10 fill:#FFFFFF,stroke:#333,stroke-width:1px, stroke-dasharray: 2

    style Group11 fill:#FFFFFF,stroke:#333,stroke-width:1px, stroke-dasharray: 2
    style Group12 fill:#FFFFFF,stroke:#333,stroke-width:1px, stroke-dasharray: 2    
    style Group13 fill:#FFFFFF,stroke:#333,stroke-width:1px, stroke-dasharray: 2        
    style Group14 fill:#FFFFFF,stroke:#333,stroke-width:1px, stroke-dasharray: 2        

    style Device0 fill:white,stroke:#333,colour: white, stroke-width:1px , stroke-dasharray: 2
    style Device1 fill:white,stroke:#333,colour: white, stroke-width:1px , stroke-dasharray: 2
    style Device2 fill:white,stroke:#333,colour: white, stroke-width:1px , stroke-dasharray: 2
    style Device3 fill:white,stroke:#333,colour: white, stroke-width:1px , stroke-dasharray: 2
    %%style Device4 fill:white,stroke:#333,colour: white, stroke-width:1px , stroke-dasharray: 2
    style Device5 fill:white,stroke:#333,colour: white, stroke-width:1px , stroke-dasharray: 2
    %%style Device6 fill:white,stroke:#333,colour: white, stroke-width:1px , stroke-dasharray: 2
    style Device7 fill:white,stroke:#333,colour: white, stroke-width:1px , stroke-dasharray: 2
    style Device8 fill:white,stroke:#333,colour: white, stroke-width:1px , stroke-dasharray: 2
    style Device9 fill:white,stroke:#333,colour: white, stroke-width:1px , stroke-dasharray: 2
    style Device10 fill:white,stroke:#333,colour: white, stroke-width:1px , stroke-dasharray: 2
    %%style Device11 fill:white,stroke:#333,colour: white, stroke-width:1px , stroke-dasharray: 2
    %%style Device12 fill:white,stroke:#333,colour: white, stroke-width:1px , stroke-dasharray: 2
    style Device13 fill:white,stroke:#333,colour: white, stroke-width:1px , stroke-dasharray: 2
    style Device14 fill:white,stroke:#333,colour: white, stroke-width:1px , stroke-dasharray: 2
    style Device15 fill:white,stroke:#333,colour: white, stroke-width:1px , stroke-dasharray: 2
    style Device16 fill:white,stroke:#333,colour: white, stroke-width:1px , stroke-dasharray: 2  
    style Device20 fill:white,stroke:#333,colour: white, stroke-width:1px , stroke-dasharray: 2   
    style Device21 fill:white,stroke:#333,colour: white, stroke-width:1px , stroke-dasharray: 2                    
    style Device22 fill:white,stroke:#333,colour: white, stroke-width:1px , stroke-dasharray: 2                        
    style Device23 fill:white,stroke:#333,colour: white, stroke-width:1px , stroke-dasharray: 2                        
    style Device24 fill:white,stroke:#333,colour: white, stroke-width:1px , stroke-dasharray: 2                    
    style Device25 fill:white,stroke:#333,colour: white, stroke-width:1px , stroke-dasharray: 2                        

    %%style InfoCallout0 fill:yellow	,stroke:#333,stroke-width:1px , stroke-dasharray: 2
    %%style InfoCallout1 fill:black	, color: white, stroke:#333,stroke-width:1px , stroke-dasharray: 2
    style GoodCallout0 fill:#90EE90	,stroke:#333,stroke-width:1px , stroke-dasharray: 2   

    %%style GoodCallout1 fill:#90EE90	,stroke:#333,stroke-width:1px , stroke-dasharray: 2   

    %%style GoodCallout3 fill:#90EE90	,stroke:#333,stroke-width:1px , stroke-dasharray: 2   
    

    %%style Location0 fill:#F6F7F7,stroke:#0352FC,stroke-width:2px

    %%style Device0 fill:white,stroke:#333,colour: white, stroke-width:1px , stroke-dasharray: 2
    %%style Group0 fill:#FFFFFF,stroke:#333,stroke-width:1px, stroke-dasharray: 2
    %%style Location0 fill:#F6F7F7,stroke:#0352FC,stroke-width:2px
    %%style WAN0 fill:#F6F7F7,stroke:#0352FC,stroke-width:2px
    %%style SUBS0 fill:#F6F7F7,stroke:#0352FC,stroke-width:2px
        
    style VNET0 fill:#CDD2D6	,stroke:#333,stroke-width:1px , stroke-dasharray: 2
    style VNET1 fill:#CDD2D6	,stroke:#333,stroke-width:1px , stroke-dasharray: 2
    style VNET2 fill:#CDD2D6	,stroke:#333,stroke-width:1px , stroke-dasharray: 2    
    style VNET3 fill:Green	,stroke:#333,stroke-width:1px , stroke-dasharray: 2 
    style VNET4 fill:Green	,stroke:#333,stroke-width:1px , stroke-dasharray: 2        

    %%style GoodCallout0 fill:#90EE90	,stroke:#333,stroke-width:1px , stroke-dasharray: 2
    %%style BadCallout0 fill:#FF5733	,stroke:#333,stroke-width:1px , stroke-dasharray: 2
    style USR0 fill:#F6F7F7,stroke:#0352FC,stroke-width:2px
    %%style USR1 fill:#F6F7F7,stroke:#0352FC,stroke-width:2px
    style USR2 fill:#F6F7F7,stroke:#0352FC,stroke-width:2px 
    style USR3 fill:#F6F7F7,stroke:#0352FC,stroke-width:2px   

    %%style UnsureCallout0 fill:#FFBB33	,stroke:#333,stroke-width:1px , stroke-dasharray: 2
    %%style InfoCallout0 fill:yellow	,stroke:#333,stroke-width:1px , stroke-dasharray: 2
  
    %%style VNET1 fill:#CDD2D6	,stroke:#333,stroke-width:1px , stroke-dasharray: 2  

    %%Index
    %%style Device1000 fill:#FFFFFF,stroke:#333,stroke-width:1px, stroke-dasharray: 2
    %%style Device1001 fill:#FFFFFF,stroke:#333,stroke-width:1px, stroke-dasharray: 2
    %%style Device1002 fill:#FFFFFF,stroke:#333,stroke-width:1px, stroke-dasharray: 2
    %%style Device1003 fill:#FFFFFF,stroke:#333,stroke-width:1px, stroke-dasharray: 2
    %%style Device1004 fill:#FFFFFF,stroke:#333,stroke-width:1px, stroke-dasharray: 2
    %%style Device0000 fill:#FFFFFF,stroke:#333,stroke-width:1px, stroke-dasharray: 2
    %% style KeyDevice1000 fill:white,stroke:#90EE90,colour: white, stroke-width:1px , stroke-dasharray: 0
    %%style USR1000 fill:#F6F7F7,stroke:#0352FC,stroke-width:2px
    %%style VNET1000 fill:#CDD2D6	,stroke:#333,stroke-width:1px , stroke-dasharray: 2  

```

## 5. AS-IS / TO-BE Detail (All Environments)

### 5.1 AS-IS/PROD

|               |AS-IS - PROD          |AS-IS - PROD        |AS-IS - PROD     |AS-IS - PROD            |AS-IS - PROD     |AS-IS - PROD            |AS-IS - PROD     |AS-IS - PROD        |AS-IS - PROD        |
|---------------|----------------------|--------------------|-----------------|------------------------|-----------------|------------------------|-----------------|--------------------|--------------------|
|Name           |archivalprod-cosmos   |cosmosdbsearchnew|azsaprodSRC001   |ServicePlan6755327f-8869|web-prod-SRC001  |ServicePlan6755327f-8868|web-uat-SRC001   |S-UAT-APP300        |W-UAT-W1060         |
|Description    |Data store            |Document search     |Data store       |App Service plan        |App Service      |App Service plan        |App Service      |Apogee pdf generator|Scheduler           |
|SuSRCription   |AZ-TG-PROD-SUB001     |AZ-TG-PROD-SUB001   |AZ-TG-PROD-SUB001|AZ-TG-PROD-SUB001       |AZ-TG-PROD-SUB001|AZ-TG-UAT-SUB001        |AZ-TG-UAT-SUB001 |AZ-TG-UAT-SUB001    |AZ-TG-UAT-SUB001    |
|Resource Group |AZ-RG-TG-SRC002       |AZ-RG-TG-SRC002     |AZ-RG-TG-SRC002  |AZ-RG-TG-SRC002         |AZ-RG-TG-SRC002  |AZ-RG-UAT-SRC001        |AZ-RG-UAT-SRC001 |AZ-RG-UAT-SRC001    |AZ-RG-UAT-ClockHouse|
|Address        |(1)                   |(2)                 |(3)              |n/a                     |(4)              |n/a                     |n/a              |n/a                 |n/a                 |
|Type           |Azure Cosmos          |Search service      |Storage account  |App Service plan        |App Service      |App Service plan        |App Service      |Virtual machine     |Virtual machine     |
|Total Data     |180 GB                |3rep x 6part, 150GB |13.7 TB          |B2                      |n/a              |B2                      |n/a              |300GB               |1000GB              |
|Sku            |Provisioned throughput|Standard            |GRS,Standard     |Basic                   |n/a              |Basic                   |n/a              |Standard F4         |Standard D4s v3     |
|IP             |n/a                   |n/a                 |n/a              |n/a                     |n/a              |n/a                     |n/a              |10.60.27.66         |10.60.4.26          |
|vNet           |n/a                   |n/a                 |n/a              |n/a                     |n/a              |n/a                     |n/a              |AZ-VN-UAT-VNET001   |AZ-VN-UAT-VNET001   |
|Subnet         |n/a                   |n/a                 |n/a              |n/a                     |n/a              |n/a                     |n/a              |AZ-SN-UAT-COR-SPL001|AZ-SN-UAT-CLT-SPL003|
|Internet Access|Yes                   |Yes                 |Yes              |n/a                     |Yes              |n/a                     |Yes              |No                  |No                  |




(1) https://archivalprod-cosmos.documents.azure.com:443/
(2) https://cosmosdbsearchnew.search.windows.net
(3) https://azsaprodSRC001.blob.core.windows.net/
(4) https://web-prod-SRC001.azurewebsites.net

##5.2 New Subnet Details
|Environment|Name|vNet|Subnet Range|
|-----------|----|----|------------|
|Prod|snet-gfn-p-arcv-d-001  |vnet-gfn-u-eun-001|10.52.44.48/28  |
|Prod|snet-gfn-p-arcv-a-001  |vnet-gfn-u-eun-001|10.52.44.64/28  |
|Prod|snet-gfn-p-arcv-a-i-001|vnet-gfn-u-eun-001|10.52.44.40/29  |
|Prod|snet-gfn-p-arcv-a-i-002|vnet-gfn-p-eun-001|10.52.44.128/29 |
|Prod|snet-gfn-p-arcv-d-002  |vnet-gfn-p-eun-001|10.52.44.80/28  |
|Prod|snet-gfn-p-arcv-a-002  |vnet-gfn-p-eun-001|10.52.44.96/28  |
|Prod|snet-gfn-p-arcv-a-i-003|vnet-gfn-p-eun-001|10.52.44.112/29  |
|Prod|snet-gfn-p-arcv-a-i-004|vnet-gfn-p-eun-001|10.52.44.120/29 |

##5.3 New Service Account details
|Environment|Name|Description|
|-----------|----|----------|
|Prod   |SA-CYB-ARCV-P| For performing read operation on archive database and storage account       |



### 5.2 AS-IS/NON PROD

|Name                    |Trajectory |
|------------------------|-----------|
|cosmosdb-cosmos      |Decom      |
|cosmosdbsearch       |Decom      |
|azsauatSRC002           |Decom      |
|azsauatSRC003           |Decom      |
|W-UAT-W1051R - decom    |Decom      |
|azsauatSRC001 - unused  |Decom      |


### TO-BE 


#### Production

|               | TO-BE                   | TO-BE                   | TO-BE                 | TO-BE                   | TO-BE                  | TO-BE                   | TO-BE                  | TO-BE                 | TO-BE                |
| ------------- | ----------------------- | ----------------------- | --------------------- | ----------------------- | ---------------------- | ----------------------- | ---------------------- | --------------------- | -------------------- |
| Environment   | PROD                    | PROD                    | PROD                  | PROD                    | PROD                   | PROD                    | PROD                   | PROD                  | PROD                 |
| Activity      | New                     | New                     | New                   | New                     | New                    | New                     | New                    | New                   | New                  |
| Name          | cosmosgfnpeunarcv001    | srch-gfn-p-eun-arcv-001 | stgfnpeunarcv002      | plan-gfn-p-eun-arcv-001 | app-gfn-p-eun-arcv-001 | plan-gfn-p-eun-arcv-002 | app-gfn-p-eun-arcv-002 | vm-arcv-p-w01         | vm-arcv-p-w02        |
| Description   | Application database    | Application search      | Storage account       | App Service plan        | App Service            | App Service plan        | App Service            | Apogee Scanner        | Management Server    |
| IP            | TBC                     | TBC                     | TBC                   | TBC                     | TBC                    | TBC                     | TBC                    | 10.52.44.71           | 10.60.4.5            |
| OS            | N/A                     | N/A                     | N/A                   | N/A                     | N/A                    | N/A                     | N/A                    | 2019                  | 2019                 |
| Resource Type | Azure Cosmos DB account | Azure Cognitive Search  | Storage account       | App Service plan        | App Service            | App Service plan        | App Service            | VM                    | VM                   |
| Sku           | Provisioned throughput  | Standard                | GRS,Standard          | P1v2                    | n/a                    | P1v2                    | n/a                    | Standard F4           | Standard D4s v3      |
| CPU           | n/a                     | n/a                     | n/a                   | n/a                     | n/a                    | n/a                     | n/a                    | 4                     | 4                    |
| RAM           | n/a                     | n/a                     | n/a                   | n/a                     | n/a                    | n/a                     | n/a                    | 8                     | 16                   |
| OS-Disk       | n/a                     | n/a                     | n/a                   | n/a                     | n/a                    | n/a                     | n/a                    | 127                   | 512                  |
| Non-OS-Disk   | n/a                     | n/a                     | n/a                   | n/a                     | n/a                    | n/a                     | n/a                    | 200                   | 600                  |
| ResourceGroup | rg-gfn-p-eun-arcv-001   | rg-gfn-p-eun-arcv-001   | rg-gfn-p-eun-arcv-001 | rg-gfn-p-eun-arcv-001   | rg-gfn-p-eun-arcv-001  | rg-gfn-p-eun-arcv-001   | rg-gfn-p-eun-arcv-001  | rg-gfn-p-eun-arcv-001 | AZ-RG-UAT-ClockHouse |
| vNet          | vnet-gfn-p-eun-001      | vnet-gfn-p-eun-001      | vnet-gfn-p-eun-001    | vnet-gfn-p-eun-001      | vnet-gfn-p-eun-001     | vnet-gfn-p-eun-001      | vnet-gfn-p-eun-001     | vnet-gfn-p-eun-001    | AZ-VN-UAT-VNET001    |
| vNet IP       | 10.52.44.0/23           | 10.52.44.0/23           | 10.52.44.0/23         | 10.52.44.0/23           | 10.52.44.0/23          | 10.52.44.0/23           | 10.52.44.0/23          | 10.52.44.0/23         | 10.60.4.0/22         |
| Subnet        | snet-gfn-p-arcv-d-001   | snet-gfn-p-arcv-d-001   | snet-gfn-p-arcv-d-001 | snet-gfn-p-arcv-a-001   | snet-gfn-p-arcv-a-001  | snet-gfn-p-arcv-a-001   | snet-gfn-p-arcv-a-001  | snet-gfn-p-arcv-a-001 | AZ-SN-UAT-CLT-SPL003 |
| Subnet IP     | 10.52.44.48/28          | 10.52.44.48/28          | 10.52.44.48/28        | 10.52.44.64/28          | 10.52.44.64/28         | 10.52.44.64/28          | 10.52.44.64/28         | 10.52.44.64/28        | 10.60.4.0/24         |
| SuSRCription  | sub-gfn-p-001           | sub-gfn-p-001           | sub-gfn-p-001         | sub-gfn-p-001           | sub-gfn-p-001          | sub-gfn-p-001           | sub-gfn-p-001          | sub-gfn-p-001         | AZ-TG-UAT-SUB001     |
| Domain        | n/a                     | n/a                     | n/a                   | n/a                     | n/a                    | n/a                     | n/a                    | towergate.local       | towergate.local      |
| Region        | eun                     | eun                     | eun                   | eun                     | eun                    | eun                     | eun                    | eun                   | eun                  |
| Application   | n/a                     | n/a                     | n/a                   | n/a                     | n/a                    | n/a                     | n/a                    | Apogee Scanner        | n/a                  |
| Repo Name     | repo.gfn                | repo.gfn                | repo.gfn              | repo.gfn                | repo.gfn               | repo.gfn                | repo.gfn               | repo.gfn              | repo.gfn             |
| Pipeline Name | arcv.gfn                | arcv.gfn                | arcv.gfn              | arcv.gfn                | arcv.gfn               | arcv.gfn                | arcv.gfn               | arcv.gfn              | arcv.gfn             |
|               |                         |                         |                       |                         |                        |                         |                        |                       |                      |


####Pre-Prod

|               | TO-BE                   | TO-BE                   | TO-BE                 | TO-BE                   | TO-BE                  | TO-BE                 |
| ------------- | ----------------------- | ----------------------- | --------------------- | ----------------------- | ---------------------- | --------------------- |
| Environment   | pre-prod                | pre-prod                | pre-prod              | pre-prod                | pre-prod               | pre-prod              |
| Activity      | New                     | New                     | New                   | New                     | New                    | New                   |
| Name          | cosmosgfnpeunarcv002    | srch-gfn-p-eun-arcv-002 | stgfnueunarcv004      | plan-gfn-p-eun-arcv-003 | app-gfn-p-eun-arcv-003 | vm-arcv-p-w03         |
| Description   | Application database    | Application search      | Storage account       | App Service plan        | App Service            | Management Server     |
| IP            | TBC                     | TBC                     | TBC                   | TBC                     | TBC                    | TBC                   |
| OS            | n/a                     | n/a                     | n/a                   | n/a                     | n/a                    | 2019                  |
| Resource Type | Azure Cosmos DB account | Azure Cognitive Search  | Storage account       | App Service plan        | App Service            | VM                    |
| Sku           | Provisioned throughput  | Standard                | GRS,Standard          | P1v2                    | n/a                    | Standard D4s v3       |
| CPU           | n/a                     | n/a                     | n/a                   | n/a                     | n/a                    | 4                     |
| RAM           | n/a                     | n/a                     | n/a                   | n/a                     | n/a                    | 16                    |
| OS-Disk       | n/a                     | n/a                     | n/a                   | n/a                     | n/a                    | 512                   |
| Non-OS-Disk   | n/a                     | n/a                     | n/a                   | n/a                     | n/a                    | 600                   |
| ResourceGroup | rg-gfn-p-eun-arcv-002   | rg-gfn-p-eun-arcv-002   | rg-gfn-p-eun-arcv-002 | rg-gfn-p-eun-arcv-002   | rg-gfn-p-eun-arcv-002  | rg-gfn-p-eun-arcv-002 |
| vNet          | vnet-gfn-p-eun-001      | vnet-gfn-p-eun-001      | vnet-gfn-p-eun-001    | vnet-gfn-p-eun-001      | vnet-gfn-p-eun-001     | vnet-gfn-p-eun-001    |
| vNet IP       | 10.52.47.0/24           | 10.52.47.0/24           | 10.52.47.0/24         | 10.52.47.0/24           | 10.52.47.0/24          | 10.52.47.0/24         |
| Subnet        | snet-gfn-p-arcv-d-002   | snet-gfn-p-arcv-d-002   | snet-gfn-p-arcv-d-002 | snet-gfn-p-arcv-a-002   | snet-gfn-p-arcv-a-002  | snet-gfn-p-arcv-a-002 |
| Subnet IP     | 10.52.44.80/28          | 10.52.44.80/28          | 10.52.44.80/28        | 10.52.44.96/28          | 10.52.44.96/28         | 10.52.44.96/28        |
| SuSRCription  | sub-gfn-p-001           | sub-gfn-p-001           | sub-gfn-p-001         | sub-gfn-p-001           | sub-gfn-p-001          | sub-gfn-p-001         |
| Domain        | n/a                     | n/a                     | n/a                   | n/a                     | n/a                    | towergate.local       |
| Region        | eun                     | eun                     | eun                   | eun                     | eun                    | eun                   |
| Application   | n/a                     | n/a                     | n/a                   | n/a                     | n/a                    | n/a                   |
| Repo Name     | repo.gfn                | repo.gfn                | repo.gfn              | repo.gfn                | repo.gfn               | repo.gfn              |
| Pipeline Name | arcv.gfn                | arcv.gfn                | arcv.gfn              | arcv.gfn                | arcv.gfn               | arcv.gfn              |
|               |                         |                         |                       |                         |                        |                       |


###Backup Storage Accounts

| Storage Account Name | Environment | SuSRCrption   | Resource Group        | Purpose                        |
| -------------------- | ----------- | ------------- | --------------------- | ------------------------------ |
| stgfnpeunarcv001     | Prod        | sub-gfn-p-001 | rg-gfn-p-eun-arcv-002 | Backup of Appservice 001 & 002 |
| stgfnpeunarcv003     | Pre-Prod    | sub-gfn-p-001 | rg-gfn-p-eun-arcv-002 | Backup of Appservice 003 & 004 |
|                      |             |               |                       |                                |


## 6. Migration Journeys

### Transition

<b>WARNING: </b>Migration methods still need to be evaulated with the specific data volumnes.

|Service Type             |Migration Method*     |Named Service                 |Migration Example |
|-------------------------|----------------------|------------------------------|------------------|
|Storage                  |azcopy                |azsaprodSRC001                |Mig Azure Storage |
|CosmosDb                 |dt                    |archivalprod-cosmos           |Mig CosmosDb      |
|Document search          |Reindex               |cosmosdbsearchnew          |TBC               |
|App Service Plan         |Deploy New            |ServicePlan6755327f-8869      |n/a               |
|App Service              |Deploy New            |web-prod-SRC001,web-uat-SRC001|n/a               |


## 7. Network Flows

TODO

## 8. Migration methods

#### Mig Azure Storage 
User requires Storage Blob Data Contributor & Storage Blob Data Reader
```
.\azcopy sync 'https://weustorageaccct01.blob.core.windows.net/data'   'https://stidigdeunerp002.blob.core.windows.net/data' --recursive
```

### Mig CosmosDb
```
.\dt /s:DocumentDB /s.ConnectionString:"AccountEndpoint=https://source0202020202.documents.azure.com:443/;AccountKey=m3Q8mV0B0sfTrknOVUi2x6LgnzNlPtz8Vc1gkpojF91hDRF67XHLwUsFoWSSmdmAmeiOuFU2k6uDeiGZxzadxw==;Database=db1;" /s.Collection:container1 /t:DocumentDB /t.ConnectionString:"AccountEndpoint=https://source0202020202.documents.azure.com:443/;AccountKey=m3Q8mV0B0sfTrknOVUi2x6LgnzNlPtz8Vc1gkpojF91hDRF67XHLwUsFoWSSmdmAmeiOuFU2k6uDeiGZxzadxw==;Database=db2;" /t.Collection:container1 /t.PartitionKey:/id
```
# 9.User Access List



## Appendix A - Design Considerations


### Design Amendment - Environment Options

"UAT" in the Ardongah Archive uses PII data as it is used for testing ingestion of previously live data.  This will immediatly cause a security compliance issue.
We should build 2 PROD environments, one of which is used for testing.  Proposal is to build 2 resource groups in prod - ending 001 and 002 - with the usage for the each environment being designated via tagging.


### Business Sign Off
[Policy Archive.msg](/.attachments/Policy%20Archive-eed13926-9c84-46b6-822f-3195c3ec0c55.msg)
[MI Trader.msg](/.attachments/MI%20Trader-a8f8d147-8e56-421f-81d9-d7f590f7ed5a.msg)


