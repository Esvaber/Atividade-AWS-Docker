# Atividade-AWS-Docker
<h3>2º Projeto para a Bolsa Compass DevSecOps.</h3>

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
<img src="https://github.com/Esvaber/Atividade-AWS-Docker/blob/main/topologia.JPG">

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
