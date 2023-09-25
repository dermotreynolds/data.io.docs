---
sort: 1
---

# Terraform Experimenting

```mermaid
graph TB
    subgraph Environment
        Device2[variables.tf] --> Device4
        Device3[locals.tf] --> Device4
        Device4[main.tf]
    end


%% style Group0 fill:#FFFFFF,stroke:#333,stroke-width:1px, stroke-dasharray: 2
%% style App0 fill:white,stroke:#333,colour: white, stroke-width:1px , stroke-dasharray: 2
style Device2 fill:#90EE90	,stroke:#333,stroke-width:1px , stroke-dasharray: 2
style Device3 fill:#90EE90	,stroke:#333,stroke-width:1px , stroke-dasharray: 2
style Device4 fill:#90EE90	,stroke:#333,stroke-width:1px , stroke-dasharray: 2

```

```bash
terraform apply -var-file="./variables.tfvars"
```