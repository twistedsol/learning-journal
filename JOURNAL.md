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
