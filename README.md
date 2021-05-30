# Logic-App-sends-email-with-attachment-from-storage-account

## Scenario ##

Here we will be crating azure logic application with http trigger which gets blobs from storage account and add it as attachment in email and send it to recipients.
We can integrate Logic app directly when bob is added into storage account also. But in this article we are using HTTP trigger so that Logic app can be invoked through http request and pass other parameters also required to be send in email along with attachment. 

Lets see how we can start.

Go to Azure Logic App > click on create > select subscription > select RG > Provide Logic App Name > LASendsEmail > select Region > Next Tags > Review and Create
Go to Storage Account and create container as email and upload sample doccument
Once login App is created > Logic App home page will appear. Choose start with common trigger and select when a http request is recieved.
 
 ![image](https://user-images.githubusercontent.com/52846897/120091768-cbb01080-c12b-11eb-924c-28003524978f.png)
 
 Please add below code in Request Body JSON Schema
 ~~~
 {
    "properties": {
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
    },
    "type": "object"
}
~~~

Click on Save
Now click on +New Step and type variable and you will see {X} variables below. Please select this and in next window in actions select Initialize variable

![image](https://user-images.githubusercontent.com/52846897/120092691-9eb32c00-c132-11eb-857d-8b0db76af890.png)

Please give name as attachments and Type as array and click on save in left top. 

Now we will check request body If there are any attachments present. So we will add boolean condition as below.

click on Add step > search for Control > Select Control > Under Actions select Condition. Once condition is selected you will get screen as below.

![image](https://user-images.githubusercontent.com/52846897/120092779-3b75c980-c133-11eb-9717-320065f8b002.png)

click on Add an action inside True Block > search for List blobs > Under actions select > List blobs

![image](https://user-images.githubusercontent.com/52846897/120092968-8f34e280-c134-11eb-9b16-6357b79c3518.png)

You can manually select storage account or If already any storage account available then It will be listed here. In our case we have already created storage account named azure204 and select it and enter connection name and click on create.

Once you click on create It will ask to specify folder. In right side under dynamic content select rootfolderPath

![image](https://user-images.githubusercontent.com/52846897/120093054-344fbb00-c135-11eb-9989-76ba9ecbb928.png)

click on save.

Next click on add an action and search for control and select foreach.
You will get select an output from previous step and click on it. In right side you will get dynamic content and select value

![image](https://user-images.githubusercontent.com/52846897/120093135-d96a9380-c135-11eb-9c4e-0d1eade058f8.png)

Next click on add action inside foreach activity

seatch for blob and select azure blob storage and under actions select get blob content

Right side under dynamic content you will find Path and click on it. select infer content type as yes

![image](https://user-images.githubusercontent.com/52846897/120093201-45e59280-c136-11eb-9878-a08fd3794aba.png)

Next click on add action inside foreach activity

search for variable and under actions click on append to array variables

In name please select attachments from dropdown and in value please enter 
```
{
  "ContentBytes": @{body('Get_blob_content')},
  "Name": @{items('For_each')?['DisplayName']}
}
```
![image](https://user-images.githubusercontent.com/52846897/120093331-200cbd80-c137-11eb-85b4-64824ada6cae.png)

Click on add action outside foreach activity

search for office 365 outlook and under actions select send an email.

It will ask you to sign in to office 365. Once you sign in It will show To subject body field.

Once you click on To in right side under dynamic content select more in when http request recieved and select To. Repeat same steps for Subject and Body also.
for subject please select Ttile and Body please select description.
Now click on add parameter and select attachments
and click on square symbol as shown below

![image](https://user-images.githubusercontent.com/52846897/120093483-441cce80-c138-11eb-898c-e47a8c9aadcb.png)

now click on attachments and in dynamic content select attachments

![image](https://user-images.githubusercontent.com/52846897/120093513-6ca4c880-c138-11eb-9ef8-ecb5b7901847.png)

click on save.

Now we will run using postman. 

Now click on When http recieved request is recieved and copy HTTP POST URL and pass below parameters
~~~
{
    "Description" : "sample attachment",
    "From": "sample user name",
    "Name" : "Name",
    "RootFolderPath": "containername/directoryname",
    "Title": "sample tile",
    "To" : "to email id",
    "Type" : "request type",
    "isFilesAttached" : "true"
}
~~~


















 
 

