# NOSImageProfile Enhancement Requirements

The `NOSImageProfile` has been updated to transition from manual reporting to a structured, machine-readable format. This ensures that critical metadata regarding platform security, release lifecycles, and test validation can be automatically ingested and processed by internal systems and catalogs.

## Enhancement Field Requirements

### 1. Machine-Consumable Test Data
To enable automated coverage analysis and validation tracking, vendors must provide structured test results within the `featureprofile_test_result` field. This structured reporting replaces manual or unstructured updates, providing a unified view of test outcomes.

*   **Field**: `repeated FeatureProfileTestResult featureprofile_test_result = 8;`
*   **Attributes**:
    *   **plan_id**: The unique identifier for the test plan, which must match the metadata defined in the `featureprofiles` repository.
    *   **commit**: The specific git commit hash of the `featureprofiles` repository used for the execution.
    *   **result**: The final status of the test (e.g., `PASSED`, `FAILED`, `NOT_EXECUTED`).

### 2. Platform Configuration Registers (PCRs)
The `pcrs` field stores the expected hardware-specific integrity values for a given NOS image. Providing this data in a structured format allows for automated verification of platform security states.

*   **Field**: `repeated PlatformConfigurationRegister pcrs = 9;`
*   **Attributes**:
    *   **index**: The vendor-specific index number (e.g., 0, 7).
    *   **values**: One or more valid hash values (typically SHA hexadecimal strings).
    *   **name**: (Optional) A human-readable description for the register (e.g., "BIOS", "Secure Boot Policy").

### 3. Image Release Type
The `image_type` field defines the release stage of the network operating system image, allowing automated ingestion pipelines to categorize builds correctly.

* **Field**: `ImageType image_type = 10;`
* **Supported Values**:
  * `IMAGETYPE_GA`: General Availability release.
  * `IMAGETYPE_EFT`: Early Field Trial release.


---

## Textproto Example

```textproto
vendor_id: OPENCONFIG
nos: "network-os"
software_version: "24.1.R1"
hardware_name: "fixed-sku-01"

# Enhancement: Multiple machine-consumable test results
featureprofile_test_result: {
  plan_id: "interfaces-base-config"
  commit: "a1b2c3d4e5f6g7h8i9j0"
  result: PASSED
}
featureprofile_test_result: {
  plan_id: "bgp-neighbor-state"
  commit: "a1b2c3d4e5f6g7h8i9j0"
  result: FAILED
}

# Enhancement: Structured platform integrity data
pcrs: {
  index: 0
  values: "abc123def456..."
  name: "BIOS"
}
pcrs: {
  index: 7
  values: "789ghi012jkl..."
  name: "Secure Boot Policy"
}

# Enhancement: Categorized image release type
image_type: IMAGETYPE_GA