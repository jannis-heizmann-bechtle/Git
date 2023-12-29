# iOS - App Whitelisting: Webclips werden nicht mehr angezeigt

Wenn mit MobileIron oder AirWatch eine App Whitlelist erstellt wird, die alle anderen Icons ausblendet, werden standardmäßig alle WebClips blockiert. Dass bedeutet dass der inhouse AppStore (Apps@Work / Catalog) auch nicht angezeigt werden.

Wenn dies nicht erwünscht ist, können pauschal alle WebClips erlaubt werden, indem folgende Bundle ID auf die Whitelist gesetzt wird:

```
com.apple.webapp
```
