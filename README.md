# Terraform, EKS, Wordpress, RDS

## 1 - Ferramentas necessárias #

Requisitos básicos do projeto:
- a) Conta no console AWS
- b) AWS CLI Instalado - https://aws.amazon.com/pt/cli/
- c) Ferramenta Kubectl - https://kubernetes.io/docs/tasks/tools/
- d) Terraform instalado e configurado - https://www.terraform.io/downloads.html


## 2 - Clonar e preparar #

Execute o comando para clonar o projeto:<br>
_**git clone https://<span></span>github.com/washingtondacosta/eks-terraform-k8s.git**_

Acessar o repositório<br>
_**eks-terraform-k8s**_

Utilizar o serviço Key Management Service (KMS) da Amazon para criar uma chave única de acesso a ser utilzada na instalação.

Para compartilhamento seguro os dados de acesso ao banco de dados contidos no arquivo db-creds.yml, podem ser encriptados utilizando o comando KMS:<br>
_**aws kms encrypt --key-id <YOUR KMS KEY> --region <AWS REGION> --plaintext fileb://db-creds.yml --output text --query CiphertextBlob > db-creds.yml.encrypted**_

Copiar o novo arquivo criado (db-creds.yml.encrypted) e renomear com o seguinte nome: rds-creds.yml.encrypted

Estando no repositório do projeto executar através do prompt o seguinte comando<br>
_**Terraform init**_

## 3 - Ambientes de Teste e Produção #

Para criar esses dois ambientes, usamos exatamente o mesmo código, mas criamos arquivos de variáveis ​​diferentes para ambientes de teste e produção, portanto, ao criar qualquer ambiente, basta passar o arquivo de variável correspondente:

Criando workspace para testing<br>
_**terraform workspace new testing**_

Selecionando workspace testing<br>
_**terraform workspace select testing**_

## 4 - Aplicando o projeto #

Estando no repositório do projeto, execute no prompt:<br>
_**terraform apply -var-file=testing.tfvars**_

## 5 - Apagando o projeto #
Estando no repositório do projeto, execute no prompt:<br>
_**terraform destroy -var-file=testing.tfvars**_

## 6 - Mapear cluster localmente #

Execute estando em qualquer diretório dentro do prompt<br>
_**aws eks --region <region-code> update-kubeconfig --name <cluster_name>**_


## 7 - Comandos Kubectl #

Verificar todos os namespaces<br>
_**kubectl get namespaces**_

Verificar todos os pods de todos os namespaces<br>
_**kubectl get pods --all-namespaces**_

Mudar o contexto default a ser visualizado, mudando para wordpress-rds<br>
_**kubectl config set-context --current --namespace=wordpress-rds**_

Verificar informações namespace especifico (wordpress-rds)<br>
_**kubectl get pods -n wordpress-rds**_<br>
_**kubectl get nodes -n wordpress-rds**_<br>
_**kubectl get svc -n wordpress-rds**_<br>
_**kubectl get deploy -n wordpress-rds**_

