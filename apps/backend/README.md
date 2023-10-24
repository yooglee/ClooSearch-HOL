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

3. 아래 버튼을 클릭하여 Bot Web App과 Bot Service를 배포하고 배포단계에서 얻은 앱 등록 ID와 비밀 값을 노트북에서 사용한 다른 모든 ENV 변수와 함께 입력합니다. 

[![Deploy To Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fpablomarin%2FGPT-Azure-Search-Engine%2Fmain%2Fapps%2Fbackend%2Fazuredeploy-backend.json)

4. 터미널에서 다음 명령을 실행하여 봇의 코드 압축합니다. 단 압축된 코드는 appㄴ/backend 안에 있어야 합니다.
```bash
zip -j backend.zip ../../common/* ./*
```
5. Azure CLI를 사용하여 2번에서 작성된 Azure App Service에 봇 코드를 배포합니다
```bash
az login -i
az webapp deployment source config-zip --resource-group "<resource-group-name>" --name "<name-of-backend-app-service>" --src "backend.zip"
```
**Note**: If you get this error: `An error occured during deployment. Status Code: 401`. **Cause**: Some FDPO Azure Subscriptions disable Azure Web Apps Basic Authentication every minute (don't know why). **Solution**:  before running the above `az webapp deployment` command, make sure that your backend azure web app has Basic Authentication ON. In the Azure Portal, you can find this settting in: `Configuration->General Settings`.
Don't worry if after running the command it says retrying many times, the zip files already uploaded and is building.

6. Azure Portal에서: **5분 정도 기다리시고** 2단계에서 만든 Azure Bot Service로 이동하여 다음을 클릭하여 Bot을 테스트합니다: **Web Chat에서 테스트합니다**

7. Azure Portal의 Bot Service에서 **Channels**를 클릭하여 multiple channels를 추가합니다. 

8. apps/frontend 폴더로 이동하여 README.md 가이드에 따라 Frontend 애플리케이션을 배포합니다. 

## Reference documentation

- [Bot Framework Documentation](https://docs.botframework.com)
- [Bot Samples code](https://github.com/microsoft/BotBuilder-Samples)
- [Bot Framework Python SDK](https://github.com/microsoft/botbuilder-python/tree/main)
- [Bot Basics](https://docs.microsoft.com/azure/bot-service/bot-builder-basics?view=azure-bot-service-4.0)
- [Azure Bot Service Introduction](https://docs.microsoft.com/azure/bot-service/bot-service-overview-introduction?view=azure-bot-service-4.0)
- [Azure Bot Service Documentation](https://docs.microsoft.com/azure/bot-service/?view=azure-bot-service-4.0)
- [Channels and Bot Connector Service](https://docs.microsoft.com/azure/bot-service/bot-concepts?view=azure-bot-service-4.0)
