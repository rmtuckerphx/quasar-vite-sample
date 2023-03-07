# NOTES

1. Start the web app, open the Chrome console, click the button

Error: `Cannot read properties of undefined (reading 'call')`

2. Installed `vite-plugin-node-polyfills` in `quasar.config.js` under `vitePlugins`
3. Start the app, click the button

Error: Buffer is not defined

4. Traced error to `micStream.setStream(media)` (`IndexPage.vue` line 27) which calls `buffer-from` which calls `new Buffer`.

If might be a Buffer global issue but I'm not sure how to configure quasar correctly.

https://github.com/nodejs/readable-stream/pull/435#issuecomment-1053641748
