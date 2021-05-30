# Logic-App-sends-email-with-attachment-from-storage-account

## Scenario ##

Here we will be crating azure logic application with http trigger which gets blobs from storage account and add it as attachment in email and send it to recipients.
We can integrate Logic app directly when bob is added into storage account also. But in this article we are using HTTP trigger so that Logic app can be invoked through http request and pass other parameters also required to be send in email along with attachment. 

Lets see how we can start.

Go to Azure Logic App > click on create > select subscription > select RG > Provide Logic App Name > LASendsEmail > select Region > Next Tags > Review and Create
Go to Storage Account and create container as email and upload sample doccument
Once login App is created > Logic App home page will appear. Choose start with common trigger and select when a http request is recieved.
 
 ![image](https://user-images.githubusercontent.com/52846897/120091768-cbb01080-c12b-11eb-924c-28003524978f.png)
 
 You will get below screen and click on Use sample payload to generate schema
 
 ![image](https://user-images.githubusercontent.com/52846897/120091841-3a8d6980-c12c-11eb-8d32-46d67dd42525.png)
 
 Please add below data
 ~~~
 {
     "Description": {
            "type": "string"
        },
        "From": {
            "type": "string"
        },
        "Name": {
            "type": "string"
        },
        "RootFolderPath": {
            "type": "string"
        },
        "Title": {
            "type": "string"
        },
        "To": {
            "type": "string"
        },
        "Type": {
            "type": "string"
        },
        "isFilesAttached": {
            "type": "string"
        }
}
 
~~~

 
 

