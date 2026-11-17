# Journal

Newest entries at the bottom.

---

## Days 2 to 4 (Oct 16 to 18)

**What I did**
- Reviewed my site and fixed problems in it.
- Built a Python log detector that looks for suspicious events in AWS-style logs (root logins, logins without MFA, brute force, public buckets, logging turned off).
- Put the code on GitHub with a README, a licence and automatic tests (GitHub Actions).
- Published the site with GitHub Pages and linked the first lab card.

**What I learned**
- A repo is more than code: a README, a licence and passing tests make it look finished.
- Tests catch my own mistakes too. One test had the wrong expected number, and the detector was right.
- GitHub's website is enough to edit, rename and delete files when I'm starting out.

---

## Day 5 (Oct 19): Misconfiguration scan

**What I did**
- Installed Checkov, a tool that scans infrastructure code for security mistakes.
- Scanned TerraGoat, a Terraform project that is vulnerable on purpose. It is for practice and must never be deployed.
- Made a copy of `s3.tf` and fixed three problems in the `data` bucket: public access, missing encryption, missing versioning.

**Result**
- Before: 25 passed, 31 failed.
- After: 32 passed, 28 failed.

**What I learned**
- Edit a copy and leave the original alone, so I always have something to compare against.
- Check that a file really has content. An empty file made the scan print nothing, and `dir` showed its length was 0.
- One typo (`true1` instead of `true`) quietly switched off a fix. Re-scanning is how I caught it.
- Check which folder VS Code has open. Terminal commands run from that folder.
- Python 3.15 was too new for Checkov's helper packages, so I used `py -3.13`.
- Saving scan output to a file (`> after.txt`) makes it easy to pull out just the summary line.

**Next**
- Write up the lab in its own repo and update the Labs card on my site.

----

## Nov 8 to 16: Cloud audit alerts (AWS)

**What I did**
- Built an alert chain in AWS: CloudTrail records activity, an EventBridge rule spots risky S3 changes, and SNS emails me.
- The rule watches for a bucket being created, a bucket policy being changed, or public access protection being removed.
- Tested it by creating a bucket. The alert email arrived in about a minute and showed what happened, who did it, when, and from which IP address.
- Deleted the test buckets afterwards.

**Result**
- I could not find the first alert email, so I checked each link instead of guessing. The rule's Monitoring tab showed 1 matched event, 1 invocation and no failed invocations, and a manual test message from SNS reached my inbox. A second test bucket then produced the alert.

**What I learned**
- A detection pipeline is a chain. When it is silent, test each link in turn.
- The Monitoring graphs show the last week by default, so an older event looks like "No data" until the date range is widened.
- The newer EventBridge builder is drag and drop. The Advanced builder shows every setting, which is easier for a first rule.
- The alert email contains the account ID and IP address, so I redact them before sharing anything.
- My free-plan account does not support some services, such as Security Hub, so I check what is allowed before planning around it.

**Next**
- Scan and harden Terraform code for the same kind of resource.

---

## Nov 17: Terraform S3 scan

**What I did**
- Wrote a deliberately careless S3 bucket in Terraform, with a public-read ACL. It was never deployed to AWS.
- Scanned it with Checkov, fixed it in a copy, and scanned again.
- Fixed the public ACL, added a public access block and versioning, and encrypted the bucket with a KMS key that has a key policy.
- Published it as the repo terraform-s3-lab and added it to the Labs page.

**Result**
- Before: 6 passed, 8 failed.
- After: 16 passed, 4 failed.
- Left on purpose: access logging, event notifications, a lifecycle rule, and cross-region replication.

**What I learned**
- Fixing one problem can create a new finding. The KMS key fixed encryption, then failed its own check until it had a policy.
- A scan counts rules, not safety. A high pass count does not mean a bucket is secure.
- Check which file I am editing before I paste. I pasted the fix into the original once, and the unchanged scan numbers showed it.
- Check the file size after a change. An unexpectedly small file means something was overwritten.
- GitHub adds "(1)" to an uploaded file name when the same name already exists on my computer, so I rename it with the edit pencil.

**Next**
- Install Terraform and run validate and plan on the fixed file.
- Run the Checkov scan in GitHub Actions, like my other labs. 
