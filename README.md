# CI/CD - Processo de Integração e Entrega Contínua

Este repositório possui um fluxo de CI/CD automatizado utilizando GitHub Actions para construção e implantação da aplicação fake-shop.


## Fluxo de Trabalho
O pipeline é acionado automaticamente em dois cenários:
1.	Quando há um push para a branch main.
2.	Quando o fluxo é disparado manualmente via workflow_dispatch.


## 1. CI - Integração Contínua
O primeiro estágio do pipeline realiza as seguintes etapas:
* Checkout do código: Obtém a versão mais recente do repositório;
* Login no Docker Hub: Autentica no Docker Hub utilizando credenciais armazenadas nos secrets do repositório;
* Construção e envio da imagem Docker:
    * A imagem é construída com base no Dockerfile localizado no diretório src;
    * A imagem é enviada para o Docker Hub com duas tags:
        * lucastandy/fake-shop-desafio:v<NÚMERO_DO_BUILD> (onde <NÚMERO_DO_BUILD> é o identificador do fluxo atual);
        * lucastandy/fake-shop-desafio (latest).

## 2.  CD - Entrega Contínua
Após a conclusão do CI, o estágio de CD inicia a implantação no Kubernetes. As etapas incluem:

* Checkout do código: Obtém novamente a versão mais recente do repositório;
* Configuração do contexto do Kubernetes: Define o contexto do cluster Kubernetes utilizando o kubeconfig armazenado nos secrets;
* Implantação no Kubernetes:
    * O deployment é atualizado utilizando o arquivo de manifesto localizado em k8s/deployment.yaml;
    * A nova imagem gerada no CI (lucastandy/fake-shop-desafio:v<NÚMERO_DO_BUILD>) é utilizada para atualizar os pods do cluster.

## Requisitos
Para que o pipeline funcione corretamente, é necessário configurar os seguintes secrets no repositório:
* DOCKERHUB_USERNAME: Nome de usuário do Docker Hub;
* DOCKERHUB_TOKEN: Token de autenticação do Docker Hub;
* K8S_KUBECONFIG: Configuração do kubeconfig para acesso ao cluster Kubernetes.

## Autor
Lucas Tandy do Nascimento Silva
 https://www.linkedin.com/in/lucas-tandy/





