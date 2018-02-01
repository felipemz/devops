# devops

README

Pergunta 1:

Para atender esta demanda, poderia ser utilizado o serviço do Rundeck instalado no próprio servidor ou apenas um nodo configurado. Seria feito uma validação para saber se o arquivo existe no local, o arquivo seria movido para o novo local a ser executado e depois no Rundeck seria executado o comando java -jar com o nome do arquivo recebido.

Para garantir o procedimento, o arquivo deve ser movido/excluido do FTP a cada execução. Desta forma é possível agendar diversas execuções no Rundeck mas garantindo que o processo só continua com a existência do arquivo no FTP.


Pergunta 2:

Como laboratório de testes, foi criado uma máquina na Digital Ocean. Um Ubuntu com 2 CPUs e 4Gb de RAM.

Nota: originalmente criei com 1 cpu e 2gb de ram, mas o minishift não iniciava, foi quando descobri que a configuração padrão dele
consome 2Gb de ram. Feito o upgrade na máquina ele iniciou normalmente, não testei com configuração inferior.

Para iniciar o procedimento, é recomendado acessar a página https://github.com/minishift/minishift . Nela contem as informações
necessárias para se instalar o Minishift.

Para utilizar o Minishift é necessário o driver KVM, neste laboratório foi utilizado o docker-machine-driver-kvm na versão 0.7.0.
Faça o PATH do driver KVM: copie o driver para /usr/local/bin/ e o configure como executável (chmod +x).
Após instalar o Minishift, faça o mesmo procedimento do PATH com o excutável minishift.
Você também vai precisar do Origin Client, a interface gráfica não fornece todas as ferramentas necessárias para administrar o Minishift.

Download do OC: https://github.com/openshift/origin/releases (A criação do PATH é da mesma forma que os anteriores).

Após os procedimentos e instalação, é possível iniciar o Minishift.

O início do Minishift pode ser visto na imagem Minishift_start.jpg neste repositório.

O usuário informado para entrar no sistema é developer. Para a senha, é necessário entrar com qualquer valor, e este valor será a
senha padrão deste usuário.

A imagem minishift_running.jpg mostra a tela de login do programa. A imagem Minishift_console.jpg mostra o sistema já logado.
O próximo passo é a instalação do Nginx, deve ser feita acessando o menu "Add to Project", Deploy Image.

A imagem Minishift_install_nginx.jpg demonstra como deve ser feito o download do container. 

Nota: Neste projeto, utilizei a imagem padrão do Nginx, mas o recomendado é você possuir um repositório próprio para armazenar suas imagens. Na imagem mostra um alerta sobre o Nginx utilizar acesso root. Por padrão o Minishift não permite este tipo de execução, sendo necessário editar a imagem docker para que ela execute com outro usuário, e somente então carrega-la no Minishift.

Para fazer o Nginx executar, fiz a alteração das permissões para permitir execução com usuário root.
ATENÇÂO: ISSO SÓ DEVE SER FEITO EM AMBIENTE DE TESTES! NUNCA EM PRODUÇÃO.

No console SSH, você deve logar como system:adm no projeto, neste teste my-project e depois permitir a execução de qualquer usuário:

#sudo oc login -u system:admin -n my-project

#sudo oc adm policy add-scc-to-user anyuid -n my-project -z default

A imagem Minishift_deploy_nginx.jpg mostra a aplicação funcionando dentro do Minishift.



Pergunta 3:

Uma rotina no CRON para executar em intervalos, de 1 em 1 minuto, para validar a existência do arquivo json gerado pelo Nginx. Caso não encontre o arquivo, envia um e-mail com a mensagem de sistema indisponível.

#!/bin/bash <br />
if [ ! -s filename ] <br />
then <br />
        echo "Arquivo nao existe" | mailx -s "Sistema indisponivel" abc@xyz.com <br />
else <br />
        echo "Arquivo encontrado" <br />
        fi <br />





