# Firebase Test Lab Orb

Easily run tests with [Firebase Test Lab](https://firebase.google.com/docs/test-lab/ "Firebase Test Lab") on your [CircleCI](https://circleci.com/ "CircleCI") projects. This orb support only Android apps.

Learn more about [Orbs](https://github.com/CircleCI-Public/config-preview-sdk/blob/master/docs/using-orbs.md "orb").

## What are Orbs?

Orbs are functions that can be configured as packages, and can be published in a reusable format. Added with CircleCI 2.1 configuration (preview as of 2018/12/7). For more details on Orb, please refer to the official document.

Added with CircleCI 2.1 configuration (preview as of 2018/12/7). Please refer to [official document] (https://circleci.com/docs/2.0/orb-intro/#section=configuration) for the details of Orb.

By publishing the settings for Firebase Test Lab together as Orb, you should be able to build a workflow including tests by Firebase Test Lab more quickly.

# Published Orb

The page of Orb registry is here.
https://circleci.com/orbs/registry/orb/horie1024/firebase-test-lab

Also, the source code is here.
https://github.com/horie1024/orb_firebaae_test_lab

# How to use

## Enable build processing enabled

CircleCI 2.1 configuration is a preview as of 2018/12/7, so set Enable build processing to `On` in Project Settings-> Advanced Settings.

![image.png](https://qiita-image-store.s3.amazonaws.com/0/43438/f0011bc5-d0e3-2b92-a54b-21e456aafcef.png)

## Import of Orb

Add the following configuration to `.circleci / config.yml` and import Orb. Specify `2.1` for version.

`` Yaml
version: 2.1

orbs:
  firebase: horie1024/firebase-test-lab@volatile
```

## Run command

Start testing with Firebase Test Lab with the `firebase / test` command.

`` Yaml
firebase/test:
    service_account_key: SERVICE_ACCOUNT_KEY
    firebase_project_id: FIREBASE_PROJECT_ID
    type: instrumentation
    test_settings: test_settings.yml:instrumentation-test
```

For `service_account_key`, specify an environment variable that stores the base64 encoded value of GCP Project service account key [^ 1]. Also, specify the Firebase project ID for `firebase_project_id`, and specify` instrumentation` or `robo` for` type`.

[^ 1]: [This article] (https://qiita.com/Horie1024/items/3c22b67634deb0dd4645#%E3%82%B5%E3%83%BC% for creating service account and storing in environment variable E 3% 83% 93% E 3% 82% B 9% E 3% 82% A 2% E 3% 82% AB% E 3% 82% A 6% E 3% B 3% E 3% 83% 88% E 3% 81% AE 4% BD% 9C% E6% 88% 90% E3% 81% E8% 82% AD% E3% 83% BC% E3% 81% AE% E3% 83% 80% E3% 82% A6% E3% 83% B3% E3% 83% AD% E3% 83% BC% E3% 83% 89).

For `test_settings`, you can specify the device to execute the test by specifying yml as follows.

`` Yaml
instrumentation-test:
  device:
    - model: Nexus5
      version: 21
      local: ja_JP
      orientation: portrait
    - model: eyes
      version: 25
      local: ja_JP
      orientation: portrait
```

For details on other parameters, please refer to the document of Orb.

https://circleci.com/orbs/registry/orb/horie1024/firebase-test-lab#commands-test

The final config.yml is:

`` Yaml
version: 2.1

orbs:
  firebase: horie1024/firebase-test-lab@volatile

jobs:
  build:
    docker:
      - image: circleci/android:api-28-alpha
    steps:
      - checkout
      - run:
          name: Download Dependencies
          command: ./gradlew androidDependencies
      - run:
          name: Build debug APK and release APK
          command: |
              ./gradlew :app:assembleDebug
              ./gradlew :app:assembleDebugAndroidTest
      - firebase/test:
          service_account_key: SERVICE_ACCOUNT_KEY
          firebase_project_id: FIREBASE_PROJECT_ID
          type: instrumentation
          test_settings: test_settings.yml:instrumentation-test
```

# Usage example in Android project

I tried to execute a test with Firebase Test Lab using the actually published Orb. The sample project is [here] (https://github.com/horie1024/orb_firebaae_test_lab_android_sample).

## Checking test results

Test results can be viewed from the Artifacts tab, and you can easily see how the screen is running.

![image.png](https://qiita-image-store.s3.amazonaws.com/0/43438/a67ab3e9-c31d-28ac-4b16-21c4c21870ea.png)

# Summary

By publishing the settings as Orb, CircleCI will be able to execute tests in Firebase Test Lab more easily.

If there is a missing function or bug in this published Orb, please give me a Pull Request.

https://github.com/horie1024/orb_firebaae_test_lab
