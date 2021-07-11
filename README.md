<!-- note marker start -->
**NOTE**: This repo contains only the documentation for the private BoltsOps Pro repo code.
Original file: https://github.com/boltopspro/asg_extension/blob/master/README.md
The docs are publish so they are available for interested customers.
For access to the source code, you must be a paying BoltOps Pro subscriber.
If are interested, you can contact us at contact@boltops.com or https://www.boltops.com

<!-- note marker end -->

# Asg Extension

[![BoltOps Badge](https://img.boltops.com/boltops/badges/boltops-badge.png)](https://www.boltops.com)

This is a [lono extension](https://lono.cloud/docs/extensions/) that provides shared helpers between common ASG blueprints.

## Installation

Add this line to the Gemfile:

    gem "asg_extension"

And then execute:

    bundle

## Usage

To be more explicit about the resources, in your template code add:

app/blueprints/demo/templates/demo.rb:

```ruby
extend_with "asg_extension"

autoscaling_parameters
launch_template_parameters
security_group_parameters

asg_mappings

autoscaling_group
security_group
instance_profile
iam_role
launch_template_resource

autoscaling_group_output
launch_template_output
```

There's more control with the explicit approach and it's clearer what resources are in the template.  Though, you'll need to update the code above if helpers are added to the extension.

For a more implicit approach, in your template code add:

app/blueprints/demo/templates/demo.rb:

```ruby
extend_with "asg_extension"

asg_extension_resources
```

This does the same thing as the example of with the explicit declarations. It is less clear what is going to be created. Though, you won't have to do anything if the extension is updated.

## UserData Success Signal

The AutoScaling Group uses an UpdatePolicy to perform a rolling update. So make sure that you a call to cfn-signal in your UserData script to let CloudFormation the instance was successfully provisioned. Example:

    /opt/aws/bin/cfn-signal -e 0 --stack ${AWS::StackName} --resource Asg --region ${AWS::Region}

If you want to disable both the CreationPolicy and UpdatePolicy options entirely. You can set the variable `@policy_options = false`. You can also set your own custom options:

configs/BLUEPRINT/variables/development.rb:

```ruby
@policy_options = {
  CreationPolicy: {
    ResourceSignal: {
      Timeout: "PT5M", # instance has 5m to finish bootstrapping
      Count: "1"
    }
  },
  UpdatePolicy: {
    AutoScalingRollingUpdate: {
      MaxBatchSize: ref("MaxBatchSize"),
      MinInstancesInService: ref("MinInstancesInService"),
      PauseTime: "PT5M", # will wait for success signal
      WaitOnResourceSignals: "true"
    }
  },
}
```
### Mix Instance Policy Default Strategy

There are several ways to launch an EC2 instance with an AutoScaling group:

1. InstanceId
2. LaunchConfigurationName
3. LaunchTemplate
4. MixedInstancesPolicy

The MixedInstancesPolicy is most flexible option and hence is used by this extension. Parameters are surfaced to allow easily control over the behavior. If you wish to use another strategy like `InstanceId` instead, it can be easily achieved with variables.  Example:

```ruby
@mixed_instances_policy = nil # remove current MixedInstancesPolicy property so we can use another strategy
@instance_id = "ami-0077174c1f13f8f04"
```
