---
layout: post
title: User Data
order: 100
---

# Modify Global User-Data For All Deployments

When new server-groups are deployed Spinnaker attaches a global [user-data](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-instance-metadata.html)  script/file that is prepended to any application specific user-data configured in a server Spinnaker pipeline. By default Armory Spinnaker comes with a user-data file which is placed in `/opt/spinnaker/config/udf/udf0`.  This can be modified and overwritten to your specific needs.

To modify the location of the `udf0` template file update your `clouddriver-local.yml` file and add the following:

```
udf:
  udfRoot: /opt/spinnaker/config/udf
```

### Related Guides

- [Dynamically defining User-Data]({% link _user_guides/expression-language.md %}#dynamically-defining-user-data)
