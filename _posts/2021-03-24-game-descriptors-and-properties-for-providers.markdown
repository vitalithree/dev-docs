---
layout: post
title:  "Game Descriptors and Properties for Providers"
date:   2021-03-24 10:06:51 +0200
---
## Definitions

The external app/data provider needs to describe its metadata on the GameBus platform. Specifically, you will have to provide so-called “game descriptors” for the application, listing which properties are available for each game session.  For each property, a type needs to be specified. The currently available types are:

* BOOL
* COORDINATE
* DATE
* DOUBLE
* INT
* PERCENTAGE
* STRING
* TIME

In summary, for your external provider, you should supply the following metadata:

* A list of game descriptors,
* A list of properties, with their corresponding type.

Or you can just reuse available game descriptors and properties or use an extension of them.

* Example of a GameSession:

```
{
  "gameDescriptorId": 101,
  "properties": [
    {
      "id": 1001,
      "value": "2015-09-29",
      "name": "Started at date",
      "type": "DATE"
    },
    {
      "id": 1002,
      "value": "01:30 AM",
      "name": "Started at time",
      "type": "TIME"
    },
    {
      "id": 1003,
      "value": 72,
      "name": "Seconds to Complete",
      "type": "INT"
    },
    {
      "id": 1004,
      "value": 321,
      "name": "Score",
      "type": "INT"
    },
    {
      "id": 1005,
      "value": 123,
      "name": "Level",
      "type": "INT"
    }
  ]
}
```

In this example, the “name” and “type” attributes are provided just for documentation purposes. They are fully ignored by our API since the actual property name and type information is already known inside GameBus. Put differently, only the attributes “id” and “value” matter in the array of property instance.

## Game Descriptors and Properties Examples

In the following spreadsheet, you will find available Game Descriptors and properties for "GameBus" Provider as well as an example integrated provider like "Google Fit".
Please pay attention to the defined combinations of Providers, Game Descriptors and Properties. This should provide you an example how different game descriptors and properties can be defined and combined for an example integrated data provider.

[Open the spreadsheet](https://docs.google.com/spreadsheets/d/1VedpRoIaJeyeWpacDodSlShhCg3VtDbrRl_E_8OvD60/edit?usp=sharing){:target="_blank"}

