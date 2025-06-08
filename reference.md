> The following article was originally published in [Dataddo's documentation](https://docs.dataddo.com/docs/transformation-pipeline-stages).

# Transformation Pipeline Stages

On this page, you will find a comprehensive list of transformation pipeline [stages](#transformation-pipeline-stages) and their [options](#stages-options).

## Transformation Pipeline Stages

| Stage Name           | Description                                                                                                                                                                                |
| -------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `$addFields`         | Adds new fields to documents.                                                                                                                                                              |
| `$bucket`            | Groups documents into specified buckets based on an expression and boundaries. [Options available](https://docs.dataddo.com/docs/transformation-pipeline-stages#stages-options).           |
| `$bucketAuto`        | Automatically distributes documents into a specified number of even buckets. [Options available](https://docs.dataddo.com/docs/transformation-pipeline-stages#stages-options).             |
| `$changeStream`      | Returns a Change Stream cursor; must be the first stage in the pipeline.                                                                                                                   |
| `$collStats`         | Provides statistics about a collection or view.                                                                                                                                            |
| `$count`             | Counts the documents at this pipeline stage.                                                                                                                                               |
| `$currentOp`         | Displays active and/or dormant operations. Requires the db.aggregate() method. [Options available](https://docs.dataddo.com/docs/transformation-pipeline-stages#stages-options).           |
| `$facet`             | Executes multiple aggregation pipelines in one stage, enabling multi-faceted aggregations.                                                                                                 |
| `$geoNear`           | Orders documents based on geospatial proximity, incorporating $match, $sort, and $limit. [Options available](https://docs.dataddo.com/docs/transformation-pipeline-stages#stages-options). |
| `$graphLookup`       | Conducts a recursive search, appending results to the output document as an array. [Options available](https://docs.dataddo.com/docs/transformation-pipeline-stages#stages-options).       |
| `$group`             | Groups documents by an expression, applying accumulator expressions to each group.                                                                                                         |
| `$indexStats`        | Reports usage statistics for each index in the collection.                                                                                                                                 |
| `$limit`             | Passes the first n documents unmodified to the pipeline.                                                                                                                                   |
| `$listLocalSessions` | Lists active sessions on the connected mongos or mongod instance.                                                                                                                          |
| `$listSessions`      | Lists sessions propagated to the system.sessions collection.                                                                                                                               |
| `$lookup`            | Performs a left outer join with another collection in the same database. [Options available](https://docs.dataddo.com/docs/transformation-pipeline-stages#stages-options).                 |
| `$match`             | Filters documents, passing only matches to the next stage.                                                                                                                                 |
| `$merge`             | Writes aggregation results to a collection; must be the last stage in the pipeline. [Options available](https://docs.dataddo.com/docs/transformation-pipeline-stages#stages-options).      |
| `$out`               | Writes aggregation results to a collection; must be the last stage. [Options available](https://docs.dataddo.com/docs/transformation-pipeline-stages#stages-options).                      |
| `$planCacheStats`    | Provides plan cache data for a collection.                                                                                                                                                 |
| `$project`           | Reshapes each document, adding or removing fields.                                                                                                                                         |
| `$redact`            | Reshapes documents based on their content, allowing field level redaction.                                                                                                                 |
| `$replaceRoot`       | Replaces a document with a specified embedded one, promoting the embedded document.                                                                                                        |
| `$replaceWith`       | Alias for `$replaceRoot`. Replaces a document with a specified embedded one.                                                                                                               |
| `$sample`            | Randomly selects a specified number of documents. [Options available](https://docs.dataddo.com/docs/transformation-pipeline-stages#stages-options).                                        |
| `$search`            | Executes a full-text search.                                                                                                                                                               |
| `$set`               | Alias for `$addFields`. Adds new fields to documents.                                                                                                                                      |
| `$skip`              | Skips the first n documents, passing the rest unmodified.                                                                                                                                  |
| `$sort`              | Reorders documents based on a specified sort key.                                                                                                                                          |
| `$sortByCount`       | Groups documents by an expression's value and counts them.                                                                                                                                 |
| `$unionWith`         | Combines results from two collections into one. [Options available](https://docs.dataddo.com/docs/transformation-pipeline-stages#stages-options).                                          |
| `$unset`             | Removes specified fields from documents.                                                                                                                                                   |
| `$unwind`            | Deconstructs an array field, outputting a document for each element. [Options available](https://docs.dataddo.com/docs/transformation-pipeline-stages#stages-options).                     |

## Stages Options

| Stage          | Option                                                                                                                                                       |
| -------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `$bucket`      | `groupBy`: An expression to group by.                                                                                                                        |
|                | `boundaries`: An array specifying the boundaries to group documents.                                                                                         |
|                | `default`: A literal that specifies the `_id` of an additional group for all documents that do not match any boundary.                                       |
| `$bucketAuto`  | `groupBy`: An expression to group by.                                                                                                                        |
|                | `buckets`: The number of buckets to create.                                                                                                                  |
| `$currentOp`   | `allUsers`: A boolean indicating if operations from all users should be returned.                                                                            |
|                | `localOps`: A boolean indicating if only operations on the local mongod instance should be returned.                                                         |
| `$geoNear`     | `near`: The geospatial point to calculate distances from.                                                                                                    |
|                | `distanceField`: The field that will contain the calculated distance.                                                                                        |
|                | `spherical`: A boolean specifying whether to treat the `near` point as a sphere.                                                                             |
|                | `maxDistance`: The maximum distance from the `near` point that the documents can be.                                                                         |
| `$graphLookup` | `from`: The target collection for the recursive search.                                                                                                      |
|                | `startWith`: The expression that specifies the value of the `connectFromField` with which to start the recursive search.                                     |
|                | `connectFromField`: The field whose value `startWith` references.                                                                                            |
|                | `connectToField`: The field in the documents of the `from` collection that is equal to the `connectFromField`.                                               |
|                | `as`: The array field added to each output document that contains the search results.                                                                        |
| `$lookup`      | `from`: The target collection.                                                                                                                               |
|                | `localField`: The field from the input documents.                                                                                                            |
|                | `foreignField`: The field from the documents of the `from` collection.                                                                                       |
|                | `as`: The name of the new array field to add to the input documents.                                                                                         |
| `$merge`       | `into`: The target collection.                                                                                                                               |
|                | `on`: The fields to match on.                                                                                                                                |
|                | `whenMatched`: The action to take when a matching document exists.                                                                                           |
|                | `whenNotMatched`: The action to take when no matching document exists.                                                                                       |
| `$out`         | `collection`: The target collection.                                                                                                                         |
| `$sample`      | `size`: The number of documents to randomly select.                                                                                                          |
| `$unionWith`   | `coll`: The name of the collection to union with.                                                                                                            |
|                | `pipeline`: An optional aggregation pipeline to apply to the specified collection before performing the union.                                               |
| `$unwind`      | `path`: The path to the array field to unwind.                                                                                                               |
|                | `includeArrayIndex`: The name of a new field to hold the array index of the element.                                                                         |
|                | `preserveNullAndEmptyArrays`: A boolean indicating whether to include as an output document those documents that have an empty array or missing array field. |
