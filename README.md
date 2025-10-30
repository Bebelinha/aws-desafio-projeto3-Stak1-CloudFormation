# Implementando a minha primeira Stake com AWS CloudFormation

Este repositório contém a minha primeira implementação de uma Stake no AWS Cloud Formation.

O Cloud Formation é um recurso da AWS que permite gerar códigos para fazer auomação de implementação de recursos como EC2, S3 , Lambda, RDS etc.

Esses códigos podem ser gerados em arquivos forma de templates em JSON e YAML auxiliando na estruturação de recursos de infraestrutura.

No CloudFormation você paga apenas pelas stakes que são criadas.

Nessa primeira stake foi criado um simples servidor Web EC2 constando uma simples página em HTML com a seguinte mensagem "Meu Servidor Web EC2 com Apache no CloudFormation!".

Foi também criado um Grupo de Segurança (Security Group) com liberação da porta 80 80, que é referente ao HTTP e a porta 22 para acesso via SSH.

O servidor EC2 foi do tipo t3.micro com o Sistema Operacional Amazon Linux 2023 x86_64 Kernel 6.1, com 2vCPU 1 GB, com 8GB de memória RAM. Esse servidor foi criado na região Norte da Virgínia (us-east-1).


Os prints do passo a passo da Stake e o arquivo em YAML constam na pasta images.


Código em YAML gerado da automação do servidor Web:

```
AWSTemplateFormatVersion: '2010-09-09'
Description: "Desafio DIO - Criação de instância EC2 com Apache e Firewall (Security Group)"

Resources:
  WebServerSecurityGroup:
    Type: AWS::EC2::SecurityGroup
    Properties:
      GroupDescription: "Permitir acesso SSH e HTTP"
      SecurityGroupIngress:
        - IpProtocol: tcp
          FromPort: 22
          ToPort: 22
          CidrIp: 0.0.0.0/0
        - IpProtocol: tcp
          FromPort: 80
          ToPort: 80
          CidrIp: 0.0.0.0/0

  WebServerInstance:
    Type: AWS::EC2::Instance
    Properties:
      ImageId: ami-07860a2d7eb515d9a  
      InstanceType: t3.micro          
      SecurityGroupIds:
        - !Ref WebServerSecurityGroup
      KeyName: Name 
      UserData:
        Fn::Base64: |
          #!/bin/bash
          yum update -y
          yum install -y httpd
          systemctl start httpd
          systemctl enable httpd
          echo "<h1>Meu Servidor Web EC2 com Apache no CloudFormation!</h1>" > /var/www/html/index.html
      Tags:
        - Key: Name
          Value: ServidorWeb
```
