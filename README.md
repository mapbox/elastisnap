Take rotating Amazon AWS EBS snapshots with this small node.js script.

* Define what you want to snapshot based on instance-id + devices on that instance.
* Put the script on an EC2 and use 'self' as one of the "jobs" keys to refer to that instance itself.
* Handles multiple devices at a time on various instances.
* If the EC2 uses iam-roles, you may omit awskey and awssecret; they'll be looked up dynamically using the EC2 metadata API

See setting.json for sample definitions. Options include:

pool - the number of snapshots to maintain before destroying old ones.
device - name of device that should be snapshotted.
description - used as the description for the snapshot along with the device name and the instance ID.

`node index.js --config=settings.json` ... throw that in a hourly, daily, etc. cronjob and set the 'pool' size and you've got rotating snapshots.

## Security considerations

This tool allows someone with root access to the instance being backed up up to also delete the snapshot backups of the same host. If someone were to maliciously comprimise a server using this tool, they could delete the backups of the server at the same time.

If that's a concern for you, consider a different tool which does not allow an instance to delete it's own snapshots and instead deletes the snapshots through another means, such as an AWS Lambda Function which runs independently of any host. 
