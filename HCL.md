### The Absolute Basics

  * **Key-Value Pairs:**

    ```hcl
    # Your app's name, simple as that!
    app_name = "EpicService"
    # How many times you've restarted your dev environment today
    restarts_today = 7
    # Is it deployed? You bet your bytes it is!
    is_deployed = true
    ```

  * **Blocks:**
    Blocks are like nested folders. They keep things tidy.

    ```hcl
    # This is a 'server' block with a label 'web'.
    server "web" {
      ip_address = "192.168.1.10"
      os         = "Linux"
    }

    # Another 'server' block, this time for 'db'.
    server "db" {
      ip_address = "192.168.1.20"
      os         = "Ubuntu"
    }
    ```

  * **Comments:**

    ```hcl
    # This server handles all the cat GIFs.
    // Seriously, all of them.
    /*
     * Multi-line comment for when you have a lot
     * of wisdom to impart. Or just a long rant.
     */
    ```

-----

### Data Types

  * **Strings:**

    ```hcl
    greeting = "Hello, developer!"
    # Interpolation: 'cause dynamic strings are cool!
    full_greeting = "Hey there, ${var.user_name}! Ready to code?"
    ```

  * **Numbers:**

    ```hcl
    port = 8080
    version = 1.23
    ```

  * **Booleans:**

    ```hcl
    debug_mode = true
    production_ready = false
    ```

  * **Lists/Tuples:**

    ```hcl
    favorite_languages = ["Go", "Python", "JavaScript"]
    # Mix and match if you're feeling wild!
    mixed_bag = ["one", 2, true]
    ```

  * **Maps/Objects:**

    ```hcl
    user_details = {
      name  = "DevOps Dan"
      email = "dan@dev.com"
      role  = "Cloud Wizard"
    }
    ```

  * **Null:**

    ```hcl
    # Use this when a value is optional or not set.
    optional_setting = null
    ```

-----

### Expressions & Interpolation

  * **References: **

    ```hcl
    # Grab a variable's value
    region = var.aws_region
    # Grab an attribute from a deployed resource (e.g., from Terraform)
    server_ip = aws_instance.my_web_server.private_ip
    ```

  * **Operators:**
    All your basic math (`+`, `-`, `*`, `/`, `%`), comparisons (`==`, `!=`, `<`, `<=`, `>`, `>=`), and logic (`&&`, `||`, `!`).

    ```hcl
    is_admin = var.user_role == "admin" || var.user_id == "root"
    ```

  * **Conditional (Ternary):**
    `condition ? value_if_true : value_if_false`

    ```hcl
    instance_type = var.is_prod ? "m5.large" : "t3.micro"
    ```

  * **Interpolation:**

    ```hcl
    server_id = "app-${var.environment}-${count.index + 1}"
    ```

-----

### Built-in Functions

  * **Strings:**

      * `lower("HELLO")` -\> `"hello"`
      * `upper("world")` -\> `"WORLD"`
      * `join("-", ["app", "01"])` -\> `"app-01"`
      * `split(",", "a,b,c")` -\> `["a", "b", "c"]`

  * **Collections:**

      * `length(["a", "b", "c"])` -\> `3`
      * `element(["x", "y", "z"], 1)` -\> `"y"`
      * `lookup({a="1", b="2"}, "b", "default")` -\> `"2"`
      * `keys({a="1", b="2"})` -\> `["a", "b"]`

  * **Numeric:**

      * `abs(-5)` -\> `5`
      * `ceil(3.14)` -\> `4`
      * `floor(3.99)` -\> `3`

  * **File Handling:**

      * `file("path/to/script.sh")`
      * `fileexists("config.yaml")`

  * **Type Conversions & Encoding:**

      * `jsonencode({"name": "json_lover"})`
      * `yamlencode({"key": "yaml_data"})`

-----

### Variables

  * **Input Variables: **
    Define what users (or other configurations) can feed into the setup.

    ```hcl
    variable "region" {
      description = "Where in the cloud are we deploying?"
      type        = string
      default     = "us-east-1"
    }

    variable "feature_flags" {
      description = "A map of boolean flags for features."
      type        = map(bool)
      default = {
        new_dashboard = true
        beta_api      = false
      }
    }
    ```

      * **Access:** `var.region`, `var.feature_flags["beta_api"]`

  * **Local Variables (`locals` block):**
    Define values for internal reuse

    ```hcl
    locals {
      # Tags for all your resources – consistency FTW!
      default_tags = {
        ManagedBy   = "Terraform"
        Environment = var.environment
      }
      # A fancy name for your load balancer
      lb_name = "${var.project_name}-web-lb"
    }
    ```

      * **Access:** `local.default_tags`, `local.lb_name`

  * **Output Values (`output` block):**

    ```hcl
    output "load_balancer_dns" {
      description = "The public DNS name to hit your app!"
      value       = aws_lb.main.dns_name
    }

    output "instance_private_ips" {
      description = "All the juicy private IPs of your app servers."
      value       = aws_instance.app_servers[*].private_ip # The splat operator!
    }
    ```

-----

### Loops & Conditionals

  * **`for_each`:**
    If you have a list of things (like environments, regions, or services) and want to create *one resource per thing*

    ```hcl
    resource "aws_s3_bucket" "app_data" {
      for_each = toset(var.environments) # ["dev", "prod"]
      bucket   = "${each.key}-my-app-bucket"
      tags     = {
        Environment = each.key
      }
    }
    ```

      * **Inside `for_each`:** Use `each.key` and `each.value`.

  * **`for` Expression:**
    For filtering and/or creating lists

    ```hcl
    # Get just the IDs of your web servers
    web_server_ids = [for s in aws_instance.web_servers : s.id]

    # Create a map from instance name to private IP
    ip_map = { for instance in aws_instance.app_servers : instance.tags.Name => instance.private_ip }

    # Filtering a list (only prod users)
    prod_users = [for u in var.all_users : u if u.environment == "prod"]
    ```

  * **`dynamic` Block:**
    To dynamically create nested blocks, like security group rules or lifecycle policies.

    ```hcl
    resource "aws_security_group" "web_access" {
      name = "web-sg"

      dynamic "ingress" {
        for_each = var.allowed_ports # e.g., [80, 443, 22]
        content {
          from_port   = ingress.value
          to_port     = ingress.value
          protocol    = "tcp"
          cidr_blocks = ["0.0.0.0/0"]
        }
      }
    }
    ```

  * **`count`:**
    Still works, but `for_each` is generally preferred for resource creation as it gives more explicit control and easier deletion.

    ```hcl
    resource "aws_instance" "dev_vm" {
      count = var.num_dev_vms # If num_dev_vms is 3, you get dev_vm[0], dev_vm[1], dev_vm[2]
      ami           = "ami-xyz"
      instance_type = "t3.small"
      tags = {
        Name = "dev-vm-${count.index}" # count.index gives you the current iteration number
      }
    }
    ```

      * **Accessing instances:** `aws_instance.dev_vm[0].id`

-----

### Data Sources & Providers

  * **Data Sources (`data` block): Read Existing Stuff\!**
    Use data sources to fetch info about existing resources or external data.

    ```hcl
    data "aws_ami" "latest_ubuntu" {
      most_recent = true
      filter {
        name   = "name"
        values = ["ubuntu/images/hvm-ssd/ubuntu-jammy-22.04-amd64-server-*"]
      }
      owners = ["099720109477"] # Canonical's AWS account ID
    }

    output "ubuntu_ami_id" {
      value = data.aws_ami.latest_ubuntu.id
    }
    ```

  * **Provider Configuration (`provider` block):**

    ```hcl
    provider "aws" {
      region  = var.aws_region # Get region from an input variable
      profile = "dev-account"  # Use a specific AWS CLI profile
    }

    provider "kubernetes" {
      config_path = "~/.kube/config" # Point to your kubeconfig
    }
    ```

-----

### Modules

Break down complex configurations into smaller, reusable pieces. Think functions or classes for infrastructure.

  * **Using a Module:**
    ```hcl
    module "network" {
      # Local path to your module (e.g., in a 'modules' directory)
      source = "./modules/vpc"
      # Or from Git: "git::https://github.com/your-org/terraform-modules.git//vpc?ref=v1.0.0"
      # Or from Terraform Registry: "terraform-aws-modules/vpc/aws"

      # Pass variables to your module like you would to a resource
      vpc_cidr          = "10.0.0.0/16"
      public_subnet_cidrs = ["10.0.1.0/24", "10.0.2.0/24"]
      name              = "${var.project_name}-network"
    }
    ```