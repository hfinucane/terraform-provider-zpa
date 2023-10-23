---
subcategory: "Log Streaming (LSS)"
layout: "zscaler"
page_title: "ZPA: lss_config_controller"
description: |-
  Creates and manages ZPA LSS Configuration for App Connector Metrics.
---

# Resource: zpa_lss_config_controller

The **zpa_lss_config_controller** resource creates and manages Log Streaming Service (LSS) in the Zscaler Private Access cloud for App Connector Metrics `zpn_ast_comprehensive_stats`.

## Example 1 - LSS App Connector Metrics - Usage

```hcl

# Get Log Type Format - "App Connector Metrics"
data "zpa_lss_config_log_type_formats" "zpn_ast_comprehensive_stats" {
  log_type = "zpn_ast_comprehensive_stats"
}

data "zpa_policy_type" "lss_siem_policy" {
  policy_type = "SIEM_POLICY"
}

data "zpa_app_connector_group" "this" {
 name = "Example100"
}
resource "zpa_lss_config_controller" "lss_app_connector_metrics" {
  config {
    name            = "LSS App Connector Metrics"
    description     = "LSS App Connector Metrics"
    enabled         = true
    format          = data.zpa_lss_config_log_type_formats.zpn_ast_comprehensive_stats.json
    lss_host        = "splunk1.acme.com"
    lss_port        = "5001"
    source_log_type = "zpn_ast_comprehensive_stats"
    use_tls         = true
  }
  connector_groups {
    id = [ data.zpa_app_connector_group.this.id ]
  }
}
```

## Argument Reference

The following arguments are supported:

### Required

* `config` - (Required)
  * `name` - (Required)
  * `format` - (Required) The format of the LSS resource. The supported formats are: `JSON`, `CSV`, and `TSV`
  * `lss_host` - (Required) The IP or FQDN of the SIEM (Log Receiver) where logs will be forwarded to.
  * `lss_port` - (Required) The destination port of the SIEM (Log Receiver) where logs will be forwarded to.
  * `source_log_type` - (Required) For `App Connector Metrics` logs use `zpn_ast_comprehensive_stats`. Refer to the [Log Type documentation](https://registry.terraform.io/providers/zscaler/zpa/latest/docs/data-sources/zpa_lss_config_log_type_formats).
  * `connector_groups` - (Required)
        - `id` - (Required) - App Connector Group ID(s) where logs will be forwarded to.

## Attribute Reference

In addition to all arguments above, the following attributes are exported:

* `config` - (Required)
  * `description` - (Optional)
  * `enabled` - (Optional)
  * `use_tls` - (Optional)
  * `source_log_type` - (Required) For `App Connector Metrics` logs use `zpn_http_trans_log`. Refer to the [Log Type documentation](https://registry.terraform.io/providers/zscaler/zpa/latest/docs/data-sources/zpa_lss_config_log_type_formats).
    * `zpn_trans_log - "User Activity"`
    * `zpn_auth_log - "User Status"`
    * `zpn_ast_auth_log - "App Connector Status"`
    * `zpn_http_trans_log - "Web Browser"`
    * `zpn_audit_log - "Audit Logs"`
    * `zpn_sys_auth_log - "Private Service Edge Status"`
    * `zpn_ast_comprehensive_stats - "App Connector Metrics"`
    * `zpn_pbroker_comprehensive_stats - "Private Service Edge Metrics"`
    * `zpn_waf_http_exchanges_log`

  * `connector_groups` - (Required)
        - `id` - (Required) - App Connector Group ID(s) where logs will be forwarded to.

## LSS Source Log Type Table

|       Source Log Type                     |            Description                 |
|-------------------------------------------|----------------------------------------|
|        `zpn_trans_log`                    |        `User Activity`                 |
|        `zpn_auth_log`                     |         `User Status`                  |
|        `zpn_ast_auth_log`                 |        `App Connector Status`          |
|        `zpn_http_trans_log`               |         `Web Browser`                  |
|        `zpn_audit_log`                    |         `Audit Logs`                   |
|        `zpn_sys_auth_log`                 |         `Private Service Edge Status`  |
|        `zpn_ast_comprehensive_stats`      |         `App Connector Metrics`        |
|        `zpn_pbroker_comprehensive_stats`  |         `Private Service Edge Metrics` |
|        `zpn_waf_http_exchanges_log`       |         `ZPA App Protection`           |

## Import

Zscaler offers a dedicated tool called Zscaler-Terraformer to allow the automated import of ZPA configurations into Terraform-compliant HashiCorp Configuration Language.
[Visit](https://github.com/zscaler/zscaler-terraformer)