## Requirements:
| Program                                                                  |                              Usecase                               |
| :----------------------------------------------------------------------- | :----------------------------------------------------------------: |
| [NodeJS](https://nodejs.org/en/download/)                                |                  Starting/setting up relay server                 |
| [XAMPP](https://www.apachefriends.org/download.html)                     | Hosting the overlay + the map-pool/playlist files for local upload |

If you plan on deploying as production, do **not** use XAMPP as it requires configuration to be safe. 

This README is assuming you're **only hosting it locally and not on a domain**

## Setup relay-server.
Install both NodeJS.

- Copy `RelayServer/Standard WebSocket` to a folder on your PC. (`C:/Tournament/RelayServer/Standard WebSocket` Is used in this example).
- Set the IP/Port to both the relay-server and the TA server in in `C:/Tournament/RelayServer/Standard WebSocket/src/settings.js`. \
(If you're using a Password for the TA-server, you also change that in there)
- Open your terminal of choice and navigate to the Socket-folder. I.e `cd C:/Tournament/RelayServer/Standard WebSocket`.
- Run `npm install -g typescript`.
- Run `npm install`.
- Run `npm start` when the installation is finished

You should now see "Connected to relay-server". If not, start from step 1 and try again. - If it keeps happening, contact [Hawk](https://discordapp.com/users/592779895084679188)
- To verify the websocket-server functions, download [WebSocketKing](https://chromewebstore.google.com/detail/websocket-king-client/cbcbkhdmedgianpaifchdaddpnmgnknn), open the extension and connect to `ws://localhost:YourPort`.
If it works, you should see `{"Type": "0","message": "You've connected to the relay server."}` in the output.

If errors still keeps happening, contact [Hawk](https://discordapp.com/users/592779895084679188)

## Setup overlay files:
- Install XAMPP/Your Webserver of choice.
- When asked to "Select Components", select the [following](https://i.imgur.com/eoPJIA9.png): 
    - Apache
    - PHP

- After installation, start the XAMPP Control Panel, and start the Apache service.
- Open a browser and go to `http://localhost/` and you should see the XAMPP welcome page.
- After verifying that the webserver is working, go to `X:/xampp/htdocs/`, delete the contents in the folder.
- Copy the contents of `Frontends/Frontend - Local` to your htdocs/public folder in your XAMPP/Webserver installation. (I'm gonna use XAMPP, so I'll copy it to `C:/xampp/htdocs/`.)

- Add the following files to browser sources in OBS:

| URL                                                                  |          Scene          |  Order   | Resolution  |
| :------------------------------------------------------------------- | :---------------------: | :------: | :---------: |
| `http://localhost/CD/CD.html`                                        |          `CD`           |    -     | `1920x1080` |
| `http://localhost/1V1/`                                              |          `1V1`          |  `Top`   | `1920x1080` |
| `http://localhost/1V1/twitchstream.html?v=0`                         |          `1V1`          | `Bottom` | `1920x1080` |
| `http://localhost/1V1/twitchstream.html?v=1`                         |          `1V1`          | `Bottom` | `1920x1080` |
| `http://localhost/PB/`                                               |    `Picks and Bans`     |  `Top`   | `1920x1080` |
| `http://localhost/2V2/`                                              |          `2V2`          |  `Top`   | `1920x1080` |
| `http://localhost/2V2/twitchstream.html?v=0`                         |          `2V2`          | `Bottom` | `1920x1080` |
| `http://localhost/2V2/twitchstream.html?v=1`                         |          `2V2`          | `Bottom` | `1920x1080` |
| `http://localhost/2V2/twitchstream.html?v=2`                         |          `2V2`          | `Bottom` | `1920x1080` |
| `http://localhost/2V2/twitchstream.html?v=3`                         |          `2V2`          | `Bottom` | `1920x1080` |
| `http://localhost/BR/BROverlay/overlay.html`                         |     `Battle Royale`     |  `Top`   | `2560x1140` |
| `http://localhost/BR/BROverlay/`                                     |     `Battle Royale`     | `Bottom` | `1920x1080` |
| `http://localhost/BR/BROverlay/twitchstream.html`                    |     `Battle Royale`     | `Bottom` | `1920x1080` |
| `http://localhost/BR/PlayerScreen/`                                  | `Battle Royale Players` |  `Top`   | `1920x1080` |

- When everything is setup, you can head over to `http://localhost/WebPanel/` in your browser and start test the connections.

Things to note:
- Players have to join the TA-server **after** you've started the relay server. 
- The `Reload overlay`-button only allows for reloading the 1V1-overlay.
- `/1V1/twitchstream.html` allows for changing of the `v`-variable, `?v=0` = Player 1, `?v=1` = Player 2.
- `/2V2/twitchstream.html` allows for changing of the `v`-variable, `?v=0` = Player 1, `?v=1` = Player 2, `?v=2` = Player 3, `?v=3` = Player 4.
- `/BR/BROverlay/twitchstream.html` does **not** allow for this.

If you have any question or problems, feel free to reach out on Discord, [Hawk](https://discordapp.com/users/592779895084679188), and I'll try to help you out as soon as possible.
