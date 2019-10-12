# Parse Server 

Here we are using Docker Compose to deploy Parse Server

## Perquisites- 

Docker & docker-compose installed on Host

## Steps - 

1. Create docker-compose.yaml file [docker-compose.yml](https://github.com/hrsikesa/parse-server/blob/master/docker-compose/docker-compose.yml)
2. In the directory where docker-compose.yaml file is present run below command 
   ```bash
   docker-compose up -d
   ```

Now our Parse Server is up and running 

3. To add data in Parse Server -  Run below command on Host were Parse Server is running
      ```bash
      curl -X POST -H "X-Parse-Application-Id: TUA9UzkoEqYadwpnPRd9PNok8SP2RXeTrnAhS4Ak" -H "Content-Type: application/json" -d '{"score":839,"playerName":"Champ","cheatMode":false}' http://localhost:1337/parse/classes/GameScore
      ```
      
    
   Will recive this response - '{"objectId":"TGFljGptGS","createdAt":"2019-10-10T02:43:37.738Z"}'
    

   To Retrive added data from Parse Server -  Run below command on Host were Parse Server is running
      ```bash   
      curl -X GET -H "X-Parse-Application-Id: TUA9UzkoEqYadwpnPRd9PNok8SP2RXeTrnAhS4Ak" http://localhost:1337/parse/classes/GameScore
      ```    
   Will recive this Response -  Will show all the added data

         '{"results":[{"objectId":"wgDzN0Q745","score":839,"playerName":"Champ","cheatMode":false,"createdAt":"2019-10-10T02:36:55.681Z","updatedAt":"2019-10-10T02:36:55.681Z"},{"objectId":"g4K2HESgEE","score":1000,"playerName":"Rishi","cheatMode":false,"createdAt":"2019-10-10T02:37:21.671Z","updatedAt":"2019-10-10T02:37:21.671Z"},{"objectId":"TGFljGptGS","score":839,"playerName":"Champ","cheatMode":false,"createdAt":"2019-10-10T02:43:37.738Z","updatedAt":"2019-10-10T02:43:37.738Z"}]}'
