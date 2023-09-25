---
sort: 2
---

# Terraform Lists

### Input variables

```terraform
# variable.tf

variable "simplelist"{
    type = list(string)
}
```

```terraform
# variables.tfvars

simplelist = [ "item1", "item2", "item3" ]

```

### 1. Raw Output.

```terraform
# main.tf

output "a_listvariable_0" {
  value = var.simplelist
}

```

```terraform
# console$

a_listvariable_0 = tolist([
  "item1",
  "item2",
  "item3",
])
```

### 2. Array Output.

```terraform
# main.tf

output "a_listvariable_1" {
  value = [ for v in var.simplelist : v ]
}
```


```terraform
# console$

a_listvariable_1 = [
  "item1",
  "item2",
  "item3",
]

```


### 3. Array Output. Selecting a specific indexed element.

```terraform
# main.tf

output "a_listvariable_1" {
  value = [ for v in var.simplelist : v ][0]
}
```


```terraform
# console$

a_listvariable_2 = "item1"
```

### 4. Array Output. Selecting a specific indexed element.

```terraform
# main.tf

output "a_listvariable_3" {
  value = var.simplelist[0]
}
```

```terraform
# console$

a_listvariable_3 = "item1"
```

### 5. Array Output. Iterating over list.


```terraform
# main.tf

output "a_listvariable_4" {
  value = [ for i,v in var.simplelist : "Item ${i} is ${v}" ]
}
```

```terraform
# console$

a_listvariable_4 = [
  "Item 0 is item1",
  "Item 1 is item2",
  "Item 2 is item3",
]
```

### 6. Map Output. Iterating over list.

```terraform
# main.tf

output "a_listvariable_5" {
  value = { for i,v in var.simplelist : i => v}
}
```

```terraform
# console$

a_listvariable_5 = {
  "0" = "item1"
  "1" = "item2"
  "2" = "item3"
}
```

### 7. Map Output. Selecting specific index element.

```terraform
# main.tf

output "a_listvariable_6" {
  value = { for i,v in var.simplelist : i => v}[0]
}
```

```terraform
# console$

a_listvariable_6 = "item1"
```

### 8. Map Output.  Selecting specific map item.

```terraform
# main.tf

output "a_listvariable_7" {
  value = { for v in var.simplelist : v => v}["item1"]
}
```

```terraform
# console$

a_listvariable_7 = "item1"
```