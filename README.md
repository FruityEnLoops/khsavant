# khsavant

KHSavant is a small webapp made with Vue.js and CoreUI to display achievements for games from the Kingdom Hearts series from Steam, but like it would on PlayStation (meaning split between each individual game).

This helps keeping track easily of what you are missing from each game, as achievements can be easily confused between two games in the same collection.

[KHSavant is live on my website!](https://blobdash.fr/khsavant/)

This is my first ever Vue.js project, and the code is likely very bad. Sorry for what you read in there!

### Running

* Clone this repo.
* Clone the [khsavant-proxy](https://github.com/FruityEnLoops/khsavant-proxy) repository and follow the README. See why under.
* Install dependencies (`npm i`), then run using `npm run dev`.
* If your request proxy runs on a different port than khsavant (which is likely), edit `App.vue` to specify where the proxy requests should go.

### Why a proxy?

Steam API requires your requests to be proxied, as they enforce CORS. A browser cannot make API requests on its own; it needs to be proxied.

[khsavant-proxy](https://github.com/FruityEnLoops/khsavant-proxy) is a very small Express project that will allow you to do so.

### Credits

@joamjoamjoam and their project [SteamAchievementViewer](https://github.com/joamjoamjoam/SteamAchievementViewer).

@televo and their project [Kingdom Hearts Re:Collection](https://github.com/Televo/kingdom-hearts-recollection), which this project uses assets from.
