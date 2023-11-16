# Terraform Cheat Sheet
<p>The purpose of this cheat sheet is both a documentation resource of Terraform's most important commands, but also in direct response to interview questions I was recently asked in a job interview.  Terraform commands are quite extensive, but these should should cover most scenarios you might encounter.</p>

## Terraform Code Formatting/Validation
<p>Terraform validate validates your code and looks for any syntax errors in the configuration files.  Can be run explicitely, but run impliciltly during execuretion of 'terraform plan' or 'terraform apply' commands</p>

| Command     | Description |
| ----------- | ----------- |
| terraform fmt|Format code per Hashicorp Language standards|
| terraform fmt -check|Checks if input is formatted|
| terraform fmt -recursive|Formats the files in subdirectories.  By default, it only processes the given directory|
| ||
| terraform validate|Validate syntax of Terraform code.|
| terraform validate -backend-config='C:\directory_path'|Validates code syntax and downloads the provider plugins to specific directory|
| terraform validate -backend=false|Validate syntax of Terraform code, but skips the backend validation|

## terraform init
<p>First command run and executed within the users working directory.  Used to initialize the working directory, which contains the working Terraform configuration files.  Safe to run multiple times and should be when there are changes to the modiles, providers or backend providers</p>

| Command     | Description |
| ----------- | ----------- |
| terraform init|Intialization of the working terraform directory and download of terraform providers.  Can be executed as many times as desired|
| terraform init -get-plugins=false|Intialization of the working terraform directory and don't download of terraform providers|

## Plan
<p>Creates the execution plan and performs a refresh of the backend.  Additionally, determines which actions are needed to be performed to achieve desired infrastructure state.  If no changes detected, you will be notified Terraform is not performing any chagnges to the infrastructure.</p>

| Command     | Description |
| ----------- | ----------- |
| terraform plan|Command creates the execution plan.|
| terraform plan -input=true|Asks for input variable, if not directly set|
| terraform plan -out=path|Writes a plan file to a given path, which can be used as input to the apply command|
| terraform plan -state=statefile|Provides path to a Terraform state file, used to look up Terraform-managed resources|
| terraform plan -var 'foo-bar'|Sets variable in the Terraform configuraiton file.  Flag can be used multiple times|
| terraform plan -var-file="filename"|Sets variables in the configuration file.  If present, terraform.tfvars or .auto.tfvars files are automatically loaded and command not necessary|

## Apply
<p>Provisions or updates the infrastructure.  Additionally, updates the state file and can be stored in the local or remote backend.</p>

| Command     | Description |
| ----------- | ----------- |
| terraform apply|Provisions and updates the infrastructure.  Also updates the tf.state file, which is stored in the local or remote backend|
| terraform apply -auto-approve|Skip interactive approval of plan before applying|
| terraform apply -backup=path:|Provides path to backup of existing state file before modification|
| terraform apply -input=true|Reqeusts variable input, if not directly set|
| terraform apply -lock-timemout=0s|Duration to retry a lock state|
| terraform apply -refresh=true|Updates the state prior to checking for differences|
| terraform apply -target=resource|Targets a specific infrastructure resource|
| terraform apply -var 'foo-bar'|Sets variable in the Terraform configuraiton file.  Flag can be used multiple times|
| terraform apply -var-file="filename"|Sets variables in the configuration file.  If present, terraform.tfvars or .auto.tfvars files are automatically loaded and command not necessary|

## Destroy
<p>When executed, presents an execution plan for all resources to be deleted and askes for confirmation, before execution, as once executed, the process can't be undone.</p>

| Command     | Description |
| ----------- | ----------- |
| terraform destroy|Delete all defined resources in configuration file and update the state file as necessary|
| terraform destroy -auto-approve|Skip interactive approval of plan before applying|
| terraform destroy -backup=path:|Provides path to backup of existing state file before modification|
| terraform destroy -target=resource|Targets a specific infrastructure resource|
| terraform destroy -var 'foo-bar'|Sets variable in the Terraform configuraiton file.  Flag can be used multiple times|
| terraform destroy -var-file="filename"|Sets variables in the configuration file.  If present, terraform.tfvars or .auto.tfvars files are automatically loaded and command not necessary|

## Comments
| Command     | Description |
| ----------- | ----------- |
|#|Single line comment|
|//|Single line comment, but alterative to #|
|/* ... */| Multi-line comment|

## miscellaneous Commands
| Command     | Description |
| ----------- | ----------- |
| terraform version|Display version of Terraform.exe, you're currently using.  Warns if version is out of date|

## Terraform Module
<p>Terraform modules are set of configuration files in a single directory.  They're a very powerful way to re-use code and stick to the DRY 'Do Not Repeat Yoursself' principle</p>
<p>Modules help organize and provide re-usability of the configuration, providing consistency and ensuring best practices.</p>  
<p>Configuration file ending with .tf or .tf.json, consisting of resources, inputs, and outputs.  The main root module that has the main configuration file, can consume other modules.  Hierarchal structure is defined as: root module can ingest child module and child module can invoke other child modules.</p>

```
module.<rootmodulename>.module.<childmodulename>
```

```
module "module-name" {
  # path to module directory
  # source argument is mandatory for all modules
  source = "../modules/module_name"

  #module inputs
  vm_id = var.vm_id
  instance_type = var.instance_type
  servers = var.servers
}
```
