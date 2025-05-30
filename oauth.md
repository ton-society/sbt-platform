## TON ID OIDC and OAuth 2.0 Integration Guide
TON ID allows apps to get users profile data and badge info via oAuth.

This guide explains how to integrate with TON ID using OAuth 2.0 Authorization Code Flow with PKCE, requesting user identity and refresh capability. [PKCE Flow demo](https://www.oauth.com/playground/authorization-code-with-pkce.html).

To obtain ```CLIENT_ID``` for your app please [contact us at Telegram](https://t.me/boldov).

## OAuth Flow Overview

### 1. Generate PKCE Values
Generate a **code verifier** and **code challenge**:
- `code_verifier` = random string (43–128 characters)
- `code_challenge` = base64url(SHA256(code_verifier))

```
export function generateCodeVerifier(): string {
  const array = new Uint8Array(32);
  window.crypto.getRandomValues(array);
  return base64URLEncode(array);
}

export async function generateCodeChallenge(verifier: string): Promise<string> {
  // Convert the verifier to a Uint8Array
  const encoder = new TextEncoder();
  const data = encoder.encode(verifier);

  // Hash the verifier
  return window.crypto.subtle
    .digest('SHA-256', data)
    .then((hash) => base64URLEncode(new Uint8Array(hash)));
}

function base64URLEncode(buffer: Uint8Array): string {
  return btoa(String.fromCharCode(...buffer))
    .replace(/\+/g, '-')
    .replace(/\//g, '_')
    .replace(/=/g, '');
}
```

### 2. Redirect User to Authorization Endpoint
Redirect the user to: `https://id.ton.org/v1/oauth2/signin` with the following query parameters:
- `response_type=code`
- `client_id=YOUR_CLIENT_ID`
- `redirect_uri=https://yourapp.com/callback`
- `scope=openid+profile+offline_access`
- `state=RANDOM_CSRF_STRING`
- `code_challenge=base64url(sha256(code_verifier))`
- `code_challenge_method=S256`

> Since we’re using PKCE, no client_secret is required during the authorization or token exchange process. The code challenge and code verifier ensure the integrity of the request instead.

After redirect to ⁠https://id.ton.org/v1/oauth2/signin, the user will be automatically redirected to the [TON ID Mini App](https://t.me/id_app) in Telegram for authorization. User approves requested scopes in the Mini App, and get redirected to the ```redirect_uri``` callback.

<img width="390" alt="Data request" src="https://github.com/user-attachments/assets/33693260-c78a-4d04-a1cc-f466f7469807" />   <img width="390" alt="Requested scope approved" src="https://github.com/user-attachments/assets/e723311f-6663-484b-bc4f-ed0c4bcdaee7" />




### 3. Handle Redirect and Extract Code

User will be redirected to your app: `https://yourapp.com/callback?code=AUTH_CODE&state=STATE&scope=openid+profile+offline_access`.

Extract:

- `code` (for token exchange)
- `state` (to verify)
- `scope` (optional, informational only)

### 4. Exchange Code for Tokens
POST to the token endpoint:
```
POST https://id.ton.org/v1/oauth2/token
Content-Type: application/x-www-form-urlencoded

grant_type=authorization_code
code=AUTH_CODE
redirect_uri=https://yourapp.com/callback
client_id=YOUR_CLIENT_ID
code_verifier=YOUR_CODE_VERIFIER
```
​
Response will contain:
```
{
  "access_token": "ACCESS_TOKEN",
  "id_token": "ID_TOKEN",
  "refresh_token": "REFRESH_TOKEN",
  "scope": "requested scopes",
  "expires_in": 3600,
  "token_type": "bearer"
}
```

## Supported scopes
### openid 
This scope facilitates sign-in with TON ID using the OpenID Connect standard, allowing the application to authenticate users and obtain an ID token. The ID token is a JWT, signed with the server's private key:

Example of token response for ```openid``` scope:
```
{
  "access_token": "ory_at_iRhbwqGqRlM48nxEPMkOPvMy8dSN9PGyg14o_L63g54.ze9bq15DaMd7RxnwEDpWK9goKeVhH1wCLgqFIENh_mA",
  "expires_in": 21600,
  "id_token": "eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCJ9.eyJhdF9oYXNoIjoiZmlPQWtEVnJCMWduVl9SYjVVUjNfUSIsImF1ZCI6WyJkZWJ1ZyJdLCJhdXRoX3RpbWUiOjE3NDY1NDIxMTYsImV4cCI6MTc0NjU2MzcxNiwiaWF0IjoxNzQ2NTQyMTE4LCJpc3MiOiJodHRwczovL3NvY2lldHkudG9uLm9yZyIsImp0aSI6ImQ0NmIzNDI4LWY5NjItNDZmMi04MjAwLWUzMzc1ZWY2YjM2YyIsInJhdCI6MTc0NjU0MjExNiwic3ViIjoiNWQ3M2EwOTQtNzhmZi00MTlkLWFkODEtYTg3Njk0YmY0NjY2In0.zyR4eQ_v_MKMfW35rvsO6xIVI1btXMLU0d2BEBJG9zMp5OWquW060dPiSQ6X_9AURKpzmmMVLLO-PqGXeIY57jM3stWfdkMAVJg5EO_YnnOhoV56A48T6P4wTnzx4vwUzT0cdRETc8D1i_iGYa_QIGzuBgCMV6k80l_lMovv3USB8GOpTeLDw8D0-QDv5K26p0ECCsvjlRinUk0gI2mWCwFht8BMw_GY2Z-pX5jZTGB3o4U1clPeKAN_ai8lbucan_3ypBvnY1KFc6ceMy07_f1ihbCC62QE3yqHqJtAu_T5VODCCqfQIBsWmf6Yl0hADP-p4xd2raaoCCjIYEcEQg",
  "scope": "openid",
  "token_type": "bearer"
}
```
​ID Token JWT decoded:
```
{
  "at_hash": "fiOAkDVrB1gnV_Rb5UR3_Q",
  "aud": ["YOUR_CLIENT_ID"], // Client ID that this token is intended for
  "auth_time": 1746542116,
  "exp": 1746563716,
  "iat": 1746542118,
  "iss": "https://id.ton.org", // The issuer URL that generated this token
  "jti": "d46b3428-f962-46f2-8200-e3375ef6b36c",
  "rat": 1746542116,
  "sub": "111-222-333" //Unique identifier of the user
}
```
### profile
Requests access to basic profile information, such as ```name```, ```picture``` and ```telegram_id```.

Request:
```
GET https://id.ton.org/v1/oauth2/userinfo
Authorization: Bearer access_token
```

Example of the response:
```
{
    "status": "success",
    "data": {
        "name": "Alex",
        "picture": "https://demo.app/alex-avatar.png",
        "sub": "111-222-333", //user id
        "telegram_id": 12345678
         "socials": {
            "twitter": true,
            "youtube": false
        }
    }
}
```
If requested along with ```openid``` scope, those fields are also populated within ID token.

### wallet
Allows to ask the user to share a wallet address linked to their TON ID during the authorization flow.

Request:
```
GET https://id.ton.org/v1/wallet
```

Example of the response:
```
{
    "status": "success",
    "data": {
        "friendly_wallet_address": "UQAw0Zxb55PR2HtfD793UTXggGNNZJcTKh3bh4u4X-nMWMAz",
        "raw_wallet_address": "0:30d19c5be793d1d87b5f0fbf775135e080634d6497132a1ddb878bb85fe9cc58"
    }
}
```

> **Sharing is optional**
>
> The user may choose to **skip** sharing their wallet address.
> If the wallet address is not shared, the `⁠/v1/wallet` endpoint will return a 
> `⁠404 Not Found` error.

### offline_access
Requests a refresh token so your app can obtain new access tokens without user interaction.

Request:
```
POST https://id.ton.org/v1/oauth2/token
Content-Type: application/x-www-form-urlencoded

grant_type=refresh_token
refresh_token=YOUR_REFRESH_TOKEN
client_id=YOUR_CLIENT_ID
```

Response:
```
{
  "access_token": "ACCESS_TOKEN",
  "id_token": "ID_TOKEN",
  "refresh_token": "REFRESH_TOKEN",
  "scope": "requested scopes",
  "expires_in": 3600,
  "token_type": "bearer"
}
```


## OAuth Discovery Info

- Issuer: https://id.ton.org
- JWKS Endpoint: https://id.ton.org/.well-known/jwks.json
- User Info: https://id.ton.org/v1/oauth2/userinfo
- Token Endpoint: https://id.ton.org/v1/oauth2/token
- Authorization URL: https://id.ton.org/v1/oauth2/signin

## Notes

- PKCE (`code_challenge`) and `state` are **required**.
- `refresh_token` is only returned if `offline_access` scope is granted and your client allows it.

## NextAuth Provider Config

If you're using **`NextAuth.js`**, here's an example provider configuration for TON ID:
```
export const TONIDProvider: OAuthConfig<any> = {
  id: 'tonid',
  name: 'TON ID',
  version: '2.0',
  type: 'oauth',
  authorization: {
    url: 'https://id.ton.org/v1/oauth2/signin',
    params: {
      scope: 'openid profile offline_access',
    },
  },
  token: 'https://id.ton.org/v1/oauth2/token',
  issuer: 'https://id.ton.org',
  jwks_endpoint: 'https://id.ton.org/.well-known/jwks.json',
  userinfo: 'https://id.ton.org/v1/oauth2/userinfo',
  clientId: process.env.TON_ID_CLIENT_ID,
  idToken: true,
  checks: ['state', 'pkce'],
  profile: (profile) => ({
    id: profile.sub,
    name: profile.name,
    username: profile.sub,
    image: profile.picture,
    telegramId: profile.telegram_id
  }),
};
```
