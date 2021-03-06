
### The below steps describe about how to retrieve Kuberenetes resources using Curl command

Begin by reviewing the kubectl configuration file. We will use the three certificates and the API server address.

```
student@lfs458-node-1a0a:~$ less ~/.kube/config

    <output_omitted>
```
We will set the certificates as variables. You may want to double-check each parameter as you set it. Begin with setting
the client-certificate-data key.

```
student@lfs458-node-1a0a:~$ export client=$(grep client-cert ~/.kube/config |cut -d" " -f 6)
student@lfs458-node-1a0a:~$ echo $client
LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUM4akNDQWRxZ0F3SUJ
2728
CHAPTER 5. APIS AND ACCESS
BZ0lJRy9wbC9rWEpNdmd3RFFZSktvWklodmNOQVFFTEJRQXdGVEVUTUJFR0
ExVUUKQXhNS2EzVmlaWEp1WlhSbGN6QWVGdzB4TnpFeU1UTXhOelEyTXpKY
UZ3MHhPREV5TVRNeE56UTJNelJhTURReApGekFWQmdOVkJBb1REbk41YzNS
<output_omitted>

```
Almost the same command, but this time collect the client-key-data as the key variable.

```
student@lfs458-node-1a0a:~$ export key=$(grep client-key-data ~/.kube/config |cut -d " " -f 6)
student@lfs458-node-1a0a:~$ echo $key
<output_omitted>

```
Finally set the auth variable with the certificate-authority-data key.

```
student@lfs458-node-1a0a:~$ export auth=$(grep certificate-authority-data ~/.kube/config |cut -d " " -f 6)
student@lfs458-node-1a0a:~$ echo $auth
<output_omitted>

```
Now encode the keys for use with curl.
```
student@lfs458-node-1a0a:~$ echo $client | base64 -d - > ./client.pem
student@lfs458-node-1a0a:~$ echo $key | base64 -d - > ./client-key.pem
student@lfs458-node-1a0a:~$ echo $auth | base64 -d - > ./ca.pem
```
Pull the API server URL from the config file.
```
student@lfs458-node-1a0a:~$ kubectl config view |grep server
server: https://10.128.0.3:6443
```
Use curl command and the encoded keys to connect to the API server.
```
student@lfs458-node-1a0a:~$ curl --cert ./client.pem \
--key ./client-key.pem \
--cacert ./ca.pem \
https://10.128.0.3:6443/api/v1/pods
{
"kind": "PodList",
"apiVersion": "v1",
"metadata": {
"selfLink": "/api/v1/pods",
"resourceVersion": "239414"
},
<output_omitted>

```
