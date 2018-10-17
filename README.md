class Lysz210\Laravel\Mixer
===
This package doesn't meant to replace [laravel-mix](https://github.com/JeffreyWay/laravel-mix) which I love, but it's a substitute of the mix function for Laravel app.  
I liked laravel-mix at first sight, it's great to have a very clever way to counter the browser caching but still update assets and make them updated, after I discover it I integreated it in every my project where I can't use directly webpack because all files are already there and restructuring the project it's simply unthinkable.

## Why Mixer?
The mix helper is very useful in views but it has some limitations and the most tight was the single manifest file in a folder, in most cases it's fine but some-times it's easier to merge more manifest together, in my case it happened because I was importing pictures that had to be made web-friendly and it produces directly the manifest like laravel-mix. And in a none laravel project my mix helper was simply a wrapper for a [Zend\Config\Config](https://github.com/zendframework/zend-config) contructed with all the files that ends with mix-manifest.json in the given folder.
```
$mixManifest = Zend\Config\Factory::fromFiles(glob(__DIR__ . '/*mix-manifest.json'));

function mix ($asset) {
  global $mixManifest;
  return $mixManifest->get($asset, $asset);
}
```
This strategy helped a lot to semplify assets management.
But when I was able to move the project to a full laravel project I hadn't this flexibility with the mix helper. But I was able to make samething similar importing the pictures in the Storage::disk('public') and build the manifest in the public disk.

The alternative to all this was using mix.copy, but the amount of immages was huge and coping every time I rebuild the assets was over-kill, I tried with 2 different webpack-mix.config.js but I don't know versioning large amount of files take very long time when iterating over all the files and applying md5_file is much much faster.