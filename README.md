# Entur - CircleCI Helm Orb
This orb tries to simply huge CircleCI configuration files by adding common Helm functionality.

## Usage

Import the orb and give it a name. Add this to the `orbs`-key in your CircleCI-configuration:
```yaml
orbs:
 helm: entur/helm@volatile 
 # volatile selects the newest version. More examples of versioning here: https://circleci.com/docs/2.0/creating-orbs/#semantic-versioning-in-orbs
```

Use the orb like this:

```yaml
jobs:
  your-job-name:
    steps:
     - helm/install-helm-client
     # do more CircleCI stuff.
``` 
         
Available commands can be found in `src/commands`. Usage examples in `examples`       

## Development

Enable the pre-commit git hook to automatically pack the orb before a commit. This hook is located in `.githooks/pre-commit`. 

To enable this hook:
```bash
git config --local core.hooksPath .githooks/
chmod +x .githooks/pre-commit
```

Disclaimer: The commands changes the folder default folder for git hooks locally (only for this project) and gives the script `pre-commit` executable rights. 
This script checks whether CircleCI is installed and packs the orb automatically to `build/org.yml`.
      

## Pack and publish orb

Make sure you have the CircleCI CLI:
```bash
curl -fLSs https://circle.ci/cli | bash 
```      

Pack the contents of src/ to a single orb file:
```bash
circleci config pack ./src > orb.yml
```

Validate that the orb is valid:
```bash
circleci orb validate orb.yml
```

Publish a dev version of the orb (note: dev orbs are mutuable. E.g. you can publish many times):
```bash
circleci orb publish entur orb.yml entur/helm@dev:first     
```      

When ready, promote the orb to production:
```bash
circleci orb publish promote entur/helm@dev:first patch
```

You can read more here: https://circleci.com/docs/2.0/creating-orbs/