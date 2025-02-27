# frb-participants [![CircleCI](https://circleci.com/gh/wealthsimple/frb-participants.svg?style=svg)](https://circleci.com/gh/wealthsimple/frb-participants) [![](https://img.shields.io/gem/v/frb-participants.svg)](https://rubygems.org/gems/frb-participants)

Provides data from the The Federal Reserve Banks' Fedwire & FedACH participants in JSON format, mapping routing number to associated bank name and branch info. It also provides a Rubygem for querying this data by routing number.

For more details, see [Federal Reserve Bank Services: Download E-Payments Routing Directories](https://www.frbservices.org/EPaymentsDirectory/download.html)

## Rubygem:

```ruby
gem 'frb-participants'
```

You can do basic queries of data:

```ruby
FrbParticipants::FedachParticipant.find_by_routing_number("322170016")
=> #<OpenStruct
      type=:ach,
      customer_name="BANK OF AMERICA, N.A. - ARIZONA",
      known_normalized_name="Bank of America",
      best_attempt_normalized_name="Bank Of America, N.A. - Arizona"
      office_type="main",
      servicing_frb_number="121000374",
      record_type_code="1",
      revision_date="121503",
      new_routing_number=nil,
      address="VA2-430-01-01",
      city="RICHMOND",
      state="VA",
      zip="23261",
      zip_extension="7025",
      telephone="8004460135">
```

```ruby
FrbParticipants::FedwireParticipant.find_by_routing_number("325182946")
=> #<OpenStruct
      type=:wire,
      telegraphic_name="UMPQUA BANK WA",
      customer_name="UMPQUA BANK",
      known_normalized_name=nil,
      best_attempt_normalized_name="Umpqua Bank",
      state="OR",
      city="HILLSBORO",
      funds_transfer_eligible=true,
      settlement_only=false,
      securities_transfer_eligible=false,
      revision_date="20160526">
```

Important! If you are using this on a production service, it is recommended that you preload the data in an initializer:

```ruby
require 'frb-participants'
FrbParticipants::Data.preload!
```

## Normalized data:

If you don't use Ruby, or only need access to the normalized data, this is available in the `/data/` directory. Below are small previews of how this data is structured for each file:

 **[fedwire-participants.json](https://github.com/wealthsimple/frb-participants/blob/master/data/fedwire-participants.json)**

```json
{
  "325182836":{
    "telegraphic_name":"LOWER VALLEY CU WA",
    "customer_name":"LOWER VALLEY CREDIT UNION",
    "state":"WA",
    "city":"SUNNYSIDE",
    "funds_transfer_eligible":true,
    "settlement_only":false,
    "securities_transfer_eligible":true,
    "revision_date":"20120702"
  },
  "325182946":{
    "telegraphic_name":"UMPQUA BANK WA",
    "customer_name":"UMPQUA BANK",
    "state":"OR",
    "city":"HILLSBORO",
    "funds_transfer_eligible":true,
    "settlement_only":false,
    "securities_transfer_eligible":false,
    "revision_date":null
  },
  ...
}
```

 **[fedach-participants.json](https://github.com/wealthsimple/frb-participants/blob/master/data/fedach-participants.json)**

```json
{
  "011000015":{
    "office_type":"main",
    "servicing_frb_number":"011000015",
    "record_type_code":"0",
    "revision_date":"122415",
    "new_routing_number":null,
    "customer_name":"FEDERAL RESERVE BANK",
    "address":"1000 PEACHTREE ST N.E.",
    "city":"ATLANTA",
    "state":"GA",
    "zip":"30309",
    "zip_extension":"4470",
    "telephone":"8773722457"
  },
  ...
}
```
