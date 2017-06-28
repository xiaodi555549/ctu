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
## popup.js
        (function(){
            var inputObj = document.getElementById('input');
            var hrObj = document.getElementById('hr');
            var resObj = document.getElementById('result');

            init();

            /**
             *程序初始化,并添加事件处理
             */
            function init()
            {
                var okObj = document.getElementById('ok');
                var clearObj = document.getElementById('clear');
                okObj.onclick = convert;
                clearObj.onclick = clear;
            }

            /**
             * 执行转换,并支持批量转换，多个数据项之间用'|'隔开
             */
            function convert()
            {
                var str = inputObj.value;
                if (str.trim() === '')
                {
                    var msg = '<span style="color:#CA4036;">Input can\'t be empty.</span>';
                    hrObj.style.display = 'block';
                    resObj.innerHTML = msg;
                    return;
                }

                var strArray = str.split('|');
                var results = strArray.map(function(str){
                    if(str.indexOf('\\u') === -1) return chineseToUnicode(str);
                    else return unicodeToChinese(str);
                    
                });
                var html = buildResultsHtml(results);

                hrObj.style.display = 'block';
                resObj.innerHTML = html;
            }

            /**
             * 中文转换为unicode字符 
             */
            function chineseToUnicode(str)
            {
                var temp = [];
                var obj = {};
                for(var i=0; i < str.length; i++)
                {
                    var number = str.charCodeAt(i);
                    var hex = number.toString(16);
                    temp.push(hex);
                }
                var result = '\\u' + temp.join('\\u');
                obj[str] = result;
                return obj;
            }


            /**
             * unicode 字符转为中文
             */
            function unicodeToChinese(str)
            {
                var temp = [];
                var obj = {};
                var numbers= str.split('\\u');
                numbers.shift();
                for(var i=0; i < numbers.length; i++)
                {
                    chinese = String.fromCharCode('0x' + numbers[i]);
                    temp.push(chinese);
                }
                var result =  temp.join('');
                obj[str] = result;
                return obj;
            }

            /**
             * 格式化转换结果
             */
            function buildResultsHtml(results)
            {
                var html = '';
                var strArray = results.map(function(item){
                    for (var key in item)
                    {
                        return '<p>' + key + ': <span style="color:#CA4036;">' + item[key] + '</span></p>';
                    }
                });

                return strArray.join('');
            }

            /**
             * 清除转换结果
             */
            function clear()
            {
                hrObj.style.display = 'none';
                inputObj.value = '';
                resObj.innerHTML = '';
            }

        })();
## wechat.png
