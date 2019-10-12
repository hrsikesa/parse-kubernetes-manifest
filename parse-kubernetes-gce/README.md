# Different ways to Deploy Parse Server on GCP Kubernetes Cluster
------------------------------------------------------------------
- Using YAML files
- Using Kompose
- Helm Charts 

## Parse Server on Kubernetes using YAML file 

      Perquisites - Kubernetes Cluster, Docker Image on Docker Hub 

      1. Inside a Directory create these YAML files  - [mongo-service.yml](https://github.com/hrsikesa/parse-server/blob/master/parse-kubernetes-gce/mongo-service.yml) , [mongo-deployment.yml](https://github.com/hrsikesa/parse-server/blob/master/parse-kubernetes-gce/mongo-deployment.yml), [parse-server-service.yml](https://github.com/hrsikesa/parse-server/blob/master/parse-kubernetes-gce/parse-server-service.yml), [parse-server-deployment.yml](https://github.com/hrsikesa/parse-server/blob/master/parse-kubernetes-gce/parse-server-deployment.yml), [mongo-persistent-storage-persistentvolumeclaim.yml](https://github.com/hrsikesa/parse-server/blob/master/parse-kubernetes-gce/mongo-persistent-storage-persistentvolumeclaim.yml )

      2. Now inside same directory where we have created YAML file run below commands to deploy Parse Server
          ```bash
          kubectl create -f mongo-service.yml
          ```
          ```bash
          kubectl create -f mongo-deployment.yml
          ```
          ```bash
          kubectl create -f parse-server-service.yml
          ```
          ```bash
          kubectl create -f parse-server-deployment.yml
          ```
          ```bash
          kubectl create -f mongo-persistent-storage-persistentvolumeclaim.yml
          ```

      3. Now go to Service in GUI to access parse-server from External IP or check by #kubectl get svc (see External IP & Port of my-parse-app-service)

      4. Verify Parse Server Deploying by adding and retrieving data from Parse Server - 

          a) To add data in Parse Server -
             ```bash
             curl -X POST -H "X-Parse-Application-Id: TUA9UzkoEqYadwpnPRd9PNok8SP2RXeTrnAhS4Ak" -H "Content-Type: application/json" -d '{"score":839,"playerName":"ATLAN","cheatMode":false}' http://35.188.66.94:1337/parse/classes/GameScore
             ```

             Will recive this response -
               '{"objectId":"gonL8NSOh7","createdAt":"2019-10-12T03:14:36.275Z"}'


          b) To Retrive added data -
            ```bash
             curl -X GET -H "X-Parse-Application-Id: TUA9UzkoEqYadwpnPRd9PNok8SP2RXeTrnAhS4Ak" http://35.188.66.94:1337/parse/classes/GameScore
             ```

             Will recive this response -
               '{"results":[{"objectId":"gonL8NSOh7","score":839,"playerName":"ATLAN","cheatMode":false,"createdAt":"2019-10-12T03:14:36.275Z","updatedAt":"2019-10-12T03:14:36.275Z"}]}'
