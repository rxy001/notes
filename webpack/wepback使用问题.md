1. 关于 `webpackPreload wepbackPrefetch` 在 webpack 4 中已支持， 但是 htmlWebpackPlugin 并不支持此功能，此 webpack api 影响性能，导致无法生成 link tag。通过 htmlWebpackPlugin 的 plugin [@vue/preload-webpack-plugin](https://www.npmjs.com/package/@vue/preload-webpack-plugin) 可解决。

   ```js
   // preload image
   addWebpackPlugin(
     new PreloadWebpackPlugin({
       rel: "preload",
       as: "image",
       include: "allChunks",
       fileWhitelist: [/\.bmp$|\.gif$|\.jpe?g$|\.png$/],
     })
   ),
     // prefetch async js css
     addWebpackPlugin(
       new PreloadWebpackPlugin({
         rel: "prefetch",
         include: "asyncChunks",
         fileWhitelist: [/\.js$|\.css$/],
         as(entry) {
           if (/\.css$/.test(entry)) return "style";
           return "script";
         },
       })
     );

   // preload initial asset
   addWebpackPlugin(
     new PreloadWebpackPlugin({
       rel: "preload",
       include: "initial",
       as(entry) {
         if (/\.css$/.test(entry)) return "style";
         if (/\.woff$/.test(entry)) return "font";
         if (/\.bmp$|\.gif$|\.jpe?g$|\.png$/.test(entry)) return "image";
         return "script";
       },
     })
   );
   ```
