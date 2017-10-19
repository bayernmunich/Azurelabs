## Deploy ASP.Net application to Azure App Service using Team Services

This lab shows how you can deploy an **ASP.Net application to Azure App Service using an CI/CD pipeline in Visual Studio Team Services**.

## Pre-requisites

1. Microsoft Azure Account:</b> You will need a valid and active azure account for the labs.

2. You need a <b>Visual Studio Team Services Account</b> and <a href="http://bit.ly/2gBL4r4">Personal Access Token</a>


## Setting up the project

1. Use <a href="https://vstsdemogenerator.azurewebsites.net" target="_blank">VSTS Demo Data Generator</a> to provision a project on your VSTS account.

   ![](images/1.png)

2. Select **PartsUnlimited** for the template

   ![](images/2.png)

3. Once the project is provisioned, select the URL to navigate to the project that you provisioned


## Configuring the CI/CD pipeline

1. Let's start from code . Navigate to the **Code** hub 

   <img src="images/4.png">

2. We have an ASP.NET app code provisioned by the demo generator system. We will deploy this to Azure app service

3. We have a Continious Integration (CI) build setup to run upon a code commit. Let's make a simple change to the code to trigger the CI build

4. Open the file **Index.cshtml** by navigating to the path **PartsUnlimited-aspnet45/src/PartsUnlimitedWebsite/Views/Home/Index.cshtml**

   <img src="images/5.png">

5. Edit the code. For this example, let's change line 28 to increase discount from **50%** to **70%** 

   <img src="images/6.png">

6. Select **Commit** to save and commit the changes. 

7. The code commit will trigger the CI build. Go to the **Build** tab to see the CI build running in progress.

   <img src="images/7.png">

   While the build is in progress, let's explore the build definition. Below is the table which gives you the glimpse of the tasks that is being used in the current build definition.

   <table width="100%">
   <thead>
      <tr>
         <th width="50%"><b>Tasks</b></th>
         <th><b>Arguments</b></th>
      </tr>
   </thead>
   <tr>
      <td><a href="http://bit.ly/2ilmcHL"><b>Nuget Installer</b></a> <img src="images/nuget.png"></td>
      <td>Restore NuGet Packages/Dependencies for the solution </td>
   </tr>
   <tr>
      <td><a href="http://bit.ly/2xPrMUY"><b>Visual Studio Build</b></a> <img src="images/visual-studio-build.png"> </td>
      <td>Uses MSBuild arguments to build the solution </td>
   </tr>
   <tr>
      <td><a href="http://bit.ly/2xPqJ7f"><b>Visual Studio Test</b></a> <img src="images/vstest.png"> </td>
      <td>Run Unit Tests using Visual Studio Test Runner </td>
   </tr>
   <tr>
      <td><a href="http://bit.ly/2grMxTQ"><b>Copy Files</b></a> <img src="images/copy-files.png"> </td>
      <td>Used to Copy files from source to destination folder using match patterns </td>
   </tr>
   <tr>
      <td><a href="http://bit.ly/2yBgXde"><b>Publish Build Artifacts</b></a> <img src="images/publish-build-artifacts.png"> </td>
      <td> Used to share the build artifacts </td>
   </tr>
   </table>
   <br/>
   
   <img src="images/8.png">

8. Once the build is completed, you can see the summary which shows **test results, code coverage** etc as shown below.

   <img src="images/9.png">

## Continuous Delivery

we have a relase pipeline configured to deploy the application. It is associate to the build and triggered when the build is successful. Let's look at the release pipeline.

1. Navigate to the **Releases** tab under **Build and Release** hub

2. Select the **PartsUnlimitedE2E** definition and choose **Edit**

3. We have two environments **QA** and **Production**

We are using **Infrastructure as a Code** in the release pipeline with an ARM template to provision the required infrastructure (Web App and SQL database) on Azure 

Once the release is successful, you can login to [Azure Portal](https://portal.azure.com) and search a **Resource Group** with the name **AspDotNet** that would have got created. It would be associated with few other resources like **SQL server, SQL DB, WebApps** etc as shown below.

<img src="images/10.png">

Navigate to one of the WebApp from the resource group and you should see the application is deployed successfully with the changes made earlier as shown.

<img src="images/11.png">
