---
id: 675
title: How to delete all entries from Java JKS Keystore
date: 2017-12-24T10:48:52+00:00
author: Manthan Dave
layout: post
guid: https://www.manthanhd.com/?p=675
permalink: /2017/12/24/how-to-delete-all-entries-from-java-jks-keystore/
categories:
  - Educational
  - Findings
tags:
  - bash
  - java
  - keytool
  - shell
---
I had to deal with this recently. After much trial and error, here's the command that you can use to wipe your Java JKS Keystore of all its entries:
<pre class="lang:sh decode:true crayon-selected" title="Command to delete all keystore entries (with plaintext password)">keytool -list -keystore ${KEYSTORE} -storepass ${KEYSTORE_PASS} -rfc | grep -Po "Alias name: \K([A-Za-z0-9\s_-]+)" | xargs -n 1 -I {} keytool -delete -alias {} -keystore ${KEYSTORE} -storepass ${KEYSTORE_PASS}</pre>
Here, the variable <code>KEYSTORE</code>is the path to your Java keystore and the variable <code>KEYSTORE_PASS</code> is the keystore's password. If you are not comfortable in using the keystore password plain text in command line, I'd suggest you use an alternative version using a file containing keystore password or name of an environment variable instead. This will hide the password from appearing in shell history. You can do this by suffixing the <code>-storepass</code> argument with <code>:file</code> or <code>:env</code> resulting in it effectively becoming <code>-storepass:file &lt;path/to/file&gt;</code> or <code>-storepass:env &lt;ENV_NAME_WITHOUT_$</code>. Here are some examples:
<pre class="lang:sh decode:true" title="Command to delete all keystore entries (with password in file)">keytool -list -keystore ${KEYSTORE} -storepass:file ${KEYSTORE_PASS_FILE} -rfc | grep -Po "Alias name: \K([A-Za-z0-9\s_-]+)" | xargs -n 1 -I {} keytool -delete -alias {} -keystore ${KEYSTORE} -storepass:file ${KEYSTORE_PASS_FILE}</pre>
In the above, notice how the `${KEYSTORE_PASS}` environment variable has changed to `${KEYSTORE_PASS_FILE}`. Use this to provide a path to the file containing your keystore password.
<pre class="lang:sh decode:true" title="Command to delete all keystore entries (with password in env)">keytool -list -keystore ${KEYSTORE} -storepass:env ${KEYSTORE_PASS_ENV} -rfc | grep -Po "Alias name: \K([A-Za-z0-9\s_-]+)" | xargs -n 1 -I {} keytool -delete -alias {} -keystore ${KEYSTORE} -storepass:env ${KEYSTORE_PASS_ENV}</pre>
Similar to previous, this one has been slightly modified to use the <code>-storepass:env</code> flag with `${KEYSTORE_PASS_ENV}` environment variable instead.