For that experimental time.

    $ bosh -d demo-jump src/bosh-lite.yml \
      --ops-file <( GITHUB_API_TOKEN=asdf src/repo-collaborators-ops org-name/repo-name )
    $ ssh -i my-github-key my-github-username@10.244.0.5

Notes...

 * [repo-collaborators-ops](src/repo-collaborators-ops) - given a repository name, fetch the keys of all users with write access; generate an operations file which will inject those users/keys into the manifest
 * [bosh-lite.yml](src/bosh-lite.yml) - sample jumpbox which sets a few BOSH_* env vars to default the bosh-lite target and will allow connections with ssh keys
 * [shell](jobs/jump/templates/bin/shell) - a pseudo-shell which boots logged in users into a container; by default it's an empty ubuntu:14.04, but could be customized by the user if they write out a ~/.jump-container. Their home directory is persisted when disconnecting (and also shared between other sessions).
 * in general you should maintain your own default container which installs your toolchain

Replay...

    $ git clone github.com/cloudfoundry/bosh-lite.git
    $ cd bosh-lite
    $ bosh login
    $ bosh upload-release https://bosh.io/d/github.com/cf-platform-eng/docker-boshrelease?v=28.0.2
    $ bosh upload-release http://bosh.io/d/github.com/cloudfoundry/os-conf-release?v=9
    $ git clone github.com/dpb587/demo-jump-release.git
    $ cd demo-jump-release
    $ bosh create-release
    $ bosh upload-release
    $ bosh -d demo-jump deploy src/bosh-lite.yml
    $ ssh dpb587@10.244.0.5
    Welcome to jump.bosh-lite.local!

    Hello dpb587, a fresh container is starting where you will be root.
    Only changes in the home directory will be persisted when you disconnect.

      container = ubuntu:16.04
            env = BOSH_CA_CERT
                  BOSH_ENVIRONMENT

    root@453eabe3d379:~#
