# proget-server-docker-jenkins

# Objetivo
Configurar um simples pipeline CI/CD com Jenkins, Docker, Github e Proget em localhost.

# Premissa
Foi utilizado a idéia da branch [proget-server-docker](https://github.com/phillrog/proget-server-docker) partindo desse ponto será o jenkins que publicara a imagem no registry localhost. Então siga as instruções para rodar o e configurar um docker-registry servidor Proget .

# Observações
Foi criado a branch [portal](https://github.com/phillrog/proget-server-docker-jenkins-portal) que é o repositório de uma aplicação angular que foi transformada em uma imagem Docker, configurada através do arquivo ```Dockerfile```. Então a imagem que será publicada se originará deste projeto mas para funcionar na sua máquina é necessário criar uma branch semelhante seu para utilizar na esteira.

# Como executar ?

## Passo 1 - Iniciar conatiners
Rode ```docker-compose up ``` na raiz deste projeto

## Passo 2 - Configurações de plugins no jenkins
Informe a chave do jenkins, cadastre um usuário, instale os plugins padrões e adicione:

<table id="plugins" class="jenkins-table sortable">
  <thead>
    <tr>
      <th initialsortdir="down"><a href="#" class="sortheader">Nome<span class="sortarrow">&nbsp;&nbsp;↓</span></a></th>
    </tr>
  </thead>
  <tbody>
    <tr data-plugin-name="Docker Pipeline" class="plugin all-dependents-disabled" data-plugin-id="docker-workflow">
      <td class="details">
        <div class="plugin-name success-plugin"><a rel="noopener noreferrer" href="https://plugins.jenkins.io/docker-workflow" class="jenkins-table__link" target="_blank">
            Docker Pipeline </a></div>
      </td>
    </tr>
    <tr data-plugin-name="Docker plugin" class="plugin all-dependents-disabled" data-plugin-id="docker-plugin">
      <td class="details">
        <div class="plugin-name success-plugin"><a rel="noopener noreferrer" href="https://plugins.jenkins.io/docker-plugin" class="jenkins-table__link" target="_blank">
            Docker plugin
          </a></div>
      </td>
    </tr>
    <tr data-plugin-name="Generic Webhook Trigger Plugin" class="plugin all-dependents-disabled" data-plugin-id="generic-webhook-trigger">
      <td class="details">
        <div class="plugin-name success-plugin"><a rel="noopener noreferrer" href="https://plugins.jenkins.io/generic-webhook-trigger" class="jenkins-table__link" target="_blank">
            Generic Webhook Trigger Plugin
          </a></div>
      </td>
    </tr>
    <tr data-plugin-name="Git plugin" class="plugin has-dependents" data-plugin-id="git">
      <td class="details">
        <div class="plugin-name success-plugin"><a rel="noopener noreferrer" href="https://plugins.jenkins.io/git" class="jenkins-table__link" target="_blank">
            Git plugin
          </a></div>
      </td>
    </tr>
  </tbody>
</table>

## Passo 3 - Configuração do docker dentro do container do Jenkins
Instale o docker no conainer do Jenkins através do script de instalação automática com os seguintes comandos:

* ```docker ps -a``` - procure o container id do Jenkins
* ```docker exec -it ID_CONTAINER_JENKINS  /bin/bash``` - entre no prompt do container Jenkins
* ```curl https://get.docker.com/ > dockerinstall && chmod 777 dockerinstall && ./dockerinstall``` - execute esta linha de comando para efetuar a instalação automatica do docker dentro do container do jenkins.

## Passo 4 - Configuração do Job Parte I
Crie um job Pipeline e cole o conteúdo do arquivo [portal-docker-plugin.jenkinsfile](./pipelines/portal-docker-plugin.jenkinsfile).
![image](https://user-images.githubusercontent.com/8622005/178378775-9ff3bee7-0115-4daa-815c-3b416566d925.png)

## Passo 5 - Configuração do Job Parte II
Configure as variáveis de string conforme abaixo:

<table id="plugins" class="jenkins-table sortable">
  <thead>
    <tr>
      <th initialsortdir="down"><a href="#" class="sortheader">Parâmetro<span class="sortarrow">&nbsp;&nbsp;↓</span></a></th>
	  <th initialsortdir="down"><a href="#" class="sortheader">Valor<span class="sortarrow">&nbsp;&nbsp;↓</span></a></th>
    </tr>
  </thead>
  <tbody>
    <tr data-plugin-name="Docker Pipeline" class="plugin all-dependents-disabled" data-plugin-id="docker-workflow">
      <td class="details">
        <div class="plugin-name success-plugin"><a rel="noopener noreferrer" href="#" class="jenkins-table__link" target="_blank">dockerRegistry</a></div>
      </td>
	  <td class="details">
        <div class="plugin-name success-plugin"><span class="jenkins-label jenkins-label--tertiary" style="margin-left: 1ch;">localhost/docker-registry-local</span></div>
      </td>
    </tr>
  <tr data-plugin-name="Docker Pipeline" class="plugin all-dependents-disabled" data-plugin-id="docker-workflow">
      <td class="details">
        <div class="plugin-name success-plugin"><a rel="noopener noreferrer" href="#" class="jenkins-table__link" target="_blank">image</a></div>
      </td>
	  <td class="details">
        <div class="plugin-name success-plugin"><span class="jenkins-label jenkins-label--tertiary" style="margin-left: 1ch;">portal-teste-jenkins</span></div>
      </td>
    </tr>
	  <tr data-plugin-name="Docker Pipeline" class="plugin all-dependents-disabled" data-plugin-id="docker-workflow">
      <td class="details">
        <div class="plugin-name success-plugin"><a rel="noopener noreferrer" href="#" class="jenkins-table__link" target="_blank">dockerRegistryUsuario</a></div>
      </td>
	  <td class="details">
        <div class="plugin-name success-plugin"><span class="jenkins-label jenkins-label--tertiary" style="margin-left: 1ch;">COLOQUE_O_USUARIO_QUE_CONFIGUROU</span></div>
      </td>
    </tr>
	<tr data-plugin-name="Docker Pipeline" class="plugin all-dependents-disabled" data-plugin-id="docker-workflow">
      <td class="details">
        <div class="plugin-name success-plugin"><a rel="noopener noreferrer" href="#" class="jenkins-table__link" target="_blank">dockerRegistrySenha</a></div>
      </td>
	  <td class="details">
        <div class="plugin-name success-plugin"><span class="jenkins-label jenkins-label--tertiary" style="margin-left: 1ch;">COLOQUE_A_SENHA_DO_USUARIO_QUE_CONFIGUROU</span></div>
      </td>
    </tr>
  </tbody>
</table>

## Passo 6 - Configuração do Job Parte III
Marque a opção abaixo
![image](https://user-images.githubusercontent.com/8622005/178379298-e8399919-c220-4aa6-9d07-60bb81360b2c.png)

E coloque um token qualquer
![image](https://user-images.githubusercontent.com/8622005/178379412-ae4d51d9-2bb6-43ab-9b30-7d247d086cac.png)

Teste o webhook http://localhost:8083/generic-webhook-trigger/invoke?token=TOKEN_JOB_QUE_CADASTROU no navegador:

![image](https://user-images.githubusercontent.com/8622005/178379502-019f4f6d-95f2-4eee-ada7-d7c38dffd605.png)

## Passo 7 - Proxy reverso [ngRok](https://ngrok.com/)
Crie uma conta no ngrok para que o endpoint generic-webhook-trigger/invoke?token=TOKEN_JOB_QUE_CADASTROU se torne público na Web.

## Passo 7 - Crie um container do ngrok
Execute:

```docker run -it -e NGROK_AUTHTOKEN=TOKEN_NGROK ngrok/ngrok http host.docker.internal:8083 ```

Ficará assim:

![image](https://user-images.githubusercontent.com/8622005/178380595-caa6fd30-632e-425a-910a-9c715d441864.png)

## Passo 8 - Configurar webhook jenkins no repositório da aplicação dockerizada
Abra seu respositório que terá sua imagem publicada no proget através do jenkins. Vá no menu ```Settings```, clique em ```Webhooks``` em seguinda ```Add Webhook```

![image](https://user-images.githubusercontent.com/8622005/178380897-4640a2ac-273f-4176-88e6-3b103ba8e67a.png)

Pegue a URL gerada no Forward, e monte o request do webhoo jenkins assim: Exemplo:

https://3988-45-4-33-150.sa.ngrok.io/generic-webhook-trigger/invoke?token=TOKEN_JOB_QUE_CADASTROU

![image](https://user-images.githubusercontent.com/8622005/178381264-6cf0f773-ce1c-4b73-9321-0b88f3a2465c.png)

O restante das opções pode ficar como estão.

## Passo 9 - Faça um teste da esteira
Vá no reposittório que possui o Dockerfile, execute os comandos git:

```git commit --allow-empty -m "Trigger"```
```git push```

e veja na tela do webhook. Tem que restornar código 200

![image](https://user-images.githubusercontent.com/8622005/178381491-828f82ee-f131-4fa5-9785-630d6042a06f.png)


## Resultado Final



