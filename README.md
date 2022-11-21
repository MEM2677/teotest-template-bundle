# Digital Assistant

This bundle enables the Digital Assistant AIKA provided by Athlos.

## Introduction

This bundle installs a server side rendered widget called `Digital Assistant` to your Entando instance,
in a page called `Digital Assistant`.  
This page uses a new page template `Digital Assistant template` which, in turn, brings JQuery 3.3.1 (at the time of writing, the default JS needed for the widget to work).

### Widget configuration 

When the widget is dropped in frame, the system asks for a key given by Athlos; the widget installed by the bundle
is already configured with a default key so please leave it unchanged.

__NOTE:__ the default assistant has a very limited functionality.

## Prerequisites

 The only prerequisite is the customization of the CSP.  This is needed because the Digital Assistant widget connects and loads resources from an external
server and so Entando has no way to know in advance which domains are to be allowed for script execution.

__NOTE:__ the CSP policy shown below is very permissive to accommodate most browsers and is just an example, thus not suitable for production. 


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
        value: ' ''strict-dynamic'' https: ''self'' ; object-src ''none''; base-uri
          ''self''; script-src-attr  ''self'' ''unsafe-hashes'' ''unsafe-inline'';
          script-src-elm ''self''  ''unsafe-inline'' ''unsafe-eval'' '
  ```

- Restart the deploy  
``ent k scale deploy xxyyzz-deployment --replicas=1``
