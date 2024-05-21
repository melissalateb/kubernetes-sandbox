

# Exercice 1 

1. **Installer Minikube**
   - Suivre les instructions sur [le site officiel](https://minikube.sigs.k8s.io/docs/start/).

2. **Installer Kubectl**
   - Suivre les instructions sur [le site officiel](https://kubernetes.io/docs/tasks/tools/install-kubectl/).

3. **Activer Hyper-V sur Windows**
   - Ouvrir PowerShell en tant qu'administrateur et exécuter les commandes suivantes :
   ```powershell
   Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V -All
   DISM /Online /Enable-Feature /All /FeatureName:Microsoft-Hyper-V
   ```

## Démarrage de Minikube (commencement de l'exercice 1)

1. **Démarrer Minikube** :
   ```powershell
   minikube start
   ```

2. **Démarrer un registre Docker local** :
   ```powershell
   docker run -d -p 5000:5000 --restart=always --name registry registry:2
   ```

3. **Activer le registre dans Minikube** :
   ```powershell
   minikube addons enable registry
   ```


## Déploiement du Pod Planner

1. **Se rendre dans le dossier `planner`** :
   ```powershell
   cd planner
   ```
2. **Se rendre dans le dossier `planner`** :

-->  **Changer le nom de l'image dans le pod.yaml
nom_utilisateur_docker/planner**

   ```powershell
   docker build -t melissalateb/planner .
   ```

3. **Push les modifications** :

```powershell
docker push melissalateb/planner 
```
**remplacer melissalateb par le nom_utilisateur_docker

4. **Appliquer la configuration du Pod** :
```powershell
kubectl apply -f pod.yaml
```

5. **Appliquer la configuration du service** :
```powershell
kubectl apply -f service.yaml
```

## Déploiement du Pod Worker

1. **Se rendre dans le dossier `worker`** :
```powershell
cd ../worker
```
2. **Se rendre dans le dossier `worker`** :

-->  **Changer le nom de l'image dans le pod.yaml
nom_utilisateur_docker/worker**
   ```powershell
   docker build -t nomuserdocker/worker .
   ```
3. **Push les modifications** :
```powershell
   docker push melissalateb/planner
```
**remplacer melissalateb par le nom_utilisateur_docker

4. **Appliquer la configuration du Pod** :
   ```powershell
   kubectl apply -f pod.yaml
   ```

5. **Appliquer la configuration du service** :
   ```powershell
   kubectl apply -f service.yaml
   ```
6. **Deploiement du worker**:

aller au dossier /worker
```sh
kubectl apply -f deplpoment.yaml

```

## Lancer les deux conteneurs grace aux images 

```powershell
docker run -d -p 8080:8080 --name worker melissalateb/worker
docker run -d -p 3001:3000 --name planner melissalateb/planner
```

## Vérifier les deux conteneurs

```sh
docker ps
```
--> Résultat 


| CONTAINER ID | IMAGE                   | COMMAND                  | CREATED         | STATUS         | PORTS                      | NAMES    |
|--------------|-------------------------|--------------------------|-----------------|----------------|----------------------------|----------|
| 76009fd52b17 | melissalateb/planner    | "docker-entrypoint.s…"   | 4 minutes ago   | Up 4 minutes   | 0.0.0.0:3001->3000/tcp     | planner  |
| a5a3d682e654 | melissalateb/worker     | "docker-entrypoint.s…"   | 6 minutes ago   | Up 6 minutes   | 0.0.0.0:8080->8080/tcp     | worker   |
| 499c5ed6c093 | registry:2              | "/entrypoint.sh /etc…"   | 3 hours ago     | Up 3 hours     | 0.0.0.0:5000->5000/tcp     | registry |
| fcdf67521535 | kicbase/stable:v0.0.44  | "/usr/local/bin/entr…"   | 3 hours ago     | Up 3 hours     | 127.0.0.1:59824->22/tcp, 127.0.0.1:59624->2376/tcp | kicbase  |

## Vérification du Déploiement

1. **Vérifier que les Pods sont en cours d'exécution** :
   ```powershell
   kubectl get pods
   ```
--> Résultat obtenu 
NAME                         | READY  |STATUS           |  RESTARTS | AGE
-----------------------------|--------|-----------------|-----------|-----
hello-node-55fdcd95bf-879z5  | 1/1    | Running         |  0        | 77m
planner                      | 1/1    | ErrImagePull    |  0        | 6m2s
worker                       | 1/1    |ImagePullBackOff |  0        |6m31s 

2. **Vérifier les services** :
   ```powershell
   kubectl get services
   ```
--> Résultats obtenu 
NAME         |TYPE           |CLUSTER-IP       |EXTERNAL-IP  |PORT(S)          |AGE
-------------|---------------|-----------------|-------------|-----------------|------
hello-node   |LoadBalancer   |10.106.196.143   |<pending>    |8080:30238/TCP   |74m
kubernetes   |ClusterIP      |10.96.0.1        |<none>       |443/TCP          |94m
planner      |LoadBalancer   |10.101.10.159    |<pending>    |8080:32236/TCP   |3m52s
worker       |LoadBalancer   |10.105.0.195     |<pending>    |8080:30759/TCP   |4m19s

3. **Vérifier l'état des conteneurs** : 

commande 
```powershell
http://localhost:5000/v2/_catalog
```
--> Résultat de la commande 
```powershell	
	repositories	
         0	"planner"
         1	"worker"
```



