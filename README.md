# docs-linksharing

This guide is for cases where files have already been uploaded, be it via uplink CLI or programatically via [MT](https://github.com/storjrd/mt-usage) and they need to be accessed from a consistent URL.

Typically, a presigned S3 URL is the correct choice. It is fine-grained to work for just a specific url so you don't make public any more than you need to.

However, presigned S3 URLs need to be generated for every single file and expiry neccessarily.

This guide shows how to use our linksharing service to make public an entire directory or bucket in advance so a browser or client can access any file it knows the path to.

## 1. Prerequisites

* [Uplink CLI](https://docs.storj.io/getting-started/quickstart-uplink-cli/uploading-your-first-object/set-up-uplink-cli)

* An Access Grant (not an *access key*)

## 2. Generate a restricted access

We need to create a new access grant for use by our browser or client. Not only do we limit it to a specific bucket and/or directory, we also remove the permission to list files. This means an attacker cannot easily download all of our public files at once without knowing the filenames.

```
uplink --access my-access-here share sj://my-bucket/my-directory --disallow-write --disallow-lists --register --publi
```

The output should resemble the following.

```
=========== ACCESS RESTRICTIONS ==========================================================
Download  : Allowed
Upload    : Disallowed
Lists     : Disallowed
Deletes   : Disallowed
NotBefore : No restriction
NotAfter  : No restriction
Paths     : sj://my-bucket/my-file
=========== SERIALIZED ACCESS WITH THE ABOVE RESTRICTIONS TO SHARE WITH OTHERS ===========
Access    : xxx
========== CREDENTIALS ===================================================================
Access Key ID: xxx
Secret Key   : xxx
Endpoint     : xxx
Public Access:  true
```

Copy the new `Access Key Id`, `Secret Key`, and `Endpoint`, we'll need it in the next step.
