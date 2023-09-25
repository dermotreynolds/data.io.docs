---
sort: 2
---

# Terraform Lists

### Input variables

``` 
# variable.tf

variable "simplelist"{
    type = list(string)
}
```

```
# variables.tfvars

simplelist = [ "item1", "item2", "item3" ]

```

### 1. Raw output

```
# main.tf

output "a_listvariable_0" {
  value = var.simplelist
}

```

```

a_listvariable_0 = tolist([
  "item1",
  "item2",
  "item3",
])
```

### 2. Array output