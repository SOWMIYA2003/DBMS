# DBMS

## index.js 
```
var express = require("express")
var bodyParser = require("body-parser")
var mongoose = require("mongoose")

const app = express()

app.use(bodyParser.json())
app.use(express.static('public'))
app.use(bodyParser.urlencoded({
    extended:true
}))

mongoose.connect('mongodb://127.0.0.1:27017/mydb',{
    useNewUrlParser: true,
    useUnifiedTopology: true
});

var db = mongoose.connection;

db.on('error',()=>console.log("Error in Connecting to Database"));
db.once('open',()=>console.log("Connected to Database"))

app.post("/sign_up",(req,res)=>{
    var name = req.body.name;
    var email = req.body.email;
    var phno = req.body.phno;
    var password = req.body.password;

    var data = {
        "name": name,
        "email" : email,
        "phno": phno,
        "password" : password
    }

    db.collection('users').insertOne(data,(err,collection)=>{
        if(err){
            throw err;
        }
        console.log("Record Inserted Successfully");
    });

    return res.redirect('signup_success.html')

})


app.get("/",(req,res)=>{
    res.set({
        "Allow-access-Allow-Origin": '*'
    })
    return res.redirect('index.html');
}).listen(3000);


console.log("Listening on PORT 3000");
```
![d5](https://user-images.githubusercontent.com/93427443/236759057-34ae9183-85c1-4a75-be1f-30516c58c556.png)
## index.html
```
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>SignUp Form - MongoDB</title>

    <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css" integrity="sha384-BVYiiSIFeK1dGmJRAkycuHAHRg32OmUcww7on3RYdg4Va+PmSTsz/K68vbdEjh4u" crossorigin="anonymous">

    <link rel="stylesheet" type="text/css" href="styles.css">
</head>

<body>

    <div class="container">
        <div class="row">
            <div class="col-md-3">

            </div>
            <div class="col-md-6 main">
                <form action="/sign_up" method="POST">
                    <h2>SignUp Form</h2>
                    <input type="text" class="box" id="name" name="name" placeholder="Name" required />
                    <br>
                    <input type="text" class="box" id="email" name="email" placeholder="Email" required />
                    <br>
                    <input type="text" class="box" id="phno" name="phno" placeholder="Mobile" required />
                    <br>
                    <input type="text" class="box" id="password" name="password" placeholder="Password" required />
                    <br>
                    <input type="submit" value="Submit" id="submit" />
                </form>
            </div>
            <div class="col-md-3">
            </div>
        </div>
    </div>

</body>

</html>
```

### style.css
```
.main {
    padding: 20px;
    font-family: 'Helvetica', serif;
    box-shadow: 5px 5px 7px 5px #888888;
}

.main h1 {
    font-size: 40px;
    text-align: center;
    font-family: 'Helvetica', serif;
}

input {
    font-family: 'Helvetica', serif;
    width: 100%;
    font-size: 20px;
    padding: 12px 20px;
    margin: 8px 0;
    border: none;
    border-bottom: 2px solid #767474;
}

input[type=submit] {
    font-family: 'Helvetica', serif;
    width: 100%;
    background-color: #767474;
    border: none;
    color: white;
    padding: 16px 32px;
    margin: 4px 2px;
    border-radius: 10px;
}
```
### signup_success.html
```
<!DOCTYPE html>
<html>

<head>
    <title> Signup Form</title>
    <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css" integrity="sha384-BVYiiSIFeK1dGmJRAkycuHAHRg32OmUcww7on3RYdg4Va+PmSTsz/K68vbdEjh4u" crossorigin="anonymous">

    <link rel="stylesheet" type="text/css" href="style.css">
</head>

<body>
    <br>
    <br>
    <br>
    <div class="container">
        <div class="row">
            <div class="col-md-3">
            </div>

            <div class="col-md-6 main">

                <h1> Signup Successful</h1>

            </div>


            <div class="col-md-3">
            </div>

        </div>
    </div>
</body>

</html>
```
#### Ternimal - Bash
```
nodemon index.js
```
![d2](https://user-images.githubusercontent.com/93427443/236757060-b485c0d3-d646-40fa-9936-2fe11eb74769.png)
### Commands
#### GIT BASH TERMINAL
```
mongod
```
![d4](https://user-images.githubusercontent.com/93427443/236757635-1c692d74-3138-45b2-b1d2-512f1d966931.png)
#### OPEN ANOTHER GIT BASH TERMINAL
```
mongo
show dbs
use mydb
db.users.find().pretty()
```
![d1](https://user-images.githubusercontent.com/93427443/236756951-700f11fc-c0e9-4169-aa74-a167fb83cde0.png)
![d3](https://user-images.githubusercontent.com/93427443/236757072-f7c7022c-9624-4e02-b551-be004e546897.png)
