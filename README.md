# Atividade-AWS-Docker
<h3>2º Projeto para a Bolsa Compass DevSecOps.</h3>

<ul>
  <a href="https://github.com/Esvaber/Atividade-AWS-Docker?tab=readme-ov-file#descri%C3%A7%C3%A3o-da-atividade"><li>Descrição da Atividade</li></a>
    <a href="https://github.com/Esvaber/Atividade-AWS-Docker/tree/main?tab=readme-ov-file#configura%C3%A7%C3%A3o-da-inst%C3%A2ncia-ec2"><li>Configuração da instância EC2</li></a>
  <ul>
    <a href="https://github.com/Esvaber/Atividade-AWS-Docker/tree/main?tab=readme-ov-file#cria%C3%A7%C3%A3o-da-vpc"><li>Criação da VPC</li></a>
    <a href="https://github.com/Esvaber/Atividade-AWS-Docker/tree/main?tab=readme-ov-file#cria%C3%A7%C3%A3o-de-subnetss"><li>Criação de subnets</li></a>
    <a href="https://github.com/Esvaber/Atividade-AWS-Docker/tree/main?tab=readme-ov-file#cria%C3%A7%C3%A3o-de-route-table"><li>Criação de route table</li></a>
  </ul>
</ul>

## Descrição da Atividade
<ol>
  <li>Instalação e configuração do DOCKER ou CONTAINERD no host EC2</li>
  <ul>
    <li>Ponto adicional para o trabalho utilizar a instalação via script de Start Instance (user_data.sh)</li>
  </ul>
  <li>Efetuar Deploy de uma aplicação Wordpress com container de aplicação RDS database Mysql</li>
  <li>Configuração da utilização do serviço EFS AWS para estáticos do container de aplicação Wordpress</li>
  <li>Configuração do serviço de Load Balancer AWS para a aplicação Wordpress</li>
</ol>

<b>Seguir a topologia abaixo:</b>
<br></br>
<img src="https://github.com/Esvaber/Atividade-AWS-Docker/blob/main/Imagens/topologia.JPG">

<h4>Pontos de atenção</h4>
<ul>
  <li>Não utilizar ip público para saída do serviços WP (Evitem publicar o serviço WP via IP Público)</li>
  <li>Sugestão para o tráfego de internet sair pelo LB (Load Balancer Classic)</li>
  <li>Pastas públicas e estáticos do wordpress sugestão de utilizar o EFS (Elastic File Sistem)</li>
  <li>Fica a critério de cada integrante usar Dockerfile ou Dockercompose</li>
  <li>Necessário demonstrar a aplicação wordpress funcionando (tela de login)</li>
  <li>Aplicação Wordpress precisa estar rodando na porta 80 ou 8080</li>
  <li>Utilizar repositório git para versionamento</li>
</ul>

## Configuração da instância EC2

### Criação da VPC
Será criado uma VPC com as seguintes configurações:
<ul>
  <li>Name: <code>vpc-projeto</code></li>
  <li>IPv4 CIDR: <code>10.0.0.0/16</code></li>
  <li>IPv6 CIDR block: <code>No IPv6 CIDR block</code></li>
</ul>

### Criação de subnets
As subnets criadas serão vinculadas à VPC criada anteriormente: <code>vpc-projeto</code>
<table>
  <tr>
    <th>Nome</th>
    <th>AZ</th>
    <th>IPv4 subnet CIDR block</th>
  </tr>
  <tr>
    <td>subnet-publica-1</td>
    <td>US East (N. Virginia) / us-east-1a</td>
    <td>10.0.1.0/24</td>
  </tr>
  <tr>
    <td>subnet-privada-1</td>
    <td>US East (N. Virginia) / us-east-1b</td>
    <td>10.0.2.0/24</td>
  </tr>
  <tr>
    <td>subnet-privada-2</td>
    <td>US East (N. Virginia) / us-east-1c</td>
    <td>10.0.3.0/24</td>
  </tr>
</table>

### Criação de route table
Criar 2 route tables:
<ul>
  <li>Nome: <code>rt-publica-projeto</code></li>
  <li>VPC: <code>vpc-projeto</code></li>
</ul>
<ul>
  <li>Nome: <code>rt-privada-projeto</code></li>
  <li>VPC: <code>vpc-projeto</code></li>
</ul>

Após a criação fazer as associações das subnets criadas anteriormente: <code>subnet-publica-1</code>, <code>subnet-privada-1</code> e <code>subnet-privada-2</code>

### Criação do Internet Gateway
Será criado o Internet Gateway com o nome <code>ig-projeto</code>.<br></br>
Em seguida editar rotas e acrescentar as configurações abaixo:
<img src="https://github.com/Esvaber/Atividade-AWS-Docker/blob/main/Imagens/associa%C3%A7%C3%A3o_ig.JPG">









Fazer a criação de uma instância EC2 que vai ter as seguintes configurações:
<ul>
  <li>Sistema Operacional: Amazon Linux 2</li>
  <li>Família: t3.small</li>
  <li></li>
</ul>
