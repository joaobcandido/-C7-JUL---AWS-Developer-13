# Laboratório – Explorando a AWS com o Amazon EC2

## Resumo do laboratório

Esse laboratório irá te fornecer uma visão básica e prática do que é uma instância EC2,
como inicializá-la pelo console e via CloudShell.
O Amazon Elastic Compute Cloud é um serviço que fornece poder computacional, com
máquinas virtuais (ou também chamadas de VMs) que irão funcionar como um
computador físico, para as mais diversas funções. Foi feito para que, sempre que
desenvolvedores precisarem de poder computacional, podem providenciar facilmente e em
poucos minutos.

### Inicializar instância através do CloudShell

### 1- criando as variaveis:
````bash
NOME_INSTANCIA="instancia-joaocandido" \
NOME_INSTANCIA="instancia-joaocandido" \
PAR_CHAVE="parchave-joaocandido"
````
### 2- criando o grupo de segurança:
````bash
SECURITY_GROUP_ID=$(aws ec2 create-security-group \
--group-name $GRUPO_SEGURANCA \
--description "Permitir HTTP" \ 
--query "GroupId" \
--output text)
````
### 3- criando as autorizaçoes:
````bash
aws ec2 authorize-security-group-ingress \
--group-id $SECURITY_GROUP_ID \
--protocol tcp --port 80 --cidr 0.0.0.0/0
````
### 4- criando a instancia ec2:
````bash
aws ec2 run-instances 
--instance-type t2.micro \
--image-id $(aws ssm get-parameters-by-path \
--path "/aws/service/ami-amazon-linux-latest" \ 
--query "Parameters[?ends_with(Name, 'al2023-ami-kernel-default-x86_64')].Value" \
--output text) \ 
--security-group-ids $SECURITY_GROUP_ID \ 
--tag-specifications "ResourceType=instance,Tags=[{Key=Name,Value='$NOME_INSTANCIA'}]" \
--key-name $PAR_CHAVE \
--user-data "IyEvYmluL2Jhc2gKeXVtIC15IGluc3RhbGwgaHR0cGQKc3lzdGVtY3RsIGVuYWJsZ
SBodHRwZApzeXN0ZW1jdGwgc3RhcnQgaHR0cGQKZWNobyAnPGh0bWw+PGg
xPk9sw6EgZG8gc2V1IHNlcnZpZG9yIHdlYiE8L2gxPjwvaHRtbD4nID4gL3Zhci93d3
cvaHRtbC9pbmRleC5odG1sCg=="
````




