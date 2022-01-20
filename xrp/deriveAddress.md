---
layout: xrptools
title: XRP Wallet Web Tools
---

<script src="https://unpkg.com/ripple-lib@1.10.0/build/ripple-latest-min.js"></script>

<div class="container-sm mt-3">
    <form class="row g-3" onsubmit="setOutput(''); deriveAddress(document.getElementById('secretInput').value); return false;">
        <div class="col-auto">
            <input type="text" readonly class="form-control-plaintext" id="secret" value="Secret">
        </div>
        <div class="col-sm">
            <input type="text" class="form-control" id="secretInput" placeholder="">
        </div>
        <div class="col-auto">
            <button type="submit" class="btn btn-primary mb-3">Derive Address</button>
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
    function deriveAddress(account) {
        const api = new ripple.RippleAPI();

        try {
            const keypair = api.deriveKeypair(account);
            const address = api.deriveAddress(keypair.publicKey);
            setOutput("Address: " + address);
        } catch (error) {
            console.log(error);
            setErrorMessage(error.message);
        }
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
