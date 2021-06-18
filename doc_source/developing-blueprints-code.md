# Writing the Blueprint Code<a name="developing-blueprints-code"></a>

Each blueprint project that you create must contain at a minimum the following files:
+ A Python layout script that defines the workflow\. The script contains a function that defines the entities \(jobs and crawlers\) in a workflow, and the dependencies between them\.
+ A configuration file, `blueprint.cfg`, which defines:
  + The full path of the workflow layout definition function\.
  + The parameters that the blueprint accepts\.

**Topics**
+ [Creating the Blueprint Layout Script](developing-blueprints-code-layout.md)
+ [Creating the Configuration File](developing-blueprints-code-config.md)
+ [Specifying Blueprint Parameters](developing-blueprints-code-parameters.md)