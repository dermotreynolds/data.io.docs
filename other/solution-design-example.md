---
sort: 2
---

# Solution Design Example

## 1. Document Status
```note
The Document Status section is used to display the current status of the design, the options are: Draft, Ready for Review, Approved)
```

Document Status:   `Approved`

## 2. Project Properties

### 2.1 Organisation Properties

|Name                       |Value    |
|---------------------------|---------|
|Company                    |         |
|Segment                    |         |
|Program Sponsor            |         |

### 2.2 Project Properties

|Name                       |Value    |
|---------------------------|---------|
|Project Title              |         |
|Project Description        |         |
|Business Application Owner |         |
|Business Technical Owner   |         |
|Operational Owner          |         |

### 2.3 TCO Properties

|Name                       |Value    |
|---------------------------|---------|
|Support/instance/year      | £1,000  |

## 3. Revision Requests
```note
The revisions requests section is used to track requests changes to the design.
```

| Date | Section | Requested Change |  Authors Comments|
|--|--|--|--|
||  |  |  |
|  |  |  |  |
|  |  |  |  |

## 4. Version Control

```note
The version control section is used to capture design modifications.
```

|Version|Date| Author |Section Changed | Reason for Change|
|--|--|--|--|--|--|
|0.1 |01/09/2021  |Dermot Reynolds  |ALL  |Document Creation  |

## 5. Requirements

| Requirement#  |Date | Description | Addressed By     | Reviewed Date  |
|---------------|-----|-------------|------------------|----------------|
|               |     |             |                  |                |


## 6. AS-IS

### 6.1 Physical Architecture

```mermaid
flowchart TD

    subgraph Group0[3rd Party]
        d1[fa:fa-server ERP System]:::DeviceClass -->
        d2[fa:fa-server 3rd Party Integration]:::DeviceClass
    end

        d99[fa:fa-cloud Internet]
        d100[fa:fa-cloud Internet]

    i0([1. No Firewall/WAF]):::BadCalloutClass -->d10
    i1([2. IaaS, not PaaS]):::BadCalloutClass --> d16
    
    subgraph Group1[Platform]
        
        d10[fa:fa-server App : Product API]:::DeviceClass 
        
        d10 -->|write|d11[fa:fa-database Storage Account: Product Payload]:::DeviceClass
        d10 --> |write|d12[fa:fa-database Queue: Product Message]:::DeviceClass
        
        d10 --> |write|d16[(fa:fa-server Elastic Search: Product)]:::DeviceClass

        d13[fa:fa-server Product Processor]:::DeviceClass

        d11 --> |2.read|d13
        d12 --> |1.read|d13

        
        d13 --> |write|d15[fa:fa-database DB: Product & Inventory]:::DeviceClass


        d20[fa:fa-server App : Inventory API]:::DeviceClass --> |write|d22[fa:fa-database Queue: Inventory Message]:::DeviceClass

        d23[fa:fa-server Inventory Processor]:::DeviceClass

        d22 --> |read|d23

        d23 --> |write|d15[(fa:fa-database DB: Product & Inventory)]:::DeviceClass


        d30[fa:fa-server Pricing API]:::DeviceClass --> |write|d32[fa:fa-database Queue: Pricing Message]:::DeviceClass

        d33[fa:fa-server Pricing Processor]:::DeviceClass

        d32 --> |read|d33

        d33 --> |write|d35[(fa:fa-database Storage: Pricing)]:::DeviceClass
       
        

        d15 -->|read|d40[fa:fa-server FE Product API]:::DeviceClass
        d16-->|read|d41[fa:fa-server FE Search API]:::DeviceClass
        d15 -->|read|d42[fa:fa-server FE Inventory API]:::DeviceClass
        d15 <-->|read&write|d43[fa:fa-server FE Order API]:::DeviceClass
        d35 -->|read|d44[fa:fa-server FE Pricing API]:::DeviceClass
    end

    i2([3. No Firewall/WAF]):::BadCalloutClass --> d41
    i3([4. Username/Password Autentication]):::BadCalloutClass -->d13
    i4([5. No Disaster Recovery]):::BadCalloutClass --> Group1
    i5([6. No network segmentation]):::BadCalloutClass --> d15
    

    subgraph Group2[User]
        d50[fa:fa-browser Angular App]:::DeviceClass
    end

    d40 -->d100
    d41 -->d100
    d42 -->d100
    d43 -->d100
    d44 -->d100

    d100 --> d50



    d2 --> d99 --> d10
            d99 --> d20
            d99 --> d30

    %% class d1,d2,d3,d4,1.5,1.6,1.7,1.8 DeviceClass
    class 1.1.1 DeviceClass
    class 2.1,2.2,2.3,2.4 GoodCalloutClass
    class Group0,Group1,Group2 GroupClass
    
    classDef DeviceClass fill:#5463FF,stroke:white, stroke-width:2px , font-size: 100%, color: white
    classDef GroupClass fill:#FFFFFF,stroke:#5463FF,stroke-width:2px, stroke-dasharray: 3
    classDef GoodCalloutClass fill:#90EE90	,stroke:#333,stroke-width:1px , stroke-dasharray: 2
    classDef BadCalloutClass fill:#FF5733	,stroke:#333,stroke-width:1px , stroke-dasharray: 2   
```

```mermaid
graph TD

    subgraph Group1["Key"]
        subgraph Group0["Location"]
        end

        Location0(("External"))
        Device3["Asset"]

        BadCallout1(("Issue"))
        GoodCallout0(("Solution"))

    end
    
        %%style Location0 fill:white,stroke:##E900FF,stroke-width:2px,font-size: 12px   
        %%style BadCallout1 fill:#FF5733	,stroke:#333,stroke-width:1px , stroke-dasharray: 2,font-size: 12px   
        %%style GoodCallout0 fill:#90EE90	,stroke:#333,stroke-width:1px , stroke-dasharray: 2,font-size: 12px   

        class Location0 LocationClass
        class Device0,Device1,Device3,Device4,Device6 DeviceClass
        class Group0 GroupClass
        class Group1 KeyClass
        class GoodCallout0 GoodCalloutClass
        class BadCallout1 BadCalloutClass

        classDef LocationClass fill:white, stroke:#E900FF, stroke-width:2px, font-size:10px
        classDef GoodCalloutClass fill:#90EE90	,stroke:#333,stroke-width:1px , stroke-dasharray: 2,font-size: 10px
        classDef BadCalloutClass fill:#FF5733	,stroke:#333,stroke-width:1px , stroke-dasharray: 2,font-size: 10px
        classDef DeviceClass fill:#5463FF,stroke:#5463FF, stroke-width:2px , font-size: 10px, color: white
        classDef GroupClass fill:#FFFFFF,stroke:#5463FF,stroke-width:2px, stroke-dasharray: 3,font-size: 10px

        classDef KeyClass fill:#FFFFFF,stroke:grey,stroke-width:2px, stroke-dasharray: 3
```

### 6.2 Bill Of Materials

|               |S-FINCO-SQL01|S-FINCO-WEB01|S-FINCO-WEB02|
|---------------|-------------|-------------|-------------|
|Description    |             |             |             |
|Asset Type     |             |             |             |
|Sku            |             |             |             |
|OS             |             |             |             |
|Disks          |             |             |             |
|Tenant         |             |             |             |
|Subscription   |             |             |             |
|Resource Group |             |             |             |
|Virtual Network|             |             |             |
|Subnet         |             |             |             |


### 6.3 Key Statistics

|Statistic            | AS-IS       |
|---------------------|-------------|
|Resources            |         3   |
|Monthly Infra Cost   |       £1,000|
|Yearly Infra Cost    |      £12,000|
|1 Year TCO           |      £15,000|
|3 Year TCO           |      £45,000|


## 7. TO-BE

### 7.1 Physical Architecture

```mermaid
flowchart LR
    subgraph Group0[AzureDevOps]
        Good0([Multiple pipielines implement segregation of duties]):::GoodCalloutClass
        Good0 -->d101
        subgraph d101[terraform/infra]
            direction LR
            Identity[Identity Pipeline]:::DeviceClass
            Policy[Policy Pipeline]:::DeviceClass
            Network[Network Pipeline]:::DeviceClass
            Infrastructure1[Landing Zone Infrastructure Pipeline]:::DeviceClass
            Infrastructure[Application Infrastructure Pipeline]:::DeviceClass
        end

        Good1([Application team can run the infrastructure pipeline]):::GoodCalloutClass
        Good1 --> Infrastructure

        d102[Application Deployments]:::DeviceClass
    end

    d101 --> Group1
    d102 --> d205



    subgraph Group1[Platform]


        d200 -.-> |private endpoint|d201
        d200 -.-> |private endpoint|d301

        subgraph Group102[Transit Subscription]
            Good2([Only internet facing service]):::GoodCalloutClass
            Good2 --> d200
            d200[fa:fa-server Front Door]:::DeviceClass
            subgraph Group30[Primary Transit vNet]
                d207[fa:fa-server Primary Transit Firewall]:::DeviceClass
            end

            subgraph Group31[Secondary Transit vNet]
                d208[fa:fa-server Secondary Transit Firewall]:::DeviceClass

            end

        end

        subgraph Group101[DevOps Subscription]
            subgraph Group40[Primary Transit vNet]
                d205[fa:fa-server Primary Hosted Agent]:::DeviceClass
            end

            subgraph Group41[Secondary Transit vNet]
                d206[fa:fa-server Secondary Hosted Agent]:::DeviceClass
            end            
        end

        Good3([Access to data controlled via NSG]):::GoodCalloutClass
        Good3 --> Group13

        subgraph Group103[Business Subscription]
            subgraph Group2[EU West Resource Group]
                subgraph Group100[App Subscription]
                    subgraph Group10[Primary App vNet]
                        subgraph Group11[web Subnet]
                            d201[fa:fa-server Function App - API]:::DeviceClass
                        end 

                        subgraph Group12[App Subnet]
                            d202[fa:fa-server Function App - Processor]:::DeviceClass
                        end


                        subgraph Group13[Data Subnet]
                            d203[fa:fa-database Storage Account]:::DeviceClass
                            d204[fa:fa-database Sql Database]:::DeviceClass
                            
                        end
                    end
                end
            end

            d205 -.-> |Deployment slots |d201
            d205 -.-> |Deployment slots |d202
            d205 -.-> |flyway project |d204

            

            subgraph Group3[EU North Resource Group]
            
                subgraph Group20[Secondary App vNet]
                    subgraph Group21[web Subnet]
                        d301[fa:fa-server Function App - API]:::DeviceClass
                    end
                    subgraph Group22[App Subnet]
                        d302[fa:fa-server Function App - Processor]:::DeviceClass
                    end            
                    subgraph Group23[Data Subnet]
                        d303[fa:fa-server Storage Account]:::DeviceClass
                        d304[fa:fa-database Sql Database]:::DeviceClass
                    end            
                end
            end
            
        end

    end

    class Group0,Group1,Group2,Group3,Group100,Group101,Group102,Group103,Group10,Group30,Group31,Group31,Group20,Group21,Group22,Group23,Group11,Group12,Group13,Group40,Group41 GroupClass

    classDef DeviceClass fill:#5463FF,stroke:white, stroke-width:2px , font-size: 100%, color: white
    classDef GroupClass fill:#FFFFFF,stroke:#5463FF,stroke-width:2px, stroke-dasharray: 3
    classDef GoodCalloutClass fill:#90EE90	,stroke:#333,stroke-width:1px , stroke-dasharray: 2
    classDef BadCalloutClass fill:#FF5733	,stroke:#333,stroke-width:1px , stroke-dasharray: 2   



```
#### Features

##### Security
1.  Only internet facing service is Front Door.
1.1.  All other services are not accessible over the internet.
2.  All application tiers are on different subnets.
2.1 Traffic between subnets is restricted via NSG.
3.  Policies enforce security standards.

##### Deployability
1.  All infrastructure is deployed using IaC(terraform)
2.  Blue/green deployments via Slots
`resource "azurerm_app_service_active_slot" "slotDemoActiveSlot" {...}`

##### Scalability
1.  Elastic Search Changed from IaaS(3 node) to PaaS.
2.  All PaaS elements can scale out.



### 7.2 Bill Of Materials

|               |S-FINCO-SQL01|S-FINCO-WEB01|AS-FINCO-WWW01|
|---------------|-------------|-------------|-------------|
|Description    |             |             |             |
|Asset Type     |             |             |             |
|Sku            |             |             |             |
|OS             |             |             |             |
|Disks          |             |             |             |
|Tenant         |             |             |             |
|Subscription   |             |             |             |
|Resource Group |             |             |             |
|Virtual Network|             |             |             |
|Subnet         |             |             |             |

### 7.3 Key Statistics

|Statistic            |    TO-BE    |
|---------------------|-------------|
|Resources            |            3|
|Monthly Infra Cost   |       £500  |
|Yearly Infra Cost    |      £6,000 |
|1 Year TCO           |      £9,000 |
|3 Year TCO           |      £27,000|

## 8 AS-IS vs TO-BE Comparison


### 8.1 Bill Of Materials



### 8.2 Key Statistics

|Statistic            | AS-IS       |    TO-BE    |Delta      |
|---------------------|-------------|-------------|-----------|
|Resources            |            3|            3| 0         |
|Monthly Infra Cost   |       £1,000|        £500 |      -£500|
|Yearly Infra Cost    |      £12,000|       £6000 |    -£6000 |
|1 Year TCO           |      £15,000|       £9000 |    -£6000 |
|3 Year TCO           |      £45,000|      £27000 |   -£18000 |

## 9 Implementation

### 9.1 Activities

|#   |Start Date|End Date  | Associated Resource | Activity         | Assigned Resource | Status     |
|----|----------|----------|---------------------|------------------|-------------------|------------|
|T1  |21/03/22  |21/03/22  |AS-FINCO-WWW01       |Deploy asset      | john@dlta.io      | `TO-DO`    |
|T2  |22/03/22  |22/03/22  |AS-FINCO-WWW01       |Deploy app        | john@dlta.io      | `TO-DO`    |
|T3  |23/03/22  |23/03/22  |AS-FINCO-WWW01       |Test app          | john@dlta.io      | `TO-DO`    |
|T4  |24/03/22  |24/03/22  |AS-FINCO-WWW01       |Service introduce | john@dlta.io      | `TO-DO`    |
|T5  |25/03/22  |25/03/22  |S-FINCO-WEB02        |Decommission      | john@dlta.io      | `TO-DO`    |

### 9.2 Roadmap

```mermaid
gantt
    title Roadmap
    dateFormat  DD-MM-YYYY
    section Section
    Deploy asset   :a1,21-03-2022, 1d
    Deploy app     :a2,after a1  , 1d
    Test app       :a3,after a2  , 1d    
    Service introduce       :a4,after a3  , 1d    
    Decommission            :a5,after a4  , 1d    
```