---
id: version-5.1.0-cors
title: Deal with Cross-Origin Issues
sidebar_label: Deal with Cross-Origin Issues
copyright: (C) 2007-2018 GoodData Corporation
original_id: cors
---

This article explains how to deal with cross-origin issues (CORS).

CORS issues occur in the following scenario: your application runs on your local dev machine [https://localhost:3000](https://localhost:3000/) or on your production domain, but you need it to call the GoodData APIs from [https://secure.gooddata.com/gdc/](https://secure.gooddata.com/gdc/).

Modern browsers do not permit this because of the security measure known as the [same-origin-policy](https://developer.mozilla.org/en-US/docs/Web/Security/Same-origin_policy).

You have to overcome the CORS restriction before you can develop or deploy your application. To do so, use the following methods:

* Use a proxy (recommended for a local dev machine)
* Enable CORS (recommended for a production domain)

## Use a proxy

You can set up a proxy to bypass the CORS restriction on a local dev machine, because making a cross-origin request from a trusted application is safe. The proxy will make the GoodData API accessible under the same hostname and port as your web application, that is, [https://localhost:3000/gdc/](https://localhost:3000/gdc/).

To set up a proxy, add the following section to the root level of your `package.json` \(this works with any application started using `react-scripts start`\):

```javascript
"proxy": {
  "/gdc": {
    "changeOrigin": true,
    "cookieDomainRewrite": "localhost",
    "secure": false,
    "target": "https://secure.gooddata.com/",
    "headers": {
      "host": "secure.gooddata.com",
      "origin": null
    }
  },
  "/*.html": {
    "changeOrigin": true,
    "secure": false,
    "target": "https://secure.gooddata.com/"
  }
},
```

The `/gdc` prefix refers to the GoodData APIs as they are hosted under [https://secure.gooddata.com/gdc](https://secure.gooddata.com/gdc). The `"secure: false"` section allows you to set up a proxy against your localhost server that may use a self-signed certificate.

If you want to connect to the [live examples](https://gooddata-examples.herokuapp.com), set all the target properties to ```https://developer.na.gooddata.com```. The ```workspaceId``` of the demo workspace is ```xms7ga4tf3g3nzucd8380o2bev8oeknp```.

In addition, proxying the `/*.html` pages allows you to easily establish a user session by logging in using the GoodData login page \(account.html\) and possibly invoke other GoodData actions that you may need during the development.

Your `package.json` should now look something like this \(the version numbers may differ\):

```javascript
{
  "name": "my-first-app",
  "version": "0.1.0",
  "private": true,
  "proxy": {
    "/gdc": {
      "changeOrigin": true,
      "cookieDomainRewrite": "localhost",
      "secure": false,
      "target": "https://secure.gooddata.com/",
      "headers": {
        "host": "secure.gooddata.com",
        "origin": null
      }
    },
    "/*.html": {
      "changeOrigin": true,
      "secure": false,
      "target": "https://secure.gooddata.com/"
    }
  },
  "dependencies": {
    "@gooddata/react-components": "^5.0.1",
    "react": "15.6.2",
    "react-dom": "15.6.2",
    "react-scripts": "1.1.1"
  },
  "scripts": {
    "start": "react-scripts start",
    "build": "react-scripts build",
    "test": "react-scripts test --env=jsdom",
    "eject": "react-scripts eject"
  }
}
```

## Enable CORS

Setting up CORS allows you to develop and run web applications that can communicate directly with the GoodData APIs.

This section does **not** address authentication. The easiest way to make sure that your API requests to the GoodData platform are authenticated is to be logged into your white-labelled domain in the same browser you are using for your local development.

### Step 1. Get a white-labeled GoodData domain

By default, you access the GoodData Portal via `https://secure.gooddata.com`. If you white-label the GoodData Portal URL, you can have it at, for example, `https://analytics.example.com`.

In general, a white-labeled domain enables you to remove branding elements from the GoodData Portal and optionally replace them with branding from your enterprise. For more information, see [White Labeling](https://help.gooddata.com/display/doc/White+Labeling).

White-labeling is done by the GoodData Support specialists per request submitted via the [GoodData Support Portal](https://support.gooddata.com/hc/en-us).

You can white-label a brand new domain \(see [White-Label a New Domain](https://help.gooddata.com/display/doc/White-Label+a+New+Domain)\) or an existing domain \([White-Label an Existing Domain](https://help.gooddata.com/display/doc/White-Label+an+Existing+Domain)\).

### Step 2. Configure CORS

To request CORS setup, submit a request via the [GoodData Support Portal](https://support.gooddata.com/hc/en-us). In your request, provide the following:

* The name of your GoodData domain.
* (Optional, but recommended) URLs to enable API calls from a local machine during development. For example, `https://local.test:8443` \(you have to set up
  `local.test` as an alias for `localhost` because using the actual localhost is not allowed\).

**Example:**
If your white-labelled GoodData domain is `analytics.example.com` and your app is expected to be hosted at `https://smart-app.example.com/`, you can request CORS by submitting the following request:

```
To: support@gooddata.com

Subject: CORS origin setup for analytics.example.com

Hello GoodData Support team, could you please enable https://smart-app.example.com and https://local.test:8443 as CORS origins for the GoodData domain associated with https://analytics.example.com/ ?

Thank you.
```

### Step 3. Update your code

Add the GoodData.UI factory calls to your executable code. To keep your code clean, you can split it into two files: `config.js` and `mycomponent.js`.

`config.js`

```javascript
import { factory as sdkFactory } from '@gooddata/gooddata-js';
const sdk = sdkFactory({ domain: 'https://analytics.example.com' });
export default {
projectId: '<workspace-id>',
sdk
};
```

`mycomponent.js`

```javascript
import config from './config';
...
<Visualization
identifier="<some-identifier>"
{...config}
/>
```

**NOTE:** If you followed the instructions from the tutorial [How to Create Your First Application with GoodData UI SDK](02_start__no_boilerplate.md), you can now remove the proxy-related code from the `package.json` file because it is not needed anymore.
