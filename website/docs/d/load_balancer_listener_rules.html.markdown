---
subcategory: "Load Balancer"
layout: "oci"
page_title: "Oracle Cloud Infrastructure: oci_load_balancer_listener_rules"
sidebar_current: "docs-oci-datasource-load_balancer-listener_rules"
description: |-
  Provides the list of Listener Rules in Oracle Cloud Infrastructure Load Balancer service
---

# Data Source: oci_load_balancer_listener_rules
This data source provides the list of Listener Rules in Oracle Cloud Infrastructure Load Balancer service.

Lists all of the rules from all of the rule sets associated with the specified listener. The response organizes
the rules in the following order:

*  Access control rules
*  Allow method rules
*  Request header rules
*  Response header rules


## Example Usage

```hcl
data "oci_load_balancer_listener_rules" "test_listener_rules" {
	#Required
	listener_name = "${oci_load_balancer_listener.test_listener.name}"
	load_balancer_id = "${oci_load_balancer_load_balancer.test_load_balancer.id}"
}
```

## Argument Reference

The following arguments are supported:

* `listener_name` - (Required) The name of the listener the rules are associated with. 
* `load_balancer_id` - (Required) The [OCID](https://docs.cloud.oracle.com/iaas/Content/General/Concepts/identifiers.htm) of the load balancer associated with the listener.


## Attributes Reference

The following attributes are exported:

* `listener_rules` - The list of listener_rules.

### ListenerRule Reference

The following attributes are exported:

* `name` - The name of the rule set that the rule belongs to. 
* `rule` - A rule object that applies to the listener.
	* `action` - The action can be one of these values: `ADD_HTTP_REQUEST_HEADER`, `ADD_HTTP_RESPONSE_HEADER`, `ALLOW`, `CONTROL_ACCESS_USING_HTTP_METHODS`, `EXTEND_HTTP_REQUEST_HEADER_VALUE`, `EXTEND_HTTP_RESPONSE_HEADER_VALUE`, `REDIRECT`, `REMOVE_HTTP_REQUEST_HEADER`, `REMOVE_HTTP_RESPONSE_HEADER`
	* `allowed_methods` - The list of HTTP methods allowed for this listener.

		By default, you can specify only the standard HTTP methods defined in the [HTTP Method Registry](http://www.iana.org/assignments/http-methods/http-methods.xhtml). You can also see a list of supported standard HTTP methods in the Load Balancing service documentation at [Managing Rule Sets](https://docs.cloud.oracle.com/iaas/Content/Balance/Tasks/managingrulesets.htm).

		Your backend application must be able to handle the methods specified in this list.

		The list of HTTP methods is extensible. If you need to configure custom HTTP methods, contact [My Oracle Support](http://support.oracle.com/) to remove the restriction for your tenancy.

		Example: ["GET", "PUT", "POST", "PROPFIND"] 
	* `conditions` - 
		* `attribute_name` - The attribute_name can be one of these values: `PATH`, `SOURCE_IP_ADDRESS`, `SOURCE_VCN_ID`, `SOURCE_VCN_IP_ADDRESS`
		* `attribute_value` - The [OCID](https://docs.cloud.oracle.com/iaas/Content/General/Concepts/identifiers.htm) of the originating VCN that an incoming packet must match.

			You can use this condition in conjunction with `SourceVcnIpAddressCondition`.

			**NOTE:** If you define this condition for a rule without a `SourceVcnIpAddressCondition`, this condition matches all incoming traffic in the specified VCN. 
		* `operator` - A string that specifies how to compare the PathMatchCondition object's `attributeValue` string to the incoming URI.
			*  **EXACT_MATCH** - The incoming URI path must exactly and completely match the `attributeValue` string.
			*  **FORCE_LONGEST_PREFIX_MATCH** - The system looks for the `attributeValue` string with the best, longest match of the beginning portion of the incoming URI path.
			*  **PREFIX_MATCH** - The beginning portion of the incoming URI path must exactly match the `attributeValue` string.
			*  **SUFFIX_MATCH** - The ending portion of the incoming URI path must exactly match the `attributeValue` string. 
	* `description` - A brief description of the access control rule. Avoid entering confidential information.

		example: `192.168.0.0/16 and 2001:db8::/32 are trusted clients. Whitelist them.` 
	* `header` - A header name that conforms to RFC 7230.  Example: `example_header_name` 
	* `prefix` - A string to prepend to the header value. The resulting header value must still conform to RFC 7230.  Example: `example_prefix_value` 
	* `redirect_uri` - 
		* `host` - The valid domain name (hostname) or IP address to use in the redirect URI.

			When this value is null, not set, or set to `{host}`, the service preserves the original domain name from the incoming HTTP request URI.

			All RedirectUri tokens are valid for this property. You can use any token more than once.

			Curly braces are valid in this property only to surround tokens, such as `{host}`

			Examples:
			*  **example.com** appears as `example.com` in the redirect URI.
			*  **in{host}** appears as `inexample.com` in the redirect URI if `example.com` is the hostname in the incoming HTTP request URI.
			*  **{port}{host}** appears as `8081example.com` in the redirect URI if `example.com` is the hostname and the port is `8081` in the incoming HTTP request URI. 
		* `path` - The HTTP URI path to use in the redirect URI.

			When this value is null, not set, or set to `{path}`, the service preserves the original path from the incoming HTTP request URI. To omit the path from the redirect URI, set this value to an empty string, "".

			All RedirectUri tokens are valid for this property. You can use any token more than once.

			The path string must begin with `/` if it does not begin with the `{path}` token.

			Examples:
			*  __/example/video/123__ appears as `/example/video/123` in the redirect URI.
			*  __/example{path}__ appears as `/example/video/123` in the redirect URI if `/video/123` is the path in the incoming HTTP request URI.
			*  __{path}/123__ appears as `/example/video/123` in the redirect URI if `/example/video` is the path in the incoming HTTP request URI.
			*  __{path}123__ appears as `/example/video123` in the redirect URI if `/example/video` is the path in the incoming HTTP request URI.
			*  __/{host}/123__ appears as `/example.com/123` in the redirect URI if `example.com` is the hostname in the incoming HTTP request URI.
			*  __/{host}/{port}__ appears as `/example.com/123` in the redirect URI if `example.com` is the hostname and `123` is the port in the incoming HTTP request URI.
			*  __/{query}__ appears as `/lang=en` in the redirect URI if the query is `lang=en` in the incoming HTTP request URI. 
		* `port` - The communication port to use in the redirect URI.

			Valid values include integers from 1 to 65535.

			When this value is null, the service preserves the original port from the incoming HTTP request URI.

			Example: `8081` 
		* `protocol` - The HTTP protocol to use in the redirect URI.

			When this value is null, not set, or set to `{protocol}`, the service preserves the original protocol from the incoming HTTP request URI. Allowed values are:
			*  HTTP
			*  HTTPS
			*  {protocol}

			`{protocol}` is the only valid token for this property. It can appear only once in the value string.

			Example: `HTTPS` 
		* `query` - The query string to use in the redirect URI.

			When this value is null, not set, or set to `{query}`, the service preserves the original query parameters from the incoming HTTP request URI.

			All `RedirectUri` tokens are valid for this property. You can use any token more than once.

			If the query string does not begin with the `{query}` token, it must begin with the question mark (?) character.

			You can specify multiple query parameters as a single string. Separate each query parameter with an ampersand (&) character. To omit all incoming query parameters from the redirect URI, set this value to an empty string, "".

			If the specified query string results in a redirect URI ending with `?` or `&`, the last character is truncated. For example, if the incoming URI is `http://host.com:8080/documents` and the query property value is `?lang=en&{query}`, the redirect URI is `http://host.com:8080/documents?lang=en`. The system truncates the final ampersand (&) because the incoming URI included no value to replace the {query} token.

			Examples:
			* **lang=en&time_zone=PST** appears as `lang=en&time_zone=PST` in the redirect URI.
			* **{query}** appears as `lang=en&time_zone=PST` in the redirect URI if `lang=en&time_zone=PST` is the query string in the incoming HTTP request. If the incoming HTTP request has no query parameters, the `{query}` token renders as an empty string.
			* **lang=en&{query}&time_zone=PST** appears as `lang=en&country=us&time_zone=PST` in the redirect URI if `country=us` is the query string in the incoming HTTP request. If the incoming HTTP request has no query parameters, this value renders as `lang=en&time_zone=PST`.
			*  **protocol={protocol}&hostname={host}** appears as `protocol=http&hostname=example.com` in the redirect URI if the protocol is `HTTP` and the hostname is `example.com` in the incoming HTTP request.
			*  **port={port}&hostname={host}** appears as `port=8080&hostname=example.com` in the redirect URI if the port is `8080` and the hostname is `example.com` in the incoming HTTP request URI. 
	* `response_code` - The HTTP status code to return when the incoming request is redirected.

		The status line returned with the code is mapped from the standard HTTP specification. Valid response codes for redirection are:
		*  301
		*  302
		*  303
		*  307
		*  308

		The default value is `302` (Found).

		Example: `301` 
	* `status_code` - The HTTP status code to return when the requested HTTP method is not in the list of allowed methods. The associated status line returned with the code is mapped from the standard HTTP specification. The default value is `405 (Method Not Allowed)`.  Example: 403 
	* `suffix` - A string to append to the header value. The resulting header value must still conform to RFC 7230.  Example: `example_suffix_value` 
	* `value` - A header value that conforms to RFC 7230.  Example: `example_value` 

