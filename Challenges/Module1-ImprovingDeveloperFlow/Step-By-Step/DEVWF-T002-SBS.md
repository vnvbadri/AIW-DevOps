# Step by Step DEVWF-T002

If you rather watch a video with step by step instructions, you can do that here
       [![Step by Step Video](https://img.youtube.com/vi/YJkaPF5k_O8/0.jpg)](https://www.youtube.com/watch?v=YJkaPF5k_O8)

1. From your GitHub repository open your Codespace if it is not already opened

    ![](https://raw.githubusercontent.com/CloudLabsAI-Azure/AIW-DevOps/main/Assets/OpenCodeSpace.png)

1. In the Terminal window, open a PowerShell Terminal by typing `pwsh`

1. Type the following command to create a Docker network named `fabrikam`:

   ```bash
   docker network create fabrikam
   ```

1. Run an instance of MongoDB to use for local testing.

   ```bash
   docker run -d --name mongo --net fabrikam -p 27017:27017 mongo
   ```

   > **Note**:  With the existing source code written for MongoDB, it can be pointed towards the Azure Cosmos DB MongoDB API endpoint. The Azure Cosmos DB Emulator could be used for local development on Windows, however, the Cosmos DB emulator does not support Linux. As a result, when using Linux for development, MongoDB is still needed for local development environments; with Azure Cosmos DB used for data storage in the cloud. This allows existing source code written for MongoDB storage to be easily migrated to using Azure Cosmos DB backend.

1. Confirm that the mongo container is running and ready.

   ```bash
   docker ps
   docker logs mongo
   ```

   ![In this screenshot of the console window, docker container list has been typed and run at the command prompt, and the “api” container is in the list. Below this the log output is shown.](https://raw.githubusercontent.com/CloudLabsAI-Azure/AIW-DevOps/main/Assets/Ex1-Task1.4.png)

1. To initialize the local database with test content, first navigate to the **content-init** directory and run npm install.

   ```
   cd content-init
   ```

   ```bash
   npm install
   ```

1. Initialize the database.

   ```bash
   node server.js
   ```

   ![This screenshot of the console window shows output from running the database initialization.](https://raw.githubusercontent.com/CloudLabsAI-Azure/AIW-DevOps/main/Assets/Ex1-Task1.7.png)

1. Confirm that the database now contains test data by connecting to the docker container by executing the below commands. You can open a new terminal window if needed

   ```bash
   docker exec -ti mongo bash
   ```
   
   ```
   mongo
   ```

   ```text
   show dbs
   use contentdb
   show collections
   ```
   
   ```
   db.speakers.find()
   ```
   
   ```
   db.sessions.find()
   ```
   
   ```
   quit()
   ```
   
   ```
   exit
   ```

   This should produce output similar to the following:

   ![This screenshot of the console window shows the data output.](https://raw.githubusercontent.com/CloudLabsAI-Azure/AIW-DevOps/main/Assets/Ex1-Task1.8.png)

1. Now navigate to the `content-api` directory and run npm install.

   ```bash
   cd ../content-api
   ```
   
   ```
   npm install
   ```

1. Start the API as a background process.

    ```bash
    node ./server.js &
    ```

   ![In this screenshot, node ./server.js & has been typed and run at the command prompt, which starts the API as a background process.](https://raw.githubusercontent.com/CloudLabsAI-Azure/AIW-DevOps/main/Assets/image47.png)
    
    ``Note: If you get the pop-up for forwarded port after the run, please click on open, and then you will be directed to the forwarded port. You can skip step 11 and 12 as you already got the forwarded port and you can move on to Step 13 ``    

1. Test the API using your browser. In the GitHub, Codespace navigate to the Remote Explorer and create a new Forwarded Port

    ![Setup port forwarding ](https://raw.githubusercontent.com/CloudLabsAI-Azure/AIW-DevOps/main/Assets/updated-port.png)

1. Next to the Forwarded Port, click the global icon. The forwarded website api will be opened in a new browser window

1. Add `/speakers` to the URL.

    ![In this screenshot, made a request to view speakers.](https://raw.githubusercontent.com/CloudLabsAI-Azure/AIW-DevOps/main/Assets/SpeakersAPI.png)

1. Navigate to the web application directory, run `npm install` and `ng build`.

   ```bash
   cd ../content-web
   ```
   
   ```
   npm install
   ng build
   ```
   
1. Now run the content-web application in the background.

    ```bash
    node ./app.js &
    ```

1. Test the web application using the port forward method. In the Remote Explorer, forward port 3000 and open the browser by clicking the globe icon next to the port. 

    ![Open the running app in your browser](https://raw.githubusercontent.com/CloudLabsAI-Azure/AIW-DevOps/main/Assets/OpenBrowser.png)

1. Your application should show data in the Speakers menu item

    ![Verify that speaker data is shown](https://raw.githubusercontent.com/CloudLabsAI-Azure/AIW-DevOps/main/Assets/neuroconf-screen.png)
    
    
Now, you can move on to the next page.
