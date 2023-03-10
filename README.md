# App-docker-k8s

Deployment de uma aplicação simples utilizando o Jenkins e a entregando de forma automática em um cluster kubernetes.

Fora criado um Dockerfile "kubeapp/Dockerfile" utilizando uma imagem base node:slim para a aplicação e enviada para um repositório do Dockerhub. Além disso, para receber o Deployment da aplicação e eventualmente fazer a escalação, provisionado via terraform, um cluster kubernetes na digitalocean com 2 nodes e outra instância para o Jenkins. Para automatizar o processo, realizei um script yaml para instalação do necessário, através do Ansible, para rodar as aplicações na máquina virtual do jenkins -- docker, kubectl e o próprio jenkins. Após o término do provisionamento da infraestrutura na digitalocean, automaticamente o ansible entra e configura a máquina com os serviços supracitados.

Configurado no Jenkins um job tipo pipeline, plugins docker, docker-pipeline, kubernetes-cli, credenciais para dockerhub e arquivo de configuração do kubectl e um trigger quando o github recebe um push com as modificações do projeto e a pipeline se inicia. O processo do jenkins é: Registrar a alteração do usuário no código, empacotar via Dockerfile, enviar para o repositório, verificar se há alguma alteração na imagem no manifesto do kubernetes e a que está no dockerhub, e se houver, atualiza a imagem dos pods.

No cluster kubernetes foram feitos deployments da aplicação, um service para a expor via LoadBalancer, database, secrets e uma configuração do prometheus e grafana para fazer a coleta das métricas e as transmitir para dashboards do grafana, respectivamente.
Em resumo, a partir do momento do push para o github do usuário, tudo ocorre de forma automática e em segundos, a aplicação com suas devidas mudanças fica exposta através de um LoadBalancer e sendo monitorada via prometheus e grafana.
