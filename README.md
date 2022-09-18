# dash-player
This repo is based on the implementation of dash which is Dynamic and Adaptive Steaming over HTTPS .This technology is used in many streaming platforms like netflix ,youtube  etc.
I have written the code for both Ends <br />
  1.Server <br />
  2.Client <br />
# Structure <br />
main.js   <br />
video  <br />
- video_id{ID}  <br />
  - my_video_manifest.mpd <br />

# Server Side Implementation <br />
==> main.js
```js
const express = require('express')
const app = express()
const fs=require('fs');
const path = require('path')
app.use(function (req, res, next) {
    res.header("Access-Control-Allow-Origin", "*"); // update to match the domain you will make the request from
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
console.log("listening att 5000");
```
