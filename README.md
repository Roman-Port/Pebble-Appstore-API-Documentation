# Pebble-Appstore-API-Documentation
Documentation for my Pebble Appstore API.

# Input
To create a request, send a GET request to https://pebble-appstore.romanport.com/api/. This URL will take in parameters through the URL. The table below describes how to do this.


| Param Name | Required | Example                            | Description |
| ---------- | -------- | ---------------------------------- | ----------- |
| &limit=    | Yes      | &limit=20                          | This is the maximum number of entries that are returned. It can be anywhere between 1-100. |
| &type=     | Yes       | &type=watchapp                     | This filters the search between watchapp and watchface. Right now, valid entries are ``watchapp`` and ``watchface``. |
| &offset=   | No       | &offset=20                         | This is the offset in the datbase that is applied after all operations except for limit. |
| &sort=     | No       | &sort=POPULAR                      | This changes the sort mode applied before any other operation. Valid requests are ``DATE``, ``POPULAR``, ``NAME``, ``CATEGORY``, and ``RANDOM``. |
| &search=   | No       | &search=Discord                    | This is a basic text search parameter. As of now, it will only see if the name contains the string passed. This will be changed in the future. |
| &category= | No       | &category=5261a8fb3b773043d500000c | This filters the search to this specific category. It takes in the original Pebble category IDs. There is a file in this repository that lists these. |
| &platform= | No       | &platform=chalk                    | This filters the search to specific hardware. Valid entries are ``chalk``, ``emery``, ``diorite``, ``aplite``, and ``basalt``. |
| &appID=    | No        | &appID=561960c8a1dd2652af00000d    | This will return the details for an app if it exists in the database. |
| &appUUID=  | No        | &appUUID=70c08ef8-8c80-46ac-83f7-1af4b43949cb | This is the same as the app ID request, but uses the UUID instead. | 
| &authorUUID= | No      | &authorUUID=52f02483af0eab7af50006d3 | This will filter the response to only be from this author. | 

## Input Examples
Here is an example of a basic query that will return the most recently updated app. The default sort is DATE. https://pebble-appstore.romanport.com/api/?limit=1&offset=0&type=watchapp. 

# Output
The output of this API is in it's own custom format. Original Pebble API output emulation is planned for the future. The basic JSON structure of the output is the following...

| Key        | Type        | Description |
| ---------- | ----------- | ----------- |
| data       | Array       | This contains all of the app data. View the table below for more info. |
| length     | Int         | The number of apps returned. |
| total      | Int         | The total number of apps your query returned. Not necessarily the number of apps listed in the output. |
| totalPages | Int         | The total number of pages your query returned. Uses your limit as the page size. |
| can        | Named Array | Tells the client if the page can be turned. View the table below for more info. |

## Output - can
Here is what the ``can`` key in the output will look like.

| Key        | Type        | Description |
| ---------- | ----------- | ----------- |
| next       | bool        | Can the page be turned to the next? |
| back       | bool        | Can the page be turned to the last? |

## Output - data
This is an array of apps. Each app has it's own layout. View the table below for the format of each app.

| Key               | Type          | Description |
| ----------------- | ------------- | ----------- |
| appSource         | string        | The URL the "source" button should point to. |
| appType           | string        | The type of app this is. Right now, valid entries are ``watchapp`` and ``watchface``. |
| path_banner       | string        | The URL to the banner image. You should look at the decoding URLs section. |
| path_screenshots  | string array  | An array containing the URLs to screenshots. |
| basalt_compat     | string (treat as int)           | Firmware version compat for basalt. Will be -1 if not compatible. | 
| path_icon         | string        | URL to the icon. |
| id                | string        | The ID of the app. | 
| category          | string        | The name of the category. |
| path_raw          | string        | URL to the raw JSON data from the Pebble servers. |
| uuid              | string        | The UUID of the app. |
| author            | string        | The name of the author. |
| devID             | string        | The ID of the developer. |
| com_ios           | string        | The URL to a companion app on IOS. Will be blank if none. |
| com_android       | string        | The URL to a companion app on Android. Will be blank if none. |
| categoryId        | string        | The ID of the category you can use to search.                 |
| lastVersion       | string        | The last version released by the developer. |
| website           | string        | The "website" button should point to this. Will be blank if none. |
| lastUpdate        | string        | The date the app was last updated. Format: MM:DD:YYYY HH:MM:SS AA |
| description       | string        | The description of the app. Line breaks are sent as ``<br>``.         |
| aplite_compat     | string (treat as int)           | Firmware version compat for aplite. Will be -1 if not compatible. |
| diorite_compat    | string (treat as int)           | Firmware version compat for diorite. Will be -1 if not compatible. |
| path_list         | string        | Path to the list image. |
| emery_compat      | string (treat as int)           | Firmware version compat for emery. Will be -1 if not compatible. |
| name              | string        | The title of the app. |
| chalk_compat      | string (treat as int)           | Firmware version compat for chalk. Will be -1 if not compatible. |
| path_pbw          | string        | The path to the PBW file. |
| hearts            | string (treat as int)           | The number of likes this app had **on the original Pebble servers**. |

# Decoding URLS.

All URLS pointing to my server have ``%rootUrl%`` in them. To decode this, replace ``%rootUrl%`` with ``https://pebble-appstore.romanport.com``. Example input: ``%rootUrl%/files/5aee3639f38588014500083b/app.pbw``. Example output: ``https://pebble-appstore.romanport.com/files/5aee3639f38588014500083b/app.pbw``.

# Output Example
Here is an output for https://pebble-appstore.romanport.com/api/?limit=1&offset=0&type=watchapp.
```json
{
   "data":[
      {
         "appSource":"https:\/\/github.com\/klejnov\/Pebble-Bobruisk-info",
         "appType":"watchapp",
         "path_banner":"https:\/\/roman-pebble-appstore.firebaseapp.com\/assets\/files\/5aee3639f38588014500083b\/banner.png",
         "path_screenshots":[
            "https:\/\/roman-pebble-appstore.firebaseapp.com\/assets\/files\/5aee3639f38588014500083b\/screenshot_0.png",
            "https:\/\/roman-pebble-appstore.firebaseapp.com\/assets\/files\/5aee3639f38588014500083b\/screenshot_1.png",
            "https:\/\/roman-pebble-appstore.firebaseapp.com\/assets\/files\/5aee3639f38588014500083b\/screenshot_2.png",
            "https:\/\/roman-pebble-appstore.firebaseapp.com\/assets\/files\/5aee3639f38588014500083b\/screenshot_3.png",
            "https:\/\/roman-pebble-appstore.firebaseapp.com\/assets\/files\/5aee3639f38588014500083b\/screenshot_4.png"
         ],
         "basalt_compat":"3",
         "path_icon":"https:\/\/roman-pebble-appstore.firebaseapp.com\/assets\/files\/5aee3639f38588014500083b\/icon.png",
         "id":"5aee3639f38588014500083b",
         "category":"Daily",
         "path_raw":"%rootUrl%\/raw\/5aee3639f38588014500083b.txt",
         "uuid":"14d027b8-4fb2-47f0-9a8a-be8d1b5a754c",
         "author":"klejnov",
         "devID":"5adb79f5461a8d1f9e000849",
         "com_ios":"",
         "com_android":"",
         "categoryId":"5261a8fb3b773043d500000c",
         "lastVersion":"1.2",
         "website":"https:\/\/vk.com\/klejnov",
         "lastUpdate":"5\/7\/2018 10:58:39 PM",
         "description":"\u0410\u043a\u0442\u0443\u0430\u043b\u044c\u043d\u0430\u044f \u043f\u043e\u0433\u043e\u0434\u0430 \u0432 \u0411\u043e\u0431\u0440\u0443\u0439\u0441\u043a\u0435 \u0438 \u0441\u0432\u0435\u0436\u0438\u0435 \u043a\u0443\u0440\u0441\u044b \u0432\u0430\u043b\u044e\u0442 \u0432\u0441\u0435\u0433\u0434\u0430 \u0443 \u0432\u0430\u0441 \u043d\u0430 \u0440\u0443\u043a\u0435 :)<br>\n\u0411\u0435\u043b\u0430\u0440\u0443\u0441\u044c. \u0411\u043e\u0431\u0440\u0443\u0439\u0441\u043a",
         "dataType":"5",
         "aplite_compat":"-1",
         "diorite_compat":"-1",
         "path_list":"https:\/\/roman-pebble-appstore.firebaseapp.com\/assets\/files\/5aee3639f38588014500083b\/list.png",
         "emery_compat":"3",
         "name":"Bobruisk info",
         "chalk_compat":"-1",
         "path_pbw":"%rootUrl%\/files\/5aee3639f38588014500083b\/app.pbw",
         "hearts":"2",
         "usrHearts":"0"
      }
   ],
   "length":1,
   "total":2789,
   "totalPages":2789,
   "can":{
      "next":true,
      "back":false
   }
}
```

# Categories And Their IDs
Here is a table with the original names, ids, and colors of the six categories. Watchapps are their own category, but aren't listed here.

| Name                    | ID                       | Color Hex Code |
| ----------------------- | ------------------------ | -------------- |
| Daily                   | 5261a8fb3b773043d500000c | #3db9e6 |
| Tools & Utilities       | 5261a8fb3b773043d500000f | #fdbf37 |
| Notifications           | 5261a8fb3b773043d5000001 | #FF9000 |
| Remotes                 | 5261a8fb3b773043d5000008 | #fc4b4b |
| Heath & Fitness         | 5261a8fb3b773043d5000004 | #98D500 |
| Games                   | 5261a8fb3b773043d5000012 | #b57ad3 |
