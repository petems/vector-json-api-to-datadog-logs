# vector-json-api-to-datadog-logs

## What is this?

A sandbox/lab repo for showing how Vector can hit an arbitrary JSON API endpoint, transform the data and then send it to the Datadog logs API.

## Why?

A customer had a usecase where they wanted to take logs from an internal API and send off to Datadog for reporting.

Doing this with Vector reduces maintenance cost of writing a custom script, and also allows the transformation data within the JSON (another customer requirement)

## How?

In this demo, Vector is running a simple topology in which the `dummy_http_json` source fetches a JSON 
entry from the `https://dummyjson.com/` API stub.

It then sends those logs off to stdout via the `console` sink for debugging, but more importantly, does a basic transformation (adding a ddsource field) and sends it
to the Datadog Logs API.

To run this scenario, make sure you have the `DD_API_KEY` environment variable set to your Datadog
API key and then run:

```bash
$ docker compose up
```

You should then see the Vector container running, fetching a log from the DummyJSON API and then sending it to Datadog

```
$ docker compose up
[+] Running 1/0
 ✔ Container vector-json-api-to-datadog-logs-vector-1  Created                                                                                                                                                                                                                                                                                                         0.0s
Attaching to vector-json-api-to-datadog-logs-vector-1
vector-json-api-to-datadog-logs-vector-1  | 2023-06-30T09:21:48.616389Z  INFO vector::app: Log level is enabled. level="info"
vector-json-api-to-datadog-logs-vector-1  | 2023-06-30T09:21:48.620369Z  INFO vector::app: Loading configs. paths=["/etc/vector/vector.toml"]
vector-json-api-to-datadog-logs-vector-1  | 2023-06-30T09:21:48.671894Z  INFO vector::topology::running: Running healthchecks.
vector-json-api-to-datadog-logs-vector-1  | 2023-06-30T09:21:48.672243Z  INFO vector: Vector has started. debug="false" version="0.30.0" arch="x86_64" revision="38c3f0b 2023-05-22 17:38:48.655488673"
vector-json-api-to-datadog-logs-vector-1  | 2023-06-30T09:21:48.675349Z  INFO vector::internal_events::api: API server running. address=0.0.0.0:8686 playground=http://0.0.0.0:8686/playground
vector-json-api-to-datadog-logs-vector-1  | 2023-06-30T09:21:48.676368Z  INFO vector::topology::builder: Healthcheck passed.
vector-json-api-to-datadog-logs-vector-1  | 2023-06-30T09:21:49.125202Z  INFO vector::topology::builder: Healthcheck passed.
vector-json-api-to-datadog-logs-vector-1  | {"brand":"Apple","category":"smartphones","description":"An apple mobile which is nothing like apple","discountPercentage":12.96,"id":1,"images":["https://i.dummyjson.com/data/products/1/1.jpg","https://i.dummyjson.com/data/products/1/2.jpg","https://i.dummyjson.com/data/products/1/3.jpg","https://i.dummyjson.com/data/products/1/4.jpg","https://i.dummyjson.com/data/products/1/thumbnail.jpg"],"price":549,"rating":4.69,"source_type":"http_client","stock":94,"thumbnail":"https://i.dummyjson.com/data/products/1/thumbnail.jpg","timestamp":"2023-06-30T09:21:49.142982628Z","title":"iPhone 9"}
```

Once you've confirmed Vector is up and running, check out the [Logs exporer][logs] and you should see the logs from the Dummy API being added to Datadog.

Example Dummy JSON Log in Datadog:
![Log Explorer Showings Dummy API Logs](https://github.com/petems/vector-json-api-to-datadog-logs/assets/1064715/a108b02e-e305-43f6-9b7a-b37cd4caa063)

[logs]: https://app.datadoghq.com/logs?query=source%3Adummyjson