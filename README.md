#Terraform, EKS, Wordpress, RDS

#1.Ferramentas necessárias#

Requisitos básicos do projeto:
- a) Conta no console AWS
- b) AWS CLI Instalado - https://aws.amazon.com/pt/cli/
- c) Ferramenta Kubectl - https://kubernetes.io/docs/tasks/tools/
- d) Terraform instalado e configurado - https://www.terraform.io/downloads.html


#2. Clonar e preparar#

1 - Execute o comando para clonar o projeto:
*********

2 - Acessar o repositório
eks-terraform-k8s

3 - Utilizar o serviço Key Management Service (KMS) da Amazon para criar uma chave única de acesso a ser utilzada na instalação.

4 - Para compartilhamento seguro os dados de acesso ao banco de dados contidos no arquivo db-creds.yml, podem ser encriptados utilizando o comando KMS:
aws kms encrypt --key-id <YOUR KMS KEY> --region <AWS REGION> --plaintext fileb://db-creds.yml --output text --query CiphertextBlob > db-creds.yml.encrypted

5 - Copiar o novo arquivo criado (db-creds.yml.encrypted) e renomear com o seguinte nome: rds-creds.yml.encrypted

6 - Estando no repositório do projeto executar através do prompt o seguinte comando
Terraform init

#3.Ambientes de Teste e Produção#

Para criar esses dois ambientes, usamos exatamente o mesmo código, mas criamos arquivos de variáveis ​​diferentes para ambientes de teste e produção, portanto, ao criar qualquer ambiente, basta passar o arquivo de variável correspondente:

7 - creating workspace for testing
terraform workspace new testing

8 - selecting testing workspace
terraform workspace select testing

#4. Aplicando o projeto#

Estando no repositório do projeto, execute no prompt:
9 - terraform apply -var-file=testing.tfvars

#5. Apagando o projeto#
10 - terraform destroy -var-file=testing.tfvars

#6. Mapear cluster localmente#

Execute estando em qualquer diretório dentro do prompt
aws eks --region <region-code> update-kubeconfig --name <cluster_name>


#7. Comandos Kubectl#

Verificar todos os namespaces
kubectl get namespaces

Verificar todos os pods de todos os namespaces
kubectl get pods --all-namespaces

Mudar o contexto default a ser visualizado, mudando para wordpress-rds
kubectl config set-context --current --namespace=wordpress-rds

Verificar informações namespace especifico (wordpress-rds)
kubectl get pods -n wordpress-rds
kubectl get nodes -n wordpress-rds
kubectl get svc -n wordpress-rds
kubectl get deploy -n wordpress-rds
