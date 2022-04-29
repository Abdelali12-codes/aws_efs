# aws_efs

## efs utils

/etc/fstab


* install the efs client
```
sudo yum install -y amazon-efs-utils
```

1. file-system-id:/ efs-mount-point efs _netdev,noresvport,tls,iam 0 0

* file-system-id:/ efs-mount-point efs _netdev,noresvport,tls,iam 0 0

2. To automatically mount with IAM authorization to a Linux instance using a credentials file

* file-system-id:/ efs-mount-point efs _netdev,noresvport,tls,iam,awsprofile=namedprofile 0 0

3. To automatically mount a file system using an EFS access point

* file-system-id efs-mount-point efs _netdev,noresvport,tls,accesspoint=access-point-id 0 0


4. mount an efs from another region

```
modify the file efs-utils.conf
and add to it the region where efs locates
#region = us-east-1
```


5. mounting one zone file system
```
sudo mount -t efs -o az=availability-zone-name,tls file-system-id mount-point/
```

6. mount using IAM
```
sudo mount -t efs -o tls,iam file-system-id efs-mount-point/
```


7. Mounting with IAM using a named profile
```
sudo mount -t efs -o tls,iam,awsprofile=namedprofile file-system-id efs-mount-point/
```
8. mounting using access point
```
sudo mount -t efs -o tls,accesspoint=access-point-id file-system-id efs-mount-point
```
9. Turn Off the ID Mapper
```
service rpcidmapd status
 sudo service rpcidmapd stop
 ```









```
{
    "Version": "2012-10-17",
    "Id": "MyFileSystemPolicy",
    "Statement": [
        {
            "Sid": "App1Access",
            "Effect": "Allow",
            "Principal": { "AWS": "arn:aws:iam::111122223333:role/app1" },
            "Action": [
                "elasticfilesystem:ClientMount",
                "elasticfilesystem:ClientWrite"
            ],
            "Condition": {
                "StringEquals": {
                    "elasticfilesystem:AccessPointArn" : "arn:aws:elasticfilesystem:us-east-1:222233334444:access-point/fsap-01234567"
                }
            }
        },
        {
            "Sid": "App2Access",
            "Effect": "Allow",
            "Principal": { "AWS": "arn:aws:iam::111122223333:role/app2" },
            "Action": [
                "elasticfilesystem:ClientMount",
                "elasticfilesystem:ClientWrite"
            ],
            "Condition": {
                "StringEquals": {
                    "elasticfilesystem:AccessPointArn" : "arn:aws:elasticfilesystem:us-east-1:222233334444:access-point/fsap-89abcdef"
                }
            }
        }
    ]
}

```

```
curl http://169.254.169.254/latest/meta-data/iam/info
```


10. to mount a volume using iam 

* add this to user mount command -o iam
