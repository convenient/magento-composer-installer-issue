# The issue

When a file is defined in `magento-deploy-ignore` and the module with that file is updated the file is deleted when it should be left alone.

Use composer v1, this issue will occur with composer v2 but we need to have 2 different versions to bump against.

Do all the below in a clone of this repo

```
git  clone https://github.com/convenient/magento-composer-installer-issue
cd magento-composer-installer-issue
```

# To replicate

```
# Verify pub/index.php exists
cat pub/index.php

# Install magento2-base at 2.3.0
composer install --ignore-platform-reqs

# Update magento2-base to 2.3.5
composer require magento/magento2-base:"2.3.5" --ignore-platform-reqs --update-with-all-dependencies

# Verify pub/index.php exists
cat pub/index.php
```

Result
- Expected = pub/index.php exists
- Actual   = pub/index.php does not exist

# To verify the fix

```
# remove vendor and composer.lock, anything uncomitted
git clean -xffd

# reset composer.json and pub/index.php
git checkout composer.json pub/index.php

# Verify pub/index.php exists
cat pub/index.php

# Pull in the branch containing the codefix
composer require magento/magento-composer-installer:"dev-fix-magento-deploy-ignore-handling as 0.1.13" --no-update --ignore-platform-reqs

# Install magento2-base at 2.3.0
composer install --ignore-platform-reqs

# Update magento2-base to 2.3.5
composer require magento/magento2-base:"2.3.5" --ignore-platform-reqs --update-with-all-dependencies

# Verify pub/index.php exists
cat pub/index.php
```
Result
- Expected = pub/index.php exists
- Actual   = pub/index.php exists
