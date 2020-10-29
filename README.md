# nspostman
This is a pre-request script for use with Postman and the Netsapiens platform

The following requires the use of Postman from https://www.postman.com/ and your Netsapiens instance

##Instructions
Under your Netsapiens collection, right click and then Edit

##Under Authorization
Set Type: OAuth 2.0
Add auth data to: Request Headers
Access Token: {{accessToken}}
Header Prefix: Bearer

##Pre-request Scripts
```javascript
let fqdn = pm.variables.get('fqdn');
let clientid = pm.variables.get('clientid');
let clientsecret = pm.variables.get('clientsecret');
let username = pm.variables.get('username');
let password = pm.variables.get('password');

let tokenUrl = 'https://' + fqdn + '/ns-api/oauth2/token/?';
let getTokenRequest = {
    method: 'POST',
    url: tokenUrl,
    body: {
        mode: 'formdata',
        formdata: [
            { key: 'grant_type', value: 'password' },
            { key: 'client_id', value: clientid },
            { key: 'client_secret', value: clientsecret },
            { key: 'username', value: username },
            { key: 'password', value: password }
        ]
    }
};

pm.sendRequest(getTokenRequest, (err, response) => {
    let jsonResponse = response.json(),
        newAccessToken = jsonResponse.access_token;

    console.log({ err, jsonResponse, newAccessToken })

    pm.environment.set('accessToken', newAccessToken);
    pm.variables.set('accessToken', newAccessToken);
    pm.environment.set('access_token', newAccessToken);
});```
##Variables
Add the following variables

fqdn = The FQDN of your portal server
clientid = Your ClientID
clientsecret = Your Client Secret
username = Your portal user name
let password = Your portal password

![Example Images](https://imgur.com/a/cSJmUDQ)
