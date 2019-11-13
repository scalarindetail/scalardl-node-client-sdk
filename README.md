## Scalar DL Node Client SDK

This is a library for Node.js applications by which the applications can interact with a [Scalar DL](https://github.com/scalar-labs/scalardl) network.

## Install

We can use any NPM package manager to install this library. For example, to install with NPM:
```
npm install @scalar-labs/scalardl-node-client-sdk
```

## HOWTO

### Create ClientService instance

`ClientService` class is the main class of this package.
It provides following functions to request Scalar DL network.

|Name|use|
|----|---|
|registerCertificate|To register a client's certificate on a Scalar DL network|
|registerContract|To register a contract (of a registered client) on the Scalar DL network|
|listContracts|To list the client's registered contracts|
|executeContract|To execute a client's registered contract|
|validateLedger|To validate an asset on the Scalar DL network to determine if it has been tampered|

Use the code snippet below to create a ClientService instance.

```
const { ClientService } = require('@scalar-labs/scalardl-web-client-sdk');
const clientService = new ClientService(clientProperties);
```

The `clientProperties` argument is mandatory for the constructor.
This is a properties example that a user `foo@example.com` would use to try to connect to the server `scalardl.example.com:50051` of the Scalar DL network.
```
{
    'scalar.ledger.client.server_host': 'scalardl.example.com',
    'scalar.ledger.client.server_port': 50051,
    'scalar.ledger.client.cert_holder_id': 'foo@example.com',
    'scalar.ledger.client.private_key_pem': "-----BEGIN EC PRIVATE KEY-----\nMHc...",
    'scalar.ledger.client.cert_pem': "-----BEGIN CERTIFICATE-----\nMIICjTCCAj...n",
    'scalar.ledger.client.cert_version': 1,
    'scalar.ledger.client.tls.enabled': false,
    'scalar.ledger.client.tls.ca_root_cert': '// not used here',
}
```

In what follows assume that we have a clientService instance.

### Register the certificate
Use the `registerCertificate` function to register a certificate on the Scalar DL network.
```
const response = await clientService.registerCertificate();
const status = response.getStatus();
const message = response.getMessage();
```
Please refer to the [Status code](#status-code) section below for the details of status.

### Register contracts
Use the `registerContract` function to register a contract.
```
const response = await clientService.registerContract('contractId', 'com.example.contract.contractName', contractUint8Array, propertiesObject);
const status = response.getStatus();
const message = response.getMessage();
```

### List registered contracts
Use listContracts function to list all registered contracts.
```
const response = await clientService.listContracts();
const message = response.getMessage();
const contracts = JSON.parse(message);
```

### Execute a contract
Use executeContract function to execute a registered contract.
```
const response = await clientService.executeContract('contractId', argumentObject);
const status = response.getStatus();
const result = JSON.parse(response.getResult());
```

### Validate an asset
Use the `validateLedger` function to validate an asset in the Scalar DL network.
```
const response = await clientService.validateLedger('assetId');
const status = response.getStatus();
const message = response.getMessage();
```

### Status code
Enumeration `StatusCode` enumerates all the possible status of a Scalar DL response.
```
StatusCode = {
  OK: 200,
  INVALID_HASH: 300,
  INVALID_PREV_HASH: 301,
  INVALID_CONTRACT: 302,
  INVALID_OUTPUT: 303,
  INVALID_NONCE: 304,
  INCONSISTENT_STATES: 305,
  INVALID_SIGNATURE: 400,
  UNLOADABLE_KEY: 401,
  UNLOADABLE_CONTRACT: 402,
  CERTIFICATE_NOT_FOUND: 403,
  CONTRACT_NOT_FOUND: 404,
  CERTIFICATE_ALREADY_REGISTERED: 405,
  CONTRACT_ALREADY_REGISTERED: 406,
  INVALID_REQUEST: 407,
  CONTRACT_CONTEXTUAL_ERROR: 408,
  ASSET_NOT_FOUND: 409,
  DATABASE_ERROR: 500,
  UNKNOWN_TRANSACTION_STATUS: 501,
  RUNTIME_ERROR: 502,
  CLIENT_IO_ERROR: 600,
  CLIENT_DATABASE_ERROR: 601,
  CLIENT_RUNTIME_ERROR: 602,
};
```

## CircleCI Project Maintenance Guideline

### 1. Environment Variables

Set environment variable on CircleCi by following [this guide](https://circleci.com/blog/new-on-circleci-import-project-environment-variables/).
Then you can use it like so :-

**$ENV_VAR**  - Represents the CircleCI environment variable

In your CircleCI configuration files.
<pre>
    - image: scalarlabs/scalar-ledger:1.3.1
     auth:
       username: <b>$DOCKERHUB_USERNAME</b>
       password: <b>$DOCKERHUB_PASSWORD</b>
</pre>

We can also use it to store the content of a certain configuration file as well, for example :-
The content of the secret files will be stored as a CircleCI environment variable, multiline configuration content will have to be parsed into a single string. For example :-
```
{
      "name": "@scalar-labs/ist-web-client-sdk",
      "description": "Web SDK for Information Banking Solution Template",
      "author": "Scalar, Inc.",
      "dependencies": {
        "axios": "0.19.0",
      }
}
```

The raw content above has to be formatted as :-
```
{\n      \"name\": \"@scalar-labs/ist-web-client-sdk\",\n      \"description\": \"Web SDK for Information Banking Solution Template\",\n      \"author\": \"Scalar, Inc.\",\n      \"dependencies\": {\n        \"axios\": \"0.19.0\",\n      }\n}
```

**Beware**: If environment variable is favoured when for keeping the schema file, then we would need to update the environment variable to reflect the latest schema file. But keep in mind that, it always depends on the command input required format.



## Contributing
This library is mainly maintained by the Scalar Engineering Team, but of course we appreciate any help.

* For asking questions, finding answers and helping other users, please go to [scalardl-user](https://groups.google.com/forum/#!forum/scalardl-user).
* For filing bugs, suggesting improvements, or requesting new features, help us out by opening an issue.

## License
Scalar DL Node client SDK is dual-licensed under both the AGPL (found in the LICENSE file in the root directory) and a commercial license. You may select, at your option, one of the above-listed licenses. Regarding the commercial license, please [contact us](https://scalar-labs.com/contact_us/) for more information.
