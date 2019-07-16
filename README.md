# kubectl-pass

> Authentication and secret management with [`pass`] and [`kubectl`]
> integration. Works as a standalone script, or kubectl plugin.
>
> `pass`, the standard Unix password manager, is a powerful way to store
> your valuable secrets with GPG keys.
> Read about it more at [passwordstore.org].

## Table of Contents

<!-- vim-markdown-toc GFM -->

* [Pre-requisites](#pre-requisites)
* [Install](#install)
* [Commands](#commands)
  * [Auth](#auth)
* [See Also](#see-also)
* [License](#license)

<!-- vim-markdown-toc -->

## Pre-requisites

* [kubectl]
* [pass]
* [jq]

## Install

Copy `kubectl-pass` somewhere in your `$PATH`:

```bash
cd ~/.local/bin
curl -LO https://github.com/rafi/kubectl-pass/raw/master/kubectl-pass
chmod ug+x kubectl-pass
```

Now you can run as a kubectl plugin, standalone, or alias:

```bash
kubectl pass -h
# OR
kubectl-pass -h
# OR
alias kubepass=kubectl-pass
kubepass -h
```

## Commands

* [x] kubectl pass [auth](#auth)
* [ ] kubectl pass [secret](#secret)

### Auth

`kubectl pass auth` reads an encrypted pass file, looks for matching keywords
and returns a Kubernetes `ExecCredential` kind manifest.
files. For example, if you are using PEM, create a new encrypted pass secret:

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

### Secret

Coming Soon...

## See Also

## License

* MIT

[kubectl]: https://kubernetes.io/docs/tasks/tools/install-kubectl/
[pass]: https://www.passwordstore.org
[passwordstore.org]: https://www.passwordstore.org
[jq]: https://github.com/stedolan/jq
