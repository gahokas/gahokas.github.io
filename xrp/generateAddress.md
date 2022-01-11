---
layout: xrptools
---
   
<script src="https://unpkg.com/ripple-lib@1.10.0/build/ripple-latest-min.js"></script>

<div class="container-sm mt-3">
    <form class="row g-3" onsubmit="setOutput(''); generateAddress(xrplServer.value); return false;">
        <div class="col-auto">
            <button type="submit" class="btn btn-primary mb-3">Generate Address</button>
        </div>
    </form>
</div>

<div class="container sm mt-3">
    <pre>
        <samp id="results">
        </samp>
    </pre>
</div>

<script>

function generateAddress(serverWebSocketURL) {
    const api = new ripple.RippleAPI({
        // server: 'wss://s1.ripple.com'
        // server: 'wss://s.altnet.rippletest.net'
        server: serverWebSocketURL
    });

    let testAddress = serverWebSocketURL.includes("test");

    api.connect().then(() => {
        try {
            return api.generateXAddress({test: testAddress, includeClassicAddress: true});
        } catch (error) {
            console.log(error);
            return 'failed';
        }
    }).then((result) => {
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
        document.getElementById('results').innerHTML = output;
    }
</script>
