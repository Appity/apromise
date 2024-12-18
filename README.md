# APromise

A simple Promise implementation for
[Ruby Async](https://github.com/socketry/async). This is level-triggered,
meaning `APromise#wait` will wait if necessary, or return the previous
resolution value if one has already been supplied.

## Installation

Add this line to your application's Gemfile:

```ruby
gem 'apromise'
```

And then execute:

```shell
bundle install
```

Or install it yourself as:

```shell
gem install apromise
```

## Usage

Within any Async reactor:

```ruby
Async do
  promise = APromise.new

  Async do
    value = promise.wait
  end

  promise.resolve(value: :example)
end
```

It's intended that `resolve` be called once and once only. Calling it more
than once may lead to unpredicable behavior.

The resolution value can be determined via a block as well:

```ruby
Async do
  promise = APromise.new

  Async do
    value = promise.wait
  end

  promise.resolve do
    :example
  end
end
```

This form makes it easier to capture and propagate any potential exceptions.

Note that the `.wait` call does not have to be established prior to the
promise being resolved. It can be called any time:

```ruby
Async do
  promise = APromise.new

  promise.resolve(value: :example)

  Async do
    value = promise.wait
  end
end
```

In this case calling `wait` returns the value immediately.

It's also possible to create a promise that comes pre-resolved as this might
prove useful in situations where the response from a call must be a promise:

```ruby
Async do
  promise = APromise.new(value: :example)

  Async do
    value = promise.wait
  end
end
```

In this case no actual waiting is performed, the value is just returned.

The `new` method also takes a block like `resolve` and will capture and
re-raise exceptions accordingly.

## Development

After checking out the repo, run `bin/setup` to install dependencies. Then, run `rake spec` to run the tests. You can also run `bin/console` for an interactive prompt that will allow you to experiment.

## Contributing

Bug reports and pull requests are welcome on GitHub at https://github.com/appity/apromise. This project is intended to be a safe, welcoming space for collaboration, and contributors are expected to adhere to the [code of conduct](https://github.com/appity/apromise/blob/master/CODE_OF_CONDUCT.md).

## License

The gem is available as open source under the terms of the [MIT License](https://opensource.org/licenses/MIT).

## Code of Conduct

Everyone interacting in the Apromise project's codebases, issue trackers, chat rooms and mailing lists is expected to follow the [code of conduct](https://github.com/appity/apromise/blob/master/CODE_OF_CONDUCT.md).
