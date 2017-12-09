# aws.md - Yes I can!

I setup a new virtualenv to work with the awscli:

```
$ mkvirtualenv aws
$ pip install awscli
(aws) $ aws --version
aws-cli/1.14.4 Python/2.7.13 Linux/4.9.0-3-amd64 botocore/1.8.8
```

Superb. I have a working aws CLI. Now I can list AMIs I am interested
in:

```
$ aws ec2 describe-images --owner self amazon --filters "Name=root-device-type,Values=ebs"
```

I don't know if the filters syntax in the last command is correct. I
get help on this topic with:

The list of AMIs is a little bit lengthy. I wonder why there are so
many windows AMIs. I want to filter them out. But how? I read the help
with:

```
$ aws ec2 describe-images help
```

but can't find any clue on how to filter for linux platforms. Ok, I
switch to `jq`. Which platforms are there?

```
$ aws ec2 describe-images --owner self amazon --filters "Name=root-device-type,Values=ebs" | jq '.[][] | .Platform' | sort | uniq -c
   1363 null
   1746 "windows"
```

So there are a lot of "windows" platforms and quite the same lot of
unknown platforms. I don't like that. So how to find the "right" Linux
AMI then? There is this page:
<http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/finding-an-ami.html>. But
that page does not answer my question well. I start to think that I
need to build my own AMI and might be I should have done this in the
first place. So how to do it? Quickly I found this page:
<http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/creating-an-ami-ebs.html>. The
option to import existing VMs jumps into my eyes:
<http://docs.aws.amazon.com/vm-import/latest/userguide/what-is-vmimport.html>. The
VM image import from the AWS CLI is able to import the following
formats: OVA, VHD, VHDX, VMDK, raw. Sounds good, so I can simply setup
a virtual machine in VirtualBox and then export this to AWS. First I
get a new Debian image file from <https://www.debian.org> (Debian
9.2.1). Then I create a new virtual maching with specs: 1024 MB RAM, 8
GB VMDK disk image.

I now need a detailed explanation on how to proceed with my shiny new
Debian disk image. I find some more detailed pointers here:
<https://aws.amazon.com/ec2/vm-import/>. First I need to upload the
disk image to S3. First I read `aws s3 help`. I read a lot about
filtering but is not what I need right now. I need to simply upload a
file to S3. Here I found a resource describing what needs to be done:
<http://docs.aws.amazon.com/AmazonS3/latest/gsg/PuttingAnObjectInABucket.html>.

I need to creat a bucket first, ok, after reading `aws s3 md help` I
know how to create a new bucket, I think `mb` stands for something
like "make bucket":

```
$ aws s3 mb s3://xmcpam
make_bucket: xmcpam
```

I think I am able to add an object to my new bucket. I try:

```
$ aws s3 cp /media/sf_VirtualBoxShare/DebianAMI.vmdk s3://xmcpam/debian.vmdk
upload: ../../media/sf_VirtualBoxShare/DebianAMI.vmdk to s3://xmcpam/debian.vmdk
```

This worked as espected, perfect. And the `ls` command also works as expected:

```
$ aws s3 ls
2017-12-07 11:18:07 xmcpam
$ aws s3 ls s3://xmcpam
2017-12-07 11:19:39 1304363008 debian.vmdk
```

Now to import the uploaded disk file as AMI there's the `aws ec2
import-image` command. Again I'll first go through its help page `aws
ec2 import-image help`. With help of the man page I build the
following command together:

```
$ aws ec2 import-image --disk-containers="Description=Debian AMI,UserBucket={S3Bucket=xmcpam,S3Key=debian.vmdk}"

An error occurred (InvalidParameter) when calling the ImportImage operation: The service role <vmimport> does not exist or does not have sufficient permissions for the service to continue
```

The command also has a `--role-name` option, but I decide to create a
new role `vmimport` just to learn how to create a new role. In `aws
iam help` I find a `create-role` command. So I read `aws iam
create-role help`. The command requires two options: `--role-name` and
`--assume-role-policy-document`. I can guess what to specify for the
former option, this should be `vmimport`. But I have no clue what to
specify for the latter option. I have to reade more about creating IAM
roles somewhere else.

I found this page:
<http://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-user.html>
and I read through it.


