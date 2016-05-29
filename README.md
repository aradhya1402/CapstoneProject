# CapstoneProject

## Screenshots

![ScreenShot1 : Details of a report](https://raw.github.com/thomasthiebaud/CapstoneProject/develop/images/Screenshot%201.png)
![ScreenShot2 : Full notification on incomming call](https://raw.github.com/thomasthiebaud/CapstoneProject/develop/images/Screenshot%203.png)

## Prerequisites

This readme asumes that you have `git` and a full Android development environment available.
You will also need [docker](https://docs.docker.com/linux/) and [docker-compose](https://docs.docker.com/compose/install/).

## Get the sources

In order to get the sources, from a terminal, run :

    git clone https://github.com/thomasthiebaud/CapstoneProject.git
    cd CapstoneProject
    git submodule init
    git submodule update

## Get a configuration file

Get a configuration file using the [doc](https://developers.google.com/identity/sign-in/android/start-integrating#add_google_play_services).
Enable both `Google Sign-in` and `Ànalytics`. Make sure you also have a `Web application` type client ID (check [here](https://developers.google.com/identity/sign-in/android/start-integrating#get_your_backend_servers_oauth_20_client_id) for more details).

Once you have a `google-services.json` file, move it to `QuietAndroid/app`

## Set up the app (1/2)

From the configuration file, get your oauth client id.
Export it (as an env variable) :

    export GOOGLE_CLIENT_ID='<your id here>'

and also add it to `QuietAndroid/gradle.properties`

    QuietServerId="<your id here>"

## Start the server

Go to the `QuietServer` directory and run

    docker-compose build
    docker-compose up

## Set up the app (2/2)

Now the server is running so you can retrieve its IP address using

    docker inspect --format '{{ .NetworkSettings.IPAddress }}' quietserver_server_1

and add this address to `QuietAndroid/gradle.properties`

    QuietServerUrl="<Quiet server ip address here>"

## Test using the emulator

You can test this app with the android emulator. In order to fake a call, once the emulator is running, you can do

    echo "gsm call <phone number here>" | nc -v  localhost 5554

If you have an error like

    Android Console: Authentication required
    Android Console: type 'auth <auth_token>' to authenticate
    Android Console: you can find your <auth_token> in
    '/xxxx/xxxx/.emulator_console_auth_token'

get the `auth_token` and run instead

    echo "auth <auth_token> \n gsm call <phone_number>" | nc -v  localhost 5554
