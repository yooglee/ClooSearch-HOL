![image](https://user-images.githubusercontent.com/113465005/226238596-cc76039e-67c2-46b6-b0bb-35d037ae66e1.png)

# ClooSearch powered by: Azure Search + Azure OpenAI + Bot Framework + Langchain + Azure SQL + CosmosDB + Bing Search API
[![Open in GitHub Codespaces](https://github.com/codespaces/badge.svg)](https://codespaces.new/MSUSAzureAccelerators/Azure-Cognitive-Search-Azure-OpenAI-Accelerator?quickstart=1)
[![Open in VS Code Dev Containers](https://img.shields.io/static/v1?style=for-the-badge&label=Remote%20-%20Containers&message=Open&color=blue&logo=visualstudiocode)](https://vscode.dev/redirect?url=vscode://ms-vscode-remote.remote-containers/cloneInVolume?url=https://github.com/MSUSAzureAccelerators/Azure-Cognitive-Search-Azure-OpenAI-Accelerator)

본 Hands-on-Lab에는 다양한 위치에 흩어져 있는 다양한 종류의 데이터를 이해할 수 있는 Multi-Channel Smart Chatbot 및 검색 엔진이 필요하며, 또한 대화형 챗봇은 문의에 대한 답변과 함께 출처 및 답변을 얻을 수 있는 방법 및 위치에 대한 설명을 제공할 수 있어야 합니다. 즉, 귀사의 비즈니스 데이터에 대한 질문을 해석, 이해 및 답변할 수 있는 프라이빗 및 보안 ChatGPT 구축을 목표로 하고 있습니다.

이 Hands-on-Lab의 목표는

1. Bot Framework로 구축되어 여러 채널(Web Chat, MS Teams, SMS, Email, Slack 등)에 동기화 할 수 있는 Backend Bot API 구성
2. 검색과 봇 UI가 있는 프론트엔드 웹 애플리케이션 구성 

이 두개지의 Goals을 통하여 사용자 자신만의 데이터를 구축된 ChatBot을 통해 보여주는 것 입니다.

**For Microsoft FTEs:** This is a customer funded VBD, below the assets for the delivery.

| **Item**                   | **Description**                                                                                                     | **Link**                                                                                                                                                |
|----------------------------|---------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------|
| VBD SKU Info and Datasheet                   | CSAM must dispatch it as "Customer Invested" against credits/hours of Unified Support Contract. Customer decides if 3 or 5 days.                                      | [ESXP SKU page](https://esxp.microsoft.com/#/omexplanding/services/14486/geo/USA/details/1)                                                                                              |
| VBD Accreditation for CSAs     | Links for CSAs to get the Accreditation needed to deliver the workshop                                                                      | [Link 1](https://learningplayer.microsoft.com/activity/s9261799/launch) , [Link 2](https://learningplayer.microsoft.com/activity/s9264662/launch) |
| VBD 3-5 day POC Asset (IP)  | The MVP to be delivered  (this GitHub repo)                                     | [Azure-Cognitive-Search-Azure-OpenAI-Accelerator](https://github.com/MSUSAzureAccelerators/Azure-Cognitive-Search-Azure-OpenAI-Accelerator)                |
| VBD Workshop Deck          | The deck introducing and explaining the workshop                                                                    | [Intro AOAI GPT Azure Smart Search Engine Accelerator.pptx](https://github.com/MSUSAzureAccelerators/Azure-Cognitive-Search-Azure-OpenAI-Accelerator/blob/main/Intro%20AOAI%20GPT%20Azure%20Smart%20Search%20Engine%20Accelerator.pptx) |
| CSA Training Video         | 2 Hour Training for Microsoft CSA's                                                                    | [POC VBD Training Recording](https://microsoft.sharepoint.com/teams/CSUDataAI/_layouts/15/stream.aspx?id=%2Fteams%2FCSUDataAI%2FShared%20Documents%2FTech%20Talk%2FAI%20%26%20ML%2FAOAI%203%2DDay%20Workshop%20Training%2FAzure%20OpenAI%20%5F%20VBD%20Delivery%20Training%20%5F%20Option%201%2D20230607%5F090249%2DMeeting%20Recording%20%281%29%2Emp4&referrer=Teams%2ETEAMS%2DELECTRON&referrerScenario=p2p%5Fns%2Dbim&ga=1) |


---
**Hands on Lab을 위한 사전 필수 항목**
* 애저 구독 (Azure subscription)
* GPT-4를 포함한 Azure Open AI에 대한 신청 승인.
* 사용자는 해당 리소스 그룹에 대한 기여자 이상의 권한 
* Resource Group 생성.
* 멀티 테넌트(Multi-tenant) 앱 등록(서비스 주체).

---
# 아키텍쳐 
![Architecture](https://github.com/MSUSAzureAccelerators/Azure-Cognitive-Search-Azure-OpenAI-Accelerator/blob/main/images/GPT-Smart-Search-Architecture.jpg)

## Flow
1. 사용자가 질문을 입력합니다.
2. 앱에서 OpenAI의 GPT-4모델은 LLM으로써 프롬프트를 사용하여 사용자의 질문을 기반으로 가장 적합한 답을 찾을 소스를 결정합니다. 
3. 네 가지 유형의 소스:
   * 3a. Azure SQL Database 
   * 3b. Azure Bing Search API (인터넷 접근) 
   * 3c. Azure Cognitive Search
      * 3c.1. OpenAI을 사용하여 상위 K개 문서 청크를 벡터화합니다. 
      * 3c.2. 벡터 기반 인텍스를 생성합니다. 
      * 3c.3. 벡터 기반 인덱스에 대해 벡터 검색을 수행하여 상위 N개 청크를 가져옵니다.
   * 3d. CSV Tabular File 
4. 사용자 질문을 기반으로 앱은 소스로부터 결과를 검색하고 답을 생성합니다. 
5. 튜플(Question and Answer)은 CosmosDB에 저장되어 대화내용을 기억 및 추가 분석을 기록할 수 있습니다.
6. 답변을 사용자 전달합니다. 

---
## Microsoft 데모

https://gptsmartsearch.azurewebsites.net/

마이크로소프트 팀의 데모봇을 열려면 [여기](https://teams.microsoft.com/l/chat/0/0?users=28:5d583679-8196-4673-9d77-c294c010bca5)를 클릭하세요. 

---

## 🔧**특성**

   - [Bot Framework](https://dev.botframework.com/) 및 [Bot Service](https://azure.microsoft.com/en-us/products/bot-services/)를 사용하여 Bot API 백엔드를 호스팅하고 MS Teams를 포함한 여러 채널에 적용 가능합니다.
   - 100% Python.
   - [Azure Cognitive Services](https://azure.microsoft.com/en-us/products/cognitive-services/)를 사용하여 언어 탐지, OCR 이미지, 키 구문 추출, 개체 인식(인물, 이메일, 주소, 조직, URL) 등 비정형 문서를 색인화하고 풍부하게 만듭니다.
   - Azure Cognitive Search의 벡터 검색 기능을 사용하여 가장 적합한 답변을 제공합니다.
   - 사용자가 시스템과 상호 작용할 때 필요에 따라 벡터를 생성합니다.
   - [LangChain](https://langchain.readthedocs.io/en/latest/)을 사용하여 Azure OpenAI와 상호 작용하고 프롬프트를 구성하여 에이전트를 만들기 위한 래퍼로 사용합니다.
   - 다국어를 지원합니다.
   - 멀티 인덱스를 지원합니다. 
   - CSV 파일 및 SQL Flavor 데이터베이스를 사용한 표 형태의 데이터를 지원합니다.
   - [Azure AI Document Intelligence SDK (former Form Recognizer)](https://learn.microsoft.com/en-us/azure/ai-services/document-intelligence/overview?view=doc-intel-3.0.0)를 사용하여 복잡하고 규모가 큰 PDF 문서를 파싱할 수 있습니다.
   - [Bing Search API](https://www.microsoft.com/en-us/bing/apis)를 사용하여 인터넷 검색 및 공개 웹사이트 질의응답을 지원합니다.
   - CosmosDB를 영구 메모리로 사용하여 사용자의 대화를 저장합니다.
   - [Streamlit](https://streamlit.io/)을 사용하여 Frontend 웹 애플리케이션을 python으로 구축합니다.
   

---

## **Hands-On-Lab의 실행단계**

Note: (Pre-requisite) You need to have an Azure OpenAI service already created

1. Fork this repo to your Github account.
2. Azure OpenAI 스튜디오에서 다음 모델을 배포합니다. ** 배포 이름이 모델 이름과 동일한지 확인합니다.**
   - "gpt-35-turbo"
   - "gpt-35-turbo-16k"
   - "gpt-4"
   - "gpt-4-32k"
   - "text-embedding-ada-002"
3. Create a Resource Group where all the assets of this accelerator are going to be. Azure OpenAI can be in different RG or a different Subscription.
4. Hands-on-Lab 실행에 필요한 Azure Infrastructure(Azure Cognitive Search, Cognitive Services 등)를 모두 만들려면 아래를 클릭하십시오:

[![Deploy To Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fpablomarin%2FGPT-Azure-Search-Engine%2Fmain%2Fazuredeploy.json) 

**Note**: 만약 사용자의 구독에 'Cognitive services multi-Service account'가 없는 경우, 수동으로 azure portal에 만들고 Responsible AI를 읽고 동의하시기 바랍니다.

5. Clone your Forked repo to your AML Compute Instance. If your repo is private, see below in Troubleshooting section how to clone a private repo.

6. **Python 3.10 conda 환경**에서 코드를 실행해야 합니다. 
7. Conda 환경을 새로 만들고, activate 시켜줍니다. 그런 다음 conda 환경에 필수 라이브러리들을 설치시켜 줍니다. 
```
conda create -n <환경명> python=3.10
conda activate <환경명>
pip install -r ./common/requirements.txt
```
설치할때, 일부 라이브러리가 dependancies 오류를 보여줄 수도 있습니다. 하지만 오류에 관계없이 올바르게 설치되었으니 안심하시고 넘어가시면 됩니다. 

8. 4단계에서 생성된 Azure Infrastructure 서비스들을 사용하여 'credentials.env'파일을 사용자 자신의 값으로 편집합니다.
9. **노트북을 순서대로 실행합니다**. 노트북은 서로 상호작용을 하기 때문에 꼭 순서대로 실행을 해주세요. 

