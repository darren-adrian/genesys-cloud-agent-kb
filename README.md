# genesys-cloud-agent-kb

This is designed as an example on how you can build out your own realtime Agent Assist example using Genesys Cloud native "Support Center". For the media type in this example I'm using WebMessaging, You should already have WebMessaging configured and routing to your agent. The example is also using [Spark](https://spark.genesys.com/7978beca0/p/31f4cc-spark) for rendering the CSS elements, this is a Genesys created CSS set to make it easy to leverage the Genesys Cloud UI elements like buttons etc. To keep the JavaScript simple I'm using the Genesys Cloud [JS SDK.](https://developer.genesys.cloud/api/rest/client-libraries/javascript/)

The Example HTML page is designed to work with your existing Genesys Cloud Support Center which is accessed through the WebMessenger DeploymentId "gc_deploymentId" you need to of already set this up in Genesys as well as the "documents" this readMe is NOT going to cover these setup items. NOTE you can get the deploymentId from the WebMessenger Widget you have setup with Support Center already enabled.

In realtime the conversation gets sent to the Knowledge Base to see if there is any matching responses. If there are they are then rendered to the agent with the option for the agent to send the article as a text String into the WebMessaging conversation.

![](/docs/images/screenShot1.png?raw=true)

This example uses "Implicit Grant" OAuth2 to your Genesys Cloud ORG environment. The first thing you will need to do is to get a ClientID from your Genesys Cloud ORG with the ability to connect to the API endpoints. This will also need to have a redirect to the Hosting location of the server url for example: "http://localhost:3333/kb.html". You will also need add the required "scopes" for the endpoints that you want to use. In this example we are using "Streaming-events", "knowledge", "users" & "conversations".

![](/docs/images/screenShot2.png?raw=true)

Once you have Created your OAuth you will need to create a "Group" to enable the users that need to have access to this new menu option visible. I would also set the visibility to be for members only. 

![](/docs/images/screenShot3.png?raw=true)

Now create the "Interaction Widget" Integration and under the configuration set the URL to the full URL including the parameters that are required from above: "clientId", "redirectURL", "region" and "gc_deploymentId". Also select the "Communication Type Filtering" to webmessaging, to ensure it only runs on this media type.

```
http://localhost:3333/kb.html?gc_region=mypurecloud.com.au&gc_clientId=ENTER_YOUR_ID&gc_redirectUrl=http://localhost:3333/kb.html&gc_deplymentId=ENTER_YOUR_ID
```

![](/docs/images/screenShot4.png?raw=true)

Under the "Advanced" section of the integration object you can optional set the "Icon" of the button that will appear in the agent UI. In my example im using the KB_Icon.svg that I have included in this repo. To set this put it as below but use your own hosting location of course.

```
{
  "monochromicIcon": {
    "vector": "https://localhost:3333/KB_Icon.svg"
  }
}
```

![](/docs/images/screenShot5.png?raw=true)

This example has been created with only 1x WebMessaging interaction on the agent desktop at one time if a user has a higher Utilization then you will need to add in controls for that into this example.

**NOTE:** If you create a KVP in the "localStorage" of your browser called "gc_debug" with the value of "true" this will enable extra debug logging to appear in the browser console log. This can be handy for testing. 

# Support Center

Ensure that you have Support Center enabled on your WebMessenger DeploymentId

![](/docs/images/screenShot6.png?raw=true)

Also the Articles are now going to be built out in the newer V2 Knowledge Interface. NOTE that currently in this latest commit I have built in support for the newer Knowledge Interface but only for Text and in the first "Paragraph". Future commits I will plan to add support for the array object and do a forEach and display images and videos etc. If you feel like it you can add the change a push a commit 👍

![](/docs/images/screenShot7.png?raw=true)
