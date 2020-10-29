# PowerAutomateUIPathAPI
This is an automation that runs from power automate and calls UIPath REST API


1. [Sharepoint] When a file is created in a folder

Site Address- Address of your sharepoint site
Folder Id- Folder in which you want the flow to start

2. [Sharepoint] Send an HTTP Request to Sharepoint // To get the number of files in the sharepoint (in my case, when the number of files is equal to 4, then trigger the flow to run, you might not need it)

Site Address
Method - GET
Uri - /_api/web/getfolderbyserverrelativeurl('/sites/PosAviation_eCommerce_Dashboard/Shared Documents/01 Order Sales Report')/ItemCount


3. Parse JSON //To read the variable from the HTTP request
Content - body
Schema - 
{
    "type": "object",
    "properties": {
        "d": {
            "type": "object",
            "properties": {
                "ItemCount": {
                    "type": "integer"
                }
            }
        }
    }
}


4.HTTP - to authenticate the orchestrator
Method - POST
URI - https://account.uipath.com/oauth/token
Headers:
  Content-Type - application/json
  X-UIPATH-TenantName - RpaDefault
  
Body:

{
  "grant_type": "refresh_token",
  "client_id": "//Get from orchestrator API access",
  "refresh_token": "//Get from orchestrator API access"
}


5. Parse Json
Content- Body
Schema-
{
    "type": "object",
    "properties": {
        "access_token": {
            "type": "string"
        },
        "id_token": {
            "type": "string"
        },
        "scope": {
            "type": "string"
        },
        "expires_in": {
            "type": "integer"
        },
        "token_type": {
            "type": "string"
        }
    }
}

6. HTTP 2  // To run job start
Method - POST
URI -https://cloud.uipath.com/[TenantID]/[TenantName]/odata/Jobs/UiPath.Server.Configuration.OData.StartJobs
Headers:
  X-UIPATH-TenantName - [TenantName]
  Authorization - Bearer //Access Token
  Content-Type - application/json
  X-UIPATH-OrganizationUnitID - 182312
  
Body:

{
  "startInfo": {
    "ReleaseKey": "//Process Key",
    "Strategy": "Specific",
    "RobotIds": [
      //Robot ID
    ],
    "NoOfRobots": 0
  }
}


