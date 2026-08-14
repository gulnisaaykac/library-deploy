# library-deploy

Library projesinin Minikube / Kubernetes manifestleri.
- API: ASP.NET (`library-api`)
- UI: Angular + nginx (`library-ui`)

## Dosyalar

| Dosya | Ne ise yarar |
|--------|----------------|
| `api.yaml` | API Deployment + Service (NodePort 30065) |
| `ui.yaml` | UI Deployment + Service (NodePort 30080) |
| `configmap.yaml` | Gizli olmayan ayarlar (env, JWT issuer/audience) |
| `secret.yaml.example` | Secret ornegi (sifre yok) |
| `secret.yaml` | Gercek sifreler (sadece lokal, gitignore) |
| `limitrange.yaml` | Tek container kaynak kurallari |
| `quota.yaml` | Namespace toplam kaynak tavani |
| `hpa.yaml` | API icin CPU tabanli otomatik scale |

## On kosul

1. Docker Desktop acik
2. `minikube start`
3. Imajlar Minikube'da yuklu:
   - `library-api:v3`
   - `library-ui:v3`

Imaj yukleme ornegi:

    minikube image load library-api:v3
    minikube image load library-ui:v3

## Uygulama sirasi

Bu komutlar dokumandir; README yazarken calistirma.
Gerektiginde PowerShell'de `C:\Users\PC\k8s` klasorunde calistirilir:

    cd C:\Users\PC\k8s
    kubectl apply -f limitrange.yaml
    kubectl apply -f quota.yaml
    kubectl apply -f configmap.yaml
    kubectl apply -f secret.yaml
    kubectl apply -f api.yaml
    kubectl apply -f ui.yaml
    kubectl apply -f hpa.yaml

## Erisim

    minikube service library-ui

UI tarayicide acilir. `/api` isteklerini nginx `library-api` servisine proxy eder.

## Kontroller

    kubectl get pods
    kubectl get hpa
    kubectl get configmap,secret
    kubectl describe pod -l app=library-api

## Onemli

- `secret.yaml` GitHub'a commit edilmez.
- SQL Server host makinede; pod `host.minikube.internal,1433` ile baglanir.


## Ingress

    minikube addons enable ingress
    kubectl apply -f ingress.yaml

Windows'ta IIS port 80'i tutuyorsa:

    Stop-Service W3SVC -Force
    minikube tunnel

Tarayici: http://127.0.0.1

Alternatif:

    kubectl port-forward -n ingress-nginx svc/ingress-nginx-controller 8080:80

Tarayici: http://127.0.0.1:8080