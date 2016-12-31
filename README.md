# Development Proxy

Testing Ansible roles places quite a strain on the network as a play will
invariably download system packages and applications.  This means local dev can
be painful, especially at my girlfriend's house as her DSL is rubbish!!!

## Usage

Install the roles with:

```
ansible-galaxy install -r requirements.yml
```

Then `vagrant up`.

See the [ansible-package-caching-proxy](https://github.com/delphix/ansible-package-caching-proxy) role for the full set of configuration options.

### Requirements
* [Vagrant Landrush] (https://github.com/vagrant-landrush/landrush)
* [Ansible] (github.com/ansible/ansible)

## Components
### apt-cacher-ng
Clients can then create */etc/apt/apt.conf.d/02proxy* with the following content:

```
Acquire::http { Proxy "http://proxy_ip:3142"; };
```

#### Usage in Ansible Testing
The test suites for my roles use test-kitchen to pass env vars from the
host to the test VM, which Ansible then looks for and creates the above entry
if needed.

If the test VM has the Landrush plugin enabled (most don't) then it can be set
to the fqdn of this VM (proxy.vagrant.test).

Otherwise the IP can be used by doing something like this:

```
export APT_CACHE_HOST="$(dig @localhost -p 10053 +short proxy.vagrant.test)"
```

### devpi
### npmjs-proxy
### docker-registry v2
### nginx caching proxy

# License
    Copyright (c) 2016 Colin Woodcock
    
    Permission is hereby granted, free of charge, to any person obtaining a copy of
    this software and associated documentation files (the "Software"), to deal in
    the Software without restriction, including without limitation the rights to
    use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies
    of the Software, and to permit persons to whom the Software is furnished to do
    so, subject to the following conditions:
    
    The above copyright notice and this permission notice shall be included in all
    copies or substantial portions of the Software.
    
    THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
    IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
    FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
    AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
    LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
    OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
    SOFTWARE.
