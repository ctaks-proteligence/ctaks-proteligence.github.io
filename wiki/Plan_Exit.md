# Plan Exit

Add support to the Plan Exit file for an optional Environment
specification that if defined will restrict files for the region to
those that match the specified Environment header value. Expected values
are as follows:

- PROD-ENV and
- TEST-ENV.

However, the field should support custom values.

If the option is included in the Plan Exit file for a given region:

- An 8-character value should be required.
- The file should fail validation if the Environment header value -
  X-HDR-ENVIRONMENT in positions 4-11 per the BCBSA documentation -
  doesn't match the defined value.
- It should be definable at the Master Station Code level at a minimum.
