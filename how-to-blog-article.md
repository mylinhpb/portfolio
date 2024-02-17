# JSON to Google Bigquery: Data Pipeline in Less than 2 Minutes
Now that we know <a href="https://blog.dataddo.com/get-started-with-google-bigquery" target="_blank">how to set up Google BigQuery</a>, it’s time to load our data. For this article, we chose to insert data from JSON, a very popular data interchange format that is also extremely lightweight, text-based, and language-independent. If you work with databases, chances are you have used JSON many times before.
So how exactly can you connect your JSON data to BigQuery? Let us introduce you to three main methods: _The Classic_, _The Data Engineer_, and _The Time-Saver_.

## 1. The Classic: Manual Data Load
This method was in fact not available until last year as BigQuery did not support JSON before that. However, since there are not just one or two limitations, if you opt for this method, we strongly recommend checking out [Google’s official documentation](https://cloud.google.com/bigquery/docs/loading-data-cloud-storage-json) first.

### Step 1: Get BigQuery Ready
Go to the <a href="https://console.cloud.google.com/bigquery?pli=1" target="_blank">BigQuery console</a> and select your project or create a new one. Next, in the menu on the left, click on three dots to create a dataset.
Pay attention to your **Data location**, this will be important for later.

### Step 2: Set up Google Storage Bucket
While you can load your data from a local JSON file, this is limited to 100 MB which is why using Google Cloud Storage is preferred. However, since we mentioned some limitations earlier, here are the most important points:
If you're loading data from your Cloud Storage bucket, the bucket must be the set in same location as your dataset
JSON data has to be newline delimited
Values in `DATE`, `TIMESTAMP` columns must be in the following format:
* YYYY-MM-DD (year-month-day)
* hh:mm:ss (hour-minute-second)
Object versioning from Cloud Storage is not supported in BigQuery
Dataset's location must be the same as your Cloud Storage bucket’s (unless the value is set to “US multi-region”)
BigQuery does not support Cloud Storage object versioning, do not include a generation number in the Cloud Storage URI

### Step 3: Is Your JSON File Newline Delimited?
Before proceeding further, make sure that your downloaded file is Newline Delimited JSON, aka whether objects all have their own line. If your data is not in this format, Google BigQuery will fail to load your file.
JSON
[{"id":1,"name":"anna"},{"id":2,"name":"barbara"},{"id":3,"name":"charlie"},{"id":4,"name":"denise"}]
JSON Newline Delimited
{"id":1,"name":"anna"}
{"id":2,"name":"barbara"}
{"id":3,"name":"charlie"}
{"id":4,"name":"denise"}

There are a few ways to convert your data such as using third-party apps or websites, which are usually quite intuitive. Nonetheless, when tested with a large amount of data, some sites can lag significantly (in one case, our Google Chrome couldn’t handle converting a 50 MB file, testing had to be done with a much smaller file instead).
You can also convert your file with Python or jq, to find out how to do that, check out these answers on Stack Overflow.

### Step 4: Create Table and Load Data
Select your Dataset and click on Create table. When choosing your Source, you can select Upload if you wish to load a file that is less than 100 MB or select a file from your Google Storage bucket. 

Finally, select your Schema (auto-detection should work), and click on Create table. Repeat steps 3 and 4 as many times as needed.
Manual Without Some Drawbacks Isn’t Manual
While this method isn’t as tedious as inputting each entry manually, it also isn’t without flaws. As it is with all manual data loads, despite being very time-consuming (and time oftentimes is money), it’s extremely error-prone. Be it downloading the wrong file, converting just part of the data, or uploading one file twice, all of these can be detrimental to your data quality and integrity.
It is amazing that Google BigQuery now allows manual loading of JSON data, however, unless you have just one file to work with, treat The Classic as your last resort. And just to be sure, see Step 1 again for all the limitations you need to pay attention to otherwise your load job will fail.

## The Data Engineer: Choose Your Language
Let’s be real, if you are a data engineer, you probably didn’t look for this guide in the first place. Nevertheless, for the data engineers who are just starting out or the curious data enthusiasts, we had to at least mention this way. 
There are multiple programming languages you can use to load your file, including Python, C#, Java, PHP, and others. For example, in Python, the code would look something like this:
from google.cloud import bigquery

# Construct a BigQuery client object.
client = bigquery.Client()

# TODO(developer): Set table_id to the ID of the table to create.
# table_id = "your-project.your_dataset.your_table_name"

job_config = bigquery.LoadJobConfig(
    schema=[
        bigquery.SchemaField("name", "STRING"),
        bigquery.SchemaField("post_abbr", "STRING"),
    ],
    source_format=bigquery.SourceFormat.NEWLINE_DELIMITED_JSON,
)
uri = "gs://cloud-samples-data/bigquery/us-states/us-states.json"

load_job = client.load_table_from_uri(
    uri,
    table_id,
    location="US",  # Must match the destination dataset location.
    job_config=job_config,
)  # Make an API request.

load_job.result()  # Waits for the job to complete.

destination_table = client.get_table(table_id)
print("Loaded {} rows.".format(destination_table.num_rows)

You can find this snippet and examples in the other programming languages on the Google BigQuery official documentation.
But if you don’t know much about coding or ETL is not exactly what you want to spend most of your time on, let’s move on to the next method.

## The Time-Saver: Pipeline in Less Than 2 Minutes with Dataddo
The timeframe in the heading might sound too good to be true, we actually timed the whole process. If you have your BigQuery project and JSON URL ready, let’s go through the time stamps!
### 0:00-0:15 Authorize Account
If you haven’t done so before, head over to the list of your connected services to authorize your Google BigQuery account. Simply click on Add Service and follow the prompts.

### 0:16-0:30 Create Destination
Next, create your destination by choosing Google BigQuery and selecting the account you just authorized. Follow the instructions to select the appropriate dataset.
0:31-1:00 Create Source
Source creation is the only part that takes a bit longer but only if you need to do some transformations. Other than that, you only need the URL of your JSON file, in this example, we used NASA’s global landslide data.
If you use Dataddo, it doesn’t matter whether or not the file is Newly Delimited, or if your date values are in there right format as the platform will take care of this for you.


### 1:01-1:30 Create Flow
Finally, go to Create Flow by choosing your Source, your Destination, and write a table name.

Afterward, all that’s left is checking your data in Google BigQuery!


## Conclusion
Despite us heavily recommending against it, The Classic method does have its uses. This applies mainly if you are working with just one or two JSON files, or you are testing things out. That said, if you have budgetary constraints, this method is perhaps not the best if you are working with large files as Google Storage is a paid service.
Although a very interesting thing to look into, The Data Engineer way’s biggest weakness is the need to know how to code. If you are unfamiliar with coding, this method could take more time than expected due to all the research that will need to get done. Then, the management and maintenance of the connection might also take your attention away from the core of your work.
In today’s day and age, data analysis insights are very important to work with which is why the time to them should be as short as possible. The Time-Saver method will minimize the time you spend on ETL/ELT because even though it’s an inevitable step, it does not bring any additional value to data analysis itself.
Sometimes you might even want to skip the database part and connect your data straight to dashboards for some quick data insights, in which case check out how to connect JSON data to Google Data Studio For Free. Dataddo now allows sending data directly to BI tools and Google Sheets for completely free!
