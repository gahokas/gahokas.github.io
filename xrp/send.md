---
layout: xrptools
title: XRP Wallet Web Tools
---
   
<script src="https://unpkg.com/ripple-lib@1.10.0/build/ripple-latest-min.js"></script>

<div class="container-sm3" id="transactionStep1">
    <form onsubmit="setOutput('');
        prepareTransaction(document.getElementById('XRPAmountId').value, document.getElementById('sourceAddressId').value, document.getElementById('destinationAddressId').value, document.getElementById('destinationTagId').value, xrplServer.value);
        transactionStep2();
        return false;">
        <div class="row g-3">
            <div class="col-auto">
                <input type="text" readonly class="form-control-plaintext" value="XRP">
            </div>
            <div class="col-sm">
                <input type="text" class="form-control" id="XRPAmountId" placeholder="amount of XRP">
            </div>
            <div class="w-100"></div>
            <div class="col-auto">
                <input type="text" readonly class="form-control-plaintext" value="Source Address">
            </div>
            <div class="col-sm">
                <input type="text" class="form-control" id="sourceAddressId" placeholder="">
            </div>
            <div class="w-100"></div>
            <div class="col-auto">
                <input type="text" readonly class="form-control-plaintext" value="Destination Address">
            </div>
            <div class="col-sm">
                <input type="text" class="form-control" id="destinationAddressId" placeholder="">
            </div>
            <div class="w-100"></div>
            <div class="col-auto">
                <input type="text" readonly class="form-control-plaintext" value="Destination Tag">
            </div>
            <div class="col-sm">
                <input type="text" class="form-control" id="destinationTagId" placeholder="(if required)">
            </div>
            <div class="w-100"></div>
            <div class="col-auto">
                <input type="text" readonly class="form-control-plaintext" value="">
            </div>
            <div class="col-sm">
                <button type="submit" class="btn btn-primary mb-3">Prepare Transaction</button>
            </div>
        </div>
    </form>
</div>

<div class="container-sm transactionStep" id="transactionStep2" style="display: none">
    <form onsubmit="setOutput('');
        signTransaction(preparedTransactionJSON, document.getElementById('secretId').value, xrplServer.value);
        transactionStep3();
        return false;">
        <div class="row g-3">
            <div class="col-auto">
                <input type="text" readonly class="form-control-plaintext" value="Secret">
            </div>
            <div class="col-sm">
                <input type="text" class="form-control" id="secretId" placeholder="">
            </div>
            <div class="w-100"></div>
            <div class="col-auto">
                <input type="text" readonly class="form-control-plaintext" value="">
            </div>
            <div class="col-sm">
                <button type="submit" class="btn btn-primary mb-3">Sign Transaction</button>
            </div>
        </div>
    </form>
</div>

<div class="container-sm transactionStep" id="transactionStep3" style="display: none">
    <form onsubmit="setOutput('');
        submitTransaction(signedTransactionBlob, xrplServer.value);
        transactionStep4();
        return false;">
        <div class="row g-3">
            <div class="col-sm">
                <button type="submit" class="btn btn-primary mb-3">Submit Transaction</button>
            </div>
        </div>
    </form>
</div>

<div class="container-sm transactionStep mt-3" id="transactionStep4" style="display: none">
    <div class="alert alert-success" role="alert">
        Transaction submitted!
    </div>
</div>

<div class="container sm mt-3">
    <pre>
        <samp id="results">
        </samp>
    </pre>
</div>

<script>

var preparedTransactionJSON;
var signedTransactionBlob;

function transactionStep2() {
    document.getElementById("transactionStep1").setAttribute('style', 'display: none');
    document.getElementById("transactionStep2").setAttribute('style', 'display: block');
}

function transactionStep3() {
    document.getElementById("transactionStep2").setAttribute('style', 'display: none');
    document.getElementById("transactionStep3").setAttribute('style', 'display: block');
}

function transactionStep4() {
    document.getElementById("transactionStep3").setAttribute('style', 'display: none');
    document.getElementById("transactionStep4").setAttribute('style', 'display: block');
}


function prepareTransaction(amountXRP, sourceAddress, destinationAddress, destinationTag, serverWebSocketURL) {
    const api = new ripple.RippleAPI({
        server: serverWebSocketURL
    });

    const transaction = {
        "TransactionType": "Payment",
        "Account": sourceAddress,
        "Amount": api.xrpToDrops(amountXRP),
        "Destination": destinationAddress,
    };

    if (isNumeric(destinationTag)) {
        transaction.DestinationTag = parseInt(destinationTag);
    }

    const transactionExpiry = {
        // Expire this transaction if it doesn't execute within ~5 minutes:
        "maxLedgerVersionOffset": 75
    }

    api.connect().then(() => {
        try {
            return api.prepareTransaction(transaction, transactionExpiry);
        } catch (error) {
            console.log(error);
            return 'failed';
        }
    }).then((result) => {
        console.log(result)
        setOutput(JSON.stringify(result, null, ' ').replace(/[{}]/g,''));
        preparedTransactionJSON = result.txJSON;
        return api.disconnect();
    }).catch((error) => {
        console.log(error);
        setErrorMessage(error.message);
    });
}

function signTransaction(txJson, secret, serverWebSocketURL) {
    const api = new ripple.RippleAPI({
        server: serverWebSocketURL
    });

    api.connect().then(() => {
        try {
            return api.sign(txJson, secret);
        } catch (error) {
            console.log(error);
            return 'failed';
        }
    }).then((result) => {
        console.log(result)
        signedTransactionBlob = result.signedTransaction;
        setOutput(JSON.stringify(result, null, ' ').replace(/[{}]/g,''));
        return api.disconnect();
    }).catch((error) => {
        console.log(error);
        setErrorMessage(error.message);
    });
}

function submitTransaction(tx_blob, serverWebSocketURL) {
    const api = new ripple.RippleAPI({
        server: serverWebSocketURL
    });

    api.connect().then(() => {
        try {
            return api.submit(tx_blob);
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
    document.getElementById('results').innerHTML = output;
}

function isNumeric(str) {
  if (typeof str != "string") return false // we only process strings!  
  return !isNaN(str) && // use type coercion to parse the _entirety_ of the string (`parseFloat` alone does not do this)...
         !isNaN(parseInt(str)) // ...and ensure strings of whitespace fail
}

</script>
