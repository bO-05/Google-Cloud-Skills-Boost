............................
TASK 1 #1: create Terraform configuration files
```
touch main.tf
touch variables.tf
mkdir modules
cd modules
mkdir instances
cd instances
touch instances.tf
touch outputs.tf
touch variables.tf
cd ..
mkdir storage
cd storage
touch storage.tf
touch outputs.tf
touch variables.tf
cd
```

............................
TASK 1 #2: Go To `variable.tf` and paste following content:
```
variable "region" {
 default = "<FILL*YOUR*REGION*HERE>"
}
variable "zone" {
 default = "<FILL*YOUR*ZONE*HERE>"
}
variable "project_id" {
 default = "<FILL*YOUR*PROJECT_ID*HERE>"
}
```

............................
TASK 1 #3: Add following to `main.tf`
```
terraform {
  required_providers {
    google = {
      source = "hashicorp/google"
      version = "4.53.0"
    }
  }
}
provider "google" {
  project     = var.project_id
  region      = var.region
  zone        = var.zone
}
module "instances" {
  source     = "./modules/instances"
}
```

............................
TASK 1 #4: Run in cloudshell
```
terraform init
```


-----------------------------------------------------------------------------------------------------------------------------------------------------------------
............................
TASK 2 #1: Go to `modules/instances/instances.tf` then paste following content

```
resource "google_compute_instance" "tf-instance-1" {
  name         = "tf-instance-1"
  machine_type = "n1-standard-1"
  zone         = "<FILL*YOUR*ZONE*HERE>"

  boot_disk {
    initialize_params {
      image = "debian-cloud/debian-10"
    }
  }
  network_interface {
 network = "default"
  }
  metadata_startup_script = <<-EOT
        #!/bin/bash
    EOT
  allow_stopping_for_update = true
}
resource "google_compute_instance" "tf-instance-2" {
  name         = "tf-instance-2"
  machine_type = "n1-standard-1"
  zone         =  "<FILL*YOUR*ZONE*HERE>"

  boot_disk {
    initialize_params {
      image = "debian-cloud/debian-10"
    }
  }
  network_interface {
 network = "default"
  }
  metadata_startup_script = <<-EOT
        #!/bin/bash
    EOT
  allow_stopping_for_update = true
}
```

............................
TASK 2 #2: Run in cloud shell

`terraform import module.instances.google_compute_instance.tf-instance-1 <FILL*YOUR*INSTANCE_ID_#1*HERE>`
`terraform import module.instances.google_compute_instance.tf-instance-2 <FILL*YOUR*INSTANCE_ID_#2*HERE>`
`terraform plan && terraform apply`

-----------------------------------------------------------------------------------------------------------------------------------------------------------------
............................
TASK 3 #1: Add following to `storage.tf`
```
resource "google_storage_bucket" "storage-bucket" {
  name          = "<FILL*YOUR*BUCKET_NAME*HERE>"
  location      = "US"
  force_destroy = true
  uniform_bucket_level_access = true
}
```

............................
TASK 3 #2: a. Add following to `main.tf`
```
module "storage" {
  source     = "./modules/storage"
}
```

TASK 3 #2: b. Run cloud shell 
```
terraform init && terraform apply
```

............................
TASK 3 #3: Make change at start in `main.tf`
```
terraform {
  backend "gcs" {
    bucket  = "<FILL*YOUR*BUCKET_NAME*HERE>"
 prefix  = "terraform/state"
  }
  required_providers {
    google = {
      source = "hashicorp/google"
      version = "4.53.0"
    }
  }
}
```

............................
TASK 3 #4: run in cloud shell
```
terraform init
```


---------------------------------------------------------------------------------------------------------------------------------------------------------------
............................
TASK 4 #1-#3: Add following to `instance.tf`
```
resource "google_compute_instance" "<FILL*YOUR*INSTANCE_NAME*HERE>" {
  name         = "<FILL*YOUR*INSTANCE_NAME*HERE>"
  machine_type = "<FILL*YOUR*MACHINE_TYPE*HERE>"
  zone         = "<FILL*YOUR*ZONE*HERE>"

  boot_disk {
    initialize_params {
      image = "debian-cloud/debian-10"
    }
  }

  network_interface {
 network = "default"
  }
  metadata_startup_script = <<-EOT
        #!/bin/bash
    EOT
  allow_stopping_for_update = true
}
```

............................
TASK 4 #4: Run cloud shell
```
terraform init && terraform apply
```


---------------------------------------------------------------------------------------------------------------------------------------------------------------
............................
TASK 5 #1: Go to `instance.tf` & remove instance 3

............................
TASK 5 #2: Run cloud shell
```
terraform init && terraform apply
```


---------------------------------------------------------------------------------------------------------------------------------------------------------------
............................
Task 6 #1-#2: Add following to `main.tf`
```
module "vpc" {
    source  = "terraform-google-modules/network/google"
    version = "~> 6.0.0"

    project_id   = "<FILL*YOUR*PROJECT_ID*HERE>"
    network_name = "<FILL*YOUR*VPC_NAME*HERE>"
    routing_mode = "GLOBAL"

    subnets = [
        {
            subnet_name           = "subnet-01"
            subnet_ip             = "10.10.10.0/24"
            subnet_region         = "<FILL*YOUR*REGION*HERE>"
        },
        {
            subnet_name           = "subnet-02"
            subnet_ip             = "10.10.20.0/24"
            subnet_region         = "<FILL*YOUR*REGION*HERE>"
            subnet_private_access = "true"
            subnet_flow_logs      = "true"
            description           = "This subnet has a description"
        },
    ]
}
```

............................
Task 6 #3: Run cloud shell
```
terraform init && terraform apply
```

............................
Task 6 #4a: **Replace everything** in `Instance.tf` with following
```
resource "google_compute_instance" "tf-instance-1"{
  name         = "tf-instance-1"
  machine_type = "n1-standard-2"
  zone         = "<FILL*YOUR*ZONE*HERE>"

  boot_disk {
    initialize_params {
      image = "debian-cloud/debian-10"
    }
  }

  network_interface {
    network = "<FILL*YOUR*VPC_NAME*HERE>"
     subnetwork = "subnet-01"
  }
  metadata_startup_script = <<-EOT
        #!/bin/bash
    EOT
  allow_stopping_for_update = true
}

resource "google_compute_instance" "tf-instance-2"{
  name         = "tf-instance-2"
  machine_type = "n1-standard-2"
  zone         = "<FILL*YOUR*ZONE*HERE>"

  boot_disk {
    initialize_params {
      image = "debian-cloud/debian-10"
    }
  }

  network_interface {
    network = "<FILL*YOUR*VPC_NAME*HERE>"
     subnetwork = "subnet-02"
  }

  metadata_startup_script = <<-EOT
        #!/bin/bash
    EOT
  allow_stopping_for_update = true
}

module "vpc" {
    source  = "terraform-google-modules/network/google"
    version = "~> 6.0.0"

    project_id   = "<FILL*YOUR*PROJECT_ID*HERE>"
    network_name = "<FILL*YOUR*VPC_NAME*HERE>"
    routing_mode = "GLOBAL"

    subnets = [
        {
            subnet_name           = "subnet-01"
            subnet_ip             = "10.10.10.0/24"
            subnet_region         = "<FILL*YOUR*REGION*HERE>"
        },
        {
            subnet_name           = "subnet-02"
            subnet_ip             = "10.10.20.0/24"
            subnet_region         = "<FILL*YOUR*REGION*HERE>"
            subnet_private_access = "true"
            subnet_flow_logs      = "true"
            description           = "This subnet has a description"
        },
    ]
}
```

............................
Task 6 #4b: run cloud shell 
```
terraform init && terraform apply
```


--------------------------------------------------------------------------------------------------------------------------------------------------------------
............................
Task 7 #1a: Add the following to `main.tf` 
```
resource "google_compute_firewall" "tf-firewall"{
  name    = "tf-firewall"
 network = "projects/<FILL*YOUR*PROJECT_ID*HERE>/global/networks/<FILL*YOUR*VPC*NAME*HERE>"

  allow {
    protocol = "tcp"
    ports    = ["80"]
  }

  source_tags = ["web"]
  source_ranges = ["0.0.0.0/0"]
}
```

............................

Task 7 #1b: run cloud shell
```
terraform init && terraform apply
```


--------------------------------------------------------------------------------------------------------------------------------------------------------------

DONE
