# Digital Assistant

This bundle enables the Digital Assistant AIKA provided by Athlos.

## Introduction

This bundle installs a server side rendered widget called `Digital Assistant` to your Entando instance,
in a page called `Digital Assistant` (code: `da_test`).  
This page uses a new page template `Digital Assistant template` which, in turn, brings JQuery 3.3.1 (at the time of writing, the default JS needed for the widget to work).

### Widget configuration 

When the widget is dropped in frame, the system asks for a key given by Athlos; the widget installed by the bundle
is already configured with a default key so please leave it unchanged.

__NOTE:__ the default assistant has a very limited set of skill.

## Prerequisites

 The only prerequisite is the customization of the CSP.  This is needed because the Digital Assistant widget connects and load resources from an external
server and so Entando has no way to know in advance which are the possible permission for inline script execution.

__NOTE:__ the CSP policy shown below is just an example and are subject to change without notice. 


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
