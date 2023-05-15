# Alert Example
<img width="601" alt="image" src="https://user-images.githubusercontent.com/17516896/224507162-53513e8a-69ab-41e2-a5d5-ea4e51a9fc89.png">

# Setup

1. [Install Docker](https://docs.docker.com/engine/install/) used to run the sniper

2. Go to https://develop.battle.net/access/clients and create a client, get the blizzard oauth client and secret ids

3. Setup a discord channel with a webhook url for sending the alert messages

4. Download [the docker image](https://hub.docker.com/repository/docker/cohenaj194/mega-alerts/general)

```
docker pull cohenaj194/mega-alerts
```

5. Make a json object with the item ids and prices that you want to snipe for!

This is what you will set for `DESIRED_ITEMS` or you can set `{}` if you only want to snipe pets.

* The item ids for items you want to snipe are as the keys
* and set the price in gold for each item as the value

For example the following looks for [item id 194641 (which is the elemental lariat)](https://undermine.exchange/#us-thrall/194641) for under 500k and another item for under 40k.

```
{"194641": 500000, "159840":40000}
```

You can find that id at the end of the undermine exchange link for the item https://undermine.exchange/#us-thrall/194641 or if you look it up on wowhead the url also has the item id https://www.wowhead.com/item=194641/design-elemental-lariat

6. Make a json object with the pet ids and prices that you want to snipe for!

This is what you will set for `DESIRED_PETS` or you can set `{}` if you only want to snipe regular items.

* The pet ids for items you want to snipe are as the keys
* and set the price in gold for each item as the value

For example the following looks for [pet species id 3390 (which is the Sophic Amalgamation)](https://undermine.exchange/#us-suramar/82800-3390) for under 3K.

```
{"3390": 3000}
```

You can find that id at the end of the undermine exchange link for the item next to `82800` (which is the item id for pet cages) https://undermine.exchange/#us-suramar/82800-3390.

7. If you only want to run the alerts on specific realms then make a json list of all the realm names and put it in a string:
```
'["Thrall", "Silvermoon"]'
```

8. Run the docker container (go to 9. if you are running it with docker desktop) with the following env vars.

Make sure to set `WOW_REGION` with either `EU` or `NA`

If you want to snipe across all realms then do not add the `HOME_REALMS` env var.
```
docker run -dit \
    --name wow-test \
    --env WEBHOOK_URL=$WEBHOOK_URL \
    --env WOW_CLIENT_ID=$WOW_CLIENT_ID \
    --env WOW_CLIENT_SECRET=$WOW_CLIENT_SECRET \
    --env DATA_SET=MEGA \
    --env WOW_REGION=EU \
    --env DESIRED_ITEMS='{"194641": 500000, "159840":40000}' \
    --env DESIRED_PETS='{"3390": 2700}' \
    cohenaj194/mega-alerts
```
If you want to snipe only on specific realms then add the `HOME_REALMS` env var in as a string:

```
docker run -dit \
    --name wow-test \
    --env WEBHOOK_URL=$WEBHOOK_URL \
    --env WOW_CLIENT_ID=$WOW_CLIENT_ID \
    --env WOW_CLIENT_SECRET=$WOW_CLIENT_SECRET \
    --env DATA_SET=MEGA \
    --env WOW_REGION=EU \
    --env DESIRED_ITEMS='{"194641": 500000, "159840":40000}' \
    --env DESIRED_PETS='{"3390": 2700}' \
    --env HOME_REALMS='["Thrall", "Silvermoon"]' \
    cohenaj194/mega-alerts
```

If you want to have the alerts trigger on all of your desired realms at several specific minutes (or all realms in the region if `HOME_REALMS` is not set) then add them into a list for the env var `EXTRA_ALERTS`. The following triggers on the selected realms for the region at 15 and 45 minutes past the hour:

```
docker run -dit \
    --name wow-test \
    --env WEBHOOK_URL=$WEBHOOK_URL \
    --env WOW_CLIENT_ID=$WOW_CLIENT_ID \
    --env WOW_CLIENT_SECRET=$WOW_CLIENT_SECRET \
    --env DATA_SET=MEGA \
    --env WOW_REGION=EU \
    --env DESIRED_ITEMS='{"194641": 500000, "159840":40000}' \
    --env DESIRED_PETS='{"3390": 2700}' \
    --env HOME_REALMS='["Thrall", "Silvermoon"]' \
    --env EXTRA_ALERTS='[15, 45]' \
    cohenaj194/mega-alerts
```

9. In docker desktop download the image and run it 

<img width="1297" alt="image" src="https://user-images.githubusercontent.com/17516896/224506498-d385e177-4fd0-41fc-ae80-78e77b2e0c7b.png">

For the env vars set all the env vars you see in the `--env` lines in step 9 and then click run:

<img width="539" alt="image" src="https://user-images.githubusercontent.com/17516896/224507016-4195f8c2-f55e-44b7-b7f4-991ffdf38d35.png">

