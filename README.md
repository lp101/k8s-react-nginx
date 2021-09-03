# 1) Development environment
Le configurazuioni per creare le immagini Docker si trovano all'interno della directory "docker".

La struttura delle directory è la seguente:
- app: i file e il codice necessari per creare l'immagine docker per l'app;
- nginx: le configurazioni per creare l'immagine oer il proxy con nginx;
- docker-compose.yml: per eseguire i container localmente con Docker.

Per creare le immagini ed eseguire i containers localmente con Docker:
$ cd docker
$ docker-compose up --build

E' possibile accedere all'app dal browser sulla porta 8081: http://localhost:8081/

# 2) Kubernetes resources
Per testare le configurazioni Kubernetes è stato utilizzato il cluster locale minikube.
E' possibile trovare le configurazioni nella directory "k8s".

Per la creazione delle configurazioni K8s è stato utilizzato "kompose":
$ cd docker
$ kompose convert

I file generati sono stati spostati nella directory k8s.

Questi file sono stati modificati per utilizzare le immagini locali e aggiungendo il service per l'app.
Per eseguire le immagini in minikube:
$ minikube start
$ eval $(minikube docker-env)
$ docker-compose up --build # Questo comando rende le immagini disponibili per minikube.

$ cd k8s
$ kubectl apply -f .

Usiamo il nome del pod nginx per creare un proxy locale:
$ kubectl get pods
   NAME                         READY   STATUS    RESTARTS   AGE
   nginx-7779cdd9f9-fl9ns       1/1     Running   0          21s
   nodeserver-544fb465f-45thk   1/1     Running   0          21s

Avviamo il proxy per poter accedere da locale all'app (il proxy Nginx è in ascolto sulla porta 8080):
$ kubectl port-forward nginx-7779cdd9f9-fl9ns 8081:8080

E' possibile accedere all'app dal browser sulla porta 8081: http://localhost:8081/

Per interrompere il servizio:
1) "ctrl + c" per fermare il proxy locale verso Minikube.
2) Eliminiamo i pods:
$ kubectl delete -f .
3) Stoppiamo Minikube:
$ minikube stop

# 3) Deployment to production



