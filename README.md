# Medbay

[![Build Status](https://travis-ci.org/tastycake/medbay.svg?branch=master)](https://travis-ci.org/tastycake/medbay)

Medbay provides a DRY method of providing web service health checks. Provide a series of lambdas and Medbay will execute and optionally benchmark them, returning the result.

## Installation

Add this line to your application's Gemfile:

```ruby
gem 'medbay'
```

And then execute:

    $ bundle

Or install it yourself as:

    $ gem install medbay

## Usage

Medbay is a complete Sinatra app ready to be mounted in Rails.

```ruby
# routes.rb
mount Medbay::App, at: '/servicehealth'
```

### Configuration Example

Simple Redis connectivity check

```ruby
# config/initializers/medbay.rb (for Rails apps)

Medbay.configure do |config|
  redis_check = Medbay::Test.new('Redis', lambda {
    result = false

    begin
      result = ($redis.ping == "PONG")
    rescue Exception => e
      result = false
    end

    return result
  })

  config.tests = [ redis_check ]
  config.benchmark = true # if you want results to include a benchmark
end
```

## Development

After checking out the repo, run `bin/setup` to install dependencies. Then, run `rake spec` to run the tests. You can also run `bin/console` for an interactive prompt that will allow you to experiment.

To install this gem onto your local machine, run `bundle exec rake install`. To release a new version, update the version number in `version.rb`, and then run `bundle exec rake release`, which will create a git tag for the version, push git commits and tags, and push the `.gem` file to [rubygems.org](https://rubygems.org).

## Contributing

Bug reports and pull requests are welcome on GitHub at https://github.com/tastycake/medbay. This project is intended to be a safe, welcoming space for collaboration, and contributors are expected to adhere to the [Contributor Covenant](contributor-covenant.org) code of conduct.


## License

The gem is available as open source under the terms of the [MIT License](http://opensource.org/licenses/MIT).
