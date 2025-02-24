# Step by Step MOVECLOUD-T002

If you rather watch a video with step by step instructions, you can do that here
   [![Step by Step Video](https://img.youtube.com/vi/mKH21IgKUSc/0.jpg)](https://www.youtube.com/watch?v=mKH21IgKUSc)

In this task, you will run the WEB and API application as a multi-container application within an Azure Web App while it connects with the CosmosDB. The INIT container, that was pushed to the registry as well, can be used to populate the CosmosDB. 

1. To be able to access the CosmosDB, you need to add the connectionstring as an environment variable to the Azure Web App. Retrieve the connectionstring of the CosmosDB from the Azure portal or use the following command by adding it to the deploy-infrastructure.ps1 and then use the Primary MongoDB ConnectionString.

        ```
        az cosmosdb keys list -n $cosmosDBName -g $resourceGroupName --type connection-strings
        ```

        ```
        pushd
       ./deploy-infrastructure.ps1
       ```
    
> **Note**: We have created the required infrastructure in Azure now we will be running our docker composition for web container and api container to do that we need a connection with             cosmos db and we are doing that by using a connection string 
    

2. Add the contentdb database as part of the connectionstring and add it as a Kubernetes secret. `....documents.azure.com:10255/contentdb?ssl=true`

         ```
         $mongodbConnectionString="connectionString=mongodb://xxx.documents.azure.com:10255/contentdb?ssl=true&replicaSet=globaldb"
         ```

   ![](https://raw.githubusercontent.com/CloudLabsAI-Azure/AIW-DevOps/main/Assets/mongoconnstring.gif)
 
3. Fill the cosmos DB by running the init container

        ```
        docker run -ti  -e MONGODB_CONNECTION="mongodb://xxx.documents.azure.com:10255/contentdb?ssl=true&replicaSet=globaldb" ghcr.io/<yourgithubaccount>/fabrikam-init
        ```
4. In the Azure Portal, navigate to the Web Application fabmedical-web-UniqueId and open the Configuration Blade. In the configuration blade, add a new Application Setting and call this MONGODB_CONNECTION. Add the MongoDB Connection String as a value and then save the settings.

   ![](https://raw.githubusercontent.com/CloudLabsAI-Azure/AIW-DevOps/main/Assets/AppSetting.png)

  If you rather want to run this as code, you can use the command. You can choose to do using one of the approaches.

        ```
        az webapp config appsettings set -n $webappName -g $resourcegroupName --settings MONGODB_CONNECTION="mongodb://xxx.documents.azure.com:10255/contentdb?ssl=true&replicaSet=globaldb"
        ```
> **Note**: If you exited the session in which you declared values for $webappName and $resourcegroupname , you need to declare that with their name.

5. In the Azure Portal, navigate to the Web Application fabmedical-web-UniqueId and open the Container Blade. In the Container blade, select the Docker Compose Tab. Select Private Registry under Image Source. 

Fill in the following data:
* Server URL: https://docker.pkg.github.com

    ```Note: if you are facing issue  due to url please use https://docker.pkg.github.com```

* Login: github-cloudlabsuser-000
* Password: Your GitHub Personal Access Token

> Note: Replace 000 with the value of your GitHub account

As a file, copy the content  `docker-compose.yml` file that you created earlier and paste into the configuration.

   ![](https://raw.githubusercontent.com/CloudLabsAI-Azure/AIW-DevOps/main/Assets/containerblade.png)

  Here is the sample docker-compose.yml:

        ```yaml
        version: "3.4"
        services:
          api:
            image: docker.pkg.github.com/github-cloudlabsuser-000/codetocloud-source/fabrikam-api:latest
            ports:
              - "3001:3001"
          web:
            image: docker.pkg.github.com/github-cloudlabsuser-000/codetocloud-source/fabrikam-web:latest
            depends_on:
               - api
            environment:
                CONTENT_API_URL: http://api:3001
            ports:
               - "3000:80"
        ```

> Note: Replace 000 with the value of your GitHub account. Please note that we are referring to the repository docker.pkg.github.com.

6. The Azure Web App will now create two containers in the web app. Navigate to the Web app https://$webappname.azurewebsites.net to validate if the application is working.

   ![](https://raw.githubusercontent.com/CloudLabsAI-Azure/AIW-DevOps/main/Assets/validate-webapp.gif)


Now, you can move on to the next page.

