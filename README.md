# vue-google-auth
Handling Youtube sign-in and sign-out for Vue.js applications

## Installation
```
yarn add https://github.com/theMotivators/vue-youtube-auth.git
or
npm add https://github.com/theMotivators/vue-youtube-auth.git
```

## Initialization
```
import youtubeAuth from 'vue-youtube-auth'

Vue.use(youtubeAuth, { clientID: 'xxxxxxxxxx-xxxxxxxxxxxxxxxxxxxxxxxxxx.apps.googleusercontent.com' })
Vue.youtubeAuth().load()
```
Ideally you shall place this in your app entry file, e.g. src/main.js

## Usage - Sign-in
### (a) Handling youtube sign-in, getting the one-time authorization code from Youtube
```
import Vue from 'vue'

Vue.youtubeAuth().signIn(function (authorizationCode) { 

  // things to do when sign-in succeeds
  
  // You can send the authorizationCode to your backend server for further processing, for example
  this.$http.post('http://your/backend/server', { code: authorizationCode, redirect_uri: 'postmessage' }).then(function (response) {
    if (response.body) {
      // ...
    }
  }, function (error) {
    console.log(error)
  })
  
}, function (error) {
  // things to do when sign-in fails
))
```

The `authorizationCode` that is being returned is the `one-time code` that you can send to your backend server, so that the server can exchange for its own access token and refresh token.


### (b) Alternatively, if you would like to directly get back the access_token and id_token
```
import Vue from 'vue'

// Just add in this line
Vue.youtubeAuth().directAccess()

Vue.youtubeAuth().signIn(function (youtubeUser) { 
  // things to do when sign-in succeeds
}, function (error) {
  // things to do when sign-in fails
))
```

The `youtubeUser` object that is being returned will be:
```
{
  "token_type": "Bearer",
  "access_token": "xxx",
  "scope": "xxx",
  "login_hint": "xxx",
  "expires_in": 3600,
  "id_token": "xxx",
  "session_state": {
    "extraQueryParams": {
      "authuser": "0"
    }
  },
  "first_issued_at": 1234567891011,
  "expires_at": 1234567891011,
  "idpId": "youtube"
}
```

## Usage - Sign-out
Handling youtube sign-out
```
import Vue from 'vue'

Vue.youtubeAuth().signOut(function () { 
  // things to do when sign-out succeeds
}, function (error) {
  // things to do when sign-out fails
))
```

## Usage with Nuxt.js
nuxt.config.js
```
plugins: [
    { src: '~plugins/youtube.js', ssr: false, injectAs: 'youtubeAuth' },
  ]
```
plugins/youtube.js
```
import Vue from 'vue'
import YoutubeAuth from 'vue-youtube-auth'
import { GOOGLE_CLIENT_ID } from '../config/constants'

Vue.use(YoutubeAuth, { clientId: GOOGLE_CLIENT_ID })
Vue.youtubeAuth().load()

export default YoutubeAuth
```
Access Youtube Authentication in any .vue file
```
this.$youtubeAuth().signIn(this.signInCallback, (error) => {
        this.error = 'Youtube sign in failed'
      });
```

## Additional Help
~~Do refer to this [sample login page HTML file](https://github.com/simmatrix/vue-youtube-auth/blob/master/sample.html).~~

If you are curious of how the entire youtube sign-in flow works, please refer to the diagram below
![Google Sign-in Flow](http://i.imgur.com/BQPXKyT.png)

### Original Repository
https://github.com/simmatrix/vue-google-auth

Thanks to @simmatrix, the original repository author.