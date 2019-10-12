# Different ways to Deploy Parse Server on GCP Kubernetes Cluster
------------------------------------------------------------------
- Using YAML files
- Using Kompose
- Helm Charts 

## Parse Server on Kubernetes using YAML file 

      Perquisites - Kubernetes Cluster, Docker Image on Docker Hub 

     1. Inside a Directory create these YAML files  - 
   [mongo-service.yml](https://github.com/hrsikesa/parse-server/blob/master/parse-kubernetes-gce/mongo-service.yml),, [mongo-deployment.yml](https://github.com/hrsikesa/parse-server/blob/master/parse-kubernetes-gce/mongo-deployment.yml), [parse-server-service.yml](https://github.com/hrsikesa/parse-server/blob/master/parse-kubernetes-gce/parse-server-service.yml), [parse-server-deployment.yml](https://github.com/hrsikesa/parse-server/blob/master/parse-kubernetes-gce/parse-server-deployment.yml), [mongo-persistent-storage-persistentvolumeclaim.yml](https://github.com/hrsikesa/parse-server/blob/master/parse-kubernetes-gce/mongo-persistent-storage-persistentvolumeclaim.yml )
   
    2. Now inside same directory where we have created YAML file run below commands to deploy Parse Server
        
          #kubectl create -f mongo-service.yml
          
          #kubectl create -f mongo-deployment.yml
          
          #kubectl create -f parse-server-service.yml
          
          #kubectl create -f parse-server-deployment.yml
          
          #kubectl create -f mongo-persistent-storage-persistentvolumeclaim.yml
          
      3. Now go to Service in GUI to access parse-server from External IP or check by #kubectl get svc (see External IP & Port   of my-parse-app-service)

      4. Verify Parse Server Deploying by adding and retrieving data from Parse Server - 

          a) To add data in Parse Server -
             
             #curl -X POST -H "X-Parse-Application-Id: TUA9UzkoEqYadwpnPRd9PNok8SP2RXeTrnAhS4Ak" -H "Content-Type: application/json" -d '{"score":839,"playerName":"ATLAN","cheatMode":false}' http://35.188.66.94:1337/parse/classes/GameScore
            

             Will recive this response -
               '{"objectId":"gonL8NSOh7","createdAt":"2019-10-12T03:14:36.275Z"}'


          b) To Retrive added data -
            
             #curl -X GET -H "X-Parse-Application-Id: TUA9UzkoEqYadwpnPRd9PNok8SP2RXeTrnAhS4Ak" http://35.188.66.94:1337/parse/classes/GameScore
             

             Will recive this response -
               '{"results":[{"objectId":"gonL8NSOh7","score":839,"playerName":"ATLAN","cheatMode":false,"createdAt":"2019-10-12T03:14:36.275Z","updatedAt":"2019-10-12T03:14:36.275Z"}]}'
               

## Parse Server with Kompose on Kubernetes -
[Kompose](http://kompose.io/) is a tool to help users who are familiar with docker-compose move to Kubernetes.
Kompose deploy application on Kubernetes using docker-compose.yml file

Perquisites - Kubernetes Cluster , [Kompose on Host](https://github.com/kubernetes/kompose) from where we will connect to our Kubernetes Cluster 
####  Steps  to follow 
1. Install Kompose on Host from where we will be connecting to our Cluster
           
       # curl -L https://github.com/kubernetes/kompose/releases/download/v1.18.0/kompose-linux-amd64 -o kompose
       # chmod +x kompose
       # sudo mv ./kompose /usr/local/bin/kompose
3. Go to directory where  docker-compose.yml file is present and run below command. Now Kompose will automatically create Services, Deployments on Kubernetes  
                     
       # kompose up 

   Now our Parse Server is up and running
   
   
## Parse Server with Helm Chart -

 Perquisites - Kubernetes Cluster

 1. Install Helm on Host from where we will be connecting to our Cluster
    
        # curl https://raw.githubusercontent.com/kubernetes/helm/master /scripts/get > get_helm.sh
        # chmod 700 get_helm.sh
        #  ./get_helm.sh 

 2. Install Helm’s server-side counterpart Tiller, 
     
         # helm init 

    To verify Tiller installation run below command by checking the output of kubectl get pods 
    
         # kubectl --namespace kube-system get pods | grep tiller    

  3. Install Parse Server on Kubernetes Cluster 
            
          #kubectl create clusterrolebinding add-on-cluster-admin --clusterrole=cluster-admin --serviceaccount=kube-system:default
     clusterrolebinding.rbac.authorization.k8s.io/add-on-cluster-admin created
           
          #helm list
          #helm install stable/parse

  4.  Then run below command to export Parse Server URL - 
               
           #export SERVICE_HOST=$(kubectl get svc --namespace default my-release-parse --template "{{ range (index .status.loadBalancer.ingress 0) }}{{.}}{{ end }}"):1337


  5. Verify Parse Server Creation - 

       a) To add data in Parse Server -
          
          #curl -X POST -H "X-Parse-Application-Id: myappID" -H "Content-Type: application/json" -d '{"score":1300,"playerName":"ATLAN","cheatMode":false}'   http://$SERVICE_HOST/parse/classes/GameScore
       
        Will recive this response -               
           {"objectId":"bUgj3eRZDs","createdAt":"2019-10-12T02:40:13.862Z"}

    
       b) To Retrive added data -
         
          #curl -X GET -H "X-Parse-Application-Id: myappID" http://$SERVICE_HOST/parse/classes/GameScore
          
        Will receive this response -
        
          {"results":[{"objectId":"bUgj3eRZDs","score":1300,"playerName":"ATLAN","cheatMode":false,"createdAt":"2019-10-12T02:40:13.862Z","updatedAt":"2019-10-12T02:40:13.862Z"}]}   
