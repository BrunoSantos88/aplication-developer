## Pré-requisitos Referencia https://github.com/badtuxx/DescomplicandoArgoCD

Para que possamos continuar daqui para frente, precisamos ter o seguinte instalado:

- Um cluster Kubernetes
- kubectl instalado
- E muita vontade de aprender

## Instalando o ArgoCD

Primeira coisa, como eu falei anteriormente, o ArgoCD é escrito em GO, o que nos ajuda demais no processo de instalação.

Aqui precisamos dividir essa instalação em duas partes, a instalação do ArgoCD como um operador no Kubernetes, e a instalação do ArgoCD como CLI, para que você possa utilizar o ArgoCD no seu dia a dia.

Ele possui ainda uma interface gráfica, que é o ArgoCD UI, mas não iremos abordar por enquanto, eu quero que a gente fique antes muito confortável com o ArgoCD CLI, que é o que iremos utilizar no nosso dia a dia.

No começo ainda vamos utilizar somente o CLI, mas muito em breve vamos utilizar manifestos para definir as nossa aplicações dentro do ArgoCD.

&nbsp;

### Instalando o ArgoCD como um operador no Kubernetes

Para instalar o ArgoCD como um operador no Kubernetes, antes precisamos criar uma namespace chamada `argocd`, e para isso basta executar o seguinte comando:

```bash
kubectl create namespace argocd
```

&nbsp;

A saída desse comando será algo parecido com isso:

```bash
namespace/argocd created
```

Agora vamos instalar o ArgoCD como um operador no Kubernetes:

```bash
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```
&nbsp;

A saída desse comando será algo parecido com isso:

```bash
namespace/argocd created
customresourcedefinition.apiextensions.k8s.io/applications.argoproj.io created
customresourcedefinition.apiextensions.k8s.io/applicationsets.argoproj.io created
customresourcedefinition.apiextensions.k8s.io/appprojects.argoproj.io created
serviceaccount/argocd-application-controller created
serviceaccount/argocd-applicationset-controller created
serviceaccount/argocd-dex-server created
serviceaccount/argocd-notifications-controller created
serviceaccount/argocd-redis created
serviceaccount/argocd-repo-server created
serviceaccount/argocd-server created
role.rbac.authorization.k8s.io/argocd-application-controller created
role.rbac.authorization.k8s.io/argocd-applicationset-controller created
role.rbac.authorization.k8s.io/argocd-dex-server created
role.rbac.authorization.k8s.io/argocd-notifications-controller created
role.rbac.authorization.k8s.io/argocd-server created
clusterrole.rbac.authorization.k8s.io/argocd-application-controller created
clusterrole.rbac.authorization.k8s.io/argocd-server created
rolebinding.rbac.authorization.k8s.io/argocd-application-controller created
rolebinding.rbac.authorization.k8s.io/argocd-applicationset-controller created
rolebinding.rbac.authorization.k8s.io/argocd-dex-server created
rolebinding.rbac.authorization.k8s.io/argocd-notifications-controller created
rolebinding.rbac.authorization.k8s.io/argocd-redis created
rolebinding.rbac.authorization.k8s.io/argocd-server created
clusterrolebinding.rbac.authorization.k8s.io/argocd-application-controller created
clusterrolebinding.rbac.authorization.k8s.io/argocd-server created
configmap/argocd-cm created
configmap/argocd-cmd-params-cm created
configmap/argocd-gpg-keys-cm created
configmap/argocd-notifications-cm created
configmap/argocd-rbac-cm created
configmap/argocd-ssh-known-hosts-cm created
configmap/argocd-tls-certs-cm created
secret/argocd-notifications-secret created
secret/argocd-secret created
service/argocd-applicationset-controller created
service/argocd-dex-server created
service/argocd-metrics created
service/argocd-notifications-controller-metrics created
service/argocd-redis created
service/argocd-repo-server created
service/argocd-server created
service/argocd-server-metrics created
deployment.apps/argocd-applicationset-controller created
deployment.apps/argocd-dex-server created
deployment.apps/argocd-notifications-controller created
deployment.apps/argocd-redis created
deployment.apps/argocd-repo-server created
deployment.apps/argocd-server created
statefulset.apps/argocd-application-controller created
networkpolicy.networking.k8s.io/argocd-application-controller-network-policy created
networkpolicy.networking.k8s.io/argocd-applicationset-controller-network-policy created
networkpolicy.networking.k8s.io/argocd-dex-server-network-policy created
networkpolicy.networking.k8s.io/argocd-notifications-controller-network-policy created
networkpolicy.networking.k8s.io/argocd-redis-network-policy created
networkpolicy.networking.k8s.io/argocd-repo-server-network-policy created
networkpolicy.networking.k8s.io/argocd-server-network-policy created
```

&nbsp;

Como você pode ver, com o comando acima configuramos o ArgoCD através da criação de vários objetos no Kubernetes, como por exemplo, um `deployment` para o `argocd-server`, um `service` para o `argocd-server`, um `configmap` para o `argocd-cm`, e por aí vai.

Caso você queira conhecer mais sobre o projeto, vá até o [repositório oficial](https://github.com/argoproj/argo-cd)

&nbsp;

Vamos ver se os pods do ArgoCD foram criados com sucesso:

```bash
kubectl get pods -n argocd
```

&nbsp;

A saída desse comando será algo parecido com isso:

```bash
NAME                                                READY   STATUS    RESTARTS   AGE
argocd-application-controller-0                     1/1     Running   0          115s
argocd-applicationset-controller-5f67f4c987-vdtpr   1/1     Running   0          117s
argocd-dex-server-5859d89dcc-c69fx                  1/1     Running   0          117s
argocd-notifications-controller-75c986587-7jznn     1/1     Running   0          116s
argocd-redis-74c8c9c8c6-mzdlv                       1/1     Running   0          116s
argocd-repo-server-76f77874d7-8qscp                 1/1     Running   0          116s
argocd-server-64d5654c48-tkv65                      1/1     Running   0          116s
```

&nbsp;

Onde temos os seguintes pods:

* argocd-application-controller-0 - Responsável por gerenciar os recursos do Kubernetes
* argocd-applicationset-controller-5f67f4c987-vdtpr - Controller responsável por gerenciar os `ApplicationSets`
* argocd-dex-server-5859d89dcc-c69fx - Responsável por gerenciar a autenticação
* argocd-notifications-controller-75c986587-7jznn - Responsável por gerenciar as notificações, como por exemplo, quando um `Application` é atualizado
* argocd-redis-74c8c9c8c6-mzdlv - Responsável por armazenar os dados do ArgoCD
* argocd-repo-server-76f77874d7-8qscp - Responsável por gerenciar os repositórios
* argocd-server-64d5654c48-tkv65 - Responsável por expor a interface gráfica do ArgoCD

&nbsp;

Pronto, apresentados. No decorrer do livro iremos falar mais sobre cada um desses componentes, mas por agora é o que você precisa saber.

Todos os nossos podes estão com o status `Running`, o que significa que eles estão funcionando corretamente.

&nbsp;


## Instalando o ArgoCD CLI

Como eu falei, o ArgoCD possui uma interface gráfica, mas também é possível interagir com ele através de comandos. Para isso, precisamos instalar o `argocd` CLI.

Nós vamos focar a primeira parte desse livro no CLI, para que você consiga entender como funciona o ArgoCD por baixo dos panos, e depois sim, se delicie com a interface gráfica.

Para instalar o `argocd` CLI no Linux, basta executar o seguinte comando:

```bash
curl -sSL -o argocd-linux-amd64 https://github.com/argoproj/argo-cd/releases/latest/download/argocd-linux-amd64

sudo install -m 555 argocd-linux-amd64 /usr/local/bin/argocd

rm argocd-linux-amd64
```

&nbsp;

Com o comando acima fizemos o download do binário do `argocd` CLI, e o instalamos no diretório `/usr/local/bin/argocd`, para fazer a instalação utilizamos o comando `install` do Linux, que é um comando que faz a instalação de arquivos e diretórios. Passamos os parâmetros `-m 555` para definir as permissões do arquivo, e o nome do arquivo que queremos instalar.

Pronto! O nosso `argocd` CLI está instalado.

Vamos ver se ele está funcionando corretamente:

```bash
argocd version
```

&nbsp;

Qual a versão do `argocd` CLI que você está utilizando? Comenta lá no Twitter e me marca para eu saber como está sendo essa sua abordagem com o ArgoCD. @badtux_, esse é o meu arroba lá no Twitter.

&nbsp;

## Autenticando no ArgoCD

Agora que já temos o ArgoCD instalado, tanto o CLI quanto o operador, precisamos fazer a autenticação no ArgoCD para que possamos dar os primeiros passos.

Antes de mais nada, precisamos saber qual o endereço do ArgoCD. Para isso, vamos executar o seguinte comando:

```bash
kubectl get svc -n argocd
```

&nbsp;

A saída desse comando será algo parecido com isso:

```bash
NAME                                      TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)                      AGE
argocd-applicationset-controller          ClusterIP   10.100.164.34    <none>        7000/TCP,8080/TCP            12m
argocd-dex-server                         ClusterIP   10.100.14.112    <none>        5556/TCP,5557/TCP,5558/TCP   12m
argocd-metrics                            ClusterIP   10.100.146.115   <none>        8082/TCP                     12m
argocd-notifications-controller-metrics   ClusterIP   10.100.81.159    <none>        9001/TCP                     12m
argocd-redis                              ClusterIP   10.100.174.178   <none>        6379/TCP                     12m
argocd-repo-server                        ClusterIP   10.100.148.141   <none>        8081/TCP,8084/TCP            12m
argocd-server                             ClusterIP   10.100.25.239    <none>        80/TCP,443/TCP               12m
argocd-server-metrics                     ClusterIP   10.100.46.64     <none>        8083/TCP                     12m
```

&nbsp;

O service que precisamos por agora do ArgoCD é o `argocd-server`, e o endereço completo é `argocd-server.argocd.svc.cluster.local`.

Vamos fazer o port-forward para acessar o ArgoCD sem precisar expor:

```bash
kubectl port-forward svc/argocd-server -n argocd 8080:443
```
ou NodePort

````
kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "NodePort"}}'

````

&nbsp;

Pronto, agora podemos acessar o ArgoCD através do endereço `localhost:8080`, tanto pelo navegador quanto pelo CLI.

Vamos continuar com a nossa saga utilizando o CLI, então vamos fazer a autenticação no ArgoCD.

Para fazer a autenticação no ArgoCD, precisamos executar o seguinte comando:

```bash
argocd login localhost:8080
```
