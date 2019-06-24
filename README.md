# Host a static website with aws

This guide shows how you can simply host a website using an AWS s3 bucket and leveraging caching with CloudFront.

Before you get started you need to have a valid aws account. After creating the account [login into the console.](https://console.aws.amazon.com/console/home)

___

### The S3 bucket
`aws S3`, known as Simple Storage Service is an object storage used to store and retrieve data through the internet. Of course it is durable and scalable.

More information about the service can be found [here.](https://aws.amazon.com/s3/faqs/)

#### Let's get started with s3
Once logged into the console, click on `Services` on the top navigation bar, type `S3` and click on the service to open the dedicated console.

![alt find service](/images/s3/find-s3-service.png)

Once in the S3 console page, click on the `+ Create bucket` button.
![alt s3 console page](/images/s3/s3-home.jpg)

This will open the bucket configuration page.
![alt new bucket](/images/s3/new-bucket.png)

Enter a name for the bucket, select your region and click on `Next`. You can skip over the `Configure options` page, unless you want specific configurations such as versioning, log, tags, etc.

Click again `Next` to access the `Set permission` page. In this page we need to uncheck the `Block all public access` checkbox. This is important, otherwise our website content won't be publicly accessible.

![alt bucket configuration](/images/s3/bucket-permissions.png)

Click `Next` again to access the 'Review' tab. If everything looks fine here, click on the `Create bucket` button to actually create the bucket.

![alt bucket configuration](/images/s3/create-bucket.png)
