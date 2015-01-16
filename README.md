#templated_file

##Overview

Allows you use create_resources() with file resources that require the template() function.
The template() function is not in Hiera's library so you are at a loss for doing the following:
```
# $hierdatadir/common.yaml
---
myfiles:
  '/tmp/foo.txt':
    ensure: file
    content: "template('module/template.erb')"

# $modulepath/module/manifests/init.pp
class module {
  create_resources( 'file', hiera('myfiles') }
}

```
or
```
# $hierdatadir/common.yaml
---
myfiles:
  '/tmp/foo.txt':
    ensure: file
    content: "%{template('module/template.erb')}"

# $modulepath/module/manifests/init.pp
class module {
  create_resources( 'file', hiera('myfiles') )
}
```

##Usage

Rather in your hieradata, specify all file attritbutes as you normally would.
Instead of using the content attribute with the template function, use the
template attribute and specify the path to the template as you would normally
in the template function.  For example:
```
# $hierdatadir/common.yaml
---
myfiles:
  '/tmp/foo.txt':
    ensure: file
    template: 'module/template.erb'

# $modulepath/module/manifests/init.pp
class module {
  create_resources( 'templated_file', hiera('myfiles') )
}
```

##NOTES

In your site.pp, you probably have the following as setup by the Puppet install:
```
File { backup => 'main' }
```
To maintain the same behavior using this type, also add to your site.pp:
```
Templated_file { backup => 'main' }
```
