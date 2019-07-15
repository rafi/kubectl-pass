# kubectl-pass

> Plugin for `kubectl` that supports various integrations with "[pass]", the
> standard Unix password manager.

## Pre-requisites

* [kubectl]
* [pass]
* [jq]

## Subcommands

* [auth](#subcommand-auth)
* More soon...

### Subcommand: Auth

Allows you to store client data secrets in pass encrypted files. For example,
if you are using PEM, create a new encrypted pass secret:

```bash
pass edit personal/k8s
```

Fill in these fields: (Make sure they are already base64 encoded)

```yaml
client-certificate-data: LS0...
client-key-data: LS0...
```

Finally, edit your `~/.kube/config` user:

```yaml
users:
- name: personal-admin
  user:
    exec:
      apiVersion: client.authentication.k8s.io/v1beta1
      args:
      - auth
      - pem
      - personal/k8s
      command: kubectl-pass
```

Now try using `kubectl` with the context you've edited. Enjoy!

[kubectl]: https://kubernetes.io/docs/tasks/tools/install-kubectl/
[pass]: https://www.passwordstore.org/
[jq]: https://github.com/stedolan/jq
