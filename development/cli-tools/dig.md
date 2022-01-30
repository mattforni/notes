# `dig`
`dig` is a command-line tool for querying DNS.

## Two Types of Arguments
There are two main types of argument that can be passed to the dig command: query and format. The query arguments tell `dig` what DNS query to make and format arguments tell `dig` how to format the output.

### Query Arguments
There are three things that are usually used to make a query:

| Argument | Description | Example | Default |
| ---------| ----------- | ------- | ------- |
| **name** | The name of the resource to query | `google.com`, `mattforni.com` | An empty name (`.``) |
| **type** | The type of resource to query | `A`, `CNAME` | `A` |
| **server** | The name of the server to query | `8.8.8.8`, `localhost` | The contents of `/etc/resolv.conf` |

The format for these arguments is:

`dig @server type name`

#### Reverse Lookup
The `-x` flag can be used to perform a reverse lookup. Under the covers it is just a query with the type `PTR` and the IP address, but it is a useful shortcut for quickly finding the name of a host.

### Formatting Argument
By default the output `dig` generates is rather verbose. The output can be pared down significantly by instructing
`dig` to print *just* the answer section, which can be done with the `+noall` and `+answer` flags. An example of
querying the Google nameservers and printing just the answer section is:

`dig +noall +answer ns google.com`

which outputs:
```bash
google.com.		3600	IN	NS	ns4.google.com.
google.com.		3600	IN	NS	ns2.google.com.
google.com.		3600	IN	NS	ns3.google.com.
google.com.		3600	IN	NS	ns1.google.com.
```

This output can be reduced event further by using the `+short` flag, which just shows the output of each record.

`dig +short ns google.com`

which outputs:

```bash
ns1.google.com.
ns3.google.com.
ns4.google.com.
ns2.google.com.
```

#### Configuration File
The default output of the `dig` can be changed by specifying a `.digrc` file in the home directory. The defaults specified in the `.digrc` can then be overridden by adding the `+all` flag to any `dig` query.`

Source: [Julia Evans - How to use dig](https://jvns.ca/blog/2021/12/04/how-to-use-dig/)