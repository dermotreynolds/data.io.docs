---
sort: 1
---

# Other Stuff

## AS-IS

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


## TO-BE (Summarised, Prod Only)

```mermaid
flowchart TD
    subgraph Group0[AzureDevOps]
        d101[terraform/infra]:::DeviceClass
        d102[Application Deployments]:::DeviceClass
    end

    d101 --> Group1
    d102 --> d205



    subgraph Group1[Platform]


        d200 -.-> |private endpoint|d201
        d200 -.-> |private endpoint|d301

        subgraph Group102[Transit Subscription]
            d200[Front Door]:::DeviceClass
            subgraph Group30[Primary Transit vNet]
                d207[Primary Transit Firewall]:::DeviceClass
            end

            subgraph Group31[Secondary Transit vNet]
                d208[Secondary Transit Firewall]:::DeviceClass

            end

        end

        subgraph Group101[DevOps Subscription]
            subgraph Group40[Primary Transit vNet]
                d205[Primary Hosted Agent]:::DeviceClass
            end

            subgraph Group41[Secondary Transit vNet]
                d206[Secondary Hosted Agent]:::DeviceClass
            end            
        end


        subgraph Group103[Business Subscription]
            subgraph Group2[EU West Resource Group]
                subgraph Group100[App Subscription]
                    subgraph Group10[Primary App vNet]
                        subgraph Group11[web Subnet]
                            d201[Function App - API]:::DeviceClass
                        end 

                        subgraph Group12[App Subnet]
                            d202[Function App - Processor]:::DeviceClass
                        end

                        subgraph Group13[Data Subnet]
                            d203[Storage Account]:::DeviceClass
                            d204[Sql Database]:::DeviceClass
                            
                        end
                    end
                end
            end

            d205 -.-> |Deployment slots |d201
            d205 -.-> |Deployment slots |d202
            d205 -.-> |flyway project |d204

            

            subgraph Group3[EU North Resource Group]
            
                subgraph Group20[Secondary vNet]
                    subgraph Group21[web Subnet]
                        d301[Function App - API]:::DeviceClass
                    end
                    subgraph Group22[App Subnet]
                        d302[Function App - Processor]:::DeviceClass
                    end            
                    subgraph Group23[Data Subnet]
                        d303[Storage Account]:::DeviceClass
                        d304[Sql Database]:::DeviceClass
                    end            
                end
            end
            
        end

    end

    class Group0,Group1,Group2,Group3 GroupClass

    classDef DeviceClass fill:#5463FF,stroke:white, stroke-width:2px , font-size: 100%, color: white
    classDef GroupClass fill:#FFFFFF,stroke:#5463FF,stroke-width:2px, stroke-dasharray: 3
    classDef GoodCalloutClass fill:#90EE90	,stroke:#333,stroke-width:1px , stroke-dasharray: 2
    classDef BadCalloutClass fill:#FF5733	,stroke:#333,stroke-width:1px , stroke-dasharray: 2   



```

```mermaid
flowchart TD

    subgraph Region0
        vNet2[Spoke Primary vNet] <-->
        vNet0[Primary Transit vNet]
        
    end

    subgraph Region1
        vNet3[Spoke Primary vNet] <-->
        vNet1[Secondary Transit vNet]
        
    end

    vNet0 <--> vNet1



```

### 1. Security
- All services are "behind" the Front Door
- 

### 1. Scalability
- PaaS services can scale on demand.

### Reliability


### Deployability


### Availability



### Maintainability