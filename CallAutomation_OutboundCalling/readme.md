﻿|page_type|languages|products
|---|---|---|
|sample|<table><tr><td>csharp</tr></td></table>|<table><tr><td>azure</td><td>azure-communication-services</td></tr></table>|

# Call Automation - Quick Start

In this quickstart, we cover how you can use Call Automation SDK to make an outbound call to a phone number and use the newly announced integration with Azure AI services to play dynamic prompts to participants using Text-to-Speech and recognize user voice input through Speech-to-Text to drive business logic in your application.

# Design

![design](./data/OutboundCallDesign.png)

## Prerequisites

- Create an Azure account with an active subscription. For details, see [Create an account for free](https://azure.microsoft.com/free/)
- Create an Azure Communication Services resource. For details, see [Create an Azure Communication Resource](https://docs.microsoft.com/azure/communication-services/quickstarts/create-communication-resource). You will need to record your resource **connection string** for this sample.
- Get a phone number for your new Azure Communication Services resource. For details, see [Get a phone number](https://learn.microsoft.com/en-us/azure/communication-services/quickstarts/telephony/get-phone-number?tabs=windows&pivots=programming-language-csharp)
- Create Azure AI Multi Service resource. For details, see [Create an Azure AI Multi service](https://learn.microsoft.com/en-us/azure/cognitive-services/cognitive-services-apis-create-account).
- Connect your Azure Communication Services resource to the Azure AI Multi Service resource. For detals, see [Connect Azure Communication Services to Azure AI services](https://learn.microsoft.com/en-us/azure/communication-services/concepts/call-automation/azure-communication-services-azure-cognitive-services-integration)
- Create and host a Azure Dev Tunnel. Instructions [here](https://learn.microsoft.com/en-us/azure/developer/dev-tunnels/get-started)
- [.NET7 Framework](https://dotnet.microsoft.com/en-us/download/dotnet/7.0) (Make sure to install version that corresponds with your visual studio instance, 32 vs 64 bit)
- (Optional) A Microsoft Teams user with a phone license that is `voice` enabled. Teams phone license is required to add Teams users to the call. Learn more about Teams licenses [here](https://www.microsoft.com/microsoft-teams/compare-microsoft-teams-bundle-options).  Learn about enabling phone system with `voice` [here](https://learn.microsoft.com/microsoftteams/setting-up-your-phone-system).   You also need to complete the prerequisite step [Authorization for your Azure Communication Services Resource](https://learn.microsoft.com/azure/communication-services/how-tos/call-automation/teams-interop-call-automation?pivots=programming-language-javascript#step-1-authorization-for-your-azure-communication-services-resource-to-enable-calling-to-microsoft-teams-users) to enable calling to Microsoft Teams users.
- 
## Before running the sample for the first time

1. Open an instance of PowerShell, Windows Terminal, Command Prompt or equivalent and navigate to the directory that you would like to clone the sample to.
2. git clone `https://github.com/Azure-Samples/Communication-Services-dotnet-quickstarts.git`.
3. Navigate to `CallAutomation_OutboundCalling` folder and open `CallAutomation_OutboundCalling.sln` file.

### Setup and host your Azure DevTunnel

[Azure DevTunnels](https://learn.microsoft.com/en-us/azure/developer/dev-tunnels/overview) is an Azure service that enables you to share local web services hosted on the internet. Use the commands below to connect your local development environment to the public internet. This creates a tunnel with a persistent endpoint URL and which allows anonymous access. We will then use this endpoint to notify your application of calling events from the ACS Call Automation service.

```bash
devtunnel create --allow-anonymous
devtunnel port create -p 8080
devtunnel host
```
### Configuring application

Open the Program.cs file to configure the following settings

1. `acsConnectionString`: Azure Communication Service resource's connection string.
2. `acsPhonenumber`: Phone number associated with the Azure Communication Service resource. For e.g. "+1425XXXAAAA"
3. `targetPhonenumber`: Target phone number to add in the call. For e.g. "+1425XXXAAAA"
4. `callbackUriHost`: Base url of the app. (For local development replace the dev tunnel url)
5. `cognitiveServiceEndpoint`: Cognitive Service Endpoint
6. `targetTeamsUserId`: (Optional) Update field with the Microsoft Teams user Id you would like to add to the call. See [Use Graph API to get Teams user Id](../../../how-tos/call-automation/teams-interop-call-automation.md#step-2-use-the-graph-api-to-get-microsoft-entra-object-id-for-teams-users-and-optionally-check-their-presence).  Uncomment the following code snippet
   ```
   await callConnection.AddParticipantAsync(
    new CallInvite(new MicrosoftTeamsUserIdentifier(targetTeamsUserId))
    {
        SourceDisplayName = "Jack (Contoso Tech Support)"
    });
   ```

### Run app locally

1. Run the `CallAutomation_OutboundCalling` project with `dotnet run`
2. Open `http://localhost:8080/swagger/index.html` or your dev tunnel url in browser
3. To initiate the call, from the swagger ui execute the `/outboundCall` endpoint or make a Http post request to `<callbackUriHost>/outboundCall`
