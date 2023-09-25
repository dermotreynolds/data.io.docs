---
sort: 4
---

# Terraform Count vs For_Each


```danger
Dont use count.
```

### 1. Terraform Count.

```terraform
# main.tf

resource "null_resource" "listvariable_count" {
    count = length(var.simplelist)
     triggers = {
        value =  var.simplelist[count.index]
    }   
}
```

```terraform
# console$

# null_resource.listvariable_count[0] will be created
  + resource "null_resource" "listvariable_count" {
      + id       = (known after apply)
      + triggers = {
          + "value" = "item1"
        }
    }

  # null_resource.listvariable_count[1] will be created
  + resource "null_resource" "listvariable_count" {
      + id       = (known after apply)
      + triggers = {
          + "value" = "item2"
        }
    }

  # null_resource.listvariable_count[2] will be created
  + resource "null_resource" "listvariable_count" {
      + id       = (known after apply)
      + triggers = {
          + "value" = "item3"
        }
    }
```

### 2. Terraform For_each.

```terraform
# main.tf

resource "null_resource" "listvariable_for_each" {
    for_each = {for v in var.simplelist: v => v}
     triggers = {
        value =  each.value
    }   
}
```

```terraform
# console$

  # null_resource.listvariable_for_each["item1"] will be created
  + resource "null_resource" "listvariable_for_each" {
      + id       = (known after apply)
      + triggers = {
          + "value" = "item1"
        }
    }

  # null_resource.listvariable_for_each["item2"] will be created
  + resource "null_resource" "listvariable_for_each" {
      + id       = (known after apply)
      + triggers = {
          + "value" = "item2"
        }
    }

  # null_resource.listvariable_for_each["item3"] will be created
  + resource "null_resource" "listvariable_for_each" {
      + id       = (known after apply)
      + triggers = {
          + "value" = "item3"
        }
    }

``````