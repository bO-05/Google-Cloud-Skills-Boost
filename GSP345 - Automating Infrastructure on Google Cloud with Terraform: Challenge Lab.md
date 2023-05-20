# GSP345 - Automating Infrastructure on Google Cloud with Terraform: Challenge Lab.md
-----------------------------------------------------------------------------------------------------------------------------------------------------------------
## TASK 1

01. Create Terraform configuration files, run in cloud shell
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

02. Go to `variable.tf` and paste following content:
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

03. Add following to `main.tf`
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

04. Run in cloud shell
```
terraform init
```


-----------------------------------------------------------------------------------------------------------------------------------------------------------------
## TASK 2

01. Go to `modules/instances/instances.tf` then paste following content

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

02. Run in cloud shell

```
terraform import module.instances.google_compute_instance.tf-instance-1 <FILL*YOUR*INSTANCE_ID_#1*HERE>
```
```
terraform import module.instances.google_compute_instance.tf-instance-2 <FILL*YOUR*INSTANCE_ID_#2*HERE>
```
```
terraform plan && terraform apply
```

-----------------------------------------------------------------------------------------------------------------------------------------------------------------
## TASK 3 

01. Add following to `storage.tf`
```
resource "google_storage_bucket" "storage-bucket" {
  name          = "<FILL*YOUR*BUCKET_NAME*HERE>"
  location      = "US"
  force_destroy = true
  uniform_bucket_level_access = true
}
```

02. a. Add following to `main.tf`
```
module "storage" {
  source     = "./modules/storage"
}
```

02. b. Run in cloud shell 
```
terraform init && terraform apply
```

03. Make change at start in `main.tf`
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

04. Run in cloud shell
```
terraform init
```


---------------------------------------------------------------------------------------------------------------------------------------------------------------
## TASK 4
01. Edit the machine type of "tf-instance-1" in `instance.tf`
```
resource "google_compute_instance" "tf-instance-1" {
  name         = "tf-instance-1"
  machine_type = "<EDIT*MACHINE_TYPE*ACCORDINGLY*HERE>"
  ... (rest of code)
}
```

02. Edit the machine type of "tf-instance-2" in `instance.tf`
```
resource "google_compute_instance" "tf-instance-2" {
  name         = "tf-instance-2"
  machine_type = "<EDIT*MACHINE_TYPE*ACCORDINGLY*HERE>"
  ... (rest of code)
}
```

03. Add following to `instance.tf`
```
resource "google_compute_instance" "<FILL*YOUR*INSTANCE_3_NAME*HERE>" {
  name         = "<FILL*YOUR*INSTANCE_3_NAME*HERE>"
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

04. Run in cloud shell
```
terraform init && terraform apply
```


---------------------------------------------------------------------------------------------------------------------------------------------------------------
## TASK 5

01. Go to `instance.tf` & remove instance 3

02. Run in cloud shell
```
terraform init && terraform apply
```


---------------------------------------------------------------------------------------------------------------------------------------------------------------
## TASK 6

1. & 2. Add following to `main.tf`
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

03. Run in cloud shell
```
terraform init && terraform apply
```

04. a. ***Replace everything*** in `Instance.tf` with following
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

04. b. Run in cloud shell 
```
terraform init && terraform apply
```


--------------------------------------------------------------------------------------------------------------------------------------------------------------
## TASK 7

01. a. Add the following to `main.tf` 
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

01. b. Run in cloud shell
```
terraform init && terraform apply
```


--------------------------------------------------------------------------------------------------------------------------------------------------------------

DONE
