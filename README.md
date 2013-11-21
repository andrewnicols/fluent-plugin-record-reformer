# fluent-plugin-record-reformer

[![Build Status](https://secure.travis-ci.org/sonots/fluent-plugin-record-reformer.png?branch=master)](http://travis-ci.org/sonots/fluent-plugin-record-reformer) [![Dependency Status](https://gemnasium.com/sonots/fluent-plugin-record-reformer.png)](https://gemnasium.com/sonots/fluent-plugin-record-reformer)

Fluentd pluging to add or replace fields of a event record

## Installation

Use RubyGems:

    gem install fluent-plugin-record-reformer

## Configuration

Example:

    <match foo.**>
      type record_reformer
      output_tag reformed.${tag}
      
      hostname ${hostname}
      tag ${tag}
      time ${time.strftime('%Y-%m-%dT%H:%M:%S%z')}
      message ${hostname} ${tag_parts.last} ${message}
    </match>

Assume following input is coming:

```js
foo.bar {"message":"hello world!", "foo":"bar"}
```

then output becomes as below (indented):

```js
reformed.foo.bar {
  "hostname":"your_hostname", 
  "tag":"foo.bar",
  "time":"2013-05-01T01:13:14Z",
  "message":"your_hostname bar hello world!",
  "foo":"bar"
}
```

Note that the keyword `output_tag` is reserved for the output tag. It can not be used as a record key. 

### Placeholders

The keys of input json are available as placeholders. In the above example, 

* ${foo}
* ${message}

shall be available. In addition, following placeholders are reserved: 

* ${hostname} hostname
* ${tag} input tag
* ${tags} input tag splitted by '.' (obsolete. use tag_parts)
* ${tag_parts} input tag splitted by '.'
* ${time} time of the event

It is also possible to write a ruby code in placeholders, so you may write some codes as

* ${time.strftime('%Y-%m-%dT%H:%M:%S%z')}
* ${tag_parts[0]}
* ${tag_parts.last}

## Notice

Please note that this plugin enables to execute any ruby codes. Do not allow anyone to write fluentd configuration from outside of your system by security reasons.

## Relatives

I created this plugin inspired by [fluent-plugin-record-modifier](https://github.com/repeatedly/fluent-plugin-record-modifier). 
I chose not to send pull requests because the implementation of this plugin became completely different with it.

## ChangeLog

See [CHANGELOG.md](CHANGELOG.md) for details.

## Contributing

1. Fork it
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Add some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create new [Pull Request](../../pull/new/master)

## Copyright

Copyright (c) 2013 Naotoshi SEO. See [LICENSE](LICENSE) for details.
