# pwa
Maniere rapide de creer une application PWA
Utilisee dans [rezocoquin](www.rezocoquin.com)


## steps 
1. edit **manifest.json**
```
{
  "name": "RezoCoquin Site echangiste",
  "short_name": "RezoCoquin",
  "start_url": ".",
  "display": "standalone",
  "background_color": "#ffffff",
  "description":"Rezocoquin, site libertin",
  "theme_color": "#ffffff",
  "scope" : "./",
  "icons": [
    {
      "src": "/android-chrome-192x192.png",
      "sizes": "192x192",
      "type": "image/png"
    },
    {
      "src": "/android-chrome-512x512.png",
      "sizes": "512x512",
      "type": "image/png"
    }
  ],
  "gcm_sender_id": "482941778795",
  "DO_NOT_CHANGE_GCM_SENDER_ID": "Do not change the GCM Sender ID"
}
```

2. Add it to your index page like:
```
<link rel="manifest" href="./manifest.json">
```
3. add the **registerSW.js** script to your index.php that will handle the registration process
```
<script src="registerSW.js"></script>
```
4. create and upload to root registerSW.js script

```
window.addEventListener('load', e => {
    registerSW();
});
async function registerSW() {
    if ('serviceWorker' in navigator) {
        try {
            await navigator.serviceWorker.register('/sw.js');
        } catch (e) {
            console.log('ServiceWorker registration failed');
        }
    }
}
```
5. Create sw.js and upload to root  
```
const cacheName = 'pwa-conf-v1';
const staticAssets = [
    './',
    './offline.html',
];
self.addEventListener('install', async event => {
    const cache = await caches.open(cacheName);
    await cache.addAll(staticAssets);
});
self.addEventListener('fetch', event => {
    fetch(event.request)
});
async function cacheFirst(req) {
    const cache = await caches.open(cacheName);
    const cachedResponse = await cache.match(req);
    return cachedResponse || fetch(req);
}

```
6. create and upload the **offline.html** page to the root
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Offline</title>
</head>
<body>
    <h1>
        Offline now !
    </h1>
</body>
</html>
```


That's all !
