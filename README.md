# concourse-lab

    docker-compose up -d
    sudo wget -O /usr/local/bin/fly "http://localhost:8080/api/v1/cli?arch=amd64&platform=linux"
    sudo chmod a+x /usr/local/bin/fly
    fly -t test login -c http://localhost:8080 -u test -p test

    # Test with Alpine based Docker-in-Docker containing Molecule (does not work).
    fly validate-pipeline --config=molecule.yml
    fly --target=test set-pipeline --config=molecule.yml --pipeline=molecule --non-interactive
    fly --target=test unpause-pipeline --pipeline=molecule

    # Test with other Alpine based Docker-in-Docker, bare (does work).
    fly validate-pipeline --config=dind-alpine.yml
    fly --target=test set-pipeline --config=dind-alpine.yml --pipeline=dind-alpine --non-interactive
    fly --target=test unpause-pipeline --pipeline=dind-alpine
