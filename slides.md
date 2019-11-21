# Continuous Delivery in Government

**_DevOps fundamentals for the public sector_**

Aidan Feldman, Civic Technologist

---

## What are we doing today

- Setting up a development environment in [Visual Studio Online](https://visualstudio.microsoft.com/services/visual-studio-online/)
- Running an application "locally"
- Automated application testing
- Deploying to infrastructure-as-a-service (IaaS)
- Deploying to a platform-as-a-service (PaaS)
- Automated security testing

---

## Level-setting

- Who could describe the difference between IaaS and PaaS?
- Who has used the command line in the past three months?

---

## Caveats

- We are using Azure, but this is not an endorsement
  - Could use any number of cloud providers
- You will need a credit/debit card
  - Just for verification; shouldn't cost anything

---

## Setup

1. Go to [Visual Studio Online](https://online.visualstudio.com/environments)
1. Sign in, or create a Microsoft+Azure account if needed
1. Create a Billing Plan
1. Create an Environment, using a `Git repository` of `afeld/delivery`
1. Click the Environment to Connect
1. [Open a terminal](https://code.visualstudio.com/docs/editor/integrated-terminal)

---

## Visual Studio Online

To run the app from Visual Studio Online:

1. Install dependencies.

   ```sh
   pip3 install -r requirements.txt --user
   ```

1. Start the app server.

   ```sh
   flask run
   ```

1. It should say `Running on http://127.0.0.1:5000/`
1. Click the Remote Explorer in the Activity Bar
1. Under `Environment Details`, then `Forwarded Ports`, click `Port :5000`

This should open a new browser tab that says "Hello World!"

---

## Test-driven development (TDD)

1. Run the tests

   ```sh
   pytest test_local.py
   ```

1. Add a test for a new route: `/bye` should return `Goodbye!`
1. Run the local tests again
1. Get the test to pass

---

## Azure App Services

Roughly following the [Python Quickstart](https://docs.microsoft.com/en-us/azure/app-service/containers/quickstart-python). To start, log into the Azure CLI.

```sh
az login
```

---

### Deploy

1. Deploy the application—you can use [another location](https://azure.microsoft.com/en-us/global-infrastructure/locations/), like `"Australia Central"`

   ```sh
   az webapp up \
     --sku F1 \
     --name <yourname>-test \
     --location "Central US"
   ```

1. It should output a URL. Copy to a new browser tab, and you should see "Hello World!"
1. Try going to the `https://` version of that same URL

---

### View logs

1. Tail the logs

   ```sh
   az webapp log tail
   ```

1. In your tab with the web page open, hit refresh
1. Look at the log output in your terminal

---

## Security tests

1. Open [`test_security.py`](test_security.py)
1. Modify the `HOST` to be your deployed app
1. Run the tests

   ```sh
   pytest test_security.py
   ```

1. Test should fail

---

### Making security tests pass

1. Force HTTPS

   ```sh
   az webapp update --https-only true
   ```

1. Run the tests again

   ```sh
   pytest test_security.py
   ```

1. The tests should now pass

---

## Cleanup

To ensure you don't get charged for anything:

1. View [all resources](https://portal.azure.com/#blade/HubsExtension/BrowseAll)
1. Select all
1. `Delete`