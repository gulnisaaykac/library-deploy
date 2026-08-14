# Sunum demo sirasi (Library + Minikube)

## 0) Baslamadan
- Docker Desktop acik
- `minikube start`
- Podlar ayaga kalksin: `kubectl get pods`

## 1) Mimariyi soyle (30 sn)
- UI (Angular + nginx) → `/api` proxy → API (.NET) → SQL (host)
- Ayarlar: ConfigMap (acik) + Secret (gizli)
- Health probe + HPA var

## 2) Cluster kaniti
    kubectl get pods
    kubectl get configmap,secret
    kubectl get hpa
    kubectl describe pod -l app=library-api

## 3) Uygulamayi ac
    minikube service library-ui
- Tarayicide UI gelsin
- Kitap / quiz kismini kisa goster (1-2 dk)

## 4) HPA'yi anlat (opsiyonel, zaman kalirsa)
- CPU hedefi asilinca replica artar (min 1, max 3)
- Daha once yuk testinde 1 → 3 gorduk (ekran/defter)

## 5) Bitirirken soyle
- Secret git'te yok
- README ile sifirdan apply sirasi yazili