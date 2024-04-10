# omadaportalxarclemwifi
how to setup your omada portal to work with arclem wifi app

Download this app for your customers
https://play.google.com/store/apps/details?id=com.arclemwifi.captiveportal.arclemwifi

this app will act as database for the vouchers that was logged in to omada.

# To embed this capability, open ung index.js file and add the following
this will be placed on the top part of index js ( please see index.js file )
```
var isFlutterInAppWebViewReady = false;
window.addEventListener("flutterInAppWebViewPlatformReady", function(event) {
 isFlutterInAppWebViewReady = true;
 window.flutter_inappwebview
    .callHandler('useVoucher')
    .then( function (result) {
        if (result.username != 'none') {
            document.getElementById("voucherCode").value = result.username;
            setTimeout(function(){
                handleSubmit();
            }, 1000);
        }
    });
});

function sendStatus(logged_in, ip, mac, username, session_time_left, uptime) {
    if (isFlutterInAppWebViewReady) {
        window.flutter_inappwebview.callHandler('userStatus', logged_in, ip, mac, username, session_time_left, uptime);
    }
}

```

then find the doAuth function in the handleSubmit function and add this line 
```
     function doAuth () {
                    Ajax.post(submitUrl, JSON.stringify(submitData).toString(), function(data){
                        data = JSON.parse(data);
                        if(!!data && data.errorCode === 0) {
                            isCommited = true;
                            // add this line below
                            sendStatus("True", submitData['apMac'] ,submitData['clientMac'],submitData['voucherCode'], "None", "None");
                      
                            setTimeout(function(){
                                window.location.href = landingUrl;
                            }, 500);
                           
                            document.getElementById("oper-hint").innerHTML = errorHintMap[data.errorCode];
                        } else{
                            document.getElementById("oper-hint").innerHTML = errorHintMap[data.errorCode];
                        }
                    });
                }
                doAuth();
```

