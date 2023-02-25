# redditor


## Development stages

### Stage 1: Create Script application in Reddit
There are 2 types of applications in Reddit: Script and Web. 
Script applications are used for automation and Web applications are used for user interaction. 
In this example I will use Script application.

### Stage 2: Authentication in Postman

Script applications requires OAuth authorization and ACCESS_TOKEN to be able to make requests to Reddit API.

```js
const root = pm.variables.get("root");
const username = pm.variables.get("username");
const password = pm.variables.get("password");
const client_id = pm.variables.get('script_client_id');
const secret = pm.variables.get('script_secret');
const url = root + "/api/v1/access_token";

const postRequest = {
  url: url,
  method: 'POST',
  timeout: 0,
  header: {
    "Content-Type": "application/x-www-form-urlencoded",
    "Authorization" : 'Basic ' + btoa(client_id + ':' + secret)
  },
  body: {
    mode: 'urlencoded',
    urlencoded: [
        {key: "grant_type", value: "password"},
        {key:"username", value: username},
        {key:"password", value: password},
    ]}
};

pm.sendRequest(postRequest, function (err, res) {
    var responseJson = res.json();
    console.log(responseJson);
    pm.environment.set('ACCESS_TOKEN', responseJson['access_token']);
    pm.variables.set('ACCESS_TOKEN', responseJson['access_token']);
});
```

### Stage 3: Create a new post request to submit a new post
