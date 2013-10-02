$(document).ready(function(){

//When script loads
API.sendChat(":arrows_counterclockwise:")
$('#button-vote-positive').click();

//global var
var total = 0;


if (localStorage.usData === undefined) {
localStorage.usData = JSON.stringify({
counter: 0,
})
}

function fanEveryone(data) {
var relationship = require('app/models/TheUserModel');
if (relationship.getRelationship(data.id) < 2) {
var fan = require('app/services/user/UserFanService');
fan = new fan(true, data.id);
var totalCount = JSON.parse(localStorage.usData);
++totalCount.counter;
console.log('Fanned new user: ' + data.username + '. Total number fanned: ' + totalCount.counter);
localStorage.usData = JSON.stringify(totalCount);
total + 1;
}
}
API.on(API.USER_FAN, fanEveryone);

//chat commands and so on below here 
var intervalMessage = setInterval(function(){message();},240000);

function message(){
var m, msgs;
msgs = ["Se torne meu fã q retribui na hora :thumbsup: ", ":star: F :star: A :star: N :star: ?", " Torne-se meu fan :100: frescura ", "", ""];

m = Math.floor(Math.random() * msgs.length);
API.sendChat(msgs[m]); 

}

API.on(API.CHAT_COMMAND, command);
function command(value)
{
switch(value)
{
case "/stopchat":
clearInterval(intervalMessage);
API.chatLog("FanBOT CHAT OFFLINE => BOT ONNLINE", alert)
break;

case "/restart":
intervalMessage = setInterval(function(){message();},240000);
break;

case "/fans?":
//API.sendChat(total + " People fanned since launched");
API.chatLog(total + " People fanned since launched", alert)
break;

case "/chat":
message();
break;

}
}

API.on(API.DJ_ADVANCE, woot);
function woot()
{
$('#button-vote-positive').click();
}

});
