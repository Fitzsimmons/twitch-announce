# Slack portion (hourly?)

1. Scan slack profiles for twitch.tv and regexp out their usernames
1. Request user ids via e.g. https://api.twitch.tv/kraken/users?login=jsfitzsimmons_creative
	* comma separated usernames
	* e.g. json:
	```json
	{
	  "_total": 1,
	  "users": [
	    {
	      "display_name": "jsfitzsimmons_creative",
	      "_id": "156416453",
	      "name": "jsfitzsimmons_creative",
	      "type": "user",
	      "bio": null,
	      "created_at": "2017-05-10T03:42:22.05476Z",
	      "updated_at": "2017-05-10T03:57:48.687738Z",
	      "logo": null
	    }
	  ]
	}
	```
1. Store all userids in dynamodb

# Twitch portion (minutely)

1. Fetch userids from dynamodb
1. Find all live streams via e.g. https://api.twitch.tv/kraken/streams/?channel=156416453
	* comma separated userids
	* e.g. json:
	```json
	{
	  "_total": 1,
	  "streams": [
	    {
	      "_id": 25245375616,
	      "game": "Creative",
	      "community_id": "",
	      "viewers": 1,
	      "video_height": 1080,
	      "average_fps": 20,
	      "delay": 0,
	      "created_at": "2017-05-10T03:57:01Z",
	      "is_playlist": false,
	      "stream_type": "live",
	      "preview": {
	        "small": "https://static-cdn.jtvnw.net/previews-ttv/live_user_jsfitzsimmons_creative-80x45.jpg",
	        "medium": "https://static-cdn.jtvnw.net/previews-ttv/live_user_jsfitzsimmons_creative-320x180.jpg",
	        "large": "https://static-cdn.jtvnw.net/previews-ttv/live_user_jsfitzsimmons_creative-640x360.jpg",
	        "template": "https://static-cdn.jtvnw.net/previews-ttv/live_user_jsfitzsimmons_creative-{width}x{height}.jpg"
	      },
	      "channel": {
	        "mature": false,
	        "status": "#programming",
	        "broadcaster_language": "en",
	        "display_name": "jsfitzsimmons_creative",
	        "game": "Creative",
	        "language": "en",
	        "_id": 156416453,
	        "name": "jsfitzsimmons_creative",
	        "created_at": "2017-05-10T03:42:22.05476Z",
	        "updated_at": "2017-05-10T03:57:48.687738Z",
	        "partner": false,
	        "logo": null,
	        "video_banner": null,
	        "profile_banner": null,
	        "profile_banner_background_color": "",
	        "url": "https://www.twitch.tv/jsfitzsimmons_creative",
	        "views": 0,
	        "followers": 0,
	        "broadcaster_type": ""
	      }
	    }
	  ]
	}

	```
1. For each active stream id:
1. Next unless it's older than 2 minutes
1. Next if the stream id is present in dynamodb
1. Add the stream id to dyanmodb
1. Announce the stream to slack
