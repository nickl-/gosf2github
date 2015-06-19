## Imports tickets from SF into GH

This was developed for the gene ontology but it is generic. For background, see:

https://docs.google.com/document/d/1iyVY8kDBJIEydoWFLG9j5BoO4VmdIXES95fVhIkFXfk/edit#

## Usage

```
gosf2github.pl [-h] [-u USERMAP] [-c COLLABINFO] [-r REPO] [-t OATH_TOKEN] [-a USERNAME] [-l LABEL]* [-s SF_TRACKER] [--dry-run] TICKETS-JSON-FILE

Migrates tickets from sourceforge to github, using new v3 GH API, documented here: https://gist.github.com/jonmagic/5282384165e0f86ef105

Requirements:

 * This assumes that you have exported your tickets from SF. E.g. from a page like this: https://sourceforge.net/p/obo/admin/export
 * You have a github account and have created an OAth token here: https://github.com/settings/tokens    
 * You have "curl" in your PATH

Example Usage:

curl -H "Authorization: token TOKEN  https://api.github.com/repos/obophenotype/cell-ontology/collaborators > cell-collab.json
gosf2github.pl -a cmungall -u users_sf2gh.json -c cell-collab.json -r obophenotype/cell-ontology -t YOUR-TOKEN-HERE cell-ontology-sf-export.json 



ARGUMENTS:

   -k | --dry-run
                 Do not execute github API calls; print curl statements instead

   -r | --repo   REPO *REQUIRED*
                 Examples: cmungall/sf-test, obophenotype/cell-ontology

   -t | --token  TOKEN *REQUIRED*
                 OATH token. Get one here: https://github.com/settings/tokens
                 Note that all tickets and issues will appear to originate from the user that generates the token

   -l | --label  LABEL
                 Add this label to all tickets, in addition to defaults and auto-added.
                 Currently the following labels are ALWAYS added: auto-migrated, a priority label (unless priority=5), a label for every SF label, a label for the milestone

   -u | --usermap USERMAP-JSON-FILE *RECOMMENDED*
                  Maps SF usernames to GH
                  Example: https://github.com/geneontology/go-site/blob/master/metadata/users_sf2gh.json

   -a | --assignee  USERNAME *RECOMMENDED*
                 Default username to assign tickets to if there is no mapping for the original SF assignee in usermap

   -c | --collaborators COLLAB-JSON-FILE *REQUIRED*
                  Required, as it is impossible to assign to a non-collaborator
                  Generate like this:
                  curl -H "Authorization: token TOKEN  https://api.github.com/repos/cmungall/sf-test/collaborators > sf-test-collab.json

   -s | --sf-tracker  NAME
                 E.g. obo/mouse-anatomy-requests
                 If specified, will append the original URL to the body of the new issue. E.g. https://sourceforge.net/p/obo/mouse-anatomy-requests/90

NOTES:

 * uses a pre-release API documented here: https://gist.github.com/jonmagic/5282384165e0f86ef105
 * milestones are converted to labels
 * all issues and comments will appear to have originated from the user who issues the OAth ticket

TIP:

Note that the API does not grant permission to create the tickets as
if they were created by the original user, so if your token was
generated from your account, it will look like you submitted the
ticket and comments.

Create an account for an agent like https://github.com/bbopjenkins -
use this account to generate the token. This may be better than having
everything show up under your own personal account


```

