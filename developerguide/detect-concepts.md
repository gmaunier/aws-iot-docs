# Concepts<a name="detect-concepts"></a>

**metric**  
AWS IoT Device Defender Detect uses metrics to detect anomalous behavior of devices\. AWS IoT Device Defender Detect compares the reported value of a metric with the expected value you provide\. These metrics can be taken from two sources: cloud\-side metrics and device\-side metrics\. There are 17 total metrics, 6 of which are supported by ML Detect\. For a list of supported metrics for ML Detect, see [Supported metrics](dd-detect-ml.md#dd-detect-ml-metrics)\.  
Abnormal behavior on the AWS IoT network is detected by using cloud\-side metrics such as the number of authorization failures, or the number or size of messages a device sends or receives through AWS IoT\.   
AWS IoT Device Defender Detect can also collect, aggregate, and monitor metrics data generated by AWS IoT devices \(for example, the ports a device is listening on, the number of bytes or packets sent, or the device's TCP connections\)\.  
You can use AWS IoT Device Defender Detect with cloud\-side metrics alone\. To use device\-side metrics, you must first deploy the AWS IoT SDK on your AWS IoT connected devices or device gateways to collect the metrics and send them to AWS IoT\. See [Sending metrics from devices](detect-device-side-metrics.md#DetectMetricsMessages)\. 

**Security Profile**  
A Security Profile defines anomalous behaviors for a group of devices \(a [thing group](thing-groups.md)\) or for all devices in your account, and specifies which actions to take when an anomaly is detected\. You can use the AWS IoT console or API commands to create a Security Profile and associate it with a group of devices\. AWS IoT Device Defender Detect starts recording security\-related data and uses the behaviors defined in the Security Profile to detect anomalies in the behavior of the devices\. 

**behavior**  
A behavior tells AWS IoT Device Defender Detect how to recognize when a device is doing something anomalous\. Any device action that doesn’t match a behavior triggers an alert\. A Rules Detect behavior consists of a metric and an absolute\-value or statistical threshold with an operator \(for example, less than or equal to, greater than or equal to\), which describe the expected device behavior\. An ML Detect behavior consists of a metric and an ML Detect configuration, which set an ML model to learn the normal behavior of devices\.

**ML model**  
An ML model is a machine learning model created to monitor each behavior a customer configures\. The model trains on metric data patterns from targeted device groups and generates three anomaly confidence thresholds \(high, medium, and low\) for the metric\-based behavior\. It inferences anomalies based on ingested metric data at the device level\. In the context of ML Detect, one ML model is created to evaluate one metric\-based behavior\. For more information, see [ML Detect](dd-detect-ml.md)\.

**confidence level**  
ML Detect supports three confidence levels: `High`, `Medium`, and `Low`\. `High` confidence means low sensitivity in anomalous behavior evaluation and frequently a lower number of alarms\. `Medium` confidence means medium sensitivity and `Low` confidence means high sensitivity and frequently a higher number of alarms\.

**dimension**  
You can define a dimension to adjust the scope of a behavior\. For example, you can define a topic filter dimension that applies a behavior to MQTT topics that match a pattern\. For information about defining a dimension for use in a Security Profile, see [CreateDimension](https://docs.aws.amazon.com/iot/latest/apireference/API_CreateDimension.html)\.

**alarm**  
When an anomaly is detected, an alarm notification can be sent through a CloudWatch metric \(see [Using AWS IoT metrics](monitoring-cloudwatch.md#how_to_use_metrics)\) or an SNS notification\. An alarm notification is also displayed in the AWS IoT console along with information about the alarm, and a history of alarms for the device\. An alarm is also sent when a monitored device stops exhibiting anomalous behavior or when it had been causing an alarm but stops reporting for an extended period\.

**alarm verification state**  
After an alarm has been created, you can verify the alarm as True positive, Benign positive, False positive, or Unknown\. You can also add a description to your alarm verification state\. You can view, organize, and filter AWS IoT Device Defender alarms by using one of the four verification states\. You can use alarm verification states and related descriptions to inform members of your team\. This helps your team to take follow\-up actions, for example, performing mitigation actions on True positive alarms, skipping Benign positive alarms, or continuing investigation on Unknown alarms\. The default verification state for all alarms is Unknown\.

**alarm suppression**  
Manage Detect alarm SNS notifications by setting behavior notification to `on` or `suppressed`\. Suppressing alarms doesn't stop Detect from performing device behavior evaluations; Detect continues to flag anomalous behaviors as violation alarms\. However, suppressed alarms wouldn't be forwarded for SNS notification\. They can only be accessed through the AWS IoT console or API\.