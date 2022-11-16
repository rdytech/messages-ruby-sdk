
# MessageMedia Messages Ruby SDK
[![MessageMedia Messages SDK RubyGem](https://github.com/messagemedia/messages-ruby-sdk/actions/workflows/ci.yml/badge.svg)](https://github.com/messagemedia/messages-ruby-sdk/actions/workflows/ci.yml)
[![Pull Requests Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg?style=flat)](http://makeapullrequest.com)
[![HitCount](http://hits.dwyl.io/messagemedia/messages-ruby-sdk.svg)](http://hits.dwyl.io/messagemedia/messages-ruby-sdk)
[![gem](https://badge.fury.io/rb/messagemedia_messages_sdk.svg)](https://rubygems.org/gems/messagemedia_messages_sdk)

The MessageMedia Messages API provides a number of endpoints for building powerful two-way messaging applications.

![Isometric](https://i.imgur.com/jJeHwf5.png)

## Table of Contents
* [Authentication](#closed_lock_with_key-authentication)
* [Errors](#interrobang-errors)
* [Information](#newspaper-information)
  * [Slack and Mailing List](#slack-and-mailing-list)
  * [Bug Reports](#bug-reports)
  * [Contributing](#contributing)
* [Installation](#star-installation)
* [Get Started](#clapper-get-started)
* [API Documentation](#closed_book-api-documentation)
* [Need help?](#confused-need-help)
* [License](#page_with_curl-license)

## :closed_lock_with_key: Authentication

Authentication is done via API keys. Sign up at https://developers.messagemedia.com/register/ to get your API keys.

Requests are authenticated using HTTP Basic Auth or HMAC. For Basic Auth, your API key will be basicAuthUserName and API secret will be basicAuthPassword. For HMAC, your API key will be hmacAuthUserName and API secret will be hmacAuthPassword. This is demonstrated in the [Send an SMS example](https://github.com/messagemedia/messages-ruby-sdk/blob/master/README.md#send-an-sms) below.

## :interrobang: Errors

Our API returns standard HTTP success or error status codes. For errors, we will also include extra information about what went wrong encoded in the response as JSON. The most common status codes are listed below.

#### HTTP Status Codes

| Code      | Title       | Description |
|-----------|-------------|-------------|
| 400 | Invalid Request | The request was invalid |
| 401 | Unauthorized | Your API credentials are invalid |
| 403 | Disabled feature | Feature not enabled |
| 404 | Not Found |	The resource does not exist |
| 50X | Internal Server Error | An error occurred with our API |

## :newspaper: Information

#### Slack and Mailing List

If you have any questions, comments, or concerns, please join our Slack channel:
https://developers.messagemedia.com/collaborate/slack/

Alternatively you can email us at:
developers@messagemedia.com

#### Bug reports

If you discover a problem with the SDK, we would like to know about it. You can raise an [issue](https://github.com/messagemedia/signingkeys-nodejs-sdk/issues) or send an email to: developers@messagemedia.com

#### Contributing

We welcome your thoughts on how we could best provide you with SDKs that would simplify how you consume our services in your application. You can fork and create pull requests for any features you would like to see or raise an [issue](https://github.com/messagemedia/messages-nodejs-sdk/issues). Please be aware that a large share of the files are auto-generated by our backend tool.

## :star: Installation
* Run the following command to install the SDK via RubyGems:
```
gem install messagemedia_messages_sdk
```

## :clapper: Get Started
It's easy to get started. Simply enter the API Key and secret you obtained from the [MessageMedia Developers Portal](https://developers.messagemedia.com) into the code snippet below and a mobile number you wish to send to.

### Send an SMS
Destination (`destinationNumber`) and source number (`sourceNumber`) should be in the [E.164](http://en.wikipedia.org/wiki/E.164) format. For example, `+61491570156` NOT `0491570156`. The code snippet below comprises of only the bare minimum parameters required to send a message. You can view the full list of parameters over [here](https://github.com/messagemedia/messages-ruby-sdk/wiki/Message-Body-Parameters). Alternatively, you can refer [this](https://github.com/messagemedia/messages-ruby-sdk/blob/master/examples/sendMessage.rb) code snippet with all the parameters in use.

```ruby
require 'message_media_messages'
require 'pp'

include MessageMediaMessages

auth_user_name = 'API_KEY'
auth_password = 'API_SECRET'
use_hmac = false

client = MessageMediaMessages::MessageMediaMessagesClient.new(
    auth_user_name: auth_user_name,
    auth_password: auth_password,
    use_hmac_authentication: use_hmac
)

messages_controller = client.messages
body = SendMessagesRequest.new
body.messages = []

body.messages[0] = Message.new
body.messages[0].content = 'My first message'
body.messages[0].destination_number = '+61491570156'

begin
  result = messages_controller.send_messages(body)
  pp result
rescue SendMessages400ResponseException => ex
  puts "Caught SendMessages400ResponseException: #{ex.message}"
rescue APIException => ex
  puts "Caught APIException: #{ex.message}"
end
```

## Send an MMS
Destination (`destinationNumber`) and source number (`sourceNumber`) should be in the [E.164](http://en.wikipedia.org/wiki/E.164) format. For example, `+61491570156` NOT `0491570156`. The code snippet below comprises of only the bare minimum parameters required to send a message. You can view the full list of parameters over [here](https://github.com/messagemedia/messages-ruby-sdk/wiki/Message-Body-Parameters). Alternatively, you can refer [this](https://github.com/messagemedia/messages-ruby-sdk/blob/master/examples/sendMessage.rb) code snippet with all the parameters in use.

```ruby
require 'message_media_messages'
require 'pp'

include MessageMediaMessages

auth_user_name = 'API_KEY'
auth_password = 'API_SECRET'
use_hmac = false

client = MessageMediaMessages::MessageMediaMessagesClient.new(
    auth_user_name: auth_user_name,
    auth_password: auth_password,
    use_hmac_authentication: use_hmac
)

messages_controller = client.messages
body = SendMessagesRequest.new
body.messages = []

body.messages[0] = Message.new
body.messages[0].content = 'My second message'
body.messages[0].destination_number = '+61491570158'
body.messages[0].format = FormatEnum::MMS
body.messages[0].media = ['https://images.pexels.com/photos/1018350/pexels-photo-1018350.jpeg?cs=srgb&dl=architecture-buildings-city-1018350.jpg']
body.messages[0].subject = 'This is an MMS message'

begin
  result = messages_controller.send_messages(body)
  pp result
rescue SendMessages400ResponseException => ex
  puts "Caught SendMessages400ResponseException: #{ex.message}"
rescue APIException => ex
  puts "Caught APIException: #{ex.message}"
end
```

### Get Status of a Message
You can get a messsage ID from a sent message by looking at the `message_id` from the response of the above example.
```ruby
require 'message_media_messages'
require 'pp'

include MessageMediaMessages

auth_user_name = 'API_KEY'
auth_password = 'API_SECRET'
use_hmac = false

client = MessageMediaMessages::MessageMediaMessagesClient.new(
    auth_user_name: auth_user_name,
    auth_password: auth_password,
    use_hmac_authentication: use_hmac
)

messages_controller = client.messages
message_id = '877c19ef-fa2e-4cec-827a-e1df9b5509f7'

begin
  result = messages_controller.get_message_status(message_id)
  pp result
rescue APIException => ex
  puts "Caught APIException: #{ex.message}"
end
```

### Get replies to a message
You can check for replies that are sent to your messages
```ruby
require 'message_media_messages'
require 'pp'

include MessageMediaMessages

auth_user_name = 'API_KEY'
auth_password = 'API_SECRET'
use_hmac = false

client = MessageMediaMessages::MessageMediaMessagesClient.new(
    auth_user_name: auth_user_name,
    auth_password: auth_password,
    use_hmac_authentication: use_hmac
)

replies_controller = client.replies
begin
  result = replies_controller.check_replies()
  pp replies
rescue APIException => ex
  puts "Caught APIException: #{ex.message}"
end
```

### Check Delivery Reports
This endpoint allows you to check for delivery reports to inbound and outbound messages.
```ruby
require 'message_media_messages'
require 'pp'

auth_user_name = 'API_KEY'
auth_password = 'API_SECRET'
use_hmac = false

client = MessageMediaMessages::MessageMediaMessagesClient.new(
    auth_user_name: auth_user_name,
    auth_password: auth_password,
    use_hmac_authentication: use_hmac
)

delivery_reports_controller = client.delivery_reports
begin
  result = delivery_reports_controller.check_delivery_reports()
  pp result
rescue APIException => ex
  puts "Caught APIException: #{ex.message}"
end
```

### Confirm Delivery Reports
This endpoint allows you to mark delivery reports as confirmed so they're no longer returned by the Check Delivery Reports function.
```ruby
require 'message_media_messages'
require 'pp'

include MessageMediaMessages

auth_user_name = 'API_KEY'
auth_password = 'API_SECRET'
use_hmac = false

client = MessageMediaMessages::MessageMediaMessagesClient.new(
    auth_user_name: auth_user_name,
    auth_password: auth_password,
    use_hmac_authentication: use_hmac
)

delivery_reports_controller = client.delivery_reports
body = ConfirmDeliveryReportsAsReceivedRequest.new
body.delivery_report_ids = ['011dcead-6988-4ad6-a1c7-6b6c68ea628d', '3487b3fa-6586-4979-a233-2d1b095c7718', 'ba28e94b-c83d-4759-98e7-ff9c7edb87a1']

begin
  result = delivery_reports_controller.confirm_delivery_reports_as_received(body)
  pp result
rescue APIException => ex
  puts "Caught APIException: #{ex.message}"
end
```

###  Check credits remaining (Prepaid accounts only)
This endpoint allows you to check for credits remaining on your prepaid account.
```ruby
require 'message_media_messages'
require 'pp'

include MessageMediaMessages

auth_user_name = 'API_KEY'
auth_password = 'API_SECRET'
use_hmac = false

client = MessageMediaMessages::MessageMediaMessagesClient.new(
    auth_user_name: auth_user_name,
    auth_password: auth_password,
    use_hmac_authentication: use_hmac
)

messages_controller = client.messages
begin
  result = messages_controller.check_credits_remaining()
  pp result
rescue APIException => ex
  puts "Caught APIException: #{ex.message}"
end
```

## :closed_book: API Reference Documentation
Check out the [full API documentation](https://developers.messagemedia.com/code/messages-api-documentation/) for more detailed information.

## :confused: Need help?
Please contact developer support at developers@messagemedia.com or check out the developer portal at [developers.messagemedia.com](https://developers.messagemedia.com/)

## :page_with_curl: License
Apache License. See the [LICENSE](LICENSE) file.
