# Twitch API

## Start

Bisogna registrare la propria app sulla console dev di twitch in modo da avere client_id e client_secret. Twitch supporta 3 differenti token:
1) ID tokens (OIDC) ⇒ fornisce un set di informazioni sull’utente. Sono rappresentati nei claims del JWT
2) User access token ⇒ autentica gli utenti e permette all’app di eseguire richieste. Da usare se l’app utilizza login con twitch o ha bisogno di richieste autenticate.
3) app access token ⇒ permette di eseguire chiamate senza autenticare il singolo l’utente ma soltanto il client. E’ quello che utilizza flownerd.
Il dominio per l’autenticazione è [https://id.twitch.tv](https://id.twitch.tv/)

Esistono 3 diversi flow di autenticazione
1) Implicit code flow ⇒ le richieste arrivano direttamente da un client come app/web app.
2) Authorization code flow ⇒ la nostra app utilizza un server che costudisce il client_secret ed esegue le richieste al server twitch.
3) Client credentials flow ⇒ utilizza app access token

## OAuth implicit code

1) Manda l’utente da autenticare al redirect URI di registrazione. Una pagina di authorization chiederò all’utente di loggarsi con twitch e di scegliere cosa la nostra app avrà il permesso di fare. Per farlo creare un button che punti alla chiamata 

```jsx
GET https://id.twitch.tv/oauth2/authorize
    ?client_id=<your client ID>
    &redirect_uri=<your registered redirect URI>
    &response_type=<type>
    &scope=<space-separated list of scopes>
```

2) L’utente una volta loggato viene portato al redirect_uri (che deve corrispondere con l’url nella console dev di twitch). L’access token non sarà visibile come query params ma come URL fragment, si può recuperare con 

```jsx
document.location.hash
```

## OAuht authorization code flow

1) La prima parte è identica all’OAuth implicit code. 

2) Una volta loggato la redirect avviene all’indirizzo

```jsx
https://<your registered redirect URI>/?code=<authorization code>
```

L’authorization code è una stringa generata randomicamente di 30 caratteri.

3) Sul server è possibile quindi recuperare l’access token con la seguente richiesta

```jsx
POST https://id.twitch.tv/oauth2/token
    ?client_id=<your client ID>
    &client_secret=<your client secret>
    &code=<authorization code received above>
    &grant_type=authorization_code
    &redirect_uri=<your registered redirect URI>
```

4) Twitch risponde con un JSON simile

```json
{
  "access_token": "<user access token>",
  "refresh_token": "<refresh token>",
  "expires_in": <number of seconds until the token expires>,
  "scope": ["<your previously listed scope(s)>"],
  "token_type": "bearer"
}
```

## Utilizzare il token

Quando bisogna eseguire una richiesta con authorization bisogna mettere l’access token nell’header.

```json
curl -H "Authorization: Bearer <access token>" https://api.twitch.tv/helix/
```

## Refreshing token

Per refreshare il token usare

```json
POST https://id.twitch.tv/oauth2/token
    --data-urlencode
    ?grant_type=refresh_token
    &refresh_token=<your refresh token>
    &client_id=<your client ID>
    &client_secret=<your client secret>
```

Twitch risponde con 401 se il token è scaduto!