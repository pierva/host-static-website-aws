# Host a static website with aws
## Cloud Developer Nanodegree - Udacity -

This guide shows how you can simply host a website using an AWS s3 bucket and leveraging caching with CloudFront.

Before you get started you need to have a valid aws account. After creating the account [login into the console.](https://console.aws.amazon.com/console/home)


## Table of Contents

+ [The S3 bucket](#the-s3-bucket)
  * [Get started with S3](#lets-get-started-with-s3)
  * [Upload files to the bucket](#upload-files-to-the-bucket)
  * [Change bucket policy](#change-bucket-policy)
  * [Change bucket properties](#change-bucket-properties)
+ [CDN with CloudFront](#cdn-with-cloudfront)
  * [Create the distribution](#create-the-distribution)
+ [Expire cached content](#expire-cached-content)
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
Now that the bucket has been created, we can upload our website content in it.
Click on the bucket name to open the bucket console.

![alt click on bucket name](/images/s3/click-on-bucket.png)

Once in the bucket console, click on the `Upload` button to upload your website files/folders.
![alt bucket console](/images/s3/bucket-console.png)

In the opened `Upload` popup drag and drop your content in it or click on the `Add files` button to choose the files from your machine.

![alt upload page](/images/s3/upload-page.png)

At this point the files have not been transferred yet to the bucket. You can see the uploaded content listed on the popup. Click the `Upload` button to transfer the files to the bucket.

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


### Change Bucket Properties
With the new policy, previously set, we can change the bucket's `Properties` in order to serve the Static Website.

Open the bucket's console by clicking on the bucket's name, click on the `Properties` tab and then click on `Static website hosting`. After clicking on the block, you'll be prompted to enter some configurations.

In this section you need to provide the `Index document` name (normally index.html) and the `Error document`, if any. If you don't have a dedicated error page, enter the same entry point entered in `Index document`.

Click on `Save`.

![alt static website hosting](/images/s3/hosting-configuration.png)



Now you'll see that the `Static website hosting` section, is enabled.

![alt static website hosting enabled](/images/s3/bucket-hosting.png)

At this point we're done with the bucket and we can proceed to CloudFlare to leverage the caching and distribution of our website.

## CDN with CloudFront
CloudFront is a fast CDN (content delivery network) service offered by Amazon aws.

CloudFront transfer content globally with low latency thanks to the wide aws infrastructure. All the points of the network (globally distributed) are called `Edge Locations`.

More information about CloudFront can be found [here](https://aws.amazon.com/cloudfront/?nc2=type_a).

### Create the Distribution
Open the CloudFront console by clicking on `Services` on the top navigation bar and typing `CloudFront`.

![alt CloudFront service](/images/cloudFront/cloud-front-service.png)

From the console click on the `Create Distribution` button.

![alt CloudFront console](/images/cloudFront/cloudFront-home.png)

This button will open the Distribution configurations page. For our purposes we need a `Web` distribution, therefore click on `Get Started` under the Web section.

![alt new distribution](/images/cloudFront/new-distribution-home.png)

In the new page we need to configure our Distribution. In particular we need to tell CloudFront where to take the resources and what's the origin path.

Click on `Origin Domain Name` and you'll see the available origins. Select the `Amazon S3 Bucket` previously created.

![alt select bucket](/images/cloudFront/select-bucket.png)

Click on `Origin Path` and add the origin to your resources in the S3 bucket. If your index.html file is located in the root of the bucket, just type `/` in the Origin Path.

![alt origin path](/images/cloudFront/origin-path.png)


You can change the `Default Cache Behavior Settings` to accommodate your needs. For instance is good to redirect `http` requests to `https`.

Scroll down and under `Distribution Settings` select where you want your content to be distributed.

Click on 'Create Distribution' to create the CloudFront distribution. This operation takes some time to complete, normally around 10 minutes.

![alt distribution settings](/images/cloudFront/distribution-settings.png)

At the end of the deployment, in the CloudFront console you'll see that the `Status` of the distribution changes to `Deployed`.
You can now access your website over the internet by accessing the `Domain Name` from the browser.

![alt deployed distribution](/images/cloudFront/deployed-distribution.png)


## Expire cached content
The default caching time, before the edge locations make a request to the source to check whether the content has been updated is 24 hrs (Default TTL 86400s).

If you update the content in the S3 bucket keeping the same file name and without versioning, you won't see the new content until the TTL time is elapsed, which in the worst case, as mentioned above is 24hrs.

To expire the content in the edge locations you can create an invalidation.

From the CloudFront console, check your distribution and click on `Distribution Settings`. Alternatively you can click on the distribution ID.


![alt cloud front console](/images/invalidations/cloud-front-console.png)

In the `Distribution Settings` page, click on the `Invalidations` tab and then on the `Create Invalidation` button.

![alt create invalidation tab](/images/invalidations/create-invalidation.png)

In the `Create Invalidation` modal, specify the path to the files you want to expire. Click on `Invalidate`.

The status of the invalidation will be `In progress`. Once the invalidation is completed, and all the edge locations updated, you'll see the status changing to `Completed`.

![alt invalidation status](/images/invalidations/invalidation-in-progress.png)
