---

copyright:
  years: 2017, 2020
lastupdated: "2020-02-18"

keywords: terraform provider plugin, terraform provider cos, terraform resources cos, terraform resources object storage, create bucket with terraform

subcollection: terraform

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:download: .download}
{:preview: .preview}
{:external: target="_blank" .external}

# Object Storage data sources
{: #object-storage-data-sources}

Review the data sources that you can use to retrieve information about your {{site.data.keyword.cos_full_notm}} instance. All data sources are imported as read-only information. You can reference the output parameters for each data source by using Terraform interpolation syntax. 
{: shortdesc}

## `ibm_cos_bucket`
{: #cos-bucket}

Retrieve information about an {{site.data.keyword.cos_full_notm}} bucket. 
{: shortdesc}

### Sample Terraform code
{: #cos-bucket-sample}

The following example shows how to retrieve information about your {{site.data.keyword.cos_full_notm}} service instance and the bucket. 
{: shortdesc}

```hcl
data "ibm_resource_group" "cos_group" {
  name = "cos-resource-group"
}

data "ibm_resource_instance" "cos_instance" {
  name              = "cos-instance"
  resource_group_id = "${data.ibm_resource_group.cos_group.id}"
  service           = "cloud-object-storage"
}

data "ibm_cos_bucket" "standard-ams03" {
  bucket_name = "a-standard-bucket-at-ams"
  resource_instance_id = "${data.ibm_resource_instance.cos_instance.id}"
  bucket_type = "single_site_location"
  region = "ams03"
}

output "bucket_private_endpoint" {
  value = "${data.ibm_cos_bucket.standard-ams03.s3_endpoint_private}"
}
```

### Input parameters
{: #cos-bucket-input}

Review the input parameters that you can specify for your data source. 
{: shortdesc}

| Input parameter | Data type | Required/ optional | Description |
| ------------- |-------------| ----- | -------------- |
| `bucket_name` | String | Required | The name of the bucket. |
| `bucket_region` | String | Required | The region of the bucket. |
| `bucket_type` | String | Required | The type of the bucket. Supported values are `single_site_location`, `region_location`, and `cross_region_location`.  |
| `resource_instance_id` | String | Required | The ID of the {{site.data.keyword.cos_full_notm}} service instance for which you want to create a bucket. |

### Output parameters
{: #cos-bucket-output}

Review the output parameters that you can access after you retrieved your data source. 
{: shortdesc}

| Output parameter | Data type | Description |
| ------------- |-------------| -------------- |
| `crn` | String | The CRN of the bucket. |
| `cross_region_location` | String | The location if you created a cross-regional bucket. |
| `id` | String | The ID of the bucket. | 
| `key_protect` | String | The CRN of the {{site.data.keyword.keymanagementservicelong_notm}} instance that you use to encrypt your data in {{site.data.keyword.cos_full_notm}}. |
| `region_location` | String | The location if you created a regional bucket. |
| `resource_instance_id` | String | The ID of {site.data.keyword.cos_full_notm}} instance. | 
| `single_site_location` | String | The location if you created a single site bucket. |
| `storage_class` | String | The storage class of the bucket. |
