---
title: gitlab-ci报500问题修复
tags:
  - gitlab
  - ci/cd
  - 500
categories:
  - 技术分享
date: 2020-01-07 13:59:26
---
# gitlab-ci报500问题修复

Enter the DB console:
For Omnibus GitLab packages:
```bash
sudo gitlab-rails dbconsole
```
### Reset CI/CD variables
Drop the table:
```bash
DELETE FROM ci_group_variables;
DELETE FROM ci_variables;
```
<!-- more -->

### Reset Runner registration tokens

```bash
-- Clear project tokens
UPDATE projects SET runners_token = null, runners_token_encrypted = null;
-- Clear group tokens
UPDATE namespaces SET runners_token = null, runners_token_encrypted = null;
-- Clear instance tokens
UPDATE application_settings SET runners_registration_token_encrypted = null;
-- Clear runner tokens
UPDATE ci_runners SET token = null, token_encrypted = null;
```
### Reset pending pipeline jobs
```bash
-- Clear build tokens
UPDATE ci_builds SET token = null, token_encrypted = null;
```
### Update configuration
```bash
gitlab-ctl reconfigure
```

