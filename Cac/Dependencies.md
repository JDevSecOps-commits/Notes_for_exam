InSpec can bring in controls from another profile  
Can skip or modify them 
Include in profile's inspec.yml file 
Example: 
```
cat >> ubuntu/inspec.yml <<EOL
name: profile-dependency
title: Profile with Dependencies
maintainer: InSpec Authors
copyright: InSpec Authors
copyright_email: support@chef.io
license: Apache-2.0
summary: InSpec Profile that is only consuming dependencies
version: 0.2.0
depends:
  - name: SSH baseline
    url: https://github.com/dev-sec/ssh-baseline
  - name: Linux Baseline
    supermarket: dev-sec/linux-baseline
EOL
```
Supermarket specifies where the profile is hosted. (On a cookbook in the chef supermarket) 
Dependencies will be cached upon first read and placed in inspec.lock file
`inspec vendor` (Run inside the profile) 

Example Inspec.lock
```
---
lockfile_version: 1
depends:
- name: SSH baseline
  resolved_source:
    url: https://github.com/dev-sec/ssh-baseline/archive/master.tar.gz
    sha256: 39984f997f33c25d58427b5e45e73d27bc98850c7c12285bfd15104507e27298
  version_constraints: []
- name: Linux Baseline
  resolved_source:
    url: https://github.com/dev-sec/linux-baseline/archive/master.tar.gz
    sha256: 54ae7290a0e2acb448fa16e1ddc94474a0f9f55c540c4b7eb08fda85018b9a49
  version_constraints: []
```

