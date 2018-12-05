# Chef - Basics

## Recipe: basic

A **recipe** is source code that holds 1+ resources.

> Recipe: ordered series of configuration states

A **resource** describes a part of the system: **what**
* `package`
* `service`
* `file`

Resources have deafult (see docs) and other actions
* `:create`
* `:delete`

Once deployed, a chef checks for the state of the resource and only runs in case of change.
This is called **test and repair approach**

Example
---
```
package 'httpd' # default action install
service 'httpd' do
    action [:enable, :start]
end

file '/var/www/html/index.html' do # default action create
    content '<html>
    <body>
        <h1>Hello Chef</h1>
    </body>
    <html/>'
end
```

## Cookbook: basic
A cookbook is a recipes project. A default cookbook can be created by running
```
$ chef generate cookbook learn_chef_httpd
```
A cookbook contains the logical structure to keep:
1. Recipes
2. Templates
3. Unit Tests
4. Integration Tests
5. Chef Automate components

To create a template file to be applied as a resource run:
```
`$ chef generate template <path_to_cookbook> <template_name>`
```
Templates are added the extension `.erb`to indicate they may contain placeholders. See tree reference below.

```
[centos@ip-172-31-82-115 cookbooks]$ tree -a learn_chef_httpd/
learn_chef_httpd/
├── Berksfile
├── chefignore
├── .delivery
│   ├── build_cookbook
│   │   ├── Berksfile
│   │   ├── chefignore
│   │   ├── data_bags
│   │   │   └── keys
│   │   │       └── delivery_builder_keys.json
│   │   ├── .kitchen.yml
│   │   ├── LICENSE
│   │   ├── metadata.rb
│   │   ├── README.md
│   │   ├── recipes
│   │   │   ├── default.rb
│   │   │   ├── deploy.rb
│   │   │   ├── functional.rb
│   │   │   ├── lint.rb
│   │   │   ├── provision.rb
│   │   │   ├── publish.rb
│   │   │   ├── quality.rb
│   │   │   ├── security.rb
│   │   │   ├── smoke.rb
│   │   │   ├── syntax.rb
│   │   │   └── unit.rb
│   │   ├── secrets
│   │   │   └── fakey-mcfakerton
│   │   └── test
│   │       └── fixtures
│   │           └── cookbooks
│   │               └── test
│   │                   ├── metadata.rb
│   │                   └── recipes
│   │                       └── default.rb
│   ├── config.json
│   └── project.toml
├── .gitignore
├── .kitchen.yml
├── LICENSE
├── metadata.rb
├── README.md
├── recipes
│   └── default.rb
├── spec
│   ├── spec_helper.rb
│   └── unit
│       └── recipes
│           └── default_spec.rb
├── templates
│   └── index.html.erb
└── test
    └── integration
        └── default
            └── default_test.rb
```


Recipe: default.rb
```
package 'httpd' # Default action install

service 'httpd' do
  action [:enable, :start]
end

template '/var/www/html/index.html' do # Default action create
  source 'index.html.erb'
end
```

Now, in order to run the cookbook, we used the `--runlist` option, which specifies the recipes in order of execution.
We should run the `chef-client`from the root of the cookbook.
```
sudo chef-client --local-mode --runlist 'recipe[learn_chef_httpd]'
```
