### Introduction

The application you will work with in this lab depends on OAuth being configured. OAuth (Open Authorization) is an open standard that allows a user to grant access to their details on a third-party application with Google sign-in feature, without having to expose their credentials to  the application.

For example, when a user signs up using Google, upon signing up,  Google responds with an access token that is stored with Google and your application. Whenever the user tries to log in to the application, a  request is made to Google along with the access token to verify the  authenticity and validity of the token, upon verification, Google  responds with the requested data. Google never sends the user's details  back to the application. It only sends the details that are specified in the scopes.

In this lab step, you will be creating an OAuth Client ID in the Google Cloud console.

### Instructions

1. In the Google Cloud console, click the **hamburger** icon on the top left corner of the window and navigate to **APIs and Services** under **All Products**.

2. In the left sidebar, click on **OAuth Consent Screen**.

3. Select the user type as **Internal** and click on **Create**.

4. Enter the app name *api-app*.

5. From the drop-down, select the **user support email** that is available to you.

6. Keep the other options as is until **Authorised Domains**.

7. Under **Authorised domains**, click on **Add Domain**.

8. Enter the domain as *cloudacademylabs.com*.

9. Enter the developer email as *student-O31JZ@cloudacademylabs.com*.

10. Click on **Save and Continue**.

    You don't need to continue with the wizard for this lab.

11. In the left sidebar, click **Credentials**.

12. At the top of the screen, click **Create Credentials**.

13. From the drop-down, choose **OAuth client ID**.

14. From the **Application Type** drop-down, choose **Web Application**:

    ![alt](https://assets.cloudacademy.com/labs/uploads/understanding-difference-between-cloud-endpoints-and-api-gateway/steps/1_creating-a-client-application-for-google-authentication/assets/credentials.png)

15. Enter the **Name** as *Web Client*.

16. Under section **Authorized JavaScript origins**, click on **Add URI**.

17. In the input box, enter *http://35.224.223.237.nip.io:8080*

18. Click on **Create**.

19. When prompted, copy your **client ID** and keep a note of it to be used later in the lab.

20. Click on **OK**.



## Introduction

App Engine is a PAAS-based service that allows you to create an  application once, then you just focus on business logic and the rest of  the infrastructure scaling will be taken care of by Google. There is no  need for maintenance of activities related to infrastructure.

In this lab step, you will be creating a python application that  serves an HTML index web page, and later you will be deploying that  application on App Engine.

## Instructions

1. Navigate to the Compute Engine by entering *compute* in the search bar and clicking the **Compute Engine** result:

   ![alt](https://assets.cloudacademy.com/bakery/media/uploads/content_engine/image-20220405173453-3-74e51282-90ce-49c6-a1ac-0fa8d53092f8.png)

   You are taken to the VM instances table and the **ca-lab-vm** instance created by the Cloud Academy lab environment is listed.

   ![alt](https://assets.cloudacademy.com/bakery/media/uploads/content_engine/image-20220316105840-1-58064011-d5fc-4096-80dc-c106ab323135.png)

    

2. Click the down arrow in the **Connect** section and click on **Open in browser window**:

   ![alt](https://assets.cloudacademy.com/bakery/media/uploads/content_engine/Screen Shot 2021-12-16 at 5.28.00 PM-27582a77-3506-433f-b4ce-9bd525d3af86.png)

   Alternatively, you can directly click **SSH**.

   Progress is then reported:

   ![alt](https://assets.cloudacademy.com/bakery/media/uploads/content_engine/image-20220526094413-1-983218b5-372e-41bd-9342-26b94add4f78.png)

   GCP automatically creates SSH keys for you and transfers them to the VM. The keys also automatically expire after several hours.

   ![alt](https://assets.cloudacademy.com/bakery/media/uploads/content_engine/image-20220328114024-1-e88fd3aa-a4e0-4615-80a4-e9508ec302e2.png)

3. To set the default App Engine Path and restart the Terminal, use the following commands:

```sh
echo "export ENDPOINTS_GAE_SDK=/usr/lib/google-cloud-sdk/platform/google_appengine" >> ~/.bashrc
source ~/.bashrc
```

```sh
gcloud app create --region=us-central
```

This command will create an App Engine application in the assigned  project. You will deploy the application later in this lab step.

Paste the following command and press enter:

```sh
git clone https://github.com/cloudacademy/example-iap
```

This command will clone the **example-iap** repository that has a simple web application that only needs a bit of configuration before it can be deployed.

The main agenda of this lab is to show how to secure your applications deployed on App Engine using Cloud IAP.

To navigate to the project root directory:

```sh
cd example-iap
```

To set the project id variable:

```sh
project_id=$(gcloud config list --format 'value(core.project)')
```

To deploy the application on App Engine, enter the following command and press *y* when prompted **Do you want to continue (Y/n)?**:

1. ```sh
   gcloud app deploy
   ```

   This command will deploy the application and split the traffic to  the latest version. At this point, only one version is there as the  application was just created in the last step. In further lab steps, a  new version will be deployed.

### Summary

In this lab step, you have created a python application having a  simple message that will be printed only if you are an authenticated  user. Lastly, you have deployed the application on App Engine.



## Introduction

Cloud IAP can be used with applications deployed on Compute Engine, App Engine, Kubernetes Engine, or on-premises apps.

In this lab step, you will be configuring Cloud IAP for the  application deployed on App Engine and then you will add your assigned  email to the IAM Principal in the list of allowed users.

## Instructions

1. Search for *iap* in the Google Cloud console search bar and click the **Identity-Aware Proxy** search result:

1. In the left sidebar, click on **Identity-Aware Proxy**.

2. If prompted, click on **Go To Identity-Aware Proxy**.

3. Toggle the **IAP** slider shown to the left of **App Engine app**:

   ![alt](https://assets.cloudacademy.com/bakery/media/uploads/blobid4-af3d0e1d-6d6d-4023-9433-aae38458aa7e.png)

4. When prompted to confirm, click **Turn On**.

5. Once enabled, select the **App Engine app**.

   Refer to the below image:

   ![alt](https://assets.cloudacademy.com/bakery/media/uploads/blobid5-a129dcee-91a6-4661-b97e-68da7f54a5e0.png)

6. In the right sidebar, click on **Add Principal**.

7. In the **New principals** input box, enter *student-O31JZ@cloudacademylabs.com*.

8. In the roles drop-down, choose the **IAP-secured Web App User** role under the Cloud IAP role before clicking **Save**.

   Refer to the below image:

   ![alt](https://assets.cloudacademy.com/labs/uploads/cloud-iap-lab/steps/4_configuring-the-cloud-iap-and-testing-the-application/assets/role-select.png)

9. Return to your browser shell, enter the following command and click the URL that is output:

1. ```
   echo "http://$project_id.uc.r.appspot.com"
   ```

   The URL will resemble the following:
   	
   	![alt](https://assets.cloudacademy.com/bakery/media/uploads/blobid1-06bc328b-60e0-409f-bb12-45d1061b2919.png)
   	
    The above URL will load the web application but because you have  enabled Cloud IAP, a Google Sign In page will load first. If you are  using an incognito/private mode browser or cookie blocker you will need  to allow cookies for this site. This is a requirement for authentication with the Google identity provider.
   	*Note*: It may take a minute for the login to be successful. Refresh the page in 30s intervals. 

   <img src="./assets/%E3%82%B9%E3%82%AF%E3%83%AA%E3%83%BC%E3%83%B3%E3%82%B7%E3%83%A7%E3%83%83%E3%83%88%202023-08-18%2013.45.15.png" alt="スクリーンショット 2023-08-18 13.45.15" style="zoom:50%;" />

2. To log in, use the credentials provided for the lab's student user.

   On successful login, it will redirect you back to the Web Application.

3. You will see a **Welcome** and **Login Successful** message.

### Summary

In this lab step, you have enabled the Cloud IAP for your App Engine  application that will function as a central authorization layer.