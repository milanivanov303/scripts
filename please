#!/bin/bash

. ./.env

export DOCKER_COMPOSE_FILE="docker-compose.yml"
export DOCKER_COMPOSE_LOCAL_FILE="docker-compose.local.yml"
export SERVICE="web"
export USER="1000"

# Set user to root for all commands
if [[ "$1" == "root" ]]; then
    shift 1
    USER="root"
fi

if [[ ! -f $DOCKER_COMPOSE_FILE ]]; then
    echo "Could not find ${DOCKER_COMPOSE_FILE} configuration file"
fi

if [[ ! -f $DOCKER_COMPOSE_LOCAL_FILE ]]; then
    echo "Could not find ${DOCKER_COMPOSE_LOCAL_FILE} configuration file"
fi

export DOCKER_COMPOSE="docker-compose --file ${DOCKER_COMPOSE_FILE} --file ${DOCKER_COMPOSE_LOCAL_FILE}"

if [[ "$1" == "start" ]]; then
    shift 1

    $DOCKER_COMPOSE down
    $DOCKER_COMPOSE up --build --detach "$@"

    if [[ -f ./start.sh ]]; then
        ./start.sh
    fi

elif [[ "$1" == "stop" ]]; then
    $DOCKER_COMPOSE down

elif [[ "$1" == "npm" ]]; then
    shift 1
    ###s
    # This is needed only because UM is a mix of PHP+VueJs
    # When we separate it in two project will remove it
    ###
    if [[ ${PWD##*/} == "user-management" ]]; then
         $DOCKER_COMPOSE exec --user $USER --workdir "/app/AppVue" $SERVICE npm "$@"
    else
        $DOCKER_COMPOSE exec --user $USER $SERVICE npm "$@"
    fi

elif [[ "$1" == "mysql" ]]; then
    shift 1
    if [[ "$1" = "" ]]; then
        $DOCKER_COMPOSE exec --user root mysql mysql "$DB_DATABASE"
    else
        $DOCKER_COMPOSE exec --user root mysql mysql "$@"
    fi

elif [[ "$1" == "mysqldump" ]]; then
    shift 1
    $DOCKER_COMPOSE exec --user root mysql mysqldump "$@"

else
    $DOCKER_COMPOSE exec --user $USER $SERVICE "$@"
fi
