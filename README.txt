1- Deploy k8s cluster from link : https://gist.github.com/mjhea0/b08ed217144ad307085a05450f3ac6be

2- create PV,PVC,deployment,secret,service(clusterIP) for MYSQL database from files : 
    => deployment-mysql.yaml
    => mysql-secrets.yaml
    => pvc-mysql.yaml
    => pv-mysql.yaml
    => service-mysql.yaml

Note: secrets are encoded base64

3- $ git clone https://github.com/jorgeacetozi/notepad.git 
4- $ cd notepad	

5- Add this CMD to Docker file to build the image using Maven
$ mvn clean install package -Dmaven.test.skip=true

6- login to your dockerhub registry or any of your registires

7- build docker image and tag it with your docker hub via cmd: 
 =>  docker build -t <repo_user_account>/<registry_name>:my-notepad-img . 

 8- push your image to your public registry :  docker push <repo_user_account>/<registry_name>:my-notepad-img

9- build secret for registry credentials to pull your image to container in pod of deployment-front-end following link : 
 https://kubernetes.io/docs/tasks/configure-pod-container/
 pull-image-private-registry/#create-a-secret-in-the-cluster-that-holds-your-authorization-token 

10- setup nginx-ingress-controller via link : https://kubernetes.github.io/ingress-nginx/deploy/#bare-metal
=> kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v0.44.0/deploy/static/provider/baremetal/deploy.yaml
 
11- create deployment for notepad front-end, 2 services nodePorts  (health-svc,notes-svc) , ingress 1 rule for 2 paths
     => deployment-notepad.yaml
     => docker-hub-reg-cred.yaml
     => ingress-health-notes.yaml
     => svc-notepad-health.yaml
     => svc-notepad-notes.yaml

12- test by : $ curl http://<notepad service hostname>:8080/health
              $ curl -H "Content-Type: application/json" -X POST -d '{"title":"Kubernetes","content":"Best container orchestration tool ever"}' http://<notepad service hostname>:8080/notes


