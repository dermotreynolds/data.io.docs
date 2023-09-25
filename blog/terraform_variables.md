---
sort: 1
---

# Quick Intro Terraform Variables

Terraform effectively has 3 variable types as depicted in the digram below.  You will note that the Output and Local are refered to a "Values", this is because - in part - their declaration syntax is different.

```mermaid
graph TB
    input(Input Variable)-->module
    subgraph module_code[Module]
        local(Local Value) --> module((Module))
    end
    module --> output(Output Value)
```

### In summary:
- <b>Input Variables</b> serve as parameters for a Terraform module, so users can customize behavior without editing the source.
- <b>Output Values</b> are like return values for a Terraform module.
- <b>Local Values</b> are a convenience feature for assigning a short name to an expression.


### Input Variable Example

Input Variables can have the following data types:
- string
- number
- bool

The type constructors allow you to specify complex types such as collections:

- list(<TYPE>)
- set(<TYPE>)
- map(<TYPE>)
- object({<ATTR NAME> = <TYPE>, ... })
- tuple([<TYPE>, ...])


Assume that we want to store a Virtual Network Name such as:

```virtual_network_name = "vnet-trans-p-001"```

The syntax for declaring an input variable is as follows:

```python
variable "virtual_network_name" {
  type        = string
  description = "The name of the virtual network."

  validation {
    condition     = length(var.image_id) > 4 && substr(var.image_id, 0, 4) == "ami-"
    error_message = "The image_id value must be a valid AMI id, starting with \"ami-\"."
  }
}
```

```python
var.virtual_network_name is "vn-trans-p-001"
│
│ The virtual_network_name value must be a valid vNet Name, starting with "vnet-".
│
│ This was checked by the validation rule at variables.tf:5,3-13.
```

var.virtual_network_name is "vnet-"
│
│ The virtual_network_name value must be a valid vNet Name, starting with "vnet-".
│
│ This was checked by the validation rule at variables.tf:5,3-13.