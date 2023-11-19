# A Step-by-Step Guide to Build a Custom Copilot Powered by Code Llama Model #

Custom Copilot is an application that employs state-of-the-art AI and large language models to assist users with complex cognitive tasks. It allows organizations to construct bespoke intelligent, conversational app experiences for their users, tailored to specific domains and unique pain points. Essentially, it’s like having your very own AI copilot to help navigate the complexities of tasks and interactions!

Lets build CodeBuddy a custom copilot powered by Code Llama, a leading AI platform for code generation and analysis. the custom Copilot integrates Azure AI Content Safety, a feature that detects and prevents harmful or inappropriate content. the copilot helps you with code completion and Infilling.

## High-level Architecture

![image](https://github.com/mahes-a/2023/assets/120069348/c91513ff-ad45-43a2-805e-bfcd17eb24d7)

## Custom Copilot Teams channel Screen Grab

![image](https://github.com/mahes-a/2023/assets/120069348/368fa3ce-57ce-44c4-9f28-3e5c3bcffd84)

## Prerequisites

- Azure subscription with Azure Machine Learning resource
- Deploying Llama 2 models require GPU compute of V100 / A100 SKUs. You can view and request AzureML compute quota [here](https://ml.azure.com/quota).
- Copilot Studio , Can be accessed from [here](https://copilotstudio.microsoft.com/).
- Teams for work or school , You can download from [here](https://go.microsoft.com/fwlink/?linkid=2187327&Lmsrc=groupChatMarketingPageWeb&Cmpid=directDownloadWin64&clcid=0x409&culture=en-us&country=us)

## Things to Note 

- The Model catalog in AML with Generative AI Models(like Code Llama) is in Preview

- Please note that this tutorial is intended for explorative and illustrative purposes only. It is meant to inspire ideas and should not be taken as prescriptive advice. Any implementation of the techniques described in this tutorial as part of your application should be thoroughly validated and evaluated to ensure accuracy, validity, compatibility with your specific use case and technical environment.


- If you aren't going use the real time Llama  deployment any more, Please delete the endpoint by following the article [here](https://learn.microsoft.com/en-us/azure/machine-learning/how-to-deploy-online-endpoints?view=azureml-api-2&tabs=azure-cli#delete-the-endpoint-and-the-deployment)

## Technical Flow

- Copilot Teams channel is enabled and deployed in Teams app. 
  
- The Copilot Conversational plugin makes HTTP Post requests with user prompts to the Azure Machine learning real-time inference endpoints  
  
- Azure Machine learning real-time inference endpoints hosts the CodeLlama-13b-hf model with built in Azure AI Content Safety monitoring
  
- The User prompts are monitored , validated and filtered by  Azure AI Content Safety resource
  
- Safe Prompts are sent to CodeLlama-13b-hf model and response from CodeLlama-13b-hf model is also monitored and validated by the Azure AI Content Safety resource
  
- Safe Responses are sent back to Teams Copilot via the Azure Machine learning real-time inference endpoints

## Steps

*As noted in the Prerequisites sections Deploying Llama 2 models requires GPU compute of V100 / A100 SKUs , Please ensure you have quota available for v100 or A100 , If you donot have quota , you may use a shared environment for a short period*

![image](https://github.com/mahes-a/2023/assets/120069348/791e54e0-cf92-42e2-ac81-09630f16930b)

**Create an Azure Machine learning real-time inference endpoint that hosts CodeLlama-13b-hf model with built in Azure AI Content Safety**

- Login into your ML Studio [here](https://ml.azure.com/)

   <img width="998" alt="image" src="https://github.com/mahes-a/StagingBuild/assets/120069348/b905bda5-79df-41cf-b444-28beb0543953">

- Select your workspace , Select "Model Catalog" menu from left and click on View models in Introducing Llama 2 section

   <img width="700" alt="image" src="https://github.com/mahes-a/2023/assets/120069348/927899d2-2f6c-45e1-a2fa-417b0f86ebeb">


- Choose Llama-2-7b-chat model 

   <img width="527" alt="image" src="https://github.com/mahes-a/2023/assets/120069348/49963b2a-f3d0-4c39-8837-cb4af5059ba2">


-  Click on Deploy -> Real-time endpoint option and ensure you chose Enable Azure AI Content Safety (Recommended) option and click proceed. Multiple layers of safety mechanism are recommended for LLM's for Responsible AI 

    <img width="501" alt="image" src="https://github.com/mahes-a/2023/assets/120069348/7e564cd7-7b1a-4bd0-9afe-9c54c7a39a2d">


    <img width="689" alt="image" src="https://github.com/mahes-a/2023/assets/120069348/852b737f-8681-4a70-8693-4310e368d8b5">


    
-  This will open a notebook

     <img width="981" alt="image" src="https://github.com/mahes-a/StagingBuild/assets/120069348/0ad10429-b803-4551-8bb2-62dcb4edee56">

- Clone the notebook and open llama-safe-online-deployment.ipynb

     <img width="1019" alt="image" src="https://github.com/mahes-a/StagingBuild/assets/120069348/6f7c1b10-1c54-46a4-bade-8b8a79d17e9d">

 -  Choose your compute, either your pre-created compute instance or serverless if you don’t have compute instance created

-  Choose the model_name as "Llama-2-7b-chat" , update endpoint_name , deployment_name  if needed and  The sku_name would be the A100 or V100 GPU SKU , at present  it defaults to Standard_NC24s_v3 SKU and execute the notebook

   <img width="893" alt="image" src="https://github.com/mahes-a/2023/assets/120069348/0abc20ed-cf9d-4ae0-9f7a-8a3774737d2f">



-  Uncomment and install dependencies

    <img width="534" alt="image" src="https://github.com/mahes-a/StagingBuild/assets/120069348/dc5bf161-2344-4dd2-88aa-4432d6b7562e">

-  Run the Get Credential section

    <img width="766" alt="image" src="https://github.com/mahes-a/StagingBuild/assets/120069348/3c8fe189-5599-452f-a3b3-525e93f089e8">

-  Enter your AML subscription_id , resource group and aml workspace name and execute

   <img width="625" alt="image" src="https://github.com/mahes-a/StagingBuild/assets/120069348/88be6260-acf2-4748-bec8-f5e33853da2f">

   
- Kindly go through the Note section to understand the  region to deploy the Azure AI Content Safety and Choose the region for deploying the content safety resource according to your needs and compliance laws , The default choses "east us" , Please change according to your needs

     <img width="994" alt="image" src="https://github.com/mahes-a/StagingBuild/assets/120069348/e5a24d4f-3ec8-44e3-a182-274b8c24224e">

- Create Azure content safety resource
  
    <img width="749" alt="image" src="https://github.com/mahes-a/StagingBuild/assets/120069348/8f16e7bf-28f2-42f4-ae0f-7220e0841b68">

    
- Optional , You can validate the created resource by searching for the content safety resource in the Azure Portal

     <img width="1169" alt="image" src="https://github.com/mahes-a/StagingBuild/assets/120069348/33dd7224-cf94-4ec0-9287-b34c19f2e00a">

- Run 3.1 and 3.2 to create a endpoint for the Llama-2-7b-chat , this would take few minutes

  <img width="736" alt="image" src="https://github.com/mahes-a/StagingBuild/assets/120069348/b0894558-35d6-4e29-b375-896248762550">

- Deploy the Llama 2 model with the content safety resource to the endpoint created in previous steps , this would take several minutes , wait for completion 
  
    <img width="698" alt="image" src="https://github.com/mahes-a/StagingBuild/assets/120069348/7230fa15-a23c-4577-81d0-5c7384ecf10e">

- Once Successful , switch to the endpoints section and click on the Llama2 endpoint in the Real-time endpoint section

    <img width="877" alt="image" src="https://github.com/mahes-a/StagingBuild/assets/120069348/cfd87fef-ec4b-4c86-ac3c-a50f4c5b6564">

    <img width="937" alt="image" src="https://github.com/mahes-a/StagingBuild/assets/120069348/5ea8d98c-7891-48bc-837c-95317fee6867">