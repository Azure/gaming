<span id="_Local_Development" class="anchor"><span id="_Toc519166846" class="anchor"></span></span>Set up Azure Functions
=========================================================================================================================

<span id="_Azure_Functions_1" class="anchor"><span id="_Toc519166847" class="anchor"></span></span>Prerequisites
----------------------------------------------------------------------------------------------------------------

The Azure Functions emulator can be installed locally on multiple
operating systems, allowing you to develop Azure Functions in your own
development environment.

In this guide we will be walking through how to install Azure Functions
Core Tools version 2.x on Windows. To learn more, visit
<https://docs.microsoft.com/en-us/azure/azure-functions/functions-run-local>.

### Verify .NET 4.7.1 for Windows is installed

If you are unsure how to verify .NET Framework versions, follow this
[link](https://docs.microsoft.com/en-us/dotnet/framework/migration-guide/how-to-determine-which-versions-are-installed).

Download and install .NET 4.7.1 for Windows using this
[link](https://www.microsoft.com/net/download/windows).

### <span id="_Configure_Pinball_Lizard" class="anchor"><span id="_Toc519166849" class="anchor"><span id="_Hlk514190895" class="anchor"></span></span></span>Install Visual C++ Redistributable Packages for Visual Studio 2013

Install the above package
[here](https://indigoslate.sharepoint.com/sites/pwa/VR%20Game%20Development%20%20Launch%20%5bAzure%20Gaming%5d/Shared%20Documents/Working/GitHub%20step%20by%20step/Visual%20C++%20Redistributable%20Packages%20for%20Visual%20Studio%202013).

**\*\*\*Note**: Make sure to install x86.

### Install Node.js

Download and install the Node.js msi installer under **Current Latest
Features** using this [link](https://nodejs.org/en/download/).

**\*\*\*Note**: Make sure to install version 10.x or later.

### Install Git for Windows and download Pinball Lizard repo

Download and install Git using this
[link](https://git-scm.com/downloads). Open PowerShell and clone the
Pinball Lizard repo by running the following command:

**\*\*\*Note**: **Set-ExecutionPolicy RemoteSigned** might be required.

```git clone https://github.com/Azure/gaming.git```

![](/media/image3.png)

### Install Azure Functions Core tools package

<span id="_Hlk518390169" class="anchor"></span>Open PowerShell and
navigate to the working directory, and then run the following command:

```npm install -g azure-functions-core-tools ```

![](/media/image4.png)
### Install VSCode and the Azure Functions extension

Download and install VSCode using this
[link](https://code.visualstudio.com/). Search for and download the
**Azure Functions** extension.

![](/media/image5.png)

Once the extension is installed, click **reload** and log into your
Azure account by clicking on the Azure logo in the Activity bar to show
the **Azure Functions** explorer. Click **Sign in to Azure...** and
follow the instructions.

![](/media/image6.png)

![](/media/image7.png)

![](/media/image8.png)

Load and configure Azure Functions locally
------------------------------------------

Now that you have the core utilities installed for Azure Functions, it’s
time to launch them. Open PowerShell and then navigate to the
**web-functions** subdirectory in the Pinball Lizard source code you
downloaded:

```cd <source\_dir>\gaming\\pinball-lizard\\web-functions```

Start the Azure Functions host with the following command:

```func host start --useHttps```

![](/media/image9.png)
Press **CTRL+C** to terminate the job.

![](/media/image10.png)

We will now implement the same logic inside VS Code to ensure that all
testing is consistent. In VS Code, open the **tasks.json** file and edit
the **command** value to match the same command we used earlier.

![](/media/image11.png)

<span id="_Hlk520715509" class="anchor"></span>

```"command": "func host start",```

change to

```"command": " func host start --useHttps ",```

Press **F5** to launch the Functions Host.

![](/media/image12.png)

> **\*\*\*Note**: VSCode has known issues with attaching a debugger when
> using Azure Functions Core tools. You can safely either ignore the
> message below or disable the debugger.
>
> ![](/media/image13.png)

Press **CTRL+C** to terminate the job.

### <span id="_Toc519166867" class="anchor"><span id="_Toc519166854" class="anchor"></span></span>Bindings and triggers

Azure Functions uses different methods—bindings and triggers—to access
the data it needs. Bindings are input and output pipelines that can
access a multitude of data types. Triggers are similar to “calling” the
function, and a single payload is required for the trigger to function.

Although there are many types of bindings that can be used, Pinball
Lizard used Cosmos DB.

Learn more about [Azure Functions triggers and
bindings](https://docs.microsoft.com/en-us/azure/azure-functions/functions-triggers-bindings).

### Configuration function

**\*\*\*NOTE:** The configuration function is the most critical piece of
Pinball Lizard. Everything else will fail if this is not functional and
in place, either locally or in Azure, before you attempt to start.

All configurations are applied through the Azure portal via Azure
Functions Application settings, or through the local Azure Functions
settings (&lt;pinball lizard
root&gt;\\web-functions\\local.settings.json).

![](/media/image14.png)

This function will return all of the configuration parameters required
for the Node.js applications. A short description has been included
about each configuration.

This guide will walk you through each of these settings step by step so
you do not have to worry about filling in the values. The links will
point to where in the guide you can find instructions for that paticular
value. Values that do not have links will accept the default value
listed.

-   [APP\_GATEWAY\_IP: \[IP ADDRESS\]](#_Azure_Application_Gateway)
    -   IP address of Azure application gateway
-   [SSL\_PASSPHRASE: \[STRING\]](#_SSL_Certs)
    -   Passphrase for SSL cert being used for certs that were generated
-   [CONFIG\_ENDPOINT\_URL: \[URL\]](#endpoint-uris)
    -   Base URL for Azure Functions endpoint (local or Azure)
-   [CONFIG\_ENDPOINT\_KEY: \[HOST KEY\]](#authorization-keys)
    -   Hashed key that the client is required to send before it will
        receive any data
    -   Key that must also be configured through Function App settings
        in the Azure Functions blade (via the Azure portal, under the
        Host Keys (All functions) section)
-   [COMMANDER\_IP: \[IP ADDRESS\]](#_Azure_container_instances)
    -   IP address given to commander container
-   COMMANDER\_PORT: \[INT\]
    -   Default: 9221
-   [COMMANDER\_TOKEN: \[STATIC KEY\]](#_Auth_Tokens)
-   COMMANDER\_NUMBER\_OF\_FORMATIONS: \[INT\]
    -   Default: 12
    -   Max number of formations that commander will keep at once
-   COMMANDER\_FORMATION\_SIZE: \[INT\]
    -   Default: 6
    -   Max number of Ice Lice per formation
-   [INTERNAL\_RELAY\_IP: \[IP ADDRESS\]](#_Record_the_Container)
    -   IP address given to internal relay container
-   INTERNAL\_RELAY\_PORT: \[INT\]
    -   Default: 9125
-   [INTERNAL\_RELAY\_AUTH\_TOKEN: \[STATIC KEY\]](#_Auth_Tokens)
-   INTERNAL\_RELAY\_ALLOWED\_HOSTS: \[CSV STRING}
    -   List of URLs that are allowed to communicate with internal relay
        (comma separated)
-   [ROUTER\_IP: \[IP ADDRESS\]](#_Azure_container_instances)
    -   IP address given to router container
-   ROUTER\_PORT: \[INT\]
    -   Default: 9010
-   ROUTER\_WS\_PORT: \[INT\]
    -   Default: 8085
-   [ROUTER\_AUTH\_TOKEN: \[STATIC KEY\]](#_Auth_Tokens)
-   [MIXER\_HANDLER\_TOKEN: \[STATIC KEY\]](#_Auth_Tokens)
-   [MIXER\_HANDLER\_IP: \[IP ADDRESS\]](#_Mixer)
    -   IP address given to Mixer container
-   MIXER\_HANDLER\_PORT: \[INT\]
    -   Default: 9110
-   [MIXER\_CHANNEL\_TOKEN: \[STATIC KEY\]](#_Mixer)
    -   Key obtained from Mixer website on a per-channel basis
-   [MIXER\_CHANNEL\_VERSION\_ID: \[INT\]](#_Mixer)
    -   Version of Mixer channel being used
-   MIXER\_BUTTON\_COOLDOWN: \[INT\]
    -   Default: 5000
    -   How long button cooldown for interaction should be
-   [PLAYFAB\_HANDLER\_IP: \[IP ADDRESS\]](#_Azure_container_instances)
    -   IP address given to PlayFab container
-   PLAYFAB\_HANDLER\_PORT: \[INT\]
    -   Default: 9111
-   [PLAYFAB\_HANDLER\_TOKEN: \[STATIC KEY}](#_Auth_Tokens)
-   [PLAYFAB\_API\_TITLE\_ID: \[STATIC KEY\]](#_PlayFab)
    -   Title obtained from PlayFab website
-   [PLAYFAB\_API\_SECRET: \[STATIC KEY\]](#_PlayFab)
    -   Key obtained from PlayFab website
-   [ROUTER\_FUNCTION\_ENDPOINT: \[URL\]](#endpoint-uris)
    -   Endpoint URL for router function located at &lt;base
        URL&gt;\\api
-   [ROUTER\_FUNCTION\_KEY: \[STATIC KEY\]](#router)
    -   Host key found in Function App settings, part of Azure Functions

When the configuration is changed, **config/reload** is called, and the
app config will be reloaded in memory.

### Create a default function

The Function App in Azure requires a default response in order to
prevent it from automatically shutting down. We will be creating a
simple HTTP trigger function that returns an “ok” response.

Open up the **web-functions** folder in VS Code and create a new
function **folder** called **default.**

![](/media/image15.png)

Within the default folder, first create a **function.json** file with
the following copied and pasted contents:
```
{
"disabled": false,
"bindings": \[
{
"authLevel": "function",
"type": "httpTrigger",
"direction": "in",
"name": "req",
"methods": \[
"get"
\]
},
{
"type": "http",
"direction": "out",
"name": "res"
}
\]
}
```
Then create an **index.js** file with the following contents within the
same default function folder:
```
'use strict'
/\*
\*
\* Returns a default response. This is required to prevent the Function
App from auto closing
\*
\*/
module.exports = function (context, req) {
context.res = {
body: 'ok'
};
context.done();
}
```
Press **CTRL+K, S** to save all changes.

Set up Cosmos DB
---------------

### Prerequisites

If you do not already have an Azure subscription, you can create one for
free at the following link: <https://aka.ms/azft-gaming>

### Create the Cosmos DB resource

From your Azure dashboard click the **+** icon in the top left corner,
search the marketplace for **Azure Cosmos DB**, and click **Create.**

![](/media/image16.png)

Name your Cosmos Db instance and select the **SQL** api. If you do not
have an existing resource group, create a new one. Choose your location
and click **Create** when finished.

**\*\*\*Note:** Naming will be unique to your environment.

![](/media/image17.png)

Once you hit save and the Cosmos DB instance is deployed, you will land
at the Quick Start page. On the left, navigate to **Collections -&gt;
Browse -&gt; + Add Database** and create a database called **VR-Game.**

![](/media/image18.png)

### Record the connection string

Once you have created the Cosmos DB resource, you will need to record
the connection string under the left side navigation to **SETTINGS -&gt;
Keys -&gt; PRIMARY CONNECTION STRING** and input the string into the
following two places: **Azure Functions App** setting (applied in later
[steps](#application-settings)) and the **local.settings.json** file in
the web-functions folder.

```AzureWebJobsDocumentDBConnectionString```

Azure Storage account
---------------------

Create an Azure Storage account by clicking **+ Create a resource -&gt;
Storage -&gt; Storage account.**

![](/media/image19.png)

Name the storage account and select the **Location** and **Resource
Group** used in previous steps.

![](/media/image20.png)

Once you have created the storage account, open the newly created
account and navigate to **SETTINGS -&gt; Access Keys** and record the
**connection string** for the following Azure Functions app setting:

![](/media/image21.png)

```  "AzureWebJobsStorage":"DefaultEndpointsProtocol=https;AccountName=[name];AccountKey=[key]"```

<span id="_Azure_Application_Gateway" class="anchor"><span id="_Toc519166860" class="anchor"></span></span>Azure Application Gateway
------------------------------------------------------------------------------------------------------------------------------------

Azure Application Gateway is a web traffic load balancer that enables
you to manage traffic to your web applications.

### Create the Gateway

Login to the Azure portal and click **+ Create a resource** found on the
upper left-hand corner of the Azure portal. Select **Networking** and
then select **Application Gateway**.

![](/media/image22.png)

Enter a name for your gateway and select the resource group you created
in previous steps. Make sure to choose the same **Location** used when
creating your resource group and Cosmos DB resource. Select an
**Instance count** of **2**.

![](/media/image23.png)

Create a new **virtual network** and **public IP**.

![](/media/image24.png)

Configure listener to use **HTTP**, then **Review** and **Create**.

![](/media/image25.png)

Once the Application Gateway has been completely deployed, record the
**frontend public IP address** (**gateway IP)** and update the following
value in the **Azure Configuration Function**.

-   APP\_GATEWAY\_IP: \[IP ADDRESS\]

Azure App Service plan
----------------------

In order to help allocate resources, we will be creating an App Service
plan in the Azure Portal.

Click **+ Create a resource** from the top left navigation and then
search for or select **App Service plan**.

![](/media/image26.png)

Change the OS type to **Windows** and the location to the **same region
as your storage account**, and then choose your existing resource group.

![](/media/image27.png)

Azure Function App
------------------

We will be creating the Function App in the portal and configuring a git
CI.

Click **+ Create a resourse -&gt; Compute -&gt; Function App**.

![](/media/image28.png)

Select **Windows OS** and **App Service**, then select the App Service
plan you created in the previous steps. Use your **existing resource
group**.

![](/media/image29.png)

### Application settings

Open up the newly created Function App and navigate to **Overview -&gt;
Application settings -&gt; General settings**. Turn off PHP support and
FTP access, as neither are needed, and change platform to **64-bit**.

![](/media/image30.png)

Under Application settings, click **+ Add new setting** and add the
application settings found in your
**web-functions\\local.settings.json** file. Click **Save** at the top
when done.

\*\*\*NOTE: The example screenshot does not contain all of the necessary
settings.

![](/media/image31.png)

#### Endpoint URIs

We will need to update the Endpoint URIs to point to the Azure Function
App. Find the Function App URL under **&lt;your\_function\_app&gt; -&gt;
Overview URL.** It will be in this format: .

Replace localhost:7071 with the above URL for both of the following
values in your application settings and local.settings.json.

**Note:** CONFIG\_ENDPOINT\_URI ends with &lt;URL&gt;/api.config, and
ROUTER\_FUNCTION\_ENDPOINT ends with &lt;URL&gt;/api as shown in the
example below:

CONFIG\_ENDPOINT\_URI

```CONFIG_ENDPOINT_URI":"https://https://pinlizfunctionapp.azurewebsites.net/api/config"```


ROUTER\_FUNCTION\_ENDPOINT

```"ROUTER_FUNCTION_ENDPOINT":"https://https://pinlizfunctionapp.azurewebsites.net/api"```

### Authorization Keys

HTTP triggers let you use keys for added security. A standard HTTP
trigger can use these as an API key, requiring the key to be present on
the request. Webhooks can use keys to authorize requests in a variety of
ways, depending on what the provider supports.

Host Keys are shared by all functions within the Function App and allow
access to any function within the Function App. We will be generating
Host Keys for the Router Function and Config Endpoint.

Open the Function App and navigate to **Platform Features -&gt; Function
app settings -&gt; Host Keys -&gt; Add a new host key.** Name the host
key but leave the value **blank** to generate a random key value**. **

![](/media/image34.png)

##### Router

Name the Router Function key as follows:

ROUTER\_FUNCTION\_KEY

Update the corresponding value in both the apps settings of the Function
App and within your local.settings.json file with the generated key.

##### Config 

Name the Router Function key as follows:

CONFIG\_ENDPOINT\_KEY

Update the corresponding value in both the apps settings of the Function
App and within your local.settings.json file with the generated key.

![](/media/image35.png)

### Code deployment using Git

Open up the Function App and navigate to **Platform features -&gt; CODE
DEPLOYMENT -&gt; Deployment options.** This action will open right tabs.
Then select **Setup** under **Deployments** tab. Then **Choose Source**
and select either **Git or Github.** The following examples will use a
local Git repository:

![](/media/image36.png)

Create a deployment user name and password and click **OK** when done.

![](/media/image37.png)

Once you have chosen the source, navigate to **General Settings -&gt;
Properties**.

![](/media/image38.png)

Record/save the **GIT URL** in properties for later use.

![](/media/image39.png)

**\*\*\*Note**: If using a local Git repo, you can change the deployment
credentials by navigating to **CODE DEPLOYMENT -&gt; Deployment
credentials**.

![](/media/image40.png)

<span id="_Auth_Tokens" class="anchor"><span id="_Toc519166864" class="anchor"></span></span>Auth tokens 
---------------------------------------------------------------------------------------------------------

To generate a random UUID for authentication between containers, use
this [online UUID generator](https://www.uuidgenerator.net/) or one of
your choosing. Once you have generated the auth tokens, insert them in
your Function App settings and in local.settings.json file for the
following values:

"ROUTER\_AUTH\_TOKEN":

"MIXER\_HANDLER\_TOKEN"

"INTERNAL\_RELAY\_AUTH\_TOKEN":

"PLAYFAB\_HANDLER\_TOKEN"

"COMMANDER\_TOKEN"

<span id="_PlayFab" class="anchor"><span id="_Toc519166865" class="anchor"></span></span>PlayFab 
-------------------------------------------------------------------------------------------------

PlayFab is a game-monitoring service that also offers dashboard views
for ranking and gameplay statistics. We used PlayFab to monitor the
winners at GDC who won prizes for their efforts.

Create a PlayFab account at this
[link](https://developer.playfab.com/en-us/sign-up). For this guide, you
can create an account with minimal information.

After creating a PlayFab account, add the information below to the
Application Settings tab of Azure Functions and to local.settings.json
file.

-   <span id="_Hlk519068014"
    class="anchor"></span>PLAYFAB\_API\_TITLE\_ID: \*\*\*PLAYFAB API
    TITLE\*\*\*

    Found under **Settings -&gt; API -&gt; Title ID**

    ![](/media/image41.png)

    ![](/media/image42.png)

-   PLAYFAB\_API\_SECRET: \*\*\*PLAYFAB API SECRET\*\*\*

    Found under **Settings-&gt; Secret Keys**

    ![](/media/image43.png)

**\*\*\*Note:** The values are required to integrate PlayFab with
Pinball Lizard and must be entered into the application settings of
Azure Functions.

<span id="_Mixer" class="anchor"><span id="_Toc519166866" class="anchor"></span></span>Mixer
--------------------------------------------------------------------------------------------

Mixer is a streaming service that allows the viewer to interact with the
person playing the game through various methods. Pinball Lizard uses a
single button. We use Mixer in Pinball Lizard by allowing viewers to
deploy a radioactive Ice Lice that the player can eat to gain the
ability to use the breath weapon.

This is also where we use the microphone built into the Odyssey headset.
The player must hold the radioactive Ice Lice near their mouth and make
a chewing noise to eat it. Afterwards, if the player makes a loud,
roaring noise, it will activate the breath weapon to inflict massive
damage.

Create a Mixer Account at this [link](https://mixer.com/).

**\*\*\*Note**: If you run into issues in Edge, try a different browser.

Create an OAuth client by navigating to **MORE -&gt; Developers -&gt;
Developer Lab**.

Click on the “LET’S CREATE ONE” button as shown in the figure below:

![](/media/image44.png)

Name the client and input **localhost** for the Hosts option. Do not
select **Use Secret Key.**

![](/media/image45.png)

Copy the Client ID to the following URI, which you can navigate to your
browser:

```
https://mixer.com/oauth/authorize?redirect\_uri=https:%2F%2Flocalhost%2Foauth-return&response\_type=token&scope=interactive%3Arobot%3Aself&client\_id=<Client_Id>
```
![](/media/image46.png)

The navigation should show 'Page not found.’ Examine the URI you were
directed to in order to find your oauth\_token. Copy the token part from
the URL similar as shown below:
```
https://localhost/oauth-return#access_token=v6G2g73HAkoKEbuEZqpalkNhEZh8cv8SPUVNN9RaZvRWx7jOg1Bfdioavtxjkv7B&token_type=Bearer&expires_in=31535528&state=
```
Here is another screen shot from a different URL.

![](/media/image47.png)

After creating a Mixer account and generating an access token, add the
following information to the Application Settings tab of Azure
Functions:

-   MIXER\_CHANNEL\_TOKEN: \*\*\*MIXER CHANNEL TOKEN\*\*\*

    -   This will be the OAuth token generated in the previous steps.

-   MIXER\_CHANNEL\_VERSION\_ID: \*\*\*MIXER CHANNEL VERSION\*\*\*

To obtain a **VERSION ID**, first navigate to **Developer lab -&gt;
MIXPLAY PROJECTS** and create a new project.

![](/media/image48.png) ![](/media/image49.png)

Next, navigate to the **Code** tab where you will find the **VERSION ID**.

![](/media/image50.png)

**Note:** <span id="_Architecture_Diagram" class="anchor"><span
id="_Local_Azure_Functions" class="anchor"></span></span>The values are
required to integrate Mixer with Pinball Lizard and must be entered into
the Application settings of Azure Functions and on local.settings.jason
file.

Create local Git repo
---------------------

We will be creating a separate Git repo for our functions. Open the
terminal in VS Code and navigate to
**&lt;pinball\_root&gt;\\pinball-lizard\\web-functions.**

![](/media/image51.png)

Type the following command to intialize the Git repo:

```git init```

![](/media/image52.png)

Type the following command to stage all files:

```git add -A```

![](/media/image53.png)

Type the following command to run your initial commit:

```git commit -m ‘initial’```

![](/media/image54.png)

Set the remote URL to point to the Azure Functions App using the Git URL
recorded in [previous steps](#code-deployment-using-git) and the
following command:

```git remote add azure <git\_url>;```

![](/media/image55.png)

Verify by running the following command:

```git remote -v```

![](/media/image56.png)

Publish Azure Functions
-----------------------

If you want to use Azure to host serverless functions, you need to
publish Azure Functions. This can be verified by checking the portal for
populated Azure Functions.

In order to publish your locally developed Azure Functions using Git,
enter the Git credentials you created earlier when you run the following
command from within the **web-functions** folder:

```git push azure master```

![](/media/image57.png)

![](/media/image58.png)

Once you have successfully published your Azure Functions, you can
verify if your deployment was successful in the Azure portal by
navigating to your Function App and expanding Functions.

![](/media/image59.png)

Learn more about [how to publish Azure
Functions](https://stackoverflow.com/questions/45387036/how-to-publish-azure-function-from-vs-code).

Verify deployment 
------------------

We will be using Postman to verify successful deployment of our
functions.

Install Postman using this [link](https://www.getpostman.com/apps).

In order to test the Config function, we will need to add the Router
Function key, located in your Function App settings, to the header of
the request. Add the following key and value under headers in your GET
request:

![](/media/image60.png)

x-functions-key &lt;ROUTER\_FUNCTION\_KEY&gt;

Create a **GET** request pointing to the **config URL** and click
**Send**.

If successful, it should return the Function configuration with
populated values from your application settings.

![](/media/image61.png)

You can run another test to see if the initialize function is generating
an instance ID, which is a requirement of several other functions. By
looking at the **index.js** for the **initialize function**, you can
determine that the function is looking for a player name in the payload,
and if one does not exist it will generate one.

![](/media/image62.png)

<span id="_Hlk520885842" class="anchor"></span>In order to test the
initialize function, you will need to add the Router Function key to the
header of the request and provide a body. Add the following **key** and
**value** under headers in your **POST**:

x-functions-key &lt;ROUTER\_FUNCTION\_KEY&gt;

![](/media/image63.png)

For the body, select **raw** and provide empty brackets, as shown below,
since the function will generate a random player name.

![](/media/image64.png)

Ensure your URI is pointing to the initialize function.

e.g. https://&lt;your\_function\_app\_ URI&gt;/api/intialize

![](/media/image65.png)

Click **Send** and you should receive a response providing you with an
instance ID. Your response should look similar to the following:

![](/media/image66.png)

<br><br>
[PREV: Introduction](/docs/01_Introduction.md)

[NEXT: Set up containers](/docs/03_Set_up_containers.md)
