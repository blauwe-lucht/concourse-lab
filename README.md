# concourse-lab

Some experiments I ran when figuring out Docker-in-Docker in Concourse.

    docker-compose up -d
    sudo wget -O /usr/local/bin/fly "http://localhost:8080/api/v1/cli?arch=amd64&platform=linux"
    sudo chmod a+x /usr/local/bin/fly
    fly -t test login -c http://localhost:8080 -u test -p test

    # Test with Alpine based Docker-in-Docker containing Molecule (does not work).
    fly validate-pipeline --config=molecule.yml
    fly --target=test set-pipeline --config=molecule.yml --pipeline=molecule --non-interactive
    fly --target=test unpause-pipeline --pipeline=molecule

    # Test with other Alpine based Docker-in-Docker, bare (almost works: docker info works, docker run hello-world doesn't).
    fly validate-pipeline --config=dind-alpine.yml
    fly --target=test set-pipeline --config=dind-alpine.yml --pipeline=dind-alpine --non-interactive
    fly --target=test unpause-pipeline --pipeline=dind-alpine

After that you need to start the builds in de UI manually.

Result: I gave up and moved to BuildBot for these kind of builds. The main reason for this is that Concourse does NOT use Docker,
but Garden/runc to start containers which is not 100% compatible with Docker. Which also means you can't bind /var/run/docker.sock.
