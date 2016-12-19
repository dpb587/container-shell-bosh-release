For that experimental time.

    $ bosh -d demo-jump src/bosh-lite.yml \
      --ops-file <( GITHUB_API_TOKEN=asdf src/repo-collaborators-ops org-name/repo-name )
    $ ssh -i my-github-key my-github-username@10.244.0.5

Notes...

 * [repo-collaborators-ops](src/repo-collaborators-ops) - given a repository name, fetch the keys of all users with write access; generate an operations file which will inject those users/keys into the manifest
 * [bosh-lite.yml](src/bosh-lite.yml) - sample jumpbox which sets a few BOSH_* env vars to default the bosh-lite target and will allow connections with ssh keys
 * [shell](jobs/jump/templates/bin/shell) - a pseudo-shell which boots logged in users into a container; by default it's an empty ubuntu:14.04, but could be customized by the user if they write out a ~/.jump-container. Their home directory is persisted when disconnecting (and also shared between other sessions).
 * in general you should maintain your own default container which installs your toolchain
