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

&nbsp;

Pronto, agora podemos acessar o ArgoCD através do endereço `localhost:8080`, tanto pelo navegador quanto pelo CLI.

Vamos continuar com a nossa saga utilizando o CLI, então vamos fazer a autenticação no ArgoCD.

Para fazer a autenticação no ArgoCD, precisamos executar o seguinte comando:

```bash
argocd login localhost:8080
```
