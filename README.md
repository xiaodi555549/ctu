# ctu
chrome plugin for convert chinese to unicode
## manifest.json
{
    "manifest_version": 2,
    "name": "chineseToUnicode",
    "description": "convert chinese to unicode",
    "version": "1.0.0",
    "permissions":[
        "https://*/*",
        "http://*/*"
    ],
    "browser_action":{
        "default_icon": "wechat.png",
        "default_popup": "popup.html"
    },
    "offline_enabled":true
}
## popup.html
<html>
    <head>
        <title>convert chinese to unicode</title>
    </head>
    <body style="width:300px;padding-bottom:10px;">
        <h1 style="font-size:18px;text-align:center;">Chinese To Unicode</h1>
        <span style="color:red;">you can also convert unicode to chinese.</span>
        <form action="" id="container">
            <input type="text" id="input" name="userInput"  placeholder=" use | to split more words." style="width:300px;height:30px;margin-top:5px;margin-bottom:10px;"/>
            <div style="text-align:center;margin-bottom:5px;">
                <button type="button" id="ok" style="width:50px;margin-right:5px;">OK</button>
                <button type="button" id="clear" style="width:50px;margin-left:5px;">Clear</button>
            </div>
            <hr id="hr" style="display:none;"/>
            <div id="result" style="height:10px;text-align:left;word-wrap:break-word;font-size:13px;">
                </div>
        </form>
        <script src="./popup.js"></script>
    </body>
</html>
