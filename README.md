<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <meta name="robots" content="noindex, nofollow">
    <title>Password Protected Page</title>
    <style>
        html, body {
            margin: 0;
            width: 100%;
            height: 100%;
            font-family: Arial, Helvetica, sans-serif;
        }
        #dialogText {
            color: white;
            background-color: #333333;
        }
        
        #dialogWrap {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            display: table;
            background-color: #EEEEEE;
        }
        
        #dialogWrapCell {
            display: table-cell;
            text-align: center;
            vertical-align: middle;
        }
        
        #mainDialog {
            max-width: 400px;
            margin: 5px;
            border: solid #AAAAAA 1px;
            border-radius: 10px;
            box-shadow: 3px 3px 5px 3px #AAAAAA;
            margin-left: auto;
            margin-right: auto;
            background-color: #FFFFFF;
            overflow: hidden;
            text-align: left;
        }
        #mainDialog > * {
            padding: 10px 30px;
        }
        #passArea {
            padding: 20px 30px;
            background-color: white;
        }
        #passArea > * {
            margin: 5px auto;
        }
        #pass {
            width: 100%;
            height: 40px;
            font-size: 30px;
        }
        
        #messageWrapper {
            float: left;
            vertical-align: middle;
            line-height: 30px;
        }
        
        .notifyText {
            display: none;
        }
        
        #invalidPass {
            color: red;
        }
        
        #success {
            color: green;
        }
        
        #submitPass {
            font-size: 20px;
            border-radius: 5px;
            background-color: #E7E7E7;
            border: solid gray 1px;
            float: right;
            cursor: pointer;
        }
        #contentFrame {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
        }
        #attribution {
            position: absolute;
            bottom: 0;
            left: 0;
            right: 0;
            text-align: center;
            padding: 10px;
            font-weight: bold;
            font-size: 0.8em;
        }
        #attribution, #attribution a {
            color: #999;
        }
        .error {
            display: none;
            color: red;
        }
    </style>
  </head>
  <body>
    <iframe id="contentFrame" frameBorder="0" allowfullscreen></iframe>
    <div id="dialogWrap">
        <div id="dialogWrapCell">
            <div id="mainDialog">
                <div id="dialogText">This page is password protected.</div>
                <div id="passArea">
                    <p id="passwordPrompt">Password</p>
                    <input id="pass" type="password" name="pass" autofocus>
                    <div>
                        <span id="messageWrapper">
                            <span id="invalidPass" class="error">Sorry, please try again.</span>
                            <span id="trycatcherror" class="error">Sorry, something went wrong.</span>
                            <span id="success" class="notifyText">Success!</span>
                            &nbsp;
                        </span>
                        <button id="submitPass" type="button">Submit</button>
                        <div style="clear: both;"></div>
                    </div>
                </div>
                <div id="securecontext" class="error">
                    <p>
                        Sorry, but password protection only works over a secure connection. Please load this page via HTTPS.
                    </p>
                </div>
                <div id="nocrypto" class="error">
                    <p>
                        Your web browser appears to be outdated. Please visit this page using a modern browser.
                    </p>
                </div>
            </div>
        </div>
    </div>
    <div id="attribution">
        Protected by <a href="https://www.maxlaumeister.com/pagecrypt/">PageCrypt</a>
    </div>
    <script>
    (function() {

        var pl = "Qely6H2hGrAh6A06hgLJFyW4Wzu056Fm65Viy9u9kRBIZ3c8FV3MBgMWtBnLQ5+fST0HwCL2lWO2OKXLL+EEYb8UIOoMCaXwh0ucyoBQ8rqKvbXPTGnnIe1o5ix2kUWyfqxHSm6MSafrj1L8haaxUGlCblGHavTVST916fSeZ1V/mrrUkWaYi48hybpae/bgWlQlZghKzy5P1EKdEXd0nOctezxU8TTBWdkX1F0BVKK60PSep11UFs7vV3UL9TW5COVAR4DfT3gpsxNkJ2A+BlS2RjTXrXnbthZ8Xh+tKFdagvbj5l5+HuwOWBdgt1xlI/GG9FmPpwxZu9OryWAlAH0p89Fw+TmfC+RuVhU2Gim8HH9xeEAyJ7LQjGB6Q0c6gw5SEHXUadKA/52Ot1Rt62cW8NsGac9QvmE0Yf1xEbtYM401La36phuXvwLRyMRGT3NiDNgMxlwG5rf987Sy16u4FZjvPuQOffb5Iy+FS6oynlihQS3A/OEcH1RM3pX8G4r/jAI9IGboYb8gxdZUos53o/izQX8dR+FY5OTwvdwfA0/yzRE31NMYrDm1n6N80MOIXSl1G63KO7hnIBoKVFjFXxaXiGec03oqXBkEQs/J6z/n5xjtRbM8kGhHFs0DP7CWXPpt8ZpyEgPsK8eoyRFkPXrb10TSqA+aWcqsGbjKPLgNWDqx8rGVK9IcNsxYn9AXQTbPaTfxK0nVIXvXL6P+z2DbHKEav4TMKnv7cBftgyE7J8S3AA6JKntDcP0sm8V+Ws57fftsm09oHoSrrkWO1709eKk9mcnsvMVx/OPpeZt0SbYKd3eY4rZkSptyYF8q9ENn4kQhqPV3JrOwFRPLM3Oj7BGgwj174igYznKBoMGVe6P1PVcpcKJGKmDYR0iCHdiQf6uFlPdsa209YWwRglCYqZ8hQELjrFGGVRna4m2yLFbyzHXlVfK5ZTPCrX7QIgl6EY1I6UM9z2yEdtN+ZeqELtWUr0tqmG/kYACUqQJD6GAWgCpbwFcVwCAWKFTuMEug6qeer5sNr7zkpfUhbHl3T8zEgpGzK4DKO3fmcBVmo4dYjm/4aQSEYggXiGmMOBguXg2ekxGvhbcaEp5G7j4+p+/aJE7Yd2is1jVUQOuW1X9dtBhFtq5lDrPJOig01alJ80eB5jccoNY7RgSCpzkSXqMNoRKre5hG0KzSlI0fTyAz6JBgLCrcBd893ASvpYAx2uqAVhYrKtvD4Te1ZB5KaQXBzDFlYqTFxaoEMm+wTRYSRi9GqpIxZVgZEe1ylOKolMeoFcHp6qhFSiW4ElXkGpcKTQU6EjB7qKiJcrPcwEuIJkk3mtw4queA/VR1QuvaQio4qvY5qOrq1JJSeBCYu1eSWjlibo84qT6x/TELvHfUJVmTvCQCWwx9drzge585sL0+cwCBwI/Bq9nPLNE5KpFBNPSdqJ9RXSGgRketvZrH8dz57FXy7o5eGX8esuR319bUg2x6gzrBOGJkCb1D2Wi/i6TmUHk/UxZaJCNpg/Ef9/VoZ67KGExisVtXmWe5pN2Qhm9odtxvtlJj2z6s9y1O/r5hWzfgnuiyFL+ahh5JUSl+x7geSE+tlgWM+dHV3CoCbf1aejekaeVqnRhBJwS2S0v5eiL5srHB7hRTc4mD5GIUDhEieQcafM3jQUrHhiiLdjrhjUPUqU3qJMvICQIBymBfIvewZeJGah0RbmCT9JZEuxBe0L3UInC5BjueGPfFp8eM5cBo4p9Z+TBGWnweXyUuag4w6zBTG+Q55xUaLJ8c2N2pLFSLdWi0JY2hE4R0Q8cq1lFqwrDf1ELjXpjj/mJzoO5OZdwcLxkwrLX4iMe+b2VlVglcQjY=";
        
        var submitPass = document.getElementById('submitPass');
        var passEl = document.getElementById('pass');
        var invalidPassEl = document.getElementById('invalidPass');
        var trycatcherror = document.getElementById('trycatcherror');
        var successEl = document.getElementById('success');
        var contentFrame = document.getElementById('contentFrame');
        
        // Sanity checks

        if (pl === "") {
            submitPass.disabled = true;
            passEl.disabled = true;
            alert("This page is meant to be used with the encryption tool. It doesn't work standalone.");
            return;
        }

        if (!isSecureContext) {
            document.querySelector("#passArea").style.display = "none";
            document.querySelector("#securecontext").style.display = "block";
            return;
        }

        if (!crypto.subtle) {
            document.querySelector("#passArea").style.display = "none";
            document.querySelector("#nocrypto").style.display = "block";
            return;
        }
        
        function str2ab(str) {
            var ustr = atob(str);
            var buf = new ArrayBuffer(ustr.length);
            var bufView = new Uint8Array(buf);
            for (var i=0, strLen=ustr.length; i < strLen; i++) {
                bufView[i] = ustr.charCodeAt(i);
            }
            return bufView;
        }

        async function deriveKey(salt, password) {
            const encoder = new TextEncoder()
            const baseKey = await crypto.subtle.importKey(
                'raw',
                encoder.encode(password),
                'PBKDF2',
                false,
                ['deriveKey'],
            )
            return await crypto.subtle.deriveKey(
                { name: 'PBKDF2', salt, iterations: 100000, hash: 'SHA-256' },
                baseKey,
                { name: 'AES-GCM', length: 256 },
                true,
                ['decrypt'],
            )
        }
        
        async function doSubmit(evt) {
            submitPass.disabled = true;
            passEl.disabled = true;

            let iv, ciphertext, key;
            
            try {
                var unencodedPl = str2ab(pl);

                const salt = unencodedPl.slice(0, 32)
                iv = unencodedPl.slice(32, 32 + 16)
                ciphertext = unencodedPl.slice(32 + 16)

                key = await deriveKey(salt, passEl.value);
            } catch (e) {
                trycatcherror.style.display = "inline";
                console.error(e);
                return;
            }

            try {
                const decryptedArray = new Uint8Array(
                    await crypto.subtle.decrypt({ name: 'AES-GCM', iv }, key, ciphertext)
                );

                let decrypted = new TextDecoder().decode(decryptedArray);

                if (decrypted === "") throw "No data returned";

                const basestr = '<base href="." target="_top">';
                const anchorfixstr = `
                    <script>
                        Array.from(document.links).forEach((anchor) => {
                            const href = anchor.getAttribute("href");
                            if (href.startsWith("#")) {
                                anchor.addEventListener("click", function(e) {
                                    e.preventDefault();
                                    const targetId = this.getAttribute("href").substring(1);
                                    const targetEl = document.getElementById(targetId);
                                    targetEl.scrollIntoView();
                                });
                            }
                        });
                    <\/script>
                `;
                
                // Set default iframe link targets to _top so all links break out of the iframe
                if (decrypted.includes("<head>")) decrypted = decrypted.replace("<head>", "<head>" + basestr);
                else if (decrypted.includes("<!DOCTYPE html>")) decrypted = decrypted.replace("<!DOCTYPE html>", "<!DOCTYPE html>" + basestr);
                else decrypted = basestr + decrypted;

                // Fix fragment links
                if (decrypted.includes("</body>")) decrypted = decrypted.replace("</body>", anchorfixstr + '</body>');
                else if (decrypted.includes("</html>")) decrypted = decrypted.replace("</html>", anchorfixstr + '</html>');
                else decrypted = decrypted + anchorfixstr;
                
                contentFrame.srcdoc = decrypted;
                
                successEl.style.display = "inline";
                setTimeout(function() {
                    dialogWrap.style.display = "none";
                }, 1000);
            } catch (e) {
                invalidPassEl.style.display = "inline";
                passEl.value = "";
                submitPass.disabled = false;
                passEl.disabled = false;
                console.error(e);
                return;
            }
        }
        
        submitPass.onclick = doSubmit;
        passEl.onkeypress = function(e){
            if (!e) e = window.event;
            var keyCode = e.keyCode || e.which;
            invalidPassEl.style.display = "none";
            if (keyCode == '13'){
              // Enter pressed
              doSubmit();
              return false;
            }
        }
    })();
    </script>
  </body>
</html>
