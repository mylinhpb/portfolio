> The following article was originally published in [Dataddo's documentation](https://docs.dataddo.com/docs/transformation-pipeline-guide). Adjusted for MD in Github.
 
# How to Write a Transformation Pipeline Script

This article provides an overview of **how to construct a data transformation script**, leveraging a [JSON](/docs/json-universal-connector)-like syntax akin to [MongoDB](/docs/mongodb)'s aggregation framework. In Dataddo, transformations are defined through a series of stages within a JSON array, with each stage representing a specific data operation.

**To create an effective transformation script, you can follow these structured steps:**
**Step 1**: [Understand Your Data](#step-1-understand-your-data)
**Step 2**: [Define Your Objectives and Choose Your Operators](#step-2)
**Step 3**: [Create Transformation Stages](#step-3)
**Step 4**: [Assemble the Pipeline](#step-4)
**Step 5**: [Final Script](e#step)

**For practical applications and specific use cases, refer to the following sections:**
* [Specific Transformation Pipeline Example with an Explanation](https://docs.dataddo.com/docs/transformation-pipeline-guide#specific-example-with-an-explanation)
* [Transformation Pipeline Practical Examples](https://docs.dataddo.com/docs/transformation-pipeline-examples)

## Step 1: Understand Your Data
Before choosing operators or defining stages, you must understand the structure and content of your data. Let's assume your dataset looks something like this:

```json
{
  "field1": [{"subfield1": "value1"}, {"subfield1": "value2"}], // An array of objects
  "field2": "some text", // A simple string
  "column1": "123", // A numeric string
  "column2": 456 // A numeric value
}
```

## Step 2: Define Your Objectives and Choose Your Operators
Decide what you want the transformed data to look like. In our example, you might want to:
| Objective | Operator |
|---|---|
| Flatten `field1` so each `subfield1` value becomes part of a separate document.| **Flatten an array**: Use `$unwind` to deconstruct `field1`.|
|Transform `field2` into all uppercase letters.|**Transform a string**: Use `$toUpper` to convert `field2` to uppercase.|
|Convert `column1` to an integer type.|**Type Casting**: Use `$toInt` to change `column1` from a string to an integer.|
|Create a new field that is the sum of `column1` and `column2`.|**Calculate a sum**: Use `$add` to compute the sum of `column1` and `column2`.|

:::(Warning) ()
For an in-depth explanation of these operators and their functions, refer to the [the list of transformation pipeline expression operators](https://docs.dataddo.com/docs/transformation-pipeline-expressions).
:::

## Step 3: Create Transformation Stages
Create your transformation stages. In this example, we will have four stages:
1. [Unwind Stage](#1-unwind-stage): Deconstructs an array field from the input documents to output a document for each element.
2. [Projection Stage](#2-projection-stage): Used to include, exclude, or add new fields.
3. [Add Fields Stage (Renaming)](#3-add-fields-stage-renaming): Adds new fields or renames existing fields.
4. [Second Projection Stage (Cleanup)](#4-second-projection-stage-cleanup): Removes fields that we no longer need in the output.

:::(Warning) ()
For an in-depth explanation of these stages and their functions, refer to the [the list of transformation pipeline stages](/docs/transformation-pipeline-stages).
:::

### 1. **Unwind Stage**
`$unwind` is often used to deconstruct an array field from the input documents to output a document for each element. If `field1` is an array, each element of this array becomes a separate document.

```json
{
  "$unwind": {
    "path": "$field1",
    "preserveNullAndEmptyArrays": true
  }
}
```

### 2. **Projection Stage**
The `$project` stage is used to include, exclude, or add new fields. Here we're also transforming `field2` to uppercase and casting `column1` to an integer.

In `$project`, you specify key value pairs, where the key will be the label of your field and the value needs to specify the path where we can fill the value.

```json
{
  "$project": {
    "newField1": "$field1.subfield1",
    "transformedField2": {
      "$toUpper": "$field2"
    },
    "newColumn1": {
      "$toInt": "$column1"
    },
    "sumColumn": {
      "$add": [
        { "$toInt": "$column1" }, 
        "$column2"
      ]
    },
    "_id": 0
  }
}

```

### 3. **Add Fields Stage (Renaming)**
The `$addFields` stage adds new fields or renames existing fields. In this example, it's used to rename newField1 to renamedField1 and sumColumn to totalSum.

```json
{
  "$addFields": {
    "renamedField1": "$newField1",
    "totalSum": "$sumColumn"
  }
}
```
### 4. **Second Projection Stage (Cleanup)**
The second `$project` stage is used to remove fields that we no longer need in the output by setting them to 0, which excludes them.

```json
{
  "$project": {
    "newField1": 0,
    "sumColumn": 0
  }
}
```

## Step 4: Assemble the Pipeline
Combine all the stages into one array, in other words, enclosed in square brackets `[ ]`, to form your pipeline.

```json
[
  { /* Unwind Stage */ },
  { /* Projection Stage */ },
  { /* Add Fields Stage */ },
  { /* Second Projection Stage */ }
]
```

## Step 5: Final Script
**Recapitulation**: In this example, we perform some basic operations on the following fields: `field1`, `field2`, `column1`, and `column2`. The basic operations are:
* Unwinding an array field
* Projecting certain fields
* Renaming fields
* Casting a string field to an integer
* Adding a new field with a computed value

```json
[
    // Stage 1: Unwind an array to normalize nested data
    {
        "$unwind": {
            "path": "$field1", // Assumes field1 is an array
            "preserveNullAndEmptyArrays": true // Optional parameter to keep items that are null or an empty array
        }
    },
    // Stage 2: Project (select or transform) specific fields
    {
        "$project": {
            "newField1": "$field1",
            "transformedField2": {
                "$toUpper": "$field2" // Example of transforming a string to uppercase
            },
            "newColumn1": {
                "$toInt": "$column1" // Casting string to integer
            },
            "sumColumn": {
                "$add": ["$column1", "$column2"] // Adding two fields together
            },
            "_id": 0 // Exclude the _id field from the output
        }
    },
    // Stage 3: Rename fields for clarity
    {
        "$addFields": {
            "renamedField1": "$newField1",
            "totalSum": "$sumColumn"
        }
    },
    // Stage 4: Remove fields no longer needed
    {
        "$project": {
            "newField1": 0,
            "sumColumn": 0
        }
    }
    // Additional stages can be added as necessary...
]
```

## Specific Example with an Explanation
1.  **API Endpoint URL**: `https://data.nasa.gov/resource/tfkf-kniw.json`
2.  **Authorization**: Not required.
3.  [**HTTP Method**](/docs/connectors#http-methods): `GET`
4.  **HTTP Headers and Body**: Not required.

**Transformation script:**

```json
[
   {
       "$unset": "_id"
   },
   {
       "$unwind": "$data"
   },
   {
       "$project": {
           "source_link_url": {
               "$ifNull": [
                   "$data.source_link.url",
                   ""
               ]
           },
           "event_date": {
               "$dateToString": {
                   "date": {
                       "$toDate": {
                           "$ifNull": [
                               "$data.event_date",
                               "1970-01-01"
                           ]
                       }
                   },
                   "format": "%Y-%m-%d %H:%M:%S"
               }
           },
           "event_id": {
               "$ifNull": [
                   "$data.event_id",
                   ""
               ]
           }
       }
   }
]
```

1. `$unset`: 
   - **Purpose**: Removes the `_id` field from all documents.
   - **Reason**: The `_id` field is automatically added but may be removed for anonymization or if it's not needed for further processing.

2. `$unwind`: 
   - **Purpose**: Deconstructs an array field named `data` from the input documents, transforming each element of the array into a separate document.
   - **Reason**: Useful for operations where array elements need to be processed as individual documents.

3. `$project`: 
   - **Purpose**: Reshapes each document by including the following new fields:
     - `source_link_url`: Sets the value to `data.source_link.url` or `""` if it's null or doesn't exist.
     - `event_date`: Converts `data.event_date` to a formatted date string or to `"1970-01-01 00:00:00"` if it's null or doesn't exist.
     - `event_id`: Sets the value to `data.event_id` or `""` if it's null or doesn't exist.
   - **Reason**: Ensures that the specified fields are present in a consistent format, providing defaults where necessary. This standardization can simplify future data handling tasks.

### Non-Objects and Non-Array Fields
In the case of non-objects and non-array fields, the script can look like this:
```json
 {        "$project": {
            "country_name": {
                "$ifNull": [
                    "$data.country_name",
                    ""
                ]
            }
      }
}
```
Howerver, when used in this example, you will get the following error.
```
cannot append value to column ID country_name: unhandled data type of slice. Please modify the transformation to parse this, column can't contain non-scalar data (objects, arrays)
```

### Nested Objects or Array Fields
In case of nested objects or array fields, the script will need to be modified. In this example, the field `source_link` is actually an object that contains **field url**. As such, we will need to flatten it.

```json
 {        "$project": {
            "source_link_url": {
                "$ifNull": [
                    "$data.source_link.url",
                    ""
                ]
            }
      }
}
```

For this script to work, make sure you first unwind the data using:
```
   {
       "$unwind": "$data"
   },
```
