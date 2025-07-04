:_mod-docs-content-type: REFERENCE
[id="example-service-endpoint-authentication-settings_{context}"]
# Example service endpoint authentication settings

You can configure authentication for service endpoints for user-defined project monitoring by using `ServiceMonitor` and `PodMonitor` custom resource definitions (CRDs). 

The following samples show different authentication settings for a `ServiceMonitor` resource.
Each sample shows how to configure a corresponding `Secret` object that contains authentication credentials and other relevant settings.

## Sample YAML authentication with a bearer token

The following sample shows bearer token settings for a `Secret` object named `example-bearer-auth` in the `ns1` namespace:

.Example bearer token secret

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: example-bearer-auth
  namespace: ns1
stringData:
  token: <authentication_token> #<1>

```
<1> Specify an authentication token.

The following sample shows bearer token authentication settings for a `ServiceMonitor` CRD. The example uses a `Secret` object named `example-bearer-auth`:

[id="sample-yaml-bearer-token_{context}"]
.Example bearer token authentication settings

```yaml
apiVersion: monitoring.coreos.com/v1
kind: ServiceMonitor
metadata:
  name: prometheus-example-monitor
  namespace: ns1 
spec:
  endpoints: 
  - authorization:
      credentials:
        key: token #<1>
        name: example-bearer-auth #<2>
    port: web
  selector: 
    matchLabels:
      app: prometheus-example-app

```
<1> The key that contains the authentication token in the specified `Secret` object.
<2> The name of the `Secret` object that contains the authentication credentials.

# [IMPORTANT]
# Do not use `bearerTokenFile` to configure bearer token. If you use the `bearerTokenFile` configuration, the `ServiceMonitor` resource is rejected.

[id="sample-yaml-basic-auth_{context}"]
## Sample YAML for Basic authentication

The following sample shows Basic authentication settings for a `Secret` object named `example-basic-auth` in the `ns1` namespace:

.Example Basic authentication secret

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: example-basic-auth
  namespace: ns1
stringData:
  user: <basic_username> #<1>
  password: <basic_password>  #<2>

```
<1> Specify a username for authentication.
<2> Specify a password for authentication.

The following sample shows Basic authentication settings for a `ServiceMonitor` CRD. The example uses a `Secret` object named `example-basic-auth`:

.Example Basic authentication settings

```yaml
apiVersion: monitoring.coreos.com/v1
kind: ServiceMonitor
metadata:
  name: prometheus-example-monitor
  namespace: ns1 
spec:
  endpoints: 
  - basicAuth:
      username:
        key: user #<1>
        name: example-basic-auth #<2>
      password:
        key: password #<3>
        name: example-basic-auth #<2>
    port: web
  selector: 
    matchLabels:
      app: prometheus-example-app

```
<1> The key that contains the username in the specified `Secret` object.
<2> The name of the `Secret` object that contains the Basic authentication.
<3> The key that contains the password in the specified `Secret` object.

[id="sample-yaml-oauth-20_{context}"]
## Sample YAML authentication with OAuth 2.0

The following sample shows OAuth 2.0 settings for a `Secret` object named `example-oauth2` in the `ns1` namespace:

.Example OAuth 2.0 secret

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: example-oauth2
  namespace: ns1
stringData:
  id: <oauth2_id> #<1>
  secret: <oauth2_secret> #<2>

```
<1> Specify an Oauth 2.0 ID.
<2> Specify an Oauth 2.0 secret.

The following sample shows OAuth 2.0 authentication settings for a `ServiceMonitor` CRD. The example uses a `Secret` object named `example-oauth2`:

.Example OAuth 2.0 authentication settings

```yaml
apiVersion: monitoring.coreos.com/v1
kind: ServiceMonitor
metadata:
  name: prometheus-example-monitor
  namespace: ns1 
spec:
  endpoints: 
  - oauth2:
      clientId: 
        secret:
          key: id #<1>
          name: example-oauth2 #<2>
      clientSecret:
        key: secret #<3>
        name: example-oauth2 #<2>
      tokenUrl: https://example.com/oauth2/token #<4>
    port: web
  selector: 
    matchLabels:
      app: prometheus-example-app

```
<1> The key that contains the OAuth 2.0 ID in the specified `Secret` object.
<2> The name of the `Secret` object that contains the OAuth 2.0 credentials.
<3> The key that contains the OAuth 2.0 secret in the specified `Secret` object.
<4> The URL used to fetch a token with the specified `clientId` and `clientSecret`.
