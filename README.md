# Automatizar-DMC-googleAPI
Como usar la API de Google "DCM/DFA Reporting And Trafficking API"


Prerrequisitos: python2

Crear un proyecto en Google Cloud con el usuario con acceso a DCM

Habilitar "DCM/DFA Reporting And Trafficking API" en las APIs del proyecto asociado

Crear un OAuth 2.0 client ID desde APIs->Credentials->OAuth Client ID->Application Type->Other del proyecto asociado, descargando json con credenciales.

Instalar libraría python para eso de apis de google: pip2 install --upgrade google-api-python-client

En terminal python2 correr: con archivo "client_secrets.json" en directorio de trabajo, primera parte para conseguir el archivo 'auth-sample.dat'. Se necesita de una autorización vía navegador del usuario con acceso a DCM.
```
import argparse
import os
import sys
import io
from googleapiclient import discovery
import httplib2
from oauth2client import client
from oauth2client import file as oauthFile
from oauth2client import tools
from oauth2client.file import Storage
import dfareporting_utils
from googleapiclient import http
from google.cloud import storage


# Only the first time (with file 'client_secrets.json' in working folder):
OAUTH_SCOPES = ['https://www.googleapis.com/auth/dfareporting']
flow = client.flow_from_clientsecrets("client_secrets.json", scope=OAUTH_SCOPES)
CREDENTIAL_STORE_FILE = 'auth-sample.dat'
# Check whether credentials exist in the credential store. Using a credential
# store allows auth credentials to be cached, so they survive multiple runs
# of the application. This avoids prompting the user for authorization every
# time the access token expires, by remembering the refresh token.
storage = Storage(CREDENTIAL_STORE_FILE)
```

Continuar con el Resto del Código según necesidades.
Por ejemplo:
```
credentials = storage.get()
storage = Storage(CREDENTIAL_STORE_FILE)
credentials = storage.get()
# Use the credentials to authorize an httplib2.Http instance.
http = credentials.authorize(httplib2.Http())
# Construct a service object via the discovery service.
service = discovery.build('dfareporting', 'v3.2', http=http)
# Construct the request.
request = service.userProfiles().list()

# Get profile Id and user names:
response = request.execute()

for profile in response['items']:
   print('Found user profile with ID %s and user name "%s".' %
         (profile['profileId'], profile['userName']))

```
Muchos métodos están disponibles en https://github.com/googleads/googleads-dfa-reporting-samples/commit/3ec2fb453dd86e464e1b04ad9a2019b11d16ca5f
