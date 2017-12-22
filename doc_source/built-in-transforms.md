# Built\-In Transforms<a name="built-in-transforms"></a>

AWS Glue provides a set of built\-in transforms that you can use to process your data\. You can call these transforms from your ETL script\. Your data passes from transform to transform in a data structure called a *DynamicFrame*, which is an extension to an Apache Spark SQL `DataFrame`\. The `DynamicFrame` contains your data, and you reference its schema to process your data\. For more information about these transforms, see [AWS Glue PySpark Transforms Reference](aws-glue-programming-python-transforms.md)\. 

AWS Glue provides the following built\-in transforms:

*ApplyMapping*  
Maps source columns and data types from a `DynamicFrame` to target columns and data types in a returned `DynamicFrame`\. You specify the mapping argument, which is a list of tuples that contain source column, source type, target column, and target type\.

*DropFields*  
Removes a field from a `DynamicFrame`\. The output `DynamicFrame` contains fewer fields than the input\. You specify which fields to remove using the `paths` argument\. The `paths` argument points to a field in the schema tree structure using dot notation\. For example, to remove field B, which is a child of field A in the tree, type **A\.B** for the path\.

*DropNullFields*  
Removes null fields from a `DynamicFrame`\. The output `DynamicFrame` does not contain fields of the null type in the schema\.

*Filter*  
Selects records from a `DynamicFrame` and returns a filtered `DynamicFrame`\. You specify a function, such as a Lambda function, which determines whether a record is output \(function returns true\) or not \(function returns false\)\.

*Join*  
Equijoin of two `DynamicFrames`\. You specify the key fields in the schema of each frame to compare for equality\. The output `DynamicFrame` contains rows where keys match\.

*Map*  
Applies a function to the records of a `DynamicFrame` and returns a transformed `DynamicFrame`\. The supplied function is applied to each input record and transforms it to an output record\. The map transform can add fields, delete fields, and perform lookups using an external API operation\. If there is an exception, processing continues, and the record is marked as an error\.

*MapToCollection*  
Applies a transform to each `DynamicFrame` in a `DynamicFrameCollection`\.

*Relationalize*  
Converts a `DynamicFrame` to a relational \(rows and columns\) form\. Based on the data's schema, this transform flattens nested structures and creates `DynamicFrames` from arrays structures\. The output is a collection of `DynamicFrames` that can result in data written to multiple tables\.

*RenameField*  
Renames a field in a `DynamicFrame`\. The output is a `DynamicFrame` with the specified field renamed\. You provide the new name and the path in the schema to the field to be renamed\.

*ResolveChoice*  
Use `ResolveChoice` to specify how a column should be handled when it contains values of multiple types\. You can choose to either cast the column to a single data type, discard one or more of the types, or retain all types in either separate columns or a structure\. You can select a different resolution policy for each column or specify a global policy that is applied to all columns\.

*SelectFields*  
Selects fields from a `DynamicFrame` to keep\. The output is a `DynamicFrame` with only the selected fields\. You provide the paths in the schema to the fields to keep\.

*SelectFromCollection*  
Selects one `DynamicFrame` from a collection of `DynamicFrames`\. The output is the selected `DynamicFrame`\. You provide an index to the `DynamicFrame` to select\.

*Spigot*  
Writes sample data from a `DynamicFrame`\. Output is a JSON file in Amazon S3\. You specify the Amazon S3 location and how to sample the `DynamicFrame`\. Sampling can be a specified number of records from the beginning of the file or a probability factor used to pick records to write\.

*SplitFields*  
Splits fields into two `DynamicFrames`\. Output is a collection of `DynamicFrames`: one with selected fields, and one with the remaining fields\. You provide the paths in the schema to the selected fields\.

*SplitRows*  
Splits rows in a `DynamicFrame` based on a predicate\. The output is a collection of two `DynamicFrames`: one with selected rows, and one with the remaining rows\. You provide the comparison based on fields in the schema\. For example, `A > 4`\.

*Unbox*  
Unboxes a string field from a `DynamicFrame`\. The output is a `DynamicFrame` with the selected string field reformatted\. The string field can be parsed and replaced with several fields\. You provide a path in the schema for the string field to reformat and its current format type\. For example, you might have a CSV file that has one field that is in JSON format `{"a": 3, "b": "foo", "c": 1.2}`\. This transform can reformat the JSON into three fields: an `int`, a `string`, and a `double`\.