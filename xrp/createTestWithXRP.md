---
layout: xrptools
---
   
<script src="https://unpkg.com/ripple-lib@1.10.0/build/ripple-latest-min.js"></script>

<div class="container-sm mt-3">
    <form class="row g-3" onsubmit="setOutput(''); generateTestAddressWithXRP(); return false;">
        <div class="col-auto">
            <button type="submit" class="btn btn-primary mb-3">Create Test Address with 1000 XRP</button>
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

async function postData(url = '', data = {}) {
  // Default options are marked with *
  const response = await fetch(url, {
    method: 'POST', // *GET, POST, PUT, DELETE, etc.
    // mode: 'no-cors', // no-cors, *cors, same-origin
    // cache: 'no-cache', // *default, no-cache, reload, force-cache, only-if-cached
    // credentials: 'same-origin', // include, *same-origin, omit
    // headers: {
    //     'Accept': 'application/json',
    //     'Content-Type': 'application/json'
    //           // 'Content-Type': 'application/x-www-form-urlencoded',
    // },
    // redirect: 'follow', // manual, *follow, error
    // referrerPolicy: 'no-referrer', // no-referrer, *no-referrer-when-downgrade, origin, origin-when-cross-origin, same-origin, strict-origin, strict-origin-when-cross-origin, unsafe-url
    // body: JSON.stringify(data) // body data type must match "Content-Type" header
  });
  return response.json(); // parses JSON response into native JavaScript objects
}

function generateTestAddressWithXRP() {
    postData('https://faucet.altnet.rippletest.net/accounts')
    .then(data => {
        console.log(data); // JSON data parsed by `data.json()` call
        setOutput(JSON.stringify(data, null, ' ').replace(/[{}]/g,""));
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
