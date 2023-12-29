# Problem mit scrollen

Workaround, falls es Probleme mit dem scrollen bei MobileIron unter Chrome / Firefox geben sollte:

Chrome:\
Enter „chrome://flags“ in the address bar of the browser\
Search for „Touch Events“\
Find „Touch Events API“ and make sure it is set to „Automatic“\
Relaunch Chrome



#### For Chrome v78 and later, follow the workaround below.

* Right click on the Chrome app shortcut  on desktop > Properties > Shortcut tab > in the Target field, add –touch-events at the end of chrom.exe“
* Example: _C:\Program Files (x86)\Google\Chrome\Application\chrome.exe“ –touch-events_

<div align="left">

<figure><img src="../../../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

</div>

* The change will force the touch api to work evertime Chrome is launched from that shortcut.

&#x20;

Firefox:\
Enter „about:config“ in the address bar of the browser\
Search for „touch\_events“\
Find „dom.w3c\_touch\_events.enabled“ and make sure it is set to „0“\
Relaunch Firefox
