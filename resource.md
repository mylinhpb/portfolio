> The following article was originally published in [Dataddo's documentation](https://docs.dataddo.com/docs/connectors). Adjusted for MD in Github.

# Type of Connectors
In this article, we’ll explore types of connectors available in Dataddo. Connectors allow Dataddo to extract data from your services. When you **configure a connector**, it allows you to define the data to be extracted, consequently creating a ***data source***. The use of connectors is implicit as you start using them immediately without even actively noticing.

When you connect your data, you use one of the following connector types:
1. [Fixed-schema connectors](#fixedschema-connectors): connectors with pre-defined set of metrics.
2. [Custom-schema connectors](#customschema-connectors): connectors with custom configuration.
3. [Universal connectors](#universal-connectors): code-only connectors.

<h2 id='#fixedschema-connectors'>Fixed-Schema Connectors</h2>

**Fixed-schema connectors** offer a pre-defined list of schemas (**datasets**) such as invoices or customers. While they limit customization options, these connectors are useful in standard use-cases thanks to a quick and straightforward setup.

Connectors that fall under this category are e.g. [Mailchimp](https://docs.dataddo.com/docs/mailchimp), [ExactOnline](https://docs.dataddo.com/docs/exact-online), [Google Search Console](https://docs.dataddo.com/docs/google-search-console).

<h2 id='#customschema-connectors'>Custom-Schema Connectors</h2>

**Custom-schema connectors** enable dynamic schema definitions and they are tailored to work with services that involve custom attributes. These connectors offer a specialized solution for your unique data integration needs.

Connectors that fall under this category are e.g. [Hubspot](https://docs.dataddo.com/docs/hubspot), [Google Analytics 4](https://docs.dataddo.com/docs/google-analytics-4), [Salesforce](https://docs.dataddo.com/docs/salesforce), [Facebook Ads](https://docs.dataddo.com/docs/facebook-ads).

<h2 id='#universal-connectors'>Universal Connectors</h2>

**Universal connectors** are code-only connectors designed for granular, low-level interaction with a wide range of data systems. These connectors are highly customizable, offering a versatile option for diverse data integration requirements.

Connectors that fall under this category are
* [JSON](https://docs.dataddo.com/docs/json-universal-connector), [XML](https://docs.dataddo.com/docs/xml-universal-connector), [CSV](https://docs.dataddo.com/docs/csv-universal-connector)
* Webhook connectors for HTTP APIs
* Database connectors like [MySQL](https://docs.dataddo.com/docs/mysql), [Postgres](https://docs.dataddo.com/docs/postgres), and [SQL Server](https://docs.dataddo.com/docs/universal-sql-server).

Connecting to various online data sources is straightforward. Our robust data transformation middleware enables connections to nearly any service with a **JSON, CSV, or XML-based API**. In most cases, you will be able to define the following parameters:
* [HTTP Method](#http-methods)
* [Transformation Pipeline](https://docs.dataddo.comhttps://docs.dataddo.com/docs/transformation-pipeline)
* [HTTP Headers](#http-headers)
* [HTTP Body](#http-body)

<h3 id='#http-methods'>HTTP Methods</h3>

In configuring a universal connector, understanding the distinction between the `GET` and `POST` HTTP methods is crucial as it influences how data is sent and received in your API interactions:
* `GET`: Use for read-only operations, where you want to retrieve data without modifying the server state. `GET` requests are ideal for fetching resources, searching, or viewing data.
* `POST`: Use when you need to send data to the server to create or update resources, perform data processing, or make changes that have side effects on the server. `POST` requests are often used in forms, login processes, and when submitting data to a server.

Here are the main differences between these two methods.

| Feature            | GET                                | POST                                     |
|-------------------- | ---------------------------------- | ---------------------------------------- |
| **Purpose**         | Retrieve data from a resource      | Submit data to be processed by a resource |
| **Data Handling**   | Data is passed as query parameters in the URL | Data is passed in the body of the HTTP request |
| **Visibility**      | Data is visible in the URL          | Data is not visible in the URL           |
| **Caching**         | Can be cached                | Not cached                  |
| **Idempotent**      | Idempotent (consistent results for identical requests) | Non-idempotent (varied results possible) |

### Transformation Pipeline

For additional flexibility, universal connectors **replicate HTTP requests**, meaning you can specify details like URLs, headers, and body content. This feature allows connections to any service outputting data in JSON, CSV, or XML format.

To process the data from these APIs, a brief **transformation script** is required. This script converts API output into a format Dataddo can interpret.

For services already supported by existing connectors, universal connectors are an alternative when more control and customization are required.

> [!NOTE]
> Related Articles
> * [Transformation Pipeline Examples](https://docs.dataddo.com/docs/transformation-pipeline)
> * [List of Transformation Pipeline Stages](https://docs.dataddo.com/docs/transformation-pipeline-stages)
> * [List of Transformation Pipeline Expression Operators](https://docs.dataddo.com/docs/transformation-pipeline-expressions)


<h3 id='#http-headers'>HTTP Headers</h3>

**HTTP headers** play a crucial role in the context of web communications, acting as the carriers of additional information in HTTP request and response messages. These name-value pairs, present in the headers of HTTP messages, control and describe the behavior of any HTTP transaction. They are most often used for authorization purposes and defining the content type in transactions.

When secure access to APIs or web resources is needed, HTTP headers are often employed for token-based authorization. This involves sending a 'static token' – a predefined and unchanging security key – within the HTTP header of a request. This token validates the identity of the requestor and determines their access permissions.

#### Authorization Use Case

To securely access data from a server, your application might use a header structure like:
```code
Authorization: Bearer YOUR_TOKEN
```

Here, **Bearer** signifies the token type, followed by the actual token string. This method ensures secure data transactions while keeping the user's credentials (like username and password) hidden. In Dataddo's HTTP headers section, the token or key would be used like this:
* **Key**: `Authorization`
* **Value**: `Bearer YOUR_TOKEN`

#### Content Type Definition Use Case

HTTP headers like 'Content-Type' define the nature of the data in the request or response. For example:
* `Content-Type: application/json` declares that the message body contains JSON-formatted data.
*  `Content-Type: text/csv` declares that the message body contains data in CSV (Comma-Separated Values) format.

<h3 id='http-body'>HTTP Body</h3>

In the **HTTP Body** tab, you'll find tthe data transmitted in an HTTP transaction message. This data is often formatted as a JSON or XML string in RESTful APIs. Typically, when you specify an HTTP body, the value for the HTTP method is either POST or PUT.

## How to Request a New Connector

If a connector for your service is not available, you can fill out this [connector request form](https://www.dataddo.com/connector-request). Please provide us with as many details as possible to facilitate the connector development.

**When requesting a new connector,** please provide us with a list of
* **Specific API endpoints** of the service that you'd like to access, or
* **Type of data** you'd like to access.

Typically, the development timeline for a new connector is around four weeks, this might vary depending on the volume of ongoing requests. For customized connector solutions, please contact our solutions team at support@dataddo.com.
