# Atividade-AWS-Docker
<h3>2º Projeto para a Bolsa Compass DevSecOps.</h3>

<ul>
  <a href="https://github.com/Esvaber/Atividade-AWS-Docker?tab=readme-ov-file#descri%C3%A7%C3%A3o-da-atividade"><li>Descrição da Atividade</li></a>
    <a href="https://github.com/Esvaber/Atividade-AWS-Docker/tree/main?tab=readme-ov-file#configura%C3%A7%C3%A3o-da-inst%C3%A2ncia-ec2"><li>Configuração da instância EC2</li></a>
  <ul>
    <a href="https://github.com/Esvaber/Atividade-AWS-Docker/tree/main?tab=readme-ov-file#cria%C3%A7%C3%A3o-da-vpc"><li>Criação da VPC</li></a>
    <a href="https://github.com/Esvaber/Atividade-AWS-Docker/tree/main?tab=readme-ov-file#cria%C3%A7%C3%A3o-de-subnetss"><li>Criação de subnets</li></a>
    <a href="https://github.com/Esvaber/Atividade-AWS-Docker/tree/main?tab=readme-ov-file#cria%C3%A7%C3%A3o-de-route-table"><li>Criação de route table</li></a>
    <li>Criação do Internet Gateway</li>
    <li>Criação de NAT Gateway</li>
  </ul>
  <li>Criação da RDS</li>
  <li>Criação do EFS</li>
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
  <tr>
    <td>subnet-publica-2</td>
    <td>US East (N. Virginia) / us-east-1c</td>
    <td>10.0.4.0/24</td>
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

Após a criação fazer as associações das subnets criadas anteriormente: <br></br>
subnet-publica-1 e subnet-publica-2 = <code>rt-publica-projeto</code> <br></br>
subnet-privada-1 e subnet-privada-2 = <code>rt-privada-projeto</code>

### Criação do Internet Gateway
Será criado o Internet Gateway com o nome <code>ig-projeto</code>.<br></br>
Em seguida editar rotas e acrescentar as configurações de Internet Gateway abaixo:
<img src="https://github.com/Esvaber/Atividade-AWS-Docker/blob/main/Imagens/associa%C3%A7%C3%A3o_ig.JPG">

### Criação de NAT Gateway
Criar um NAT Gateway com o nome <code>natgw-projeto</code> e associado à <code>subnet-publica-1</code>.
Usarei um IP Elástico já criado no Projeto anterior (52.202.207.69). <br></br>
Editar a tabela de rotas como a imagem abaixo:<br>
<img src="https://github.com/Esvaber/Atividade-AWS-Docker/blob/main/Imagens/NAT.JPG">







Fazer a criação de uma instância EC2 que vai ter as seguintes configurações:
<ul>
  <li>Sistema Operacional: Amazon Linux 2</li>
  <li>Família: t3.small</li>
  <li></li>
</ul>

### Criação da RDS
Usaremos a <code>Criação padrão</code>, escolhendo MySQL em <code>Opções de Mecanismo</code> e usaremos a versão <code>MySQL 8.0.35</code>.<br>
Estaremo usando o <code>Nível Gratuito</code> e identificaremos a base de dados com o nome <code>wordpress</code>.<br>
Após a escolha da Login e Senha, especificarei o uso da VPC <code>vpc-projeto</code>.<br>
Será selecionado o Grupo de Segurança criado para a RDS: <code>sgprojeto_rds</code>. Ela possui a regra de entrada abaixo:<br>
<img src="https://github.com/Esvaber/Atividade-AWS-Docker/blob/main/Imagens/sg_rds_regras.JPG">

### Criação do EFS
Criaremos um EFS com o nome <code>projeto-efs</code> e associado ao VPC <code>vpc-projeto</code>.<br>
Dentro das opções de personalização em <b>Acesso à Rede</b>, selecionaremos o security group <code>sgprojetos_efs</code>, criado com a seguinte configuração de entrada:<br>
<img src="">
