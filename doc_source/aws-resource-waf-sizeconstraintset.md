# AWS::WAF::SizeConstraintSet<a name="aws-resource-waf-sizeconstraintset"></a>

The `AWS::WAF::SizeConstraintSet` resource specifies a size constraint that AWS WAF uses to check the size of a web request and which parts of the request to check\. For more information, see [CreateSizeConstraintSet](http://docs.aws.amazon.com/waf/latest/APIReference/API_CreateSizeConstraintSet.html) in the *AWS WAF API Reference*\.


+ [Syntax](#aws-resource-waf-sizeconstraintset-syntax)
+ [Properties](#w3ab2c21c10e1051b9)
+ [Return Value](#w3ab2c21c10e1051c11)
+ [Examples](#w3ab2c21c10e1051c13)

## Syntax<a name="aws-resource-waf-sizeconstraintset-syntax"></a>

To declare this entity in your AWS CloudFormation template, use the following syntax:

### JSON<a name="aws-resource-waf-sizeconstraintset-syntax.json"></a>

```
{
  "Type" : "AWS::WAF::SizeConstraintSet",
  "Properties" : {
    "[[ERROR] BAD/MISSING LINK TEXT](#cfn-waf-sizeconstraintset-name)" : String,
    "[[ERROR] BAD/MISSING LINK TEXT](#cfn-waf-sizeconstraintset-sizeconstraints)" : [ SizeConstraint, ... ]
  }
}
```

### YAML<a name="aws-resource-waf-sizeconstraintset-syntax.yaml"></a>

```
Type: "AWS::WAF::SizeConstraintSet"
Properties: 
  [[ERROR] BAD/MISSING LINK TEXT](#cfn-waf-sizeconstraintset-name): String
  [[ERROR] BAD/MISSING LINK TEXT](#cfn-waf-sizeconstraintset-sizeconstraints):
    - SizeConstraint
```

## Properties<a name="w3ab2c21c10e1051b9"></a>

`Name`  
A friendly name or description for the `SizeConstraintSet`\.  
*Required: *Yes  
*Type*: String  
*Update requires*: Replacement

`SizeConstraints`  
The size constraint and the part of the web request to check\.  
*Required: *Yes  
*Type*: List of [AWS WAF SizeConstraintSet SizeConstraint](aws-properties-waf-sizeconstraintset-sizeconstraint.md)  
*Update requires*: No interruption

## Return Value<a name="w3ab2c21c10e1051c11"></a>

### Ref<a name="w3ab2c21c10e1051c11b2"></a>

When the logical ID of this resource is provided to the `Ref` intrinsic function, `Ref` returns the resource physical ID, such as `1234a1a-a1b1-12a1-abcd-a123b123456`\.

For more information about using the `Ref` function, see Ref\.

## Examples<a name="w3ab2c21c10e1051c13"></a>

The following examples show you how to define a size constraint, add it to a rule, and add the rule to a web access control list \(ACL\)\.

### Define a Size Constraint<a name="w3ab2c21c10e1051c13b4"></a>

The following example checks that the body of an HTTP request equals `4096` bytes\.

#### JSON<a name="aws-resource-waf-sizeconstraintset-example1.json"></a>

```
"MySizeConstraint": {
  "Type": "AWS::WAF::SizeConstraintSet",
  "Properties": {
    "Name": "SizeConstraints",
    "SizeConstraints": [
      {
        "ComparisonOperator": "EQ",
        "FieldToMatch": {
          "Type": "BODY"
        },
        "Size": "4096",
        "TextTransformation": "NONE"
      }
    ]
  }
}
```

#### YAML<a name="aws-resource-waf-sizeconstraintset-example1.yaml"></a>

```
  MySizeConstraint: 
    Type: "AWS::WAF::SizeConstraintSet"
    Properties: 
      Name: "SizeConstraints"
      SizeConstraints: 
        - 
          ComparisonOperator: "EQ"
          FieldToMatch: 
            Type: "BODY"
          Size: "4096"
          TextTransformation: "NONE"
```

### Associate a `SizeConstraintSet` with a Web ACL Rule<a name="w3ab2c21c10e1051c13b6"></a>

The following example associates the `MySizeConstraint` object with a web ACL rule\.

#### JSON<a name="aws-resource-waf-sizeconstraintset-example2.json"></a>

```
"SizeConstraintRule" : {
  "Type": "AWS::WAF::Rule",
  "Properties": {
    "Name": "SizeConstraintRule",
    "MetricName" : "SizeConstraintRule",
    "Predicates": [
      {
        "DataId" : {  "Ref" : "MySizeConstraint" },
        "Negated" : false,
        "Type" : "SizeConstraint"
      }
    ]
  }
}
```

#### YAML<a name="aws-resource-waf-sizeconstraintset-example2.yaml"></a>

```
SizeConstraintRule: 
  Type: "AWS::WAF::Rule"
  Properties: 
    Name: "SizeConstraintRule"
    MetricName: "SizeConstraintRule"
    Predicates: 
      - 
        DataId: 
          Ref: "MySizeConstraint"
        Negated: false
        Type: "SizeConstraint"
```

### Create a Web ACL<a name="w3ab2c21c10e1051c13b8"></a>

The following example associates the `SizeConstraintRule` rule with a web ACL\. The web ACL blocks all requests except for requests with a body size equal to `4096` bytes\.

#### JSON<a name="aws-resource-waf-sizeconstraintset-example3.json"></a>

```
"MyWebACL": {
  "Type": "AWS::WAF::WebACL",
  "Properties": {
    "Name": "Web ACL to allow requests with a specific size",
    "DefaultAction": {
      "Type": "BLOCK"
    },
    "MetricName" : "SizeConstraintWebACL",
    "Rules": [
      {
        "Action" : {
          "Type" : "ALLOW"
        },
        "Priority" : 1,
        "RuleId" : { "Ref" : "SizeConstraintRule" }
      }
    ]
  }
}
```

#### YAML<a name="aws-resource-waf-sizeconstraintset-example3.yaml"></a>

```
MyWebACL: 
  Type: "AWS::WAF::WebACL"
  Properties: 
    Name: "Web ACL to allow requests with a specific size"
    DefaultAction: 
      Type: "BLOCK"
    MetricName: "SizeConstraintWebACL"
    Rules: 
      - 
        Action: 
          Type: "ALLOW"
        Priority: 1
        RuleId: 
          Ref: "SizeConstraintRule"
```