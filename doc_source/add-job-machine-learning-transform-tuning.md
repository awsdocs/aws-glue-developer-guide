# Tuning Machine Learning Transforms in AWS Glue<a name="add-job-machine-learning-transform-tuning"></a>

You can tune your machine learning transforms in AWS Glue to improve the results of your data\-cleansing jobs to meet your objectives\. To improve your transform, you can teach it by generating a labeling set, adding labels, and then repeating these steps several times until you get your desired results\. You can also tune by changing some machine learning parameters\.  

For more information about machine learning transforms, see [Matching Records with AWS Lake Formation FindMatches](machine-learning.md)\.

**Topics**
+ [Machine Learning Measurements](#machine-learning-terminology)
+ [Deciding Between Precision and Recall](#machine-learning-precision-recall-tradeoff)
+ [Deciding Between Accuracy and Cost](#machine-learning-accuracy-cost-tradeoff)
+ [Teaching the Find Matches Transform](#machine-learning-teaching)

## Machine Learning Measurements<a name="machine-learning-terminology"></a>

To understand the measurements that are used to tune your machine learning transform, you should be familiar with the following terminology:

**True positive \(TP\)**  
A match in the data that the transform correctly found, sometimes called a *hit*\.

**True negative \(TN\)**  
A nonmatch in the data that the transform correctly rejected\.

**False positive \(FP\)**  
A nonmatch in the data that the transform incorrectly classified as a match, sometimes called a *false alarm*\.

**False negative \(FN\)**  
A match in the data that the transform didn't find, sometimes called a *miss*\.

For more information about the terminology that is used in machine learning, see [Confusion matrix](https://en.wikipedia.org/wiki/Confusion_matrix) in Wikipedia\.

To tune your machine learning transforms, you can change the value of the following measurements in the **Advanced properties** of the transform\.
+ **Precision** measures how well the transform finds true positives from the total true positives possible\. For more information, see [Precision and recall](https://en.wikipedia.org/wiki/Precision_and_recall) in Wikipedia\.
+ **Recall** measures how well the transform finds true positives from the total records in the source data\. For more information, see [Precision and recall](https://en.wikipedia.org/wiki/Precision_and_recall) in Wikipedia\.
+ **Accuracy ** measures how well the transform finds true positives and true negatives\. Increasing accuracy requires more machine resources and cost\. But it also results in increased recall\. For more information, see [Accuracy and precision](https://en.wikipedia.org/wiki/Accuracy_and_precision#In_information_systems) in Wikipedia\.
+ **Cost** measures how many compute resources \(and thus money\) are consumed to run the transform\.

## Deciding Between Precision and Recall<a name="machine-learning-precision-recall-tradeoff"></a>

Each `FindMatches` transform contains a `precision-recall` parameter\. You use this parameter to specify one of the following:
+ If you are more concerned about the transform falsely reporting that two records match when they actually don't match, then you should emphasize *precision*\. 
+ If you are more concerned about the transform failing to detect records that really do match, then you should emphasize *recall*\.

You can make this trade\-off on the AWS Glue console or by using the AWS Glue machine learning API operations\.

**When to Favor Precision**  
Favor precision if you are more concerned about the risk that `FindMatches` results in a pair of records matching when they don't actually match\.  To favor precision, choose a *higher* precision\-recall trade\-off value\. With a higher value, the `FindMatches` transform requires more evidence to decide that a pair of records should be matched\. The transform is tuned to bias toward saying that records do not match\.

For example, suppose that you're using `FindMatches` to detect duplicate items in a video catalog, and you provide a higher precision\-recall value to the transform\. If your transform incorrectly detects that *Star Wars: A New Hope* is the same as *Star Wars: The Empire Strikes Back*, a customer who wants *A New Hope* might be shown *The Empire Strikes Back*\. This would be a poor customer experience\. 

However, if the transform fails to detect that *Star Wars: A New Hope* and *Star Wars: Episode IV—A New Hope* are the same item, the customer might be confused at first but might eventually recognize them as the same\. It would be a mistake, but not as bad as the previous scenario\.

**When to Favor Recall**  
Favor recall if you are more concerned about the risk that the `FindMatches` transform results might fail to detect a pair of records that actually do match\.  To favor recall, choose a *lower* precision\-recall trade\-off value\. With a lower value, the `FindMatches` transform requires less evidence to decide that a pair of records should be matched\. The transform is tuned to bias toward saying that records do match\.

For example, this might be a priority for a security organization\. Suppose that you are matching customers against a list of known defrauders, and it is important to determine whether a customer is a defrauder\. You are using `FindMatches` to match the defrauder list against the customer list\. Every time `FindMatches` detects a match between the two lists, a human auditor is assigned to verify that the person is, in fact, a defrauder\. Your organization might prefer to choose recall over precision\. In other words, you would rather have the auditors manually review and reject some cases when the customer is not a defrauder than fail to identify that a customer is, in fact, on the defrauder list\.

**How to Favor Both Precision and Recall**  
The best way to improve both precision and recall is to label more data\. As you label more data, the overall accuracy of the `FindMatches` transform improves, thus improving both precision and recall\. Nevertheless, even with the most accurate transform, there is always a gray area where you need to experiment with favoring precision or recall, or choose a value in the middle\. 

## Deciding Between Accuracy and Cost<a name="machine-learning-accuracy-cost-tradeoff"></a>

Each `FindMatches` transform contains an `accuracy-cost` parameter\. You can use this parameter to specify one of the following:
+ If you are more concerned with the transform accurately reporting that two records match, then you should emphasize *accuracy*\.
+ If you are more concerned about the cost or speed of running the transform, then you should emphasize *lower cost*\.

You can make this trade\-off on the AWS Glue console or by using the AWS Glue machine learning API operations\.

**When to Favor Accuracy**  
Favor accuracy if you are more concerned about the risk that the `find matches` results won't contain matches\. To favor accuracy, choose a *higher* accuracy\-cost trade\-off value\. With a higher value, the `FindMatches` transform requires more time to do a more thorough search for correctly matching records\. Note that this parameter doesn't make it less likely to falsely call a nonmatching record pair a match\. The transform is tuned to bias towards spending more time finding matches\.

**When to Favor Cost**  
Favor cost if you are more concerned about the cost of running the `find matches` transform and less about how many matches are found\. To favor cost, choose a *lower* accuracy\-cost trade\-off value\. With a lower value, the `FindMatches` transform requires fewer resources to run\. The transform is tuned to bias towards finding fewer matches\. If the results are acceptable when favoring lower cost, use this setting\.

**How to Favor Both Accuracy and Lower Cost**  
It takes more machine time to examine more pairs of records to determine whether they might be matches\. If you want to reduce cost without reducing quality, here are some steps you can take: 
+ Eliminate records in your data source that you aren't concerned about matching\.
+ Eliminate columns from your data source that you are sure aren't useful for making a match/no\-match decision\. A good way of deciding this is to eliminate columns that you don't think affect your own decision about whether a set of records is “the same\.”

## Teaching the Find Matches Transform<a name="machine-learning-teaching"></a>

Each `FindMatches` transform must be taught what should be considered a match and what should not be considered a match\. You teach your transform by adding labels to a file and uploading your choices to AWS Glue\. 

You can orchestrate this labeling on the AWS Glue console or by using the AWS Glue machine learning API operations\.

**How Many Times Should I Add Labels? How Many Labels Do I Need?**  
The answers to these questions are mostly up to you\. You must evaluate whether `FindMatches` is delivering the level of accuracy that you need and whether you think the extra labeling effort is worth it for you\. The best way to decide this is to look at the “Precision,” “Recall,” and “Area under the precision recall curve” metrics that you can generate when you choose **Estimate quality** on the AWS Glue console\. After you label more sets of tasks, rerun these metrics and verify whether they have improved\. If, after labeling a few sets of tasks, you don't see improvement on the metric that you are focusing on, the transform quality might have reached a plateau\.

**Why Are Both True Positive and True Negative Labels Needed?**  
The `FindMatches` transform needs both positive and negative examples to learn what you think is a match\. If you are labeling `FindMatches`\-generated training data \(for example, using the **I do not have labels** option\), `FindMatches` tries to generate a set of “label set ids” for you\. Within each task, you give the same “label” to some records and different “labels” to other records\. In other words, the tasks generally are not either all the same or all different \(but it's okay if a particular task is all “the same” or all “not the same”\)\.

If you are teaching your `FindMatches` transform using the **Upload labels from S3** option, try to include both examples of matching and nonmatching records\. It's acceptable to have only one type\. These labels help you build a more accurate `FindMatches` transform, but you still need to label some records that you generate using the **Generate labeling file** option\.

**How Can I Enforce That the Transform Matches Exactly as I Taught It?**  
The `FindMatches` transform learns from the labels that you provide, so it might generate records pairs that don't respect the provided labels\. To enforce that the `FindMatches` transform respects your labels, select **EnforceProvidedLabels** in **FindMatchesParameter**\.

**What Techniques Can You Use When an ML Transform Identifies Items as Matches That Are Not True Matches?**  
You can use the following techniques:
+ Increase the `precisionRecallTradeoff` to a higher value\. This eventually results in finding fewer matches, but it should also break up your big cluster when it reaches a high enough value\. 
+ Take the output rows corresponding to the incorrect results and reformat them as a labeling set \(removing the `match_id` column and adding a `labeling_set_id` and `label` column\)\. If necessary, break up \(subdivide\) into multiple labeling sets to ensure that the labeler can keep each labeling set in mind while assigning labels\. Then, correctly label the matching sets and upload the label file and append it to your existing labels\. This might teach your transformer enough about what it is looking for to understand the pattern\. 
+ \(Advanced\) Finally, look at that data to see if there is a pattern that you can detect that the system is not noticing\. Preprocess that data using standard AWS Glue functions to *normalize* the data\. Highlight what you want the algorithm to learn from by separating data that you know to be differently important into their own columns\. Or construct combined columns from columns whose data you know to be related\. 