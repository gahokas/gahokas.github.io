---
layout: xrptools
title: XRP Web Wallet and Tools
---
    
<script src="https://unpkg.com/ripple-lib@1.10.0/build/ripple-latest-min.js"></script>

<div class="container-sm mt-3">
    <form class="row g-3" onsubmit="setOutput(''); getAccountInfoAPI(document.getElementById('xrpAddress').value, xrplServer.value); return false;">
        <div class="col-auto">
            <input type="text" readonly class="form-control-plaintext" id="staticXRPAddress" value="XRP Address">
        </div>
        <div class="col-sm">
            <input type="text" class="form-control" id="xrpAddress" placeholder="">
        </div>
        <div class="col-auto">
            <button type="submit" class="btn btn-primary mb-3">Account Info</button>
        </div>
    </form>
</div>

<div class="container sm mt-3">
    <pre>
        <samp id="accountInfoResults">
        </samp>
    </pre>
</div>

<script>
    function getAccountInfoAPI(account, serverWebSocketURL) {
        const api = new ripple.RippleAPI({
            // server: 'wss://s1.ripple.com'
            // server: 'wss://s.altnet.rippletest.net'
            server: serverWebSocketURL
        });

        api.connect().then(() => {
            try {
                return api.getAccountInfo(account);
            } catch (error) {
                console.log(error);
                return 'failed';
            }
        }).then((result) => {
            console.log(result)
            setOutput(JSON.stringify(result, null, ' ').replace(/[{}]/g,''));
            return api.disconnect();
        }).catch((error) => {
            console.log(error);
            setErrorMessage(error.message);
        });
    }

    function setErrorMessage(message) {
        if (message.includes('instance.address is not exactly one from')) {
            setOutput("Invalid address");
        }
        else {
            setOutput(message);
        }
    }

    function setOutput(output) {
        document.getElementById('accountInfoResults').innerHTML = output;
    }
</script>
