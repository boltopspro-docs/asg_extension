<!-- note marker start -->
**NOTE**: This repo contains only the documentation for the private BoltsOps Pro repo code.
Original file: https://github.com/boltopspro/asg_extension/blob/master/CHANGELOG.md
The docs are publish so they are available for interested customers.
For access to the source code, you must be a paying BoltOps Pro subscriber.
If are interested, you can contact us at contact@boltops.com or https://www.boltops.com

<!-- note marker end -->

# Change Log

All notable changes to this project will be documented in this file.
This project *tries* to adhere to [Semantic Versioning](http://semver.org/), even before v1.0.

## [1.0.0]
- rename to asg_extension
- #2 Improvements and rename
- Rename logical id ASG to Asg
- reduce number of parameters so downstream blueprints can have room to add their own parameters
- increase Update Policy timeout from 5M to 8M
- rename `@iam_policies_replace`

## [0.3.0]
- #1 Improvements: AMIs, IAM, VolumeSize, SecurityGroup, zeitwerk autoloader

## [0.2.0]
- Add rolling update policy. Can be disabled with `@policy_options = false`

## [0.1.1]
- no update policy

## [0.1.0]
- Initial release.
