# Connect to an external IdP

## Step 1: Configure the IdP

We will be using WSO2 Identity Server (WSO2 IS) as the IdP, and at the time of writing this document, the version in use is WSO2 IS 7.1.

Refer to the WSO2 Identity Server v7.1 <a href = "https://is.docs.wso2.com/en/latest/get-started/quick-set-up/">official documentation</a> to set up the server locally, then log in to the Identity Server Management Console.

First navigate to the **API Resources** section and add a new API resource.

<a href="{{base_path}}/assets/img/configurations/idp/create-new-api-resource.png"><img src="{{base_path}}/assets/img/configurations/idp/create-new-api-resource.png" alt="Create new API resource" width="60%" style="padding-top: 20px" ></a>

Add the **Identifier** and the **Display Name** and click on **Next**.

<a href="{{base_path}}/assets/img/configurations/idp/configure-api-resource.png"><img src="{{base_path}}/assets/img/configurations/idp/configure-api-resource.png" alt="Configure API resource" width="45%" style="padding-top: 20px" ></a>

Add the required scopes and click **Next**. 

**WSO2 Integrator: WebSubHub** uses the following scopes: *register_topic*, *deregister_topic*, *subscribe*, *unsubscribe*, and *content_update*. Ensure these are added before proceeding.

<a href="{{base_path}}/assets/img/configurations/idp/configure-scopes.png"><img src="{{base_path}}/assets/img/configurations/idp/configure-scopes.png" alt="Configure relevant scopes" width="45%" style="padding-top: 20px" ></a>

Once completed click on **Create**.

<a href="{{base_path}}/assets/img/configurations/idp/complete-api-resource-creation.png"><img src="{{base_path}}/assets/img/configurations/idp/complete-api-resource-creation.png" alt="Configure relevant scopes" width="45%" style="padding-top: 20px" ></a>

Then navigate to the **Applications** tab and create a new application.

<a href="{{base_path}}/assets/img/configurations/idp/create-new-app.png"><img src="{{base_path}}/assets/img/configurations/idp/create-new-app.png" alt="Create new application" width="60%" style="padding-top: 20px" ></a>

Select **M2M Application** from the options.

<a href="{{base_path}}/assets/img/configurations/idp/select-m2m-application.png"><img src="{{base_path}}/assets/img/configurations/idp/select-m2m-application.png" alt="Select M2M application" width="60%" style="padding-top: 20px" ></a>

Update the name and click on **Create**.

<a href="{{base_path}}/assets/img/configurations/idp/create-m2m-application.png"><img src="{{base_path}}/assets/img/configurations/idp/create-m2m-application.png" alt="Create M2M application" width="45%" style="padding-top: 20px" ></a>

Navigate to the **Protocol** section on the top navigation.

<a href="{{base_path}}/assets/img/configurations/idp/application-protocol.png"><img src="{{base_path}}/assets/img/configurations/idp/application-protocol.png" alt="Create M2M application" width="45%" style="padding-top: 20px" ></a>

Update the **Token Type** to *JWT* and add an **Audience** called *websubhub*  and click **Update**.

<a href="{{base_path}}/assets/img/configurations/idp/update-app-protocol-configurations.png"><img src="{{base_path}}/assets/img/configurations/idp/update-app-protocol-configurations.png" alt="Update application protocol configurations" width="45%" style="padding-top: 20px" ></a>

Then navigate to the **API Authorization** section on the top navigation and click on **+ Authorize an API Resource**.

<a href="{{base_path}}/assets/img/configurations/idp/add-authz-api-resource.png"><img src="{{base_path}}/assets/img/configurations/idp/add-authz-api-resource.png" alt="Add an authorized API resource" width="45%" style="padding-top: 20px" ></a>

Select the previously created *WSO2 WebSubHub* API resource from the drop down.

<a href="{{base_path}}/assets/img/configurations/idp/select-websubhub-api.png"><img src="{{base_path}}/assets/img/configurations/idp/select-websubhub-api.png" alt="Select WebSubHub API resource" width="45%" style="padding-top: 20px" ></a>

Click on **Select All** in the **Authorized Scopes** section and click on **Finish**.

<a href="{{base_path}}/assets/img/configurations/idp/select-websubhub-scopes.png"><img src="{{base_path}}/assets/img/configurations/idp/select-websubhub-scopes.png" alt="Select WebSubHub API scopes" width="45%" style="padding-top: 20px" ></a>

Now use the following cURL command to retrieve the access token from the WSO2 Identity server.

```sh
    $ curl -u <client-id>:<client-secret> \
        -d "grant_type=client_credentials&scope=<scopes>" \
        https://localhost:9443/oauth2/token -k
```

## Step 2: Configure the WSO2 Integrator: WebSubHub

### Configure WebSubHub

Add the configurations related to the WebSubHub authentication in the `conf/Config.toml` and restart the WebSubHub.

```toml
    [websubhub.config.server.auth]
    issuer = "https://localhost:9443/oauth2/token"
    audience = "websubhub"
    signature.url = "https://localhost:9443/oauth2/jwks"
    signature.secureSocket.disable = true
```

## Step 3: Invoke WSO2 Integrator: WebSubHub operations

Use the following cURL command to retrieve an access token from the WSO2 Identity Server. In this example, the token is requested with the *register_topic* scope.

```sh
    $ curl -u <client-id>:<client-secret> \
        -d "grant_type=client_credentials&scope=register_topic" \
        https://localhost:9443/oauth2/token -k
```

Use the access token obtained from the above cURL command to create a new topic.

```sh
    $ curl -X POST 'https://localhost:<websubhub-port>/hub' \
        -H 'Content-Type: application/x-www-form-urlencoded' \
        -H 'Authorization: Bearer <access_token>' \
        -d 'hub.mode=register&hub.topic=<topic-name>' -k
```

