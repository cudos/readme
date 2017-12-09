# ansible.md - remote me up

I have had ansible installed through debian package management. There
is nothing wrong with the ansible debian package. Though I want to work
with the pip package. So I do the following:

```
$ sudo apt-get --purge remove ansible
$ mkvirtualenv ansible
$ pip install ansible
```

After this operation we should have the ansible pip package installed. Check it:

```
$ ansible --version
ansible 2.4.2.0
  config file = None
  configured module search path = [u'/home/jens/.ansible/plugins/modules', u'/usr/share/ansible/plugins/modules']
  ansible python module location = /home/jens/.virtualenvs/ansible/local/lib/python2.7/site-packages/ansible
  executable location = /home/jens/.virtualenvs/ansible/bin/ansible
  python version = 2.7.13 (default, Jan 19 2017, 14:48:08) [GCC 6.3.0 20170118]
```

Perfect.

I have an AWS account. I want ansible to list my running instances. I
run `ansible --help` and find the `--list-hosts` option. I run
`ansible --list-hosts` but this should not work, ansible does not know
anything about my AWS account yet. So, as expected the last command
fails and outputs: ERROR! Extraneous options or arguments. I go to
<https://www.ansible.com/quick-start-video> and watch the quick start
video. This won't hurt a lot... Ok, this was a nice and short
introduction getting the very basics.
