## Requirements:
| Program                                                                  |                                               Usecase                                                |
| :----------------------------------------------------------------------- | :--------------------------------------------------------------------------------------------------: |
| [NodeJS](https://nodejs.org/en/download/)                                |                                        Starting relay server                                         |
| [XAMPP](https://www.apachefriends.org/download.html)                     | Webserver for hosting it online + Domain/IP with HTTPS/SSL Certificate. Allows for transportability. |

This README is assuming you're **hosting it on your own dedicated server with a domain pointed to it**. 

Why HTTPS? Because of Twitch-restricting embeds to HTTPS unless you're on localhost with a local webserver.

Don't have an SSL certificate for your domain? Use [Certbot](https://certbot.eff.org/instructions) to obtain a SSL certificate and private key.


## DISCLAIMER: 
If you plan on deploying as production, do **not** use XAMPP as it requires configuration to be safe. 

## Setup relay-server.
Install both NodeJS.

- Copy `RelayServer/Secure WebSocket` to a folder on your PC. (Gonna be using `C:/Tournament/RelayServer/Secure WebSocket` as an example in this).
- Copy your `certificate.pem` and `privatkey.pem` to the `Keys`-folder. Rename to `cert.pem` and `privkey.pem` respectively. \
(I **highly** recommmend symlinking the files, to ensure Certbot automatically can renew them on expiration)
- Change the IP/Port to both the relay-server and the TA server in in `C:/Tournament/RelayServer/Secure WebSocket/src/settings.js`. \
(If you're using a Password for the TA-server, you also change that in there)
- Open your terminal of choice and navigate to the Socket-folder. I.e `cd C:/Tournament/RelayServer/Secure WebSocket`.
- Run `npm install -g typescript`.
- Run `npm install`.
- Run `npm start` when the installation is finished

You should now see "Connected to relay-server". If not, start from step 1 and try again. - If it keeps happening, contact [Hawk](https://discordapp.com/users/592779895084679188)
- To verify the websocket-server functions, head over to [WebSocketKing](https://websocketking.com/) and connect to `wss://domain.com:2223`.
If it works, and you see `{"Type": "0","message": "You've connected to the relay server."}` in the output, congratulations, you have a secured WebSocket server

If errors still keeps happening, contact [Hawk](https://discordapp.com/users/592779895084679188)

## Setup on Webserver:
- Copy the contents of `Frontends/Frontend - Domain/` to your folder of choice on your webserver. I'm gonna use XAMPP, so I'll copy it to `C:/xampp/htdocs/`.
- Change the IP in the following files:

| Filepath                                              | Line  |
| :---------------------------------------------------- | :---: |
| `{YourFolder}/1V1/js/MainHandlers.js`                 |  `1`  |
| `{YourFolder}/1V1/twitchstream.html`                  | `38`  |
| `{YourFolder}/PB/js/MainHandlers.js`                  |  `1`  |
| `{YourFolder}/2V2/js/MainHandlers.js`                 |  `1`  |
| `{YourFolder}/2V2/twitchstream.html`                  | `38`  |
| `{YourFolder}/PB2V2/js/MainHandlers.js`               |  `1`  |
| `{YourFolder}/BR/BROverlay/js/VisualsHandlers.js`     |  `1`  |
| `{YourFolder}/BR/BROverlay/overlay.html`              | `273` |
| `{YourFolder}/BR/BROverlay/twitchstream.html`         | `35`  |
| `{YourFolder}/BR/PlayerScreen/js/MainHandlers.js`     |  `1`  |
| `{YourFolder}/WebPanel/assets/js/MainHandlers.js`     |  `1`  |

- Change parent-domain in the following files, to the domain you're accessing it through (I.e "https://danesaber.cf/", remember to add subdomain if you're using one):

| Filepath                                           | Line  |
| :------------------------------------------------- | :---: |
| `{YourFolder}/1V1/twitchstream.html`               | `39`  |
| `{YourFolder}/2V2/twitchstream.html` | `39`  |
| `{YourFolder}/BR/BROverlay/twitchstream.html`      | `36`  |

- Add the following files to browser sources in OBS/Or Import the Scene-collection(W/ scene-structure), and point the links to the correct paths:

| URL                                                                   |          Scene          |  Order   | Resolution  |
| :-------------------------------------------------------------------- | :---------------------: | :------: | :---------: |
| `https://domain/{YourFolder}/CD/CD.html`                              |       `Countdown`       |    -     | `1920x1080` |
| `https://domain/{YourFolder}/1V1/`                                    |          `1V1`          |  `Top`   | `1920x1080` |
| `https://domain/{YourFolder}/1V1/twitchstream.html?v=0`               |          `1V1`          | `Bottom` | `1920x1080` |
| `https://domain/{YourFolder}/1V1/twitchstream.html?v=1`               |          `1V1`          | `Bottom` | `1920x1080` |
| `https://domain/{YourFolder}/PB/`                                     |    `Picks and Bans`     |  `Top`   | `1920x1080` |
| `https://domain/{YourFolder}/2V2/`                                    |          `2V2`          |  `Top`   | `1920x1080` |
| `https://domain/{YourFolder}/2V2/twitchstream.html?v=0`               |          `2V2`          | `Bottom` | `1920x1080` |
| `https://domain/{YourFolder}/2V2/twitchstream.html?v=1`               |          `2V2`          | `Bottom` | `1920x1080` |
| `https://domain/{YourFolder}/2V2/twitchstream.html?v=2`               |          `2V2`          | `Bottom` | `1920x1080` |
| `https://domain/{YourFolder}/2V2/twitchstream.html?v=3`               |          `2V2`          | `Bottom` | `1920x1080` |
| `https://domain/{YourFolder}/BR/BROverlay/overlay.html`               |     `Battle Royale`     |  `Top`   | `2560x1140` |
| `https://domain/{YourFolder}/BR/BROverlay/`                           |     `Battle Royale`     | `Middle` | `1920x1080` |
| `https://domain/{YourFolder}/BR/BROverlay/twitchstream.html`          |     `Battle Royale`     | `Bottom` | `1920x1080` |
| `https://domain/{YourFolder}/BR/PlayerScreen/`                        | `Battle Royale Players` |  `Top`   | `1920x1080` |

- When everything is setup, you can head over to `https://domain/{YourFolder}/WebPanel/` in your browser and start hosting matches.

## Things to note:
- Players have to join the TA-server **after** you've started the relay server. 
- The `Reload overlay`-button only allows for reloading the 1V1-overlay.
- `/1V1/twitchstream.html` allows for changing of the `v`-variable, `?v=0` = Player 1, `?v=1` = Player 2.
- `/2V2/twitchstream.html` allows for changing of the `v`-variable, `?v=0` = Player 1, `?v=1` = Player 2, `?v=2` = Player 3, `?v=3` = Player 4.
- `/BR/BROverlay/twitchstream.html` does **not** allow for this.

If you have any question or problems, feel free to reach out on Discord, [Hawk](https://discordapp.com/users/592779895084679188), and I'll try to help you out as soon as possible.
