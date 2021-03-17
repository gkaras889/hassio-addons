
# Home Assistant Add-ons: Monocle Gateway

## Prerequisites

You'll need an account and an API token on [Monocle Cam Portal](https://portal.monoclecam.com). 
Obviously you'll need a camera and it must be added to your profile.

## Installation

The installation of this add-on is pretty straightforward and not different in
comparison to installing any other Hass.io add-on.

1. Add my Home Assistant add-ons repository (**https://github.com/gkaras889/hassio-addons**) to your Home Assistant instance.
1. Install the "Monocle Gateway" add-on.
1. Configure the `monocle_token` option.
1. Start the "Monocle Gateway" add-on.
1. Check the logs of the "Monocle Gateway" add-on to see if everything
    went well. Ask your Alexa to show you your cam

## Configuration

**Note**: _Remember to restart the add-on when the configuration is changed._

Monocle Gateway add-on configuration:

```yaml
monocle_token: 'your_token'
```


### Basic options

#### Option `monocle_token` (Required)

Set your API token generated on your [Monocle Cam Account page](https://portal.monoclecam.com/account 


## Contributing

This is an active project. I'm always open to people who want to
use the code or contribute to it.


## Trademark legal notice

This add-on is not created, developed, affiliated, supported, maintained or endorsed by **shadeBlue LLC**.
All product names, logos, brands, trademarks and registered trademarks are property of their respective owners. All company, product, and service names used are for identification purposes only.
Use of these names, logos, trademarks, and brands does not imply endorsement.


## License

Copyright (c) 2021 GKaras


[repository]: https://github.com/gkaras889/hassio-addons
[hassiohelp]: https://t.me/HassioHelp
