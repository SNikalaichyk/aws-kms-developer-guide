# Working With Keys<a name="programming-keys"></a>

The examples in this topic use the AWS KMS API to create, view, enable, and disable AWS KMS customer master keys, and to generate data keys\.

**Topics**
+ [Creating a Customer Master Key](#creating-keys)
+ [Generating a Data Key](#generate-datakeys)
+ [Viewing a Custom Master Key](#describing-keys)
+ [Getting Key IDs and Key ARNs of Customer Master Keys](#listing-keys)
+ [Enabling Customer Master Keys](#enable-keys)
+ [Disabling Customer Master Keys](#disable-keys)

## Creating a Customer Master Key<a name="creating-keys"></a>

To create a [customer master key](concepts.md#master_keys) \(CMK\), use the [CreateKey](https://docs.aws.amazon.com/kms/latest/APIReference/API_CreateKey.html) operation\. The examples in this section create a symmetric CMK\. The `Description` parameter used in these examples is optional\.

In languages that require a client object, these examples use the AWS KMS client object that you created in [Creating a Client](programming-client.md)\.

------
#### [ Java ]

For details, see the [createKey method](https://docs.aws.amazon.com/AWSJavaSDK/latest/javadoc/com/amazonaws/services/kms/AWSKMSClient.html#createKey-com.amazonaws.services.kms.model.CreateKeyRequest-) in the *AWS SDK for Java API Reference*\.

```
// Create a CMK
//
String desc = "Key for protecting critical data";
    
CreateKeyRequest req = new CreateKeyRequest().withDescription(desc);
CreateKeyResult result = kmsClient.createKey(req);
```

------
#### [ C\# ]

For details, see the [CreateKey method](https://docs.aws.amazon.com/sdkfornet/v3/apidocs/items/KeyManagementService/MKeyManagementServiceCreateKeyCreateKeyRequest.html) in the *AWS SDK for \.NET*\.

```
// Create a CMK
//
String desc = "Key for protecting critical data";

CreateKeyRequest req = new CreateKeyRequest()
{
    Description = desc
};
CreateKeyResponse response = kmsClient.CreateKey(req);
```

------
#### [ Python ]

For details, see the [create\_key method](http://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/kms.html#KMS.Client.create_key) in the AWS SDK for Python \(Boto 3\)\.

```
# Create a CMK

desc = 'Key for protecting critical data'

response = kms_client.create_key(
    Description=desc
)
```

------
#### [ Ruby ]

For details, see the [create\_key](https://docs.aws.amazon.com/sdk-for-ruby/v3/api/Aws/KMS/Client.html#create_key-instance_method) instance method in the [AWS SDK for Ruby](https://docs.aws.amazon.com/sdk-for-ruby/v3/api/Aws/KMS.html)\.

```
# Create a CMK

desc = 'Key for protecting critical data'

response = kmsClient.create_key({
  description: desc
})
```

------
#### [ PHP ]

For details, see the [CreateKey method](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-kms-2014-11-01.html#createkey) in the *AWS SDK for PHP*\.

```
// Create a CMK
//
$desc = "Key for protecting critical data";

$result = $KmsClient->createKey([
    'Description' => $desc
]);
```

------
#### [ Node\.js ]

For details, see the [createKey property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/KMS.html#createKey-property) in the *AWS SDK for JavaScript in Node\.js*\.

```
// Create a CMK
//
const Description = 'Key for protecting critical data';

kmsClient.createKey({ Description }, (err, data) => {
  ...
});
```

------
#### [ PowerShell ]

To create a CMK in PowerShell, use the [New\-KmsKey](https://docs.aws.amazon.com/powershell/latest/reference/items/New-KMSKey.html) cmdlet\.

```
# Create a CMK

$desc = 'Key for protecting critical data'
New-KmsKey -Description $desc
```

------

## Generating a Data Key<a name="generate-datakeys"></a>

To generate a symmetric data key, use the [GenerateDataKey](https://docs.aws.amazon.com/kms/latest/APIReference/API_GenerateDataKey.html) operation\. This operation takes a symmetric CMK and returns a plaintext data key and a copy of that data key encrypted under the CMK that you specified\. You must specify either a `KeySpec` or `NumberOfBytes` \(but not both\) in each command\.

In languages that require a client object, these examples use the AWS KMS client object that you created in [Creating a Client](programming-client.md)\.

------
#### [ Java ]

For details, see the [generateDataKey method](https://docs.aws.amazon.com/AWSJavaSDK/latest/javadoc/com/amazonaws/services/kms/AWSKMSClient.html#generateDataKey-com.amazonaws.services.kms.model.GenerateDataKeyRequest-) in the *AWS SDK for Java API Reference*\.

```
// Generate a data key
//
// Replace the following fictitious CMK ARN with a valid CMK ID or ARN
String keyId = "arn:aws:kms:us-west-2:111122223333:key/1234abcd-12ab-34cd-56ef-1234567890ab";

GenerateDataKeyRequest dataKeyRequest = new GenerateDataKeyRequest();
dataKeyRequest.setKeyId(keyId);
dataKeyRequest.setKeySpec("AES_256");

GenerateDataKeyResult dataKeyResult = kmsClient.generateDataKey(dataKeyRequest);

ByteBuffer plaintextKey = dataKeyResult.getPlaintext();

ByteBuffer encryptedKey = dataKeyResult.getCiphertextBlob();
```

------
#### [ C\# ]

For details, see the [GenerateDataKey method](https://docs.aws.amazon.com/sdkfornet/v3/apidocs/items/KeyManagementService/MKeyManagementServiceGenerateDataKeyGenerateDataKeyRequest.html) in the *AWS SDK for \.NET*\.

```
// Generate a data key
//
// Replace the following fictitious CMK ARN with a valid CMK ID or ARN
String keyId = "arn:aws:kms:us-west-2:111122223333:key/1234abcd-12ab-34cd-56ef-1234567890ab";
GenerateDataKeyRequest dataKeyRequest = new GenerateDataKeyRequest()
{
    KeyId = keyId,
    KeySpec = DataKeySpec.AES_256
};

GenerateDataKeyResponse dataKeyResponse = kmsClient.GenerateDataKey(dataKeyRequest);

MemoryStream plaintextKey = dataKeyResponse.Plaintext;

MemoryStream encryptedKey = dataKeyResponse.CiphertextBlob;
```

------
#### [ Python ]

For details, see the [generate\_date\_key method](http://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/kms.html#KMS.Client.generate_data_key) in the AWS SDK for Python \(Boto 3\)\.

```
# Generate a data key

# Replace the following fictitious CMK ARN with a valid CMK ID or ARN
key_id = 'arn:aws:kms:us-west-2:111122223333:key/1234abcd-12ab-34cd-56ef-1234567890ab'

response = kms_client.generate_data_key(
    KeyId=key_id,
    KeySpec='AES_256'
)

plaintext_key = response['Plaintext']

encrypted_key = response['CiphertextBlob']
```

------
#### [ Ruby ]

For details, see the [generate\_data\_key](https://docs.aws.amazon.com/sdk-for-ruby/v3/api/Aws/KMS/Client.html#generate_data_key-instance_method) instance method in the [AWS SDK for Ruby](https://docs.aws.amazon.com/sdk-for-ruby/v3/api/Aws/KMS.html)\.

```
# Generate a data key

# Replace the following fictitious CMK ARN with a valid CMK ID or ARN
keyId = 'arn:aws:kms:us-west-2:111122223333:key/1234abcd-12ab-34cd-56ef-1234567890ab'

response = kmsClient.generate_data_key({
  key_id: keyId,
  key_spec: 'AES_256'
})

plaintextKey = response.plaintext

encryptedKey = response.ciphertext_blob
```

------
#### [ PHP ]

For details, see the [GenerateDataKey method](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-kms-2014-11-01.html#generatedatakey) in the *AWS SDK for PHP*\.

```
// Generate a data key
//
// Replace the following fictitious CMK ARN with a valid CMK ID or ARN
$keyId = 'arn:aws:kms:us-west-2:111122223333:key/1234abcd-12ab-34cd-56ef-1234567890ab';
$keySpec = 'AES_256';

$result = $KmsClient->generateDataKey([
    'KeyId' => $keyId, 
    'KeySpec' => $keySpec,
]);

$plaintextKey = $result['Plaintext'];

$encryptedKey = $result['CiphertextBlob'];
```

------
#### [ Node\.js ]

For details, see the [generateDataKey property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/KMS.html#generateDataKey-property) in the *AWS SDK for JavaScript in Node\.js*\.

```
// Generate a data key
//
// Replace the following fictitious CMK ARN with a valid CMK ID or ARN
const KeyId = 'arn:aws:kms:us-west-2:111122223333:key/1234abcd-12ab-34cd-56ef-1234567890ab';
const KeySpec = 'AES_256';
kmsClient.generateDataKey({ KeyId, KeySpec }, (err, data) => {
  if (err) console.log(err, err.stack);
  else {
    const { CiphertextBlob, Plaintext } = data;
      ...
  }
});
```

------
#### [ PowerShell ]

To generate a symmetric data key, use the [New\-KMSDataKey](https://docs.aws.amazon.com/powershell/latest/reference/items/New-KMSDataKey.html) cmdlet\.

In the output, the plaintext key \(in the `Plaintext` property\) and the encrypted key \(in the `CiphertextBlob` property\) are [MemoryStream](https://docs.microsoft.com/en-us/dotnet/api/system.io.memorystream) objects\. To convert them to strings, use the methods of the `MemoryStream` class, or a cmdlet or function that converts `MemoryStream` objects to strings, such as the [ConvertFrom\-MemoryStream](https://convert.readthedocs.io/en/latest/functions/ConvertFrom-MemoryStream/) and [ConvertFrom\-Base64](https://convert.readthedocs.io/en/latest/functions/ConvertFrom-Base64/) functions in the [Convert](https://www.powershellgallery.com/packages/Convert/) module\.

```
# Generate a data key

# Replace the following fictitious CMK ARN with a valid CMK ID or ARN  
$keyId = 'arn:aws:kms:us-west-2:111122223333:key/1234abcd-12ab-34cd-56ef-1234567890ab'
$keySpec = 'AES_256'

$response = New-KmsDataKey -KeyId $keyId -KeySpec $keySpec
$plaintextKey = $response.Plaintext
$encryptedKey = $response.CiphertextBlob
```

------

## Viewing a Custom Master Key<a name="describing-keys"></a>

To get detailed information about a customer master key \(CMK\), including the CMK ARN and [key state](key-state.md), use the [DescribeKey](https://docs.aws.amazon.com/kms/latest/APIReference/API_DescribeKey.html) operation\. 

`DescribeKey` does not get aliases\. To get aliases, use the [ListAliases](https://docs.aws.amazon.com/kms/latest/APIReference/API_ListAliases.html) operation\. For examples, see [Working with Aliases](programming-aliases.md)\.

In languages that require a client object, these examples use the AWS KMS client object that you created in [Creating a Client](programming-client.md)\.

------
#### [ Java ]

For details, see the [describeKey method](https://docs.aws.amazon.com/AWSJavaSDK/latest/javadoc/com/amazonaws/services/kms/AWSKMS.html#describeKey-com.amazonaws.services.kms.model.DescribeKeyRequest-) in the *AWS SDK for Java API Reference*\.

```
// Describe a CMK
//
// Replace the following fictitious CMK ARN with a valid CMK ID or ARN
String keyId = "arn:aws:kms:us-west-2:111122223333:key/1234abcd-12ab-34cd-56ef-1234567890ab";

DescribeKeyRequest req = new DescribeKeyRequest().withKeyId(keyId);
DescribeKeyResult result = kmsClient.describeKey(req);
```

------
#### [ C\# ]

For details, see the [DescribeKey method](https://docs.aws.amazon.com/sdkfornet/v3/apidocs/items/KeyManagementService/MKeyManagementServiceDescribeKeyDescribeKeyRequest.html) in the *AWS SDK for \.NET*\. 

```
// Describe a CMK
//
// Replace the following fictitious CMK ARN with a valid CMK ID or ARN
String keyId = "arn:aws:kms:us-west-2:111122223333:key/1234abcd-12ab-34cd-56ef-1234567890ab";

DescribeKeyRequest describeKeyRequest = new DescribeKeyRequest()
{
    KeyId = keyId
};

DescribeKeyResponse describeKeyResponse = kmsClient.DescribeKey(describeKeyRequest);
```

------
#### [ Python ]

For details, see the [describe\_key method](http://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/kms.html#KMS.Client.describe_key) in the AWS SDK for Python \(Boto 3\)\.

```
# Describe a CMK

# Replace the following fictitious CMK ARN with a valid CMK ID or ARN
key_id = 'arn:aws:kms:us-west-2:111122223333:key/1234abcd-12ab-34cd-56ef-1234567890ab'

response = kms_client.describe_key(
    KeyId=key_id
)
```

------
#### [ Ruby ]

For details, see the [describe\_key](https://docs.aws.amazon.com/sdk-for-ruby/v3/api/Aws/KMS/Client.html#describe_key-instance_method) instance method in the [AWS SDK for Ruby](https://docs.aws.amazon.com/sdk-for-ruby/v3/api/Aws/KMS.html)\.

```
# Describe a CMK

# Replace the following fictitious CMK ARN with a valid CMK ID or ARN
keyId = 'arn:aws:kms:us-west-2:111122223333:key/1234abcd-12ab-34cd-56ef-1234567890ab'

response = kmsClient.describe_key({
  key_id: keyId
})
```

------
#### [ PHP ]

For details, see the [DescribeKey method](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-kms-2014-11-01.html#describekey) in the *AWS SDK for PHP*\.

```
// Describe a CMK
//
// Replace the following fictitious CMK ARN with a valid CMK ID or ARN
$keyId = 'arn:aws:kms:us-west-2:111122223333:key/1234abcd-12ab-34cd-56ef-1234567890ab';

$result = $KmsClient->describeKey([
    'KeyId' => $keyId, 
]);
```

------
#### [ Node\.js ]

For details, see the [describeKey property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/KMS.html#describeKey-property) in the *AWS SDK for JavaScript in Node\.js*\.

```
// Describe a CMK
//
// Replace the following fictitious CMK ARN with a valid CMK ID or ARN
const KeyId = 'arn:aws:kms:us-west-2:111122223333:key/1234abcd-12ab-34cd-56ef-1234567890ab';
kmsClient.describeKey({ KeyId }, (err, data) => {
  ...
});
```

------
#### [ PowerShell ]

To get detailed information about a CMK, use the [Get\-KmsKey](https://docs.aws.amazon.com/powershell/latest/reference/items/Get-KMSKey.html) cmdlet\.

```
# Describe a CMK

# Replace the following fictitious CMK ARN with a valid CMK ID or ARN
$keyId = 'arn:aws:kms:us-west-2:111122223333:key/1234abcd-12ab-34cd-56ef-1234567890ab'
Get-KmsKey -KeyId $keyId
```

------

## Getting Key IDs and Key ARNs of Customer Master Keys<a name="listing-keys"></a>

To get the IDs and ARNs of the customer master keys, use the [ListKeys](https://docs.aws.amazon.com/kms/latest/APIReference/API_ListKeys.html) operation\. These examples use the optional `Limit` parameter, which sets the maximum number of CMKs returned in each call\. 

In languages that require a client object, these examples use the AWS KMS client object that you created in [Creating a Client](programming-client.md)\.

------
#### [ Java ]

For details, see the [listKeys method](https://docs.aws.amazon.com/AWSJavaSDK/latest/javadoc/com/amazonaws/services/kms/AWSKMSClient.html#listKeys-com.amazonaws.services.kms.model.ListKeysRequest-) in the *AWS SDK for Java API Reference*\.

```
// List CMKs in this account
//
Integer limit = 10;

ListKeysRequest req = new ListKeysRequest().withLimit(limit);
ListKeysResult result = kmsClient.listKeys(req);
```

------
#### [ C\# ]

For details, see the [ListKeys method](https://docs.aws.amazon.com/sdkfornet/v3/apidocs/items/KeyManagementService/MKeyManagementServiceListKeysListKeysRequest.html) in the *AWS SDK for \.NET*\.

```
// List CMKs in this account
//
int limit = 10;

ListKeysRequest listKeysRequest = new ListKeysRequest()
{
    Limit = limit
};
ListKeysResponse listKeysResponse = kmsClient.ListKeys(listKeysRequest);
```

------
#### [ Python ]

For details, see the [list\_keys method](http://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/kms.html#KMS.Client.list_keys) in the AWS SDK for Python \(Boto 3\)\.

```
# List CMKs in this account

response = kms_client.list_keys(
    Limit=10
)
```

------
#### [ Ruby ]

For details, see the [list\_keys](https://docs.aws.amazon.com/sdk-for-ruby/v3/api/Aws/KMS/Client.html#list_keys-instance_method) instance method in the [AWS SDK for Ruby](https://docs.aws.amazon.com/sdk-for-ruby/v3/api/Aws/KMS.html)\.

```
# List CMKS in this account

response = kmsClient.list_keys({
  limit: 10
})
```

------
#### [ PHP ]

For details, see the [ListKeys method](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-kms-2014-11-01.html#listkeys) in the *AWS SDK for PHP*\.

```
// List CMKs in this account
//
$limit = 10;

$result = $KmsClient->listKeys([
    'Limit' => $limit,
]);
```

------
#### [ Node\.js ]

For details, see the [listKeys property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/KMS.html#listKeys-property) in the *AWS SDK for JavaScript in Node\.js*\.

```
// List CMKs in this account
//
const Limit = 10;
kmsClient.listKeys({ Limit }, (err, data) => {
  ...
});
```

------
#### [ PowerShell ]

To get the key ID and ARN of all CMKs in the account and Region, use the [Get\-KmsKeyList](https://docs.aws.amazon.com/powershell/latest/reference/items/Get-KMSKeyList.html) cmdlet\.

```
# List CMKs in this account

$limit = 10
Get-KmsKeyList -Limit $limit
```

------

## Enabling Customer Master Keys<a name="enable-keys"></a>

To enable a disabled customer master key \(CMK\), use the [EnableKey](https://docs.aws.amazon.com/kms/latest/APIReference/API_EnableKey.html) operation\.

In languages that require a client object, these examples use the AWS KMS client object that you created in [Creating a Client](programming-client.md)\.

------
#### [ Java ]

For details about the Java implementation, see the [enableKey method](https://docs.aws.amazon.com/AWSJavaSDK/latest/javadoc/com/amazonaws/services/kms/AWSKMSClient.html#enableKey-com.amazonaws.services.kms.model.EnableKeyRequest-) in the *AWS SDK for Java API Reference*\.

```
// Enable a CMK
//
// Replace the following fictitious CMK ARN with a valid CMK ID or ARN
String keyId = "arn:aws:kms:us-west-2:111122223333:key/1234abcd-12ab-34cd-56ef-1234567890ab";

EnableKeyRequest req = new EnableKeyRequest().withKeyId(keyId);
kmsClient.enableKey(req);
```

------
#### [ C\# ]

For details, see the [EnableKey method](https://docs.aws.amazon.com/sdkfornet/v3/apidocs/items/KeyManagementService/MKeyManagementServiceEnableKeyEnableKeyRequest.html) in the *AWS SDK for \.NET*\.

```
// Enable a CMK
//
// Replace the following fictitious CMK ARN with a valid CMK ID or ARN
String keyId = "arn:aws:kms:us-west-2:111122223333:key/1234abcd-12ab-34cd-56ef-1234567890ab";

EnableKeyRequest enableKeyRequest = new EnableKeyRequest()
{
    KeyId = keyId
};
kmsClient.EnableKey(enableKeyRequest);
```

------
#### [ Python ]

For details, see the [enable\_key method](http://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/kms.html#KMS.Client.enable_key) in the AWS SDK for Python \(Boto 3\)\.

```
# Enable a CMK

# Replace the following fictitious CMK ARN with a valid CMK ID or ARN
key_id = 'arn:aws:kms:us-west-2:111122223333:key/1234abcd-12ab-34cd-56ef-1234567890ab'

response = kms_client.enable_key(
    KeyId=key_id
)
```

------
#### [ Ruby ]

For details, see the [enable\_key](https://docs.aws.amazon.com/sdk-for-ruby/v3/api/Aws/KMS/Client.html#enable_key-instance_method) instance method in the [AWS SDK for Ruby](https://docs.aws.amazon.com/sdk-for-ruby/v3/api/Aws/KMS.html)\.

```
# Enable a CMK

# Replace the following fictitious CMK ARN with a valid CMK ID or ARN
keyId = 'arn:aws:kms:us-west-2:111122223333:key/1234abcd-12ab-34cd-56ef-1234567890ab'

response = kmsClient.enable_key({
  key_id: keyId
})
```

------
#### [ PHP ]

For details, see the [EnableKey method](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-kms-2014-11-01.html#enablekey) in the *AWS SDK for PHP*\.

```
// Enable a CMK
//
// Replace the following fictitious CMK ARN with a valid CMK ID or ARN
$keyId = 'arn:aws:kms:us-west-2:111122223333:key/1234abcd-12ab-34cd-56ef-1234567890ab';

$result = $KmsClient->enableKey([
    'KeyId' => $keyId, 
]);
```

------
#### [ Node\.js ]

For details, see the [enableKey property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/KMS.html#enableKey-property) in the *AWS SDK for JavaScript in Node\.js*\.

```
// Enable a CMK
//
// Replace the following fictitious CMK ARN with a valid CMK ID or ARN
const KeyId = 'arn:aws:kms:us-west-2:111122223333:key/1234abcd-12ab-34cd-56ef-1234567890ab';
kmsClient.enableKey({ KeyId }, (err, data) => {
  ...
});
```

------
#### [ PowerShell ]

To enable a CMK, use the [Enable\-KmsKey](https://docs.aws.amazon.com/powershell/latest/reference/items/Enable-KMSKey.html) cmdlet\.

```
# Enable a CMK

# Replace the following fictitious CMK ARN with a valid CMK ID or ARN
$keyId = 'arn:aws:kms:us-west-2:111122223333:key/1234abcd-12ab-34cd-56ef-1234567890ab'
Enable-KmsKey -KeyId $keyId
```

------

## Disabling Customer Master Keys<a name="disable-keys"></a>

To disable a CMK, use the [DisableKey](https://docs.aws.amazon.com/kms/latest/APIReference/API_DisableKey.html) operation\. Disabling a CMK prevents it from being used in cryptographic operations\.

In languages that require a client object, these examples use the AWS KMS client object that you created in [Creating a Client](programming-client.md)\.

------
#### [ Java ]

For details, see the [disableKey method](https://docs.aws.amazon.com/AWSJavaSDK/latest/javadoc/com/amazonaws/services/kms/AWSKMSClient.html#disableKey-com.amazonaws.services.kms.model.DisableKeyRequest-) in the *AWS SDK for Java API Reference*\.

```
// Disable a CMK
//
// Replace the following fictitious CMK ARN with a valid CMK ID or ARN
String keyId = "arn:aws:kms:us-west-2:111122223333:key/1234abcd-12ab-34cd-56ef-1234567890ab";

DisableKeyRequest req = new DisableKeyRequest().withKeyId(keyId);
kmsClient.disableKey(req);
```

------
#### [ C\# ]

For details, see the [DisableKey method](https://docs.aws.amazon.com/sdkfornet/v3/apidocs/items/KeyManagementService/MKeyManagementServiceDisableKeyDisableKeyRequest.html) in the *AWS SDK for \.NET*\.

```
// Disable a CMK
//
// Replace the following fictitious CMK ARN with a valid CMK ID or ARN
String keyId = "arn:aws:kms:us-west-2:111122223333:key/1234abcd-12ab-34cd-56ef-1234567890ab";

DisableKeyRequest disableKeyRequest = new DisableKeyRequest()
{
    KeyId = keyId
};
kmsClient.DisableKey(disableKeyRequest);
```

------
#### [ Python ]

For details, see the [disable\_key method](http://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/kms.html#KMS.Client.disable_key) in the AWS SDK for Python \(Boto 3\)\.

```
# Disable a CMK

# Replace the following fictitious CMK ARN with a valid CMK ID or ARN
key_id = 'arn:aws:kms:us-west-2:111122223333:key/1234abcd-12ab-34cd-56ef-1234567890ab'

response = kms_client.disable_key(
    KeyId=key_id
)
```

------
#### [ Ruby ]

For details, see the [disable\_key](https://docs.aws.amazon.com/sdk-for-ruby/v3/api/Aws/KMS/Client.html#disable_key-instance_method) instance method in the [AWS SDK for Ruby](https://docs.aws.amazon.com/sdk-for-ruby/v3/api/Aws/KMS.html)\.

```
# Disable a CMK

# Replace the following fictitious CMK ARN with a valid CMK ID or ARN
keyId = 'arn:aws:kms:us-west-2:111122223333:key/1234abcd-12ab-34cd-56ef-1234567890ab'

response = kmsClient.disable_key({
  key_id: keyId
})
```

------
#### [ PHP ]

For details, see the [DisableKey method](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-kms-2014-11-01.html#disablekey) in the *AWS SDK for PHP*\.

```
// Disable a CMK
//
// Replace the following fictitious CMK ARN with a valid CMK ID or ARN
$keyId = 'arn:aws:kms:us-west-2:111122223333:key/1234abcd-12ab-34cd-56ef-1234567890ab';

$result = $KmsClient->disableKey([
    'KeyId' => $keyId, 
]);
```

------
#### [ Node\.js ]

For details, see the [disableKey property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/KMS.html#disableKey-property) in the *AWS SDK for JavaScript in Node\.js*\.

```
// Disable a CMK
//
// Replace the following fictitious CMK ARN with a valid CMK ID or ARN
const KeyId = 'arn:aws:kms:us-west-2:111122223333:key/1234abcd-12ab-34cd-56ef-1234567890ab';
kmsClient.disableKey({ KeyId }, (err, data) => {
  ...
});
```

------
#### [ PowerShell ]

To disable a CMK, use the [Disable\-KmsKey](https://docs.aws.amazon.com/powershell/latest/reference/items/Disable-KMSKey.html) cmdlet\.

```
# Disable a CMK

# Replace the following fictitious CMK ARN with a valid CMK ID or ARN
$keyId = 'arn:aws:kms:us-west-2:111122223333:key/1234abcd-12ab-34cd-56ef-1234567890ab'
Disable-KmsKey -KeyId $keyId
```

------