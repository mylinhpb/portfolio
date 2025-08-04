> The following article was originally published in [Dataddo's documentation](https://docs.dataddo.com/docs/creating-a-data-source). Adjusted for MD in Github.

# How to Create a Data Source
In the first step of our quickstart tutorial, we will guide you through creating a ***data source***, allowing you to choose the specific data you want to extract from your service.

## Prerequisites
Before starting, make sure your account (e.g. Facebook account or GA4 account) has sufficient permissions to access and extract your data. Usually at least **admin-level permissions** are required.

## Configure Your Data Source

1. In the [Dataddo app](https://app.dataddo.com/), click on the **Create a Source** button under **Quick Actions**. Select your service from which you want to extract your data.
2. If your connector has a fixed schema, **select your dataset**.
    1. **Fixed-schema connectors** offer pre-selected set of attributes aka datasets (e.g. [Stripe](https://docs.dataddo.com/docs/stripe), [Gusto](https://docs.dataddo.com/docs/gusto)).  If you are not sure which dataset you need but know the metrics and attributes, use the **Search by Name or Attribute** function.
    2. **Custom-schema connectors** allow complete freedom when composing your ***data source*** (e.g. [Google Ads](https://docs.dataddo.com/docs/google-ads), [Facebook Ads](https://docs.dataddo.com/docs/facebook-ads)).
3. Click on **Add New Account** at the bottom of the drop-down and authorize the connection of your service to Dataddo.

> [!TIP]
> When you add a new account, you also create a new ***authorizer***. You can always [add more authorizers](https://docs.dataddo.com/docs/authorized-services) or accounts later.

![Account - Quickstart Create a Source](https://github.com/mylinhpb/portfolio/assets/145331760/1048adfb-ea72-4a2f-843e-7f7fda2e717e)


## Select Metrics, Dimensions, or Attributes
In this step, select your metrics, dimensions, or attributes to specify what data you want to extract. In short, you are **defining the actual data model** of the source and choosing what columns will be included in the ***data source***.

![Account - Quickstart Select Metrics](https://github.com/mylinhpb/portfolio/assets/145331760/e43ccb7f-277b-4d0b-ab67-89401aaa7f56)

## Configure Snapshotting and Finish Creating Your Data Source
1. Select the frequency of your [snapshotting](https://docs.dataddo.com/docs/extraction). Snapshots determine how often your data should be extracted. Recommended default is **daily** snapshotting to make sure you have up-to-date data.
2. [Optional] In the **Advanced Settings**, you can choose a custom **date range** and at what **time** data should be extracted.
3. Click on the **Test Data** button to preview and confirm your data.
4. **Save** your configuration.

Well done, your new ***data source*** is ready!

> [!TIP]
> **Test Data** is also a great feature for **data exploration** when you are unsure about what certain attributes mean. As your table will be populated with live data, it will be easy to discover values for each attribute.


> [!NOTE]
> If you want to extract and load historical data, see [our articles on data backfilling]https://docs.dataddo.com/docs/data-backfilling).
