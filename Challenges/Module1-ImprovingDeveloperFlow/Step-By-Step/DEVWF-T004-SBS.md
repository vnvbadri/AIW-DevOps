# Step by Step DEVWF-T004

If you rather watch a video with step by step instructions, you can do that here
[![Step by Step Video](https://img.youtube.com/vi/w80WzGtWpVc/0.jpg)](https://www.youtube.com/watch?v=w80WzGtWpVc)

In this task, you will merge the Pull Request containing 3 multi-staged Docker files to your main branch, while linking to an Azure Boards project. The multi-staged Docker files will be used to build a new version of your WEB, API and INIT container.

>This task has a Starter solution, that creates a Pull Request containing some files and instructions. 

1. In your GitHub Codespace, open a PowerShell Terminal and run the starter solution. A Pull Request with 3 Docker files will be created

      ```
      Workshop-Step Start "DEVWF-T004"
      ```

      ![](https://raw.githubusercontent.com/CloudLabsAI-Azure/AIW-DevOps/main/Assets/devwf004.gif)

2. In your GitHub repository, navigate to the Tab Pull Requests and open the Pull Request with DEVWF-T004 in the title

      ![Shows the menu item for navigating to the Pull Request](https://raw.githubusercontent.com/CloudLabsAI-Azure/AIW-DevOps/main/Assets/PullRequestDEVWF-T004.png)

3. In the Pull Request, check the conversation, Commits, Checks, and Files Changed Tabs, and go through the instructions and changes.

4. On the Conversation Tab, press the **Merge Pull Request** Button, to merge the files into the main branch. Link the Pull Request to your Azure Boards Work item for Module 1 by typing AB#Module1WorkItemID in the title or description of the Pull Request Commit Message. 

      ![Shows the button for merging a Pull Request in GitHub](https://raw.githubusercontent.com/CloudLabsAI-Azure/AIW-DevOps/main/Assets/mergePullRequest.png)

Now your repository contains 3 new "multi-staged" docker file.

6. In your GitHub Codespace, update your files to the latest version by pulling them.

      ![](https://raw.githubusercontent.com/CloudLabsAI-Azure/AIW-DevOps/main/Assets/2020-10-05-12-10-11.png)

7. Open your PowerShell terminal window. From the content-api folder containing the API application files and the new Dockerfile, type the following command to create a Docker image for the API application. This command does the following:

      - Executes the Docker build command to produce the image
      - Tags the resulting image with the name content-api (-t)
      - The final dot (".") indicates to use the Dockerfile in this current directory context. By default, this file is expected to have the name "Dockerfile" (case sensitive).

      

      ```
      cd /workspaces/CodeToCloud-Source/content-api
      
      docker build -t fabrikam-api .
      ```

8. Do the same for the `content-web` and the `content-init` containers

      ```bash
      cd ../content-init
      docker build -t fabrikam-init .
   
      cd ../content-web
      docker build -t fabrikam-web .
      ```

9. Make sure you remove all running images to avoid conflict with ports in use. When you run `docker ps -a` you see all containers that are running or are stopped. Remove all containers, except the `cloudenvimage` and the `mongo` container. The `cloudenvimage` contains your GitHub Codespace and the `mongo` contains your populated database.

      ```
      docker rm -f <containername or id>
      ```

      ![](https://raw.githubusercontent.com/CloudLabsAI-Azure/AIW-DevOps/main/Assets/remove-container.gif)

10. Run all containers and check whether the application still works as expected

      ```bash
      docker run -d --name api -p 3001:3001 --net fabrikam fabrikam-api
      docker run -d --name web -p 3000:80 --net fabrikam fabrikam-web
      ```

11. When you are done, navigate to Source Control on the codespace on the left and then click on three dots at the top. Then select commit -> Commit all and finally push your changes to your GitHub repository.

      ![Push from Visual Studio Code](https://raw.githubusercontent.com/CloudLabsAI-Azure/AIW-DevOps/main/Assets/commitandpush.png)
      
      
Now, you can move on to the next page.
