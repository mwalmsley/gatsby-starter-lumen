---
template: post
title: GCR; Permission Denied - Unable to Determine Upload Size
slug: /posts/gcr_permission_denied_upload_size/
draft: False
date: '2019-09-22T23:46:37.121Z'
description: >-
    Why this error happens and how to fix it
category: cloud
tags:
  - google cloud
  - containers
  - docker
  - bugs
---

Google Cloud has [excellent container support](https://cloud.google.com/compute/docs/containers/deploying-containers), including GCR - a private container registry that automatically stores your containers on GCS. But GCR has a weird bug.

The first time I try to push a new image:

    docker push $IMAGE_URI

The upload runs for a few seconds and then fails with the curious error:

    denied: Unable to determine the upload's size

What's going on? 

GCR works by creating a bucket in GCS for your container. For reasons best known to Google, **project owners do not automatically get write access to GCR buckets**. You need to explicitly give yourself permission.

To do this, follow [this guide](https://cloud.google.com/container-registry/docs/access-control#granting_users_and_other_projects_access_to_a_registry). Click the bucket, then permissions, add member. Enter your Google or Service account. Search for and select the role 'Storage Admin'. 

You should know be able to push to your own GCR. 

Happy coding,

Mike
