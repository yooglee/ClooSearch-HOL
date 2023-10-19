<h1 align="center">
Backend Web Application - Bot API + Bot Service
</h1>

봇은 [Bot Framework](https://dev.botframework.com)를 사용하여 생성하였습니다.

사용되는 서비스 및 도구:

- Azure App Service (Web App) - Chatbot API Hosting
- Azure Bot Service - A service for managing communication through various channels

## Azure Web App에 봇 배포

아래의 가이드는 웹 채팅, MS Teams, Twilio, SMS, 이메일, 슬랙 등 여러 채널에 봇을 노출시키는 Azure Bot 서비스와 연결된 Bot API를 Azure Wep App으로 실행하는 단계입니다..

1. Azure Portal에서: Azure Active Directory -> App Registrations에서 Multi-Tenant App Registration (Service Principal) 생성

2. Multi-Tenant App Registration에서, Secret 생성 (단, 값을 따로 저장해야 한다)

3. Deploy the Bot Web App and the Bot Service by clicking the Button below and type the App Registration ID and Secret Value that you got in Step 1 along with all the other ENV variables you used in the Notebooks 

[![Deploy To Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fpablomarin%2FGPT-Azure-Search-Engine%2Fmain%2Fapps%2Fbackend%2Fazuredeploy-backend.json)

3. Zip the code of the bot by executing the following command in the terminal (**you have to be inside the app/backend/ folder**):
```bash
zip -j backend.zip ../../common/* ./*
```
4. Using the Azure CLI deploy the bot code to the Azure App Service created on Step 2
```bash
az login -i
az webapp deployment source config-zip --resource-group "<resource-group-name>" --name "<name-of-backend-app-service>" --src "backend.zip"
```
**Note**: If you get this error: `An error occured during deployment. Status Code: 401`. **Cause**: Some FDPO Azure Subscriptions disable Azure Web Apps Basic Authentication every minute (don't know why). **Solution**:  before running the above `az webapp deployment` command, make sure that your backend azure web app has Basic Authentication ON. In the Azure Portal, you can find this settting in: `Configuration->General Settings`.
Don't worry if after running the command it says retrying many times, the zip files already uploaded and is building.

5. In the Azure Portal: **Wait around 5 minutes** and test your bot by going to your Azure Bot Service created in Step 2 and clicking on: **Test in Web Chat**

6. In the Azure Portal: In your Bot Service , add multiple channels (Including Teams) by clicking in **Channels**

7. Go to apps/frontend folder and follow the steps in README.md to deploy a Frontend application that uses the bot.

## Reference documentation

- [Bot Framework Documentation](https://docs.botframework.com)
- [Bot Samples code](https://github.com/microsoft/BotBuilder-Samples)
- [Bot Framework Python SDK](https://github.com/microsoft/botbuilder-python/tree/main)
- [Bot Basics](https://docs.microsoft.com/azure/bot-service/bot-builder-basics?view=azure-bot-service-4.0)
- [Azure Bot Service Introduction](https://docs.microsoft.com/azure/bot-service/bot-service-overview-introduction?view=azure-bot-service-4.0)
- [Azure Bot Service Documentation](https://docs.microsoft.com/azure/bot-service/?view=azure-bot-service-4.0)
- [Channels and Bot Connector Service](https://docs.microsoft.com/azure/bot-service/bot-concepts?view=azure-bot-service-4.0)
