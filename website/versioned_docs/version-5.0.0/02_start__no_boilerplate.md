---
title: Create Your First Application
sidebar_label: Create Your First Application
copyright: (C) 2007-2018 GoodData Corporation
id: version-5.0.0-ht_create_your_first_visualization
original_id: ht_create_your_first_visualization
---

This tutorial will guide you through the process of creating your first analytical application using GoodData.UI with the Facebook’s `create-react-app` tool.

After you complete this tutorial, you will be able to display various measures and charts from your GoodData workspace within the context of your React application.

**NOTE:** Before you start this tutorial, make sure that you have a GoodData account (see [About GoodData.UI](01_intro__about_gooddataui.md#supported-technologies)).

## Step 1. Get create-react-app
Run the following command from the command line:
```bash
yarn global add create-react-app
```
This command installs the `create-react-app` tool that will help you create a functional skeleton of a React application. The current version of `create-react-app` installs React 16.

## Step 2. Create your React application
1. Run the following command from the command line:
    ```bash
    create-react-app my-first-app
    ```
    This command creates a directory named `my-first-app` with a functional skeleton of a React application. You can see the command output that looks something like the following:
    ```bash
    Success! Created my-first-app at /Users/user-name/dev/my-first-app
    ```

2. Change your current working directory to `my-first-app` (for example, by running `cd my-first-app` on Mac or Linux).

3. Downgrade your version of React to 15.6.2. This is because GoodData.UI requires React 15.6.2 and the current version of `create-react-app` installs React 16. To downgrade, run the following command:
    ```bash
    yarn install
    yarn upgrade react@15.6.2 react-dom@15.6.2
    ```

## Step 3. Add GoodData SDK dependencies
Run the following command from the command line:
```bash
yarn add @gooddata/react-components
```
This command adds the latest `@gooddata/react-components` to the list of your project dependencies in `package.json` and downloads all required packages.


## Step 4. Start the development server
**Before** you start your development server, prevent cross-origin issues by [adding proxy settings](30_tips__cors.md).

> **Careful:** Only use the proxy authentication for development. Do not use this authentication method for production.

> **Careful:** If you are only using the [development proxy](30_tips__cors.md#on-your-local-dev-machine), you will still need to autenticate users by going to `/account.html` or by calling the `sdk.user.login()` method, and filling in valid GoodData credentials.

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

* If you are on Windows and using Microsoft Edge or Internet Explorer:
    Set `cookieDomainRewrite` to the IP address on which your local web server runs, for example:
    ```javascript
    "cookieDomainRewrite": "127.0.0.1"
    ```
    You can get your IP address from the console output after the server started.

> Switch `target` and `host` to ``https://developer.na.gooddata.com`` when using the [live examples](https://gooddata-examples.herokuapp.com/).

Start the server by running the following command:

* If you are on Mac or Linux:
    ```bash
    HTTPS=true yarn start
    ```
* If you are on Windows:
    ```bash
    set HTTPS=true&&npm start
    ```

**Always** run your local development server using HTTP**S** because the GoodData API responses set cookies with the `secure` flag. Specifically, it means that if you skip the `HTTPS=true` part, you will not be able to log in.

## Step 5. Establish a session
Open [https://localhost:3000/account.html](https://localhost:3000/account.html) in your browser, and enter your GoodData credentials. You are now logged in to GoodData.

**NOTE:**
* If you are on Windows and using Microsoft Edge or Internet Explorer, replace `localhost` in this URI with the IP address that you get after running the server.
* If you see a warning about an insecure connection due to using a self-signed certificate, accept the exception. You can trust your localhost.

For the purpose of this tutorial, you are asked to establish a client session by simply logging in to GoodData.

In your production environment, your end users may be authenticated using [single sign-on](30_tips__sso.md).

## Step 6. Add GoodData components
Open [https://localhost:3000/](https://localhost:3000/) in your browser.
The default page generated by the create-react-app tool is displayed and looks like the following example:

![screenshot "Welcome to React"](assets/Welcome_to_React.png)

Now, you can start adding your first GoodData component:
1. Open `src/App.js` in a text editor.
2. Add the following line to the other `import`s at the beginning of the `App.js` file:
    ```javascript
    import { LineChart } from '@gooddata/react-components';
    ```
3. Add the following line to the other `import`s at the beginning of the `App.js` file to load CSS:
    ```javascript
    import '@gooddata/react-components/styles/css/main.css';
    ```
4. Add a simple line chart by appending the following lines in the `render()` method:
    ```javascript
    <div style={{ height: 300 }}>
      <LineChart
          projectId='xms7ga4tf3g3nzucd8380o2bev8oeknp'
          measures={measures}
          trendBy={attribute}
          config={{
              colors: ['#14b2e2']
          }}
      />
    </div>
    ```
    > This example uses the workspace ID from the [live examples](https://gooddata-examples.herokuapp.com/). If you want to use this code in your workspace, replace the properties with the appropriate values from your workspace. For more details, see [Line Chart](10_vis__line_chart_component.md).
5. Save the changes. The content of your `App.js` file should now look something like the following example:
    ```javascript
    import React, { Component } from 'react';
    import { LineChart } from '@gooddata/react-components';
    import '@gooddata/react-components/styles/css/main.css';

    import logo from './logo.svg';
    import './App.css';

    const measures = [
        {
            measure: {
                localIdentifier: 'franchiseFeesIdentifier',
                definition: {
                    measureDefinition: {
                        item: {
                            identifier: 'aaEGaXAEgB7U'
                        }
                    }
                },
                format: '#,##0'
            }
        }
    ];

    const attribute = {
        visualizationAttribute: {
            displayForm: {
                identifier: 'date.abm81lMifn6q'
            },
            localIdentifier: 'month'
        }
    };

    class App extends Component {
       render() {
          return (
             <div className="App">
                <div className="App-header">
                   <img src={logo} className="App-logo" alt="logo" />
                   <h2>Welcome to React</h2>
                </div>
                <div style={{ height: 300 }}>
                   <LineChart
                      projectId='xms7ga4tf3g3nzucd8380o2bev8oeknp'
                      measures={measures}
                      trendBy={attribute}
                      config={{
                          colors: ['#14b2e2']
                      }}
                  />
                </div>
                <p className="App-intro">
                   To get started, edit <code>src/App.js</code> and save to reload.
                </p>
             </div>
          );
       }
    }

    export default App;
    ```
6. Return to your browser window. The default page now looks like the following:

![Welcome to GoodData components](assets/Welcome_to_React_GoodData_component.png)

## Step 7. Keep your code clean
GoodData.UI provides a tool named [gdc-catalog-export](02_start__catalog_export.md) that can help you keep the list of object identifiers organized in a Javascript file within your application.

For example, see the following component used in this tutorial:

```javascript
const measures = [
    {
        measure: {
            localIdentifier: 'franchiseFeesIdentifier',
            definition: {
                measureDefinition: {
                    item: {
                        identifier: 'aaEGaXAEgB7U'
                    }
                }
            },
            format: '#,##0'
        }
    }
];

<div style={{ height: 300 }}>
  <LineChart
      projectId='xms7ga4tf3g3nzucd8380o2bev8oeknp'
      measures={measures}
      trendBy={attribute}
      config={{
          colors: ['#14b2e2']
      }}
  />
</div>
```

In this component, `projectId="xms7ga4tf3g3nzucd8380o2bev8oeknp"` is a hardcoded reference to our workspace ID, and the measure identifier is a hardcoded reference to a measure.

With the [gdc-catalog-export](02_start__catalog_export.md) tool, you can save the list of all measures, attributes and other relevant objects to a JSON file.

To install the tool, run the following command from the command line:
```bash
yarn global add gdc-catalog-export
```

After you installed the tool, do the following:
1. Ensure that you are in the root folder of your app, run the following command, and fill in the interactive prompts:
    ```bash
    $ gdc-catalog-export --output src/catalog.json
    ```

    > When working with the [live examples](https://gooddata-examples.herokuapp.com/), add `--hostname https://developer.na.gooddata.com`.

    The `src/catalog.json` file becomes a JSON file in your application.
    ```javascript
    {
      ...
      "$ Franchise Fees": {
        "identifier": "aaEGaXAEgB7U",
        "tags": "franchise_fees",
        "title": "$ Franchise Fees"
      },
      ...
    }
    ```
2. Import the `catalog.json` file into your `App.js` file.
   You can now reference the measure using its human-readable alias \(`$ Franchise Fees`\) instead of its identifier \(`aaEGaXAEgB7U`\). Your new `App.js` file would look like the following:
    ```javascript
    import React, { Component } from 'react';
    import { LineChart } from '@gooddata/react-components';
    import '@gooddata/react-components/styles/css/main.css';

    import logo from './logo.svg';
    import './App.css';

    import { CatalogHelper } from '@gooddata/react-components';
    import catalogJson from './catalog.json';
    const C = new CatalogHelper(catalogJson);

    const measures = [
    {
        measure: {
            localIdentifier: 'franchiseFeesIdentifier',
            definition: {
                measureDefinition: {
                    item: {
                        identifier: C.measure('$ Franchise Fees')
                    }
                }
            },
            format: '#,##0'
        }
      }
    ];

    const attribute = {
        visualizationAttribute: {
            displayForm: {
                identifier: 'date.abm81lMifn6q'
            },
            localIdentifier: 'month'
        }
    };

    class App extends Component {
       render() {
          return (
             <div className="App">
                <div className="App-header">
                   <img src={logo} className="App-logo" alt="logo" />
                   <h2>Welcome to React</h2>
                </div>
                <div style={{ height: 300 }}>
                  <LineChart
                      projectId='xms7ga4tf3g3nzucd8380o2bev8oeknp'
                      measures={measures}
                      trendBy={attribute}
                      config={{
                          colors: ['#14b2e2']
                      }}
                  />
                </div>
                <p className="App-intro">
                   To get started, edit <code>src/App.js</code> and save to reload.
                </p>
             </div>
          );
       }
    }

    export default App;
    ```

Notice that the code in the `App.js` file still includes the hardcoded reference to the workspace \(`xms7ga4tf3g3nzucd8380o2bev8oeknp`\). In your real application, you may prefer to pass the workspace ID via URL or a hash parameter, or it may be retrieved from your server-side APIs \(if you are integrating GoodData into an existing application\). It depends on your application's architecture.

## Next steps

Here are some suggestions about what you can do after you created your first visualization:

* Add more elements: tables, charts, custom visualizations. For more information, see [how to use visual components](10_vis__start_with_visual_components.md).
* [Enable drilling](15_props__drillable_item.md).
* Authenticate your users using [Single Sign-on (SSO)](30_tips__sso.md) rather than sending them to a proxied GoodData login page.
