# Terraform Project Structure

```text
blog/
├── environments/
│   ├── dev/
│   │   ├── main.tf
│   │   ├── variables.tf
│   │   ├── provider.tf
│   │   ├── outputs.tf
│   │   └── dev.tfvars
│   ├── staging/
│   │   └──  (same files as dev)
│   └── prod/
│       └──  (same files as dev)
└── modules/
    ├── network/
    │   ├── main.tf
    │   ├── variables.tf
    │   └── outputs.tf
    ├── compute/
    │   ├── main.tf
    │   ├── variables.tf
    │   └── outputs.tf
    └── data/
        ├── main.tf
        ├── variables.tf
        └── outputs.tf
```

## Folder Details

- **environments/**: Contains separate configurations for each environment:
  - **dev/**: Development setup with `main.tf`, `variables.tf`, `provider.tf`, `outputs.tf`, and `dev.tfvars`.
  - **staging/**: Staging setup, mirroring the dev folder with its own variable files.
  - **prod/**: Production setup, structured like dev and staging but with production values.

- **modules/**: Reusable Terraform modules:
  - **network/**: Defines networking resources (VPCs, subnets) via `main.tf`, `variables.tf`, and `outputs.tf`.
  - **compute/**: Manages compute resources (instances), with module inputs and outputs.
  - **data/**: Handles data resources (e.g., S3 buckets) through dedicated module files.

## Purpose and Best Practices

- **Environment Folders**: Separate configs for `dev`, `staging`, and `prod` allow isolated changes per environment.
- **Reusable Modules**: Centralize common infrastructure (network, compute, data) to avoid duplication and ensure consistency.
- **Scalability**: This layout supports adding new environments or modules without affecting existing code.
- **Maintainability**: Clear separation of concerns makes it easier to update variables, providers, and outputs independently.