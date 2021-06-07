# kubeless

## Deployment

### Example Function

```js
module.exports = {
    hello: async function (event, context) {
        console.log("event")
        console.log(event.extensions.request.query)
        return "hello"
    }
}
```

```bash 
kubeless function deploy <funcName> --runtime nodejs14 \
                                --dependencies package.json \
                                --handler <fileName>.<function> \
                                --from-file <fileName>.js
                                
```

HttpTrigger using Traefik (this will create an ingress)
```yaml
apiVersion: kubeless.io/v1beta1
kind: HTTPTrigger
metadata:
  name: hello
  annotations:
    cert-manager.io/cluster-issuer: letsencrypt-prod
spec:
 function-name: hello
 host-name: kubeless.dfinkbeiner.de
 path: hello
 gateway: traefik
 tls-secret: hello-tls
```

## Use Cases


### Telegram Bot


1. Use Bot Father to create a new bot 
2. set webhook via 

```
POST https://api.telegram.org/bot<token>/setWebhook
{
  "url": "path to function ingress" # you should protect your endpoint e.g. using a secret via: ?token=<secret>
}
