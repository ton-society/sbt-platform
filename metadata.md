# SBT metadata standard
For onchain analytics of user activities we need to add information about the activity inside an SBT.
This standard proposal aims to align the metadata of SBTs issued on TON both by TON Society and ecosystem projects.

> It's important to use **one collection for each activity reward** and put there only SBT's related to exact this reward.

## Collection metadata
```json
{
  "name": "TON Meetup for Developers and Founders",
  "description": "Participants of the event which takes place on Dec, 19 2023 at Hong Kong.",
  "image":"https://s.getgems.io/nft/c/62695cb92d780b7496caea3a/nft/56/629b9349e034e8e582cf6448.png",
  "cover_image":"https://s.getgems.io/nft/c/62695cb92d780b7496caea3a/cover.png",
  "social_links": [
    "https://society.ton.org/ton-meetup-for-developers-and-founders"
  ]
}

```

| | | |
|-|-|-|
| name         | Collection name                                      | For activities published at [TON Society Activity Catalog](https://society.ton.org/activities) we recommend to use Activity title. Recommended length: 15-30 characters. Example: ```TON Meetup for Developers and Founders```|
| description | Collection description | Describe the purpose of the activity. For events you could use the template: ```"Participants of the event which takes place on %date% at %place%"``` |
| image | Collection image URL | We recommend using SBT image as collection image. For activities published at [TON Society Activity Catalog](https://society.ton.org/activities), you can use a special image based on Organizer
| cover_image | Cover image URL | Use any cover or reuse the collection image
| social_links | Collection social links | For activities published at TON Society Activities Catalog, we recommend to use activity address like ```https://society.ton.org/ton-meetup-for-developers-and-founders```


## Item metadata

```json
{
  "name": "TON Meetup for Developers and Founders",
  "description": "Reward for participation",
  "content_url":"https://s.getgems.io/nft/c/62695cb92d780b7496caea3a/cover.png",
  "content_type": "video/mp4",
  "image":"https://s.getgems.io/nft/c/62695cb92d780b7496caea3a/nft/56/629b9349e034e8e582cf6448.png",
  "buttons": [
    {
      "label": "Open in TON Society",
      "uri": "https://society.ton.org/welcome"
    }
  ],
  "activity_type": "Event",
  "organizer": "TON Society Asia",
  "original_activity_url": "https://lu.ma/9ovl6qeq",
  "start_date_iso": "2024-02-22",
  "end_date_iso": "2024-02-22",
  "place": { 
    "type": "Offline",
    "country_code_iso": "HK",
    "place_coordinates_osm": "22.2793278,114.1628131",
    "venue_name": "Congress center"
  },
  "user_data": "the event org was amazing!",
  "attributes": [
    {
        "trait_type": "Organizer",
        "value": "TON Society Asia"
    },
    {
        "trait_type": "Date",
        "value": "22 Feb 2024"
    },
    {
        "trait_type": "Place",
        "value": "Hong Kong, Hong Kong"
    }
  ]
}

```

| | | |
|-|-|-|
| name         | SBT name                                      | For activities published at [TON Society Activity Catalog](https://society.ton.org/activities) we recommend to use Activity title. Recommended length: 15-30 characters. Example: ```TON Meetup for Developers and Founders```|
| description | SBT description | Describe the reason why a person received this SBT. Example: ```"Reward for participation"``` |
| content_url | URL for an additional content | We recommend to use identical videos for all SBTs within one collection. For activities published at [TON Society Activity Catalog](https://society.ton.org/activities), we recommend using a special video based on Organizer instead of plain image. You can use any video for your reward as well (no more than 1000 MB, recommended size 1000x1000, formats: mp4, webm, quicktime or mpeg)
| content_type | Type of content used for SBT display | If you use video, please specify the format. Example: ```video/mp4```
| image | SBT image URL | We recommend to use identical images for all SBTs within one collection. For activities published at [TON Society Activity Catalog](https://society.ton.org/activities), we recommend using a special image based on Organizer. If you use video in ```content_url``` field, place the image of the first frame here
| buttons | A set of attributes describing TON Society registration link | This button will be displayed at marketplaces and user wallets when a person views SBT. To onboard a person to TON Society you may use: ```{ "label": "View TON Society Profile", "uri": "https://society.ton.org/welcome" }``` 
| activity_type | Type of activity | One of the values that could be obtained from TON Society via API. Examples: ```event```, ```contest```, ```hackathon```, ```quest```. <br /><br /> Use ```Other``` if your activity is not defined. 
| organizer | Organizer category | For activities that are outside TON Society use ```Community``` or specify Organizer name.<br /><br /> For TON Society activities:  TON Society Hub name, ```TON Syndicate``` or ```Community``` will be used as a value.
| original_activity_url | Address used to complete the activity | URL for event info, quest, post in Telegram, etc. Example: ```https://lu.ma/9ovl6qeq```
| start_date_iso | Activity start date |Leave blank if activity is not time specific. Otherwise specify in [ISO 8601 format](https://en.wikipedia.org/wiki/ISO_8601). Example: ```2024-02-22```
| end_date_iso | Activity end date | Leave blank if activity is not time specific. Otherwise specify in [ISO 8601 format](https://en.wikipedia.org/wiki/ISO_8601). Example: ```2024-02-22```
| place | A set of attributes describing the place where activity happens | ```type```: Use ```Online``` for online events or ```Offline``` for offline events <br /><br /> ```country_code_iso```: If type is ```Offline```, specify two-letter country code from [ISO 3166-1](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2). Example: ```FI``` <br /><br /> ```place_coordinates_osm```: If ```type``` is ```Offline```, specify place coordinates (lat, lon). These coordinates [could be matched](https://stackoverflow.com/a/11600949) with city name using [OpenStreetMap database](https://wiki.openstreetmap.org/wiki/Tag:place%3Dcity). Example: ```22.2793278,114.1628131``` for Hong Kong <br /><br /> ```venue_name```: If ```type``` is ```Offline```, specify venue name
| user_data | Any additional data provided by user when completing an activity | Add data as a text
| attributes | Additional attributes you want to add. They will be displayed for SBT on marketplaces and at user wallets | Optional. Input the data as ```trait_type: value``` array. Example: ```"attributes": [{"trait_type": "Organizer","value": "TON Society Asia"},{"trait_type": "Date","value": "22 Feb 2024"},{"trait_type": "Place","value": "Hong Kong, Hong Kong"}]```
