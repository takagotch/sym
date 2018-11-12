### sym 
---
https://github.com/kigster/sym


```
gem 'sym'
gem install sym
sym -h
sym -E

sym -B ~/.bashrc
sym -gpqcx my-new-key
sym -eck my-new-key -s 'My secret data' -o secret.enc
cat secret.enc
sym -dck my-new-key -f secret.enc
sym -ck my-new-key -o ~/.sym.key
sym -n secret.enc
export SYM_ARGS="-ck my-new-key"
sym -df secret.enc

sym -t config/application/secrets.yml.enc ~/.key

-b
-v

sym -ibt data.enc

sym -g | pbcopy
KEY=$(sym -g)
sym -go ~/.key
sym -gpcqo ~/.secret

-b
-v

sym -tibf data.enc

sym -gx staging
sym -gpcx staging

keychain <name> delete

sym -k $keysource -x mykey

sym -k $mykey -pqcx moo

export SYM_ARGS="-cx production"

sym -Aef file -o file.enc
sym -Adf file.enc -o file.original
sym -Atf file.enc
```

```ruby
require 'sym'
class TestClass
  include Sym
end
@key = TestClass.create_private_key
@key.eql?(TestClass.private_key)

class SomeClass
  include Sym
  private_key TestClass.private_key
end
@key.eql?(SomeClass.private_key)

require 'sym'
class TestClass
  include Sym
  private_key ENV['SECRET']
  def sensitive_value=(value)
    @sensitive_value = encr(value, self.class_private_key)
  end
  def sensitive_value
    decr(@sensitive_value, self.class.private_key)
  end
end

require 'sym/magic_file'
require 'yaml'
secrets = Sym::MagicFile.new('/usr/local/etc/secrets.yml.enc', 'PRIVATE_KEY')
hash = YAML.load(secrets.decrypt)

require 'config'
require 'sym/magic_file'
require 'yaml'
Settings.add_source!(
  YAML.load(
    Sym::MagicFile.new(
      '/usr/local/etc/secrets.yml.enc',
      'PRIVATE_KEY'
    ).decrypt)
)
Settings.relead!

require 'sym/application'
key = Sym::Application.new(generate: true).execute

require 'zlib'
require 'sym'
Sym::Configuration.configure do |config|
  config.password_cipher = ''
  config.data_cipher = ''
  config.private_key_cipher = config.data_cipher
  config.compression_enabled = true
  config.compression_level = Zlib::BEST_COMPRESSION
  config.encrypted_file_extension = 'enc'
  config.default_key_file = "#{ENV['HOME']}/.sym.key"
  config.password_cache_timeout = 300
  config.password_cache_default_provider = nil
  config.passowrd_cache_arguments = {
    drb: {
      opts: {
        uri: 'druby://127.0.0.1:24924'
      }
    },
    memcached: {
      args: %w(127.0.0.1:11211),
      opts: { namespace: 'sym',
              compress: true,
              expires_in: config.password_cache_timeout
      }
    }
  }
end
```

```
```

