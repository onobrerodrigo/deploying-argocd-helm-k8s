# Instalação do ArgoCD com Helm
Este guia explica como instalar o ArgoCD em um cluster Kubernetes utilizando o Helm, bem como como realizar atualizações no values.yaml para configurar o ingress.

## Pré-requisitos
Antes de iniciar a instalação, certifique-se de ter os seguintes itens configurados:

* Kubernetes: Acesso a um cluster Kubernetes.
* Helm: O Helm deve estar instalado e configurado em sua máquina local.
* Certificado SSL/TLS: Se você planeja usar HTTPS, deve ter um certificado válido para o domínio especificado.

## Passo 1: Instalação do ArgoCD

Para instalar o ArgoCD com o Helm, você deve ter um ```values.yaml``` configurado. Abaixo está um exemplo de comando de instalação, mas antes é necessário adicionar o repositório do ArgoCD

```bash
helm repo add argo https://argoproj.github.io/argo-helm
```

```bash
helm install --values values.yaml argocd argo/argo-cd --create-namespace --namespace argocd
```

O arquivo ```values.yaml``` é crucial, pois ele contém todas as configurações personalizadas para a instalação do ArgoCD. Duas configurações importantes são as definições de domínio e hostname para o ingress.

### Exemplo de Configuração no values.yaml
```bash
# Configurações principais para o ArgoCD usando Helm

# -- Default domain usado por todos os componentes
# Utilizado para ingresses, certificados, SSO, notificações, etc.
domain: argocd.ginga.io

# -- Hostname do servidor Argo CD
# @default -- `""` (padrão global.domain)
hostname: "argocd.ginga.io"
```

Essas configurações garantem que todos os componentes do ArgoCD utilizem o domínio ```argocd.ginga.io```, e que o servidor ArgoCD seja acessível através do mesmo hostname.

## Passo 2: Atualização do ArgoCD

Se você precisar atualizar as configurações do ArgoCD, por exemplo, alterando o domínio ou qualquer outra configuração no ```values.yaml```, você pode usar o comando helm upgrade para aplicar as mudanças sem precisar desinstalar e reinstalar o ArgoCD.

```bash
helm upgrade argocd argo/argo-cd --namespace argocd --values values.yaml
```

>Observações sobre Atualização

* Revisão do values.yaml: Antes de atualizar, revise o ```values.yaml``` para garantir que todas as alterações estão corretas e não irão causar interrupções no serviço.
* Flags Adicionais: Se necessário, você pode adicionar flags como --force para forçar a atualização, caso contrário, o Helm irá aplicar apenas as diferenças necessárias.

```bash
helm upgrade argocd argo/argo-cd --namespace argocd --values values.yaml --force
```
* Verificação de Mudanças: Se você tiver o plugin [helm diff](https://github.com/databus23/helm-diff) instalado, é possível visualizar as mudanças que serão feitas com o comando:

```bash
helm diff upgrade argocd argo/argo-cd --namespace argocd --values values.yaml
```
## Considerações Finais

A utilização do Helm para gerenciar a instalação do ArgoCD oferece uma maneira declarativa de definir, instalar e atualizar as configurações de software complexo em Kubernetes. Ao seguir este guia, você pode facilmente configurar e ajustar o ArgoCD para atender às necessidades específicas do seu ambiente.

>Nota: É altamente recomendável que você personalize o values.yaml de acordo com as necessidades do seu cluster e políticas de segurança antes de realizar a instalação ou atualização.