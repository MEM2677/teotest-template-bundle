# Digital Assistant

This bundle enables the Digital Assistant AIKA provided by Athlos 

## Introduction

This bundle installs a server side rendered widget called `Virtual Assistant` to your Entando instance.
This widget directly communicate with Athlos server for execution.

### Widget configuration 

When the widget is installed in a frame the system asks for a key given by Athlos when a subscription is made.
It's possible to save the form leaving the key blank, in this case the default assistant, with limited functionalities, will be used.

## Prerequisites

 - Entando 7.0+ instance with JQuery 3.3.1
 - CSP setup

## CSP customization
 
 - Find the deployment of the de-app with the following command  
 ``ent k get deploy``

 - Scale the deploy to zero  
 ``ent k scale deploy xxyyzz-deployment --replicas=0`` where xyz is the name of the Entando application

 - edit the deployment
 ``ent k edit deploy xxyyzz-deployment``  

- add the snipped below in the bottom of the list of the environment variables found in the deployment

   ```text
   - name: CSP_HEADER_ENABLED
   value: "true"
   - name: CSP_HEADER_EXTRACONFIG
   value: ' ''unsafe-eval''  ''unsafe-hashes''  https: ''self'' https://dev.athlos.it
   https://sdkathlos.it https://aka.ms https://sdk.amazonaws.com https://csspeechstorage.blob.core.windows.net;
   connect-src ''unsafe-eval'' ''self'' https://dev.athlos.it https://sdkathlos.it
   https://*.amazonaws.com/ https://aka.ms https://sdk.amazonaws.com https://csspeechstorage.blob.core.windows.net
   https://api.mymemory.translated.net blob: ; base-uri ''self''; script-src-attr
   ''self'' ''unsafe-hashes'' ''unsafe-inline''  https://dev.athlos.it https://sdkathlos.it
   https://aka.ms https://sdk.amazonaws.com https://csspeechstorage.blob.core.windows.net  '
   ```
  
   This is needed because Entando does not know in advance all the JS scripts remotely leaded from the Athlos main server.
 - Restart the deploy  
 ``ent k scale deploy xxyyzz-deployment --replicas=1``