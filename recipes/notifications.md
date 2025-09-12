
# Send notifications when BirdNET-Pi detects a bird
The BirdNET-PI uses Apprise to send notifications. You can send these notifications to multiple destinations such as Telegram and Discord. For a comprehensive list of all supported platforms see [Apprise documentation](https://github.com/caronc/apprise?tab=readme-ov-file#supported-notifications).

In the menu of the BirdNET-Pi dashboard, click on `Tools`. Then, select `Settings`. Notifications can be configured under `Notifications`. 

BirdNET-Pi allows you to configure when you want to receive notifications. You can also change the notification text. 

When you're ready, make sure to click on `Update Settings` at the bottom of the page.

## Telegram
On Telegram, BirdNET-Pi can send a notification to a Telegram Bot. So, let's create one!

### Telegram Bot
Check out the detailed [Apprise documentation for using Telegram](https://github.com/caronc/apprise/wiki/Notify_telegram) for instructions on how to create your own Bot on Telegram.

You now need to figure out the `chat_id` to configure the Telegram notification correctly. You can use the following URL for this: `https://api.telegram.org/bot{YOURBOTTOKEN}/getUpdates`

For instance, if your Bot token is `7760312342:AAXx5HAxiANK9w-qXucHEl42sEe6GWcc36c` you can use the following URL: `https://api.telegram.org/bot7760312342:AAXx5HAxiANK9w-qXucHEl42sEe6GWcc36c/getUpdates`

Now, combine the information and add Telegram to the Apprise Notifications Configuration under `Settings` on the BirdNET-Pi using the format `tgram://BOT_TOKEN/CHAT_ID`: 

```
tgram://7760312342:AAXx5HAxiANK9w-qXucHEl42sEe6GWcc36c/-1002369230550
```

For more information see [this documentation](https://gist.github.com/nafiesl/4ad622f344cd1dc3bb1ecbe468ff9f8a).

### Telegram Group
Do you want to follow detections with multiple people? Our suggestion is to create a public Telegram Group to collect BirdNET-Pi notifications. To achieve this, add your Telegram Bot to this group and use the `chatid` of the group when configuring the Apprise notification via Telegram on your BirdNET-Pi.

## Signal
Signal does not support bots as Telegram does. Instead, you can run the [signal-cli-rest-api service](https://github.com/bbernhard/signal-cli-rest-api) on the BirdNET-Pi that is able to send messages over Signal using an authenticated Signal account. 

Simplest way to install and run this service is using [Docker](https://www.docker.com/). Follow [these instructions](https://raspberrytips.com/docker-on-raspberry-pi/) to setup Docker on your Raspberry Pi.

### Setting up signal-cli-rest-api service

Copy the Docker compose file `compose.yaml` provided in this GitHub repository (see `/signal-cli-rest-api`) and run the stack by running the following command in directory where you placed `compose.yaml`:

```
docker compose up -d
```

When the service is running, you should be able to request a QR-code to connect your Signal account (on your mobile phone) with the BirdNET-Pi. Open a browser and connect to `http://birdnet.local:81/v1/qrcodelink?device_name=signal-api` assuming `birdnet.local` is how your BirdNET-Pi is known on the network, you can also provide its IP address.

### Test the service

Send a message to another Signal user using the authenticated phonenumber (e.g. +31612345678 sends message to +31687654321):

```
curl -X POST -H "Content-Type: application/json" 'http://birdnet.local:81/v2/send' \
     -d '{"message": "Test via Signal API!", "number": "+31612345678", "recipients": [ "+31687654321" ]}
```

Using a Signal group is a convenient way to address a community of people. Create the app group in your Signal client, add other users, and make sure to send a message so the group becomes visible to the service. 

Then, list all groups and lookup your newly created group:

```
curl -X GET -H "Content-Type: application/json" http://birdnet.local:81/v1/groups/+31612345678 
```

The group will have its own id, e.g. `group.XHRoZ3h1RDVhY1hnY1dLVjRvZ0xRekJLZGNYcXRTQjV3eStJZWtyNmErYz0=`. Try sending a message to this group using the service:

```
curl -X POST -H "Content-Type: application/json" 'http://birdnet.local:81/v2/send' \
     -d '{"message": "Test via Signal API naar groep!", "number": "+31612345678", "recipients": [ "group.XHRoZ3h1RDVhY1hnY1dLVjRvZ0xRekJLZGNYcXRTQjV3eStJZWtyNmErYz0="]}'
```

If everything works, your ready to configure the BirdNET-Pi. Now, combine the information and add Telegram to the Apprise Notifications Configuration under `Settings` on the BirdNET-Pi using the format `signal://HOSTNAME:PORT/FROM_NUMBER/TO_NUMBER_OR_GROUP_ID`: 

```
signal://localhost:81/+31612345678/group.XHRoZ3h1RDVhY1hnY1dLVjRvZ0xRekJLZGNYcXRTQjV3eStJZWtyNmErYz0=
```
