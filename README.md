# Solidus Bolt

[![CircleCI](https://circleci.com/gh/solidusio-contrib/solidus_bolt.svg?style=shield)](https://circleci.com/gh/solidusio-contrib/solidus_bolt)
[![codecov](https://codecov.io/gh/solidusio-contrib/solidus_bolt/branch/master/graph/badge.svg)](https://codecov.io/gh/solidusio-contrib/solidus_bolt)

<!-- Explain what your extension does. -->

## Installation

Add solidus_bolt to your Gemfile:

```ruby
gem 'solidus_bolt'
```

Bundle your dependencies and run the installation generator:

```shell
bin/rails generate solidus_bolt:install
```

## Usage

### Setting up Bolt Configuration

Many of the API calls handled by this gem use the variables set in Bolt Configuration. Since this extension's seeds automatically generate a Bolt Configuration, the easiest and safest way to configure it would be by setting the following environment variables:

```
BOLT_BEARER_TOKEN
BOLT_ENVIRONMENT
BOLT_MERCHANT_PUBLIC_ID
BOLT_MERCHANT_ID
BOLT_API_KEY
BOLT_SIGNING_SECRET
BOLT_PUBLISHABLE_KEY
```

Alternatively you can setup the Bolt Configuration manually by visiting `/admin/bolt`

### Creating a new Payment Method

Assuming you've used environment variables to configure your Bolt Configuration, creating a Bolt payment method is very easy:

1. Visit `/admin/payment_methods/new`
2. Set `provider` to SolidusBolt::PaymentMethod
3. Click "Save"
4. Choose `bolt_credentials` from the `Preference Source` select
5. Click `Update` to save

If you've instead decided to setup the Bolt Configuration manually, follow the same process mentioned above but at step 4 pick `bolt_config_credentials` instead of `bolt_credentials`.

In both cases you can alternatively create a payment method from the Rails console with:

```ruby
SolidusBolt::PaymentMethod.create(
  name: "Bolt",
  preference_source: "bolt_credentials" # or "bolt_config_credentials"
)
```

The final (not recommended) option is to not select any `Preference Source` at step 4 and instead fill up the inputs manually.

### How to set the webhooks

(For latest up to date guide check [Bolt's Documentation](https://help.bolt.com/developers/guides/webhooks/))

1. Login to your [Bolt Merchant Dashboard](https://merchant.bolt.com/).

2. Navigate to **Developers**.

3. Scroll to **Merchant API**.

4. Add your webhook endpoints.

Important use cases include:

- Notifying your e-commerce store when a transaction has been approved or rejected by Bolt.
- Sending your e-commerce store with the `transaction_id`, which is necessary for back-office operations.
- Sending your e-commerce store more information about a transaction such as credit card details.

## Development

### Testing the extension

First bundle your dependencies, then run `bin/rake`. `bin/rake` will default to building the dummy
app if it does not exist, then it will run specs. The dummy app can be regenerated by using
`bin/rake extension:test_app`.

```shell
bin/rake
```

To run [Rubocop](https://github.com/bbatsov/rubocop) static code analysis run

```shell
bundle exec rubocop
```

When testing your application's integration with this extension you may use its factories.
Simply add this require statement to your `spec/spec_helper.rb`:

```ruby
require 'solidus_bolt/testing_support/factories'
```

Or, if you are using `FactoryBot.definition_file_paths`, you can load Solidus core
factories along with this extension's factories using this statement:

```ruby
SolidusDevSupport::TestingSupport::Factories.load_for(SolidusBolt::Engine)
```

### Running the sandbox

To run this extension in a sandboxed Solidus application, you can run `bin/sandbox`. The path for
the sandbox app is `./sandbox` and `bin/rails` will forward any Rails commands to
`sandbox/bin/rails`.

Here's an example:

```
$ bin/rails server
=> Booting Puma
=> Rails 6.0.2.1 application starting in development
* Listening on tcp://127.0.0.1:3000
Use Ctrl-C to stop
```

### Updating the changelog

Before and after releases the changelog should be updated to reflect the up-to-date status of
the project:

```shell
bin/rake changelog
git add CHANGELOG.md
git commit -m "Update the changelog"
```

### Releasing new versions

Please refer to the dedicated [page](https://github.com/solidusio/solidus/wiki/How-to-release-extensions) on Solidus wiki.

## License

Copyright (c) 2022 [name of extension author], released under the New BSD License.
