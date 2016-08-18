**Vagrant FAQs**  

- [How to cleanup orphaned vagrant boxes](#vagrant-cleanup)

## <a name="vagrant-cleanup" ></a> How to cleanup orphaned vagrant boxes?

I sometimes forgot to shutdown a Vagrant box and then removed entire folder that contained the Vagrant project, gone with .vagrant folder
that contains the box metadata.

When I run `vagrant global-status`, it shows up in running state, but I won't be able to use `vagrant destroy`, `vagrant gloabl-status --prune`
to get rid of it.

The follow clean up process worked in this my case. First find it in VirtualBox manager, save its state, unregister it, then you should
be able to prune it. These are the steps:

* Find the box you want to cleanup:

```
$ vagrant global-status
```
Note the box name and file path that you want to clean up, which is running but the directory for the box is gone. The following VBoxManage command
needs box uuid, but since you don't have the box uuid, you want to use the box name and file path to find the best match.

* Run VBoxManage 

```
$ VBoxManage list runningvms 
"my-vbox_test_1460569039540_53997" {a17bceb8-fe85-413b-b785-e8025e06b4bf}
"foo_default_1460673696387_19372" {04ec71fd-3e27-4798-b012-a9c55b257c5b}
```

The listed VMs has a name as prefix. Find the *uuid* that you want to destroy. Say you want to destory "foo_default_1460673696387_19372", 
it's uuid is "04ec71fd-3e27-4798-b012-a9c55b257c5b"

```
$ VBoxManage savevms 04ec71fd-3e27-4798-b012-a9c55b257c5b
$ VBoxManage savevms 04ec71fd-3e27-4798-b012-a9c55b257c5b
$ VBoxManage discardstate 04ec71fd-3e27-4798-b012-a9c55b257c5b
$ VBoxManage unregistervm 04ec71fd-3e27-4798-b012-a9c55b257c5b
```

* Cleanup
```
$ vagrant global-status --prune
```  
