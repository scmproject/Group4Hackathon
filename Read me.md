Problem Description: 
1)Develop a user-friendly search engine to efficiently locate and explain specific topics, concepts, or questions within NCERT textbooks. 
2)Help class teachers to prepare exam question papers. 
3)Help students to do the homework.

This is a user-friendly search engine to efficiently locate and explain specific topics, concepts, or questions within NCERT textbooks.
Also help class teachers to prepare questions for exams only with in the content provided.


Use your own data with Azure OpenAI
The Azure OpenAI Service enables you to use your own data with the intelligence of the underlying LLM. You can limit the model to only use your data for pertinent topics, or blend it with results from the pre-trained model.

In scenario for this exercise, You are a user-friendly search engine to efficiently locate and explain specific topics, concepts, or questions within NCERT textbooks.
Also help class teachers to prepare questions for exams only with in the content provided.

Provision an Azure OpenAI resource
If you don't already have one, provision an Azure OpenAI resource in your Azure subscription.

Sign into the Azure portal at https://portal.azure.com.

Create an Azure OpenAI resource with the following settings:

Subscription: Select an Azure subscription that has been approved for access to the Azure OpenAI service
Resource group: Choose or create a resource group
Region: Make a random choice from any of the following regions*
Australia East
Canada East
East US
East US 2
France Central
Japan East
North Central US
Sweden Central
Switzerland North
UK South
Name: A unique name of your choice
Pricing tier: Standard S0
* Azure OpenAI resources are constrained by regional quotas. The listed regions include default quota for the model type(s) used in this exercise. Randomly choosing a region reduces the risk of a single region reaching its quota limit in scenarios where you are sharing a subscription with other users. In the event of a quota limit being reached later in the exercise, there's a possibility you may need to create another resource in a different region.

Wait for deployment to complete. Then go to the deployed Azure OpenAI resource in the Azure portal.

Deploy a model
Azure OpenAI provides a web-based portal named Azure OpenAI Studio, that you can use to deploy, manage, and explore models. You'll start your exploration of Azure OpenAI by using Azure OpenAI Studio to deploy a model.

On the Overview page for your Azure OpenAI resource, use the Go to Azure OpenAI Studio button to open Azure OpenAI Studio in a new browser tab.

In Azure OpenAI Studio, on the Deployments page, view your existing model deployments. If you don't already have one, create a new deployment of the gpt-35-turbo-16k model with the following settings:

Model: gpt-35-turbo-16k (must be this model to use the features in this exercise)
Model version: Auto-update to default
Deployment name: A unique name of your choice. You'll use this name later in the lab.
Advanced options
Content filter: Default
Deployment type: Standard
Tokens per minute rate limit: 5K*
Enable dynamic quota: Enabled
* A rate limit of 5,000 tokens per minute is more than adequate to complete this exercise while leaving capacity for other people using the same subscription.

Observe normal chat behavior without adding your own data
Before connecting Azure OpenAI to your data, let's first observe how the base model responds to queries without any grounding data.

In Azure OpenAI Studio at https://oai.azure.com, in the Playground section, select the Chat page. The Chat playground page consists of three main sections:

Setup - used to set the context for the model's responses.
Chat session - used to submit chat messages and view responses.
Configuration - used to configure settings for the model deployment.
In the Configuration section, ensure that your model deployment is selected.

In the Setup area, select the default system message template to set the context for the chat session. The default system message is You are an AI assistant that helps people find information.

In the Chat session, submit the following queries, and review the responses:

Explain the process of photosynthesis in plants and its importance in the ecosystem?
How do plants adapt to different environments to obtain nutrients?
Discuss the role of chlorophyll in plant nutrition and how it affects the growth of plants?.

Try similar questions about Nutrition in Plants,Nutrition in Animals, Heat, Acids, Bases, and Salts, Physical and Chemical Changes that will be included in our subjects.

Connect your data in the chat playground
Now you'll add some data for a fictional travel agent company named Margie's Travel. Then you'll see how the Azure OpenAI model responds when using the brochures from Margie's Travel as grounding data.

In a new browser tab, download an archive of data from NCERT Book Links: https://ncert.nic.in/textbook.php?fesc1=0-11. Extract the PDF's to a folder on your PC.

In Azure OpenAI Studio, in the Chat playground, in the Setup section, select Add your data.

Select Add a data source and choose Upload files.

You'll need to create a storage account and Azure AI Search resource. Under the dropdown for the storage resource, select Create a new Azure Blob storage resource, and create a storage account with the following settings. Anything not specified leave as the default.

Subscription: Your Azure subscription
Resource group: Select the same resource group as your Azure OpenAI resource
Storage account name: Enter a unique name
Region: Select the same region as your Azure OpenAI resource
Redundancy: Locally-redundant storage (LRS)
While the storage account resource is being created, return to Azure OpenAI Studio and select Create a new Azure AI Search resource with the following settings. Anything not specified leave as the default.

Subscription: Your Azure subscription
Resource group: Select the same resource group as your Azure OpenAI resource
Service name: Enter a unique name
Location: Select the same location as your Azure OpenAI resource
Pricing tier: Basic
Wait until your search resource has been deployed, then switch back to the Azure AI Studio.

In the Add data, enter the following values for your data source, then select Next.

Select data source: Upload files
Subscription: Your Azure subscription
Select Azure Blob storage resource: Use the Refresh button to repopulate the list, and then choose the storage resource you created
Turn on CORS when prompted
Select Azure AI Search resource: Use the Refresh button to repopulate the list, and then choose the search resource you created
Enter the index name: margiestravel
Add vector search to this search resource: unchecked
I acknowledge that connecting to an Azure AI Search account will incur usage to my account : checked
On the Upload files page, upload the PDFs you downloaded, and then select Next.

On the Data management page select the Keyword search type from the drop-down, and then select Next.

On the Review and finish page select Save and close, which will add your data. This may take a few minutes, during which you need to leave your window open. Once complete, you'll see the data source, search resource, and index specified in the Setup section.

Tip: Occasionally the connection between your new search index and Azure OpenAI Studio takes too long. If you've waited for a few minutes and it still hasn't connected, check your AI Search resources in Azure portal. If you see the completed index, you can disconnect the data connection in Azure OpenAI Studio and re-add it by specifying an Azure AI Search data source and selecting your new index.

Chat with a model grounded in your data
Now that you've added your data, ask the same questions as you did previously, and see how the response differs.



Run your notebook
Open the python notebook NCERT_RAG.ipynb

Change the Modelname, Endpoint, OpenAI_key, azure_search_endpoint, azure_search_key in the notebook.
2.After making the changes, execute the notebook

Review the response to the prompt Tell me about London, which should includes an answer as well as some details of the data used to ground the prompt, which was obtained from your search service.
