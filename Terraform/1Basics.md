
# 🚀 Terraform Blocks & Commands — Complete Beginner’s README

> This is a **from-zero** guide. We’ll explain *every* important Terraform block, why it exists, how to use it, and what happens when you run each command. Includes mini tasks so you can practice.

---

## 1) What is Terraform? (and how it “thinks”)

**Terraform** is an *Infrastructure as Code (IaC)* tool by HashiCorp. You write configuration files (`.tf`), and Terraform makes your real cloud match that desired configuration.

- **Declarative**: you describe the end state, not step-by-step scripts.
- **Idempotent**: you can re-apply safely; Terraform figures out the delta.
- **Provider-based**: AWS, Azure, GCP, Kubernetes, GitHub, etc.
- **Stateful**: Terraform keeps a **state file** (usually `terraform.tfstate`) to remember what it created and map resources in your code to real resources in the cloud.

**How it works (high-level):**
1. You write `.tf` files (desired state).
2. `terraform plan` compares desired vs actual and proposes changes.
3. `terraform apply` executes those changes via **provider plugins**.
4. The **state file** is updated so Terraform knows what exists next time.

> ⚠️ Treat the state file like a secret: it can contain sensitive info (IDs, IPs, some passwords).


---

## 2) Why use Terraform?

- **Automation & Speed**: No more manual clicking in consoles.
- **Consistency**: Same code → same infra across dev/stage/prod.
- **Collaboration**: Version control (Git), code reviews, CI/CD.
- **Multi-cloud**: One tool, many providers.
- **Safety**: See a plan before you apply; destroy everything cleanly.
- **Modularity**: Reuse modules and standardize best practices.


---

## 3) Anatomy of Terraform Configuration (All the Important Blocks)

Terraform configs are **blocks** written in HCL (HashiCorp Configuration Language). A block looks like:

```
block_type "LABEL_1" "LABEL_2" {
  argument = value
  nested_block { ... }
}
```

Below are the essential block types you will encounter.

---

### A. `terraform` BLOCK (Terraform’s own settings)

Used to set **Terraform version**, **required providers**, and **backend** (where to store state).

```hcl
terraform {
  required_version = ">= 1.5.0"

  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }

  backend "s3" {
    bucket         = "my-tf-state-bucket"
    key            = "env/dev/terraform.tfstate"
    region         = "ap-south-1"
    dynamodb_table = "my-tf-locks"   # for state locking
    encrypt        = true
  }
}
```

**What it does:**
- `required_version`: Prevents using too-old Terraform.
- `required_providers`: Pins provider versions (avoids surprises).
- `backend`: Configures **remote state storage** (S3 recommended for teams), and **locking** (DynamoDB).

> 🧠 Backend is **not** a resource. It’s how Terraform stores its state. When you run `init`, Terraform configures this and may ask to migrate local state to remote.

**Alternative (Terraform Cloud / Enterprise):**
```hcl
terraform {
  cloud {
    organization = "your-org"
    workspaces { name = "dev" }
  }
}
```

**Task A**
- Create an S3 bucket and DynamoDB table (via console or Terraform later).
- Configure the `backend "s3"` block and run `terraform init`. Notice how Terraform asks to migrate state.


---

### B. `provider` BLOCK (cloud/API configuration)

Defines how to connect to a provider (like AWS).

```hcl
provider "aws" {
  region = var.region

  default_tags {
    tags = {
      Project = "Demo"
      Owner   = "sohel"
    }
  }
}
```

**Advanced – Multiple Provider Instances (aliases):**
```hcl
provider "aws" {
  alias  = "use1"
  region = "us-east-1"
}

provider "aws" {
  alias  = "aps1"
  region = "ap-south-1"
}

resource "aws_s3_bucket" "logs" {
  provider = aws.use1   # choose which provider to use
  bucket   = "my-logs-bucket-12345"
}
```

**Task B**
- Configure the AWS provider with your region.
- (Optional) Create two provider aliases and use one for a simple resource.


---

### C. `resource` BLOCK (creates or manages real infrastructure)

This is the heart of Terraform.

```hcl
resource "aws_vpc" "main" {
  cidr_block           = "10.0.0.0/16"
  enable_dns_support   = true
  enable_dns_hostnames = true

  tags = { Name = "tf-vpc" }
}
```

**Meta-arguments you’ll often use:**
- `count`: create N copies.
- `for_each`: create 1-per-key from a map/set (preferred over `count`).
- `depends_on`: force dependency (Terraform usually infers this).
- `provider`: choose a specific provider instance.
- `lifecycle`: fine-tune creation/deletion behavior.

```hcl
resource "aws_subnet" "this" {
  for_each          = { web = "10.0.1.0/24", app = "10.0.2.0/24" }
  vpc_id            = aws_vpc.main.id
  cidr_block        = each.value
  availability_zone = "ap-south-1a"

  tags = { Name = "subnet-${each.key}" }

  lifecycle {
    prevent_destroy       = false
    create_before_destroy = true
    ignore_changes        = [tags]
    replace_triggered_by  = [aws_vpc.main.id]
  }
}
```

**Nested blocks commonly seen inside resources:**
- `lifecycle {}` (as above)
- `provisioner {}` (use sparingly; prefer user_data or config management tools)
- `connection {}` (for provisioners)
- Resource-specific nested blocks (e.g., `ingress` inside `aws_security_group`)

**Dynamic nested blocks** (generate repeated child blocks from variables):
```hcl
variable "ingress_rules" {
  type = list(object({
    from = number
    to   = number
    cidr = string
  }))
  default = [
    { from = 22, to = 22, cidr = "0.0.0.0/0" },
    { from = 80, to = 80, cidr = "0.0.0.0/0" }
  ]
}

resource "aws_security_group" "web" {
  name   = "web-sg"
  vpc_id = aws_vpc.main.id

  dynamic "ingress" {
    for_each = var.ingress_rules
    content {
      from_port   = ingress.value.from
      to_port     = ingress.value.to
      protocol    = "tcp"
      cidr_blocks = [ingress.value.cidr]
    }
  }
}
```

**Task C**
- Create an `aws_vpc`.
- Add 2 subnets using `for_each`.
- Create a security group with `dynamic` ingress rules.


---

### D. `data` BLOCK (reads existing data from a provider)

Use **data sources** to *fetch* information without creating anything.

```hcl
data "aws_availability_zones" "available" {
  state = "available"
}

data "aws_ami" "amazon_linux" {
  most_recent = true
  owners      = ["amazon"]
  filter {
    name   = "name"
    values = ["al2023-ami-*-x86_64"]
  }
}
```

**Use cases:**
- Get the latest AMI ID, AZ names, caller identity, existing VPC IDs, etc.
- Feed these into your `resource` blocks.

**Task D**
- Fetch `aws_availability_zones` and print them in an output.


---

### E. `variable` BLOCK (inputs)

Define inputs to make code reusable. You can set them via:
- `terraform.tfvars` / `*.auto.tfvars` files
- CLI flags `-var / -var-file`
- Env vars `TF_VAR_<name>`

```hcl
variable "region" {
  description = "AWS region"
  type        = string
  default     = "ap-south-1"
}

variable "subnets" {
  description = "Map of subnet CIDRs"
  type        = map(string)
  default     = { web = "10.0.1.0/24", app = "10.0.2.0/24" }
  validation {
    condition     = alltrue([for cidr in values(var.subnets) : can(cidrnetmask(cidr))])
    error_message = "Each subnet must be a valid CIDR."
  }
}
```

**Task E**
- Add a `variable` for VPC CIDR with validation.
- Pass it using a `terraform.tfvars` file.


---

### F. `locals` BLOCK (computed values)

Use locals to avoid repeating expressions.

```hcl
locals {
  project          = "shop"
  name_prefix      = "${local.project}-dev"
  subnet_cidrs     = ["10.0.1.0/24", "10.0.2.0/24"]
  subnet_cidrs_map = { for i, cidr in local.subnet_cidrs : "sn${i+1}" => cidr }
}
```

**Task F**
- Build a `locals.subnet_cidrs_map` from a list and use it with `for_each`.


---

### G. `output` BLOCK (expose values after apply)

```hcl
output "vpc_id" {
  value       = aws_vpc.main.id
  description = "The VPC ID"
}

output "subnet_ids" {
  value     = [for s in aws_subnet.this : s.id]
  sensitive = false
}
```

**Task G**
- Output VPC ID, subnet IDs, and your chosen AMI ID from a data source.


---

### H. `module` BLOCK (reuse code packages)

Modules let you organize and reuse infrastructure patterns. Modules can come from:
- Local paths, Terraform Registry, Git URLs.

**Calling a published module (example: VPC):**
```hcl
module "vpc" {
  source  = "terraform-aws-modules/vpc/aws"
  version = "~> 5.0"

  name = "demo-vpc"
  cidr = "10.0.0.0/16"

  azs             = ["ap-south-1a", "ap-south-1b"]
  private_subnets = ["10.0.1.0/24", "10.0.2.0/24"]
  public_subnets  = ["10.0.11.0/24", "10.0.12.0/24"]
  enable_nat_gateway = true
}
```

**Local module (your own code):**
```
root/
├─ main.tf
└─ modules/
   └─ network/
      ├─ main.tf
      ├─ variables.tf
      └─ outputs.tf
```

```hcl
module "network" {
  source   = "./modules/network"
  vpc_cidr = "10.1.0.0/16"
}
```

**Task H**
- Create a local `modules/network` module that creates a VPC + 2 subnets.
- Call it from root and output the VPC ID from `module.network`.


---

### I. `moved` BLOCK (safe refactors without recreating)

When you rename or move resources in code, use `moved` to keep state aligned and avoid destroy/recreate.

```hcl
# Before (old address):
# resource "aws_vpc" "main" { ... }

# After renaming to "core":
resource "aws_vpc" "core" { ... }

moved {
  from = aws_vpc.main
  to   = aws_vpc.core
}
```

**Task I**
- Rename a resource (e.g., `aws_vpc.main` → `aws_vpc.core`) and add a `moved` block.
- Run `plan` and confirm no destroy/create, only an address move.


---

## 4) Backends & State — What’s Actually Happening?

**Local backend** (default):  
- State is stored in `terraform.tfstate` in your project folder.

**Remote backend (S3 + DynamoDB)** (recommended for teams):  
- State lives in S3 (versioned, encrypted).  
- Locking via DynamoDB prevents two people from applying at once.  
- On `terraform init`, Terraform configures or migrates the backend.

**What happens during common actions:**
- `init`: Downloads providers to `.terraform/`, configures backend, may migrate state.
- `plan`: Reads state, queries real cloud, shows proposed changes.
- `apply`: Creates a dependency graph, locks state, makes provider API calls, writes new state.
- `destroy`: Same as apply but deletes in a safe order.
- `show`: Renders the current state.
- `state` subcommands: Low-level surgery on state (be careful).


---

## 5) Basic (and Essential) Commands — With Explanations

### `terraform fmt`
Formats your `.tf` files to a standard style.

### `terraform init`
Initializes providers and backend. Creates `.terraform/` folder and `.terraform.lock.hcl` (locks provider versions). If backend differs, prompts to migrate state.

### `terraform validate`
Static check for configuration errors (types, unknown arguments, etc.).

### `terraform plan`
Compares desired state (code) vs actual state (cloud + state file). Prints the execution plan.  
Tips:
- `terraform plan -out plan.bin` saves a plan file you can apply later.
- `terraform plan -var-file=dev.tfvars` to use specific values.

### `terraform apply`
Executes the plan and updates state.  
Tips:
- `terraform apply plan.bin` applies a previously saved plan.
- `terraform apply -replace=aws_instance.web` forces recreation of a resource (modern replacement for `taint`).

### `terraform destroy`
Deletes all managed resources. Use with caution.
- `terraform destroy -target=aws_s3_bucket.logs` destroys only a target (advanced).

### `terraform show`
Displays current state in human-readable form. `terraform show -json` for machine-readable.

### `terraform state *`
Low-level commands to inspect/modify state:
- `terraform state list`
- `terraform state show <addr>`
- `terraform state mv <from> <to>`
- `terraform state rm <addr>`

### `terraform import`
Brings an **existing** real resource under Terraform management.  
> Note: Import **does not** create code for you — you must write the `resource` block, then run `import` to attach state.
```
terraform import aws_vpc.main vpc-0abc123def
```

### `terraform workspace *`
Lightweight separation of state (dev/stage/prod). Prefer separate backends/projects for serious environments, but workspaces are fine for sandboxes.
- `terraform workspace new dev`
- `terraform workspace select dev`
- `terraform workspace list`

### Useful env vars
- `TF_LOG=INFO` (or DEBUG, TRACE) for troubleshooting.
- `TF_VAR_<name>` to pass variables via environment (e.g., `TF_VAR_region=ap-south-1`).


---

## 6) Hands-on Practice Checklist

- [ ] Initialize a project with `terraform` + `provider` blocks.
- [ ] Create a VPC (`resource`) with tags.
- [ ] Fetch available AZs with a `data` source and output them.
- [ ] Add `variable` + validation for VPC CIDR.
- [ ] Use `locals` to compute a name prefix.
- [ ] Create subnets with `for_each` from a map built in `locals`.
- [ ] Write `output`s for VPC ID and subnet IDs.
- [ ] Move/rename a resource and use a `moved` block.
- [ ] Extract to a local `module` and call it from root.
- [ ] Configure an S3 + DynamoDB backend and re-`init`.
- [ ] Practice `plan`, `apply`, `show`, `state list`, `import`, and `destroy`.


---

## 7) Common Pitfalls & Tips

- **Don’t commit raw state** to Git when using a local backend. Prefer remote (S3/Terraform Cloud).
- **Pin provider versions** to avoid accidental upgrades.
- Use **`for_each`** over `count` when possible for stable addressing.
- Add **validation** blocks for critical variables (CIDR, allowed values).
- Use **`-replace=`** in `apply` to recreate a resource safely.
- Keep **provisioners** as a last resort; prefer user data or config tools (Ansible, cloud-init).


---

## 8) Mini Reference (Block Cheatsheet)

- `terraform { required_version, required_providers, backend, cloud }`
- `provider "<name>" { region, alias, default_tags, ... }`
- `resource "<type>" "<name>" { args...  meta: count, for_each, depends_on, provider, lifecycle }`
- `data "<type>" "<name>" { args... }`
- `variable "<name>" { type, default, description, validation, sensitive }`
- `locals { name = expr, ... }`
- `output "<name>" { value, description, sensitive, depends_on }`
- `module "<name>" { source, version, inputs... }`
- `moved { from = <addr>, to = <addr> }`
- `dynamic "<blockname>" { for_each, content { ... } }`


---

### You’re Ready
With these blocks and commands, you can read, understand, and build almost all common Terraform setups with confidence.
