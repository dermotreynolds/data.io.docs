---
sort: 1
---

# Application Deployment Design Template (Draft)

```tip
This template is based on a combination of personal experience and for Azure Landing Zone design area of the Microsoft Cloud Adoption Framework for Azure.

It should cover or use inputs from the items below.

Feel free to use it and adopt it for your purposes.

```

![Alt text](./Infrastructure%20Ecosystem.png)


#### Document Status

Document Status:   **`Draft`**      *(Options are: `Draft`, `Ready for Review`, `Approved` )*

#### Revision Requests
```note
The revisions requests section is used to track requests changes to the design.
```

| Date | Section | Requested Change |  Authors Comments|
|--|--|--|--|
|  |  |  |  |
|  |  |  |  |
|  |  |  |  |

#### Version Control

```note
The version control section is used to capture design modifications.
```

|Version|Date| Author |Section Changed | Reason for Change|
|--|--|--|--|--|--|
|0.1 |01/09/2021  |Dermot Reynolds  |ALL  |Document Creation  |

## 1. Application Definition
```mermaid
flowchart TD

   subgraph Group0[Application Definition]
        1([Business Owner]):::LocationClass
        2([IT Operations Owner]):::LocationClass
        3([Application Name]):::LocationClass
        4([Business Segment]):::LocationClass
        5([Hours of Support]):::LocationClass
        6([Hours of Operation]):::LocationClass
   end
   class Group0 GroupClass
   classDef LocationClass fill:#2FE15B, stroke:#2FE15B, stroke-width:2px, font-size:12px, color:white, 
   classDef GroupClass fill:white,stroke:#2979ff,stroke-width:2px, stroke-dasharray: 0,font-size: 12px, color:black, padding-left:0em;
```


|Name                       |Value    |Description                              |
|---------------------------|---------|-----------------------------------------|
|Business Unit              |         |This is used to detemine the subscription|
|Application Name           |         |This is used to determine the names of the resources|
|Description                |         |                                         |
|Business Application Owner |         |Tag Value                                |
|Business Technical Owner   |         |Tag Value                                |
|Operational Owner          |         |Tag Value                                |



## 2. Requirements

```mermaid
flowchart TD

   subgraph Group0[Requirements]
        1([Business Drivers]):::LocationClass
        2([Technical Drivers]):::LocationClass
        3([Functional Requirements]):::LocationClass
        4([Non Functional Requirements]):::LocationClass
   
   end
   class Group0 GroupClass
   classDef LocationClass fill:#2FE15B, stroke:#2FE15B, stroke-width:2px, font-size:12px, color:white, 
   classDef GroupClass fill:white,stroke:#2979ff,stroke-width:2px, stroke-dasharray: 0,font-size: 12px, color:black, padding-left:0em;
```

### 2.1 Functional & Non-Functional Requirements

| Requirement#  |Functional/NFR|Date | Description | Addressed By     | Reviewed Date  |
|---------------|--------------|-----|-------------|------------------|----------------|
|               |              |     |             |                  |                |

### 2.2 Seasonality

`Is this application or elements of it, busier at specific times of the day or year?  Are there key events that take place at specific times?`



## 3. Current Mode Of Operation
```mermaid
flowchart TD

   subgraph Group0[Current Mode Of Operation]
        1([Physical Architecture]):::LocationClass
        2([Assets]):::LocationClass
        3([Key Issues]):::LocationClass
   end
   class Group0 GroupClass
   classDef LocationClass fill:#2FE15B, stroke:#2FE15B, stroke-width:2px, font-size:12px, color:white, 
   classDef GroupClass fill:white,stroke:#2979ff,stroke-width:2px, stroke-dasharray: 0,font-size: 12px, color:black, padding-left:0em;
```

```note
If this is a transformation or migration please provide the AS-IS detail.
```

### 3.1 Physical Architecture

```mermaid
graph LR

    subgraph Group0["External"]
        Location0(("Customers")) -->
        Location1(("Cloud WAF")) -->
        Location2(("Internet"))
    end

        Location2 --> Device3
        Location2 --> Device4

        BadCallout1(("Tech Debt")) --> Device4
    subgraph Group5["Azure"]
            
        subgraph Group2["DMZ"]
            
            Device3["S-FINCO-WEB01"]
            Device4["S-FINCO-WEB02"]
        end

        subgraph Group4[" Prod "]
            Device6[S-FINCO-SQL01]
        end      
    end
    
        Device3-->Device6
        Device4-->Device6
    
     

    style Location0 fill:white,stroke:#E900FF,stroke-width:2px
 
    style Location1 fill:white,stroke:#E900FF,stroke-width:2px

    style Location2 fill:white,stroke:#E900FF,stroke-width:2px

    %% style Location4 fill:#ECECEC,stroke:#007fff,stroke-width:2px                

    %% style Device0 fill:#007fff,stroke:#007fff, stroke-width:2px , font-size: 80%, color: white
    %% style Device1 fill:white,stroke:#007fff,colour: white, stroke-width:1px , stroke-dasharray: 0
    
    %% style Device3 fill:white,stroke:#007fff,colour: white, stroke-width:1px , stroke-dasharray: 0        
    %% style Device4 fill:white,stroke:#007fff,colour: white, stroke-width:1px , stroke-dasharray: 0        
   
    %% style Device6 fill:white,stroke:#007fff,colour: white, stroke-width:1px , stroke-dasharray: 0     

     linkStyle 0 stroke:#90EE90,stroke-width:1.5px,color:blue;                       
     linkStyle 1 stroke:#90EE90,stroke-width:1.5px,color:blue;
     linkStyle 2 stroke:#90EE90,stroke-width:1.5px,color:blue;
     linkStyle 3 stroke:#90EE90,stroke-width:1.5px,color:blue;
     linkStyle 4 stroke:#90EE90,stroke-width:1.5px,color:blue;
     linkStyle 5 stroke:#90EE90,stroke-width:1.5px,color:blue;
     linkStyle 6 stroke:#90EE90,stroke-width:1.5px,color:blue;
    %% linkStyle 7 stroke:#90EE90,stroke-width:1.5px,color:blue;        
    %% linkStyle 8 stroke:#90EE90,stroke-width:1.5px,color:blue;            
    %% linkStyle 9 stroke:#90EE90,stroke-width:1.5px,color:blue;            


        style BadCallout1 fill:#FF5733	,stroke:#333,stroke-width:1px , stroke-dasharray: 2   

        class Device0,Device1,Device3,Device4,Device6 DeviceClass
        class Group0,Group1,Group2,Group4,Group5 GroupClass

        classDef DeviceClass fill:#5463FF,stroke:#5463FF, stroke-width:2px , font-size: 100%, color: white
        classDef GroupClass fill:#FFFFFF,stroke:#5463FF,stroke-width:2px, stroke-dasharray: 3
        %%style Device0 fill:white,stroke:#333,colour: white, stroke-width:1px , stroke-dasharray: 2
        %%style Group0 fill:#FFFFFF,stroke:#333,stroke-width:1px, stroke-dasharray: 2
        %%style Location0 fill:#F6F7F7,stroke:#0352FC,stroke-width:2px
        %%style WAN0 fill:#F6F7F7,stroke:#0352FC,stroke-width:2px
        %%style SUBS0 fill:#F6F7F7,stroke:#0352FC,stroke-width:2px
        %%style VNET0 fill:#54A8F9,stroke:#333,stroke-width:1px , color:#FFFFFF, stroke-dasharray: 2
        %%style SUBNET0 fill:#CDD2D6	,stroke:#333,stroke-width:1px , stroke-dasharray: 2
        %%style GoodCallout0 fill:#90EE90	,stroke:#333,stroke-width:1px , stroke-dasharray: 2
        %%style BadCallout0 fill:#FF5733	,stroke:#333,stroke-width:1px , stroke-dasharray: 2
        %%style USR0 fill:#F6F7F7,stroke:#0352FC,stroke-width:2px
        %%style UnsureCallout0 fill:#FFBB33	,stroke:#333,stroke-width:1px , stroke-dasharray: 2
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

### 3.2 Assets

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

### 3.3 Issues



## 4. Future Mode Of Operation

### 4.1 Identity & Access Management
```mermaid
flowchart LR
   subgraph Group0[Identity & Access Management]
        1([Privilege Access Management]):::LocationClass
        2([Privilege Identity Management]):::LocationClass
        3([Managed Identities]):::LocationClass
        4([Service Principals]):::LocationClass
        3([Role Base Access Control]):::LocationClass
   end
   class Group0 GroupClass
   classDef LocationClass fill:#2FE15B, stroke:#2FE15B, stroke-width:2px, font-size:12px, color:white, 
   classDef GroupClass fill:white,stroke:#2979ff,stroke-width:2px, stroke-dasharray: 0,font-size: 12px, color:black, padding-left:0em;
```
#### 4.1.1 Privilege Access Management    
#### 4.1.2 Privilege Identity Management  
#### 4.1.3 Managed Identities             
#### 4.1.4 Service Principals             
#### 4.1.5 Role Base Access Control       

### 4.2 Protect & Recover
#### 4.2.1 Backup
#### 4.2.2 Disaster Recovery

### 4.3 Monitoring & Alerting
#### 4.3.1 Network Watcher
#### 4.3.2 Diagnostics / Log Analytics
#### 4.3.3 Application Insights
#### 4.3.4 Azure Monitor
#### 4.3.5 Alerts

### 4.4 Resource Organisation
#### 4.4.1 Subscription
#### 4.4.2 Management Group
#### 4.4.3 Resource Group
#### 4.4.4 Tagging

### 4.5 Governance
#### 4.5.1 Cost Management
##### 4.5.1.1 AS-IS Cost

|Statistic            | AS-IS       |
|---------------------|-------------|
|Resources            |         3   |
|Monthly Infra Cost   |       £1,000|
|Yearly Infra Cost    |      £12,000|
|1 Year TCO           |      £15,000|
|3 Year TCO           |      £45,000|

##### 4.5.1.2 TO-BE Cost

|Statistic            |    TO-BE    |
|---------------------|-------------|
|Resources            |            3|
|Monthly Infra Cost   |       £500  |
|Yearly Infra Cost    |      £6,000 |
|1 Year TCO           |      £9,000 |
|3 Year TCO           |      £27,000|

##### 4.5.1.3 Cost Comparison

##### 4.5.1.4 Budget

#### 4.5.2 Capacity Management
#### 4.5.3 Azure Policy

### 4.6 Security
#### 4.6.1 Network Security Groups
#### 4.6.2 Firewall Rules
#### 4.6.3  DDoS
#### 4.6.4  WAF
#### 4.6.5  Security Center
#### 4.6.6  Sentinel
#### 4.6.7  Secrets Management
#### 4.6.8  Encryption

### 4.7 Networking
#### 4.7.1 Virtual Network
#### 4.7.2 Subnet
#### 4.7.3 Route Table

### 4.8 Dependencies
#### 4.8.1 Platform Dependencies
#### 4.8.2 DNS
#### 4.8.3 Application Dependencies 

### 4.9 DevOps
#### 4.9.1 Infrastructure Deployment
#### 4.9.2 Application Deployment

### 4.10 Architecture

#### 4.10.1 Environments

#### 4.10.2 Context

``Diagram showing where this application sits``

#### 4.10.3 Physical Architecture

```mermaid
graph LR

    subgraph Group0["External"]
        Location0(("Customers")) -->
        Device0["WAF"]

    end
        Device0 --> Location1
        Location1(("Internet"))

        Location1 --> Device3
        Location1 --> Device4

        GoodCallout1(("PaaS")) --> Device4
    subgraph Group5["Azure"]
            
        subgraph Group2["DMZ"]
            
            Device3["S-FINCO-WEB01"]
            Device4["AS-FINCO-WWW01"]
        end

        subgraph Group4[" Prod "]
            Device6[S-FINCO-SQL01]
        end      
    end
    
        Device3-->Device6
        Device4-->Device6
    
     

    style Location0 fill:#ECECEC,stroke:#007fff,stroke-width:2px
 
    style Location1 fill:#ECECEC,stroke:#007fff,stroke-width:2px        

    %% style Location4 fill:#ECECEC,stroke:#007fff,stroke-width:2px                

    %% style Device0 fill:#007fff,stroke:#007fff, stroke-width:2px , font-size: 80%, color: white
    %% style Device1 fill:white,stroke:#007fff,colour: white, stroke-width:1px , stroke-dasharray: 0
    
    %% style Device3 fill:white,stroke:#007fff,colour: white, stroke-width:1px , stroke-dasharray: 0        
    %% style Device4 fill:white,stroke:#007fff,colour: white, stroke-width:1px , stroke-dasharray: 0        
   
    %% style Device6 fill:white,stroke:#007fff,colour: white, stroke-width:1px , stroke-dasharray: 0     

     linkStyle 0 stroke:#90EE90,stroke-width:1.5px,color:blue;                       
     linkStyle 1 stroke:#90EE90,stroke-width:1.5px,color:blue;
     linkStyle 2 stroke:#90EE90,stroke-width:1.5px,color:blue;
     linkStyle 3 stroke:#90EE90,stroke-width:1.5px,color:blue;
     linkStyle 4 stroke:#90EE90,stroke-width:1.5px,color:blue;
     linkStyle 5 stroke:#90EE90,stroke-width:1.5px,color:blue;
    %% linkStyle 6 stroke:#90EE90,stroke-width:1.5px,color:blue;
    %% linkStyle 7 stroke:#90EE90,stroke-width:1.5px,color:blue;        
    %% linkStyle 8 stroke:#90EE90,stroke-width:1.5px,color:blue;            
    %% linkStyle 9 stroke:#90EE90,stroke-width:1.5px,color:blue;            


        style GoodCallout1 fill:#90EE90	,stroke:#333,stroke-width:1px , stroke-dasharray: 2

        class Device0,Device1,Device3,Device4,Device6 DeviceClass
        class Group0,Group1,Group2,Group4,Group5 GroupClass

        classDef DeviceClass fill:#5463FF,stroke:#5463FF, stroke-width:2px , font-size: 100%, color: white
        classDef GroupClass fill:#FFFFFF,stroke:#5463FF,stroke-width:2px, stroke-dasharray: 3
        %%style Device0 fill:white,stroke:#333,colour: white, stroke-width:1px , stroke-dasharray: 2
        %%style Group0 fill:#FFFFFF,stroke:#333,stroke-width:1px, stroke-dasharray: 2
        %%style Location0 fill:#F6F7F7,stroke:#0352FC,stroke-width:2px
        %%style WAN0 fill:#F6F7F7,stroke:#0352FC,stroke-width:2px
        %%style SUBS0 fill:#F6F7F7,stroke:#0352FC,stroke-width:2px
        %%style VNET0 fill:#54A8F9,stroke:#333,stroke-width:1px , color:#FFFFFF, stroke-dasharray: 2
        %%style SUBNET0 fill:#CDD2D6	,stroke:#333,stroke-width:1px , stroke-dasharray: 2
        %%style GoodCallout0 fill:#90EE90	,stroke:#333,stroke-width:1px , stroke-dasharray: 2
        %%style BadCallout0 fill:#FF5733	,stroke:#333,stroke-width:1px , stroke-dasharray: 2
        %%style USR0 fill:#F6F7F7,stroke:#0352FC,stroke-width:2px
        %%style UnsureCallout0 fill:#FFBB33	,stroke:#333,stroke-width:1px , stroke-dasharray: 2
```

#### 4.10.3 Bill Of Materials

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

### 4.11 Optimisations

```tip
If you were looking to optimise this solution in 12 months time what aspects would you consider?

Are there any features that can be implemented to make this solution more efficient?
Can we right size workloads at specific times of the day?
Have we used a specific sku that provides greater flexibility?
Have we added additional features to make it easier to troubleshoot?
```

#### 4.11.1 Cost

#### 4.11.1 Management

#### 4.11.1 Viability

#### 4.11.1 Refactoring

### 4.12 Service Introduction / Handover

```tip
If you were asked to support this in 12 months time at 3am what material would you look at?
```
#### 4.12.1 Training Videos

#### 4.12.2 How Tos


## Appendix A - References

### Subscription Vending
https://learn.microsoft.com/en-us/azure/architecture/landing-zones/subscription-vending

### Security Best Practice
https://learn.microsoft.com/en-us/azure/security/fundamentals/best-practices-and-patterns

### Azure Landing Zones

https://learn.microsoft.com/en-us/azure/cloud-adoption-framework/ready/landing-zone/