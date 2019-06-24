# Host a static website with aws

This guide shows how you can simply host a website using an AWS s3 bucket and leveraging caching with CloudFront.

Before you get started you need to have a valid aws account. After creating the account [login into the console.](https://console.aws.amazon.com/console/home)

___

## The S3 bucket
`aws S3`, known as Simple Storage Service is an object storage used to store and retrieve data through the internet. Of course it is durable and scalable.

More information about the service can be found [here.](https://aws.amazon.com/s3/faqs/)

### Let's get started with s3
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

![alt create bucket](/images/s3/create-bucket.png)

### Upload files to the bucket
Now that the bucket has been created, we can actually upload our website content in it.
Click on the bucket name to open the bucket console.

![alt click on bucket name](/images/s3/click-on-bucket.png)

Once in the bucket console, click on the `Upload` button to upload your website files/folders.
![alt bucket console](/images/s3/bucket-console.png)

In the opened `Upload` popup drag and drop your content in it or click on the `Add files` button to choose the files from your machine.

![alt upload page](/images/s3/upload-page.png)

At this point the files have not been uploaded yet to the console. But you can see the uploaded content listed on the popup. Click the `Upload` button to transfer the files to the bucket.

![alt files ready](/images/s3/files-ready.png)

In the bucket console page you can now see your files listed under the `Overview` tab.

![alt files uploaded](/images/s3/overview-tab.png)

### Change Bucket Policy

At this point the files are not yet publicly available. We need to change the bucket policy by clicking on the `Permissions` tab and then the `Bucket Policy` button.

![alt permissions tab](/images/s3/bucket-policy-tab.png)

In the policy page we need to specify what accesses we want to give to our files. We can grant `get` access to `everyone` to our bucket `resources`'s. This will make our static website resources publicly available.

In the editor enter the below json and change the bucket name with your bucket's name. Basically, here we're giving access to all the resources in the bucket to everyone. You can see the `*` after the bucket name.

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "AddPerm",
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::your-bucket-name/*"
        }
    ]
}
```
![alt change policy json](/images/s3/change-policy-json.png)

After saving the policy, you'll see that a yellow `Public` tag will be added under `Permissions` and `Bucket Policy`.

If you go back in the `S3 buckets` console by clicking the `Amazon S3` link on the top left, you'll see that the bucket `Access` is changed to `Public`.

![alt bucket public](/images/s3/access-public.png)

You can test that the resources are publicly available by entering the bucket (click on the bucket name), then in the `Overview` tab, click on one of the files (for instance on `index.html`), then in the new page, under the `Overview` tab, you'll find the URL under `Objcet URL`. Click on the link and you should see your file.
