---
layout: xrptools
---
   
<script src="https://unpkg.com/ripple-lib@1.10.0/build/ripple-latest-min.js"></script>

<div class="container-sm mt-3">
    <form class="row g-3" onsubmit="setOutput(''); getTransaction(document.getElementById('transactionID').value, xrplServer.value); return false;">
        <div class="col-auto">
            <input type="text" readonly class="form-control-plaintext" id="staticTransactionID" value="Transaction ID">
        </div>
        <div class="col-sm">
            <input type="text" class="form-control" id="transactionID" placeholder="">
        </div>
        <div class="col-auto">
            <button type="submit" class="btn btn-primary mb-3">Transaction Info</button>
        </div>
    </form>
</div>

<div class="container sm mt-3">
    <pre>
        <samp id="transactionIDResults">
        </samp>
    </pre>
</div>

<script>

    function getTransaction(transactionID, serverWebSocketURL) {
        const api = new ripple.RippleAPI({
            // server: 'wss://s1.ripple.com'
            // server: 'wss://s.altnet.rippletest.net'
            server: serverWebSocketURL
        });

        api.connect().then(() => {
            try {
                return api.getTransaction(transactionID);
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
        document.getElementById('transactionIDResults').innerHTML = output;
    }
</script>
