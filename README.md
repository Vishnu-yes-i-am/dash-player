# dash-player
This repo is based on the implementation of dash which is Dynamic and Adaptive Steaming over HTTPS .This technology is used in many streaming platforms like netflix ,youtube  etc.
I have written the code for both Ends <br />
  1.Server <br />
  2.Client <br />

### Directory Structure
    .
    ├── main.js                  
    ├── video .                 
              ├── id_{ID} .                   
                           ├── my_video_manifest.mpd         #file to provide as a source to client
                           ├── stream_360p                
                           ├── stram_480p
                           └── stream_720p
## Server Side Implementation <br />
==> main.js
```js
const express = require('express')
const app = express()
const fs=require('fs');
const path = require('path')
app.use(function (req, res, next) {
    res.header("Access-Control-Allow-Origin", "*");
    res.header("Access-Control-Allow-Headers", "*");
    next();
});
app.get('/video/id_*/play', function (req, res, next) {

        const filepath=path.join(__dirname, `${req.path.replace("/play","")}/my_video_manifest.mpd`);
        console.log(filepath);
        try {
         if (fs.existsSync(filepath)) {
                res.sendFile(filepath);
                }
        else{
                res.status(404).send("File Not Found");
        }
        }
        catch(err) {
        console.error(err);
        }
        });
app.get("/video/id_*/:fname", (req, res) => {
        const filepath=path.join(__dirname, `${req.path}`);
        try {
                if (fs.existsSync(filepath)) {
                        res.status(206).sendFile(filepath);
                }
               else{
                res.status(404).send("File Not Found");                                          }
        }
         catch(err) {
                 console.error(err);
                }
})
app.listen(5000);
console.log("listening at 5000");
```
## Client Side Implementation <br />

```html
<!doctype html>
<html>

<head>
    <title>Dash-vm Player</title>
    <style>
        video {
            width: 600px;
            height: 350px;
        }
    </style>
</head>

<body>
    <div>
        <video id="videoPlayer" controls></video>
    </div>
    <script src="http://cdn.dashjs.org/latest/dash.all.min.js"></script>
    <script>
        (function () {
            var url = "http://SERVER_ADDRESS:5000/video/id_1/play";
            var player = dashjs.MediaPlayer().create();
            player.initialize(document.querySelector("#videoPlayer"), url, true);
        })();
    </script>
</body>

</html>
```
